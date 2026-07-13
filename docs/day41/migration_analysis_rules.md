# Day 41 — AI-Assisted API Migration Analyzer

## Phase 5: Migration Analysis Rules

## 1. Purpose

This document defines the deterministic rules used by the **AI-Assisted API Migration Analyzer** to evaluate a legacy API against a selected target architecture.

The rules convert normalized source-system information into:

* Architecture gaps
* Migration risks
* Compatibility findings
* Missing-information findings
* Recommendations
* Migration tasks
* Readiness, complexity, and risk scores

The deterministic rule engine must produce useful results even when AI assistance is disabled or unavailable.

---

## 2. Rule Engine Objectives

The rule engine must:

* Produce repeatable results for the same input.
* Keep findings traceable to source evidence.
* Separate factual findings from generated explanations.
* Avoid inventing missing implementation details.
* Support configurable target architectures.
* Assign consistent severity, priority, and confidence values.
* Allow additional rules to be added without changing the analysis workflow.
* Convert findings into actionable migration work items.

---

## 3. Rule Processing Flow

```text
Normalized Source System
        ↓
Source Completeness Validation
        ↓
Endpoint Analysis Rules
        ↓
Architecture Comparison Rules
        ↓
Dependency and Data Rules
        ↓
Security and Observability Rules
        ↓
Testing and Deployment Rules
        ↓
Gap and Risk Generation
        ↓
Recommendation Generation
        ↓
Migration Task Generation
        ↓
Score Calculation
```

Each rule evaluates one specific condition and returns either:

```text
MATCHED
NOT_MATCHED
NOT_APPLICABLE
INSUFFICIENT_EVIDENCE
```

---

## 4. Rule Definition Model

Each migration rule should follow a consistent structure.

```typescript
interface MigrationRuleDefinition {
  id: string;
  name: string;
  description: string;
  category: MigrationGapCategory;
  severity: MigrationSeverity;
  priority: MigrationPriority;
  confidence: number;
  condition: MigrationRuleCondition;
  recommendationTemplate: string;
  taskTemplate?: MigrationTaskTemplate;
  enabled: boolean;
}
```

Suggested condition model:

```typescript
interface MigrationRuleCondition {
  field: string;
  operator: MigrationRuleOperator;
  expectedValue?: unknown;
}
```

Supported operators may include:

```typescript
type MigrationRuleOperator =
  | "EXISTS"
  | "NOT_EXISTS"
  | "EQUALS"
  | "NOT_EQUALS"
  | "CONTAINS"
  | "NOT_CONTAINS"
  | "GREATER_THAN"
  | "LESS_THAN"
  | "EMPTY"
  | "NOT_EMPTY"
  | "MATCHES_PATTERN";
```

---

## 5. Rule Result Model

When a rule matches, it should produce a structured result.

```typescript
interface MigrationRuleResult {
  ruleId: string;
  status:
    | "MATCHED"
    | "NOT_MATCHED"
    | "NOT_APPLICABLE"
    | "INSUFFICIENT_EVIDENCE";
  findingId?: string;
  evidence: AnalysisEvidence[];
  confidence: number;
  details?: Record<string, unknown>;
}
```

A matched rule may generate:

* One migration gap
* One or more risks
* One recommendation
* One or more migration tasks

---

## 6. Source Completeness Rules

## 6.1 Missing Application Metadata

### Rule ID

```text
SRC-001
```

### Condition

The source system does not provide one or more of:

* Application name
* Primary language
* Framework
* Source artifacts

### Result

Create a missing-information finding.

### Severity

```text
MEDIUM
```

### Recommendation

Provide the missing application metadata before finalizing migration scope and estimates.

---

## 6.2 No Endpoint Evidence

### Rule ID

```text
SRC-002
```

### Condition

No endpoint definitions are detected.

### Result

Create a high-severity analysis gap because the system cannot verify API behavior.

### Severity

```text
HIGH
```

### Risk

The migration plan may omit production API workflows.

### Recommendation

Provide route definitions, controller files, handlers, OpenAPI contracts, or API documentation.

---

## 6.3 Missing Target Architecture

### Rule ID

```text
SRC-003
```

### Condition

No target architecture definition is selected.

### Severity

```text
CRITICAL
```

### Result

Stop architecture comparison while allowing source inventory extraction to complete.

### Recommendation

Select or define a target architecture before generating migration gaps and recommendations.

---

## 6.4 Unsupported Artifact Type

### Rule ID

```text
SRC-004
```

### Condition

The source input contains an artifact type that the parser cannot analyze.

### Severity

```text
LOW
```

### Result

Mark the artifact as unsupported without failing the complete analysis.

### Recommendation

Convert the artifact into source code, text, JSON, OpenAPI, schema, or supported documentation format.

---

## 7. Endpoint Architecture Rules

## 7.1 Handler Contains Business Logic

### Rule ID

```text
API-001
```

### Condition

The endpoint handler performs one or more of:

* Business calculations
* Status transitions
* Database updates
* Complex conditional workflows
* Direct downstream orchestration

### Category

```text
ARCHITECTURE
```

### Severity

```text
HIGH
```

### Finding

Business logic is coupled to the transport layer.

### Risk

* Reduced testability
* Difficult code reuse
* Increased migration complexity
* Higher regression risk

### Recommendation

Move business logic into a dedicated service layer and keep handlers responsible for transport concerns only.

### Suggested Task

```text
Extract endpoint business logic into a service component.
```

---

## 7.2 Handler Accesses Database Directly

### Rule ID

```text
API-002
```

### Condition

An HTTP handler directly executes SQL or calls a database client.

### Severity

```text
HIGH
```

### Finding

The API does not maintain handler, service, and repository separation.

### Recommendation

Introduce a repository interface and move persistence logic out of the handler.

---

## 7.3 Missing Service Layer

### Rule ID

```text
API-003
```

### Condition

The target architecture requires a service layer, but no service component is identified.

### Severity

```text
HIGH
```

### Recommendation

Create endpoint-oriented service methods that implement business workflows independently of HTTP handling.

---

## 7.4 Missing Repository Layer

### Rule ID

```text
API-004
```

### Condition

The target architecture requires a repository layer, but persistence is performed elsewhere.

### Severity

```text
HIGH
```

### Recommendation

Introduce repository interfaces and implementation files for database operations.

---

## 7.5 Inconsistent Request Validation

### Rule ID

```text
API-005
```

### Condition

Required fields are validated inconsistently across endpoints.

### Severity

```text
MEDIUM
```

### Risk

Invalid requests may reach the business or persistence layers.

### Recommendation

Centralize request validation and return a consistent validation-error structure.

---

## 7.6 Missing Request Validation

### Rule ID

```text
API-006
```

### Condition

An endpoint accepts a request body but no field validation is detected.

### Severity

```text
HIGH
```

### Recommendation

Add explicit validation for required headers, path parameters, query parameters, and request-body fields.

---

## 7.7 Inconsistent Response Contract

### Rule ID

```text
API-007
```

### Condition

Endpoints return inconsistent success or error response structures.

### Severity

```text
MEDIUM
```

### Recommendation

Adopt a shared response and error contract aligned with the target API standards.

---

## 7.8 Missing HTTP Status Mapping

### Rule ID

```text
API-008
```

### Condition

Business or downstream errors are returned without explicit HTTP status mapping.

### Severity

```text
MEDIUM
```

### Recommendation

Create a centralized error mapper that converts domain errors into defined HTTP status codes and response bodies.

---

## 8. API Contract Rules

## 8.1 Missing API Contract

### Rule ID

```text
CONTRACT-001
```

### Condition

No OpenAPI specification, request model, response model, or equivalent documentation is available.

### Severity

```text
HIGH
```

### Risk

The migrated API may unintentionally change client-visible behavior.

### Recommendation

Create a version-controlled API contract before implementation begins.

---

## 8.2 Legacy Contract Incompatible with Target Standard

### Rule ID

```text
CONTRACT-002
```

### Condition

The legacy API contract violates one or more target standards.

Examples:

* Incorrect route naming
* Unsupported media type
* Nonstandard error body
* Missing API version
* Unsupported field types
* Inconsistent field naming

### Severity

```text
MEDIUM
```

### Recommendation

Define a compatibility strategy before changing the public contract.

---

## 8.3 Required Field Not Enforced

### Rule ID

```text
CONTRACT-003
```

### Condition

A field is documented as required but is not validated in implementation.

### Severity

```text
HIGH
```

### Recommendation

Align runtime validation with the published API contract.

---

## 8.4 Implementation Requires Undocumented Field

### Rule ID

```text
CONTRACT-004
```

### Condition

The implementation depends on a field not documented in the API contract.

### Severity

```text
HIGH
```

### Risk

Clients may fail after migration because hidden assumptions remain undocumented.

### Recommendation

Document the field or remove the hidden dependency.

---

## 9. Dependency Rules

## 9.1 Undocumented External Dependency

### Rule ID

```text
DEP-001
```

### Condition

The source calls an external system not included in the dependency inventory.

### Severity

```text
HIGH
```

### Recommendation

Document the external contract, ownership, authentication, timeout, retry, and failure behavior.

---

## 9.2 Hard-Coded Dependency URL

### Rule ID

```text
DEP-002
```

### Condition

An external URL, host, port, or environment-specific path is hard-coded.

### Severity

```text
HIGH
```

### Recommendation

Move dependency configuration into environment-aware configuration and secret-management mechanisms.

---

## 9.3 Missing Timeout

### Rule ID

```text
DEP-003
```

### Condition

An outbound network call does not define a timeout.

### Severity

```text
HIGH
```

### Risk

Requests may hang and exhaust application resources.

### Recommendation

Add target-appropriate connection, request, and response timeouts.

---

## 9.4 Missing Retry Policy

### Rule ID

```text
DEP-004
```

### Condition

A retry-safe external operation has no retry strategy.

### Severity

```text
MEDIUM
```

### Recommendation

Add bounded retries with backoff only for idempotent or explicitly retry-safe operations.

---

## 9.5 Unsafe Retry Behavior

### Rule ID

```text
DEP-005
```

### Condition

A non-idempotent operation is retried without an idempotency strategy.

### Severity

```text
CRITICAL
```

### Risk

Duplicate financial, transactional, or state-changing operations may occur.

### Recommendation

Introduce idempotency keys, duplicate detection, or disable unsafe retries.

---

## 9.6 Unsupported Legacy Library

### Rule ID

```text
DEP-006
```

### Condition

A dependency is unavailable, deprecated, or unsupported in the target platform.

### Severity

```text
HIGH
```

### Recommendation

Replace the dependency with an approved target-platform alternative.

---

## 10. Data and Persistence Rules

## 10.1 Vendor-Specific SQL

### Rule ID

```text
DATA-001
```

### Condition

Queries use source-database-specific functions, syntax, data types, hints, or locking behavior.

### Severity

```text
HIGH
```

### Recommendation

Translate and validate queries against the target database before implementation completion.

---

## 10.2 Transaction Boundary Not Defined

### Rule ID

```text
DATA-002
```

### Condition

Multiple related write operations occur without an identifiable transaction boundary.

### Severity

```text
CRITICAL
```

### Risk

Partial updates may leave the system in an inconsistent state.

### Recommendation

Define an atomic transaction covering all required reads, validations, and writes.

---

## 10.3 Concurrency Control Missing

### Rule ID

```text
DATA-003
```

### Condition

A read-modify-write workflow exists without locking, version checking, or transactional protection.

### Severity

```text
CRITICAL
```

### Recommendation

Introduce row locking, optimistic concurrency, transactional mutations, or equivalent target-database controls.

---

## 10.4 Direct Data Model Coupling

### Rule ID

```text
DATA-004
```

### Condition

API request or response models are reused directly as persistence entities.

### Severity

```text
MEDIUM
```

### Recommendation

Separate transport models from persistence models and define explicit mapping functions.

---

## 10.5 Missing Audit Behavior

### Rule ID

```text
DATA-005
```

### Condition

The target architecture requires audit records, but state-changing operations do not create them.

### Severity

```text
HIGH
```

### Recommendation

Add audit persistence containing operation, actor, request identifier, status, timestamps, and relevant state changes.

---

## 10.6 Non-Atomic Audit Write

### Rule ID

```text
DATA-006
```

### Condition

The business update and its audit record are written in separate, non-atomic operations.

### Severity

```text
HIGH
```

### Recommendation

Write business and audit changes within the same transaction where supported.

---

## 10.7 Data-Type Compatibility Risk

### Rule ID

```text
DATA-007
```

### Condition

A source data type does not map safely to the target datastore.

Examples:

* Decimal values mapped to floating-point types
* Source timestamps without timezone semantics
* Unsupported large-object types
* Different maximum string lengths

### Severity

```text
HIGH
```

### Recommendation

Define and test an explicit data-type mapping before migration.

---

## 11. Business Logic Rules

## 11.1 Undocumented Status Transition

### Rule ID

```text
BUS-001
```

### Condition

The implementation changes a status value without documentation or named constants.

### Severity

```text
HIGH
```

### Recommendation

Document the state transition and replace raw values with typed constants or enums.

---

## 11.2 Magic Value Used in Business Rule

### Rule ID

```text
BUS-002
```

### Condition

A numeric or string literal controls business behavior without a meaningful constant.

### Severity

```text
MEDIUM
```

### Recommendation

Replace the literal with a domain constant and document its business meaning.

---

## 11.3 Duplicate Business Rule

### Rule ID

```text
BUS-003
```

### Condition

Equivalent business logic is repeated across multiple handlers or services.

### Severity

```text
MEDIUM
```

### Recommendation

Extract the shared rule into a reusable domain or service helper.

---

## 11.4 Business Rule Has No Evidence

### Rule ID

```text
BUS-004
```

### Condition

A rule is inferred but cannot be linked to source code, documentation, configuration, or tests.

### Severity

```text
LOW
```

### Result

Mark the rule as low confidence.

### Recommendation

Confirm the rule with a system owner before implementation.

---

## 11.5 Financial Calculation Uses Floating Point

### Rule ID

```text
BUS-005
```

### Condition

Currency calculations use binary floating-point values.

### Severity

```text
CRITICAL
```

### Risk

Rounding errors may affect balances, settlements, or reconciliation.

### Recommendation

Use decimal or integer minor-unit arithmetic according to target standards.

---

## 12. Security Rules

## 12.1 Hard-Coded Credential

### Rule ID

```text
SEC-001
```

### Condition

A password, API key, token, private key, or secret is detected in source content.

### Severity

```text
CRITICAL
```

### Recommendation

Remove the credential, rotate it, and use the approved target secret-management solution.

### Output Restriction

The analyzer must not reproduce the detected credential in its report.

---

## 12.2 Missing Authentication Evidence

### Rule ID

```text
SEC-002
```

### Condition

The target architecture requires authentication, but no authentication logic is detected.

### Severity

```text
CRITICAL
```

### Recommendation

Define and implement the required authentication mechanism before deployment.

---

## 12.3 Missing Authorization Check

### Rule ID

```text
SEC-003
```

### Condition

An authenticated endpoint performs protected operations without authorization validation.

### Severity

```text
CRITICAL
```

### Recommendation

Add role, scope, resource-owner, or policy-based authorization checks.

---

## 12.4 Sensitive Data Logged

### Rule ID

```text
SEC-004
```

### Condition

Logs include credentials, full tokens, payment data, personally identifiable information, or private payloads.

### Severity

```text
CRITICAL
```

### Recommendation

Redact or exclude sensitive fields and use approved structured logging practices.

---

## 12.5 Missing Input Sanitization

### Rule ID

```text
SEC-005
```

### Condition

User-controlled values are passed into SQL, paths, templates, or commands without safe parameterization.

### Severity

```text
CRITICAL
```

### Recommendation

Use parameterized operations and target-platform validation libraries.

---

## 13. Configuration Rules

## 13.1 Environment-Specific Value in Source

### Rule ID

```text
CFG-001
```

### Condition

Environment-specific configuration is embedded in application code.

### Severity

```text
HIGH
```

### Recommendation

Move the value to validated external configuration.

---

## 13.2 Missing Configuration Validation

### Rule ID

```text
CFG-002
```

### Condition

Required configuration values are used without startup validation.

### Severity

```text
MEDIUM
```

### Recommendation

Validate required configuration during startup and fail with a clear error when invalid.

---

## 13.3 Configuration Default Is Unsafe

### Rule ID

```text
CFG-003
```

### Condition

A missing configuration value silently falls back to an unsafe production default.

### Severity

```text
HIGH
```

### Recommendation

Use secure defaults or require explicit configuration.

---

## 14. Error-Handling Rules

## 14.1 Raw Internal Error Returned to Client

### Rule ID

```text
ERR-001
```

### Condition

Internal stack traces, SQL errors, file paths, or downstream response details are returned to the client.

### Severity

```text
HIGH
```

### Recommendation

Return a safe public error while preserving detailed diagnostics in protected logs.

---

## 14.2 Error Is Ignored

### Rule ID

```text
ERR-002
```

### Condition

An operation returns an error that is discarded or overwritten.

### Severity

```text
HIGH
```

### Recommendation

Handle the error explicitly and preserve failure context.

---

## 14.3 Inconsistent Domain Error Mapping

### Rule ID

```text
ERR-003
```

### Condition

Equivalent business failures return different response codes or messages across endpoints.

### Severity

```text
MEDIUM
```

### Recommendation

Define shared domain errors and centralized transport mapping.

---

## 14.4 Downstream Error Passed Through Directly

### Rule ID

```text
ERR-004
```

### Condition

A downstream service response is exposed directly without normalization.

### Severity

```text
MEDIUM
```

### Recommendation

Map downstream failures to the target API error contract.

---

## 15. Observability Rules

## 15.1 Missing Correlation Identifier

### Rule ID

```text
OBS-001
```

### Condition

Requests and downstream calls cannot be linked using a correlation or trace identifier.

### Severity

```text
HIGH
```

### Recommendation

Propagate a correlation or trace identifier through handlers, services, repositories, and external clients.

---

## 15.2 Unstructured Logging

### Rule ID

```text
OBS-002
```

### Condition

Logs rely primarily on plain text without structured fields.

### Severity

```text
MEDIUM
```

### Recommendation

Adopt structured logs containing operation, endpoint, request identifier, status, duration, and failure category.

---

## 15.3 Missing Operation Metrics

### Rule ID

```text
OBS-003
```

### Condition

No evidence exists for request count, error count, latency, or dependency metrics.

### Severity

```text
MEDIUM
```

### Recommendation

Add service-level and dependency-level metrics aligned with the target observability standard.

---

## 15.4 Missing Health Endpoint

### Rule ID

```text
OBS-004
```

### Condition

The target platform requires health checks but no health endpoint is detected.

### Severity

```text
MEDIUM
```

### Recommendation

Add liveness and readiness checks appropriate to the deployment platform.

---

## 16. Testing Rules

## 16.1 No Automated Test Evidence

### Rule ID

```text
TEST-001
```

### Condition

No unit, integration, contract, or end-to-end tests are detected.

### Severity

```text
HIGH
```

### Recommendation

Create a regression test baseline before changing implementation behavior.

---

## 16.2 Critical Business Rule Not Tested

### Rule ID

```text
TEST-002
```

### Condition

A high-severity business rule has no linked test evidence.

### Severity

```text
HIGH
```

### Recommendation

Add focused tests covering success, rejection, boundary, and failure scenarios.

---

## 16.3 External Contract Not Tested

### Rule ID

```text
TEST-003
```

### Condition

An external dependency exists without contract or integration test evidence.

### Severity

```text
HIGH
```

### Recommendation

Add contract tests using approved mocks, fixtures, or controlled integration environments.

---

## 16.4 Error Paths Not Tested

### Rule ID

```text
TEST-004
```

### Condition

Tests cover successful processing but not validation, downstream, database, or business failures.

### Severity

```text
MEDIUM
```

### Recommendation

Add negative-path tests for each major failure category.

---

## 16.5 Migration Compatibility Test Missing

### Rule ID

```text
TEST-005
```

### Condition

No test compares legacy and target responses for equivalent requests.

### Severity

```text
HIGH
```

### Recommendation

Create compatibility tests that execute identical fixtures against legacy and migrated implementations.

---

## 17. Deployment Rules

## 17.1 Missing Deployment Definition

### Rule ID

```text
DEPLOY-001
```

### Condition

No container, deployment manifest, infrastructure configuration, or runtime instructions are available.

### Severity

```text
MEDIUM
```

### Recommendation

Define the target deployment model and required runtime configuration.

---

## 17.2 Application State Stored Locally

### Rule ID

```text
DEPLOY-002
```

### Condition

The application stores required runtime state in local memory or local files, but the target platform is horizontally scalable.

### Severity

```text
HIGH
```

### Recommendation

Move shared runtime state to an approved external datastore or remove the state dependency.

---

## 17.3 Graceful Shutdown Missing

### Rule ID

```text
DEPLOY-003
```

### Condition

No shutdown handling is detected for active requests or external resources.

### Severity

```text
MEDIUM
```

### Recommendation

Implement graceful shutdown with request draining and resource cleanup.

---

## 18. Documentation Rules

## 18.1 Endpoint Documentation Missing

### Rule ID

```text
DOC-001
```

### Condition

An implemented endpoint has no contract or workflow documentation.

### Severity

```text
MEDIUM
```

### Recommendation

Document request, response, validation, business rules, downstream calls, persistence, and error behavior.

---

## 18.2 Architecture Documentation Missing

### Rule ID

```text
DOC-002
```

### Condition

The system has no architecture or dependency documentation.

### Severity

```text
MEDIUM
```

### Recommendation

Create a system-context and component-level architecture document.

---

## 18.3 Documentation Conflicts with Code

### Rule ID

```text
DOC-003
```

### Condition

Documented behavior differs from detected implementation behavior.

### Severity

```text
HIGH
```

### Recommendation

Resolve the conflict with the product and engineering owners before migration.

---

## 19. Severity Assignment Rules

Severity reflects the technical or business impact of a finding.

### Critical

Use `CRITICAL` when the finding may cause:

* Security exposure
* Financial inconsistency
* Data corruption
* Duplicate state-changing operations
* Missing authentication or authorization
* Broken transaction integrity
* Production unavailability

### High

Use `HIGH` when the finding may cause:

* Incorrect API behavior
* Major migration rework
* Missing business functionality
* Unsupported dependencies
* Contract incompatibility
* Significant operational risk

### Medium

Use `MEDIUM` when the finding may cause:

* Maintainability issues
* Reduced observability
* Inconsistent behavior
* Moderate testing gaps
* Deployment friction

### Low

Use `LOW` when the finding represents:

* Minor documentation gaps
* Optional improvements
* Low-confidence inferred issues
* Non-blocking cleanup work

---

## 20. Confidence Assignment Rules

Confidence represents how strongly the available evidence supports a finding.

Suggested confidence values:

| Evidence Quality                            | Confidence |
| ------------------------------------------- | ---------: |
| Direct source-code evidence                 |       0.95 |
| Direct configuration or schema evidence     |       0.95 |
| Explicit API documentation                  |       0.90 |
| Automated test evidence                     |       0.90 |
| Consistent evidence from multiple artifacts |       0.95 |
| Code pattern inference                      |       0.75 |
| Comment-only evidence                       |       0.60 |
| User-provided statement                     |       0.70 |
| AI-only inference                           |       0.50 |
| No source evidence                          |       0.25 |

A finding with confidence below `0.60` should be clearly marked for human verification.

---

## 21. Recommendation Generation Rules

Recommendations must:

* Address a specific matched finding.
* Reference the selected target architecture.
* Avoid unsupported implementation claims.
* Explain the required technical action.
* Be written as an executable engineering recommendation.
* Preserve compatibility unless the finding explicitly requires a contract change.

Example transformation:

```text
Finding:
Handler directly executes SQL.

Recommendation:
Introduce an account repository interface and move SQL execution from the
HTTP handler into a transaction-aware repository implementation.
```

Recommendations should not use vague language such as:

```text
Improve the code.
Fix the architecture.
Use best practices.
Make the API scalable.
```

---

## 22. Migration Task Generation Rules

Each high or critical finding should generate at least one migration task.

Medium findings may generate tasks when they affect implementation quality or target compliance.

Low findings may remain as recommendations unless explicitly selected by the user.

Example generated task:

```text
Task ID: MIG-API-002
Priority: P1
Complexity: MEDIUM

Title:
Extract account persistence from the HTTP handler

Description:
Create an account repository interface and move all account SQL operations
from the handler into a repository implementation.

Acceptance Criteria:
- Handler contains no SQL statements.
- Service depends on the repository interface.
- Repository methods accept context.
- Existing API behavior remains unchanged.
- Unit tests cover repository success and failure paths.
```

---

## 23. Priority Assignment Rules

### P0

Use when the task blocks safe migration or production deployment.

Examples:

* Remove exposed secrets
* Add missing authentication
* Correct transaction-integrity defects
* Prevent duplicate financial operations

### P1

Use for high-impact architecture or compatibility work.

Examples:

* Separate handlers and repositories
* Replace unsupported dependencies
* Implement required validation
* Preserve critical API contracts

### P2

Use for important quality and maintainability improvements.

Examples:

* Add structured logging
* Improve test coverage
* Centralize error mapping
* Externalize configuration

### P3

Use for non-blocking cleanup and documentation.

Examples:

* Rename inconsistent symbols
* Improve low-risk documentation
* Remove verified dead code

---

## 24. Complexity Assignment Rules

### Small

Typical characteristics:

* One file or isolated component
* No contract change
* Minimal testing impact

### Medium

Typical characteristics:

* Multiple files
* One endpoint or dependency
* New tests required
* Limited architecture change

### Large

Typical characteristics:

* Multiple endpoints
* Data-model or contract changes
* Transaction or security changes
* Broad regression testing

### Extra Large

Typical characteristics:

* Cross-system migration
* Major datastore replacement
* Shared framework replacement
* Significant unknown behavior
* Multiple teams required

---

## 25. Readiness Score Rules

The migration-readiness score should begin at `100`.

Suggested deductions:

| Finding                           | Deduction |
| --------------------------------- | --------: |
| Missing target architecture       |        30 |
| No endpoint evidence              |        20 |
| Missing API contract              |        10 |
| No automated tests                |        15 |
| Missing dependency documentation  |        10 |
| Missing database schema evidence  |        10 |
| Documentation conflicts with code |        10 |
| Missing deployment definition     |         5 |
| Missing observability evidence    |         5 |
| Each unresolved critical unknown  |        10 |

The final readiness score must remain between `0` and `100`.

Suggested interpretation:

| Score  | Meaning                          |
| ------ | -------------------------------- |
| 80–100 | Ready for planned migration      |
| 60–79  | Ready with targeted discovery    |
| 40–59  | Significant preparation required |
| 20–39  | High uncertainty and rework risk |
| 0–19   | Migration should not begin       |

---

## 26. Complexity Score Rules

The migration-complexity score should begin at `0`.

Suggested additions:

| Condition                        | Addition |
| -------------------------------- | -------: |
| Each endpoint                    |        2 |
| Each external dependency         |        3 |
| Each datastore                   |        5 |
| Transactional workflow           |        8 |
| Concurrency-sensitive workflow   |        8 |
| Unsupported framework            |       10 |
| Unsupported database feature     |       10 |
| Critical business-rule density   |       10 |
| Security integration replacement |        8 |
| Public contract change           |       10 |
| Missing source evidence          |        5 |
| Each critical gap                |        8 |
| Each high gap                    |        4 |

The final score must be capped at `100`.

Suggested interpretation:

| Score  | Meaning                   |
| ------ | ------------------------- |
| 0–20   | Low complexity            |
| 21–40  | Moderate complexity       |
| 41–60  | High complexity           |
| 61–80  | Very high complexity      |
| 81–100 | Enterprise transformation |

---

## 27. Risk Score Rules

The migration-risk score should begin at `0`.

Suggested finding weights:

| Severity | Weight |
| -------- | -----: |
| Critical |     15 |
| High     |      8 |
| Medium   |      4 |
| Low      |      1 |

Additional risk factors:

| Condition                         | Addition |
| --------------------------------- | -------: |
| Missing transaction evidence      |       10 |
| Unknown external contract         |        8 |
| Missing authentication evidence   |       15 |
| Data migration required           |        8 |
| No compatibility tests            |        8 |
| Documentation conflicts with code |        6 |
| Low-confidence critical rule      |        5 |

The final risk score must be capped at `100`.

Suggested interpretation:

| Score  | Meaning                 |
| ------ | ----------------------- |
| 0–20   | Low risk                |
| 21–40  | Controlled risk         |
| 41–60  | Significant risk        |
| 61–80  | High risk               |
| 81–100 | Critical migration risk |

---

## 28. Duplicate Finding Prevention

The rule engine must avoid creating duplicate findings.

Findings may be considered duplicates when they share:

* The same rule ID
* The same endpoint or component
* The same source evidence
* The same category and technical cause

When duplicate findings are detected:

1. Merge their evidence.
2. Retain the highest severity.
3. Retain the highest confidence.
4. Combine affected endpoints or components.
5. Generate only one recommendation unless separate remediation is required.

---

## 29. Missing-Evidence Handling

The analyzer must not treat missing evidence as proof that a feature does not exist.

Example:

```text
No authentication code was found.
```

This must be represented as:

```text
Authentication implementation could not be verified from the supplied artifacts.
```

It must not automatically state:

```text
The application has no authentication.
```

A definitive statement is allowed only when direct evidence supports it.

---

## 30. Rule Configuration Structure

Recommended file:

```text
src/data/migrationRules.ts
```

Suggested structure:

```typescript
export const migrationRules: MigrationRuleDefinition[] = [
  {
    id: "API-002",
    name: "Handler accesses database directly",
    description:
      "Detects database access performed directly from the transport layer.",
    category: "ARCHITECTURE",
    severity: "HIGH",
    priority: "P1",
    confidence: 0.95,
    condition: {
      field: "endpoint.handler.databaseOperations",
      operator: "NOT_EMPTY",
    },
    recommendationTemplate:
      "Introduce a repository interface and move persistence logic out of the handler.",
    enabled: true,
  },
];
```

The rule engine should not depend on UI components or React state.

---

## 31. Rule Evaluation Function

Recommended function signature:

```typescript
function evaluateMigrationRules(
  context: MigrationAnalysisContext,
  rules: MigrationRuleDefinition[]
): MigrationRuleResult[];
```

The evaluation process should:

1. Ignore disabled rules.
2. Verify whether the rule applies to the current context.
3. Evaluate the configured condition.
4. Collect matching evidence.
5. Calculate confidence.
6. Generate the structured finding.
7. Generate the recommendation.
8. Generate tasks when required.
9. Return the rule result.

---

## 32. Deterministic Output Requirement

For the same normalized input, target architecture, rule configuration, and analysis version, the engine must return the same:

* Findings
* Severity values
* Priorities
* Confidence values
* Recommendations
* Migration tasks
* Scores

AI-generated wording may vary, but it must not alter deterministic rule results unless a user explicitly accepts the change.

---

## 33. Phase 5 Validation Checklist

Phase 5 is complete when:

* Source-completeness rules are defined.
* Endpoint architecture rules are defined.
* API-contract rules are defined.
* Dependency rules are defined.
* Data and transaction rules are defined.
* Business-logic rules are defined.
* Security rules are defined.
* Configuration rules are defined.
* Error-handling rules are defined.
* Observability rules are defined.
* Testing rules are defined.
* Deployment and documentation rules are defined.
* Severity and confidence logic are documented.
* Recommendation and task-generation behavior is defined.
* Readiness, complexity, and risk scoring rules are documented.
* Duplicate and missing-evidence handling are defined.
* The rules can be implemented as configuration-driven TypeScript data.

---

## 34. Phase 5 Outcome

The **AI-Assisted API Migration Analyzer** now has a deterministic migration-analysis framework.

The engine can evaluate a normalized legacy API using configurable rules and generate:

```text
Source Findings
      ↓
Architecture Gaps
      ↓
Migration Risks
      ↓
Evidence-Based Recommendations
      ↓
Prioritized Migration Tasks
      ↓
Readiness, Complexity, and Risk Scores
```

These rules provide the foundation for implementing the analysis engine without depending entirely on AI-generated conclusions.
