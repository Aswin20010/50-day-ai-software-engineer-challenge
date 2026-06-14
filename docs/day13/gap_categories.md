# Day 13 — Phase 2: Gap Categories

## Purpose

This document defines the major gap categories used by the Gap Analysis Framework.

The goal of gap categorization is to classify every mismatch found between:

1. Confluence requirement documents
2. Node-RED workflow implementations
3. Excel intake workbooks

Each gap category helps identify what type of requirement or implementation detail is missing, incorrect, duplicated, or inconsistent.

---

## Why Gap Categories Are Needed

Gap analysis without categories can become difficult to review.

If every issue is only described in plain text, it becomes hard to understand:

* What kind of issue occurred
* Which artifact is incomplete
* How serious the issue is
* Which team or system layer should fix it
* Whether similar issues are repeated across multiple APIs

Gap categories make the output structured and measurable.

They allow the system to group issues by type, such as:

```text
Missing header
Missing validation
Incorrect mapping
Missing downstream service
Missing database query
Technology mismatch
```

This makes the gap analysis output easier to review, score, and improve.

---

## Primary Gap Categories

The following categories will be used by the framework.

---

## 1. Missing Endpoint Gap

### Definition

A missing endpoint gap occurs when an API endpoint exists in the Confluence requirement but is missing from the Node-RED flow or Excel intake workbook.

### Example

Confluence defines:

```text
POST /api/v1/discount/evaluateTransactionDiscount
```

But the Excel `api_endpoints` sheet does not contain this endpoint.

### Impact

This is a major issue because the generated service may not create the required API route.

### Typical Fix

Add the missing endpoint details to the correct artifact.

Required details may include:

* HTTP method
* Endpoint path
* Operation name
* Request headers
* Request body
* Response body
* Validation rules

---

## 2. Missing Header Gap

### Definition

A missing header gap occurs when a required request header exists in the source requirement but is not represented in the workflow or Excel intake workbook.

### Example

Confluence requires:

```text
requestorInfo-clientId
requestorInfo-subclientId
requestorInfo-traceId
x-macys-apikey
```

But the Excel `api_endpoints` sheet only includes:

```text
requestorInfo-clientId
requestorInfo-subclientId
```

### Impact

Missing headers can cause authentication failures, tracing issues, downstream service failures, or incomplete request validation.

### Typical Fix

Add the missing header to the header definition and validation rules.

---

## 3. Missing Request Field Gap

### Definition

A missing request field gap occurs when a request body field is defined in Confluence but is missing from the workflow or Excel workbook.

### Example

Confluence defines:

```text
account.encryptInfo.encryptedData
account.encryptInfo.keyType
account.entryMode
```

But the Excel request body only includes:

```text
account.encryptInfo.encryptedData
account.encryptInfo.keyType
```

The `account.entryMode` field is missing.

### Impact

Missing request fields can cause the generated DTO, validation logic, or service logic to be incomplete.

### Typical Fix

Add the missing request field with:

* Field name
* Data type
* Required or optional status
* Validation rule
* Mapping target

---

## 4. Missing Response Field Gap

### Definition

A missing response field gap occurs when a response field defined in the requirement or workflow is missing from the Excel workbook or generated output schema.

### Example

Confluence expects:

```json
{
  "accountInfo": {
    "internalAccount": "string",
    "internalAccountToken": "string",
    "lookupType": "string",
    "rc": "string"
  }
}
```

But the Excel response definition does not include `lookupType`.

### Impact

The final API response may not match the expected contract.

### Typical Fix

Add the missing response field to the response body mapping and output schema.

---

## 5. Missing Validation Gap

### Definition

A missing validation gap occurs when a required validation rule exists in Confluence or Node-RED but is missing from the Excel intake workbook.

### Example

Confluence says:

```text
Division-id must be numeric.
```

But the Excel validation rules do not mention numeric validation for `Division-id`.

### Impact

Invalid requests may be accepted by generated code.

### Typical Fix

Add the missing validation rule to the correct endpoint row or validation section.

---

## 6. Missing Enum Validation Gap

### Definition

A missing enum validation gap occurs when a field has a fixed list of allowed values in the requirement, but the allowed values are not represented in the workflow or workbook.

### Example

Confluence defines `entryMode` allowed values:

```text
KEYED
BARCODE_SCAN
SWIPED
TAP
TOKENIZED
```

But the Excel workbook only says:

```text
entryMode is required
```

and does not include the enum list.

### Impact

The generated API may allow unsupported values.

### Typical Fix

Add the full enum list to the validation rule.

---

## 7. Missing Business Rule Gap

### Definition

A missing business rule gap occurs when a business rule exists in the requirement but is not represented in Node-RED or Excel.

### Example

Confluence says:

```text
If the account is an employee account, apply employee discount logic.
If the account is a new account, apply new account discount logic.
```

But the Excel `business_requirements` sheet does not include this decision rule.

### Impact

The generated service logic may not match the required business behavior.

### Typical Fix

Add the missing business rule with:

* Business rule ID
* Description
* Condition
* Expected action
* Acceptance criteria

---

## 8. Missing Downstream Service Gap

### Definition

A missing downstream service gap occurs when the requirement or workflow calls an external service, but the Excel workbook does not define it.

### Example

Confluence defines a call to:

```text
PIIDLookup service
```

But the Excel `external_services` sheet does not include a row for that service.

### Impact

The generated code may not create the required REST or SOAP client.

### Typical Fix

Add the downstream service details, including:

* Service ID
* Service type
* Endpoint URL
* HTTP method
* Request headers
* Request body
* Response mapping
* Error handling behavior

---

## 9. Missing Database Query Gap

### Definition

A missing database query gap occurs when a requirement or workflow needs a database lookup, insert, update, or delete operation, but the Excel workbook does not define the query.

### Example

The requirement needs a query to check active discount records, but the `queries` sheet does not include it.

### Impact

The generated repository layer may be incomplete.

### Typical Fix

Add the missing query with:

* Query ID
* Query purpose
* Table name
* SQL statement
* Input parameters
* Output fields

---

## 10. Missing Entity Gap

### Definition

A missing entity gap occurs when a database table or domain entity is referenced by the requirement but is missing from the Excel `entities` sheet.

### Example

Confluence references:

```text
Emp_Disc_Profile
Disc_By_Dept_Vnd_Class
```

But the Excel workbook does not include entity definitions for these tables.

### Impact

The generated model or repository layer may not understand the required database structure.

### Typical Fix

Add the entity metadata, including:

* Entity name
* Table name
* Key fields
* Important columns
* Relationship to business rules

---

## 11. Incorrect Field Mapping Gap

### Definition

An incorrect field mapping gap occurs when a field is mapped to the wrong source or target.

### Example

The requirement says:

```text
traceId should come from requestorInfo-traceId header.
```

But the workflow maps `traceId` from the request body.

### Impact

The generated code may use the wrong input value.

### Typical Fix

Correct the field source and target mapping.

---

## 12. Incorrect Response Mapping Gap

### Definition

An incorrect response mapping gap occurs when the API response is built using the wrong downstream field, database field, or computed value.

### Example

Downstream returns:

```text
return_code_o
```

The REST response should map it to:

```text
code
```

But the workbook maps it to:

```text
message
```

### Impact

The API response may be contractually incorrect.

### Typical Fix

Update the response mapping rule to match the requirement.

---

## 13. Incorrect Service Mapping Gap

### Definition

An incorrect service mapping gap occurs when the workflow or Excel workbook maps an endpoint to the wrong service or wrong operation.

### Example

The API should call:

```text
/accountinterrogation/piidLookup
```

But the workbook maps it to a different account service.

### Impact

Generated code may call the wrong downstream dependency.

### Typical Fix

Correct the service ID, service operation, and mapping references.

---

## 14. Incorrect Data Type Gap

### Definition

An incorrect data type gap occurs when a field is represented with the wrong type.

### Example

Confluence defines:

```text
division: integer
```

But Excel defines:

```text
division: string
```

### Impact

This can cause validation errors, DTO mismatch, database mismatch, or runtime conversion issues.

### Typical Fix

Update the data type to match the source requirement.

---

## 15. Incorrect Required or Optional Status Gap

### Definition

This gap occurs when a required field is marked optional or an optional field is marked required.

### Example

Confluence says:

```text
requestorInfo-traceId is optional.
```

But the Excel workbook marks it as required.

### Impact

The generated API may reject valid requests or accept invalid requests.

### Typical Fix

Correct the required or optional flag.

---

## 16. Technology Mismatch Gap

### Definition

A technology mismatch gap occurs when the generated artifact uses a technology that does not match the project target.

### Example

The project target is:

```text
Go + PostgreSQL
```

But the workbook or generated output refers to:

```text
Node.js + SQLite
```

### Impact

The generated code may be unsuitable for the intended production environment.

### Typical Fix

Update the technical requirement fields to match the correct target technology.

---

## 17. Error Handling Gap

### Definition

An error handling gap occurs when expected error behavior is missing or incorrectly represented.

### Example

Confluence says:

```text
If downstream PIIDLookup returns 401, return an authentication error response.
```

But the workbook does not define downstream error handling.

### Impact

The generated code may return unclear or incorrect errors.

### Typical Fix

Add error handling rules for:

* Validation errors
* Authentication errors
* Downstream service errors
* Database errors
* Business rule failures

---

## 18. Extra Artifact Gap

### Definition

An extra artifact gap occurs when the workflow or workbook contains fields, services, entities, or rules that are not present in the source requirement.

### Example

The Excel workbook includes:

```text
Kafka event publishing
```

But the Confluence requirement does not mention Kafka.

### Impact

The generated code may include unnecessary or incorrect functionality.

### Typical Fix

Remove the unsupported artifact or mark it as an assumption requiring confirmation.

---

## 19. Naming Mismatch Gap

### Definition

A naming mismatch gap occurs when the same concept is represented using inconsistent names across artifacts.

### Example

Confluence uses:

```text
Division-id
```

Node-RED uses:

```text
divisionNbr
```

Excel uses:

```text
division_id
```

### Impact

Naming mismatches can cause mapping confusion and generated DTO errors.

### Typical Fix

Add a normalized mapping rule or align the naming convention.

---

## 20. Workflow Step Gap

### Definition

A workflow step gap occurs when a required processing step exists in one artifact but is missing from another.

### Example

Node-RED contains:

```text
Validate Headers → Validate Body → Call Downstream Service → Map Response
```

But the Excel `endpoint_service_mapping` only contains:

```text
Call Downstream Service → Map Response
```

The validation steps are missing.

### Impact

Generated service orchestration may be incomplete.

### Typical Fix

Add the missing workflow steps to the endpoint mapping sheet.

---

## Recommended Gap Category Format

Each gap category should be represented using a consistent structure.

```json
{
  "gap_category": "Missing Header Gap",
  "description": "A required header exists in the source requirement but is missing from the target artifact.",
  "example": "x-macys-apikey is required in Confluence but missing from Excel api_endpoints.",
  "impact": "Generated API may fail authentication or request validation.",
  "recommended_action": "Add the missing header to the api_endpoints sheet and validation rules."
}
```

---

## Category Grouping

For reporting, the categories can be grouped into larger families.

### Contract Gaps

These relate to the external API contract.

```text
Missing Endpoint Gap
Missing Header Gap
Missing Request Field Gap
Missing Response Field Gap
Incorrect Data Type Gap
Incorrect Required or Optional Status Gap
```

### Validation Gaps

These relate to input checking.

```text
Missing Validation Gap
Missing Enum Validation Gap
Error Handling Gap
```

### Business Logic Gaps

These relate to rule execution.

```text
Missing Business Rule Gap
Workflow Step Gap
Incorrect Field Mapping Gap
Incorrect Response Mapping Gap
```

### Integration Gaps

These relate to external or internal dependencies.

```text
Missing Downstream Service Gap
Incorrect Service Mapping Gap
Missing Database Query Gap
Missing Entity Gap
```

### Consistency Gaps

These relate to artifact alignment.

```text
Technology Mismatch Gap
Extra Artifact Gap
Naming Mismatch Gap
```

---

## Summary

Gap categories help the Gap Analysis Framework classify issues in a structured way.

They make it easier to detect, report, prioritize, and fix inconsistencies between Confluence requirements, Node-RED workflows, and Excel intake workbooks.

These categories will be used in later phases to define the gap output schema, severity levels, and evaluation method for the 50-Day AI Software Engineer Challenge.
