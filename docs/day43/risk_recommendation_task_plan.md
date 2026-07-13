# Day 43 — Migration Risk, Recommendation, and Task Engine

## Phase 1: Risk and Remediation Implementation Plan

## 1. Purpose

This document defines the Day 43 implementation plan for the **AI-Assisted API Migration Analyzer**.

Day 42 completed the deterministic domain foundation. The analyzer can now:

* Validate migration-analysis requests
* Normalize source artifacts
* Build a source inventory
* Detect basic API endpoint evidence
* Resolve a configured target architecture
* Evaluate deterministic migration rules
* Generate structured migration gaps
* Deduplicate gaps
* Calculate preliminary migration scores
* Analyze a realistic Account Discount API sample

Day 43 extends the analyzer from identifying technical gaps to producing actionable remediation guidance.

The primary objective is to convert each relevant migration gap into:

* A structured migration risk
* A clear mitigation
* An engineering recommendation
* A prioritized migration task
* Acceptance criteria
* A deterministic implementation sequence

---

## 2. Current Project Status

The Project 2 codebase is located at:

```text
C:\Users\BH31021\api-factory-generator\api-migration-analyzer
```

The current analysis flow is:

```text
Migration Request
        ↓
Request Validation
        ↓
Artifact Normalization
        ↓
Source Inventory Extraction
        ↓
Endpoint Detection
        ↓
Target Architecture Resolution
        ↓
Migration Rule Evaluation
        ↓
Gap Generation
        ↓
Gap Deduplication
        ↓
Score Calculation
        ↓
Structured API Response
```

The Day 42 API response already contains:

* `ruleResults`
* `gaps`
* `scores`
* `missingInformation`
* `sourceInventory`
* `architectureComparison`

However, the following collections are still empty:

```json
{
  "risks": [],
  "recommendations": [],
  "migrationTasks": []
}
```

Day 43 will populate these collections deterministically.

---

## 3. Day 43 Objective

The Day 43 objective is:

```text
Build the Migration Risk, Recommendation, and Task Engine
```

By the end of Day 43, the analyzer should be able to:

1. Convert migration gaps into structured risks.
2. Assign likelihood, impact, and overall severity.
3. Generate deterministic mitigations.
4. Generate engineering recommendations from rule templates.
5. Deduplicate equivalent recommendations.
6. Convert recommendations into implementation-ready tasks.
7. Assign task priority.
8. Assign implementation complexity.
9. Include acceptance criteria.
10. Link risks, recommendations, and tasks back to gaps.
11. Update summary counts and scores.
12. Return deterministic remediation output for the Account Discount sample.

---

## 4. Day 43 Scope

Day 43 includes:

```text
Risk Generation
Risk Severity Mapping
Risk Deduplication
Recommendation Generation
Recommendation Deduplication
Migration Task Generation
Priority Assignment
Complexity Assignment
Acceptance Criteria Generation
Dependency Ordering
Score Integration
Summary Integration
Sample Validation
Focused Unit Testing
Completion Documentation
```

Day 43 does not include:

* AI-generated recommendations
* User interface development
* Full dependency graph analysis
* Automated source-code refactoring
* GitHub integration
* File upload
* Persistent project storage
* Background processing
* Migration execution tracking
* Pull-request generation
* Full AST parsing

The goal is to complete deterministic remediation generation before expanding the product.

---

## 5. Day 43 Phases

Day 43 is divided into seven phases.

### Phase 1 — Risk and Remediation Implementation Plan

Create:

```text
docs/day43/risk_recommendation_task_plan.md
```

This document defines:

* Current system status
* Day 43 objective
* Risk-generation design
* Recommendation-generation design
* Task-generation design
* Priority and complexity rules
* Deduplication behavior
* Orchestrator integration
* Validation requirements
* Definition of done

---

### Phase 2 — Risk Generation Engine

Create:

```text
src/lib/migration/generateMigrationRisks.ts
```

The engine will convert migration gaps into structured risks.

Each generated risk should include:

* Stable risk ID
* Title
* Description
* Likelihood
* Impact
* Overall severity
* Mitigation
* Related gap IDs
* Affected components
* Evidence
* Deterministic origin

Expected main function:

```typescript
generateMigrationRisks(
  gaps,
  ruleDefinitions,
)
```

The function must remain independent of React and API-route code.

---

### Phase 3 — Risk Deduplication and Severity Helpers

Create:

```text
src/lib/migration/migrationSeverityHelpers.ts
src/lib/migration/deduplicateMigrationRisks.ts
```

The severity helper should provide reusable logic for:

* Severity ranking
* Highest-severity selection
* Priority mapping
* Likelihood mapping
* Impact mapping
* Likelihood-and-impact combination
* Safe score clamping

The risk deduplicator should merge equivalent risks while preserving:

* Highest severity
* Highest likelihood
* Highest impact
* Combined related gaps
* Combined affected components
* Combined evidence

---

### Phase 4 — Recommendation Generation Engine

Create:

```text
src/lib/migration/generateMigrationRecommendations.ts
```

Recommendations should be generated from:

* Matched migration gaps
* Migration rule definitions
* Target architecture
* Related risks

Each recommendation should include:

* Stable recommendation ID
* Title
* Description
* Priority
* Related gap IDs
* Related risk IDs
* Target architecture ID
* Evidence
* Deterministic origin

Expected main function:

```typescript
generateMigrationRecommendations(
  gaps,
  risks,
  rules,
  targetArchitecture,
)
```

Recommendations must use rule templates rather than vague generic wording.

---

### Phase 5 — Migration Task Generation Engine

Create:

```text
src/lib/migration/generateMigrationTasks.ts
```

Tasks should be generated from:

* Recommendations
* Migration gaps
* Rule task templates
* Priority rules
* Complexity rules

Each task should include:

* Stable task ID
* Title
* Description
* Priority
* Complexity
* Related gap IDs
* Related recommendation IDs
* Dependencies
* Acceptance criteria

Expected main function:

```typescript
generateMigrationTasks(
  recommendations,
  gaps,
  rules,
)
```

Tasks should be sorted into a practical implementation order.

---

### Phase 6 — Orchestrator Integration and Sample Validation

Update:

```text
src/lib/migration/migrationAnalysisOrchestrator.ts
```

The analysis flow should become:

```text
Evaluate Rules
    ↓
Generate Gaps
    ↓
Deduplicate Gaps
    ↓
Generate Risks
    ↓
Deduplicate Risks
    ↓
Generate Recommendations
    ↓
Deduplicate Recommendations
    ↓
Generate Migration Tasks
    ↓
Calculate Scores
    ↓
Build Summary
```

The Account Discount sample should return non-empty:

```json
{
  "risks": [],
  "recommendations": [],
  "migrationTasks": []
}
```

after Day 43 implementation.

---

### Phase 7 — Tests and Completion Report

Create focused unit tests for:

```text
migrationSeverityHelpers
generateMigrationRisks
deduplicateMigrationRisks
generateMigrationRecommendations
generateMigrationTasks
```

Create:

```text
docs/day43/day43_completion_report.md
```

The report should document:

* Files created
* Files updated
* Generated risks
* Generated recommendations
* Generated tasks
* Score changes
* Summary changes
* Sample API output
* Deterministic validation
* Lint result
* Build result
* Known limitations
* Day 44 recommendation

---

## 6. Risk Generation Design

A migration gap represents a difference between the source implementation and the selected target architecture.

A migration risk explains the consequence of leaving that gap unresolved.

Example:

```text
Gap:
Repository layer could not be verified.

Risk:
Persistence logic may remain coupled to business services, increasing
datastore migration complexity and transaction regression risk.
```

The risk engine should not simply copy the gap description.

It should answer:

* What can go wrong?
* How likely is it?
* What is the impact?
* Which components are affected?
* How should the risk be mitigated?

---

## 7. Migration Risk Model

The existing `MigrationRisk` model includes:

```typescript
export interface MigrationRisk {
  id: string;
  title: string;
  description: string;
  likelihood: MigrationSeverity;
  impact: MigrationSeverity;
  severity: MigrationSeverity;
  mitigation: string;
  relatedGapIds: string[];
  affectedComponents: string[];
  evidence: AnalysisEvidence[];
  origin: FindingOrigin;
}
```

Day 43 should continue using this model unless a small extension is required.

Potential optional fields may include:

```typescript
category?: MigrationGapCategory;
ruleIds?: string[];
```

These fields should only be added if they improve deduplication or traceability.

---

## 8. Risk Severity Mapping

The initial mapping should be deterministic.

| Gap severity | Likelihood | Impact   | Risk severity |
| ------------ | ---------- | -------- | ------------- |
| CRITICAL     | HIGH       | CRITICAL | CRITICAL      |
| HIGH         | MEDIUM     | HIGH     | HIGH          |
| MEDIUM       | MEDIUM     | MEDIUM   | MEDIUM        |
| LOW          | LOW        | LOW      | LOW           |

The mapping may later become configurable.

For Day 43, it should be centralized in:

```text
migrationSeverityHelpers.ts
```

and not repeated across multiple files.

---

## 9. Likelihood Interpretation

### High Likelihood

Use when the unresolved gap is expected to affect the migration directly.

Examples:

* Missing authentication
* Missing transaction control
* Unsupported runtime dependency
* Hard-coded environment dependency

### Medium Likelihood

Use when the gap is likely to cause implementation or delivery problems but may not fail every migration.

Examples:

* Missing repository abstraction
* Missing API contract
* Missing test evidence
* Missing deployment definition

### Low Likelihood

Use when the issue is unlikely to block migration but still affects quality or maintainability.

Examples:

* Minor documentation gap
* Low-confidence cleanup
* Optional architecture improvement

---

## 10. Impact Interpretation

### Critical Impact

Use when the issue may cause:

* Security exposure
* Financial inconsistency
* Data corruption
* Duplicate transactions
* Production unavailability

### High Impact

Use when the issue may cause:

* Significant rework
* Contract incompatibility
* Incorrect business behavior
* Migration failure for important workflows

### Medium Impact

Use when the issue may cause:

* Operational friction
* Maintainability problems
* Reduced observability
* Moderate delivery delays

### Low Impact

Use when the issue is non-blocking and limited in scope.

---

## 11. Initial Risk Templates

The first implementation should support meaningful risk wording for the initial Day 42 rules.

### SRC-002

Risk title:

```text
Incomplete endpoint inventory may omit production workflows
```

Risk description:

```text
The migration scope may exclude active API routes because endpoint evidence
could not be verified from the supplied artifacts.
```

Mitigation:

```text
Collect route registrations, handlers, OpenAPI definitions, and API
documentation before finalizing migration scope.
```

---

### API-003

Risk title:

```text
Business logic may remain coupled to transport handling
```

Risk description:

```text
Without a verified service layer, business workflows may remain embedded in
handlers, reducing testability and increasing regression risk.
```

Mitigation:

```text
Introduce independently testable service interfaces and move business
workflow logic out of HTTP handlers.
```

---

### API-004

Risk title:

```text
Persistence coupling may increase migration and transaction risk
```

Risk description:

```text
Without a verified repository layer, datastore-specific logic may remain
coupled to services or handlers, making migration and transaction validation
more difficult.
```

Mitigation:

```text
Introduce repository interfaces and isolate SQL or datastore-specific
operations inside transaction-aware repository implementations.
```

---

### CONTRACT-001

Risk title:

```text
Client-visible behavior may change without a contract baseline
```

Risk description:

```text
Without a verified API contract, the migrated implementation may
unintentionally change requests, responses, validation rules, or errors.
```

Mitigation:

```text
Create a version-controlled OpenAPI or equivalent contract before changing
legacy API behavior.
```

---

### OBS-001

Risk title:

```text
Production failures may be difficult to trace
```

Risk description:

```text
Without verified correlation identifier propagation, requests may not be
traceable across handlers, services, repositories, and downstream systems.
```

Mitigation:

```text
Add correlation identifier extraction, generation, context propagation,
structured logging, and downstream forwarding.
```

---

### TEST-001

Risk title:

```text
Migration regressions may remain undetected
```

Risk description:

```text
Without automated test evidence, changes to critical business behavior may
not be detected before deployment.
```

Mitigation:

```text
Create regression, integration, and compatibility tests for critical success
and failure paths.
```

---

### DEPLOY-001

Risk title:

```text
Runtime and deployment requirements may be discovered late
```

Risk description:

```text
Without a verified deployment definition, container, runtime, configuration,
health-check, and infrastructure requirements may delay release readiness.
```

Mitigation:

```text
Create a deployment definition covering the container, runtime configuration,
health checks, and environment requirements.
```

---

## 12. Stable Risk Identifiers

Risk IDs must be deterministic.

Recommended format:

```text
risk-<rule-id>-<affected-component>
```

Example:

```text
risk-api-004-repository-layer
```

The same normalized input must produce the same risk IDs.

The risk ID must not use:

* Random UUIDs
* Timestamps
* Array position
* Current analysis ID

---

## 13. Risk Deduplication Rules

Risks should be treated as duplicates when they share:

* The same technical cause
* The same related rule or gap
* The same affected component
* The same category

When duplicates are merged:

1. Keep the highest likelihood.
2. Keep the highest impact.
3. Keep the highest overall severity.
4. Merge related gap IDs.
5. Merge affected components.
6. Merge evidence.
7. Keep deterministic ordering.

---

## 14. Recommendation Generation Design

A recommendation defines the technical action required to address a migration gap and reduce its risk.

Recommendations should be:

* Specific
* Actionable
* Target-architecture aligned
* Evidence-oriented
* Linked to gaps and risks
* Independent of AI wording

Example:

```text
Introduce an account repository interface and move account-header and
transaction-detail SQL operations from the discount service into a
transaction-aware repository implementation.
```

Avoid:

```text
Improve the database architecture.
```

---

## 15. Recommendation Sources

Recommendations should be generated from:

* `MigrationGap`
* `MigrationRisk`
* `MigrationRuleDefinition.recommendationTemplate`
* Selected target architecture

The rule template should remain the primary source.

The generator may add context such as:

* Affected component
* Target architecture name
* Related endpoint
* Risk title

It must not invent unsupported implementation details.

---

## 16. Recommendation Priority Mapping

The initial priority mapping should be:

| Gap severity | Recommendation priority |
| ------------ | ----------------------- |
| CRITICAL     | P0                      |
| HIGH         | P1                      |
| MEDIUM       | P2                      |
| LOW          | P3                      |

This mapping should be implemented once in:

```text
migrationSeverityHelpers.ts
```

and reused by recommendation and task generation.

---

## 17. Stable Recommendation Identifiers

Recommended format:

```text
recommendation-<rule-id>-<affected-component>
```

Example:

```text
recommendation-api-004-repository-layer
```

Recommendation IDs must remain deterministic for the same normalized input.

---

## 18. Recommendation Deduplication

Equivalent recommendations should be merged.

Duplicate detection should consider:

* Rule ID
* Recommendation title
* Affected component
* Related gap IDs
* Target architecture

Merged recommendations should retain:

* Highest priority
* Combined gap IDs
* Combined risk IDs
* Combined evidence
* Deterministic ordering

Recommendation deduplication may be implemented:

* Inside `generateMigrationRecommendations.ts`, or
* In a separate `deduplicateMigrationRecommendations.ts`

A separate helper is preferred if the generator becomes large.

---

## 19. Migration Task Generation Design

A migration task converts a recommendation into an implementation-ready work item.

A task should answer:

* What must be changed?
* Where should it be changed?
* Why is it required?
* What priority does it have?
* How complex is it?
* What must be true for the task to be complete?
* Which tasks must happen first?

---

## 20. Migration Task Model

The existing model includes:

```typescript
export interface MigrationTask {
  id: string;
  title: string;
  description: string;
  priority: MigrationPriority;
  complexity: MigrationComplexity;
  relatedGapIds: string[];
  relatedRecommendationIds: string[];
  dependencies: string[];
  acceptanceCriteria: string[];
}
```

This model is sufficient for the Day 43 MVP.

Optional future fields may include:

```typescript
affectedComponents?: string[];
sequence?: number;
```

These should only be added if required for sorting or UI presentation.

---

## 21. Task Generation Rules

A task should be generated when:

* A matched rule has a task template.
* A recommendation addresses a critical or high gap.
* A medium-severity gap has an explicit task template.
* The task is not a duplicate of an existing generated task.

Low-severity findings may remain recommendations unless their rule explicitly defines a task template.

---

## 22. Task Priority Mapping

Task priority should follow the related gap severity:

| Severity | Priority |
| -------- | -------- |
| CRITICAL | P0       |
| HIGH     | P1       |
| MEDIUM   | P2       |
| LOW      | P3       |

When a task relates to multiple gaps, use the highest priority.

---

## 23. Task Complexity Mapping

Rule task templates already include a default complexity.

The generator should use the template complexity first.

When no template complexity exists, infer complexity using:

### Small

* One isolated file
* One local configuration change
* No public contract impact

### Medium

* One component or endpoint
* Several files
* New focused tests required

### Large

* New architecture layer
* Multiple components
* Transaction or persistence changes
* Broad test updates

### Extra Large

* Cross-system change
* Datastore migration
* Public contract redesign
* Security model replacement
* Multiple teams required

---

## 24. Acceptance Criteria Rules

Acceptance criteria should come from:

```text
MigrationRuleDefinition.taskTemplate.acceptanceCriteria
```

The generator should:

* Preserve the template criteria.
* Remove exact duplicates.
* Keep deterministic ordering.
* Require at least one criterion.
* Avoid generating vague criteria.

Good:

```text
Services depend on repository interfaces rather than database clients.
```

Avoid:

```text
The code is improved.
```

---

## 25. Stable Task Identifiers

Recommended format:

```text
task-<rule-id>-<affected-component>
```

Example:

```text
task-api-004-repository-layer
```

The same input must produce the same task IDs.

---

## 26. Task Dependency Ordering

Initial task ordering should follow:

```text
Discovery and Contract
        ↓
Security and Data Integrity
        ↓
Architecture Refactoring
        ↓
Dependency Integration
        ↓
Testing
        ↓
Observability
        ↓
Deployment
        ↓
Documentation
```

For the current sample:

```text
Introduce repository abstractions
        ↓
Create the target deployment definition
```

These tasks may be independent unless the implementation explicitly requires one before the other.

Task dependencies should only be added when a real ordering requirement exists.

---

## 27. Orchestrator Integration

The orchestrator currently creates:

* Rule results
* Gaps
* Scores
* Summary

Day 43 should update it to create:

* Risks
* Recommendations
* Tasks

Recommended processing sequence:

```typescript
const enabledRules = getEnabledMigrationRules();

const ruleResults = evaluateMigrationRules(
  analysisContext,
  enabledRules,
);

const generatedGaps = generateMigrationGaps(
  ruleResults,
  enabledRules,
);

const gaps = deduplicateMigrationGaps(
  generatedGaps,
);

const generatedRisks = generateMigrationRisks(
  gaps,
  enabledRules,
);

const risks = deduplicateMigrationRisks(
  generatedRisks,
);

const recommendations =
  generateMigrationRecommendations(
    gaps,
    risks,
    enabledRules,
    targetArchitecture,
  );

const migrationTasks = generateMigrationTasks(
  recommendations,
  gaps,
  enabledRules,
);
```

The exact function signatures may vary slightly, but the responsibilities must remain separated.

---

## 28. Summary Integration

The existing summary fields already include:

```text
gapCount
riskCount
recommendationCount
migrationTaskCount
criticalFindingCount
highFindingCount
```

Day 43 should ensure these counts use the generated collections.

Example expected summary:

```json
{
  "gapCount": 2,
  "riskCount": 2,
  "recommendationCount": 2,
  "migrationTaskCount": 2
}
```

The exact counts depend on rule matches and deduplication.

---

## 29. Score Integration

The risk score should be updated to use generated risks rather than gaps alone.

Suggested weights:

| Risk severity | Weight |
| ------------- | -----: |
| CRITICAL      |     15 |
| HIGH          |      8 |
| MEDIUM        |      4 |
| LOW           |      1 |

The score should also consider:

* Missing evidence
* Critical unknowns
* Number of affected components

The final score must remain between:

```text
0–100
```

Readiness and complexity scores may continue using migration gaps.

---

## 30. Expected Account Discount Sample Output

The Day 42 sample is expected to identify:

```text
API-004
DEPLOY-001
```

Day 43 should convert those gaps into approximately:

```text
2 risks
2 recommendations
2 tasks
```

Expected risk examples:

```text
Persistence coupling may increase migration and transaction risk
Runtime and deployment requirements may be discovered late
```

Expected recommendations:

```text
Introduce repository abstractions
Define the target deployment model
```

Expected tasks:

```text
Introduce repository abstractions
Create the target deployment definition
```

---

## 31. Example Repository Risk

```json
{
  "id": "risk-api-004-repository-layer",
  "title": "Persistence coupling may increase migration and transaction risk",
  "description": "Without a verified repository layer, datastore-specific logic may remain coupled to services or handlers.",
  "likelihood": "MEDIUM",
  "impact": "HIGH",
  "severity": "HIGH",
  "mitigation": "Introduce repository interfaces and isolate datastore-specific operations.",
  "relatedGapIds": [
    "gap-api-004-repository-layer"
  ],
  "affectedComponents": [
    "REPOSITORY_LAYER"
  ],
  "origin": "DETERMINISTIC"
}
```

---

## 32. Example Repository Recommendation

```json
{
  "id": "recommendation-api-004-repository-layer",
  "title": "Introduce repository abstractions",
  "description": "Introduce repository interfaces and move database access into transaction-aware repository implementations.",
  "relatedGapIds": [
    "gap-api-004-repository-layer"
  ],
  "relatedRiskIds": [
    "risk-api-004-repository-layer"
  ],
  "targetArchitectureId": "go-cloud-native-api",
  "priority": "P1",
  "origin": "DETERMINISTIC"
}
```

---

## 33. Example Repository Task

```json
{
  "id": "task-api-004-repository-layer",
  "title": "Introduce repository abstractions",
  "description": "Create repository interfaces and implementations for all database operations used by the migrated API.",
  "priority": "P1",
  "complexity": "LARGE",
  "relatedGapIds": [
    "gap-api-004-repository-layer"
  ],
  "relatedRecommendationIds": [
    "recommendation-api-004-repository-layer"
  ],
  "dependencies": [],
  "acceptanceCriteria": [
    "Handlers contain no database operations.",
    "Services depend on repository interfaces.",
    "Repository implementations own datastore-specific behavior.",
    "Repository methods accept request context.",
    "Repository success and failure paths are tested."
  ]
}
```

---

## 34. Deterministic Output Requirements

The same normalized input must produce the same:

* Risk IDs
* Risk titles
* Risk severities
* Risk likelihood
* Risk impact
* Recommendation IDs
* Recommendation priorities
* Task IDs
* Task priorities
* Task complexities
* Acceptance criteria
* Related gap and risk links
* Summary counts
* Scores

The following values may differ:

```text
analysisId
metadata.createdAt
metadata.completedAt
```

---

## 35. Validation Commands

Run commands from:

```powershell
cd C:\Users\BH31021\api-factory-generator\api-migration-analyzer
```

Run:

```powershell
npm run lint
npm run build
```

Start the application:

```powershell
npm run dev
```

Do not run these scripts from the parent folder.

---

## 36. Sample Analysis Validation

Run:

```powershell
$response = Invoke-RestMethod `
    -Uri "http://localhost:3000/api/migration-samples/account-discount/analyze" `
    -Method POST
```

Inspect risks:

```powershell
$response.risks |
    Select-Object `
        id,
        title,
        likelihood,
        impact,
        severity |
    Format-Table -AutoSize
```

Inspect recommendations:

```powershell
$response.recommendations |
    Select-Object `
        id,
        title,
        priority,
        targetArchitectureId |
    Format-Table -AutoSize
```

Inspect tasks:

```powershell
$response.migrationTasks |
    Select-Object `
        id,
        title,
        priority,
        complexity |
    Format-Table -AutoSize
```

Inspect summary:

```powershell
$response.summary |
    ConvertTo-Json -Depth 10
```

---

## 37. Deterministic Validation

Run the sample twice:

```powershell
$response1 = Invoke-RestMethod `
    -Uri "http://localhost:3000/api/migration-samples/account-discount/analyze" `
    -Method POST

$response2 = Invoke-RestMethod `
    -Uri "http://localhost:3000/api/migration-samples/account-discount/analyze" `
    -Method POST
```

Compare risk IDs:

```powershell
Compare-Object `
    $response1.risks.id `
    $response2.risks.id
```

Compare recommendation IDs:

```powershell
Compare-Object `
    $response1.recommendations.id `
    $response2.recommendations.id
```

Compare task IDs:

```powershell
Compare-Object `
    $response1.migrationTasks.id `
    $response2.migrationTasks.id
```

Expected result:

```text
No differences
```

---

## 38. Unit Test Plan

Recommended test files:

```text
src/lib/migration/migrationSeverityHelpers.test.ts
src/lib/migration/generateMigrationRisks.test.ts
src/lib/migration/deduplicateMigrationRisks.test.ts
src/lib/migration/generateMigrationRecommendations.test.ts
src/lib/migration/generateMigrationTasks.test.ts
```

Each test suite should cover:

* Empty input
* One matched gap
* Multiple gaps
* Duplicate findings
* Severity mapping
* Priority mapping
* Complexity mapping
* Stable IDs
* Evidence merging
* Acceptance criteria
* Deterministic ordering

---

## 39. Known Limitations

At the end of Day 43:

* Risks are template-based.
* Recommendations are deterministic rather than AI-enhanced.
* Task complexity is rule-based.
* Dependencies between tasks are limited.
* Evidence may remain file-level.
* Only the initial seven rules generate remediation.
* No browser interface exists.
* No user editing or approval workflow exists.
* No persistent task tracking exists.

These limitations are acceptable for the deterministic MVP.

---

## 40. Definition of Done

Day 43 is complete when:

* A reusable severity helper exists.
* Migration gaps generate structured risks.
* Risk likelihood and impact are assigned.
* Risk severity is deterministic.
* Duplicate risks are merged.
* Risks include mitigation text.
* Migration gaps generate recommendations.
* Recommendations use rule templates.
* Recommendations include priority.
* Recommendations link to gaps and risks.
* Recommendations are deduplicated.
* Recommendations generate migration tasks.
* Tasks include priority and complexity.
* Tasks include acceptance criteria.
* Task IDs remain deterministic.
* Summary counts use generated output.
* Risk scores use generated risks.
* The Account Discount sample returns non-empty risks.
* The sample returns non-empty recommendations.
* The sample returns non-empty tasks.
* Deterministic comparison passes.
* Unit tests pass.
* Lint passes.
* Production build passes.
* Completion documentation exists.

---

## 41. Day 43 Completion Checklist

* [ ] `docs/day43/risk_recommendation_task_plan.md` created
* [ ] `migrationSeverityHelpers.ts` created
* [ ] `generateMigrationRisks.ts` created
* [ ] `deduplicateMigrationRisks.ts` created
* [ ] Risk templates implemented
* [ ] Risk IDs deterministic
* [ ] Risk likelihood assigned
* [ ] Risk impact assigned
* [ ] Risk severity assigned
* [ ] Risk mitigation generated
* [ ] Recommendation generator created
* [ ] Recommendation IDs deterministic
* [ ] Recommendation priorities assigned
* [ ] Recommendation deduplication implemented
* [ ] Task generator created
* [ ] Task IDs deterministic
* [ ] Task priorities assigned
* [ ] Task complexities assigned
* [ ] Acceptance criteria generated
* [ ] Orchestrator updated
* [ ] Summary updated
* [ ] Risk scoring updated
* [ ] Sample risks generated
* [ ] Sample recommendations generated
* [ ] Sample tasks generated
* [ ] Unit tests added
* [ ] Lint passes
* [ ] Production build passes
* [ ] Completion report created

---

## 42. Day 43 Outcome

At the end of Day 43, the analyzer should support:

```text
Deterministic Migration Rules
        ↓
Structured Migration Gaps
        ↓
Migration Risks
        ↓
Risk Mitigations
        ↓
Engineering Recommendations
        ↓
Prioritized Migration Tasks
        ↓
Acceptance Criteria
        ↓
Updated Scores and Summary
```

The expected milestone is:

```text
The AI-Assisted API Migration Analyzer can explain why migration gaps matter
and convert them into deterministic, implementation-ready remediation work.
```

This creates the foundation required for Day 44 to focus on richer evidence, analysis quality, testing, or the first browser-based workflow.
