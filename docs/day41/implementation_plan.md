# Day 41 — AI-Assisted API Migration Analyzer

## Phase 6: Implementation Plan

## 1. Purpose

This document defines the implementation plan for building the first working version of the **AI-Assisted API Migration Analyzer**.

The plan converts the Day 41 architecture, data model, and migration-analysis rules into a practical sequence of development tasks.

The implementation must prioritize:

* A functional end-to-end analysis workflow
* Deterministic migration findings
* Clear separation between UI and analysis logic
* Traceable evidence
* Structured JSON output
* Testable pure functions
* A realistic MVP that can be completed within the remaining 50-Day Roadmap

---

## 2. MVP Implementation Goal

The initial MVP should allow a user to:

1. Create a migration-analysis project.
2. Provide legacy API information.
3. Select a target architecture.
4. Run deterministic migration analysis.
5. Review detected endpoints and dependencies.
6. Review architecture gaps and migration risks.
7. Review generated recommendations and migration tasks.
8. View readiness, complexity, and risk scores.
9. Export the complete analysis as JSON.

The MVP does not need to analyze an entire Git repository automatically.

The first version may accept:

* Pasted source code
* Pasted API documentation
* JSON contracts
* OpenAPI fragments
* Database schema descriptions
* Repository structure descriptions
* Manually entered source-system metadata

---

## 3. Implementation Principles

The implementation should follow these principles:

* Build the normalized data model before building the UI.
* Implement deterministic analysis before AI integration.
* Keep analysis functions independent of React.
* Prefer pure functions for rules, scoring, and transformations.
* Preserve source evidence throughout the workflow.
* Return partial results when some artifacts cannot be analyzed.
* Avoid silently guessing missing information.
* Keep the initial source parser simple and extensible.
* Use fixture-based development.
* Complete one end-to-end vertical slice before adding advanced features.

---

## 4. Recommended Implementation Order

```text
Project Types
    ↓
Static Target Architecture Definitions
    ↓
Migration Rule Configuration
    ↓
Input Validation
    ↓
Source Normalization
    ↓
Inventory Extraction
    ↓
Rule Evaluation
    ↓
Gap and Risk Generation
    ↓
Score Calculation
    ↓
Recommendation and Task Generation
    ↓
Analysis Service
    ↓
API Route
    ↓
React Workflow Hook
    ↓
User Interface
    ↓
JSON Export
    ↓
Tests and Validation
```

---

## 5. Phase 6 Deliverables

The implementation should produce the following core files:

```text
src/
├── app/
│   ├── page.tsx
│   └── api/
│       └── migration-analysis/
│           └── route.ts
│
├── components/
│   └── migration/
│       ├── MigrationProjectForm.tsx
│       ├── SourceArtifactEditor.tsx
│       ├── SourceInputPanel.tsx
│       ├── TargetArchitecturePanel.tsx
│       ├── AnalysisProgress.tsx
│       ├── MigrationSummaryPanel.tsx
│       ├── SourceInventoryPanel.tsx
│       ├── EndpointFindingsPanel.tsx
│       ├── GapFindingsPanel.tsx
│       ├── RiskFindingsPanel.tsx
│       ├── RecommendationPanel.tsx
│       ├── MigrationTaskPanel.tsx
│       └── AnalysisExportPanel.tsx
│
├── hooks/
│   └── useMigrationAnalysis.ts
│
├── services/
│   ├── migrationAnalysisService.ts
│   └── migrationReportService.ts
│
├── lib/
│   └── migration/
│       ├── validateMigrationRequest.ts
│       ├── normalizeSourceArtifacts.ts
│       ├── extractSourceInventory.ts
│       ├── analyzeEndpoints.ts
│       ├── analyzeDependencies.ts
│       ├── analyzeDataAccess.ts
│       ├── extractBusinessRules.ts
│       ├── compareTargetArchitecture.ts
│       ├── evaluateMigrationRules.ts
│       ├── generateMigrationGaps.ts
│       ├── generateMigrationRisks.ts
│       ├── generateRecommendations.ts
│       ├── generateMigrationTasks.ts
│       ├── calculateMigrationScores.ts
│       ├── deduplicateFindings.ts
│       └── evidenceHelpers.ts
│
├── types/
│   └── migration.ts
│
├── data/
│   ├── targetArchitectures.ts
│   ├── migrationRules.ts
│   └── sampleMigrationProjects.ts
│
└── tests/
    ├── fixtures/
    │   └── legacyAccountDiscountApi.ts
    ├── unit/
    └── integration/
```

---

## 6. Step 1 — Create the Migration Types

Recommended file:

```text
src/types/migration.ts
```

The file should define all shared domain types.

Minimum required types:

```typescript
export type MigrationAnalysisStatus =
  | "IDLE"
  | "VALIDATING"
  | "PARSING_SOURCE"
  | "ANALYZING"
  | "COMPARING"
  | "GENERATING_REPORT"
  | "COMPLETED"
  | "FAILED";

export type MigrationSeverity =
  | "LOW"
  | "MEDIUM"
  | "HIGH"
  | "CRITICAL";

export type MigrationPriority =
  | "P0"
  | "P1"
  | "P2"
  | "P3";

export type MigrationComplexity =
  | "SMALL"
  | "MEDIUM"
  | "LARGE"
  | "EXTRA_LARGE";

export type FindingOrigin =
  | "DETERMINISTIC"
  | "AI_ASSISTED"
  | "USER_PROVIDED";
```

The type file should also include:

* `MigrationAnalysisRequest`
* `MigrationAnalysisResult`
* `MigrationProject`
* `SourceSystem`
* `SourceArtifact`
* `SourceInventory`
* `EndpointFinding`
* `DependencyFinding`
* `DataFinding`
* `MigrationBusinessRule`
* `MigrationGap`
* `MigrationRisk`
* `MigrationRecommendation`
* `MigrationTask`
* `MigrationScores`
* `MissingInformationFinding`
* `AnalysisEvidence`
* `TargetArchitecture`
* `MigrationRuleDefinition`
* `MigrationRuleResult`
* `MigrationAnalysisError`
* `MigrationAnalysisMetadata`

All analysis modules should import these shared types rather than defining local duplicates.

---

## 7. Step 2 — Define Target Architectures

Recommended file:

```text
src/data/targetArchitectures.ts
```

The first MVP target architecture should be:

```text
Go Cloud-Native Layered API
```

It should define expectations for:

* Go service implementation
* HTTP handler layer
* Service layer
* Repository layer
* External client layer
* Request validation
* Centralized error mapping
* Structured logging
* Correlation identifiers
* Configuration validation
* Context propagation
* Transaction-aware persistence
* Unit and integration testing
* Container deployment
* Liveness and readiness endpoints

Example structure:

```typescript
export const targetArchitectures: TargetArchitecture[] = [
  {
    id: "go-cloud-native-api",
    name: "Go Cloud-Native Layered API",
    description:
      "A production-oriented Go API using handler, service, repository, and client layers.",
    language: "Go",
    framework: "net/http",
    requirements: {
      requiresHandlerLayer: true,
      requiresServiceLayer: true,
      requiresRepositoryLayer: true,
      requiresClientLayer: true,
      requiresStructuredLogging: true,
      requiresCorrelationId: true,
      requiresHealthChecks: true,
      requiresConfigurationValidation: true,
      requiresAutomatedTests: true,
    },
  },
];
```

A second optional target may be added later:

```text
TypeScript Serverless API
```

The MVP should not require multiple targets before the first end-to-end workflow works.

---

## 8. Step 3 — Implement Migration Rule Configuration

Recommended file:

```text
src/data/migrationRules.ts
```

The first implementation should include a focused subset of high-value rules.

Recommended initial rules:

```text
SRC-002      No endpoint evidence
SRC-003      Missing target architecture
API-001      Handler contains business logic
API-002      Handler accesses database directly
API-003      Missing service layer
API-004      Missing repository layer
API-006      Missing request validation
CONTRACT-001 Missing API contract
DEP-001      Undocumented external dependency
DEP-002      Hard-coded dependency URL
DEP-003      Missing timeout
DATA-002     Transaction boundary not defined
DATA-003     Concurrency control missing
DATA-005     Missing audit behavior
BUS-001      Undocumented status transition
BUS-002      Magic value used in business rule
SEC-001      Hard-coded credential
SEC-002      Missing authentication evidence
ERR-001      Raw internal error returned
OBS-001      Missing correlation identifier
OBS-002      Unstructured logging
TEST-001     No automated test evidence
TEST-002     Critical business rule not tested
DEPLOY-001   Missing deployment definition
DOC-001      Endpoint documentation missing
```

The remaining Day 41 rules can be added after the core evaluator is stable.

---

## 9. Step 4 — Implement Request Validation

Recommended file:

```text
src/lib/migration/validateMigrationRequest.ts
```

The validator should verify:

* Project name exists.
* Source application name exists.
* At least one source artifact exists.
* Target architecture ID exists.
* Artifact names are not empty.
* Artifact content is not empty.
* Artifact size is within the configured limit.
* Supported artifact types are used.
* Analysis options are valid.

Suggested function:

```typescript
export function validateMigrationRequest(
  request: MigrationAnalysisRequest
): MigrationAnalysisError[];
```

Validation should return all discovered errors rather than stopping after the first error.

Example validation result:

```typescript
[
  {
    code: "MISSING_PROJECT_NAME",
    message: "Project name is required.",
    field: "projectName",
    recoverable: true,
  },
];
```

---

## 10. Step 5 — Normalize Source Artifacts

Recommended file:

```text
src/lib/migration/normalizeSourceArtifacts.ts
```

Responsibilities:

* Trim artifact names and content.
* Generate stable artifact IDs.
* Normalize language values.
* Normalize artifact types.
* Remove empty artifacts.
* Preserve supplied metadata.
* Record unsupported artifact types.
* Prevent duplicate artifact IDs.
* Normalize line endings for evidence references.

Suggested function:

```typescript
export function normalizeSourceArtifacts(
  artifacts: SourceArtifactInput[]
): SourceArtifact[];
```

The normalizer must not change business meaning.

---

## 11. Step 6 — Extract the Source Inventory

Recommended file:

```text
src/lib/migration/extractSourceInventory.ts
```

The first extractor should use conservative text-pattern detection.

It may identify:

* Language names
* Framework names
* Route declarations
* Handler names
* Service names
* Repository names
* SQL statements
* Table names
* External URLs
* Environment-variable references
* Authentication headers
* Logging statements
* Test files
* Docker or deployment files

Suggested function:

```typescript
export function extractSourceInventory(
  sourceSystem: SourceSystem
): SourceInventory;
```

The output should include:

```typescript
{
  detectedLanguages: [],
  detectedFrameworks: [],
  endpoints: [],
  services: [],
  repositories: [],
  externalDependencies: [],
  dataStores: [],
  configurationKeys: [],
  securityMechanisms: [],
  testArtifacts: [],
  deploymentArtifacts: [],
  evidence: [],
}
```

The initial extractor does not need a full abstract syntax tree.

---

## 12. Step 7 — Implement Endpoint Analysis

Recommended file:

```text
src/lib/migration/analyzeEndpoints.ts
```

The analyzer should generate one `EndpointFinding` for each detected endpoint.

It should attempt to identify:

* Method
* Route
* Handler
* Request fields
* Headers
* Validation
* Service calls
* Repository calls
* External calls
* Database operations
* Success responses
* Error responses
* Status transitions
* Transaction behavior
* Logging behavior
* Test evidence

Suggested function:

```typescript
export function analyzeEndpoints(
  artifacts: SourceArtifact[],
  inventory: SourceInventory
): EndpointFinding[];
```

Unknown properties must remain undefined or be added to missing-information findings.

They must not be invented.

---

## 13. Step 8 — Implement Dependency Analysis

Recommended file:

```text
src/lib/migration/analyzeDependencies.ts
```

The first version should classify detected dependencies as:

```text
REUSE
REPLACE
REFACTOR
REMOVE
UNKNOWN
```

Classification examples:

* Standard library dependency: `REUSE`
* Unsupported legacy framework: `REPLACE`
* Hard-coded downstream client: `REFACTOR`
* Verified unused package: `REMOVE`
* Unclear proprietary system: `UNKNOWN`

Suggested function:

```typescript
export function analyzeDependencies(
  artifacts: SourceArtifact[],
  inventory: SourceInventory
): DependencyFinding[];
```

Each dependency finding should include evidence and confidence.

---

## 14. Step 9 — Implement Data Access Analysis

Recommended file:

```text
src/lib/migration/analyzeDataAccess.ts
```

The initial analyzer should identify:

* SQL queries
* Table names
* Insert operations
* Update operations
* Delete operations
* Transactions
* Locks
* Audit inserts
* Direct handler database access
* Potential read-modify-write workflows
* Currency fields
* Timestamp fields

Suggested function:

```typescript
export function analyzeDataAccess(
  artifacts: SourceArtifact[],
  endpoints: EndpointFinding[]
): DataFinding[];
```

The implementation should detect evidence but should not claim transaction safety unless clear transaction handling exists.

---

## 15. Step 10 — Extract Business Rules

Recommended file:

```text
src/lib/migration/extractBusinessRules.ts
```

The first implementation should focus on recognizable patterns:

* Required-field validation
* Status comparisons
* Status transitions
* Amount validation
* Balance updates
* Authorization checks
* Duplicate checks
* Success and decline conditions
* Configuration-dependent behavior
* Error-code mapping

Suggested function:

```typescript
export function extractBusinessRules(
  artifacts: SourceArtifact[],
  endpoints: EndpointFinding[]
): MigrationBusinessRule[];
```

Every business rule should include:

* Rule title
* Description
* Category
* Affected endpoint
* Evidence
* Confidence
* Origin

---

## 16. Step 11 — Compare with the Target Architecture

Recommended file:

```text
src/lib/migration/compareTargetArchitecture.ts
```

The comparator should create a normalized comparison context.

Suggested function:

```typescript
export function compareTargetArchitecture(
  sourceInventory: SourceInventory,
  endpoints: EndpointFinding[],
  dependencies: DependencyFinding[],
  dataFindings: DataFinding[],
  targetArchitecture: TargetArchitecture
): ArchitectureComparisonResult;
```

The result should indicate whether required capabilities are:

```text
PRESENT
PARTIAL
MISSING
UNKNOWN
NOT_APPLICABLE
```

Example:

```typescript
{
  capability: "SERVICE_LAYER",
  sourceStatus: "MISSING",
  targetRequirement: "REQUIRED",
  evidence: [],
}
```

---

## 17. Step 12 — Implement Rule Evaluation

Recommended file:

```text
src/lib/migration/evaluateMigrationRules.ts
```

Suggested function:

```typescript
export function evaluateMigrationRules(
  context: MigrationAnalysisContext,
  rules: MigrationRuleDefinition[]
): MigrationRuleResult[];
```

The evaluator should:

1. Ignore disabled rules.
2. Determine whether each rule applies.
3. Evaluate the condition.
4. Collect evidence.
5. Assign confidence.
6. Return a deterministic result.
7. Avoid generating UI-specific text.
8. Preserve rule IDs for traceability.

The evaluator should support the following statuses:

```text
MATCHED
NOT_MATCHED
NOT_APPLICABLE
INSUFFICIENT_EVIDENCE
```

---

## 18. Step 13 — Generate Gaps

Recommended file:

```text
src/lib/migration/generateMigrationGaps.ts
```

Suggested function:

```typescript
export function generateMigrationGaps(
  ruleResults: MigrationRuleResult[],
  rules: MigrationRuleDefinition[]
): MigrationGap[];
```

Each gap should include:

* Gap ID
* Rule ID
* Category
* Title
* Description
* Severity
* Confidence
* Evidence
* Affected endpoints
* Target expectation
* Migration impact
* Recommendation reference

IDs should be stable for the same deterministic input.

Example:

```text
GAP-API-002-ACCOUNT-CREATE
```

---

## 19. Step 14 — Generate Risks

Recommended file:

```text
src/lib/migration/generateMigrationRisks.ts
```

Risks should be derived from matched rules and gaps.

Suggested function:

```typescript
export function generateMigrationRisks(
  gaps: MigrationGap[],
  context: MigrationAnalysisContext
): MigrationRisk[];
```

Risk properties should include:

* Risk ID
* Title
* Description
* Likelihood
* Impact
* Severity
* Evidence
* Affected components
* Mitigation
* Related gap IDs

The first version may calculate likelihood and impact using configured rule defaults.

---

## 20. Step 15 — Generate Recommendations

Recommended file:

```text
src/lib/migration/generateRecommendations.ts
```

Suggested function:

```typescript
export function generateRecommendations(
  gaps: MigrationGap[],
  risks: MigrationRisk[],
  targetArchitecture: TargetArchitecture
): MigrationRecommendation[];
```

Recommendations should be generated from deterministic templates.

The implementation should replace template placeholders such as:

```text
{{endpointName}}
{{componentName}}
{{targetArchitectureName}}
{{dependencyName}}
```

Example:

```text
Introduce a repository interface for the account-create endpoint and move
database operations out of the HTTP handler.
```

AI-generated wording can be added later through a separate adapter.

---

## 21. Step 16 — Generate Migration Tasks

Recommended file:

```text
src/lib/migration/generateMigrationTasks.ts
```

Suggested function:

```typescript
export function generateMigrationTasks(
  gaps: MigrationGap[],
  recommendations: MigrationRecommendation[]
): MigrationTask[];
```

Task-generation logic should:

* Generate at least one task for each critical or high gap.
* Assign priority using severity and impact.
* Assign complexity using affected components and scope.
* Add acceptance criteria.
* Link source gap IDs.
* Add task dependencies.
* Sort tasks into a recommended sequence.

Suggested implementation sequence:

```text
Discovery and Contract Tasks
    ↓
Security and Data Integrity Tasks
    ↓
Architecture Refactoring Tasks
    ↓
Dependency Integration Tasks
    ↓
Testing Tasks
    ↓
Observability and Deployment Tasks
    ↓
Documentation Tasks
```

---

## 22. Step 17 — Implement Score Calculation

Recommended file:

```text
src/lib/migration/calculateMigrationScores.ts
```

Suggested functions:

```typescript
export function calculateReadinessScore(
  context: MigrationAnalysisContext,
  gaps: MigrationGap[]
): number;

export function calculateComplexityScore(
  context: MigrationAnalysisContext,
  gaps: MigrationGap[]
): number;

export function calculateRiskScore(
  risks: MigrationRisk[]
): number;
```

A combined function may return:

```typescript
export function calculateMigrationScores(
  context: MigrationAnalysisContext,
  gaps: MigrationGap[],
  risks: MigrationRisk[]
): MigrationScores;
```

Requirements:

* Scores must remain between `0` and `100`.
* Weight constants should be named.
* Calculation logic should be pure.
* Tests should cover minimum, maximum, and mixed findings.
* Score explanations should be included in the result.

---

## 23. Step 18 — Deduplicate Findings

Recommended file:

```text
src/lib/migration/deduplicateFindings.ts
```

Suggested functions:

```typescript
export function deduplicateGaps(
  gaps: MigrationGap[]
): MigrationGap[];

export function deduplicateRisks(
  risks: MigrationRisk[]
): MigrationRisk[];

export function deduplicateRecommendations(
  recommendations: MigrationRecommendation[]
): MigrationRecommendation[];
```

Duplicate detection should consider:

* Rule ID
* Endpoint ID
* Component ID
* Evidence location
* Technical cause
* Category

Merged findings should retain:

* Highest severity
* Highest confidence
* Combined evidence
* Combined affected components

---

## 24. Step 19 — Build the Analysis Service

Recommended file:

```text
src/services/migrationAnalysisService.ts
```

Primary service function:

```typescript
export async function analyzeMigrationProject(
  request: MigrationAnalysisRequest
): Promise<MigrationAnalysisResult>;
```

Recommended service sequence:

```typescript
validate request
normalize artifacts
resolve target architecture
extract source inventory
analyze endpoints
analyze dependencies
analyze data access
extract business rules
compare source with target
evaluate migration rules
generate gaps
deduplicate gaps
generate risks
deduplicate risks
generate recommendations
generate migration tasks
calculate scores
generate summary
return structured result
```

The service should be the only module that coordinates the complete workflow.

Individual analyzers should not call each other unnecessarily.

---

## 25. Step 20 — Implement the API Route

Recommended file:

```text
src/app/api/migration-analysis/route.ts
```

The route should:

* Accept `POST` requests.
* Parse JSON safely.
* Call `analyzeMigrationProject`.
* Return a structured success response.
* Return normalized validation errors.
* Avoid returning internal stack traces.
* Log only safe metadata.
* Add a generated analysis ID.
* Include analysis-version metadata.

Suggested success status:

```text
200 OK
```

Suggested validation status:

```text
400 Bad Request
```

Suggested unsupported-input status:

```text
422 Unprocessable Entity
```

Suggested unexpected-error status:

```text
500 Internal Server Error
```

---

## 26. Step 21 — Create the Workflow Hook

Recommended file:

```text
src/hooks/useMigrationAnalysis.ts
```

The hook should manage:

* Project input
* Source artifacts
* Selected target architecture
* Analysis options
* Validation errors
* Current workflow status
* Current analysis stage
* Completed result
* API error
* Reset behavior

Suggested result interface:

```typescript
interface UseMigrationAnalysisResult {
  project: MigrationProjectInput;
  result: MigrationAnalysisResult | null;
  status: MigrationAnalysisStatus;
  error: MigrationAnalysisError | null;
  validationErrors: MigrationAnalysisError[];
  analyze: () => Promise<void>;
  reset: () => void;
  updateProject: (
    updates: Partial<MigrationProjectInput>
  ) => void;
}
```

The hook should not contain migration-analysis rules.

---

## 27. Step 22 — Build the Project Setup UI

Recommended component:

```text
MigrationProjectForm.tsx
```

Fields should include:

* Project name
* Source application name
* Primary language
* Source framework
* Project description
* Target architecture
* AI assistance toggle
* Generate migration tasks toggle
* Calculate scores toggle

The form should display inline validation errors.

---

## 28. Step 23 — Build the Source Input UI

Recommended components:

```text
SourceInputPanel.tsx
SourceArtifactEditor.tsx
```

Users should be able to add multiple artifacts.

Each artifact should include:

* Name
* Type
* Language
* Content
* Optional description

Supported MVP artifact types:

```text
SOURCE_CODE
API_DOCUMENTATION
OPENAPI
DATABASE_SCHEMA
CONFIGURATION
REPOSITORY_STRUCTURE
TEST_CODE
DEPLOYMENT_CONFIGURATION
OTHER_TEXT
```

The UI should provide:

* Add artifact
* Remove artifact
* Edit artifact
* Character count
* Artifact type selection
* Sample artifact loading

---

## 29. Step 24 — Build the Analysis Progress UI

Recommended component:

```text
AnalysisProgress.tsx
```

The UI should display the current stage:

```text
Validating project
Normalizing source
Extracting source inventory
Analyzing endpoints
Analyzing dependencies
Comparing target architecture
Generating findings
Calculating scores
Generating migration plan
Completed
```

The progress display should not report fabricated percentages.

Stage-based progress is sufficient for the MVP.

---

## 30. Step 25 — Build the Summary UI

Recommended component:

```text
MigrationSummaryPanel.tsx
```

The summary should display:

* Project name
* Source application
* Target architecture
* Number of artifacts
* Number of endpoints
* Number of dependencies
* Number of business rules
* Number of gaps
* Number of risks
* Number of recommendations
* Number of migration tasks
* Readiness score
* Complexity score
* Risk score

The summary should visually distinguish critical and high findings.

---

## 31. Step 26 — Build the Findings UI

Recommended components:

```text
EndpointFindingsPanel.tsx
GapFindingsPanel.tsx
RiskFindingsPanel.tsx
RecommendationPanel.tsx
MigrationTaskPanel.tsx
```

Each gap should display:

* Severity
* Category
* Title
* Description
* Confidence
* Affected endpoint or component
* Evidence
* Migration impact
* Recommended action

Each risk should display:

* Likelihood
* Impact
* Severity
* Mitigation
* Related gaps

Each task should display:

* Priority
* Complexity
* Description
* Acceptance criteria
* Dependencies
* Related finding IDs

---

## 32. Step 27 — Implement JSON Export

Recommended file:

```text
src/services/migrationReportService.ts
```

Suggested functions:

```typescript
export function serializeMigrationReport(
  result: MigrationAnalysisResult
): string;

export function buildMigrationReportFilename(
  result: MigrationAnalysisResult
): string;
```

Recommended filename format:

```text
<project-name>-migration-analysis.json
```

Example:

```text
legacy-account-discount-api-migration-analysis.json
```

The exported JSON should include:

* Analysis metadata
* Source inventory
* Endpoint findings
* Dependency findings
* Data findings
* Business rules
* Gaps
* Risks
* Recommendations
* Migration tasks
* Scores
* Missing information

Secrets and full sensitive values must not be exported.

---

## 33. Step 28 — Create a Sample Migration Project

Recommended file:

```text
src/data/sampleMigrationProjects.ts
```

The first sample should represent:

```text
Legacy Account Discount API
```

Suggested endpoints:

```text
POST /api/v1/discount/interrogateAccount
POST /api/v1/discount/evaluateTransactionDiscount
POST /api/v1/discount/commitTransactionDiscount
POST /api/v1/discount/rollbackTransactionDiscount
```

The sample may include:

* Direct handler database access
* Missing repository separation
* External identity lookup dependency
* Transactional commit workflow
* Row-locking requirement
* Status transitions
* Structured headers
* Validation requirements
* Limited test evidence
* Missing deployment documentation

This fixture should intentionally trigger several deterministic rules.

---

## 34. Step 29 — Unit Test Plan

Unit tests should be implemented for the pure analysis modules.

Recommended tests:

```text
validateMigrationRequest.test.ts
normalizeSourceArtifacts.test.ts
extractSourceInventory.test.ts
analyzeEndpoints.test.ts
analyzeDependencies.test.ts
analyzeDataAccess.test.ts
extractBusinessRules.test.ts
compareTargetArchitecture.test.ts
evaluateMigrationRules.test.ts
generateMigrationGaps.test.ts
generateMigrationRisks.test.ts
generateRecommendations.test.ts
generateMigrationTasks.test.ts
calculateMigrationScores.test.ts
deduplicateFindings.test.ts
migrationReportService.test.ts
```

Each rule evaluator test should include:

* Matching input
* Non-matching input
* Insufficient-evidence input
* Not-applicable input
* Evidence verification
* Confidence verification

---

## 35. Step 30 — Integration Test Plan

Recommended integration file:

```text
tests/integration/migrationAnalysisService.test.ts
```

Integration scenarios should include:

### Scenario 1 — Valid Legacy API

Expected:

* Analysis completes.
* Endpoints are detected.
* Gaps are generated.
* Risks are generated.
* Recommendations are generated.
* Migration tasks are generated.
* Scores are returned.

### Scenario 2 — Missing Target Architecture

Expected:

* Source inventory is produced where possible.
* Architecture comparison does not run.
* A critical validation or source gap is returned.

### Scenario 3 — No Endpoint Evidence

Expected:

* `SRC-002` is matched.
* Readiness score is reduced.
* Missing-information guidance is generated.

### Scenario 4 — AI Assistance Disabled

Expected:

* Complete deterministic analysis succeeds.
* No AI provider is called.
* Findings remain usable.

### Scenario 5 — Unsupported Artifact

Expected:

* Supported artifacts are still analyzed.
* Unsupported artifact is reported.
* Entire analysis does not fail.

---

## 36. Step 31 — Browser Validation Plan

The completed MVP should be tested manually in the browser.

Validation steps:

1. Open the application.
2. Enter a migration project name.
3. Select the Go cloud-native target architecture.
4. Load the sample legacy project.
5. Review all loaded source artifacts.
6. Start the analysis.
7. Verify each workflow stage appears.
8. Confirm analysis completes.
9. Review summary counts.
10. Verify endpoint findings.
11. Verify high and critical gaps.
12. Verify risks and mitigations.
13. Verify generated migration tasks.
14. Verify all scores remain between `0` and `100`.
15. Export the report.
16. Open the exported JSON.
17. Confirm the JSON matches the displayed analysis.
18. Reset the workflow.
19. Confirm all analysis state is cleared.
20. Run a second analysis without refreshing the page.

---

## 37. Step 32 — Error Validation Plan

The following errors must be tested:

* Empty project name
* Empty source application name
* No source artifacts
* Empty artifact content
* Unsupported artifact type
* Invalid target architecture ID
* Malformed API request
* Analysis service exception
* Export attempted without a completed result
* Oversized source input
* Duplicate artifact identifier

Errors should be displayed in user-readable language.

Internal exceptions should not be shown directly.

---

## 38. Suggested Work Breakdown

## Work Package 1 — Foundation

Implement:

* Migration types
* Target architecture definition
* Initial rule configuration
* Sample project fixture

Completion condition:

```text
The project compiles and all core domain types are available.
```

---

## Work Package 2 — Source Analysis

Implement:

* Input validation
* Source normalization
* Source inventory extraction
* Endpoint analysis
* Dependency analysis
* Data analysis
* Business-rule extraction

Completion condition:

```text
A sample legacy API can be converted into structured source findings.
```

---

## Work Package 3 — Migration Evaluation

Implement:

* Target comparison
* Rule evaluator
* Gap generation
* Risk generation
* Deduplication
* Recommendation generation
* Task generation
* Score calculation

Completion condition:

```text
Structured source findings produce deterministic migration outputs.
```

---

## Work Package 4 — Application Workflow

Implement:

* Migration analysis service
* API route
* Workflow hook
* Error handling
* Analysis-stage state

Completion condition:

```text
The complete migration analysis can be executed through one service boundary.
```

---

## Work Package 5 — User Interface

Implement:

* Project form
* Artifact editor
* Target selection
* Progress indicator
* Summary panel
* Findings panels
* Task panel
* Export panel

Completion condition:

```text
A user can complete the full analysis workflow from the browser.
```

---

## Work Package 6 — Validation

Implement:

* Unit tests
* Integration tests
* Browser validation
* Error validation
* JSON export verification

Completion condition:

```text
The MVP is stable, repeatable, and ready for demonstration.
```

---

## 39. MVP Scope Controls

The following features should remain outside the initial implementation:

* Direct GitHub or GitLab repository ingestion
* ZIP repository parsing
* Full AST parsing for multiple languages
* Automatic code refactoring
* Automatic pull-request generation
* Persistent project storage
* Multi-user authentication
* Team collaboration
* Migration execution tracking
* Vector databases
* Background job processing
* Live dependency calls
* Production AI provider integration
* Enterprise-scale repository indexing

These features should not delay the first working vertical slice.

---

## 40. Definition of Done

Phase 6 implementation planning is complete when the plan clearly defines:

* Required source files
* Implementation order
* Type creation
* Target architecture configuration
* Rule configuration
* Validation behavior
* Source normalization
* Inventory extraction
* Endpoint analysis
* Dependency analysis
* Data analysis
* Business-rule extraction
* Architecture comparison
* Rule evaluation
* Gap generation
* Risk generation
* Recommendation generation
* Migration-task generation
* Score calculation
* Analysis service
* API route
* React workflow hook
* User interface
* Export behavior
* Testing strategy
* Browser-validation steps
* MVP scope boundaries

---

## 41. Phase 6 Completion Checklist

* [ ] Core implementation sequence is documented.
* [ ] Required files and folders are identified.
* [ ] MVP source inputs are defined.
* [ ] Target architecture implementation is planned.
* [ ] Initial migration-rule subset is selected.
* [ ] Source-analysis modules are defined.
* [ ] Rule-evaluation modules are defined.
* [ ] Gap and risk generation are planned.
* [ ] Recommendation and task generation are planned.
* [ ] Score-calculation implementation is defined.
* [ ] Service and API boundaries are documented.
* [ ] Workflow-hook responsibilities are defined.
* [ ] UI components are listed.
* [ ] JSON export behavior is documented.
* [ ] Fixture-based testing is planned.
* [ ] Unit and integration tests are identified.
* [ ] Browser and error validation are documented.
* [ ] Out-of-scope features are clearly separated.
* [ ] Definition of done is established.

---

## 42. Phase 6 Outcome

The project now has an implementation-ready plan for converting the Day 41 architecture and migration rules into a functional MVP.

The recommended build path is:

```text
Build the Domain Model
        ↓
Normalize and Analyze Source Inputs
        ↓
Evaluate Deterministic Migration Rules
        ↓
Generate Gaps, Risks, Recommendations, and Tasks
        ↓
Calculate Migration Scores
        ↓
Expose One Analysis Service and API Route
        ↓
Build the Browser Workflow
        ↓
Validate with a Realistic Legacy API Fixture
```

The next development phase can begin with the shared TypeScript domain model and the first deterministic end-to-end migration-analysis slice.
