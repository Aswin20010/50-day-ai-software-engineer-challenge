# Day 41 — AI-Assisted API Migration Analyzer

## Phase 3: Technical Architecture

## 1. Purpose

This document defines the technical architecture for the **AI-Assisted API Migration Analyzer**.

The system analyzes a legacy API implementation and compares it with a target modernization architecture. It produces structured migration findings, compatibility risks, transformation recommendations, and implementation-ready migration tasks.

The architecture is designed to support the initial MVP while remaining extensible for future AI-assisted code analysis, dependency mapping, automated refactoring, and migration artifact generation.

---

## 2. Architecture Goals

The technical architecture must:

* Support analysis of legacy API source code and supporting documentation.
* Extract API endpoints, request models, response models, dependencies, business rules, and persistence behavior.
* Compare the existing implementation against a defined target architecture.
* Identify migration gaps, risks, incompatibilities, and missing information.
* Produce deterministic structured output before using AI-generated explanations.
* Keep analysis logic independent from the user interface.
* Support both rule-based and AI-assisted analysis.
* Preserve traceability between source evidence and generated findings.
* Allow new source languages and target platforms to be added later.
* Remain simple enough to implement within the remaining roadmap schedule.

---

## 3. High-Level Architecture

```text
┌──────────────────────────────────────────────┐
│                 User Interface               │
│                                              │
│  Project Setup                               │
│  Source Input                                │
│  Target Architecture Selection               │
│  Analysis Results                            │
│  Migration Report                            │
└──────────────────────┬───────────────────────┘
                       │
                       ▼
┌──────────────────────────────────────────────┐
│           Application Workflow Layer         │
│                                              │
│  Input Validation                            │
│  Analysis Orchestration                      │
│  State Management                            │
│  Error Handling                              │
│  Result Aggregation                          │
└──────────────────────┬───────────────────────┘
                       │
                       ▼
┌──────────────────────────────────────────────┐
│             Migration Analysis Engine        │
│                                              │
│  Source Inventory Extractor                  │
│  Endpoint Analyzer                           │
│  Dependency Analyzer                         │
│  Data Access Analyzer                        │
│  Business Rule Extractor                     │
│  Target Architecture Comparator              │
│  Gap and Risk Analyzer                       │
│  Recommendation Generator                    │
└──────────────────────┬───────────────────────┘
                       │
          ┌────────────┴────────────┐
          ▼                         ▼
┌──────────────────────┐  ┌──────────────────────┐
│ Rule-Based Analysis  │  │ AI-Assisted Analysis │
│                      │  │                      │
│ Static rules         │  │ Summarization        │
│ Pattern matching     │  │ Classification       │
│ Compatibility checks │  │ Recommendation text  │
│ Score calculation    │  │ Missing-info hints   │
└──────────┬───────────┘  └──────────┬───────────┘
           └────────────┬────────────┘
                        ▼
┌──────────────────────────────────────────────┐
│            Structured Analysis Model         │
│                                              │
│  Source System                               │
│  Endpoints                                   │
│  Dependencies                                │
│  Data Stores                                 │
│  Business Rules                              │
│  Migration Gaps                              │
│  Risks                                       │
│  Recommendations                             │
│  Migration Tasks                             │
└──────────────────────┬───────────────────────┘
                       │
                       ▼
┌──────────────────────────────────────────────┐
│              Output Generation               │
│                                              │
│  Migration Summary                           │
│  Endpoint Findings                           │
│  Risk Report                                 │
│  Gap Report                                  │
│  Migration Task Plan                         │
│  Structured JSON Export                      │
└──────────────────────────────────────────────┘
```

---

## 4. Recommended Technology Stack

The second project should reuse the proven foundation from Project 1 where practical.

### Frontend

* Next.js
* React
* TypeScript
* App Router
* Client-side workflow state for the MVP
* Reusable presentation components

### Analysis Layer

* TypeScript service modules
* Deterministic rule-based analysis
* Structured JSON models
* Optional LLM adapter isolated behind an interface

### Validation

* TypeScript type definitions
* Runtime validation helpers
* Explicit required-field validation
* Safe parsing of uploaded or pasted content

### Testing

* Unit tests for pure analysis functions
* Integration tests for the full analysis workflow
* Browser validation for major user flows
* Fixture-based tests using sample legacy APIs

### Storage

Persistent database storage is not required for the initial MVP.

The MVP may use:

* Browser memory for active analysis state
* JSON fixtures for sample projects
* Downloadable JSON reports
* Local application constants for target architecture definitions

A persistence layer can be introduced later without changing the core analysis engine.

---

## 5. Proposed Project Structure

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
│       ├── SourceInputPanel.tsx
│       ├── TargetArchitecturePanel.tsx
│       ├── AnalysisProgress.tsx
│       ├── MigrationSummaryPanel.tsx
│       ├── EndpointFindingsPanel.tsx
│       ├── RiskFindingsPanel.tsx
│       ├── GapFindingsPanel.tsx
│       └── MigrationTaskPanel.tsx
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
│       ├── sourceParser.ts
│       ├── sourceInventoryExtractor.ts
│       ├── endpointAnalyzer.ts
│       ├── dependencyAnalyzer.ts
│       ├── dataAccessAnalyzer.ts
│       ├── businessRuleAnalyzer.ts
│       ├── architectureComparator.ts
│       ├── gapAnalyzer.ts
│       ├── riskAnalyzer.ts
│       ├── recommendationGenerator.ts
│       ├── migrationTaskGenerator.ts
│       ├── migrationScoreCalculator.ts
│       ├── evidenceHelpers.ts
│       └── validation.ts
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
    ├── unit/
    └── integration/
```

---

## 6. Core Architecture Layers

## 6.1 Presentation Layer

The presentation layer allows the user to:

* Define a migration project.
* Provide legacy source material.
* Select or configure a target architecture.
* Start the analysis.
* Review migration findings.
* Export the generated report.

The presentation layer must not contain migration analysis rules.

Its responsibilities are limited to:

* User input collection
* Validation feedback
* Loading and error states
* Analysis result rendering
* User interaction
* Report download actions

---

## 6.2 Workflow Orchestration Layer

The workflow orchestration layer coordinates the complete migration analysis lifecycle.

Recommended hook:

```text
useMigrationAnalysis
```

The hook should manage:

* Current project input
* Selected target architecture
* Analysis status
* Validation errors
* Raw analysis output
* Migration summary
* Risk findings
* Gap findings
* Generated migration tasks
* Workflow reset behavior

Suggested workflow states:

```text
IDLE
VALIDATING
PARSING_SOURCE
ANALYZING
COMPARING
GENERATING_REPORT
COMPLETED
FAILED
```

This layer calls the analysis service and transforms service responses into UI-ready state.

---

## 6.3 Migration Analysis Service

The migration analysis service is the main entry point into the analysis engine.

Suggested responsibility:

```typescript
analyzeMigrationProject(
  request: MigrationAnalysisRequest
): Promise<MigrationAnalysisResult>
```

The service should perform the following sequence:

1. Validate the input.
2. Parse the supplied legacy source.
3. Build the source-system inventory.
4. Analyze API endpoints.
5. Analyze external and internal dependencies.
6. Analyze persistence and data-access behavior.
7. extract identifiable business rules.
8. Compare findings against the selected target architecture.
9. Generate gaps and risks.
10. Calculate migration readiness and complexity scores.
11. Generate recommendations.
12. Generate migration tasks.
13. Return one structured result.

The service must return structured data even when AI-assisted enrichment is unavailable.

---

## 7. Analysis Engine Components

## 7.1 Source Parser

The source parser converts raw input into normalized analysis units.

Supported MVP inputs may include:

* Pasted source code
* Pasted API documentation
* JSON API contracts
* OpenAPI fragments
* Repository structure descriptions
* Configuration snippets
* Database schema descriptions

Normalized source units should contain:

```typescript
interface SourceArtifact {
  id: string;
  name: string;
  type: SourceArtifactType;
  language?: string;
  content: string;
  metadata?: Record<string, unknown>;
}
```

The parser should not attempt to complete missing information automatically. Missing information must be captured explicitly.

---

## 7.2 Source Inventory Extractor

The source inventory extractor identifies major artifacts in the legacy system.

It should detect:

* Application name
* Language
* Framework
* API endpoints
* Request and response models
* Service classes or modules
* Repository or DAO components
* External clients
* Database dependencies
* Configuration values
* Authentication mechanisms
* Logging behavior
* Error-handling patterns
* Test assets
* Deployment files

The output becomes the evidence base for later analysis.

---

## 7.3 Endpoint Analyzer

The endpoint analyzer examines each detected API endpoint.

For every endpoint, it should identify:

* HTTP method
* Route
* Handler or controller
* Request headers
* Path parameters
* Query parameters
* Request body
* Validation behavior
* Service calls
* Repository calls
* External calls
* Response models
* Error responses
* Authentication requirements
* Business rules
* Transaction boundaries
* Observability behavior

Each endpoint finding must include evidence indicating where the information came from.

---

## 7.4 Dependency Analyzer

The dependency analyzer identifies dependencies that may affect migration.

Dependencies can include:

* Internal modules
* Shared libraries
* External APIs
* Databases
* Message brokers
* File systems
* Authentication providers
* Secrets or certificate stores
* Cloud services
* Runtime-specific packages
* Legacy frameworks
* Operating-system dependencies

Each dependency should receive a migration classification:

```text
REUSE
REPLACE
REFACTOR
REMOVE
UNKNOWN
```

---

## 7.5 Data Access Analyzer

The data access analyzer evaluates persistence-related behavior.

It should identify:

* Database technology
* Tables or collections
* Queries
* Stored procedures
* Transactions
* Locks
* Connection patterns
* Data mappings
* Schema assumptions
* Audit behavior
* Read and write paths

Migration findings should highlight:

* Unsupported database features
* Vendor-specific SQL
* Transaction-model differences
* Data-type compatibility issues
* Concurrency risks
* Missing schema evidence
* Data migration requirements

---

## 7.6 Business Rule Analyzer

The business rule analyzer converts implementation logic into structured rules.

Example rule structure:

```typescript
interface MigrationBusinessRule {
  id: string;
  title: string;
  description: string;
  sourceArtifactId: string;
  sourceReference?: string;
  category: string;
  confidence: number;
}
```

Business rules may be extracted from:

* Conditional statements
* Validation methods
* Status transitions
* Database update logic
* Error mappings
* Configuration branches
* Comments
* Documentation
* Test expectations

Rules should remain traceable to their original source.

---

## 7.7 Target Architecture Comparator

The comparator evaluates the legacy implementation against a target architecture definition.

A target architecture may specify:

* Target language
* Target framework
* API design standards
* Required project layers
* Authentication approach
* Logging requirements
* Error-response standard
* Configuration strategy
* Database technology
* Testing standards
* Deployment environment
* Observability requirements
* Security controls

Example comparison:

```text
Legacy:
Handler directly executes SQL.

Target:
Handler → Service → Repository separation required.

Finding:
Architecture gap requiring repository extraction and service-layer introduction.
```

The comparator must generate deterministic findings from configured rules.

---

## 7.8 Gap Analyzer

The gap analyzer identifies differences between the source system and the target architecture.

Gap categories should include:

```text
ARCHITECTURE
API_CONTRACT
SECURITY
DATA
DEPENDENCY
BUSINESS_LOGIC
ERROR_HANDLING
OBSERVABILITY
TESTING
CONFIGURATION
DEPLOYMENT
DOCUMENTATION
```

Each gap should include:

* Unique identifier
* Category
* Title
* Description
* Source evidence
* Expected target behavior
* Severity
* Migration impact
* Recommended action
* Confidence score

Severity levels:

```text
LOW
MEDIUM
HIGH
CRITICAL
```

---

## 7.9 Risk Analyzer

The risk analyzer determines risks that may affect migration delivery or runtime behavior.

Example risks:

* Business logic is embedded directly in controllers.
* Database transactions cannot be reproduced in the target datastore.
* External service contracts are undocumented.
* Authentication behavior is coupled to the legacy platform.
* Error mappings are inconsistent.
* Tests do not cover critical status transitions.
* Required configuration is hard-coded.
* Multiple endpoints depend on shared mutable state.
* Source evidence is incomplete.

Each risk should include:

* Likelihood
* Impact
* Severity
* Evidence
* Mitigation
* Affected endpoints
* Affected dependencies

---

## 7.10 Recommendation Generator

The recommendation generator converts findings into actionable technical guidance.

Recommendations should be:

* Specific
* Evidence-based
* Linked to one or more gaps
* Compatible with the selected target architecture
* Suitable for engineering implementation

Example:

```text
Introduce a repository interface for account persistence and move SQL execution
out of the HTTP handler. Add transaction-aware repository methods for the commit
and rollback workflows.
```

AI may improve the wording of recommendations, but the underlying recommendation category must originate from deterministic analysis rules.

---

## 7.11 Migration Task Generator

The migration task generator converts findings into implementation-ready work items.

Each task should include:

* Task ID
* Title
* Description
* Related endpoint or component
* Source finding IDs
* Priority
* Estimated complexity
* Dependencies
* Acceptance criteria
* Suggested implementation order

Suggested priority values:

```text
P0
P1
P2
P3
```

Suggested complexity values:

```text
SMALL
MEDIUM
LARGE
EXTRA_LARGE
```

---

## 8. Rule-Based and AI-Assisted Design

The system should use a hybrid architecture.

## 8.1 Rule-Based Responsibilities

Rule-based logic should handle:

* Required-field validation
* Input normalization
* Endpoint detection patterns
* Dependency classification
* Architecture comparisons
* Severity assignment
* Score calculation
* Missing-information detection
* Migration task templates

These operations must remain repeatable and testable.

## 8.2 AI-Assisted Responsibilities

AI may assist with:

* Summarizing legacy implementation behavior
* Converting code patterns into readable business rules
* Classifying unclear dependencies
* Explaining complex migration risks
* Generating recommendation descriptions
* Suggesting missing investigation questions
* Producing executive report summaries

AI output must not silently overwrite deterministic findings.

The final result should distinguish between:

```text
DETERMINISTIC
AI_ASSISTED
USER_PROVIDED
```

---

## 9. Evidence and Traceability

Every major finding must reference source evidence.

Suggested evidence model:

```typescript
interface AnalysisEvidence {
  artifactId: string;
  artifactName: string;
  referenceType: "LINE" | "SECTION" | "SYMBOL" | "FILE" | "USER_INPUT";
  reference?: string;
  excerpt?: string;
}
```

Traceability is required for:

* Endpoint definitions
* Business rules
* Dependencies
* Database behavior
* Architecture gaps
* Migration risks
* Recommendations
* Migration tasks

A recommendation without evidence should be marked as low confidence.

---

## 10. Scoring Architecture

The MVP should calculate the following scores:

### Migration Readiness Score

Indicates how prepared the source system is for migration.

Factors may include:

* Documentation completeness
* Test coverage evidence
* Layer separation
* Contract clarity
* Dependency clarity
* Configuration maturity
* Observability maturity

### Migration Complexity Score

Indicates expected migration difficulty.

Factors may include:

* Number of endpoints
* Number of dependencies
* Legacy framework coupling
* Database complexity
* Transaction complexity
* Business-rule density
* Security integration complexity
* Number of critical gaps

### Risk Score

Indicates the overall delivery and runtime risk.

Factors may include:

* Critical findings
* Unknown dependencies
* Missing documentation
* Unsupported technology
* Incomplete test evidence
* Data-consistency risks

Suggested score range:

```text
0–100
```

All score calculations must be implemented as pure functions with documented weights.

---

## 11. API Boundary

The UI should communicate with the analysis engine through one clear service boundary.

Suggested endpoint:

```http
POST /api/migration-analysis
```

Example request:

```json
{
  "projectName": "Legacy Account Discount API Migration",
  "sourceSystem": {
    "applicationName": "ads-accountdiscount-api",
    "language": "Go",
    "framework": "net/http",
    "artifacts": []
  },
  "targetArchitectureId": "go-cloud-native-api",
  "analysisOptions": {
    "includeAiAssistance": false,
    "generateMigrationTasks": true,
    "calculateScores": true
  }
}
```

Example response shape:

```json
{
  "analysisId": "analysis-001",
  "status": "COMPLETED",
  "summary": {},
  "sourceInventory": {},
  "endpointFindings": [],
  "dependencyFindings": [],
  "dataFindings": [],
  "businessRules": [],
  "gaps": [],
  "risks": [],
  "recommendations": [],
  "migrationTasks": [],
  "scores": {
    "readiness": 0,
    "complexity": 0,
    "risk": 0
  },
  "missingInformation": [],
  "metadata": {}
}
```

---

## 12. Error Handling Architecture

The application should distinguish between:

* Input validation errors
* Unsupported input errors
* Source parsing errors
* Analysis failures
* AI provider failures
* Report generation failures
* Unexpected application errors

Suggested normalized error structure:

```typescript
interface MigrationAnalysisError {
  code: string;
  message: string;
  field?: string;
  recoverable: boolean;
  details?: Record<string, unknown>;
}
```

AI provider failure must not fail the entire analysis when deterministic analysis has completed successfully.

---

## 13. Security and Privacy Considerations

The application may receive proprietary source code and architecture documentation.

The architecture must therefore:

* Avoid logging full source content.
* Avoid exposing secrets in generated reports.
* Detect common secret patterns where practical.
* Keep AI integration optional.
* Clearly indicate when source content may be sent to an external model.
* Separate source content from application telemetry.
* Avoid persisting uploaded source material in the MVP.
* Sanitize filenames and user-provided metadata.
* Apply reasonable input-size limits.

The application should never intentionally reproduce detected credentials, tokens, private keys, or passwords in its output.

---

## 14. Performance Considerations

The MVP should optimize for small and medium migration assessments rather than complete enterprise repositories.

Recommended controls:

* Limit individual artifact size.
* Limit total project input size.
* Process artifacts independently where possible.
* Use pure analysis functions.
* Avoid repeated parsing of unchanged content.
* Compute summary counts from normalized analysis results.
* Keep expensive AI enrichment optional.
* Display progress by analysis stage.

Large-repository ingestion can be added in a future phase.

---

## 15. Testing Architecture

## 15.1 Unit Tests

Unit tests should cover:

* Source normalization
* Endpoint extraction
* Dependency classification
* Architecture comparison rules
* Gap severity calculation
* Risk scoring
* Migration readiness scoring
* Task generation
* Missing-information detection
* Evidence mapping

## 15.2 Integration Tests

Integration tests should validate:

* Complete request-to-result flow
* Multiple endpoint analysis
* Unsupported source input handling
* Target architecture comparison
* Report generation
* AI-disabled fallback behavior

## 15.3 Fixture Projects

At least one complete migration fixture should be included.

Recommended initial fixture:

```text
Legacy Account Discount API
```

The fixture should include:

* Endpoint definitions
* Service logic
* Repository behavior
* Database references
* External service calls
* Business rules
* Error mappings
* Target architecture definition

---

## 16. Extensibility

The architecture should allow future support for:

* Git repository ingestion
* ZIP-file analysis
* AST-based source parsing
* Multiple programming languages
* OpenAPI import
* Database schema import
* Dependency graph visualization
* Automated code transformation
* Target project scaffold generation
* Migration pull-request generation
* Human review and approval
* Migration progress tracking
* Report history
* Multi-project comparison
* IDE and CI/CD integration

These capabilities are outside the Day 41 MVP scope but must not require replacement of the core structured analysis model.

---

## 17. Technical Decisions

| Decision              | Selected Approach                    | Reason                                                         |
| --------------------- | ------------------------------------ | -------------------------------------------------------------- |
| Application framework | Next.js with TypeScript              | Reuses Project 1 experience and supports UI plus server routes |
| Analysis approach     | Hybrid deterministic and AI-assisted | Maintains reliability while enabling richer explanations       |
| MVP persistence       | No database required                 | Reduces implementation scope and privacy risk                  |
| Core output           | Structured JSON                      | Supports UI rendering, testing, export, and future automation  |
| Source traceability   | Evidence attached to findings        | Improves trust and reviewability                               |
| AI dependency         | Optional adapter                     | Prevents AI availability from blocking core analysis           |
| Scoring               | Pure deterministic functions         | Enables repeatable and testable results                        |
| Target architecture   | Configuration-driven definitions     | Allows future target platforms without rewriting the engine    |
| Report generation     | Derived from structured results      | Prevents inconsistencies between UI and downloaded reports     |

---

## 18. Day 41 Phase 3 Completion Criteria

Phase 3 is complete when:

* The high-level system architecture is defined.
* Presentation, workflow, service, analysis, and output layers are separated.
* Core analyzer responsibilities are documented.
* Rule-based and AI-assisted responsibilities are clearly separated.
* Evidence and traceability requirements are defined.
* Error-handling and security expectations are documented.
* A recommended source-code structure is provided.
* The API boundary and result flow are documented.
* Testing and future extensibility are covered.
* The architecture is ready to support the Phase 4 data-model implementation.

---

## 19. Phase 3 Outcome

The **AI-Assisted API Migration Analyzer** now has a clear technical foundation.

The implementation should follow this primary flow:

```text
Source Inputs
    ↓
Validation and Normalization
    ↓
Source Inventory Extraction
    ↓
Endpoint, Dependency, Data, and Business Rule Analysis
    ↓
Target Architecture Comparison
    ↓
Gap and Risk Detection
    ↓
Recommendation and Migration Task Generation
    ↓
Structured Migration Analysis Result
    ↓
UI Presentation and Report Export
```

This architecture provides a practical MVP design while establishing a foundation for a future enterprise-grade software modernization platform.
