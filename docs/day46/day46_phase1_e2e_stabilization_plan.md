# Day 46 — Phase 1: Migration Analyzer E2E Stabilization Plan

## 1. Phase Overview

Day 46 focuses on stabilizing the complete browser workflow for the **AI-Assisted API Migration Analyzer**.

The objective of this phase is to identify and resolve the structural, testing, server-execution, selector, and browser-validation issues that prevent the migration analyzer workflow from running reliably through the complete Playwright end-to-end test suite.

The stabilized workflow must support:

* Desktop Chromium
* Tablet Chromium
* Mobile Chromium
* Production builds
* Vitest unit and integration tests
* Playwright end-to-end tests
* Server-side migration analysis execution
* Reliable rerun and reset behavior

---

## 2. Primary Objective

Stabilize the API Migration Analyzer so that a user can complete the following workflow entirely through the browser:

1. Open the Migration Analyzer application.
2. Select a sample source project.
3. Select a target architecture.
4. Review or modify analysis options.
5. Submit the migration analysis request.
6. Observe the loading and execution state.
7. Receive a completed migration analysis result.
8. Review summary scorecards.
9. Review rule evaluation results.
10. Review migration gaps.
11. Review migration risks.
12. Review recommendations.
13. Review generated migration tasks.
14. Open and inspect supporting evidence.
15. Reset the workflow.
16. Run another migration analysis.
17. Repeat the workflow on desktop, tablet, and mobile layouts.

---

## 3. Current Browser Workflow

The intended browser execution path is:

```text
MigrationAnalysisForm
        ↓
useMigrationAnalysisWorkflow
        ↓
executeMigrationAnalysis
        ↓
POST /api/migration-analysis
        ↓
migrationAnalysisService
        ↓
migrationAnalysisOrchestrator
        ↓
MigrationAnalysisResult
        ↓
MigrationAnalysisResults
```

The browser must never import or execute the migration analysis orchestrator directly.

All migration analysis execution must remain behind the server API route.

---

## 4. Current Stabilization Problems

The current implementation may contain one or more of the following issues:

* Browser hooks importing server-only modules.
* Incorrect relative import paths after project restructuring.
* API client files missing or located in an unexpected directory.
* Vitest discovering Playwright test files.
* Playwright tests being executed in the Vitest environment.
* Testing Library state leaking between tests.
* Missing explicit cleanup after component tests.
* Browser selectors not matching the rendered interface.
* Tests depending on unstable text or DOM structure.
* Loading states completing too quickly for browser assertions.
* Reset behavior leaving stale analysis data.
* Results sections rendering inconsistently across viewport sizes.
* Evidence viewer controls not exposing stable accessible labels.
* Desktop-only assumptions in the E2E test suite.
* Production build failures caused by client/server boundary violations.

---

## 5. Stabilization Focus Areas

Day 46 stabilization work is divided into six focus areas.

### Focus Area 1: Align Imports with the Actual Project Structure

Verify that every workflow, API-client, component, service, route, and test import points to an existing file.

Important imports to validate include:

```text
MigrationAnalysisForm
MigrationAnalysisResults
useMigrationAnalysisWorkflow
executeMigrationAnalysis
migrationAnalysisService
migrationAnalysisOrchestrator
MigrationAnalysisRequest
MigrationAnalysisResult
SourceArtifact
TargetArchitecture
sampleSourceProjects
targetArchitectures
```

The project must not depend on outdated file names or directories that were used during earlier implementation phases.

#### Required checks

* Verify the exact location of the workflow hook.
* Verify the exact location of the API client.
* Verify the exact location of the API route.
* Verify the exact location of the migration analysis service.
* Verify the exact location of the orchestrator.
* Verify component import aliases.
* Verify shared type imports.
* Verify sample data imports.
* Remove duplicate or obsolete imports.
* Remove unused imports.
* Confirm all imports work on case-sensitive file systems.

---

### Focus Area 2: Keep Migration Analysis Execution on the Server

The browser workflow must call the migration analysis API.

The browser hook must not call:

```typescript
runMigrationAnalysisArchitecture(...)
```

directly.

The hook must call an API-client method similar to:

```typescript
executeMigrationAnalysis(request)
```

The API client must send:

```http
POST /api/migration-analysis
Content-Type: application/json
```

The server route must:

1. Parse the request body.
2. Validate the migration analysis request.
3. Call the migration analysis service.
4. Execute the migration analysis orchestrator.
5. Return a `MigrationAnalysisResult`.
6. Convert unexpected failures into a safe error response.

#### Required architectural boundary

```text
Client component
    ↓
Client hook
    ↓
API client
    ↓
HTTP request
    ↓
Server route
    ↓
Server service
    ↓
Migration orchestrator
```

#### Prohibited browser dependency path

```text
Client component
    ↓
Client hook
    ↓
Migration orchestrator
```

This separation is required because the orchestrator may depend on Node.js APIs, filesystem access, server-only modules, or implementation details that must not be included in the browser bundle.

---

### Focus Area 3: Separate Vitest and Playwright Tests

Vitest and Playwright must operate as independent test systems.

Vitest should execute:

* Utility tests
* Domain tests
* Service tests
* Hook tests
* Component tests
* Orchestrator tests

Playwright should execute:

* Browser workflow tests
* Responsive layout tests
* End-to-end API integration tests
* User interaction tests

Vitest must exclude Playwright specifications.

Recommended exclusion patterns include:

```typescript
exclude: [
  "**/node_modules/**",
  "**/dist/**",
  "**/.next/**",
  "**/e2e/**",
  "**/*.spec.ts",
  "**/*.e2e.ts",
]
```

The exact configuration must match the naming convention used by the project.

#### Required test separation

```text
Vitest
├── *.test.ts
├── *.test.tsx
└── Unit and integration tests

Playwright
├── e2e/*.spec.ts
├── tests/e2e/*.spec.ts
└── Browser workflow tests
```

#### Validation commands

```bash
npm run test
```

or:

```bash
npx vitest run
```

Playwright:

```bash
npx playwright test
```

Running Vitest must not attempt to load:

```text
@playwright/test
```

Running Playwright must not depend on Vitest globals such as:

```text
describe
it
expect
vi
```

from the Vitest package.

---

### Focus Area 4: Stabilize Testing Library Cleanup

Component and hook tests must not leak rendered components, DOM state, mocks, or timers between tests.

Testing Library cleanup should run after every test.

Recommended setup:

```typescript
import {
  afterEach,
} from "vitest";

import {
  cleanup,
} from "@testing-library/react";

afterEach(() => {
  cleanup();
});
```

Additional cleanup may include:

```typescript
afterEach(() => {
  vi.clearAllMocks();
  vi.restoreAllMocks();
});
```

Fake timers must be restored when used:

```typescript
afterEach(() => {
  vi.useRealTimers();
});
```

#### Required outcomes

* Each test starts with an empty document body.
* Previous analysis results do not remain mounted.
* Mock API responses do not leak between tests.
* Loading-state tests do not affect later tests.
* Reset tests operate on a fresh workflow instance.
* Tests pass independently and as part of the complete suite.

---

### Focus Area 5: Align Browser Selectors with the Rendered Interface

Playwright selectors must be based on stable, accessible UI contracts.

Preferred selector priority:

1. Role and accessible name
2. Label
3. Placeholder
4. Visible text
5. Stable test ID

Preferred examples:

```typescript
page.getByRole("button", {
  name: /analyze migration/i,
});
```

```typescript
page.getByLabel(/source project/i);
```

```typescript
page.getByRole("heading", {
  name: /migration analysis results/i,
});
```

```typescript
page.getByTestId("migration-risk-list");
```

Avoid selectors based on:

* Generated CSS class names
* Deep DOM paths
* Element position
* Styling implementation
* Unstable text fragments
* Arbitrary timeout delays

Avoid:

```typescript
page.locator("div:nth-child(4) > div > button");
```

Prefer:

```typescript
page.getByRole("button", {
  name: /view evidence/i,
});
```

#### UI elements requiring stable selectors

* Sample project selector
* Target architecture selector
* Analysis submit button
* Reset button
* Loading indicator
* Error message
* Overall migration score
* Compatibility scorecard
* Complexity scorecard
* Readiness scorecard
* Rule result list
* Gap list
* Risk list
* Recommendation list
* Migration task list
* Evidence viewer
* Evidence close button
* Rerun action

---

### Focus Area 6: Validate the Complete E2E Workflow

The final browser test must validate the full migration analysis lifecycle.

#### Required E2E scenario

```text
Given the Migration Analyzer page is open
And no analysis result is displayed

When the user selects a sample source project
And selects a target architecture
And submits the migration analysis

Then a loading state is displayed
And the analysis request is sent to the server
And the results interface is displayed
And summary scorecards are visible
And rule results are visible
And migration gaps are visible
And migration risks are visible
And recommendations are visible
And migration tasks are visible

When the user opens an evidence item

Then the evidence viewer is displayed
And the evidence content is visible

When the user closes the evidence viewer
And resets the workflow

Then the results are removed
And the form returns to its initial state
And another analysis can be executed
```

---

## 6. Required Files to Verify

The following file categories must exist before completing Day 46 stabilization.

The exact folder names may differ, but every responsibility must have a clear implementation.

### Browser workflow

```text
src/components/migration/MigrationAnalysisForm.tsx
src/components/migration/MigrationAnalysisResults.tsx
src/hooks/useMigrationAnalysisWorkflow.ts
```

### API client

```text
src/lib/api/migrationAnalysisClient.ts
```

or:

```text
src/services/migrationAnalysisClient.ts
```

### Server API route

For Next.js App Router:

```text
src/app/api/migration-analysis/route.ts
```

### Server service

```text
src/server/services/migrationAnalysisService.ts
```

or an equivalent server-only location.

### Migration engine

```text
src/lib/migration/analysis/migrationAnalysisOrchestrator.ts
```

### Test configuration

```text
vitest.config.ts
playwright.config.ts
src/test/setup.ts
```

### Browser tests

```text
e2e/migration-analysis.spec.ts
```

or:

```text
tests/e2e/migration-analysis.spec.ts
```

---

## 7. Request and Response Contract

The browser API client and server route must share the same migration analysis contract.

### Request contract

```typescript
export interface MigrationAnalysisRequest {
  sourceArtifact: SourceArtifact;
  targetArchitectureId: string;
  includeEvidence?: boolean;
  includeRecommendations?: boolean;
  includeMigrationTasks?: boolean;
}
```

The exact interface should follow the existing project model.

The browser must not construct fields that are unsupported by the server.

The server must not expect fields that the browser never sends.

### Response contract

```typescript
export interface MigrationAnalysisResult {
  summary: {
    overallScore: number;
    compatibilityScore: number;
    complexityScore: number;
    readinessScore: number;
  };
  ruleResults: readonly MigrationRuleResult[];
  gaps: readonly MigrationGap[];
  risks: readonly MigrationRisk[];
  recommendations: readonly MigrationRecommendation[];
  tasks: readonly MigrationTask[];
  evidence: readonly MigrationEvidence[];
}
```

The existing project type remains the source of truth.

This phase must not create a competing duplicate result interface.

---

## 8. API Error Contract

The API route should return predictable errors.

Example error response:

```typescript
export interface MigrationAnalysisErrorResponse {
  error: string;
  message: string;
}
```

Recommended status behavior:

| Situation                       | Status |
| ------------------------------- | -----: |
| Valid analysis completed        |  `200` |
| Invalid request                 |  `400` |
| Unsupported target architecture |  `400` |
| Missing source artifact         |  `400` |
| Analysis execution failure      |  `500` |

The browser hook should convert API errors into a user-readable workflow state.

Example states:

```typescript
type MigrationWorkflowStatus =
  | "idle"
  | "loading"
  | "success"
  | "error";
```

---

## 9. Workflow State Requirements

The workflow hook should maintain a single authoritative state for browser execution.

Recommended state shape:

```typescript
interface MigrationAnalysisWorkflowState {
  status: "idle" | "loading" | "success" | "error";
  result: MigrationAnalysisResult | null;
  error: string | null;
}
```

### Initial state

```typescript
{
  status: "idle",
  result: null,
  error: null,
}
```

### Loading state

```typescript
{
  status: "loading",
  result: null,
  error: null,
}
```

### Success state

```typescript
{
  status: "success",
  result,
  error: null,
}
```

### Error state

```typescript
{
  status: "error",
  result: null,
  error: errorMessage,
}
```

### Reset state

Reset must restore:

```typescript
{
  status: "idle",
  result: null,
  error: null,
}
```

Reset must also clear form-level values when the current product behavior requires a full workflow reset.

---

## 10. Responsive Browser Requirements

The browser workflow must be validated at multiple viewport sizes.

### Desktop

Suggested viewport:

```text
1440 × 900
```

Expected behavior:

* Form controls display without overlap.
* Results use the full available content width.
* Scorecards appear in a multi-column layout.
* Evidence viewer remains readable.
* Long recommendation and risk text wraps correctly.

### Tablet

Suggested viewport:

```text
768 × 1024
```

Expected behavior:

* Form controls remain usable.
* Scorecards collapse into fewer columns.
* Results do not overflow horizontally.
* Evidence viewer fits within the viewport.
* Buttons maintain usable touch targets.

### Mobile

Suggested viewport:

```text
390 × 844
```

Expected behavior:

* Form controls stack vertically.
* No horizontal page scrolling is introduced.
* Scorecards display in a single-column layout.
* Long identifiers and evidence paths wrap safely.
* Buttons remain visible and selectable.
* Evidence viewer supports mobile scrolling.
* Reset and rerun controls remain accessible.

---

## 11. Playwright Project Requirements

The Playwright configuration should include separate browser projects for responsive validation.

Example:

```typescript
projects: [
  {
    name: "desktop-chromium",
    use: {
      browserName: "chromium",
      viewport: {
        width: 1440,
        height: 900,
      },
    },
  },
  {
    name: "tablet-chromium",
    use: {
      browserName: "chromium",
      viewport: {
        width: 768,
        height: 1024,
      },
    },
  },
  {
    name: "mobile-chromium",
    use: {
      browserName: "chromium",
      viewport: {
        width: 390,
        height: 844,
      },
    },
  },
];
```

The actual configuration can use Playwright device presets when appropriate, but the complete workflow must be validated across all three layout categories.

---

## 12. Phase 1 Implementation Sequence

Day 46 Phase 1 should be completed in the following order.

### Step 1: Verify the project structure

Confirm that all required browser, API, service, orchestrator, type, configuration, and test files exist.

### Step 2: Correct import paths

Update incorrect imports in:

* Workflow hook
* API client
* API route
* Server service
* Components
* Vitest tests
* Playwright tests

### Step 3: Confirm the server boundary

Ensure:

```text
useMigrationAnalysisWorkflow
```

calls:

```text
executeMigrationAnalysis
```

and does not import:

```text
migrationAnalysisOrchestrator
```

### Step 4: Confirm the API route

Verify that:

```text
POST /api/migration-analysis
```

accepts the browser request and returns a valid migration result.

### Step 5: Separate test runners

Exclude Playwright tests from Vitest and confirm that each runner discovers only its own tests.

### Step 6: Stabilize test cleanup

Add or verify Testing Library cleanup and mock restoration.

### Step 7: Review browser selectors

Replace unstable selectors with accessible role, label, text, or test-ID selectors.

### Step 8: Run desktop E2E validation

Validate the complete desktop workflow before expanding to tablet and mobile.

---

## 13. Validation Commands

### Install dependencies

```bash
npm install
```

### Run lint

```bash
npm run lint
```

### Run Vitest

```bash
npx vitest run
```

### Run the production build

```bash
npm run build
```

### Install Playwright browser dependencies

```bash
npx playwright install chromium
```

### Run the desktop Playwright project

```bash
npx playwright test --project=desktop-chromium
```

### Run the complete Playwright suite

```bash
npx playwright test
```

### Open the Playwright report

```bash
npx playwright show-report
```

---

## 14. Phase 1 Acceptance Criteria

Day 46 Phase 1 is complete only when all of the following are true.

### Project structure

* [ ] All required migration workflow files exist.
* [ ] All import paths resolve successfully.
* [ ] No duplicate migration request or result models exist.
* [ ] No client component imports a server-only module.
* [ ] No client hook imports the orchestrator directly.

### Server execution

* [ ] The browser hook calls the API client.
* [ ] The API client calls `POST /api/migration-analysis`.
* [ ] The API route calls the server-side migration analysis service.
* [ ] The service calls the migration analysis orchestrator.
* [ ] The server returns a valid `MigrationAnalysisResult`.
* [ ] Errors are converted into a predictable API response.

### Vitest

* [ ] Playwright specifications are excluded from Vitest.
* [ ] Testing Library cleanup runs after each test.
* [ ] Mocks are cleared or restored after each test.
* [ ] Vitest completes without unhandled errors.
* [ ] Existing migration domain tests continue to pass.

### Build quality

* [ ] ESLint reports zero errors.
* [ ] ESLint reports zero warnings.
* [ ] The production build succeeds.
* [ ] No client/server boundary errors occur.
* [ ] No TypeScript compilation errors remain.

### Playwright

* [ ] The application starts successfully for Playwright.
* [ ] The migration page loads.
* [ ] The sample project selector works.
* [ ] The target architecture selector works.
* [ ] The analysis submit button works.
* [ ] The loading state is observable.
* [ ] Results render after successful execution.
* [ ] Summary scorecards are visible.
* [ ] Rule results are visible.
* [ ] Gaps are visible.
* [ ] Risks are visible.
* [ ] Recommendations are visible.
* [ ] Migration tasks are visible.
* [ ] The evidence viewer opens.
* [ ] The evidence viewer closes.
* [ ] Reset returns the workflow to its initial state.
* [ ] The desktop Chromium E2E workflow passes.

---

## 15. Out of Scope for Phase 1

The following tasks are not part of Day 46 Phase 1 unless required to fix a stabilization defect:

* Adding new migration analysis rules
* Adding new target architectures
* Changing migration scoring formulas
* Redesigning the complete UI
* Adding authentication
* Adding database persistence
* Adding external AI-provider integration
* Adding PDF or document export
* Adding analytics
* Adding production deployment infrastructure
* Rewriting the migration analysis domain engine
* Changing existing migration business behavior

The purpose of this phase is stabilization, not feature expansion.

---

## 16. Expected Phase 1 Result

At the end of Day 46 Phase 1, the project should have a stable foundation for complete end-to-end validation.

The expected architecture is:

```text
Browser UI
    ↓
MigrationAnalysisForm
    ↓
useMigrationAnalysisWorkflow
    ↓
executeMigrationAnalysis
    ↓
POST /api/migration-analysis
    ↓
migrationAnalysisService
    ↓
migrationAnalysisOrchestrator
    ↓
MigrationAnalysisResult
    ↓
MigrationAnalysisResults
```

Vitest and Playwright must operate independently:

```text
Vitest
    └── Domain, service, hook, and component validation

Playwright
    └── Desktop, tablet, and mobile browser validation
```

The primary completion milestone for Phase 1 is:

> The production build succeeds, Vitest is isolated from Playwright, server-side analysis execution is preserved, and the complete desktop Chromium migration workflow passes through the browser.
