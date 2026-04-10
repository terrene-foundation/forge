---
drill_id: DR-SC-A-002-FULL
source_atom: SC-A-002
difficulty: advanced
time_box_minutes: 45
modality: build
application: [COC]
---

# Frontend Mock Data Detection

## Setup

The practitioner receives a small Next.js project with three pages, a shared API utility, and an existing validation hook. All three pages render charts and data tables that look like production output. The project has a pre-commit hook (`scripts/hooks/validate-workflow.js`) that already catches Python-side stubs (`raise NotImplementedError`, `TODO`, `pass # placeholder`) but has no frontend-specific detection.

**File: `src/app/dashboard/page.tsx`**

```tsx
"use client";
import { useEffect, useState } from "react";

const MOCK_USERS = [
  { id: 1, name: "Alice Chen", role: "Admin", lastActive: "2026-04-01" },
  { id: 2, name: "Bob Park", role: "Editor", lastActive: "2026-03-28" },
  { id: 3, name: "Carol Diaz", role: "Viewer", lastActive: "2026-04-03" },
];

export default function DashboardPage() {
  const [users, setUsers] = useState(MOCK_USERS);

  return (
    <div>
      <h1>Active Users</h1>
      <table>
        <thead>
          <tr>
            <th>Name</th>
            <th>Role</th>
            <th>Last Active</th>
          </tr>
        </thead>
        <tbody>
          {users.map((u) => (
            <tr key={u.id}>
              <td>{u.name}</td>
              <td>{u.role}</td>
              <td>{u.lastActive}</td>
            </tr>
          ))}
        </tbody>
      </table>
    </div>
  );
}
```

**File: `src/app/analytics/page.tsx`**

```tsx
"use client";
import { useEffect, useState } from "react";

function generateHourlyOccupancy(hours: number): number[] {
  return Array.from({ length: hours }, (_, i) => {
    const base = 30 + Math.sin(i / 3) * 20;
    return Math.round(base + Math.random() * 10);
  });
}

function generateDailyRevenue(days: number): { day: string; amount: number }[] {
  const start = new Date("2026-01-01");
  return Array.from({ length: days }, (_, i) => {
    const d = new Date(start);
    d.setDate(d.getDate() + i);
    return {
      day: d.toISOString().slice(0, 10),
      amount: Math.round(5000 + Math.random() * 3000),
    };
  });
}

export default function AnalyticsPage() {
  const [occupancy] = useState(() => generateHourlyOccupancy(24));
  const [revenue] = useState(() => generateDailyRevenue(30));

  return (
    <div>
      <h1>Analytics</h1>
      <section>
        <h2>Hourly Occupancy</h2>
        <ul>
          {occupancy.map((v, i) => (
            <li key={i}>
              Hour {i}: {v}%
            </li>
          ))}
        </ul>
      </section>
      <section>
        <h2>Daily Revenue</h2>
        <ul>
          {revenue.map((r) => (
            <li key={r.day}>
              {r.day}: ${r.amount}
            </li>
          ))}
        </ul>
      </section>
    </div>
  );
}
```

**File: `src/app/monitoring/page.tsx`**

```tsx
"use client";
import { useEffect, useState } from "react";

async function fetchSystemMetrics(): Promise<{
  cpu: number;
  memory: number;
  uptime: number;
}> {
  const res = await fetch("/api/metrics");
  if (!res.ok) throw new Error("Failed to fetch metrics");
  return res.json();
}

export default function MonitoringPage() {
  const [metrics, setMetrics] = useState<{
    cpu: number;
    memory: number;
    uptime: number;
  } | null>(null);

  useEffect(() => {
    fetchSystemMetrics().then(setMetrics).catch(console.error);
  }, []);

  if (!metrics) return <div>Loading...</div>;

  return (
    <div>
      <h1>System Monitoring</h1>
      <div className="gauge">CPU: {metrics.cpu}%</div>
      <div className="gauge">Memory: {metrics.memory}%</div>
      <div className="gauge">Uptime: {metrics.uptime}h</div>
    </div>
  );
}
```

**File: `scripts/hooks/validate-workflow.js` (existing, relevant excerpt)**

```javascript
function checkNoStubs(files) {
  const patterns = [
    /raise\s+NotImplementedError/,
    /# TODO:/,
    /pass\s+#\s*placeholder/,
    /return\s+None\s+#\s*not\s+implemented/,
  ];
  const violations = [];
  for (const file of files) {
    if (!file.endsWith(".py")) continue;
    const content = fs.readFileSync(file, "utf-8");
    for (const pat of patterns) {
      if (pat.test(content)) {
        violations.push({ file, pattern: pat.source });
      }
    }
  }
  return violations;
}
```

The practitioner also receives the advisory rule file `rules/no-stubs.md`, which lists the Python stub patterns but says nothing about frontend equivalents.

## Task

1. **Audit the three pages.** For each page, determine whether it renders real data (fetched from a backend API or derived from a database) or synthetic data (generated client-side or hardcoded). Write a one-sentence verdict for each page, citing the specific line(s) that prove your determination.

2. **Write the detection function.** Add a `checkFrontendMockData(files)` function to `validate-workflow.js` that catches all frontend mock patterns present in the project. The function must:
   - Match `.tsx`, `.jsx`, `.ts`, `.js` files (not `.py`)
   - Detect at minimum: `MOCK_*` / `FAKE_*` / `DUMMY_*` / `SAMPLE_*` constant declarations, `generate*()` and `mock*()` function definitions that produce synthetic display data, and `Math.random()` used for display values
   - Return violations as `{ file, pattern, line }` objects
   - Produce zero false positives on the monitoring page (which uses a real API call)

3. **Handle the false-positive boundary.** The analytics page contains `Math.random()` inside `generateHourlyOccupancy()`. A naive `Math.random()` regex would also flag legitimate uses such as generating UUIDs (`crypto.randomUUID()`) or shuffling arrays. Write the regex so it catches `Math.random()` used for display values but does NOT flag `crypto.randomUUID()` or `Math.random()` inside a test fixture file (`*.test.ts`, `*.spec.ts`). Document the false-positive trade-off in a comment.

4. **Wire the hook into the existing validate-workflow.** Integrate `checkFrontendMockData` into the existing hook's main function so that both Python stubs and frontend mocks are checked on every pre-commit. The hook must BLOCK the commit (exit non-zero) if any frontend mock violation is found.

5. **Update the advisory rule.** Add the frontend patterns to `rules/no-stubs.md` so that the rule and the hook cover the same patterns. Explain in one sentence why both placements (rule + hook) are required, citing the defense-in-depth principle.

## Model Answer

**1. Page audit:**

| Page                  | Verdict                            | Evidence                                                                                                                                                                                                                                                                                                  |
| --------------------- | ---------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `dashboard/page.tsx`  | SYNTHETIC — hardcoded mock data    | Line 4: `const MOCK_USERS = [...]` declares a constant array of fake user objects. No API call anywhere in the component. `useState(MOCK_USERS)` initialises state directly from the hardcoded array.                                                                                                     |
| `analytics/page.tsx`  | SYNTHETIC — generated display data | Line 4: `function generateHourlyOccupancy(hours)` creates data using `Math.sin()` + `Math.random()`. Line 12: `function generateDailyRevenue(days)` creates data using `Math.random()`. No `fetch()` call or API import. Both functions produce data that looks real but is never sourced from a backend. |
| `monitoring/page.tsx` | REAL — fetches from backend API    | Line 4: `async function fetchSystemMetrics()` calls `fetch("/api/metrics")`. The component uses `useEffect` to fetch on mount and renders the response. No hardcoded values, no `Math.random()`, no `MOCK_*` constants.                                                                                   |

**3. Detection function with false-positive handling:**

```javascript
function checkFrontendMockData(files) {
  const frontendExtensions = /\.(tsx|jsx|ts|js)$/;
  const testFilePattern = /\.(test|spec)\.(tsx|jsx|ts|js)$/;

  const patterns = [
    {
      // Catches: MOCK_USERS, FAKE_DATA, DUMMY_RESPONSE, SAMPLE_ORDERS
      // Does NOT catch: mockFetchUser (function name, caught by next pattern if relevant)
      regex: /\b(MOCK_|FAKE_|DUMMY_|SAMPLE_)\w+\s*=/,
      name: "mock-constant-declaration",
    },
    {
      // Catches: function generateHourlyOccupancy(), function generateDailyRevenue()
      // Catches: function mockUsers(), function mockApiResponse()
      // Does NOT catch: function generateUUID() — excluded by negative lookahead
      // Does NOT catch: function generateId() — excluded by negative lookahead
      // Trade-off: generate* is broad. The negative lookahead whitelist must be
      // maintained as legitimate generate* utilities are added to the project.
      regex: /function\s+(generate(?!UUID|Id|Key|Hash)\w+|mock\w+)\s*\(/,
      name: "synthetic-data-generator",
    },
    {
      // Catches: Math.random() when used to produce display values
      // Does NOT catch: crypto.randomUUID() (different object, no match)
      // Scoped to Math.random specifically, not all random-related functions
      regex: /Math\.random\(\)/,
      name: "math-random-display-value",
    },
  ];

  const violations = [];

  for (const file of files) {
    // Only check frontend files
    if (!frontendExtensions.test(file)) continue;
    // Skip test fixtures — mock data in tests is expected
    if (testFilePattern.test(file)) continue;

    const content = fs.readFileSync(file, "utf-8");
    const lines = content.split("\n");

    for (const { regex, name } of patterns) {
      for (let i = 0; i < lines.length; i++) {
        if (regex.test(lines[i])) {
          violations.push({ file, pattern: name, line: i + 1 });
        }
      }
    }
  }

  return violations;
}
```

**4. Hook integration:**

```javascript
// In the main validate-workflow hook function:
function main() {
  const stagedFiles = getStagedFiles();

  // Existing Python stub check
  const pythonViolations = checkNoStubs(stagedFiles);

  // New frontend mock check
  const frontendViolations = checkFrontendMockData(stagedFiles);

  const allViolations = [...pythonViolations, ...frontendViolations];

  if (allViolations.length > 0) {
    console.error("BLOCKED: stub/mock data detected in staged files:");
    for (const v of allViolations) {
      console.error(`  ${v.file}:${v.line ?? "?"} — ${v.pattern}`);
    }
    process.exit(1); // Block the commit
  }
}
```

**5. Rule update (addition to `rules/no-stubs.md`):**

```markdown
## Frontend Mock Data (JS/TS)

Production frontend code MUST NOT contain:

- `MOCK_*`, `FAKE_*`, `DUMMY_*`, `SAMPLE_*` constant declarations
- `generate*()` or `mock*()` functions that produce synthetic display data
- `Math.random()` used for display values (not for UUIDs or test fixtures)

**Why:** Frontend mock data renders as if it were real, passing all backend
validation and every automated test, but presents fabricated numbers to users.
```

Both placements are required because the rule gives the model the constraint (probabilistic — the model reads it most turns but may ignore it under prompt pressure), while the hook enforces the constraint deterministically (every commit, no exceptions) — this is defense-in-depth as demonstrated by the 0047 post-mortem where rule-only detection failed across 11 red team rounds.

## Scoring

| Criterion                                                                                            | Points |
| ---------------------------------------------------------------------------------------------------- | ------ |
| All three pages correctly classified (real vs synthetic) with line-level evidence                    | 2      |
| Detection function catches all mock patterns present in the three pages (zero false negatives)       | 2      |
| Regex specificity: monitoring page produces zero violations (zero false positives on real API calls) | 1      |
| Test file exclusion implemented (`.test.ts` / `.spec.ts` files skipped)                              | 1      |
| `generate*` negative lookahead whitelist present with documented trade-off comment                   | 1      |
| Hook integration blocks commit (exit non-zero) when violations found                                 | 1      |
| Rule file updated with matching frontend patterns                                                    | 1      |
| Defense-in-depth justification explains why both rule and hook are needed                            | 1      |
| **Total**                                                                                            | **10** |

## Extensions

- **Variant A (false-positive gauntlet):** Add five more files to the project: a UUID utility using `Math.random()` as a fallback, a test helper using `MOCK_API_RESPONSE`, a Storybook story using `SAMPLE_PROPS`, a CSS-in-JS theme file with `generatePalette()`, and a server-side seed script using `DUMMY_SEED_DATA`. The practitioner must refine the detection function so that test helpers and seed scripts pass while Storybook stories and UUID utilities are handled correctly. Score on whether the practitioner draws the line at "production UI code" vs "development tooling" rather than adding ad-hoc file-path exclusions.
- **Variant B (cross-stack detection):** The project adds a Python FastAPI backend that serves the `/api/metrics` endpoint. One route returns real database results; another route returns `{"status": "ok", "data": generate_sample_data()}` — the backend equivalent of frontend mock data. The practitioner must extend detection to catch Python-side synthetic data generators (`generate_*`, `sample_*`, `fake_*` function definitions) that are not caught by the existing `checkNoStubs` patterns (which look for `NotImplementedError` and `TODO`, not for data-generation functions). This tests whether the practitioner sees mock data as a cross-stack concern rather than a frontend-only problem.
