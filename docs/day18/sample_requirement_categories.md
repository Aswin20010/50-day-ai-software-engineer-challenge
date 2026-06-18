# Day 18 — Sample Requirement Categories

## Purpose

This document defines the sample requirement categories for the workflow evaluation dataset in the 50-Day AI Software Engineer Challenge.

The purpose of this document is to identify the different types of enterprise API requirements that should be included in the evaluation dataset.

The evaluation dataset should not contain only one type of API requirement. It should include a balanced set of requirement categories so the AI workflow generation system can be tested across different enterprise API patterns.

---

## Why Requirement Categories Are Needed

Enterprise API requirements can vary widely.

Some APIs only validate input and return a simple response.

Other APIs may:

* call downstream services
* perform database lookups
* apply business rules
* transform responses
* handle complex error scenarios
* include ambiguous or incomplete requirement sections

If the evaluation dataset contains only simple APIs, the system may look accurate even though it cannot handle real enterprise complexity.

Requirement categories help ensure that the dataset tests different workflow generation capabilities.

---

## Dataset Category Goals

The sample requirement categories should help test whether the AI system can:

1. Extract endpoint information correctly.
2. Identify required and optional headers.
3. Extract request body fields.
4. Capture validation rules.
5. Map business rules into workflow steps.
6. Identify downstream service calls.
7. Identify database interactions.
8. Preserve correct workflow step order.
9. Map success and error responses.
10. Avoid hallucinated logic.
11. Handle ambiguous requirement sections carefully.
12. Produce artifact-ready workflow output.

---

## Category Summary

| Category ID | Category Name                     | Complexity     |
| ----------- | --------------------------------- | -------------- |
| CAT-001     | Validation-Only API               | Low            |
| CAT-002     | Simple Create API                 | Low to Medium  |
| CAT-003     | Database Lookup API               | Medium         |
| CAT-004     | Downstream REST Wrapper API       | Medium         |
| CAT-005     | Response Transformation API       | Medium         |
| CAT-006     | Business Rule Branching API       | Medium to High |
| CAT-007     | Multi-Step Orchestration API      | High           |
| CAT-008     | Calculation-Based API             | High           |
| CAT-009     | Error-Handling-Heavy API          | Medium to High |
| CAT-010     | Ambiguous Requirement API         | Medium to High |
| CAT-011     | Existing System Modernization API | High           |
| CAT-012     | Mixed Enterprise API              | High           |

---

## CAT-001 — Validation-Only API

## Description

A validation-only API focuses mainly on request validation.

It does not require complex business logic, database access, or downstream service calls.

This category is useful for testing whether the AI can correctly extract basic API contract information.

---

## What This Category Tests

This category tests:

* endpoint extraction
* required header extraction
* optional header extraction
* request body extraction
* required field validation
* enum validation
* data type validation
* simple response mapping
* validation error handling

---

## Typical Requirement Pattern

```text
The API must validate required headers and request body fields.
If any required field is missing, return a validation error.
If all fields are valid, return a success response.
```

---

## Example

```text
POST /api/v1/user/preferences

Required headers:
Client-Id
Subclient-Id
Trace-Id

Request body:
userId required string
preferenceType required enum EMAIL, SMS, PUSH
enabled required boolean

Rules:
Reject the request if required headers are missing.
Reject the request if preferenceType is not valid.
Return success if validation passes.
```

---

## Expected Workflow Pattern

```text
1. Receive request
2. Validate required headers
3. Validate request body
4. Validate enum fields
5. Return success response
6. Return validation error if validation fails
```

---

## CAT-002 — Simple Create API

## Description

A simple create API accepts request data and creates a new record.

This category may include database insert behavior but has limited business logic.

---

## What This Category Tests

This category tests:

* endpoint extraction
* request body mapping
* database insert mapping
* generated ID response mapping
* basic error handling
* artifact readiness for Excel and Go generation later

---

## Typical Requirement Pattern

```text
The API must validate the request and insert a new record into a table.
After successful insert, return the generated identifier.
```

---

## Example

```text
POST /api/v1/clienteling/messages

The API creates an application message.
Required body fields are title, message, messageSeverity, and startDateTime.
The API inserts the message into APP_MSGS and returns the generated id.
```

---

## Expected Workflow Pattern

```text
1. Receive request
2. Validate headers
3. Validate request body
4. Build database insert request
5. Insert record
6. Map generated id to response
7. Return success or database error
```

---

## CAT-003 — Database Lookup API

## Description

A database lookup API retrieves data from a database based on request inputs.

This category is useful for testing database query mapping.

---

## What This Category Tests

This category tests:

* query purpose extraction
* table name extraction
* input parameter mapping
* output field mapping
* no-record handling
* database error handling
* response mapping from database result

---

## Typical Requirement Pattern

```text
The API must validate the request, query a database table, and return matching data.
If no record exists, return a not-found or empty response based on the requirement.
```

---

## Example

```text
GET /api/v1/customer/profile

Required query parameter:
customerId

Database:
Query CUSTOMER_PROFILE using customerId.

Response:
Return customer name, status, and loyalty tier.
```

---

## Expected Workflow Pattern

```text
1. Receive request
2. Validate headers
3. Validate query parameters
4. Query database
5. Handle no-record scenario
6. Map database result to response
7. Return success or error response
```

---

## CAT-004 — Downstream REST Wrapper API

## Description

A downstream REST wrapper API receives an API request, maps it into a downstream request, calls an external service, and maps the downstream response back to the client.

This category is important because many enterprise APIs act as wrappers around internal services.

---

## What This Category Tests

This category tests:

* downstream service identification
* downstream endpoint mapping
* downstream request body mapping
* downstream header propagation
* authentication or signature rule extraction
* downstream response mapping
* downstream error handling

---

## Typical Requirement Pattern

```text
The API must validate the request, call a downstream REST service, map the response, and return the final API response.
```

---

## Example

```text
POST /api/v1/account/interrogate

The API receives encrypted account information.
It validates required headers and request body fields.
It calls the downstream PIID lookup service.
The downstream response is mapped to accountInfo in the final response.
```

---

## Expected Workflow Pattern

```text
1. Receive request
2. Validate headers
3. Validate request body
4. Build downstream request
5. Propagate required headers
6. Call downstream REST service
7. Validate downstream response
8. Map downstream response to final API response
9. Handle downstream errors
```

---

## CAT-005 — Response Transformation API

## Description

A response transformation API focuses on converting data from one structure into another.

The source may be a downstream response, database result, or internal business object.

---

## What This Category Tests

This category tests:

* source-to-target field mapping
* nested response mapping
* field renaming
* field type conversion
* default value handling
* response filtering
* avoiding unsupported response fields

---

## Typical Requirement Pattern

```text
The API must call a source system or read data, then transform the response into the required API response format.
```

---

## Example

```text
The downstream service returns:
fabricType
fabric
color

The API response must return:
result.fabricType
result.fabric
result.color

If downstream errors exist, return errors array.
```

---

## Expected Workflow Pattern

```text
1. Receive request
2. Validate request
3. Retrieve source response
4. Extract required fields
5. Transform field names and structure
6. Remove unsupported fields
7. Return final response
8. Handle mapping errors
```

---

## CAT-006 — Business Rule Branching API

## Description

A business rule branching API contains conditional logic that changes the workflow path.

This category is important for testing whether AI can preserve decision-making logic.

---

## What This Category Tests

This category tests:

* business rule extraction
* condition extraction
* branching workflow representation
* reject conditions
* alternate paths
* rule ordering
* traceability to business requirements

---

## Typical Requirement Pattern

```text
If condition A is true, follow path A.
If condition B is true, follow path B.
If neither condition is true, reject or return default behavior.
```

---

## Example

```text
If account is an employee account, apply employee discount rules.
If account is a new account and eligible, apply new account discount rules.
If neither condition applies, return no discount.
```

---

## Expected Workflow Pattern

```text
1. Receive request
2. Validate request
3. Retrieve account details
4. Check employee account condition
5. If employee, apply employee path
6. Else check new account condition
7. If new account, apply new account path
8. Else return no discount
9. Map final response
```

---

## CAT-007 — Multi-Step Orchestration API

## Description

A multi-step orchestration API performs several operations across validations, downstream services, database checks, and business logic.

This category is used for high-complexity enterprise workflows.

---

## What This Category Tests

This category tests:

* multi-step workflow generation
* correct step sequencing
* dependency between steps
* downstream and database coordination
* complex response construction
* failure path handling
* artifact readiness

---

## Typical Requirement Pattern

```text
The API must validate the request, call one or more services, query one or more tables, apply business rules, and return a mapped response.
```

---

## Example

```text
Evaluate transaction discount:
Validate headers and request body.
Call account interrogation service.
Determine account type.
Query discount rule tables.
Check exclusions.
Apply discount calculation.
Return item-level and transaction-level discount details.
```

---

## Expected Workflow Pattern

```text
1. Receive request
2. Validate headers
3. Validate body
4. Call account service
5. Validate account response
6. Query base rules
7. Query exclusions
8. Query additional rules
9. Apply business calculations
10. Build final response
11. Handle validation, service, database, and business errors
```

---

## CAT-008 — Calculation-Based API

## Description

A calculation-based API applies formulas or numerical business logic.

This category is useful for testing whether the AI can preserve calculation rules accurately.

---

## What This Category Tests

This category tests:

* formula extraction
* percentage calculation
* cap or threshold logic
* rounding logic
* ordering of calculations
* conditional calculation paths
* response mapping of calculated values

---

## Typical Requirement Pattern

```text
The API must calculate a value based on input fields, business rules, limits, and thresholds.
```

---

## Example

```text
Calculate discount amount using item price and discount percentage.
Apply cap amount if the calculated discount exceeds the allowed maximum.
Round final discount amount to two decimal places.
```

---

## Expected Workflow Pattern

```text
1. Receive request
2. Validate input values
3. Retrieve calculation rules
4. Apply base calculation
5. Apply cap or threshold
6. Apply rounding
7. Map calculated values to response
8. Handle invalid calculation inputs
```

---

## CAT-009 — Error-Handling-Heavy API

## Description

An error-handling-heavy API has several defined failure scenarios.

This category is useful for testing whether the workflow captures more than the happy path.

---

## What This Category Tests

This category tests:

* validation error handling
* business error handling
* downstream service error handling
* database error handling
* timeout handling
* no-record handling
* error response mapping
* HTTP status mapping

---

## Typical Requirement Pattern

```text
The API must return specific error responses for validation failures, business failures, downstream failures, and database failures.
```

---

## Example

```text
If required header is missing, return HTTP 400.
If downstream service returns 401, return unauthorized error.
If downstream service times out, return service unavailable.
If database insert fails, return internal server error.
```

---

## Expected Workflow Pattern

```text
1. Receive request
2. Validate input
3. Return validation error if invalid
4. Call downstream or database
5. Handle downstream failure
6. Handle database failure
7. Handle business rejection
8. Map error response
9. Return success only if all required steps pass
```

---

## CAT-010 — Ambiguous Requirement API

## Description

An ambiguous requirement API contains unclear, incomplete, or conflicting requirement details.

This category is useful for testing whether the AI avoids guessing unsupported logic.

---

## What This Category Tests

This category tests:

* ambiguity detection
* assumption control
* review note generation
* traceability
* hallucination prevention
* safe workflow generation

---

## Typical Requirement Pattern

```text
The requirement provides partial information but does not fully define all fields, errors, or mappings.
```

---

## Example

```text
The API should call the customer service and return customer details.
Headers are required as per standard API rules.
Errors should be handled appropriately.
```

---

## Expected Workflow Pattern

```text
1. Receive request
2. Capture only clearly stated details
3. Mark unclear headers as ambiguous
4. Mark unclear response mapping as ambiguous
5. Mark unclear error handling as ambiguous
6. Avoid inventing unsupported fields or rules
7. Flag requirement for human review
```

---

## CAT-011 — Existing System Modernization API

## Description

An existing system modernization API involves converting an older implementation or legacy workflow into a modern API workflow.

This category is aligned with the broader research goal of enterprise software modernization.

---

## What This Category Tests

This category tests:

* legacy-to-modern workflow interpretation
* preservation of business behavior
* mapping from existing flow to structured workflow
* identifying reusable business logic
* detecting missing modern API contract details
* artifact generation readiness

---

## Typical Requirement Pattern

```text
The current system performs a legacy workflow.
The new API should preserve the same behavior while exposing a modern REST contract.
```

---

## Example

```text
A legacy SOAP service receives transfer details and returns return code, message, transfer number, and carton number.
The new REST wrapper must call the SOAP service, map the SOAP response, and return a REST response.
```

---

## Expected Workflow Pattern

```text
1. Receive REST request
2. Validate REST input
3. Map REST request to legacy request format
4. Call legacy service
5. Parse legacy response
6. Map legacy output to REST response
7. Preserve return code and message behavior
8. Handle legacy service errors
```

---

## CAT-012 — Mixed Enterprise API

## Description

A mixed enterprise API combines multiple patterns into one requirement.

It may include validation, downstream calls, database checks, business rules, calculations, and response transformation.

This is the most realistic and difficult category.

---

## What This Category Tests

This category tests the complete system.

It evaluates whether AI can generate a workflow that is:

* complete
* accurate
* traceable
* correctly ordered
* free of hallucinated logic
* ready for downstream artifact generation

---

## Typical Requirement Pattern

```text
The API must validate input, call downstream services, query tables, apply business logic, perform calculations, transform the response, and handle errors.
```

---

## Example

```text
Evaluate discount for a transaction:
Validate required headers and body fields.
Call account interrogation service.
Identify employee or new account status.
Query discount profile tables.
Check exclusions.
Apply base and additional discount rules.
Apply cap logic.
Return transaction-level and item-level discount response.
```

---

## Expected Workflow Pattern

```text
1. Receive request
2. Validate headers
3. Validate body
4. Validate enum fields
5. Call downstream account service
6. Validate downstream response
7. Query required database tables
8. Apply business branching
9. Apply calculation rules
10. Apply response transformation
11. Return success response
12. Return validation, business, downstream, or database errors
```

---

## Recommended Dataset Distribution

For the first evaluation dataset, the recommended target is 10 to 15 dataset samples.

A balanced 12-sample dataset can be organized as follows:

| Category                          | Number of Samples |
| --------------------------------- | ----------------: |
| Validation-Only API               |                 1 |
| Simple Create API                 |                 1 |
| Database Lookup API               |                 1 |
| Downstream REST Wrapper API       |                 2 |
| Response Transformation API       |                 1 |
| Business Rule Branching API       |                 1 |
| Multi-Step Orchestration API      |                 1 |
| Calculation-Based API             |                 1 |
| Error-Handling-Heavy API          |                 1 |
| Ambiguous Requirement API         |                 1 |
| Existing System Modernization API |                 1 |
| Mixed Enterprise API              |                 1 |

This gives enough variety without making the dataset too large for the 50-day challenge.

---

## Dataset Difficulty Progression

The dataset should move from simple to complex examples.

Recommended progression:

```text
Level 1:
Validation-only API
Simple create API

Level 2:
Database lookup API
Downstream REST wrapper API
Response transformation API

Level 3:
Business rule branching API
Calculation-based API
Error-handling-heavy API

Level 4:
Multi-step orchestration API
Existing system modernization API
Mixed enterprise API

Level 5:
Ambiguous requirement API
```

This progression helps evaluate how the system performs as complexity increases.

---

## How Categories Connect to Workflow Quality Metrics

Each category should test different quality metrics.

| Category                          | Primary Metrics Tested                               |
| --------------------------------- | ---------------------------------------------------- |
| Validation-Only API               | Validation Accuracy, Requirement Coverage            |
| Simple Create API                 | Step Completeness, Database Mapping                  |
| Database Lookup API               | Database Mapping, Response Mapping                   |
| Downstream REST Wrapper API       | Downstream Mapping, Response Mapping                 |
| Response Transformation API       | Response Mapping, Hallucination Control              |
| Business Rule Branching API       | Business Rule Coverage, Step Ordering                |
| Multi-Step Orchestration API      | Step Completeness, Step Ordering, Artifact Readiness |
| Calculation-Based API             | Business Rule Coverage, Response Mapping             |
| Error-Handling-Heavy API          | Error Handling Completeness                          |
| Ambiguous Requirement API         | Hallucination Control, Traceability                  |
| Existing System Modernization API | Requirement Coverage, Downstream Mapping             |
| Mixed Enterprise API              | Overall Workflow Quality                             |

---

## How Categories Connect to Failure Analysis

The categories also help identify where the AI system struggles.

Examples:

| Category                          | Common Failure Risk                   |
| --------------------------------- | ------------------------------------- |
| Validation-Only API               | Missing enum validation               |
| Simple Create API                 | Missing generated ID response mapping |
| Database Lookup API               | Wrong table or no-record behavior     |
| Downstream REST Wrapper API       | Incorrect downstream request mapping  |
| Response Transformation API       | Returning unsupported fields          |
| Business Rule Branching API       | Missing alternate path                |
| Multi-Step Orchestration API      | Wrong step order                      |
| Calculation-Based API             | Incorrect cap or rounding logic       |
| Error-Handling-Heavy API          | Happy-path-only workflow              |
| Ambiguous Requirement API         | Hallucinated assumptions              |
| Existing System Modernization API | Losing legacy behavior                |
| Mixed Enterprise API              | Incomplete orchestration              |

---

## Dataset Selection Rules

When selecting sample requirements for the dataset, follow these rules:

1. Select requirements that represent real enterprise API patterns.
2. Include both simple and complex examples.
3. Avoid duplicate scenarios that test the same behavior.
4. Include at least one ambiguous requirement sample.
5. Include at least one downstream wrapper sample.
6. Include at least one database interaction sample.
7. Include at least one business rule branching sample.
8. Include at least one error-heavy sample.
9. Include at least one mixed enterprise orchestration sample.
10. Ensure each sample has enough detail to create a ground truth workflow.

---

## What Makes a Good Dataset Sample

A good dataset sample should have:

* clear endpoint information
* clear request structure
* identifiable validation rules
* identifiable response structure
* clear expected workflow behavior
* enough complexity to test the AI system
* source traceability
* review notes for unclear areas

A good sample does not need to be large, but it should be complete enough to evaluate workflow generation.

---

## What Makes a Weak Dataset Sample

A weak dataset sample may have:

* no clear endpoint
* no request or response details
* no business logic
* no expected behavior
* too much missing information
* no ground truth workflow
* duplicated logic already covered by another sample
* confidential information that cannot be committed to GitHub

Weak samples should not be used for formal evaluation unless they are intentionally included as ambiguous requirement samples.

---

## Recommended Initial Dataset Candidates

Based on this project, the first dataset can include sanitized versions of the following patterns:

| Candidate                               | Category                          |
| --------------------------------------- | --------------------------------- |
| Create Application Message API          | Simple Create API                 |
| Application Message Overlap Check API   | Business Rule Branching API       |
| Account Interrogation API               | Downstream REST Wrapper API       |
| Evaluate Transaction Discount API       | Mixed Enterprise API              |
| Fabric Types API                        | Downstream REST Wrapper API       |
| Colors API                              | Response Transformation API       |
| SOAP Transfer Wrapper API               | Existing System Modernization API |
| Customer Profile Lookup API             | Database Lookup API               |
| Notification Preference API             | Validation-Only API               |
| Discount Calculation API                | Calculation-Based API             |
| Service Failure Handling API            | Error-Handling-Heavy API          |
| Incomplete Customer Service Requirement | Ambiguous Requirement API         |

These candidates should be sanitized and rewritten as generic enterprise examples before being added to the evaluation dataset.

---

## Category Review Checklist

Before adding a sample to the dataset, answer the following:

| Check                                         | Answer |
| --------------------------------------------- | ------ |
| What category does this sample belong to?     |        |
| What capability does this sample test?        |        |
| Is the requirement realistic?                 |        |
| Is the source requirement clear enough?       |        |
| Are ambiguous areas marked?                   |        |
| Can a ground truth workflow be created?       |        |
| Does this sample duplicate another sample?    |        |
| Is the sample sanitized?                      |        |
| Is the sample useful for research evaluation? |        |

---

## How This Supports the Research Project

The sample requirement categories help make the evaluation dataset stronger.

They ensure that the AI workflow generation system is not tested only on easy cases.

The categories support the research by allowing the project to report results by requirement type.

Example:

```text
The system achieved 92% average workflow quality on validation-only APIs,
86% on downstream wrapper APIs,
and 74% on mixed enterprise orchestration APIs.
```

This kind of category-based reporting makes the research more useful and honest.

It can show where the system performs well and where improvement is still needed.

---

## Summary

This document defines the sample requirement categories for the workflow evaluation dataset.

The dataset should include a balanced set of enterprise API requirement patterns, including:

* validation-only APIs
* simple create APIs
* database lookup APIs
* downstream REST wrapper APIs
* response transformation APIs
* business rule branching APIs
* multi-step orchestration APIs
* calculation-based APIs
* error-handling-heavy APIs
* ambiguous requirement APIs
* existing system modernization APIs
* mixed enterprise APIs

These categories will help evaluate the AI system across realistic enterprise API scenarios.

Day 18 Phase 2 establishes the category structure needed to create a meaningful, balanced, and research-ready workflow evaluation dataset.
