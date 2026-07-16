# Day 47 — Phase 1: Production Validation Plan

## 1. Phase Overview

Day 47 is the final technical validation and project-freeze day for **Project 2: API Migration Analyzer**.

The purpose of Phase 1 is to define a controlled validation sequence that verifies the application is stable before it is frozen for documentation, portfolio packaging, GitHub presentation, and interview use.

This phase does not introduce new product features. It establishes:

* The validation scope
* The required command sequence
* The expected results
* The evidence that must be captured
* The process for resolving failures
* The project-freeze acceptance criteria

Recommended file location:

```text
docs/day47/production_validation_plan.md
```

---

## 2. Day 47 Primary Objective

Confirm that the API Migration Analyzer is technically complete and can reliably execute its full browser workflow in a production-oriented environment.

The final application must support:

1. Loading the Migration Analyzer page.
2. Selecting a sample source project.
3. Selecting a target architecture.
4. Configuring available analysis options.
5. Submitting a migration analysis request.
6. Displaying a loading state.
7. Executing analysis through the server API.
8. Displaying summary scorecards.
9. Displaying rule results.
10. Displaying migration gaps.
11. Displaying migration risks.
12. Displaying recommendations.
13. Displaying migration tasks.
14. Opening supporting evidence.
15. Closing the evidence viewer.
16. Resetting the workflow.
17. Running another analysis.
18. Recovering from analysis and network errors.
19. Operating correctly on desktop, tablet, and mobile layouts.

---

## 3. Final Application Architecture

The application must preserve the following execution flow:

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

### Required client boundary

Client-side files may include:

* React components
* Browser workflow hooks
* API-client functions
* Browser-safe types
* Browser-safe utilities

### Required server boundary

Server-side files may include:

* Next.js API routes
* Server services
* Migration analysis orchestrator
* File-processing utilities
* Node.js APIs
* Server-only implementation details

### Prohibited dependency path

```text
Browser component
        ↓
Browser hook
        ↓
Migration analysis orchestrator
```

The browser must never directly import or execute the migration analysis orchestrator.

---

## 4. Validation Scope

Day 47 production validation covers seven technical areas.

### 4.1 Static code quality

Validate:

* ESLint
* TypeScript
* Import resolution
* Export correctness
* Client/server boundaries
* Unused code
* Duplicate declarations

### 4.2 Unit and integration tests

Validate:

* Domain utilities
* Evidence extraction
* Capability extraction
* Endpoint extraction
* Rule evaluation
* Gap generation
* Risk generation
* Recommendation generation
* Migration-task generation
* Orchestrator behavior
* Server-service behavior
* Browser workflow hook
* React components

### 4.3 Production build

Validate:

* Next.js compilation
* API-route compilation
* Client bundle generation
* Server bundle generation
* Static-page generation
* Type checking during build
* Environment configuration
* Production startup

### 4.4 Desktop browser workflow

Validate:

* Form interaction
* API execution
* Loading state
* Results rendering
* Evidence viewer
* Reset
* Rerun
* Error recovery

### 4.5 Tablet layout

Validate:

* Responsive form layout
* Scorecard layout
* Results readability
* Dialog behavior
* Touch-target usability
* Overflow protection

### 4.6 Mobile layout

Validate:

* Vertical form layout
* Single-column scorecards
* Responsive results
* Long-text wrapping
* Evidence scrolling
* Button accessibility
* Overflow protection

### 4.7 Project freeze

Validate:

* No unresolved critical defects
* No unfinished required workflow
* No temporary debugging code
* No skipped required tests
* No stale documentation claims
* No browser/server architecture violation

---

## 5. Required Validation Order

Validation must be executed in the following order:

```text
1. Dependency and environment check
        ↓
2. ESLint
        ↓
3. TypeScript validation
        ↓
4. Vitest
        ↓
5. Production build
        ↓
6. Production startup smoke test
        ↓
7. Desktop Playwright
        ↓
8. Tablet Playwright
        ↓
9. Mobile Playwright
        ↓
10. Complete Playwright suite
        ↓
11. Final project-freeze review
```

A later validation stage should not be treated as complete while an earlier stage remains failing.

For example:

* Do not treat Playwright success as sufficient if TypeScript fails.
* Do not freeze the project if the production build fails.
* Do not mark Day 47 complete while mobile validation remains broken.
* Do not ignore unit-test failures because the browser workflow appears functional.

---

## 6. Pre-Validation Environment Check

Before executing the final commands, verify the development environment.

### Required tools

* Supported Node.js version
* npm
* Chromium for Playwright
* Project dependencies
* Required environment variables

### Verify Node.js

```powershell
node --version
```

### Verify npm

```powershell
npm --version
```

### Install dependencies

```powershell
npm install
```

Use the project lock file as the source of truth.

When a clean reproducible installation is required, use:

```powershell
npm ci
```

Do not switch between `npm install` and `npm ci` without understanding whether the lock file is current.

### Install Playwright Chromium

```powershell
npx playwright install chromium
```

### Verify available npm scripts

```powershell
npm run
```

Confirm that the expected scripts are present, such as:

```json
{
  "scripts": {
    "dev": "next dev",
    "build": "next build",
    "start": "next start",
    "lint": "eslint",
    "test": "vitest",
    "test:e2e": "playwright test"
  }
}
```

The actual `package.json` remains the source of truth.

---

## 7. Phase 1 Validation Commands

The primary Day 47 command sequence is:

```powershell
npm run lint
npx tsc --noEmit
npx vitest run
npm run build
npx playwright test
```

Each command must complete with an exit code of `0`.

If the project defines equivalent npm scripts, those scripts may be used instead:

```powershell
npm run lint
npm run typecheck
npm run test
npm run build
npm run test:e2e
```

Do not execute both versions unnecessarily. Use the commands that match the current project configuration.

---

## 8. Lint Validation Plan

Run:

```powershell
npm run lint
```

### Required result

```text
ESLint errors:   0
ESLint warnings: 0
Exit code:       0
```

### Items to inspect

* Unused imports
* Unused variables
* Incorrect React hook dependencies
* Ref usage violations
* Unsafe type assertions
* Explicit `any`
* Duplicate imports
* Incorrect JSX accessibility
* Missing button types
* Invalid client/server imports
* Test-only globals in production files

### Lint failures that must not be ignored

Examples include:

```text
Cannot access refs during render
```

```text
React Hook useEffect has a missing dependency
```

```text
Unexpected any
```

```text
Variable is assigned but never used
```

```text
Import is defined but never used
```

Do not disable a lint rule merely to obtain a passing result unless the exception is technically justified and documented.

---

## 9. TypeScript Validation Plan

Run:

```powershell
npx tsc --noEmit
```

### Required result

```text
TypeScript errors: 0
Exit code:        0
```

### Areas to validate

* Migration request contract
* Migration result contract
* Workflow state
* API-client return types
* API error handling
* Server route request parsing
* Target architecture lookup
* Sample source-project lookup
* Component props
* Evidence viewer props
* Readonly arrays
* Optional result sections
* Vitest mocks
* Playwright fixtures

### Common issues to reject

* Duplicate `targetArchitectures` exports
* Incorrect relative imports
* Missing module declarations
* Assigning mutable arrays to readonly properties
* Incorrect mock return types
* Possibly `undefined` architecture selection
* API response treated as a result before checking `response.ok`
* Client files importing server-only types through runtime imports

Use type-only imports where appropriate:

```typescript
import type {
  MigrationAnalysisRequest,
  MigrationAnalysisResult,
} from "../types/migration";
```

---

## 10. Vitest Validation Plan

Run:

```powershell
npx vitest run
```

### Required result

```text
Failed suites: 0
Failed tests:  0
Unhandled errors: 0
Exit code:     0
```

### Required test categories

* Evidence helper tests
* Capability evidence extraction tests
* Endpoint evidence extraction tests
* Migration rule tests
* Gap deduplication tests
* Risk deduplication tests
* Recommendation-generation tests
* Task-generation tests
* Orchestrator tests
* Server-service tests
* Workflow-hook tests
* Component tests

### Test-runner isolation

Vitest must not execute Playwright test files.

Vitest should exclude patterns such as:

```typescript
exclude: [
  "**/node_modules/**",
  "**/dist/**",
  "**/.next/**",
  "**/e2e/**",
  "**/*.spec.ts",
  "**/*.e2e.ts",
],
```

Adjust these patterns to the project's actual naming convention.

### Testing Library cleanup

Confirm the test setup includes equivalent cleanup behavior:

```typescript
import {
  afterEach,
  vi,
} from "vitest";

import {
  cleanup,
} from "@testing-library/react";

afterEach(() => {
  cleanup();
  vi.clearAllMocks();
  vi.restoreAllMocks();
  vi.useRealTimers();
});
```

### Tests must pass in both modes

A test must pass:

* When executed individually
* When executed as part of the complete test suite

A test that passes only when run alone is not considered stable.

---

## 11. Production Build Validation Plan

Run:

```powershell
npm run build
```

### Required result

```text
Compilation: Successful
Type checking: Successful
Route generation: Successful
Exit code: 0
```

### Validate build output for

* Compilation errors
* TypeScript errors
* Missing dependencies
* Invalid dynamic imports
* Client/server boundary violations
* Static-render failures
* API-route failures
* Environment-variable failures
* Hydration-related warnings
* Workspace-root warnings
* Multiple lock-file warnings

A warning does not automatically fail the build, but every warning must be reviewed.

### Multiple lock-file warning

If Next.js reports that it inferred the wrong workspace root because multiple lock files exist, determine which lock file belongs to the application.

Possible corrections include:

* Removing an obsolete lock file
* Running commands from the intended project root
* Configuring the correct Turbopack root
* Consolidating workspace dependency management

Do not remove a lock file without confirming it is unused.

---

## 12. Production Startup Smoke Test

After the build succeeds, run:

```powershell
npm run start
```

Open:

```text
http://localhost:3000
```

### Validate

* Application starts without runtime errors.
* Migration Analyzer page loads.
* Static assets load correctly.
* Styles load correctly.
* API route is available.
* Browser console has no critical errors.
* Server console has no unhandled exceptions.
* A sample migration analysis can be executed.
* Results render in production mode.

Development-mode success alone is not sufficient for the final project freeze.

---

## 13. Playwright Validation Plan

### Desktop project

Run:

```powershell
npx playwright test --project=desktop-chromium
```

Recommended viewport:

```text
1440 × 900
```

### Tablet project

Run:

```powershell
npx playwright test --project=tablet-chromium
```

Recommended viewport:

```text
768 × 1024
```

### Mobile project

Run:

```powershell
npx playwright test --project=mobile-chromium
```

Recommended viewport:

```text
390 × 844
```

### Complete suite

After individual projects pass, run:

```powershell
npx playwright test
```

### View report

```powershell
npx playwright show-report
```

---

## 14. Required Desktop E2E Scenario

The desktop browser test must validate the complete workflow.

```text
Given the Migration Analyzer page is open
And no previous analysis result is displayed

When the user selects a sample source project
And selects a target architecture
And submits the migration analysis

Then the loading state is displayed
And the analysis request completes successfully
And the summary scorecards are visible
And the rule results are visible
And migration gaps are visible
And migration risks are visible
And recommendations are visible
And migration tasks are visible

When the user opens supporting evidence

Then the evidence viewer is displayed
And the evidence details are readable

When the user closes the evidence viewer
And resets the workflow

Then the result is removed
And the workflow returns to its initial state

When the user runs another analysis

Then the second analysis completes successfully
And no stale data from the first result remains
```

---

## 15. Required Error-Recovery Scenario

The browser suite must validate failure and recovery behavior.

```text
Given the Migration Analyzer page is open

When an analysis request fails

Then the loading state ends
And a user-readable error message is displayed
And incomplete results are not displayed

When the user resets the workflow
And submits a valid analysis

Then the error is removed
And the analysis completes successfully
And the results interface is displayed
```

### Failure conditions to cover

At least one browser or integration test should cover:

* HTTP `400`
* HTTP `500`
* Rejected network request
* Invalid target architecture
* Missing required form selection

Not every failure requires a separate E2E test if equivalent behavior is already verified through lower-level tests, but the browser-level error state must be validated.

---

## 16. Responsive Validation Plan

### Desktop requirements

* Multi-column scorecards display correctly.
* Form controls do not overlap.
* Results use available page width.
* Evidence viewer remains readable.
* No horizontal page overflow occurs.

### Tablet requirements

* Form controls remain accessible.
* Scorecards reduce columns appropriately.
* Result sections remain readable.
* Dialogs remain inside the viewport.
* Buttons provide usable touch targets.
* No horizontal page overflow occurs.

### Mobile requirements

* Form controls stack vertically.
* Labels remain connected to fields.
* Submit and reset buttons remain accessible.
* Scorecards render in one column.
* Long descriptions wrap.
* Source paths and identifiers wrap.
* Evidence content scrolls vertically.
* Dialog controls remain visible.
* No horizontal page overflow occurs.

### Overflow assertion

A browser test may verify:

```typescript
const hasHorizontalOverflow = await page.evaluate(() => {
  return document.documentElement.scrollWidth >
    document.documentElement.clientWidth;
});

expect(hasHorizontalOverflow).toBe(false);
```

---

## 17. Stable Browser Selector Plan

Playwright tests must prefer accessible selectors.

### Preferred selectors

```typescript
page.getByRole("button", {
  name: /analyze migration/i,
});
```

```typescript
page.getByLabel(/sample source project/i);
```

```typescript
page.getByLabel(/target architecture/i);
```

```typescript
page.getByRole("heading", {
  name: /migration analysis results/i,
});
```

```typescript
page.getByRole("dialog", {
  name: /migration evidence/i,
});
```

### Test IDs

Use test IDs only when a semantic selector is insufficient.

Examples:

```text
migration-overall-score
migration-rule-results
migration-gap-list
migration-risk-list
migration-recommendation-list
migration-task-list
migration-evidence-viewer
```

### Selectors to avoid

Do not use:

```typescript
page.locator("div:nth-child(3) > div > button");
```

Do not depend on:

* Generated class names
* CSS framework classes
* Exact card position
* Deep DOM hierarchy
* Arbitrary sleep delays
* Partial text that appears in multiple sections

---

## 18. Validation Evidence to Capture

The following evidence should be recorded for the Day 47 completion report.

### Command evidence

Capture the final result for:

```text
npm run lint
npx tsc --noEmit
npx vitest run
npm run build
npx playwright test
```

For each command, record:

* Date executed
* Exit code
* Number of passed tests
* Number of failed tests
* Relevant warnings
* Final status

### Browser evidence

Capture screenshots or Playwright artifacts for:

* Initial analysis form
* Loading state
* Completed summary scorecards
* Rule results
* Migration gaps
* Migration risks
* Recommendations
* Migration tasks
* Open evidence viewer
* Mobile layout
* Error state when practical

### Report evidence

Capture:

* Playwright HTML report
* Failure traces when a test initially fails
* Final successful rerun
* Remaining non-blocking limitations

---

## 19. Failure-Handling Workflow

When a validation step fails, use the following process.

### Step 1: Preserve the exact failure

Record:

* Command
* Failing file
* Failing test
* Error message
* Stack trace
* Expected result
* Actual result

### Step 2: Classify the failure

Use one category:

```text
IMPORT
TYPE
LINT
UNIT_TEST
COMPONENT_TEST
SERVER_API
BUILD
BROWSER_SELECTOR
WORKFLOW_STATE
RESPONSIVE_LAYOUT
TEST_CONFIGURATION
ENVIRONMENT
```

### Step 3: Fix the smallest responsible layer

Examples:

* Selector mismatch → fix selector or accessibility contract.
* Incorrect result mock → fix test fixture.
* Client imports server module → fix architecture boundary.
* Missing result field → fix shared type or orchestrator output.
* Mobile overflow → fix responsive layout.
* State leakage → fix cleanup.
* Playwright loaded by Vitest → fix test configuration.

### Step 4: Run focused validation

Examples:

```powershell
npx vitest run src/hooks/useMigrationAnalysisWorkflow.test.ts
```

```powershell
npx playwright test e2e/migration-analysis.spec.ts --project=desktop-chromium
```

### Step 5: Run the full affected suite

After the focused test passes, rerun the complete relevant suite.

### Step 6: Run the final command sequence

A focused passing test is not sufficient for project freeze.

---

## 20. Defect Severity Classification

### Critical

A defect is critical when it:

* Prevents the application from starting.
* Prevents production build completion.
* Prevents migration analysis execution.
* Causes data or result corruption.
* Breaks all browser workflows.
* Violates the client/server boundary.
* Causes the complete test suite to fail.

Critical defects block Day 47 completion.

### High

A defect is high when it:

* Breaks a required result section.
* Prevents reset or rerun.
* Breaks the evidence viewer.
* Breaks tablet or mobile operation.
* Causes important API errors to be hidden.
* Creates duplicate requests.

High defects block project freeze.

### Medium

A defect is medium when it:

* Causes layout inconsistency.
* Produces a non-critical accessibility issue.
* Displays unclear text.
* Causes a minor responsive defect.
* Creates a warning without runtime impact.

Medium defects should be fixed when feasible. Any deferred medium defect must be documented.

### Low

A defect is low when it:

* Is cosmetic.
* Does not affect the required workflow.
* Does not affect build or test stability.
* Can be safely handled after the roadmap.

Low defects may be deferred and documented.

---

## 21. Project-Freeze Rules

After Day 47 is complete:

* Do not add new migration rules during the documentation phase.
* Do not add new target architectures during the documentation phase.
* Do not redesign the application unless a critical presentation issue is discovered.
* Do not refactor stable domain logic without a demonstrated defect.
* Do not rename public interfaces casually.
* Do not alter deterministic sample outputs without updating tests and documentation.
* Do not add unrelated dependencies.
* Do not commit temporary debugging code.
* Do not leave skipped tests without an explanation.

Allowed post-freeze changes include:

* README updates
* Screenshots
* Architecture diagrams
* Repository metadata
* Typographical corrections
* Portfolio descriptions
* Interview documentation
* Critical defect fixes

Any critical code fix after freeze must be followed by the complete Day 47 validation sequence again.

---

## 22. Out of Scope

The following work is outside Day 47 unless required to resolve a release-blocking defect:

* New migration-analysis capabilities
* New target architectures
* New rule families
* External AI-provider integration
* Authentication
* Database persistence
* Export to PDF
* Export to CSV
* User accounts
* Multi-user collaboration
* Cloud deployment
* Analytics dashboards
* Performance optimization unrelated to a measured defect
* Research-paper drafting
* LinkedIn content
* Resume updates

Those activities belong to later roadmap phases.

---

## 23. Day 47 Acceptance Checklist

### Environment

* [ ] Correct project directory confirmed.
* [ ] Supported Node.js version confirmed.
* [ ] Dependencies installed.
* [ ] Playwright Chromium installed.
* [ ] Required environment variables available.
* [ ] Expected npm scripts confirmed.

### Lint

* [ ] ESLint completes successfully.
* [ ] ESLint reports zero errors.
* [ ] ESLint reports zero warnings.
* [ ] No rules were disabled only to hide defects.

### TypeScript

* [ ] `tsc --noEmit` completes successfully.
* [ ] No duplicate declarations remain.
* [ ] No unresolved imports remain.
* [ ] Request and result types are consistent.
* [ ] Client/server type imports are safe.

### Vitest

* [ ] All suites pass.
* [ ] All tests pass.
* [ ] No Playwright tests are discovered.
* [ ] No unhandled errors occur.
* [ ] No mock leakage occurs.
* [ ] No DOM leakage occurs.
* [ ] Fake timers are restored.

### Production build

* [ ] Next.js build succeeds.
* [ ] API route compiles.
* [ ] Client bundle compiles.
* [ ] Server bundle compiles.
* [ ] No client/server boundary violation occurs.
* [ ] Build warnings have been reviewed.
* [ ] Production server starts.

### Desktop E2E

* [ ] Page loads.
* [ ] Source project selection works.
* [ ] Target architecture selection works.
* [ ] Analysis submission works.
* [ ] Loading state appears.
* [ ] Results render.
* [ ] Scorecards render.
* [ ] Rule results render.
* [ ] Gaps render.
* [ ] Risks render.
* [ ] Recommendations render.
* [ ] Tasks render.
* [ ] Evidence viewer works.
* [ ] Reset works.
* [ ] Rerun works.
* [ ] Error recovery works.
* [ ] No horizontal overflow occurs.

### Tablet E2E

* [ ] Full workflow passes.
* [ ] Controls remain usable.
* [ ] Scorecards adapt correctly.
* [ ] Results remain readable.
* [ ] Evidence viewer fits the viewport.
* [ ] No horizontal overflow occurs.

### Mobile E2E

* [ ] Full workflow passes.
* [ ] Controls stack correctly.
* [ ] Buttons remain accessible.
* [ ] Scorecards render in one column.
* [ ] Long text wraps correctly.
* [ ] Evidence content is scrollable.
* [ ] No horizontal overflow occurs.

### Freeze

* [ ] No critical defects remain.
* [ ] No high-severity defects remain.
* [ ] Deferred issues are documented.
* [ ] Temporary debugging code is removed.
* [ ] Skipped tests are reviewed.
* [ ] Validation evidence is captured.
* [ ] Project 2 is ready for Day 48 documentation.

---

## 24. Final Validation Record Template

Use the following template while completing Day 47:

```text
Day 47 Production Validation Record

Validation Date:
[YYYY-MM-DD]

Project:
API Migration Analyzer

Node.js Version:
[VERSION]

npm Version:
[VERSION]

1. ESLint
Command:
npm run lint

Status:
[PASS / FAIL]

Errors:
[COUNT]

Warnings:
[COUNT]

Notes:
[DETAILS]

2. TypeScript
Command:
npx tsc --noEmit

Status:
[PASS / FAIL]

Errors:
[COUNT]

Notes:
[DETAILS]

3. Vitest
Command:
npx vitest run

Status:
[PASS / FAIL]

Test Files Passed:
[COUNT]

Tests Passed:
[COUNT]

Tests Failed:
[COUNT]

Notes:
[DETAILS]

4. Production Build
Command:
npm run build

Status:
[PASS / FAIL]

Warnings:
[DETAILS]

Notes:
[DETAILS]

5. Desktop Playwright
Command:
npx playwright test --project=desktop-chromium

Status:
[PASS / FAIL]

Passed:
[COUNT]

Failed:
[COUNT]

Notes:
[DETAILS]

6. Tablet Playwright
Command:
npx playwright test --project=tablet-chromium

Status:
[PASS / FAIL]

Passed:
[COUNT]

Failed:
[COUNT]

Notes:
[DETAILS]

7. Mobile Playwright
Command:
npx playwright test --project=mobile-chromium

Status:
[PASS / FAIL]

Passed:
[COUNT]

Failed:
[COUNT]

Notes:
[DETAILS]

8. Complete Playwright Suite
Command:
npx playwright test

Status:
[PASS / FAIL]

Passed:
[COUNT]

Failed:
[COUNT]

Notes:
[DETAILS]

Remaining Defects:
[LIST OR NONE]

Deferred Improvements:
[LIST OR NONE]

Final Freeze Decision:
[APPROVED / BLOCKED]
```

---

## 25. Phase 1 Completion Criteria

Day 47 Phase 1 is complete when:

* The validation scope is documented.
* The command order is documented.
* Required expected results are defined.
* Browser validation scenarios are defined.
* Responsive requirements are defined.
* Error-recovery requirements are defined.
* Failure classification is defined.
* Freeze rules are defined.
* Evidence requirements are defined.
* The final acceptance checklist is ready for execution.

Phase 1 does not declare the project technically complete.

It declares that the project is:

```text
READY FOR FINAL PRODUCTION VALIDATION
```

---

## 26. Expected Phase 1 Outcome

At the end of Phase 1, the team has a repeatable production validation process:

```text
Code quality
    ↓
Type safety
    ↓
Unit and integration correctness
    ↓
Production compilation
    ↓
Production runtime
    ↓
Desktop browser correctness
    ↓
Tablet browser correctness
    ↓
Mobile browser correctness
    ↓
Project freeze
```

The remaining Day 47 phases will execute this plan, resolve identified defects, capture evidence, and determine whether Project 2 can be officially frozen.
