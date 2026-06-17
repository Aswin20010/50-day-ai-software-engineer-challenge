# Day 16 — Phase 4: Hallucination Guardrails

## Purpose

This document defines hallucination guardrails for the future AI-assisted enterprise API artifact generation and gap analysis system.

The purpose of these guardrails is to prevent the AI from inventing unsupported information while extracting requirements, modeling workflows, generating Excel intake content, detecting gaps, and producing evaluation summaries.

In enterprise API development, unsupported assumptions can create serious downstream issues.

A hallucinated field, service, table, query, validation rule, or business rule can cause the generated artifacts to become incorrect.

The guardrails in this document are designed to keep the system evidence-based, traceable, and reviewable.

---

## Why Hallucination Guardrails Are Needed

AI systems can sometimes produce information that sounds correct but is not supported by the source artifact.

In this project, that risk must be controlled carefully because the output may later influence:

```text
Node-RED workflow design
Excel intake workbook generation
Gap analysis reports
Go code generation readiness
Research evaluation results
```

If unsupported information enters the pipeline, the final generated output may not match the original requirement.

Examples of risky hallucinations include:

* Adding a header that was not mentioned in the requirement
* Adding a request field that does not exist in the sample payload
* Inventing a downstream service URL
* Creating a database table that was not referenced
* Assuming SQLite when the target is PostgreSQL
* Adding generic CRUD logic to a business-rule API
* Assigning Critical severity without evidence
* Claiming experiment results before metrics are actually measured

These guardrails help prevent those issues.

---

## Core Guardrail Principle

The most important rule is:

```text
Do not invent unsupported details.
```

If information is missing, unclear, or ambiguous, the AI should mark it clearly instead of guessing.

Allowed labels include:

```text
Not Found
Missing
Ambiguous
Needs Manual Review
TBD
Low Confidence
Unsupported Assumption
```

---

# Guardrail 1 — Source Artifact Authority

## Rule

The AI must treat the provided source artifacts as the authority.

For most requirement-related tasks, the Confluence requirement is the primary source of truth.

The AI should not override the source artifact with general knowledge, assumptions, or patterns from unrelated APIs.

---

## Correct Behavior

If the Confluence requirement says:

```text
POST /api/v1/discount/interrogateAccount
```

The AI should preserve that endpoint exactly.

---

## Incorrect Behavior

The AI should not change it to:

```text
POST /api/v1/account/interrogate
```

unless the source artifact explicitly defines that path.

---

## Application

This guardrail applies to:

```text
Requirement Extraction Agent
Workflow Modeling Agent
Excel Intake Generation Agent
Gap Analysis Agent
Reviewer Agent
```

---

# Guardrail 2 — No Invented Endpoints

## Rule

The AI must not invent API endpoints.

An endpoint can only be included if it is explicitly present in the source requirement, workflow artifact, Excel workbook, or benchmark definition.

---

## Correct Behavior

If only this endpoint is provided:

```text
POST /api/v1/fabrics
```

The AI should only extract or generate content for that endpoint.

---

## Incorrect Behavior

The AI should not add:

```text
GET /api/v1/fabrics
PUT /api/v1/fabrics/{id}
DELETE /api/v1/fabrics/{id}
```

unless those endpoints are present in the source.

---

## Risk

Invented endpoints can cause incorrect API generation and misleading project documentation.

---

# Guardrail 3 — No Invented Headers

## Rule

The AI must not add request headers that are not found in the source artifact.

If a common enterprise header is expected but not present, it should be marked as missing or needing manual review.

---

## Correct Behavior

If the source lists:

```text
Client-Id
Subclient-Id
Division-Id
Trace-Id
```

The AI should extract only those headers.

---

## Incorrect Behavior

The AI should not automatically add:

```text
Authorization
Content-Type
Accept
x-api-key
x-correlation-id
```

unless the source provides evidence.

---

## Exception

If a header is technically implied by a sample curl or payload format, the AI may mark it as:

```text
Inferred — Needs Manual Review
```

It should not mark it as confirmed.

---

# Guardrail 4 — No Invented Request or Response Fields

## Rule

The AI must not invent request body fields or response body fields.

Fields should come from:

```text
Requirement text
Sample request
Sample response
Schema section
Workflow mapping
Excel workbook field list
```

---

## Correct Behavior

If the request sample contains:

```json
{
  "account": {
    "encryptInfo": {
      "encryptedData": "string",
      "keyType": "TOKEN"
    }
  }
}
```

The AI can extract:

```text
account.encryptInfo.encryptedData
account.encryptInfo.keyType
```

---

## Incorrect Behavior

The AI should not add:

```text
account.customerId
account.cardNumber
account.phoneNumber
account.emailAddress
```

unless they exist in the source.

---

## Risk

Invented fields can produce incorrect DTOs, validations, and Go code.

---

# Guardrail 5 — No Invented Enum Values

## Rule

The AI must not invent enum values.

Enum values must come directly from the source artifact.

---

## Correct Behavior

If the source says:

```text
messageSeverity allowed values: INFO, CRITICAL
```

The AI should use only:

```text
INFO
CRITICAL
```

---

## Incorrect Behavior

The AI should not add:

```text
WARNING
ERROR
DEBUG
```

unless the source explicitly lists those values.

---

## Missing Enum Handling

If the source says a field is an enum but does not list values, the AI should output:

```text
Enum values not found — Needs Manual Review
```

---

# Guardrail 6 — No Invented Business Rules

## Rule

The AI must not invent business rules.

Business rules should come from requirement process text, explicit acceptance criteria, workflow decisions, or structured business requirement sections.

---

## Correct Behavior

If the source says:

```text
If X-Acting-As-Type is not global, reject the request.
```

The AI can extract this as a business rule.

---

## Incorrect Behavior

The AI should not add:

```text
If user role is Admin, bypass validation.
```

unless the source states it.

---

## Risk

Invented business rules are high risk because they can change actual application behavior.

---

# Guardrail 7 — No Invented Downstream Services

## Rule

The AI must not invent downstream REST, SOAP, database, Kafka, Pub/Sub, or file dependencies.

External services must be supported by the source artifact.

---

## Correct Behavior

If the source mentions:

```text
PIIDLookup downstream service
```

The AI can include that service.

---

## Incorrect Behavior

The AI should not add:

```text
Customer Profile Service
Account History Service
Kafka Event Publisher
Notification Service
```

unless the source includes them.

---

## Unsupported Service Handling

If a service appears likely but is not confirmed, mark it as:

```text
Possible service dependency — Needs Manual Review
```

---

# Guardrail 8 — No Invented Database Tables or Queries

## Rule

The AI must not invent database tables, entities, or SQL queries.

Database details must come from the source requirement, workflow, Excel workbook, or known project artifact.

---

## Correct Behavior

If the source references:

```text
APP_MSGS
```

The AI can create an entity or query reference for `APP_MSGS`.

---

## Incorrect Behavior

The AI should not invent:

```text
CUSTOMER_PROFILE
ACCOUNT_MASTER
DISCOUNT_HISTORY
TRANSACTION_LOG
```

unless those tables are present in the source.

---

## Query Handling

If the requirement says a lookup is needed but does not provide exact SQL, the AI should write:

```text
Query required — SQL not provided — Needs Manual Review
```

It should not fabricate production SQL.

---

# Guardrail 9 — Preserve Technology Assumptions

## Rule

The AI must preserve the target technology stack defined by the project.

For this challenge, the target implementation direction is:

```text
Go + PostgreSQL
```

unless a source artifact explicitly states otherwise.

---

## Correct Behavior

If the Excel or technical requirement says PostgreSQL, preserve PostgreSQL.

---

## Incorrect Behavior

The AI should not change PostgreSQL to:

```text
SQLite
MongoDB
MySQL
DynamoDB
```

without source evidence.

---

## Special Rule

If Node-RED or a prototype used SQLite for local testing, that should not override the enterprise target of PostgreSQL.

It should be documented as:

```text
Prototype-only detail, not target production database.
```

---

# Guardrail 10 — Do Not Convert APIs into Generic CRUD

## Rule

The AI must not assume that an enterprise API is a generic CRUD API.

Many APIs in this project are business-rule or integration APIs.

---

## Correct Behavior

For an API like:

```text
POST /api/v1/discount/evaluateTransactionDiscount
```

The AI should focus on:

```text
Discount logic
Eligibility rules
Database lookups
Response mapping
```

---

## Incorrect Behavior

The AI should not create generic operations such as:

```text
Create Discount
Update Discount
Delete Discount
List Discounts
```

unless the source defines those operations.

---

# Guardrail 11 — Evidence Required for Confirmed Claims

## Rule

Any confirmed extraction, gap, or recommendation should be supported by evidence.

Evidence may come from:

```text
Confluence text
Sample request
Sample response
Node-RED node
Excel sheet row
Manual baseline
Benchmark notes
```

---

## Correct Behavior

A gap should include evidence such as:

```text
Confluence: x-macys-apikey is listed as required.
Excel: x-macys-apikey is not present in required_headers.
```

---

## Incorrect Behavior

The AI should not write:

```text
This is missing because APIs usually require it.
```

That is not source evidence.

---

# Guardrail 12 — Separate Confirmed Facts from Assumptions

## Rule

The AI must clearly separate confirmed facts from assumptions.

---

## Correct Format

```text
Confirmed:
- Endpoint is POST /api/v1/fabrics.

Assumption:
- Timeout behavior may be required but is not defined.

Needs Manual Review:
- Whether Trace-Id should be required or optional is unclear.
```

---

## Incorrect Format

The AI should not present assumptions as confirmed facts.

---

# Guardrail 13 — Ambiguity Handling

## Rule

If the source is unclear, the AI should not force a conclusion.

The AI should mark the item as ambiguous.

---

## Allowed Labels

```text
Ambiguous
Needs Manual Review
Low Confidence
Not Enough Evidence
```

---

## Example

If the source says:

```text
Trace-Id should be passed if available.
```

The AI should mark it as:

```text
Optional or conditional — Needs Manual Review if workbook requires strict classification.
```

It should not automatically mark it as required.

---

# Guardrail 14 — Severity Assignment Control

## Rule

The AI must assign gap severity using the severity rules defined in Day 13.

Severity should not be random or exaggerated.

---

## Correct Behavior

A missing endpoint is usually:

```text
Critical
```

A missing required header is usually:

```text
High
```

A naming mismatch with documented mapping is usually:

```text
Low or Informational
```

---

## Incorrect Behavior

The AI should not mark a minor formatting issue as Critical.

The AI should not mark a missing core business rule as Low.

---

# Guardrail 15 — Avoid Over-Reporting Naming Differences

## Rule

The AI must not treat every naming difference as a real gap.

Enterprise artifacts often use different naming styles.

Examples:

```text
Division-id
division_id
divisionNbr
division
```

These may refer to the same concept.

---

## Correct Behavior

If the mapping is clear, report:

```text
Naming variation observed, but no confirmed gap.
```

or mark it as:

```text
Informational
```

---

## Incorrect Behavior

Do not automatically classify every naming variation as a High severity gap.

---

# Guardrail 16 — Do Not Hide Missing Information

## Rule

The AI should not silently skip missing information.

If a required element cannot be found, it should be reported clearly.

---

## Correct Behavior

```text
Required response mapping is not found in Excel.
```

---

## Incorrect Behavior

The AI should not omit the issue to make the artifact appear complete.

---

# Guardrail 17 — Do Not Claim Measured Results Before Evaluation

## Rule

The AI must not claim experiment results before the experiment is actually run.

---

## Correct Wording Before Evaluation

```text
Target: 85%+ gap detection accuracy.
Planned metric: Manual review reduction.
Expected evaluation format: Per-case benchmark summary.
```

---

## Incorrect Wording Before Evaluation

```text
The framework achieved 85% gap detection accuracy.
The project reduced manual review time by 70%.
```

Those claims should only be made after actual measured results exist.

---

# Guardrail 18 — Human Review for Critical and High Gaps

## Rule

Critical and High gaps should require human review before final approval.

The AI can identify and recommend fixes, but it should not act as final authority for high-risk decisions.

---

## Correct Behavior

```text
Status: Blocked
Reason: One unresolved Critical gap exists.
Required Action: Human reviewer must confirm the correction before proceeding.
```

---

## Incorrect Behavior

```text
The artifact is approved for production because AI says so.
```

---

# Guardrail 19 — Confidence Scoring

## Rule

The AI should assign confidence to important outputs.

Allowed confidence values:

```text
High
Medium
Low
```

---

## Confidence Guidance

| Confidence | Meaning                               |
| ---------- | ------------------------------------- |
| High       | Strong evidence exists in the source  |
| Medium     | Evidence exists but may be incomplete |
| Low        | Weak evidence or ambiguity exists     |

---

## Example

```text
Field: account.entryMode
Status: Required
Confidence: Medium
Reason: Present in validation text, but not clearly shown in all sample requests.
```

---

# Guardrail 20 — Output Schema Compliance

## Rule

The AI should follow the expected output schema for each task.

Examples:

```text
Requirement extraction schema
Workflow representation schema
Excel intake sheet format
Gap output schema
Evaluation result schema
Review summary format
```

---

## Risk

If the AI does not follow schemas, downstream automation and evaluation become harder.

---

# Agent-Specific Guardrails

## Requirement Extraction Agent

Must not:

```text
Invent missing fields
Normalize field names without preserving originals
Convert examples into requirements without noting source
Assume optionality without evidence
```

Must:

```text
Preserve exact names
Mark missing details
Include evidence
Track confidence
```

---

## Workflow Modeling Agent

Must not:

```text
Add unsupported workflow steps
Skip required validation
Invent service calls
Invent database lookups
Change business rule order without source support
```

Must:

```text
Represent steps in order
Mark unclear steps
Preserve decision logic
Identify error paths when present
```

---

## Excel Intake Generation Agent

Must not:

```text
Add unsupported Excel rows
Add unsupported services
Add unsupported tables
Change the target stack
Convert custom logic into CRUD
```

Must:

```text
Use source-supported values
Mark TBD fields
Preserve validations
Preserve mapping details
```

---

## Gap Analysis Agent

Must not:

```text
Report gaps without evidence
Treat mapped naming differences as defects
Ignore unsupported assumptions
Assign severity randomly
```

Must:

```text
Compare sources directionally
Include evidence
Assign severity consistently
Provide actionable recommendations
```

---

## Evaluation Agent

Must not:

```text
Change metric formulas
Hide false positives
Hide false negatives
Overstate results
Treat partial matches as exact without noting it
```

Must:

```text
Use the manual baseline
Calculate metrics honestly
Report limitations
Separate measured results from targets
```

---

## Reviewer Agent

Must not:

```text
Approve unresolved Critical gaps
Ignore High-risk findings
Treat AI output as final authority
Claim production readiness without evidence
```

Must:

```text
Flag blockers
Summarize risks
Prepare human review actions
Separate Ready, Needs Review, Blocked, and Not Ready
```

---

# Hallucination Risk Examples

## Example 1: Invented Header

### Bad Output

```text
Required header: Authorization
```

### Problem

The source did not mention Authorization.

### Correct Output

```text
Authorization header not found in source. Needs Manual Review if authentication is required.
```

---

## Example 2: Invented Database Query

### Bad Output

```sql
SELECT * FROM customer_profile WHERE customer_id = ?
```

### Problem

The source did not mention `customer_profile`.

### Correct Output

```text
Database lookup appears required, but table name and SQL are not provided. Marked as Needs Manual Review.
```

---

## Example 3: Invented Business Rule

### Bad Output

```text
If user is Admin, skip overlap validation.
```

### Problem

The source did not define this rule.

### Correct Output

```text
No Admin bypass rule found in source.
```

---

## Example 4: Overstated Research Result

### Bad Output

```text
The framework achieved 90% accuracy.
```

### Problem

The experiment has not been run yet.

### Correct Output

```text
Target accuracy is 85% or higher. Actual accuracy will be measured during the experiment phase.
```

---

# Guardrail Checklist

Before accepting any AI output, verify:

| Check          | Question                                                               |
| -------------- | ---------------------------------------------------------------------- |
| Source support | Is the item supported by the input artifact?                           |
| No invention   | Did the AI avoid inventing fields, services, or queries?               |
| Evidence       | Is evidence included for confirmed claims?                             |
| Confidence     | Is confidence assigned where needed?                                   |
| Ambiguity      | Are unclear items marked for manual review?                            |
| Schema         | Does the output follow the required schema?                            |
| Severity       | Are severity levels assigned consistently?                             |
| Technology     | Does the output preserve Go + PostgreSQL unless source says otherwise? |
| Results        | Are planned targets separated from measured results?                   |
| Human review   | Are Critical and High gaps flagged for human review?                   |

---

# Recommended Output Labels

The following labels should be used consistently:

```text
Confirmed
Not Found
Missing
Ambiguous
Needs Manual Review
TBD
Low Confidence
Unsupported Assumption
Inferred from Source
Prototype-Only Detail
Blocking Gap
Ready
Needs Review
Blocked
Not Ready
```

---

## Summary

Hallucination guardrails are essential for keeping the AI-assisted enterprise API artifact generation pipeline reliable.

The AI must not invent unsupported endpoints, headers, fields, services, tables, queries, business rules, technology assumptions, or research results.

When information is missing or unclear, the AI should mark it clearly and route it to manual review.

These guardrails protect the quality of requirement extraction, workflow modeling, Excel intake generation, gap analysis, evaluation, and research reporting.

By enforcing these rules, the project becomes more trustworthy, repeatable, and suitable for future implementation.
