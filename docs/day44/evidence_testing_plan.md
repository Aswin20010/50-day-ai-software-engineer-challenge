# Day 44 — Evidence Quality and Automated Test Foundation

## Phase 1: Evidence and Testing Implementation Plan

## 1. Purpose

This document defines the Day 44 implementation plan for the **AI-Assisted API Migration Analyzer**.

Day 43 completed the deterministic remediation pipeline. The analyzer can now:

* Validate migration-analysis requests
* Normalize source artifacts
* Build a source inventory
* Detect endpoint evidence
* Resolve a configured target architecture
* Evaluate migration rules
* Generate migration gaps
* Generate and deduplicate risks
* Generate recommendations
* Generate implementation-ready migration tasks
* Calculate readiness, complexity, and risk scores
* Return linked analysis output through API routes

Day 44 focuses on improving the trustworthiness and maintainability of that pipeline.

The primary objectives are:

* Improve evidence precision
* Add line-level source references
* Centralize evidence utilities
* Improve endpoint detection quality
* Improve capability evidence
* Prevent false-positive deployment detection
* Establish a TypeScript test framework
* Add focused unit tests
* Add full service integration tests
* Verify deterministic behavior automatically
* Establish a measurable coverage baseline

---

## 2. Current Project Status

Project 2 is located at:

```text
C:\Users\BH31021\api-factory-generator\api-migration-analyzer
```

The current Day 43 analysis flow is:

```text
Migration Analysis Request
        ↓
Request Validation
        ↓
Artifact Normalization
        ↓
Source Inventory Extraction
        ↓
Endpoint Detection
        ↓
Architecture Comparison
        ↓
Migration Rule Evaluation
        ↓
Gap Generation
        ↓
Gap Deduplication
        ↓
Risk Generation
        ↓
Risk Deduplication
        ↓
Recommendation Generation
        ↓
Migration Task Generation
        ↓
Score Calculation
        ↓
Summary Generation
        ↓
Structured API Response
```

The current result contains:

```text
ruleResults
gaps
risks
recommendations
migrationTasks
scores
summary
missingInformation
```

The primary remaining quality issue is that much of the evidence is still:

```text
File-level
Empty
Derived indirectly
Not line-referenced
Repeated across findings
```

Day 44 will make the output more traceable and verifiable.

---

## 3. Day 44 Objective

The Day 44 objective is:

```text
Build the Evidence Quality and Automated Test Foundation
```

By the end of Day 44, the analyzer should be able to:

1. Create line-level source evidence.
2. Produce short and readable excerpts.
3. Deduplicate evidence consistently.
4. Sort evidence deterministically.
5. Detect endpoints across multiple frameworks.
6. Attach evidence to architecture capabilities.
7. Distinguish generic configuration from deployment definitions.
8. Test deterministic helpers independently.
9. Test the complete sample-analysis workflow.
10. Detect regressions through automated tests.
11. Measure test coverage.
12. Maintain stable output across identical inputs.

---

## 4. Day 44 Scope

Day 44 includes:

```text
Evidence Helper Foundation
Line Number Detection
Source Excerpt Generation
Evidence Deduplication
Evidence Sorting
Endpoint Extractor Refactor
Multi-Framework Endpoint Detection
Capability Evidence Extraction
Deployment Detection Validation
Vitest Configuration
Unit Test Fixtures
Unit Tests
Integration Tests
Deterministic Comparison Tests
Coverage Reporting
Completion Documentation
```

Day 44 does not include:

* Browser UI development
* AI-generated findings
* Repository upload
* GitHub integration
* Persistent analysis storage
* Authentication
* Automated source-code changes
* Pull-request generation
* Full AST parsing
* Background processing
* User task management

The purpose of Day 44 is quality, not new product breadth.

---

## 5. Day 44 Phases

Day 44 is divided into seven phases.

### Phase 1 — Evidence and Testing Implementation Plan

Create:

```text
docs/day44/evidence_testing_plan.md
```

This document defines:

* Current evidence limitations
* Evidence-model expectations
* Endpoint extraction requirements
* Capability evidence requirements
* Testing framework configuration
* Unit test boundaries
* Integration test scenarios
* Coverage targets
* Validation commands
* Definition of done

---

### Phase 2 — Evidence Extraction Foundation

Create:

```text
src/lib/migration/evidenceHelpers.ts
```

This module will centralize evidence behavior.

Expected responsibilities:

```text
Create line evidence
Create file evidence
Find source line numbers
Build source excerpts
Normalize source excerpts
Create evidence keys
Deduplicate evidence
Merge evidence
Sort evidence
```

Expected functions may include:

```typescript
createFileEvidence()
createLineEvidence()
findLineNumber()
buildLineExcerpt()
createEvidenceKey()
deduplicateEvidence()
mergeEvidence()
sortEvidence()
```

The implementation should remain independent of API and UI code.

---

### Phase 3 — Endpoint Evidence Improvements

Create:

```text
src/lib/migration/extractEndpointEvidence.ts
```

Move endpoint extraction out of:

```text
src/lib/migration/migrationAnalysisOrchestrator.ts
```

The extractor should support:

```text
Documentation routes
Go net/http HandleFunc
Go router method calls
Express routes
Fastify routes
Spring MVC annotations
ASP.NET route attributes
```

Each detected endpoint should contain:

* Stable endpoint ID
* HTTP method where available
* Route
* Handler name where available
* Artifact ID
* Line-level evidence

The extractor must merge equivalent endpoint findings.

---

### Phase 4 — Capability Evidence Improvements

Create:

```text
src/lib/migration/extractCapabilityEvidence.ts
```

The capability extractor should detect evidence for:

```text
Service layer
Repository layer
Automated tests
Deployment configuration
Correlation identifiers
Structured logging
Distributed tracing
Metrics
Security mechanisms
Configuration keys
```

The architecture comparison should no longer return empty evidence for detected capabilities.

Expected improvement:

```json
{
  "capability": "SERVICE_LAYER",
  "sourceStatus": "PRESENT",
  "evidence": [
    {
      "artifactId": "sample-account-discount-service",
      "artifactName": "internal/service/discount_service.go",
      "referenceType": "LINE",
      "reference": "15",
      "excerpt": "type DiscountService interface {"
    }
  ]
}
```

---

### Phase 5 — Unit Test Foundation

Configure:

```text
Vitest
```

Expected scripts:

```json
{
  "scripts": {
    "test": "vitest run",
    "test:watch": "vitest",
    "test:coverage": "vitest run --coverage"
  }
}
```

Create test fixtures under:

```text
src/test/fixtures/
```

Add focused tests for:

```text
evidenceHelpers
migrationSeverityHelpers
evaluateMigrationRules
generateMigrationGaps
generateMigrationRisks
deduplicateMigrationRisks
generateMigrationRecommendations
generateMigrationTasks
```

---

### Phase 6 — Integration and Deterministic Tests

Create:

```text
src/services/migrationAnalysisService.test.ts
```

Test the complete Account Discount sample through the service layer.

Validate:

```text
status = COMPLETED
API-004 = MATCHED
DEPLOY-001 = MATCHED
gapCount = 2
riskCount = 2
recommendationCount = 2
migrationTaskCount = 2
```

Also validate:

* Task generation disabled
* Score calculation disabled
* Unsupported target architecture
* Missing project name
* Missing artifacts
* Empty artifact content
* Duplicate artifact identifiers
* Generic environment configuration is not deployment evidence
* Dockerfile is deployment evidence
* Repeated analysis produces identical deterministic collections

---

### Phase 7 — Validation and Completion Report

Create:

```text
docs/day44/day44_completion_report.md
```

The report should record:

* Files created
* Files updated
* Evidence improvements
* Endpoint patterns supported
* Capability evidence behavior
* Test framework configuration
* Test suites added
* Test count
* Coverage result
* Lint result
* Build result
* Sample analysis result
* Deterministic comparison result
* Known limitations
* Day 45 recommendation

---

## 6. Current Evidence Limitations

The current implementation contains several evidence limitations.

### Empty Architecture Evidence

Architecture capability findings may contain:

```typescript
evidence: []
```

even when the capability is marked:

```text
PRESENT
```

This makes the finding difficult to verify.

---

### File-Level Evidence Only

Some findings identify a source artifact but not the exact source location.

Example:

```json
{
  "referenceType": "FILE",
  "artifactName": "internal/router/routes.go"
}
```

This is useful but not precise enough for engineering review.

---

### Duplicate Evidence Logic

Evidence deduplication logic currently exists in multiple modules.

This creates a risk of:

* Inconsistent keys
* Different sorting behavior
* Repeated evidence
* Future maintenance problems

Day 44 will centralize evidence behavior.

---

### Endpoint Detection Inside the Orchestrator

Endpoint extraction currently lives inside the main orchestrator.

This makes the orchestrator larger and harder to test.

Day 44 will move endpoint extraction into a focused module.

---

### Deployment False Positives

Generic configuration artifacts must not automatically count as deployment definitions.

Example:

```text
config/application.env.example
```

This proves that configuration exists.

It does not prove that the application has:

```text
Dockerfile
Kubernetes manifest
Cloud deployment file
Terraform definition
Helm chart
```

The deployment detector must preserve this distinction.

---

## 7. Evidence Model Requirements

The existing evidence model should support:

```typescript
export interface AnalysisEvidence {
  artifactId: string;
  artifactName: string;
  referenceType: EvidenceReferenceType;
  reference?: string;
  excerpt?: string;
}
```

Supported reference types should include:

```typescript
export type EvidenceReferenceType =
  | "FILE"
  | "LINE"
  | "SECTION";
```

No duplicate type should be introduced if equivalent types already exist.

---

## 8. Line Evidence Requirements

Line-level evidence should include:

```text
artifactId
artifactName
referenceType = LINE
reference
excerpt
```

Example:

```json
{
  "artifactId": "sample-account-discount-routes",
  "artifactName": "internal/router/routes.go",
  "referenceType": "LINE",
  "reference": "12",
  "excerpt": "mux.HandleFunc(\"/api/v1/discount/interrogateAccount\", discountHandler.InterrogateAccount)"
}
```

Line numbers should be:

* One-based
* Deterministic
* Derived from normalized artifact content
* Returned as strings if required by the current model

---

## 9. Excerpt Requirements

Evidence excerpts should be:

* Short
* Human-readable
* Trimmed
* Single-line where practical
* Free from full file content
* Long enough to support the finding
* Deterministically generated

Recommended maximum length:

```text
200 characters
```

Long excerpts should be truncated safely.

Suggested output:

```text
type DiscountService interface {
```

Avoid returning entire functions or full SQL statements unless required.

---

## 10. Evidence Key Strategy

Evidence deduplication should use a stable key based on:

```text
artifactId
referenceType
reference
excerpt
```

Recommended format:

```typescript
[
  evidence.artifactId,
  evidence.referenceType,
  evidence.reference ?? "",
  evidence.excerpt ?? "",
].join("|")
```

`artifactName` may be included if needed, but `artifactId` should remain the primary identifier.

---

## 11. Evidence Sorting Strategy

Evidence should be sorted deterministically.

Recommended sorting order:

1. Artifact name
2. Artifact ID
3. Reference type
4. Numeric line number
5. Excerpt

Line references should be sorted numerically when possible.

Example:

```text
2
10
20
```

not:

```text
10
2
20
```

---

## 12. Evidence Merging Rules

When evidence collections are merged:

1. Remove exact duplicates.
2. Preserve all unique source references.
3. Use stable sorting.
4. Do not mutate the input arrays.
5. Do not include full artifact content.
6. Preserve line references.
7. Preserve excerpts where available.

---

## 13. Endpoint Detection Requirements

The endpoint extractor should support multiple common patterns.

### Documentation Format

```text
POST /api/v1/discount/interrogateAccount
```

---

### Go `HandleFunc`

```go
mux.HandleFunc(
    "/api/v1/discount/interrogateAccount",
    discountHandler.InterrogateAccount,
)
```

---

### Go Router Methods

Examples:

```go
router.POST("/api/v1/accounts", handler.Create)
router.GET("/api/v1/accounts/:id", handler.Get)
```

---

### Express

```typescript
router.post("/api/v1/accounts", handler);
```

---

### Fastify

```typescript
fastify.get("/api/v1/accounts", handler);
```

---

### Spring MVC

```java
@PostMapping("/api/v1/accounts")
```

or:

```java
@RequestMapping(
    method = RequestMethod.POST,
    value = "/api/v1/accounts"
)
```

---

### ASP.NET

```csharp
[HttpPost("/api/v1/accounts")]
```

or:

```csharp
[Route("api/v1/accounts")]
```

The initial implementation may remain regex-based.

---

## 14. Stable Endpoint Identifiers

Endpoint IDs should be deterministic.

Recommended format:

```text
endpoint-<method>-<route>
```

Example:

```text
endpoint-post-api-v1-discount-interrogateaccount
```

When the HTTP method is unavailable:

```text
endpoint-unknown-method-api-v1-discount-interrogateaccount
```

IDs must not use:

* Random UUIDs
* Timestamps
* Array position
* Analysis ID

---

## 15. Endpoint Deduplication

The same endpoint may be detected in:

* Route source code
* Handler source code
* API documentation
* OpenAPI artifacts

Equivalent endpoints should merge when they share:

```text
HTTP method
Normalized route
```

When method evidence is unavailable, route and handler information may be used cautiously.

Merged endpoints should retain:

* Method
* Route
* Handler name
* Combined evidence
* Stable ID

---

## 16. Route Normalization

Routes should be normalized before comparison.

Potential normalization includes:

* Trim whitespace
* Ensure leading slash
* Remove duplicate slashes
* Preserve path parameters
* Normalize framework-specific parameter syntax where safe

Examples that may eventually be equivalent:

```text
/accounts/:id
/accounts/{id}
```

For Day 44, this normalization may remain conservative.

Do not merge routes when equivalence is uncertain.

---

## 17. Capability Evidence Requirements

The following capabilities should include evidence when detected:

```text
SERVICE_LAYER
REPOSITORY_LAYER
AUTOMATED_TESTING
DEPLOYMENT_CONFIGURATION
SECURITY
CORRELATION_ID
STRUCTURED_LOGGING
DISTRIBUTED_TRACING
METRICS
CONFIGURATION
```

The exact capability names should remain compatible with the existing domain types.

---

## 18. Service Layer Evidence

Detect patterns such as:

```text
type SomethingService interface
class SomethingService
interface SomethingService
struct SomethingService
```

Example evidence:

```json
{
  "artifactName": "internal/service/discount_service.go",
  "referenceType": "LINE",
  "reference": "15",
  "excerpt": "type DiscountService interface {"
}
```

---

## 19. Repository Layer Evidence

Detect patterns such as:

```text
Repository
DAO
DataAccess
Store
Persistence
```

Detection should remain conservative to avoid classifying unrelated words as repository abstractions.

Example:

```text
type AccountRepository interface
```

---

## 20. Test Evidence

Test evidence may come from:

```text
TEST_CODE artifact type
_test.go
.test.ts
.spec.ts
describe(
test(
it(
JUnit annotations
xUnit attributes
```

Test evidence should reference the exact declaration where practical.

---

## 21. Deployment Evidence

Deployment artifacts should include:

```text
Dockerfile
docker-compose.yml
compose.yml
Kubernetes manifests
Helm charts
Terraform files
Cloud Build configuration
Cloud Run service definitions
Serverless definitions
Procfile
Vercel configuration
Netlify configuration
```

Generic configuration files must not count automatically.

The deployment helper should be tested directly.

---

## 22. Observability Evidence

Correlation detection may include:

```text
x-correlation-id
correlationId
correlation_id
traceId
trace_id
x-request-id
```

Structured logging detection may include:

```text
slog
zap
zerolog
logrus
logger.info
logger.error
```

Tracing detection may include:

```text
OpenTelemetry
otel
startSpan
trace.Span
```

Metrics detection may include:

```text
Prometheus
CounterVec
HistogramVec
meter.create
metrics.
```

Each mechanism should include source evidence.

---

## 23. Security Evidence

Security detection may include:

```text
Authorization
Bearer
OAuth
JWT
API key
HMAC
mTLS
```

The analyzer must avoid including real secret values in evidence excerpts.

Only structural evidence should be returned.

---

## 24. Configuration Key Evidence

Configuration-key detection may include:

```text
os.Getenv("DATABASE_URL")
process.env.DATABASE_URL
env("DATABASE_URL")
```

Configuration keys may be included in inventory output.

Evidence should show the lookup expression rather than sensitive runtime values.

---

## 25. Testing Framework Selection

Day 44 should use:

```text
Vitest
```

Reasons:

* Strong TypeScript support
* Fast execution
* Familiar Jest-style API
* Easy coverage integration
* Good compatibility with Next.js utility modules
* Minimal configuration for pure functions

The implementation should avoid browser-environment dependencies unless required.

Default environment:

```text
node
```

---

## 26. Expected Test Configuration

Potential file:

```text
vitest.config.ts
```

Expected configuration:

```typescript
import { defineConfig } from "vitest/config";
import path from "node:path";

export default defineConfig({
  test: {
    environment: "node",
    globals: true,
    coverage: {
      provider: "v8",
      reporter: [
        "text",
        "html",
        "json-summary",
      ],
    },
  },
  resolve: {
    alias: {
      "@": path.resolve(__dirname, "./src"),
    },
  },
});
```

The exact configuration should remain compatible with the current project.

---

## 27. Package Scripts

Expected package scripts:

```json
{
  "scripts": {
    "test": "vitest run",
    "test:watch": "vitest",
    "test:coverage": "vitest run --coverage"
  }
}
```

Do not remove existing scripts such as:

```text
dev
build
start
lint
```

---

## 28. Test Fixture Strategy

Create reusable fixtures under:

```text
src/test/fixtures/
```

Recommended files:

```text
artifacts.ts
gaps.ts
risks.ts
recommendations.ts
rules.ts
```

Fixtures should be:

* Small
* Readable
* Deterministic
* Independent of real credentials
* Focused on one behavior
* Reusable across tests

---

## 29. Evidence Helper Test Scenarios

Test:

* File evidence creation
* Line evidence creation
* First-line match
* Middle-line match
* Last-line match
* Missing match
* Excerpt trimming
* Excerpt truncation
* Duplicate removal
* Numeric line sorting
* Evidence merging
* Input immutability

---

## 30. Severity Helper Test Scenarios

Test:

```text
CRITICAL → P0
HIGH → P1
MEDIUM → P2
LOW → P3
```

Also test:

* Highest severity
* Highest priority
* Likelihood mapping
* Impact mapping
* Risk combination
* Score clamping
* Stable severity sorting

---

## 31. Rule Evaluator Test Scenarios

Test:

* `MATCHED`
* `NOT_MATCHED`
* `NOT_APPLICABLE`
* `INSUFFICIENT_EVIDENCE`
* Empty arrays
* Non-empty arrays
* Missing contract artifact
* Existing contract artifact
* Repository capability unknown
* Repository capability present
* Correlation mechanism present
* Unsupported operator fallback

---

## 32. Gap Generator Test Scenarios

Test:

* Empty rule results
* One matched rule
* Non-matched rule ignored
* Disabled rule ignored
* Stable gap ID
* Correct severity
* Correct category
* Correct evidence
* Correct affected components
* Duplicate gap handling

---

## 33. Risk Generator Test Scenarios

Test:

* Empty gaps
* Known rule template
* Fallback risk template
* Likelihood mapping
* Impact mapping
* Risk severity
* Stable risk ID
* Related gap IDs
* Evidence preservation
* Deterministic sorting

---

## 34. Risk Deduplication Test Scenarios

Test:

* Duplicate risks merge
* Distinct risks remain separate
* Highest likelihood retained
* Highest impact retained
* Highest severity retained
* Related gaps merge
* Rule IDs merge
* Components merge
* Evidence merges
* Stable ID selected

---

## 35. Recommendation Test Scenarios

Test:

* Empty gaps
* Known recommendation template
* Fallback recommendation
* Related risk linking
* Priority mapping
* Stable recommendation ID
* Target architecture ID
* Evidence preservation
* Duplicate recommendation merging
* Deterministic sorting

---

## 36. Migration Task Test Scenarios

Test:

* Empty recommendations
* Task template exists
* Missing task template
* P0 task generation
* P1 task generation
* P2 task generation
* P3 task behavior
* Complexity selection
* Acceptance criteria deduplication
* Stable task IDs
* Duplicate task merging
* Task generation disabled

---

## 37. Service Integration Test Scenarios

The complete sample integration test should verify:

```text
status = COMPLETED
```

Expected rule results:

```text
SRC-002       NOT_MATCHED
API-003       NOT_MATCHED
API-004       MATCHED
CONTRACT-001  NOT_MATCHED
OBS-001       NOT_MATCHED
TEST-001      NOT_MATCHED
DEPLOY-001    MATCHED
```

Expected summary:

```text
gapCount            = 2
riskCount           = 2
recommendationCount = 2
migrationTaskCount  = 2
```

---

## 38. Deployment Detection Integration Tests

### Generic Configuration

Input:

```text
config/application.env.example
```

Expected:

```text
deploymentArtifacts.length = 0
DEPLOY-001 = MATCHED
```

---

### Dockerfile

Input:

```dockerfile
FROM golang:1.24
WORKDIR /app
COPY . .
RUN go build ./...
```

Expected:

```text
deploymentArtifacts.length > 0
DEPLOY-001 = NOT_MATCHED
```

---

### Kubernetes Manifest

Input:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: account-api
```

Expected:

```text
deploymentArtifacts.length > 0
DEPLOY-001 = NOT_MATCHED
```

---

## 39. Task Disablement Test

Set:

```typescript
generateMigrationTasks: false
```

Expected:

```text
gaps.length > 0
risks.length > 0
recommendations.length > 0
migrationTasks.length = 0
summary.migrationTaskCount = 0
```

---

## 40. Score Disablement Test

Set:

```typescript
calculateScores: false
```

Expected:

```json
{
  "readiness": 0,
  "complexity": 0,
  "risk": 0
}
```

Other collections should still be generated.

---

## 41. Deterministic Test Strategy

Run the same analysis twice.

Normalize away fields that are expected to change:

```text
analysisId
metadata.createdAt
metadata.completedAt
metadata.durationMs
```

Compare:

```text
sourceInventory
architectureComparison
ruleResults
gaps
risks
recommendations
migrationTasks
scores
summary
missingInformation
```

Expected:

```text
Exact equality
```

---

## 42. Coverage Targets

Day 44 should establish a meaningful baseline.

Target:

```text
Overall deterministic migration modules: 80%+
```

Target for core generators:

```text
90%+
```

Core modules include:

```text
evaluateMigrationRules
generateMigrationGaps
generateMigrationRisks
deduplicateMigrationRisks
generateMigrationRecommendations
generateMigrationTasks
migrationSeverityHelpers
evidenceHelpers
```

Coverage should be used to identify missing behavior, not to add meaningless tests.

---

## 43. Validation Commands

Run all commands from:

```powershell
cd C:\Users\BH31021\api-factory-generator\api-migration-analyzer
```

Install test dependencies if required:

```powershell
npm install -D vitest @vitest/coverage-v8
```

Run lint:

```powershell
npm run lint
```

Run tests:

```powershell
npm run test
```

Run coverage:

```powershell
npm run test:coverage
```

Run production build:

```powershell
npm run build
```

Start the development server:

```powershell
npm run dev
```

---

## 44. Manual API Validation

Run:

```powershell
$response = Invoke-RestMethod `
    -Uri "http://localhost:3000/api/migration-samples/account-discount/analyze" `
    -Method POST
```

Check counts:

```powershell
$response.summary |
    Select-Object `
        gapCount,
        riskCount,
        recommendationCount,
        migrationTaskCount |
    Format-List
```

Expected:

```text
gapCount            : 2
riskCount           : 2
recommendationCount : 2
migrationTaskCount  : 2
```

---

## 45. Evidence Validation

Inspect endpoint evidence:

```powershell
$response.sourceInventory.endpoints |
    Select-Object `
        method,
        route,
        handlerName,
        evidence |
    ConvertTo-Json -Depth 20
```

Expected evidence should include:

```text
referenceType = LINE
reference = numeric line
excerpt = short source line
```

---

## 46. Capability Evidence Validation

Inspect:

```powershell
$response.architectureComparison.capabilities |
    Select-Object `
        capability,
        sourceStatus,
        evidence |
    ConvertTo-Json -Depth 20
```

Capabilities marked `PRESENT` should contain evidence where source support exists.

Capabilities marked `UNKNOWN` may contain an empty evidence array.

---

## 47. Day 44 Definition of Done

Day 44 is complete when:

* Evidence helpers are centralized.
* Line evidence can be created.
* Evidence excerpts are normalized.
* Evidence is deduplicated.
* Evidence is deterministically sorted.
* Endpoint extraction is moved out of the orchestrator.
* Multiple endpoint frameworks are supported.
* Detected endpoints contain line evidence.
* Capability detection contains evidence.
* Generic configuration is not deployment evidence.
* Dockerfile is deployment evidence.
* Kubernetes configuration is deployment evidence.
* Vitest is configured.
* Unit test fixtures exist.
* Evidence helper tests pass.
* Severity helper tests pass.
* Rule evaluator tests pass.
* Gap generator tests pass.
* Risk generator tests pass.
* Risk deduplication tests pass.
* Recommendation tests pass.
* Migration task tests pass.
* Service integration tests pass.
* Deterministic comparison passes.
* Coverage baseline is met.
* Lint passes.
* Production build passes.
* Completion documentation exists.

---

## 48. Day 44 Completion Checklist

* [ ] `docs/day44/evidence_testing_plan.md` created
* [ ] `evidenceHelpers.ts` created
* [ ] File evidence helper implemented
* [ ] Line evidence helper implemented
* [ ] Excerpt generation implemented
* [ ] Evidence key implemented
* [ ] Evidence deduplication implemented
* [ ] Evidence sorting implemented
* [ ] Evidence merging implemented
* [ ] `extractEndpointEvidence.ts` created
* [ ] Endpoint logic removed from orchestrator
* [ ] Documentation endpoint detection added
* [ ] Go endpoint detection added
* [ ] Express endpoint detection added
* [ ] Fastify endpoint detection added
* [ ] Spring endpoint detection added
* [ ] ASP.NET endpoint detection added
* [ ] Endpoint line evidence added
* [ ] `extractCapabilityEvidence.ts` created
* [ ] Service evidence added
* [ ] Repository evidence added
* [ ] Test evidence added
* [ ] Deployment evidence added
* [ ] Observability evidence added
* [ ] Security evidence added
* [ ] Configuration evidence added
* [ ] Generic configuration false positive fixed
* [ ] Vitest installed
* [ ] Vitest configuration created
* [ ] Test scripts added
* [ ] Test fixtures created
* [ ] Unit tests added
* [ ] Integration tests added
* [ ] Task disablement tested
* [ ] Score disablement tested
* [ ] Deterministic output tested
* [ ] Coverage report generated
* [ ] Lint passes
* [ ] Tests pass
* [ ] Production build passes
* [ ] Completion report created

---

## 49. Known Limitations

At the end of Day 44:

* Endpoint extraction remains regex-based.
* Line evidence depends on normalized source text.
* Multi-line declarations may use the first matching line.
* Route composition may not be fully resolved.
* Framework-specific middleware is not deeply analyzed.
* Capability detection remains naming-pattern based.
* Evidence confidence is not yet statistically calibrated.
* No AST parser is included.
* No browser evidence viewer exists.
* Test coverage will initially focus on deterministic modules.

These limitations are acceptable for the current MVP.

---

## 50. Recommended Day 45 Objective

Recommended Day 45 title:

```text
Migration Analyzer Browser Workflow
```

Day 45 should focus on:

1. Project input form
2. Sample project selection
3. Target architecture selection
4. Analysis execution
5. Loading and error states
6. Summary scorecards
7. Rule-result display
8. Gap display
9. Risk display
10. Recommendation display
11. Migration task display
12. Evidence inspection
13. Deterministic sample validation
14. Browser usability checks

Expected outcome:

```text
Users can run and inspect a complete migration analysis through a browser
without manually calling API endpoints.
```

---

## 51. Final Day 44 Outcome

At the end of Day 44, the analyzer should support:

```text
Source Artifacts
        ↓
Precise Evidence Extraction
        ↓
Endpoint and Capability Detection
        ↓
Deterministic Analysis
        ↓
Gaps, Risks, Recommendations, and Tasks
        ↓
Automated Unit and Integration Validation
        ↓
Coverage and Regression Protection
```

The expected milestone is:

```text
The AI-Assisted API Migration Analyzer produces traceable evidence-backed
findings and is protected by a deterministic automated test foundation.
```

This creates the quality foundation required before exposing the analysis workflow through the browser.
