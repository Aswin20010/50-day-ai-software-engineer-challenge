# Day 50 – Phase 2

# Final Technical Validation Report

## Projects

1. **AI-Assisted API Factory**
2. **API Migration Analyzer**

---

# 1. Phase 2 Objective

The objective of Day 50 Phase 2 is to define and record the final technical validation process for both projects.

This phase verifies:

* Dependency installation.
* ESLint compliance.
* TypeScript compilation.
* Unit-test execution.
* Production build readiness.
* Project-specific validation scripts.
* Manual browser behavior.
* Playwright status.
* Known warnings and failures.
* Final technical readiness.

This document provides the exact validation order, commands, evidence format, troubleshooting rules, and result templates.

The final status of a validation step must be based on the actual command output. A planned or skipped validation must not be reported as passed.

---

# 2. Recommended File Location

```text
docs/day50/final_validation_report.md
```

---

# 3. Validation Status Definitions

Use only the following statuses:

| Status               | Meaning                                                      |
| -------------------- | ------------------------------------------------------------ |
| Passed               | Command or validation completed successfully                 |
| Passed with warnings | Completed successfully but produced non-blocking warnings    |
| Failed               | Command returned an error or unacceptable warning            |
| Blocked              | Validation could not run because of another unresolved issue |
| Skipped              | Validation was intentionally not executed                    |
| Not applicable       | Validation does not apply to the project                     |
| Pending              | Validation has not yet been executed                         |

A skipped or blocked validation must include:

* Reason.
* Impact.
* Required follow-up.
* Whether it blocks release.

---

# 4. Validation Principles

The final validation must follow these rules:

1. Run commands from the correct repository root.
2. Save complete command output.
3. Do not hide warnings.
4. Do not manually edit results after execution.
5. Fix one validation category at a time.
6. Rerun a failed command after applying a fix.
7. Record skipped tests explicitly.
8. Separate automated browser validation from manual browser validation.
9. Do not claim production readiness based only on unit tests.
10. Do not report planned research evaluation as technical validation.
11. Do not upgrade dependencies during final validation unless required.
12. Preserve the final Git commit or working-tree state used for validation.

---

# 5. Validation Scope

The technical validation includes:

## API Factory

* Repository status.
* Node.js and npm versions.
* Clean dependency installation.
* ESLint.
* Unit tests.
* TypeScript validation.
* Production build.
* Manual browser validation.
* Environment-independent startup.
* Known warning review.

## API Migration Analyzer

* Repository status.
* Node.js and npm versions.
* Clean dependency installation.
* ESLint with zero allowed warnings.
* Unit tests.
* TypeScript validation.
* Production build.
* Project validation script.
* Manual browser validation.
* Playwright final classification.
* Known warning review.

---

# 6. Pre-Validation Environment Record

Before running project commands, record the local environment.

Run:

```powershell
node --version
npm --version
git --version
```

Optional when Playwright is installed:

```powershell
npx playwright --version
```

Record:

```text
Operating system:
Node.js version:
npm version:
Git version:
Playwright version:
Validation date:
Validated by:
```

The final report should state the exact versions used.

---

# 7. Repository State Validation

Run from each project root:

```powershell
git status
git branch --show-current
git log -1 --oneline
```

Record:

* Repository path.
* Current branch.
* Latest commit.
* Modified tracked files.
* Untracked files.
* Whether validation was run on a clean working tree.

A clean working tree is preferred but not required during development. Any uncommitted changes must be understood and documented.

---

# 8. Validation Evidence Folder

Store validation evidence under:

```text
docs/day50/validation-evidence/
```

Recommended structure:

```text
docs/
└── day50/
    └── validation-evidence/
        ├── api-factory/
        │   ├── environment.txt
        │   ├── git-status.txt
        │   ├── npm-ci.txt
        │   ├── lint.txt
        │   ├── test.txt
        │   ├── typecheck.txt
        │   ├── build.txt
        │   └── manual-browser-validation.md
        └── api-migration-analyzer/
            ├── environment.txt
            ├── git-status.txt
            ├── npm-ci.txt
            ├── lint.txt
            ├── test.txt
            ├── typecheck.txt
            ├── build.txt
            ├── validate-phase2.txt
            ├── playwright.txt
            └── manual-browser-validation.md
```

Do not commit evidence that exposes local usernames, secrets, or confidential paths.

---

# 9. PowerShell Output Capture

Use `Tee-Object` to display and save command output.

Example:

```powershell
npm run lint 2>&1 |
Tee-Object -FilePath ".\docs\day50\validation-evidence\lint.txt"
```

To preserve the exit code:

```powershell
npm run lint 2>&1 |
Tee-Object -FilePath ".\docs\day50\validation-evidence\lint.txt"

$lintExitCode = $LASTEXITCODE

if ($lintExitCode -ne 0) {
    Write-Host "Lint failed with exit code $lintExitCode"
    exit $lintExitCode
}
```

When running commands manually one after another, record `$LASTEXITCODE` before executing another command.

---

# 10. Project 1 – API Factory Validation Order

Run the API Factory validation in this order:

1. Confirm repository state.
2. Confirm Node.js and npm versions.
3. Install dependencies.
4. Run ESLint.
5. Run unit tests.
6. Run TypeScript validation.
7. Run production build.
8. Start the application.
9. Perform manual browser validation.
10. Record final status.

---

# 11. API Factory – Dependency Installation

## Preferred Clean Installation

```powershell
npm ci
```

Use `npm install` only when:

* The lockfile is missing.
* The lockfile is intentionally being regenerated.
* A dependency change is part of the current work.

## Pass Criteria

* Command exits with code `0`.
* Dependencies install without missing-package errors.
* Lockfile and package manifest are consistent.
* No install script fails.
* No real credential is requested during installation.

## Warning Handling

Record:

* Deprecated packages.
* Peer-dependency warnings.
* Security audit warnings.
* Unsupported engine warnings.

Security audit warnings should be reviewed separately. Do not run `npm audit fix --force` automatically during final validation.

---

# 12. API Factory – ESLint Validation

Run:

```powershell
npm run lint
```

For strict validation:

```powershell
npm run lint -- --max-warnings=0
```

## Pass Criteria

* Zero ESLint errors.
* Zero warnings when `--max-warnings=0` is used.
* No accessibility warning is ignored without explanation.
* No unused import or variable warning remains.
* No rule is disabled solely to hide a defect.

## Result Template

```text
Command:
Exit code:
Errors:
Warnings:
Files affected:
Status:
Corrective action:
Rerun result:
```

---

# 13. API Factory – Unit-Test Validation

Run:

```powershell
npm run test
```

When the script does not exist, inspect `package.json` and use the configured test command.

Possible direct command:

```powershell
npx vitest run
```

## Pass Criteria

* Test runner exits with code `0`.
* No failed suite.
* No failed test.
* No unhandled rejection.
* No unexpected snapshot change.
* No test is silently skipped.

## Test Areas to Confirm

* Requirement extraction helpers.
* Requirement normalization.
* Missing-field detection.
* Rule construction.
* Extraction workflow state.
* State reset.
* Form validation.
* Error-state handling.

## Skipped Test Review

Search:

```powershell
Get-ChildItem .\src -Recurse -File |
Where-Object {
    $_.Extension -in @(".ts", ".tsx", ".js", ".jsx")
} |
Select-String -Pattern `
    'describe\.skip',
    'it\.skip',
    'test\.skip',
    'xit\(',
    'xdescribe\('
```

Document every skipped test.

---

# 14. API Factory – TypeScript Validation

First inspect `package.json` for a type-check script.

Preferred:

```powershell
npm run typecheck
```

When no script exists:

```powershell
npx tsc --noEmit
```

## Pass Criteria

* Zero TypeScript errors.
* Path aliases resolve.
* Tests compile under the intended configuration.
* No missing type declaration blocks compilation.
* No source file depends on a local-only path.

If `next build` performs the complete type check, record that relationship. A separate `typecheck` script is still useful for quicker validation.

---

# 15. API Factory – Production Build

Run:

```powershell
npm run build
```

## Pass Criteria

* Next.js build completes.
* TypeScript validation completes.
* Required routes compile.
* No missing environment variable blocks a deterministic sample build.
* No module-resolution error occurs.
* No server/client boundary error occurs.
* No static-generation failure occurs.

## Warning Review

Record:

* Workspace-root warning.
* Deprecated configuration warning.
* Bundle-size warning.
* Dynamic rendering warning.
* Missing metadata warning.

A successful command with significant warnings should be classified as `Passed with warnings`.

---

# 16. API Factory – Application Startup

After a successful build, run:

```powershell
npm run start
```

For development-mode manual validation:

```powershell
npm run dev
```

Record the local URL printed by the application.

## Startup Pass Criteria

* Server starts without crashing.
* Main page loads.
* No immediate terminal exception.
* No browser-console error blocks the workflow.
* Static assets load.
* Main interactions are available.

Stop the application after validation with:

```text
Ctrl+C
```

---

# 17. API Factory – Manual Browser Validation

Validate the main workflow manually.

## Input State

* [ ] Main page loads.
* [ ] Requirement input is visible.
* [ ] File input works where supported.
* [ ] Required controls have labels.
* [ ] Submit action is initially valid or correctly disabled.
* [ ] Sample input can be selected or entered.

## Execution State

* [ ] Extraction starts after submission.
* [ ] Loading state is visible.
* [ ] Repeated submission is prevented where necessary.
* [ ] Unexpected errors are handled.

## Success State

* [ ] Flow name renders.
* [ ] Extracted endpoint information renders.
* [ ] Request and response fields render.
* [ ] Validation rules render.
* [ ] Business rules render.
* [ ] Missing fields render.
* [ ] Normalized output is visible where designed.
* [ ] Reset action clears the workflow safely.

## Error State

* [ ] Empty or invalid input produces a useful message.
* [ ] Error does not expose a stack trace.
* [ ] Error does not expose local paths.
* [ ] User can correct input and retry.

## Responsive State

* [ ] Desktop layout is usable.
* [ ] Narrow viewport remains readable.
* [ ] Controls do not overlap.
* [ ] Long content wraps or scrolls correctly.

---

# 18. API Factory Validation Result Template

| Validation               | Command or Method                         | Status  | Evidence | Notes |
| ------------------------ | ----------------------------------------- | ------- | -------- | ----- |
| Repository state         | `git status`                              | Pending | TBD      |       |
| Dependency install       | `npm ci`                                  | Pending | TBD      |       |
| ESLint                   | `npm run lint -- --max-warnings=0`        | Pending | TBD      |       |
| Unit tests               | `npm run test`                            | Pending | TBD      |       |
| TypeScript               | `npm run typecheck` or `npx tsc --noEmit` | Pending | TBD      |       |
| Production build         | `npm run build`                           | Pending | TBD      |       |
| Startup                  | `npm run start`                           | Pending | TBD      |       |
| Manual browser workflow  | Manual                                    | Pending | TBD      |       |
| Responsive validation    | Manual                                    | Pending | TBD      |       |
| Final API Factory status | Combined                                  | Pending | TBD      |       |

---

# 19. Project 2 – Migration Analyzer Validation Order

Run the Migration Analyzer validation in this order:

1. Confirm repository state.
2. Confirm Node.js and npm versions.
3. Install dependencies.
4. Run ESLint with zero warnings.
5. Run unit tests.
6. Run TypeScript validation.
7. Run production build.
8. Run `validate:phase2`.
9. Start the application.
10. Perform manual browser validation.
11. Determine Playwright status.
12. Record final status.

---

# 20. Migration Analyzer – Dependency Installation

Run:

```powershell
npm ci
```

## Pass Criteria

* Exit code `0`.
* Package manifest and lockfile agree.
* No package is missing.
* No test or build dependency fails to install.
* No browser installation is unexpectedly required for non-browser tests.

If Playwright browsers are needed later, install them separately:

```powershell
npx playwright install
```

This should not be required for lint, unit tests, or production build.

---

# 21. Migration Analyzer – ESLint Validation

Run:

```powershell
npm run lint -- --max-warnings=0
```

## Known Prior Issue

The file:

```text
scripts/day47-build-validation.mjs
```

previously had an unused `spawn` import.

Before validation, confirm:

* The unused import has been removed or used.
* No warning remains in validation scripts.
* Accessibility warnings are resolved.
* `aria-pressed` is used only on proper toggle controls.
* Test files comply with the lint configuration.

## Pass Criteria

* Zero errors.
* Zero warnings.
* Exit code `0`.

---

# 22. Migration Analyzer – Unit-Test Validation

Run:

```powershell
npm run test
```

Possible direct command:

```powershell
npx vitest run
```

## Key Test Areas

Confirm coverage for:

* `evidenceHelpers`
* `extractEndpointEvidence`
* `extractCapabilityEvidence`
* Gap deduplication
* Risk deduplication
* Recommendation generation
* Migration-task generation
* Analysis orchestrator
* Browser request builder
* Browser form validation
* Browser sample-project mapping

## Prior Failure Area

A previous test issue involved:

```text
selectedProject.request
```

Review tests around browser sample projects and form request construction.

The final tests should reflect the actual sample-project structure rather than an obsolete `request` property.

## Pass Criteria

* All suites pass.
* No unexpected skipped tests.
* No unhandled errors.
* No test uses unavailable local files.
* Test results are deterministic across reruns.

---

# 23. Migration Analyzer – Targeted Test Commands

When the full suite fails, isolate the affected module.

Examples:

```powershell
npx vitest run src/lib/migration-analysis/evidenceHelpers.test.ts
```

```powershell
npx vitest run src/lib/migration-analysis/extractEndpointEvidence.test.ts
```

```powershell
npx vitest run src/lib/migration-analysis/extractCapabilityEvidence.test.ts
```

```powershell
npx vitest run src/lib/migration-analysis/migrationAnalysisOrchestrator.test.ts
```

```powershell
npx vitest run src/lib/migration-analyzer-browser/migrationAnalyzerBrowser.test.ts
```

Use the actual repository paths if they differ.

A targeted pass does not replace the required final full-suite run.

---

# 24. Migration Analyzer – TypeScript Validation

Run:

```powershell
npm run typecheck
```

When unavailable:

```powershell
npx tsc --noEmit
```

## Pass Criteria

* Zero TypeScript errors.
* Browser sample-project types match their data.
* Target architecture IDs resolve.
* Request-builder return type matches orchestrator input.
* Evidence, gap, risk, recommendation, and task models align.
* Readonly arrays are used consistently where expected.
* No duplicate export causes a conflict.

---

# 25. Migration Analyzer – Production Build

Run:

```powershell
npm run build
```

## Pass Criteria

* Production build completes.
* Type checking passes.
* Static routes compile.
* Browser components obey client/server boundaries.
* Sample data can be imported during build.
* No environment-specific file is required.
* No dynamic import fails.
* No unsupported Node.js API is bundled into a client component.

## Known Warning Area

The project previously showed a Next.js workspace-root warning.

If the build passes with this warning:

1. Record it.
2. Determine whether it affects correctness.
3. Fix the workspace configuration where safe.
4. Otherwise classify the build as `Passed with warnings`.

---

# 26. Migration Analyzer – Project Validation Script

Run:

```powershell
npm run validate:phase2
```

The script previously executed validation stages such as:

* ESLint.
* Unit tests.
* Production build.
* Additional project validation.

## Audit the Script Before Trusting It

Confirm:

* It runs current scripts.
* It does not refer to deleted files.
* It checks exit codes.
* It does not treat failed commands as success.
* It prints a clear final summary.
* Optional browser validation is labeled optional.
* No unused imports create lint warnings.

## Pass Criteria

* All required stages pass.
* Script exits with code `0`.
* No required validation is silently skipped.
* Final summary matches the actual command results.

---

# 27. Suggested `validate:phase2` Result Format

The script should produce a result similar to:

```text
============================================================

Validation: ESLint
Status: PASSED

Validation: Unit tests
Status: PASSED

Validation: Production build
Status: PASSED

Validation: Browser automation
Status: SKIPPED
Reason: Playwright workflow not stabilized

============================================================

Required validations passed: 3
Required validations failed: 0
Optional validations skipped: 1

Overall status: PASSED WITH DOCUMENTED LIMITATION
```

This is a format example, not an achieved result.

---

# 28. Migration Analyzer – Application Startup

Run:

```powershell
npm run dev
```

After a successful build, also validate:

```powershell
npm run start
```

## Pass Criteria

* Application starts.
* Main page loads.
* Sample project data loads.
* Target architecture options load.
* No terminal crash occurs.
* No blocking browser-console error occurs.

---

# 29. Migration Analyzer – Manual Browser Validation

## Form State

* [ ] Sample project options render.
* [ ] Target architecture options render.
* [ ] Default selection is valid or clearly empty.
* [ ] Selected-state styling is visible.
* [ ] Keyboard interaction works.
* [ ] Required-field errors render clearly.
* [ ] Analyze button state is correct.

## Execution State

* [ ] Analysis begins after valid submission.
* [ ] Loading indicator appears.
* [ ] Duplicate submission is prevented.
* [ ] Prior error state is cleared appropriately.

## Summary State

* [ ] Overall readiness or summary classification renders.
* [ ] Endpoint count is accurate.
* [ ] Evidence count is accurate.
* [ ] Gap count is accurate.
* [ ] Risk count is accurate.
* [ ] Recommendation count is accurate.
* [ ] Task count is accurate.

## Rule Results

* [ ] Passed rules render.
* [ ] Partial rules render.
* [ ] Failed rules render.
* [ ] Not-applicable rules render safely where supported.
* [ ] Insufficient-evidence results render safely where supported.
* [ ] Severity or weight is represented correctly.

## Gap Results

* [ ] Gap title and description render.
* [ ] Severity renders.
* [ ] Expected capability renders.
* [ ] Observed evidence renders.
* [ ] Duplicate gaps are absent.
* [ ] Empty gap state renders correctly.

## Risk Results

* [ ] Risk category renders.
* [ ] Severity renders.
* [ ] Risk description renders.
* [ ] Linked gaps or evidence render.
* [ ] Duplicate risks are absent.
* [ ] Empty risk state renders correctly.

## Recommendations

* [ ] Recommendations render.
* [ ] Priorities render.
* [ ] Linked findings render.
* [ ] Duplicate recommendations are absent.

## Migration Tasks

* [ ] Task title renders.
* [ ] Description renders.
* [ ] Priority renders.
* [ ] Dependencies render safely.
* [ ] Acceptance criteria render.
* [ ] Affected artifacts render.
* [ ] Empty task state renders correctly.

## Evidence Viewer

* [ ] Evidence category renders.
* [ ] Artifact name or path renders.
* [ ] Evidence description renders.
* [ ] Linked rule or finding is visible.
* [ ] Long evidence content does not break layout.
* [ ] Missing source location is handled safely.

## Error State

* [ ] Invalid form submission is blocked.
* [ ] Analysis error message is readable.
* [ ] Error does not expose a stack trace.
* [ ] User can modify input and retry.
* [ ] Prior successful result does not create misleading stale data.

## Responsive State

* [ ] Desktop layout works.
* [ ] Tablet-width layout works.
* [ ] Narrow layout remains usable.
* [ ] Scorecards wrap correctly.
* [ ] Tables or evidence sections scroll safely.
* [ ] No horizontal overflow blocks controls.

---

# 30. Manual Browser Validation Cases

Use at least three sample scenarios.

## Scenario 1 – Well-Structured API

Expected:

* Higher readiness.
* More passed rules.
* Few high-severity gaps.
* Few high-severity risks.

## Scenario 2 – Legacy API

Expected:

* Lower readiness.
* Multiple gaps.
* Multiple risks.
* Actionable recommendations.
* Ordered migration tasks.

## Scenario 3 – Invalid or Incomplete Form

Expected:

* Form validation prevents execution.
* Missing selections are identified.
* No analysis result is produced.
* User can correct and retry.

---

# 31. Playwright Final Status Decision

The Playwright status must be classified as one of the following.

## Option A – Passed

Use only when:

```powershell
npx playwright test
```

completes with no failed tests.

## Option B – Failed

Use when Playwright executes but one or more required tests fail.

## Option C – Skipped With Documented Limitation

Use when Playwright remains unstable and Day 50 relies on:

* Unit tests.
* Lint.
* TypeScript validation.
* Production build.
* Manual browser validation.

## Option D – Removed From Supported Validation

Use only if:

* Playwright configuration and tests are removed intentionally.
* README does not claim browser automation.
* Manual browser validation is documented.
* Future work records the missing end-to-end automation.

The current roadmap context supports Option C when errors remain unresolved.

---

# 32. Playwright Commands

When attempting browser validation:

```powershell
npx playwright install
```

```powershell
npx playwright test
```

For a specific test:

```powershell
npx playwright test tests/migration-analyzer.spec.ts
```

For headed debugging:

```powershell
npx playwright test --headed
```

For report viewing:

```powershell
npx playwright show-report
```

Use actual test paths from the repository.

---

# 33. Playwright Failure Record

Record:

```text
Command:
Test file:
Browser:
Failure stage:
Error message:
Number of passed tests:
Number of failed tests:
Number of skipped tests:
Root cause:
Attempted fixes:
Final classification:
Release impact:
Follow-up:
```

Do not paste an entire extremely long stack trace into the main validation report. Preserve it in the evidence file and summarize the root failure.

---

# 34. Accessibility Validation

Run lint-based accessibility checks as part of ESLint.

Manual checks should confirm:

* Buttons use button elements.
* Labels are associated with inputs.
* Selection controls expose selected state.
* `aria-pressed` is used only for toggle buttons.
* Keyboard focus is visible.
* Error text is associated with invalid inputs.
* Content is understandable without color alone.

Optional automated checks may be added later with tools such as Axe, but they should not be reported as completed unless executed.

---

# 35. Browser Console Validation

During manual browser testing:

1. Open browser developer tools.
2. Clear the console.
3. Reload the page.
4. Execute the complete workflow.
5. Record blocking errors and repeated warnings.

## Pass Criteria

* No uncaught exception.
* No hydration failure.
* No missing key warning repeated across lists.
* No failed local asset request.
* No accessibility warning caused by incorrect component usage.
* No sensitive data is logged.

Non-blocking framework development messages should be classified separately.

---

# 36. Network Validation

During manual testing, inspect the Network panel where relevant.

Confirm:

* Required application assets return successfully.
* No unexpected external request is made in deterministic mode.
* AI-enabled requests are clearly controlled.
* Failed requests produce a useful UI error.
* No API key appears in browser-visible request URLs.
* No source maps expose confidential content in a public build.

---

# 37. Final Warning Classification

Classify warnings as:

| Classification      | Meaning                                            |
| ------------------- | -------------------------------------------------- |
| Blocking            | Must be fixed before completion                    |
| Major               | Does not immediately fail, but affects reliability |
| Minor               | Low-impact issue that should be documented         |
| Informational       | Does not require action                            |
| Accepted limitation | Intentionally deferred with documented reason      |

Examples:

* ESLint warning under zero-warning policy: Blocking.
* Failed unit test: Blocking.
* Build workspace-root warning: Minor or Major depending on impact.
* Skipped Playwright: Accepted limitation when manual validation passes.
* Missing optional metadata: Informational.
* Accessibility warning: Major or Blocking depending on user impact.

---

# 38. Failure Resolution Order

Fix failures in this order:

1. Dependency installation.
2. TypeScript syntax and type errors.
3. ESLint errors and warnings.
4. Unit-test failures.
5. Production build failures.
6. Project validation-script failures.
7. Runtime startup failures.
8. Manual browser workflow failures.
9. Playwright failures.
10. Minor documentation warnings.

A downstream command should not be trusted until prerequisite failures are resolved.

---

# 39. Validation Rerun Policy

After applying a fix:

* Rerun the failed targeted command.
* Rerun the complete related validation category.
* Rerun the production build when source code changes.
* Rerun the full test suite after targeted tests pass.
* Rerun `validate:phase2` after any Migration Analyzer validation fix.
* Update evidence files.
* Record the final successful output, while preserving the failure summary.

---

# 40. Combined PowerShell Sequence – API Factory

Run from the API Factory root:

```powershell
$ErrorActionPreference = "Stop"

Write-Host "`n=== Environment ==="
node --version
npm --version
git branch --show-current
git status --short

Write-Host "`n=== Install ==="
npm ci
if ($LASTEXITCODE -ne 0) {
    throw "npm ci failed"
}

Write-Host "`n=== Lint ==="
npm run lint -- --max-warnings=0
if ($LASTEXITCODE -ne 0) {
    throw "Lint failed"
}

Write-Host "`n=== Unit Tests ==="
npm run test
if ($LASTEXITCODE -ne 0) {
    throw "Unit tests failed"
}

Write-Host "`n=== TypeScript ==="
if ((Get-Content package.json -Raw) -match '"typecheck"') {
    npm run typecheck
} else {
    npx tsc --noEmit
}

if ($LASTEXITCODE -ne 0) {
    throw "TypeScript validation failed"
}

Write-Host "`n=== Production Build ==="
npm run build
if ($LASTEXITCODE -ne 0) {
    throw "Production build failed"
}

Write-Host "`nAPI Factory required automated validations passed."
```

This script assumes a `test` script exists. Adjust it only after checking `package.json`.

---

# 41. Combined PowerShell Sequence – Migration Analyzer

Run from the Migration Analyzer root:

```powershell
$ErrorActionPreference = "Stop"

Write-Host "`n=== Environment ==="
node --version
npm --version
git branch --show-current
git status --short

Write-Host "`n=== Install ==="
npm ci
if ($LASTEXITCODE -ne 0) {
    throw "npm ci failed"
}

Write-Host "`n=== Lint ==="
npm run lint -- --max-warnings=0
if ($LASTEXITCODE -ne 0) {
    throw "Lint failed"
}

Write-Host "`n=== Unit Tests ==="
npm run test
if ($LASTEXITCODE -ne 0) {
    throw "Unit tests failed"
}

Write-Host "`n=== TypeScript ==="
if ((Get-Content package.json -Raw) -match '"typecheck"') {
    npm run typecheck
} else {
    npx tsc --noEmit
}

if ($LASTEXITCODE -ne 0) {
    throw "TypeScript validation failed"
}

Write-Host "`n=== Production Build ==="
npm run build
if ($LASTEXITCODE -ne 0) {
    throw "Production build failed"
}

Write-Host "`n=== Project Validation ==="
npm run validate:phase2
if ($LASTEXITCODE -ne 0) {
    throw "validate:phase2 failed"
}

Write-Host "`nMigration Analyzer required automated validations passed."
```

---

# 42. Combined Command Caution

The combined sequences are useful only after individual commands have been tested.

During troubleshooting:

* Run commands individually.
* Preserve complete output.
* Fix the first failure.
* Do not continue through multiple failing stages.
* Do not use `;` in PowerShell when later commands should depend on earlier success.

---

# 43. Validation Result Record

Use one record per command.

```md
## Validation Record

### Project

API Factory or API Migration Analyzer

### Validation

Lint, test, typecheck, build, or browser

### Command

`npm run lint -- --max-warnings=0`

### Started

TBD

### Completed

TBD

### Exit Code

TBD

### Status

Pending

### Summary

TBD

### Errors

TBD

### Warnings

TBD

### Corrective Action

TBD

### Rerun Result

TBD

### Evidence File

TBD
```

---

# 44. API Factory Final Validation Summary Template

```md
## API Factory Final Validation Summary

| Validation | Status | Evidence |
|---|---|---|
| Dependency installation | Pending | |
| ESLint | Pending | |
| Unit tests | Pending | |
| TypeScript | Pending | |
| Production build | Pending | |
| Application startup | Pending | |
| Manual browser workflow | Pending | |
| Responsive behavior | Pending | |

### Blocking Issues

TBD

### Accepted Limitations

TBD

### Final Classification

Pending
```

---

# 45. Migration Analyzer Final Validation Summary Template

```md
## Migration Analyzer Final Validation Summary

| Validation | Status | Evidence |
|---|---|---|
| Dependency installation | Pending | |
| ESLint with zero warnings | Pending | |
| Unit tests | Pending | |
| TypeScript | Pending | |
| Production build | Pending | |
| `validate:phase2` | Pending | |
| Application startup | Pending | |
| Manual browser workflow | Pending | |
| Evidence viewer | Pending | |
| Responsive behavior | Pending | |
| Playwright | Pending | |

### Blocking Issues

TBD

### Accepted Limitations

TBD

### Final Classification

Pending
```

---

# 46. Final Technical Readiness Classifications

Use one classification per project.

## Ready

Use when:

* Required dependency installation passes.
* ESLint passes.
* Unit tests pass.
* TypeScript passes.
* Production build passes.
* Main browser workflow works.
* No blocking issue remains.

## Ready With Documented Limitations

Use when all required validation passes, but one or more non-blocking limitations remain, such as skipped Playwright automation.

## Partially Ready

Use when the application demonstrates core functionality but a required automated validation remains unresolved.

## Not Ready

Use when:

* Build fails.
* Core workflow fails.
* Required tests fail.
* Confidential data is present.
* Installation is broken.
* A blocking security issue remains.

---

# 47. Recommended Final Classification Based on Current Context

Before commands are rerun, the correct provisional classification is:

| Project                       | Provisional Classification        |
| ----------------------------- | --------------------------------- |
| API Factory                   | Pending final validation          |
| API Migration Analyzer        | Pending final validation          |
| Playwright browser automation | Skipped or pending stabilization  |
| Manual browser workflow       | Pending final Day 50 verification |

Do not pre-fill these as passed.

---

# 48. Release Gate

The following must pass before the project is described as technically complete:

* [ ] Dependency installation.
* [ ] ESLint.
* [ ] Unit tests.
* [ ] TypeScript validation.
* [ ] Production build.
* [ ] Application startup.
* [ ] Main manual browser workflow.
* [ ] Secret and confidential-data review.
* [ ] Accurate README validation instructions.

Playwright may remain a documented limitation if it is explicitly excluded from the required release gate.

---

# 49. Research Validation Separation

The following are not part of Day 50 technical validation:

* Requirement extraction accuracy measurement.
* Gap-detection precision and recall.
* Risk-detection precision and recall.
* Manual versus AI-only versus hybrid comparison.
* Human reviewer scoring.
* Time-reduction measurement.
* Statistical analysis.

These belong to the post-roadmap empirical research evaluation.

A successful build or unit-test run does not prove the research hypotheses.

---

# 50. Known Issues Register Template

| Issue ID | Project            | Description                    | Severity | Status |             Release Blocker | Follow-Up               |
| -------- | ------------------ | ------------------------------ | -------- | ------ | --------------------------: | ----------------------- |
| D50-I01  | Migration Analyzer | Playwright automation unstable | Medium   | Open   | No, when manually validated | Stabilize after roadmap |
| D50-I02  | TBD                | TBD                            | TBD      | TBD    |                         TBD | TBD                     |

Only retain issues that are confirmed during validation.

---

# 51. Phase 2 Completion Checklist

* [x] Defined validation status values.
* [x] Defined validation principles.
* [x] Defined environment recording.
* [x] Defined repository-state validation.
* [x] Defined validation evidence storage.
* [x] Defined API Factory validation order.
* [x] Defined API Factory lint validation.
* [x] Defined API Factory unit-test validation.
* [x] Defined API Factory type checking.
* [x] Defined API Factory build validation.
* [x] Defined API Factory manual browser validation.
* [x] Defined Migration Analyzer validation order.
* [x] Defined Migration Analyzer strict lint validation.
* [x] Defined Migration Analyzer unit-test validation.
* [x] Defined targeted test commands.
* [x] Defined Migration Analyzer type checking.
* [x] Defined Migration Analyzer production build.
* [x] Defined `validate:phase2` validation.
* [x] Defined manual browser scenarios.
* [x] Defined Playwright classifications.
* [x] Defined accessibility checks.
* [x] Defined browser-console checks.
* [x] Defined warning classification.
* [x] Defined failure-resolution order.
* [x] Defined rerun policy.
* [x] Provided combined PowerShell sequences.
* [x] Defined result-record templates.
* [x] Defined readiness classifications.
* [x] Defined the release gate.
* [x] Separated technical validation from research evaluation.

---

# 52. Phase 2 Completion Status

**Status:** Completed as a technical-validation plan and report template

**Actual command execution status:** Pending final execution in both current project repositories

**Recommended file:**

```text
docs/day50/final_validation_report.md
```

---

# 53. Next Phase

**Day 50 Phase 3 – Final README and Project Documentation**

Phase 3 will define the final public documentation for both projects, including:

* Project overview.
* Architecture.
* Features.
* Technology stack.
* Installation.
* Commands.
* Usage workflow.
* Screenshots.
* Testing.
* Known limitations.
* Research relationship.
* Public-release checklist.
