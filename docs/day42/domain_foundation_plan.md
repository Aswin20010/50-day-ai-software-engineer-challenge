# Day 42 — Migration Analyzer Domain Foundation

## Phase 1: Foundation Implementation Plan

## 1. Purpose

This document defines the Day 42 implementation plan for the **AI-Assisted API Migration Analyzer**.

Day 41 established the product requirements, technical architecture, migration domain model, deterministic rule strategy, and implementation roadmap.

Day 42 begins the first production implementation of Project 2 by converting the Day 41 architecture into a reusable and deterministic migration-analysis foundation.

The primary goal is to create a working vertical slice that can:

* Represent a migration-analysis request
* Validate source inputs
* Normalize source artifacts
* Resolve a target architecture
* Evaluate deterministic migration rules
* Generate structured migration gaps
* Analyze a realistic sample legacy API
* Return repeatable results through the existing API route

---

## 2. Current Project Status

The new project is located in:

```text
C:\Users\BH31021\api-factory-generator\api-migration-analyzer
```

Project 2 is maintained separately from Project 1:

```text
api-factory-generator/
├── api-factory-ui/
└── api-migration-analyzer/
```

The Day 41 Phase 3 architecture skeleton is already working.

The existing application currently includes:

```text
src/
├── app/
│   └── api/
│       └── migration-analysis/
│           └── route.ts
│
├── lib/
│   └── migration/
│       ├── migrationAnalysisOrchestrator.ts
│       └── validateMigrationAnalysisRequest.ts
│
├── services/
│   └── migrationAnalysisService.ts
│
└── types/
    └── migration.ts
```

The migration-analysis endpoint currently supports:

* JSON request parsing
* Request validation
* Source-artifact normalization
* Target architecture resolution
* Preliminary source inventory extraction
* Architecture capability comparison
* Missing-information findings
* Preliminary migration scoring
* Structured success and error responses

The current endpoint returns:

```text
POST /api/migration-analysis
```

The existing response already includes:

* Analysis identifier
* Analysis status
* Source system
* Source inventory
* Architecture comparison
* Missing information
* Preliminary scores
* Analysis metadata

However, before Day 42, these collections remain empty:

```json
{
  "ruleResults": [],
  "gaps": [],
  "risks": [],
  "recommendations": [],
  "migrationTasks": []
}
```

Day 42 will begin filling the deterministic rule and gap portions of the result.

---

## 3. Day 42 Objective

The Day 42 objective is:

```text
Build the Migration Analyzer Domain Foundation
```

By the end of Day 42, the system should be able to:

1. Resolve target architectures from reusable configuration.
2. Load a deterministic migration-rule catalog.
3. Evaluate migration rules against normalized analysis data.
4. Return rule statuses such as `MATCHED` and `NOT_MATCHED`.
5. Convert matched rules into structured migration gaps.
6. Preserve deterministic gap identifiers.
7. Analyze a realistic legacy Account Discount API sample.
8. Detect endpoint evidence from supplied artifacts.
9. Return non-empty migration gaps.
10. Pass lint and production build validation.

---

## 4. Day 42 Scope

Day 42 includes the following implementation areas:

```text
Target Architecture Configuration
Migration Rule Configuration
Rule Evaluation
Gap Generation
Gap Deduplication
Source Inventory Improvements
Sample Migration Project
Endpoint Evidence Detection
Score Integration
Validation
Completion Reporting
```

Day 42 does not yet include:

* Migration risk generation
* Recommendation generation
* Migration task generation
* Full dependency analysis
* Full SQL parsing
* Full business-rule extraction
* AI provider integration
* Browser user interface
* Repository upload
* GitHub integration
* Persistent storage

Those capabilities will be implemented during later roadmap days.

---

## 5. Day 42 Phases

Day 42 is divided into seven phases.

### Phase 1 — Foundation Implementation Plan

Create:

```text
docs/day42/domain_foundation_plan.md
```

This document defines:

* Current project status
* Day 42 goals
* Implementation sequence
* Required files
* Scope controls
* Validation requirements
* Completion criteria

---

### Phase 2 — Target Architecture Configuration

Create:

```text
src/data/targetArchitectures.ts
```

Move target architecture definitions out of the orchestrator.

Implement reusable functions for:

* Listing target architectures
* Finding an architecture by ID
* Validating architecture IDs
* Requiring a supported architecture

Update:

```text
src/lib/migration/migrationAnalysisOrchestrator.ts
src/lib/migration/validateMigrationAnalysisRequest.ts
src/services/migrationAnalysisService.ts
src/app/api/migration-analysis/route.ts
```

Expected result:

```text
The Go Cloud-Native Layered API definition is configuration-driven and no longer hard-coded in the orchestrator.
```

---

### Phase 3 — Migration Rule Foundation

Create:

```text
src/data/migrationRules.ts
```

Define the first deterministic migration-rule catalog.

Initial rules:

```text
SRC-002
API-003
API-004
CONTRACT-001
OBS-001
TEST-001
DEPLOY-001
```

The rules will detect:

* Missing endpoint evidence
* Unverified service layer
* Unverified repository layer
* Missing API contract
* Missing correlation identifier evidence
* Missing automated tests
* Missing deployment configuration

Add reusable helper functions for:

* Listing all rules
* Listing enabled rules
* Finding a rule by ID
* Filtering rules by category
* Filtering rules by scope

Expected result:

```text
The project contains a reusable, typed, configuration-driven migration-rule catalog.
```

---

### Phase 4 — Rule Evaluation Engine

Create:

```text
src/lib/migration/evaluateMigrationRules.ts
```

The evaluator will support:

```text
MATCHED
NOT_MATCHED
NOT_APPLICABLE
INSUFFICIENT_EVIDENCE
```

Supported operators include:

```text
EXISTS
NOT_EXISTS
EQUALS
NOT_EQUALS
CONTAINS
NOT_CONTAINS
EMPTY
NOT_EMPTY
GREATER_THAN
LESS_THAN
MATCHES_PATTERN
```

The evaluator must:

* Resolve fields from the migration-analysis context
* Evaluate configured conditions
* Return deterministic statuses
* Preserve rule IDs
* Assign confidence
* Capture affected components
* Avoid producing UI-specific output
* Return insufficient evidence when data cannot be resolved

Expected result:

```text
Each configured rule returns a deterministic MigrationRuleResult.
```

---

### Phase 5 — Gap Generation

Create:

```text
src/lib/migration/generateMigrationGaps.ts
src/lib/migration/deduplicateMigrationGaps.ts
```

Convert matched rule results into structured gaps.

Each gap should include:

* Gap ID
* Rule ID
* Category
* Title
* Description
* Severity
* Confidence
* Target expectation
* Migration impact
* Affected components
* Affected endpoints
* Evidence
* Origin

Gap IDs must remain stable for the same deterministic input.

Expected result:

```text
Matched rule results produce non-empty structured migration gaps.
```

---

### Phase 6 — Sample Migration Project

Create:

```text
src/data/sampleMigrationProjects.ts
```

Create a realistic sample:

```text
Legacy Account Discount API Migration
```

The sample should include:

* Go HTTP handler
* Route registration
* Service interface
* Service implementation
* External PIID lookup client
* Request and response models
* PostgreSQL schema
* API documentation
* Configuration example
* Unit test artifact
* Repository structure description

The sample should intentionally contain:

* A service layer
* API documentation
* Correlation identifier behavior
* Automated tests
* No repository abstraction
* No deployment definition

Create optional development routes:

```text
GET  /api/migration-samples
POST /api/migration-samples/account-discount/analyze
```

Expected result:

```text
The sample can be loaded and analyzed without manually constructing a request.
```

---

### Phase 7 — Validation and Completion Report

Create:

```text
docs/day42/day42_completion_report.md
```

The final phase must document:

* Files created
* Files updated
* Rule results
* Generated gaps
* Sample analysis behavior
* Lint result
* Build result
* API validation
* Known limitations
* Day 43 recommendation

---

## 6. Target Architecture Implementation

The initial target architecture is:

```text
Go Cloud-Native Layered API
```

Its required capabilities include:

* Handler layer
* Service layer
* Repository layer
* External client layer
* Request validation
* Structured logging
* Correlation identifier propagation
* Health checks
* Configuration validation
* Automated tests
* Centralized error mapping
* Transaction-aware persistence

The target architecture must be defined in:

```text
src/data/targetArchitectures.ts
```

The orchestrator must retrieve it through a reusable resolver rather than a local constant.

---

## 7. Initial Migration Rules

The first rule set is intentionally small.

### SRC-002 — No Endpoint Evidence

Matches when:

```text
sourceInventory.endpoints is empty
```

Purpose:

* Prevent incomplete API inventory
* Identify insufficient route evidence
* Reduce the risk of missing production workflows

---

### API-003 — Service Layer Could Not Be Verified

Matches when:

```text
SERVICE_LAYER sourceStatus equals UNKNOWN
```

Purpose:

* Identify missing separation between transport and business logic
* Encourage testable service components

---

### API-004 — Repository Layer Could Not Be Verified

Matches when:

```text
REPOSITORY_LAYER sourceStatus equals UNKNOWN
```

Purpose:

* Identify missing persistence abstraction
* Support future datastore migration
* Improve transaction testability

---

### CONTRACT-001 — API Contract Could Not Be Verified

Matches when no artifact of type:

```text
OPENAPI
API_DOCUMENTATION
```

is available.

Purpose:

* Protect compatibility
* Establish a contract baseline
* Prevent undocumented behavior changes

---

### OBS-001 — Correlation Identifier Could Not Be Verified

Matches when:

```text
CORRELATION_ID
```

is not detected in the source inventory.

Purpose:

* Improve request traceability
* Support production diagnostics
* Support downstream correlation

---

### TEST-001 — Automated Tests Could Not Be Verified

Matches when:

```text
sourceInventory.testArtifacts is empty
```

Purpose:

* Identify missing regression protection
* Encourage compatibility testing
* Reduce migration regression risk

---

### DEPLOY-001 — Deployment Definition Could Not Be Verified

Matches when:

```text
sourceInventory.deploymentArtifacts is empty
```

Purpose:

* Identify missing runtime and deployment requirements
* Prevent late deployment discovery
* Establish container and health-check expectations

---

## 8. Source Inventory Enhancements

Day 42 will improve source inventory extraction.

New inventory capabilities should include:

* Observability mechanism detection
* Correlation identifier detection
* Structured logging detection
* Tracing detection
* Metrics detection
* Endpoint detection from documentation
* Endpoint detection from Go route registration
* Endpoint detection from Express route syntax

The source inventory model should include:

```typescript
observabilityMechanisms: string[];
```

Possible values include:

```text
CORRELATION_ID
STRUCTURED_LOGGING
DISTRIBUTED_TRACING
METRICS
```

---

## 9. Endpoint Detection Scope

The initial endpoint extractor should remain conservative.

It may detect:

### Documentation Routes

Example:

```text
POST /api/v1/discount/interrogateAccount
```

### Go `HandleFunc` Routes

Example:

```go
mux.HandleFunc(
    "/api/v1/discount/interrogateAccount",
    handler.InterrogateAccount,
)
```

### Express Routes

Example:

```typescript
router.post("/api/v1/accounts", handler);
```

The extractor does not need to:

* Fully understand middleware
* Resolve dynamic route composition
* Parse all framework conventions
* Build a full AST
* Infer undocumented methods
* Execute source code

Unknown values must remain undefined.

---

## 10. Deterministic Analysis Requirements

The same normalized input must return the same:

* Source inventory
* Endpoint IDs
* Rule statuses
* Rule confidence
* Gap IDs
* Gap categories
* Gap severities
* Gap titles
* Affected components
* Scores

The following values may differ between runs:

```text
analysisId
metadata.createdAt
metadata.completedAt
```

AI assistance must remain disabled for Day 42 validation.

---

## 11. Required Files

The expected Day 42 structure is:

```text
api-migration-analyzer/
├── docs/
│   └── day42/
│       ├── domain_foundation_plan.md
│       └── day42_completion_report.md
│
└── src/
    ├── app/
    │   └── api/
    │       ├── migration-analysis/
    │       │   └── route.ts
    │       │
    │       ├── migration-rules/
    │       │   └── route.ts
    │       │
    │       └── migration-samples/
    │           ├── route.ts
    │           └── account-discount/
    │               └── analyze/
    │                   └── route.ts
    │
    ├── data/
    │   ├── migrationRules.ts
    │   ├── sampleMigrationProjects.ts
    │   └── targetArchitectures.ts
    │
    ├── lib/
    │   └── migration/
    │       ├── deduplicateMigrationGaps.ts
    │       ├── evaluateMigrationRules.ts
    │       ├── generateMigrationGaps.ts
    │       ├── migrationAnalysisOrchestrator.ts
    │       └── validateMigrationAnalysisRequest.ts
    │
    ├── services/
    │   └── migrationAnalysisService.ts
    │
    └── types/
        └── migration.ts
```

The `/api/migration-rules` route is optional and may be retained only for development verification.

---

## 12. API Response Expectations

Before Day 42:

```json
{
  "ruleResults": [],
  "gaps": []
}
```

After Day 42:

```json
{
  "ruleResults": [
    {
      "ruleId": "API-004",
      "status": "MATCHED",
      "confidence": 0.8
    }
  ],
  "gaps": [
    {
      "ruleId": "API-004",
      "category": "ARCHITECTURE",
      "severity": "HIGH",
      "origin": "DETERMINISTIC"
    }
  ]
}
```

The exact result depends on the supplied artifacts.

---

## 13. Sample Project Expected Results

The Day 42 Account Discount sample should contain enough evidence to satisfy:

```text
API-003
CONTRACT-001
OBS-001
TEST-001
SRC-002
```

It should intentionally trigger:

```text
API-004
DEPLOY-001
```

Expected approximate rule results:

```text
SRC-002       NOT_MATCHED
API-003       NOT_MATCHED
API-004       MATCHED
CONTRACT-001  NOT_MATCHED
OBS-001       NOT_MATCHED
TEST-001      NOT_MATCHED
DEPLOY-001    MATCHED
```

Expected gap count:

```text
2
```

Expected gaps:

```text
API-004
DEPLOY-001
```

---

## 14. Score Integration

Day 42 scoring should use generated gaps.

### Readiness

The readiness score should decrease based on:

* Critical gaps
* High gaps
* Medium gaps
* Low gaps
* Missing evidence

### Complexity

The complexity score should consider:

* Artifact count
* Critical gaps
* High gaps
* Medium gaps

### Risk

The risk score should consider:

* Gap severity
* Missing evidence

All scores must remain within:

```text
0–100
```

The score output must include explanations.

---

## 15. Error Handling Requirements

Day 42 must preserve current error behavior.

The API must continue handling:

* Invalid JSON
* Missing project name
* Missing source application name
* Missing artifacts
* Empty artifact content
* Unsupported artifact type
* Unsupported target architecture
* Oversized input
* Unexpected analysis failures

Error responses must remain structured.

Example:

```json
{
  "status": "FAILED",
  "errors": [
    {
      "code": "UNSUPPORTED_TARGET_ARCHITECTURE",
      "message": "The selected target architecture is not supported.",
      "field": "targetArchitectureId",
      "recoverable": true
    }
  ]
}
```

Internal stack traces must not be returned.

---

## 16. Security Requirements

The sample and analyzer must avoid exposing real credentials.

The sample configuration may contain example values only.

The analyzer should not:

* Log full source artifacts
* Log sensitive request bodies
* Reproduce credentials in findings
* Persist submitted artifacts
* Send content to an AI provider
* Execute supplied code

Day 42 analysis remains local and deterministic.

---

## 17. Validation Commands

Run all commands inside:

```powershell
cd C:\Users\BH31021\api-factory-generator\api-migration-analyzer
```

Run lint:

```powershell
npm run lint
```

Run production build:

```powershell
npm run build
```

Start the development server:

```powershell
npm run dev
```

Do not run project scripts from:

```text
C:\Users\BH31021\api-factory-generator
```

The parent directory does not contain the Project 2 `package.json` scripts.

---

## 18. API Validation

### Validate Available Rules

Optional route:

```text
GET /api/migration-rules
```

Expected rule count:

```text
7
```

Expected IDs:

```text
SRC-002
API-003
API-004
CONTRACT-001
OBS-001
TEST-001
DEPLOY-001
```

---

### Validate Available Samples

Route:

```text
GET /api/migration-samples
```

Expected sample count:

```text
1
```

Expected project:

```text
Legacy Account Discount API Migration
```

---

### Run Sample Analysis

Route:

```text
POST /api/migration-samples/account-discount/analyze
```

Expected:

* Analysis status is `COMPLETED`
* Endpoint inventory is non-empty
* Rule results are present
* Gap count is nonzero
* Gaps contain deterministic origin
* Scores remain between `0` and `100`

---

## 19. Deterministic Validation

Run the same sample analysis twice.

Compare:

```text
ruleResults
gap IDs
gap severities
gap confidence
scores
```

Expected:

```text
No differences
```

Ignore differences in:

```text
analysisId
createdAt
completedAt
```

---

## 20. Testing Expectations

Day 42 validation is primarily based on:

* TypeScript compilation
* ESLint
* Next.js production build
* API route testing
* Deterministic result comparison
* Sample project analysis

Unit tests may be added in a later day when the rule evaluator and analyzers stabilize.

However, all new functions should be written as pure or near-pure functions where practical so they can be unit tested later.

---

## 21. MVP Scope Controls

The following features must not be added during Day 42:

* AI-generated findings
* Risk-generation engine
* Recommendation-generation engine
* Migration-task generation
* Full repository parser
* AST parser
* File uploads
* GitHub integration
* Persistent database
* Authentication
* Background processing
* Automatic source-code rewriting
* Pull-request generation
* Complex UI development

The goal is to complete the deterministic domain foundation before expanding scope.

---

## 22. Known Limitations

At the end of Day 42, the analyzer will still have limitations.

### Endpoint Detection

The extractor uses regular expressions rather than a language parser.

### Service and Repository Detection

Detection relies on naming patterns such as:

```text
Service
Repository
DAO
```

### Evidence

Some initial findings may contain file-level evidence rather than exact line references.

### Risk and Recommendations

These outputs remain empty until later implementation phases.

### AI Assistance

AI assistance remains disabled.

### Source Size

The MVP is intended for small and medium manually supplied artifact collections.

These limitations are acceptable for the Day 42 vertical slice.

---

## 23. Definition of Done

Day 42 is complete when:

* Target architecture configuration exists.
* Target architecture resolution is reusable.
* Seven deterministic rules are configured.
* Rule-related TypeScript types exist.
* Source inventory includes observability mechanisms.
* Rule evaluation returns deterministic statuses.
* Rule results are returned by the API.
* Matched rules generate structured migration gaps.
* Duplicate gaps are merged.
* Gap IDs remain stable.
* Scores use generated gaps.
* The Account Discount sample exists.
* The sample can be listed through an API route.
* The sample can be analyzed through an API route.
* Endpoint evidence is detected.
* The sample produces non-empty gaps.
* Lint passes.
* Production build passes.
* The completion report is created.

---

## 24. Day 42 Completion Checklist

* [ ] `docs/day42/domain_foundation_plan.md` created
* [ ] `src/data/targetArchitectures.ts` created
* [ ] Orchestrator target architecture constant removed
* [ ] Target architecture validation updated
* [ ] `src/data/migrationRules.ts` created
* [ ] Seven initial rules configured
* [ ] Migration rule types added
* [ ] Observability inventory added
* [ ] `evaluateMigrationRules.ts` created
* [ ] Rule statuses returned by the API
* [ ] `generateMigrationGaps.ts` created
* [ ] `deduplicateMigrationGaps.ts` created
* [ ] Gap scoring integrated
* [ ] `sampleMigrationProjects.ts` created
* [ ] Account Discount fixture added
* [ ] Sample listing route added
* [ ] Sample analysis route added
* [ ] Endpoint extraction added
* [ ] Sample endpoint inventory is non-empty
* [ ] Sample rule results are deterministic
* [ ] Sample gaps are deterministic
* [ ] Lint passes
* [ ] Production build passes
* [ ] Day 42 completion report created

---

## 25. Day 42 Outcome

At the end of Day 42, the project should support the following flow:

```text
Migration Analysis Request
        ↓
Validation
        ↓
Artifact Normalization
        ↓
Source Inventory Extraction
        ↓
Endpoint and Capability Detection
        ↓
Target Architecture Resolution
        ↓
Deterministic Rule Evaluation
        ↓
Structured Migration Gap Generation
        ↓
Gap Deduplication
        ↓
Migration Score Calculation
        ↓
Structured API Response
```

The expected milestone is:

```text
The AI-Assisted API Migration Analyzer can evaluate a realistic legacy API
sample using configurable deterministic rules and return repeatable,
evidence-oriented migration gaps.
```

This creates the domain foundation required for Day 43 to begin migration risk, recommendation, and task generation.
