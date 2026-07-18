# Day 49 – Phase 4

# Evaluation Dataset, Ground-Truth Templates, and Results Collection Plan

## Project

**AI-Assisted API Factory and Migration Analyzer**

## Research Paper Working Title

**AI-Assisted API Modernization: A Rule-Based Architecture for Requirement Extraction, API Generation, and Migration Analysis**

---

# 1. Phase 4 Objective

The objective of Day 49 Phase 4 is to convert the research methodology defined in Phase 3 into concrete evaluation artifacts.

This phase defines:

* Evaluation dataset cases.
* Requirement-case templates.
* Source-project case templates.
* Ground-truth JSON structures.
* Experiment manifests.
* AI-only and hybrid run records.
* Human-review worksheets.
* Result-collection templates.
* Failure-classification rules.
* Dataset validation requirements.
* Evaluation execution checklists.

This phase prepares the files needed to perform the research experiments. It does not assign achieved research results.

---

# 2. Phase 4 Deliverables

Phase 4 produces the following planned artifacts:

```text
evaluation/
├── README.md
├── datasets/
│   ├── requirements/
│   ├── source-projects/
│   └── manifests/
├── ground-truth/
│   ├── requirements/
│   ├── migration-analysis/
│   └── schemas/
├── prompts/
│   ├── ai-only/
│   └── hybrid/
├── runs/
│   ├── manual/
│   ├── ai-only/
│   └── hybrid/
├── reviews/
├── results/
│   ├── accuracy/
│   ├── consistency/
│   ├── timing/
│   ├── traceability/
│   ├── human-review/
│   └── failures/
└── scripts/
```

The primary Phase 4 planning document should be stored at:

```text
docs/day49/evaluation_dataset_and_templates.md
```

---

# 3. Evaluation Dataset Overview

The evaluation dataset should contain multiple API cases with different levels of complexity.

The complete target dataset contains eight cases.

| Case ID | Case Name                         | Primary Purpose                             | Complexity |
| ------- | --------------------------------- | ------------------------------------------- | ---------- |
| C1      | Simple Product API                | Basic extraction and endpoint analysis      | Low        |
| C2      | Customer Validation API           | Complex validation-rule extraction          | Medium     |
| C3      | Wallet Transaction API            | Transaction and status-flow analysis        | High       |
| C4      | Identity Integration API          | External-client and resilience analysis     | High       |
| C5      | Legacy Order API                  | Architecture-gap and risk detection         | High       |
| C6      | Partially Modernized Discount API | Partial architecture-rule evaluation        | High       |
| C7      | Secure Layered Account API        | False-positive control                      | Medium     |
| C8      | Incomplete Shipment Requirement   | Missing and ambiguous requirement detection | Medium     |

The case names should remain generic and must not reveal confidential company systems.

---

# 4. Case C1 – Simple Product API

## 4.1 Purpose

Case C1 evaluates whether the API Factory and Migration Analyzer can correctly process a simple, well-defined API.

It provides the low-complexity baseline for:

* Endpoint extraction.
* Field extraction.
* Basic validation.
* Layer detection.
* False-positive measurement.

---

## 4.2 Requirement Scenario

The system manages basic product records.

Required endpoints:

1. Create a product.
2. Retrieve a product by identifier.
3. Update a product.
4. Delete a product.
5. List active products.

---

## 4.3 Expected Endpoints

| Endpoint ID | Method | Path                           |
| ----------- | ------ | ------------------------------ |
| C1-E1       | POST   | `/api/v1/products`             |
| C1-E2       | GET    | `/api/v1/products/{productId}` |
| C1-E3       | PUT    | `/api/v1/products/{productId}` |
| C1-E4       | DELETE | `/api/v1/products/{productId}` |
| C1-E5       | GET    | `/api/v1/products`             |

---

## 4.4 Expected Requirement Categories

The ground truth should contain:

* Product identifier.
* Product name.
* Description.
* Price.
* Active status.
* Required-field rules.
* Positive-price validation.
* Not-found response.
* Duplicate-product response.
* Standard success responses.

---

## 4.5 Expected Architecture Characteristics

The source implementation should include:

* Route layer.
* Handler layer.
* Service layer.
* Repository layer.
* Request validation.
* Centralized error response.
* Unit tests.

This case should contain few or no major architecture gaps.

---

# 5. Case C2 – Customer Validation API

## 5.1 Purpose

Case C2 evaluates complex validation extraction and deterministic validation.

---

## 5.2 Requirement Scenario

The API registers customer profiles.

The request contains:

* First name.
* Last name.
* Email.
* Phone number.
* Date of birth.
* Country.
* State.
* Postal code.
* Preferred-contact method.

---

## 5.3 Required Validation Rules

The ground truth should include:

* First name is required.
* Last name is required.
* Email is required when preferred contact is email.
* Phone is required when preferred contact is phone.
* Email must use a valid format.
* Phone must use an approved format.
* Customer must meet the configured minimum age.
* State is required for configured countries.
* Postal code format depends on country.
* Preferred-contact method must be an approved value.

---

## 5.4 Intentionally Injected Invalid Outputs

The deterministic validator should be tested against:

* Missing email requirement.
* Invalid preferred-contact value.
* Unsupported field type.
* Duplicate email rule.
* Rule referencing an unknown field.
* Conditional rule missing its condition.
* Invalid date format.
* Contradictory required flags.

---

## 5.5 Expected Evaluation Focus

Measure:

* Rule extraction precision.
* Rule extraction recall.
* Conditional-rule accuracy.
* Defect-detection rate.
* Validator false-positive rate.

---

# 6. Case C3 – Wallet Transaction API

## 6.1 Purpose

Case C3 evaluates transactional behavior, status transitions, amount handling, and data-integrity analysis.

---

## 6.2 Requirement Scenario

The API manages a stored-value wallet.

Example operations:

1. Create wallet account.
2. Add funds.
3. Retrieve balance.
4. Place redemption hold.
5. Settle redemption.
6. Void an eligible transaction.

---

## 6.3 Expected Business Rules

Ground truth should cover:

* Wallet must exist.
* Wallet must be in an eligible status.
* Transaction amount must be positive where required.
* Balance cannot become negative.
* Commit creates a memo transaction.
* Settlement changes the transaction to audited.
* Status transitions must follow the configured state machine.
* Duplicate operation identifiers must be rejected or handled idempotently.
* Database updates must be atomic.
* Transaction history must be preserved.

---

## 6.4 Expected Evidence

Evidence should include:

* Account status validation.
* Balance calculation.
* Transaction creation.
* Transaction status updates.
* Repository transaction boundaries.
* Audit record creation.
* Error handling.
* Tests for success and failure paths.

---

## 6.5 Expected Gap Categories

Potential intentional gaps:

* Missing idempotency control.
* Missing transaction rollback handling.
* Missing structured audit logging.
* Missing concurrency test.
* Missing invalid status-transition test.

---

# 7. Case C4 – Identity Integration API

## 7.1 Purpose

Case C4 evaluates external-service integration, authentication requirements, timeout behavior, tracing, and failure handling.

---

## 7.2 Requirement Scenario

The API receives an encrypted identity reference and calls a downstream identity-resolution service.

Expected flow:

1. Validate request headers.
2. Validate identity format.
3. Call downstream identity service.
4. Apply timeout.
5. Map downstream response.
6. Return normalized customer identity.
7. Preserve trace identifiers.
8. Handle downstream failure.

---

## 7.3 Expected Evidence

The analyzer should detect:

* External client.
* Configurable downstream URL.
* Authentication headers.
* Timeout configuration.
* Correlation or trace identifier.
* Error mapping.
* Retry behavior, when present.
* Logging without exposing secrets.
* Unit tests with mocked downstream behavior.

---

## 7.4 Expected Risks

Potential intentional risks:

* Hard-coded endpoint.
* Missing timeout.
* Missing retry or circuit-breaker behavior.
* Sensitive data written to logs.
* Downstream error exposed directly.
* Missing trace propagation.
* No integration test.

---

# 8. Case C5 – Legacy Order API

## 8.1 Purpose

Case C5 is the primary modernization-analysis case.

It should represent a legacy API with substantial architecture and maintainability issues.

---

## 8.2 Source Characteristics

The source project may contain:

* Routes and business logic in one file.
* Direct database access from handlers.
* Hard-coded configuration.
* Repeated validation.
* Unstructured console logging.
* Inconsistent error responses.
* Limited automated testing.
* Shared mutable state.
* Missing dependency injection.
* Large functions with mixed responsibilities.

---

## 8.3 Expected Architecture Gaps

Ground truth should include:

* Missing handler-service-repository separation.
* Missing centralized validation.
* Missing centralized error handling.
* Missing configuration abstraction.
* Missing structured logging.
* Missing tracing.
* Missing transaction management.
* Low test coverage.
* Tight coupling to external services.
* Limited deployment-readiness documentation.

---

## 8.4 Expected Risks

Ground truth should include:

* Data-integrity risk.
* Maintainability risk.
* Regression risk.
* Operational-observability risk.
* Configuration risk.
* Security risk where secrets are hard-coded.
* Migration-complexity risk.

---

## 8.5 Expected Recommendations

Recommendations should include:

* Introduce layered architecture.
* Extract repository operations.
* Add request validation.
* Centralize error responses.
* Externalize configuration.
* Add structured logging.
* Add tracing and correlation.
* Add automated tests.
* Add transaction boundaries.
* Migrate incrementally by endpoint.

---

# 9. Case C6 – Partially Modernized Discount API

## 9.1 Purpose

Case C6 evaluates whether the analyzer can recognize partial compliance instead of classifying every rule as pass or fail.

---

## 9.2 Source Characteristics

The source project should include:

* Handler layer.
* Service layer.
* Repository layer.
* Basic validation.
* Some unit tests.
* Structured configuration.
* Partial logging.
* Limited tracing.
* Incomplete integration tests.
* Inconsistent error mapping.

---

## 9.3 Expected Rule Outcomes

| Category                  | Expected Outcome  |
| ------------------------- | ----------------- |
| Layer separation          | Passed            |
| Input validation          | Partially passed  |
| Configuration             | Passed            |
| Error handling            | Partially passed  |
| Structured logging        | Partially passed  |
| Tracing                   | Failed            |
| Unit testing              | Partially passed  |
| Integration testing       | Failed            |
| Data transaction handling | Passed or partial |
| Documentation             | Partial           |

---

## 9.4 Evaluation Focus

Measure whether the system:

* Correctly identifies existing capabilities.
* Avoids duplicate recommendations.
* Produces partial outcomes.
* Assigns reasonable severity.
* Does not penalize non-applicable rules.
* Generates targeted rather than generic recommendations.

---

# 10. Case C7 – Secure Layered Account API

## 10.1 Purpose

Case C7 evaluates false-positive control.

The source should represent a well-designed API aligned with most target-architecture requirements.

---

## 10.2 Source Characteristics

The project should include:

* Clear route layer.
* Handler layer.
* Service layer.
* Repository layer.
* Dependency injection.
* Schema validation.
* Authentication.
* Authorization.
* Centralized error handling.
* Structured logging.
* Trace propagation.
* Database transactions.
* Unit tests.
* Integration tests.
* Environment-based configuration.
* Health endpoint.
* API documentation.

---

## 10.3 Expected Findings

The analyzer should produce:

* High readiness score.
* Few major gaps.
* Few or no high-severity risks.
* Evidence for passed rules.
* Limited recommendations focused only on genuine improvements.

---

## 10.4 Evaluation Focus

Measure:

* False-positive gap rate.
* False-positive risk rate.
* Evidence accuracy.
* Correct recognition of implemented capabilities.
* Scoring agreement with expert review.

---

# 11. Case C8 – Incomplete Shipment Requirement

## 11.1 Purpose

Case C8 evaluates whether the API Factory identifies missing and conflicting information rather than inventing requirements.

---

## 11.2 Requirement Scenario

The input describes a shipment-status API but intentionally omits or conflicts on:

* Endpoint method.
* Final path.
* Required identifier.
* Response fields.
* Error codes.
* Authentication.
* Retry behavior.
* Shipment-number requirement.
* Status terminology.

---

## 11.3 Expected System Behavior

The system should:

* Extract only explicit requirements.
* Mark unresolved fields.
* Identify ambiguous terminology.
* Identify contradictory statements.
* Avoid inventing response fields.
* Avoid selecting an unsupported HTTP method.
* Avoid creating security requirements without evidence.
* Request or record missing information for human review.

---

## 11.4 Expected Missing Fields

The ground truth should explicitly list:

* `httpMethod`
* `endpointPath`
* `shipmentIdentifier`
* `successResponse`
* `errorResponses`
* `authenticationRequirement`
* `statusDefinition`

---

# 12. Requirement Dataset File Template

Each requirement case should use a Markdown file.

Recommended location:

```text
evaluation/datasets/requirements/C1-simple-product-api.md
```

Template:

```md
# Evaluation Case

## Case ID

C1

## Case Name

Simple Product API

## Context

Describe the business purpose of the API.

## Functional Requirements

List the explicitly stated functional requirements.

## Endpoint Requirements

List known endpoint requirements.

## Request Requirements

List known request fields, headers and parameters.

## Response Requirements

List known response fields and statuses.

## Validation Requirements

List explicit validation rules.

## Business Rules

List explicit business rules.

## Error Requirements

List known error conditions and responses.

## Security Requirements

List explicit authentication and authorization requirements.

## Integration Requirements

List external-service and database requirements.

## Non-Functional Requirements

List logging, tracing, latency, availability or test requirements.

## Known Ambiguities

List intentionally missing or conflicting information.
```

---

# 13. Source-Project Dataset Manifest

Every source-code case should have a source manifest.

Recommended location:

```text
evaluation/datasets/source-projects/C5/manifest.json
```

Template:

```json
{
  "caseId": "C5",
  "caseName": "Legacy Order API",
  "language": "TypeScript",
  "framework": "Express",
  "sourceRoot": "./src",
  "artifactFiles": [
    {
      "artifactId": "C5-A1",
      "path": "src/routes/orders.ts",
      "artifactType": "route"
    },
    {
      "artifactId": "C5-A2",
      "path": "src/db/orders.ts",
      "artifactType": "repository"
    },
    {
      "artifactId": "C5-A3",
      "path": "src/config.ts",
      "artifactType": "configuration"
    }
  ],
  "targetArchitectureId": "layered-enterprise-api",
  "containsIntentionalDefects": true,
  "notes": [
    "Business logic is intentionally placed in the route file.",
    "Configuration values are intentionally hard-coded."
  ]
}
```

---

# 14. Requirement Ground-Truth Template

Recommended location:

```text
evaluation/ground-truth/requirements/C1-ground-truth.json
```

Template:

```json
{
  "caseId": "C1",
  "version": "1.0.0",
  "flowName": "Product Management",
  "endpoints": [
    {
      "endpointId": "C1-E1",
      "name": "Create Product",
      "method": "POST",
      "path": "/api/v1/products",
      "headers": [],
      "pathParameters": [],
      "queryParameters": [],
      "requestFields": [
        {
          "fieldId": "C1-E1-F1",
          "name": "name",
          "type": "string",
          "required": true
        },
        {
          "fieldId": "C1-E1-F2",
          "name": "price",
          "type": "number",
          "required": true
        }
      ],
      "responseFields": [
        {
          "fieldId": "C1-E1-R1",
          "name": "productId",
          "type": "string",
          "required": true
        }
      ],
      "rules": [
        {
          "ruleId": "C1-E1-V1",
          "category": "validation",
          "description": "Product price must be greater than zero.",
          "targetField": "price"
        }
      ],
      "errors": [
        {
          "status": 409,
          "code": "PRODUCT_ALREADY_EXISTS",
          "description": "A matching product already exists."
        }
      ]
    }
  ],
  "missingInformation": [],
  "ambiguities": [],
  "review": {
    "status": "approved",
    "reviewers": [
      "reviewer-1"
    ],
    "frozenAt": "TBD"
  }
}
```

---

# 15. Migration Ground-Truth Template

Recommended location:

```text
evaluation/ground-truth/migration-analysis/C5-ground-truth.json
```

Template:

```json
{
  "caseId": "C5",
  "version": "1.0.0",
  "targetArchitectureId": "layered-enterprise-api",
  "expectedEvidence": [
    {
      "evidenceId": "C5-EV1",
      "category": "layering",
      "artifactId": "C5-A1",
      "description": "The route file contains business logic and database access."
    }
  ],
  "expectedRuleResults": [
    {
      "ruleId": "ARCH-LAYER-001",
      "expectedStatus": "failed",
      "supportingEvidenceIds": [
        "C5-EV1"
      ]
    }
  ],
  "expectedGaps": [
    {
      "gapId": "C5-G1",
      "category": "architecture",
      "severity": "high",
      "description": "Business logic is not separated from the HTTP route layer.",
      "supportingEvidenceIds": [
        "C5-EV1"
      ],
      "architectureRuleIds": [
        "ARCH-LAYER-001"
      ]
    }
  ],
  "expectedRisks": [
    {
      "riskId": "C5-R1",
      "category": "maintainability",
      "severity": "high",
      "description": "Mixed responsibilities increase regression and maintenance risk.",
      "linkedGapIds": [
        "C5-G1"
      ]
    }
  ],
  "expectedRecommendations": [
    {
      "recommendationId": "C5-REC1",
      "priority": "high",
      "description": "Move business logic into a service layer and data access into a repository layer.",
      "linkedGapIds": [
        "C5-G1"
      ],
      "linkedRiskIds": [
        "C5-R1"
      ]
    }
  ],
  "nonApplicableRuleIds": [],
  "review": {
    "status": "approved",
    "reviewers": [
      "reviewer-1"
    ],
    "frozenAt": "TBD"
  }
}
```

---

# 16. Ground-Truth Matching Rules

Generated output and ground truth may use different identifiers or wording.

The comparison process should match items using the following priority:

1. Stable identifier.
2. Normalized endpoint method and path.
3. Category and target field.
4. Category and source evidence.
5. Human-reviewed semantic equivalence.

---

## 16.1 Normalization Rules

Before comparison:

* Trim whitespace.
* Convert HTTP methods to uppercase.
* Normalize repeated slashes in paths.
* Remove trailing slashes except for root.
* Convert configured category names to canonical values.
* Normalize severity names.
* Sort unordered identifier collections.
* Remove duplicate items.
* Preserve semantically meaningful case in descriptions where necessary.

---

## 16.2 Match Classification

Each generated item should be classified as:

* `exact`
* `semantic`
* `partial`
* `incorrect`
* `unsupported`
* `missing`

---

## 16.3 Partial Match

A partial match may apply when:

* The main finding is correct but severity is wrong.
* The endpoint is correct but one field is missing.
* The rule is correct but its target is incomplete.
* The recommendation addresses the gap but lacks implementation detail.

Partial credit rules must be defined before calculating metrics.

---

# 17. Experiment Manifest Template

Recommended location:

```text
evaluation/datasets/manifests/C1-experiment.json
```

Template:

```json
{
  "experimentId": "EXP-C1",
  "caseId": "C1",
  "datasetVersion": "1.0.0",
  "groundTruthVersion": "1.0.0",
  "targetArchitectureId": "layered-enterprise-api",
  "inputs": {
    "requirementFile": "../requirements/C1-simple-product-api.md",
    "sourceProjectManifest": "../source-projects/C1/manifest.json"
  },
  "approaches": {
    "manual": {
      "enabled": true,
      "repetitions": 1
    },
    "aiOnly": {
      "enabled": true,
      "repetitions": 3,
      "promptVersion": "ai-only-v1"
    },
    "hybrid": {
      "enabled": true,
      "repetitions": 3,
      "promptVersion": "hybrid-v1",
      "schemaVersion": "1.0.0",
      "ruleSetVersion": "1.0.0"
    },
    "deterministicRerun": {
      "enabled": true,
      "repetitions": 3
    }
  },
  "expectedOutputs": {
    "requirementAnalysis": true,
    "migrationAnalysis": true,
    "humanReview": true
  }
}
```

---

# 18. AI-Only Prompt Template

Recommended location:

```text
evaluation/prompts/ai-only/api-analysis-v1.md
```

Template:

```md
You are analyzing an enterprise API requirement or source-project description.

Produce the following:

1. Structured API requirements.
2. Endpoint definitions.
3. Request and response fields.
4. Validation and business rules.
5. Architecture gaps.
6. Technical risks.
7. Modernization recommendations.
8. Migration tasks.

Use only information supported by the provided input.

Do not assume undocumented functionality.

Return valid JSON using the requested output structure.
```

The exact output schema used for the AI-only baseline must be frozen before the experiments.

The prompt should not include:

* Ground truth.
* Expected gaps.
* Expected scores.
* Hybrid validation errors.
* Manual corrections.

---

# 19. Hybrid Extraction Prompt Template

Recommended location:

```text
evaluation/prompts/hybrid/requirement-extraction-v1.md
```

Template:

```md
Extract only the API requirements explicitly stated or directly supported by the input.

Return candidate values for:

- Flow name.
- Endpoints.
- HTTP methods.
- Paths.
- Headers.
- Parameters.
- Request fields.
- Response fields.
- Validation rules.
- Business rules.
- Error behavior.
- Security requirements.
- External dependencies.
- Missing information.
- Ambiguities.

Do not invent missing requirements.

When information is unavailable, add it to missingInformation.

When statements conflict, add them to ambiguities.

The output will be validated by a deterministic schema and rule engine.
```

---

# 20. Run Metadata Template

Recommended location:

```text
evaluation/runs/{approach}/{runId}/metadata.json
```

Template:

```json
{
  "runId": "C1-HYBRID-001",
  "experimentId": "EXP-C1",
  "caseId": "C1",
  "approach": "hybrid",
  "status": "completed",
  "startedAt": "TBD",
  "completedAt": "TBD",
  "durationMs": 0,
  "datasetVersion": "1.0.0",
  "groundTruthVersion": "1.0.0",
  "promptVersion": "hybrid-v1",
  "schemaVersion": "1.0.0",
  "ruleSetVersion": "1.0.0",
  "targetArchitectureId": "layered-enterprise-api",
  "model": {
    "name": "TBD",
    "temperature": "TBD",
    "topP": "TBD",
    "maxTokens": "TBD"
  },
  "inputChecksum": "TBD",
  "outputChecksum": "TBD",
  "failure": null
}
```

---

# 21. Run Output Folder

Every run should preserve both raw and processed output.

```text
evaluation/runs/hybrid/C1-HYBRID-001/
├── metadata.json
├── raw-input.md
├── raw-ai-output.json
├── normalized-output.json
├── schema-validation.json
├── rule-validation.json
├── evidence.json
├── analysis-result.json
├── comparison.json
└── reviewer-summary.json
```

For manual runs:

```text
evaluation/runs/manual/C1-MANUAL-001/
├── metadata.json
├── provided-input.md
├── submitted-output.json
├── comparison.json
└── reviewer-summary.json
```

---

# 22. Validation Result Template

Recommended file:

```text
schema-validation.json
```

Template:

```json
{
  "valid": false,
  "errorCount": 2,
  "warningCount": 1,
  "errors": [
    {
      "code": "MISSING_REQUIRED_FIELD",
      "path": "endpoints[0].path",
      "message": "Endpoint path is required."
    },
    {
      "code": "UNSUPPORTED_HTTP_METHOD",
      "path": "endpoints[1].method",
      "message": "The supplied HTTP method is not supported."
    }
  ],
  "warnings": [
    {
      "code": "MISSING_ERROR_DEFINITION",
      "path": "endpoints[0].errors",
      "message": "No error behavior was supplied."
    }
  ]
}
```

---

# 23. Comparison Result Template

Recommended file:

```text
comparison.json
```

Template:

```json
{
  "caseId": "C1",
  "runId": "C1-HYBRID-001",
  "categories": [
    {
      "category": "endpoints",
      "truePositives": 5,
      "falsePositives": 0,
      "falseNegatives": 0,
      "precision": 1,
      "recall": 1,
      "f1Score": 1,
      "matches": [
        {
          "expectedId": "C1-E1",
          "generatedId": "endpoint-1",
          "classification": "exact",
          "reviewRequired": false
        }
      ]
    }
  ],
  "structuralValidity": true,
  "traceabilityCoverage": 1,
  "correctionCount": 0,
  "notes": []
}
```

Values above are examples only and must not be reused as research results unless obtained during the actual evaluation.

---

# 24. Failure Record Template

Failed runs must be preserved.

Recommended file:

```text
evaluation/results/failures/{runId}.json
```

Template:

```json
{
  "runId": "C2-AI-ONLY-002",
  "caseId": "C2",
  "approach": "ai-only",
  "failureCategory": "invalid-structured-output",
  "stage": "output-parsing",
  "message": "The generated output was not valid JSON.",
  "recoverable": false,
  "includedInCompletionRate": true,
  "includedInAccuracyMetrics": false,
  "rawOutputPreserved": true,
  "notes": []
}
```

---

# 25. Failure Categories

Use standardized failure categories:

* `input-error`
* `configuration-error`
* `model-error`
* `timeout`
* `empty-output`
* `invalid-structured-output`
* `schema-validation-failure`
* `rule-engine-failure`
* `evidence-extraction-failure`
* `analysis-orchestration-failure`
* `comparison-failure`
* `review-incomplete`
* `unknown`

A schema validation failure may still be a measurable result rather than a system failure. The distinction must be recorded clearly.

---

# 26. Human Reviewer Worksheet

Recommended location:

```text
evaluation/reviews/C1-HYBRID-001-reviewer-1.md
```

Template:

```md
# Evaluation Review Worksheet

## Run Information

- Run ID:
- Case ID:
- Approach:
- Reviewer ID:
- Review started:
- Review completed:

## Correctness

Score from 1 to 5:

Comments:

## Completeness

Score from 1 to 5:

Comments:

## Explainability

Score from 1 to 5:

Comments:

## Evidence Quality

Score from 1 to 5:

Comments:

## Recommendation Actionability

Score from 1 to 5:

Comments:

## Migration Task Quality

Score from 1 to 5:

Comments:

## Overall Confidence

Score from 1 to 5:

Comments:

## Findings Requiring Deletion

List incorrect or unsupported findings.

## Missing Findings

List expected findings not generated.

## Findings Requiring Modification

List partially correct findings.

## Evidence Corrections

List invalid or insufficient evidence links.

## Review Summary

- Number accepted without change:
- Number accepted with minor change:
- Number requiring major change:
- Number rejected:
- Number added manually:
- Review duration:
```

---

# 27. Blind Review Procedure

Where practical, reviewers should not know whether an output came from:

* Manual analysis.
* AI-only analysis.
* Hybrid analysis.

Use anonymized labels such as:

* Output A.
* Output B.
* Output C.

The mapping should be stored separately until reviews are complete.

This reduces preference bias toward the proposed system.

---

# 28. Review Assignment Template

Recommended file:

```text
evaluation/reviews/assignments.json
```

Template:

```json
{
  "reviewBatchId": "RB-001",
  "caseId": "C1",
  "reviewers": [
    {
      "reviewerId": "reviewer-1",
      "outputs": [
        {
          "blindOutputId": "C1-O-A",
          "runId": "C1-HYBRID-001"
        },
        {
          "blindOutputId": "C1-O-B",
          "runId": "C1-AI-ONLY-001"
        }
      ]
    }
  ],
  "randomized": true,
  "mappingHiddenUntil": "review-complete"
}
```

The visible reviewer package should not expose the `runId` mapping.

---

# 29. Timing Worksheet

Recommended file:

```text
evaluation/results/timing/timing-records.csv
```

Columns:

```csv
run_id,case_id,approach,input_preparation_ms,system_execution_ms,human_review_ms,human_correction_ms,total_workflow_ms,status
```

Timing definitions:

* `input_preparation_ms`: Time spent preparing usable input.
* `system_execution_ms`: Machine processing time.
* `human_review_ms`: Time spent verifying output.
* `human_correction_ms`: Time spent correcting output.
* `total_workflow_ms`: Complete end-to-end time.

---

# 30. Accuracy Results Worksheet

Recommended file:

```text
evaluation/results/accuracy/accuracy-results.csv
```

Columns:

```csv
run_id,case_id,approach,category,true_positives,false_positives,false_negatives,precision,recall,f1_score
```

Categories may include:

* endpoints
* request-fields
* response-fields
* validations
* business-rules
* evidence
* architecture-gaps
* risks
* recommendations
* migration-tasks

---

# 31. Traceability Results Worksheet

Recommended file:

```text
evaluation/results/traceability/traceability-results.csv
```

Columns:

```csv
run_id,case_id,approach,total_findings,findings_with_links,resolvable_links,supporting_links,invalid_links,traceability_coverage,evidence_accuracy
```

Definitions:

* `findings_with_links`: Findings containing at least one reference.
* `resolvable_links`: References that point to an existing artifact.
* `supporting_links`: References that actually support the finding.
* `invalid_links`: Broken or unsupported references.

---

# 32. Consistency Results Worksheet

Recommended file:

```text
evaluation/results/consistency/consistency-results.csv
```

Columns:

```csv
case_id,approach,run_a,run_b,endpoint_overlap,requirement_overlap,gap_overlap,risk_overlap,recommendation_overlap,score_difference
```

For three runs, compare:

* Run 1 with Run 2.
* Run 1 with Run 3.
* Run 2 with Run 3.

---

# 33. Human Review Results Worksheet

Recommended file:

```text
evaluation/results/human-review/review-results.csv
```

Columns:

```csv
reviewer_id,blind_output_id,run_id,case_id,approach,correctness,completeness,explainability,evidence_quality,actionability,task_quality,confidence,review_duration_ms,correction_count
```

The `approach` field may be added only after blind review is complete.

---

# 34. Defect-Injection Dataset

Create a dedicated deterministic-validator case.

Recommended location:

```text
evaluation/datasets/invalid-inputs/
```

Files:

```text
missing-endpoint-path.json
unsupported-http-method.json
duplicate-endpoint-id.json
invalid-field-type.json
unknown-rule-target.json
broken-evidence-reference.json
recommendation-without-finding.json
task-without-acceptance-criteria.json
circular-task-dependency.json
```

Manifest:

```json
{
  "datasetId": "VALIDATION-DEFECTS-001",
  "version": "1.0.0",
  "defects": [
    {
      "file": "missing-endpoint-path.json",
      "expectedErrorCode": "MISSING_REQUIRED_FIELD"
    },
    {
      "file": "unsupported-http-method.json",
      "expectedErrorCode": "UNSUPPORTED_HTTP_METHOD"
    }
  ]
}
```

---

# 35. Dataset Validation Checklist

Before freezing a case:

* [ ] The case has a unique identifier.
* [ ] The case description is complete.
* [ ] Confidential information has been removed.
* [ ] Requirement inputs are readable without outside context.
* [ ] Source artifact manifests resolve correctly.
* [ ] Ground-truth identifiers are unique.
* [ ] Ground-truth endpoint methods and paths are normalized.
* [ ] Expected evidence links resolve.
* [ ] Every expected gap links to a rule.
* [ ] Every expected risk links to evidence or a gap.
* [ ] Recommendations link to supported findings.
* [ ] Non-applicable rules are documented.
* [ ] Ambiguities are explicitly recorded.
* [ ] Ground truth has been reviewed.
* [ ] The version has been frozen.

---

# 36. Run Validation Checklist

Before executing a run:

* [ ] Correct experiment manifest selected.
* [ ] Dataset version recorded.
* [ ] Ground-truth version recorded.
* [ ] Prompt version recorded.
* [ ] Model configuration recorded.
* [ ] Schema version recorded.
* [ ] Rule-set version recorded.
* [ ] Target architecture recorded.
* [ ] Input checksum generated.
* [ ] Previous output cleared.
* [ ] Timer ready.
* [ ] Ground truth hidden from the evaluated approach.

After execution:

* [ ] Raw output saved.
* [ ] Processed output saved.
* [ ] Validation result saved.
* [ ] Failure state recorded.
* [ ] Output checksum generated.
* [ ] Comparison executed.
* [ ] Review package generated.
* [ ] Timing data recorded.

---

# 37. Ground-Truth Review Checklist

The reviewer should confirm:

* [ ] Every explicit endpoint is included.
* [ ] No unsupported endpoint is included.
* [ ] Request fields are correct.
* [ ] Response fields are correct.
* [ ] Required and optional flags are correct.
* [ ] Validation rules are correct.
* [ ] Business rules are correct.
* [ ] Missing information is not silently completed.
* [ ] Source evidence accurately represents the implementation.
* [ ] Gaps are based on target-architecture rules.
* [ ] Risks are technically justified.
* [ ] Severities are reasonable.
* [ ] Recommendations address actual findings.
* [ ] Non-applicable rules are excluded.
* [ ] Confidential content is removed.

---

# 38. Dataset Versioning

Use semantic versioning:

```text
MAJOR.MINOR.PATCH
```

Examples:

* `1.0.0`: Initial frozen dataset.
* `1.1.0`: New case or significant new expected content.
* `1.0.1`: Typographical or metadata correction that does not change expected results.
* `2.0.0`: Breaking ground-truth or schema change.

Every version change should include:

* Date.
* Author.
* Changed files.
* Reason.
* Expected metric impact.

---

# 39. Dataset Change Log Template

Recommended file:

```text
evaluation/CHANGELOG.md
```

Template:

```md
# Evaluation Dataset Change Log

## 1.0.0

### Added

- Initial eight evaluation cases.
- Requirement ground truth.
- Migration-analysis ground truth.
- Experiment manifests.
- Reviewer worksheets.

### Changed

- None.

### Fixed

- None.

### Metric Impact

- Initial baseline version.
```

---

# 40. Data Exclusion Rules

A run may be excluded from a specific metric only when:

* The metric is not applicable.
* The input was corrupted before execution.
* The system environment failed independently of the evaluated approach.
* The reviewer did not complete the assigned review.

A run must not be excluded merely because it performed poorly.

Every exclusion must record:

* Run ID.
* Metric excluded.
* Reason.
* Approver.
* Whether the run remains in completion-rate reporting.

---

# 41. Result Aggregation Rules

Aggregate results at the following levels:

1. Per run.
2. Per case.
3. Per approach.
4. Per output category.
5. Overall.

Do not report only an overall average.

Overall averages can hide:

* Poor performance on high-complexity cases.
* High false-positive rates.
* Incomplete outputs.
* Failed runs.
* Differences between requirement extraction and migration analysis.

---

# 42. Planned Result Summary

The final results summary should include:

## 42.1 Dataset Information

* Number of cases.
* Number of endpoints.
* Number of expected requirements.
* Number of expected gaps.
* Number of expected risks.
* Number of total runs.
* Number of reviewers.

## 42.2 Accuracy

* Requirement precision, recall and F1.
* Gap precision, recall and F1.
* Risk precision, recall and F1.
* Evidence accuracy.

## 42.3 Reliability

* Structural-validity rate.
* Workflow-completion rate.
* Failed-run count.
* Deterministic-rerun stability.

## 42.4 Explainability

* Traceability coverage.
* Invalid evidence-link count.
* Human explainability rating.
* Human confidence rating.

## 42.5 Efficiency

* System execution time.
* Review time.
* Correction time.
* End-to-end time reduction.

---

# 43. Recommended Initial Execution Order

Execute the evaluation in this order:

1. Validate all ground-truth files.
2. Run defect-injection validation tests.
3. Execute C1 as a pilot.
4. Review the C1 comparison process.
5. Correct evaluation scripts, not ground truth, where necessary.
6. Freeze the final comparison logic.
7. Execute C2 and C8 for requirement extraction.
8. Execute C5 and C7 for migration analysis.
9. Execute C3, C4 and C6.
10. Run repeated AI-only and hybrid experiments.
11. Run deterministic consistency tests.
12. Create blind reviewer packages.
13. Collect reviewer results.
14. Aggregate metrics.
15. Perform failure analysis.
16. Write the results and discussion sections.

---

# 44. Minimum Viable Evaluation

When time is limited, the minimum viable evaluation should include:

* C1 – Simple Product API.
* C2 – Customer Validation API.
* C5 – Legacy Order API.
* C7 – Secure Layered Account API.
* C8 – Incomplete Shipment Requirement.

Minimum runs:

* One manual run per selected case.
* Three AI-only runs per selected case.
* Three hybrid runs per selected case.
* Three deterministic reruns for at least two cases.

This produces a minimum of:

```text
5 manual runs
15 AI-only runs
15 hybrid runs
6 deterministic reruns
```

Total minimum planned executions:

```text
41 runs
```

The paper must state when the minimum evaluation is used instead of the complete eight-case plan.

---

# 45. Research Claim Protection

The dataset and templates should prevent unsupported claims.

Do not claim:

* Perfect extraction.
* Complete replacement of engineers.
* Universal architecture scoring.
* Production security validation.
* Guaranteed migration success.
* Generalization across all languages and frameworks.
* Statistically significant improvement without sufficient samples.

Supported claims should be limited to the tested:

* Cases.
* Languages.
* Frameworks.
* Rule sets.
* Models.
* Target architectures.
* Reviewer population.

---

# 46. Phase 4 Completion Checklist

* [x] Defined the complete evaluation dataset.
* [x] Defined eight evaluation cases.
* [x] Defined requirement dataset templates.
* [x] Defined source-project manifests.
* [x] Defined requirement ground-truth files.
* [x] Defined migration-analysis ground-truth files.
* [x] Defined matching and normalization rules.
* [x] Defined experiment manifests.
* [x] Defined AI-only prompts.
* [x] Defined hybrid prompts.
* [x] Defined run metadata.
* [x] Defined run output folders.
* [x] Defined validation results.
* [x] Defined comparison results.
* [x] Defined failure records.
* [x] Defined standardized failure categories.
* [x] Defined human-review worksheets.
* [x] Defined blind review.
* [x] Defined timing worksheets.
* [x] Defined accuracy worksheets.
* [x] Defined traceability worksheets.
* [x] Defined consistency worksheets.
* [x] Defined human-review result files.
* [x] Defined defect-injection cases.
* [x] Defined dataset and run checklists.
* [x] Defined versioning and change control.
* [x] Defined data-exclusion rules.
* [x] Defined result aggregation.
* [x] Defined the execution order.
* [x] Defined the minimum viable evaluation.

---

# 47. Phase 4 Completion Status

**Status:** Completed

**Recommended file:**

```text
docs/day49/evaluation_dataset_and_templates.md
```

**Next Phase:**

Day 49 Phase 5 will define the paper figures, architecture diagrams, result tables, chart plan, screenshot evidence, and final publication-asset checklist.
