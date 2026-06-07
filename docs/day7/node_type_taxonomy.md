# Day 7 — Node Type Taxonomy

## Purpose

This document defines the workflow node type taxonomy for the 50-Day AI Software Engineer Challenge.

The purpose of this taxonomy is to identify the common types of workflow steps that appear in enterprise API development and represent them as reusable node categories.

These node types will later help generate:

* Node-RED flows
* Excel intake mappings
* Go service structures
* API workflow documentation

No implementation or coding is performed on Day 7.

---

## What Is a Workflow Node Type?

A workflow node type represents a category of action within an API execution flow.

Each node type describes what kind of work a step performs.

Examples include:

* Receiving an HTTP request
* Validating headers
* Validating request body fields
* Applying business rules
* Calling a downstream service
* Executing a database query
* Mapping a response
* Returning an HTTP response

The node type helps the system understand how each step should behave and how it may later be converted into a Node-RED node, Go code layer, or Excel row.

---

## Why Node Type Taxonomy Is Needed

Requirement documents usually describe API behavior in mixed natural language.

For example:

```text
The service must validate Division-Id and Store-Number headers before calling the downstream API.
```

This sentence contains multiple workflow actions:

1. Validate `Division-Id`
2. Validate `Store-Number`
3. Call downstream API

A node type taxonomy allows the system to split this requirement into clear workflow steps.

---

## Core Node Types

The following node types represent the standard categories used in the workflow representation layer.

---

## 1. HTTP_INPUT

### Description

Represents the API entry point where the request enters the system.

### Responsibilities

* Define HTTP method
* Define endpoint path
* Capture request headers
* Capture path parameters
* Capture query parameters
* Capture request body

### Example

```json
{
  "node_type": "HTTP_INPUT",
  "step_name": "Receive API Request",
  "description": "Receives POST request for the API endpoint."
}
```

### Node-RED Equivalent

```text
http in node
```

### Go Equivalent

```text
Handler route registration
```

---

## 2. HEADER_VALIDATION

### Description

Represents validation of required or optional request headers.

### Responsibilities

* Check mandatory headers
* Validate header data types
* Validate allowed values
* Normalize header names
* Store validated headers in request context

### Example

```json
{
  "node_type": "HEADER_VALIDATION",
  "step_name": "Validate Required Headers",
  "description": "Validates that Division-Id, Store-Number, and Trace-Id headers are present."
}
```

### Node-RED Equivalent

```text
function node
```

### Go Equivalent

```text
Handler validation / middleware
```

---

## 3. BODY_VALIDATION

### Description

Represents validation of request body fields.

### Responsibilities

* Check required body fields
* Validate field data types
* Validate nested objects
* Validate array structures
* Validate minimum and maximum constraints

### Example

```json
{
  "node_type": "BODY_VALIDATION",
  "step_name": "Validate Request Body",
  "description": "Validates that required request body fields are present and correctly formatted."
}
```

### Node-RED Equivalent

```text
function node
```

### Go Equivalent

```text
DTO validation / request binding
```

---

## 4. ENUM_VALIDATION

### Description

Represents validation of fields that must match predefined allowed values.

### Responsibilities

* Validate enum fields
* Reject unsupported values
* Return clear validation errors
* Preserve allowed values for documentation

### Example

```json
{
  "node_type": "ENUM_VALIDATION",
  "step_name": "Validate Enum Fields",
  "description": "Validates that messageSeverity contains one of the allowed values."
}
```

### Node-RED Equivalent

```text
function node
```

### Go Equivalent

```text
Validation package / DTO validation
```

---

## 5. REQUEST_TRANSFORMATION

### Description

Represents transformation of incoming request data into an internal structure or downstream request format.

### Responsibilities

* Rename fields
* Convert data types
* Build downstream request payloads
* Apply padding or formatting rules
* Combine headers and body fields into a service request

### Example

```json
{
  "node_type": "REQUEST_TRANSFORMATION",
  "step_name": "Build Downstream Request",
  "description": "Transforms the incoming API request into the downstream service request format."
}
```

### Node-RED Equivalent

```text
function node
```

### Go Equivalent

```text
Mapper layer / service request builder
```

---

## 6. BUSINESS_RULE

### Description

Represents business logic that must be evaluated before, during, or after service execution.

### Responsibilities

* Apply domain-specific conditions
* Check eligibility rules
* Compare values
* Determine processing path
* Decide whether to continue or reject the request

### Example

```json
{
  "node_type": "BUSINESS_RULE",
  "step_name": "Check Active Message Overlap",
  "description": "Checks whether an active message already exists for the same division and overlapping time window."
}
```

### Node-RED Equivalent

```text
function node
```

### Go Equivalent

```text
Service layer
```

---

## 7. DATABASE_QUERY

### Description

Represents a read operation against a database.

### Responsibilities

* Query reference data
* Check existing records
* Fetch configuration values
* Retrieve business rule data
* Return query results to the workflow context

### Example

```json
{
  "node_type": "DATABASE_QUERY",
  "step_name": "Check Existing Discount Rule",
  "description": "Queries the database to determine whether a matching discount rule exists."
}
```

### Node-RED Equivalent

```text
database node / function node
```

### Go Equivalent

```text
Repository layer
```

---

## 8. DATABASE_COMMAND

### Description

Represents a database write operation.

### Responsibilities

* Insert records
* Update records
* Delete records
* Return generated IDs
* Handle database write failures

### Example

```json
{
  "node_type": "DATABASE_COMMAND",
  "step_name": "Insert Application Message",
  "description": "Inserts a new application message record into the database and returns the generated ID."
}
```

### Node-RED Equivalent

```text
database node / function node
```

### Go Equivalent

```text
Repository layer
```

---

## 9. EXTERNAL_SERVICE_CALL

### Description

Represents a call to an external REST, SOAP, GraphQL, or internal service.

### Responsibilities

* Prepare request headers
* Prepare request payload
* Invoke downstream service
* Capture status code
* Capture response body
* Handle timeout or service failure

### Example

```json
{
  "node_type": "EXTERNAL_SERVICE_CALL",
  "step_name": "Call Account Interrogation Service",
  "description": "Calls the downstream account interrogation service to retrieve account details."
}
```

### Node-RED Equivalent

```text
http request node / reusable subflow
```

### Go Equivalent

```text
Client layer
```

---

## 10. RESPONSE_MAPPING

### Description

Represents mapping of internal, database, or downstream response data into the final API response format.

### Responsibilities

* Select response fields
* Rename fields
* Flatten or nest response objects
* Convert data types
* Build success response payload

### Example

```json
{
  "node_type": "RESPONSE_MAPPING",
  "step_name": "Map Final Response",
  "description": "Maps service results into the final response returned to the API consumer."
}
```

### Node-RED Equivalent

```text
function node
```

### Go Equivalent

```text
Mapper layer / service layer
```

---

## 11. ERROR_HANDLING

### Description

Represents error detection, error normalization, and error response construction.

### Responsibilities

* Catch validation failures
* Catch database errors
* Catch downstream service errors
* Map internal errors to HTTP status codes
* Build standardized error responses

### Example

```json
{
  "node_type": "ERROR_HANDLING",
  "step_name": "Handle Downstream Error",
  "description": "Maps downstream service failures into a standardized API error response."
}
```

### Node-RED Equivalent

```text
catch node / function node
```

### Go Equivalent

```text
Error package / middleware
```

---

## 12. HTTP_RESPONSE

### Description

Represents the final response returned to the API client.

### Responsibilities

* Set HTTP status code
* Set response headers
* Return success body
* Return error body
* End request processing

### Example

```json
{
  "node_type": "HTTP_RESPONSE",
  "step_name": "Return API Response",
  "description": "Returns the final success or error response to the API caller."
}
```

### Node-RED Equivalent

```text
http response node
```

### Go Equivalent

```text
Handler response writer
```

---

## Recommended Standard Execution Order

A typical workflow should follow this order:

```text
HTTP_INPUT
    ↓
HEADER_VALIDATION
    ↓
BODY_VALIDATION
    ↓
ENUM_VALIDATION
    ↓
REQUEST_TRANSFORMATION
    ↓
BUSINESS_RULE
    ↓
DATABASE_QUERY / EXTERNAL_SERVICE_CALL
    ↓
DATABASE_COMMAND
    ↓
RESPONSE_MAPPING
    ↓
ERROR_HANDLING
    ↓
HTTP_RESPONSE
```

---

## Node Type Summary Table

| Node Type                | Main Purpose                      | Node-RED Equivalent  | Go Equivalent              |
| ------------------------ | --------------------------------- | -------------------- | -------------------------- |
| `HTTP_INPUT`             | Receive API request               | `http in`            | Handler route              |
| `HEADER_VALIDATION`      | Validate request headers          | `function`           | Handler / middleware       |
| `BODY_VALIDATION`        | Validate request body             | `function`           | DTO validation             |
| `ENUM_VALIDATION`        | Validate allowed values           | `function`           | Validation package         |
| `REQUEST_TRANSFORMATION` | Build internal/downstream request | `function`           | Mapper / service builder   |
| `BUSINESS_RULE`          | Apply business logic              | `function`           | Service layer              |
| `DATABASE_QUERY`         | Read from database                | Database node        | Repository layer           |
| `DATABASE_COMMAND`       | Write to database                 | Database node        | Repository layer           |
| `EXTERNAL_SERVICE_CALL`  | Call downstream service           | HTTP node / subflow  | Client layer               |
| `RESPONSE_MAPPING`       | Build final response              | `function`           | Mapper / service layer     |
| `ERROR_HANDLING`         | Normalize errors                  | `catch` / `function` | Error package / middleware |
| `HTTP_RESPONSE`          | Return response                   | `http response`      | Handler response writer    |

---

## Design Notes

The taxonomy should remain stable and reusable across different API types.

It should support:

* Simple CRUD APIs
* API wrappers
* SOAP wrapper flows
* REST orchestration flows
* Database-driven APIs
* Business-rule-heavy enterprise APIs
* Future Node-RED flow generation
* Future Go code generation

---

## What This Taxonomy Does Not Define

This document does not define:

* Actual Node-RED JSON
* Go source code
* Database schema
* Runtime configuration
* LLM prompt execution
* Test cases

Those activities will be handled in later days.

---

## Summary

The node type taxonomy provides a reusable vocabulary for describing API workflow behavior.

It helps convert natural language requirements into structured execution steps.

This taxonomy will be used by later phases to support workflow generation, Node-RED flow creation, Excel intake preparation, and Go code generation.
