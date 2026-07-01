# Day 31 — API Integration Plan

## Purpose

The purpose of Day 31 is to define how the API Factory UI will communicate with backend services.

Until now, the frontend has focused on page structure, layout, forms, and user experience. Day 31 introduces the integration layer that allows the UI to send user input to backend APIs and display generated factory outputs.

This document explains:

* Which UI areas require backend API calls
* What data will be sent from the frontend
* What responses are expected from the backend
* How loading, success, and error states should behave
* How API configuration should be managed

---

## Current Project Context

The API Factory UI is part of the larger API Factory Generator project.

The long-term goal of the project is to help convert enterprise API requirements into structured outputs such as:

* API endpoint definitions
* Request and response schemas
* Workflow steps
* Node-RED flow definitions
* Excel factory intake files
* Gap analysis reports
* Generated implementation artifacts

The UI acts as the entry point for users to provide requirement information and view generated results.

---

## Day 31 Scope

Day 31 focuses only on frontend-to-backend API integration planning and setup.

The goal is not to finalize the full factory generation system. Instead, the goal is to create a clean and reusable integration foundation so future days can connect additional backend services without duplicating logic.

---

## Integration Goals

The frontend should be able to:

1. Accept requirement input from the user.
2. Submit the requirement data to a backend API.
3. Show a loading state while the backend processes the request.
4. Display the structured backend response.
5. Display meaningful error messages when the backend fails.
6. Use environment-based configuration for the backend base URL.
7. Keep API logic reusable and separate from UI components.

---

## Primary UI Areas Requiring API Integration

### 1. Requirement Input Page

This page allows the user to enter or paste requirement details.

Example inputs may include:

* API name
* Business description
* Endpoint path
* HTTP method
* Request headers
* Request body
* Expected response
* Downstream system details
* Database interaction notes
* Business rules
* Error handling rules

The requirement input page must submit this information to the backend.

---

### 2. Factory Output Page

This page displays the generated output returned from the backend.

The backend response may include:

* Parsed API requirement summary
* Endpoint metadata
* Request schema
* Response schema
* Workflow steps
* Validation rules
* Database mapping
* Error scenarios
* Recommended next actions

The frontend should display the response in a readable format.

---

### 3. Status and Error Display Area

The UI should clearly show the current state of the backend request.

Supported states:

| State   | Description                                     |
| ------- | ----------------------------------------------- |
| Idle    | No request has been submitted yet               |
| Loading | Request is being sent or backend is processing  |
| Success | Backend returned a valid response               |
| Error   | Backend request failed or returned invalid data |

---

## Proposed Backend API Endpoint

For Day 31, the frontend should be designed around a single backend integration endpoint.

```http
POST /api/factory/requirements/analyze
```

This endpoint will receive requirement details from the UI and return structured factory output.

---

## Request Payload Structure

The frontend should send a JSON payload similar to the following:

```json
{
  "apiName": "string",
  "description": "string",
  "method": "POST",
  "endpoint": "/api/v1/example",
  "headers": [
    {
      "name": "string",
      "required": true,
      "description": "string"
    }
  ],
  "requestBody": "string",
  "responseBody": "string",
  "businessRules": "string",
  "databaseNotes": "string",
  "downstreamNotes": "string",
  "errorHandlingNotes": "string"
}
```

---

## Request Field Definitions

| Field              |   Type | Required | Description                                          |
| ------------------ | -----: | -------: | ---------------------------------------------------- |
| apiName            | string |      Yes | Name of the API being created or analyzed            |
| description        | string |      Yes | Business-level explanation of the API purpose        |
| method             | string |      Yes | HTTP method such as GET, POST, PUT, PATCH, or DELETE |
| endpoint           | string |      Yes | API endpoint path                                    |
| headers            |  array |       No | List of expected request headers                     |
| requestBody        | string |       No | Raw request body or request example                  |
| responseBody       | string |       No | Raw response body or response example                |
| businessRules      | string |       No | Business validations and decision rules              |
| databaseNotes      | string |       No | Database table, insert, update, or lookup details    |
| downstreamNotes    | string |       No | Downstream API or service interaction details        |
| errorHandlingNotes | string |       No | Error mapping and failure behavior                   |

---

## Expected Backend Response Structure

The backend should return a structured JSON response.

```json
{
  "success": true,
  "message": "Requirement analysis completed successfully",
  "data": {
    "apiSummary": {
      "apiName": "string",
      "method": "POST",
      "endpoint": "/api/v1/example",
      "description": "string"
    },
    "workflowSteps": [
      {
        "stepNumber": 1,
        "stepName": "Validate Headers",
        "stepType": "validation",
        "description": "Validate all required request headers"
      }
    ],
    "requestSchema": {},
    "responseSchema": {},
    "businessRules": [],
    "databaseMappings": [],
    "errorScenarios": [],
    "nextActions": []
  }
}
```

---

## Response Field Definitions

| Field            |    Type | Description                                                      |
| ---------------- | ------: | ---------------------------------------------------------------- |
| success          | boolean | Indicates whether the backend successfully processed the request |
| message          |  string | Human-readable status message                                    |
| data             |  object | Structured factory output                                        |
| apiSummary       |  object | High-level API information                                       |
| workflowSteps    |   array | Ordered steps required to implement the API                      |
| requestSchema    |  object | Structured request schema generated from the input               |
| responseSchema   |  object | Structured response schema generated from the input              |
| businessRules    |   array | Extracted business rules                                         |
| databaseMappings |   array | Database interactions identified from the requirement            |
| errorScenarios   |   array | Expected errors and mappings                                     |
| nextActions      |   array | Suggested follow-up implementation steps                         |

---

## Frontend API Client Strategy

The frontend should not call `fetch` directly inside every component.

Instead, Day 31 should introduce a reusable API client layer.

Recommended location:

```text
src/lib/apiClient.ts
```

The API client should be responsible for:

* Reading the backend base URL
* Building full endpoint URLs
* Setting request headers
* Sending JSON payloads
* Parsing JSON responses
* Handling HTTP errors
* Returning clean success or error objects to the UI

---

## Environment Configuration

The backend base URL should be configured using an environment variable.

Recommended file:

```text
.env.local
```

Recommended variable:

```env
NEXT_PUBLIC_API_BASE_URL=http://localhost:8080
```

The frontend should never hardcode backend URLs directly inside components.

Correct usage:

```ts
const baseUrl = process.env.NEXT_PUBLIC_API_BASE_URL;
```

---

## Error Handling Plan

The UI should handle the following error types:

### 1. Network Error

Occurs when the backend is not running or unreachable.

Example UI message:

```text
Unable to connect to the backend service. Please check whether the backend is running.
```

---

### 2. Validation Error

Occurs when required fields are missing or invalid.

Example UI message:

```text
Please complete the required fields before submitting.
```

---

### 3. Backend Processing Error

Occurs when the backend receives the request but cannot process it.

Example UI message:

```text
The backend could not analyze the requirement. Please review the input and try again.
```

---

### 4. Unexpected Response Error

Occurs when the backend response does not match the expected format.

Example UI message:

```text
The backend returned an unexpected response format.
```

---

## Loading State Plan

When the user submits the requirement form:

1. Disable the submit button.
2. Show a loading indicator.
3. Prevent duplicate submissions.
4. Clear previous errors.
5. Keep the user input visible.
6. Display the backend response after completion.

Example button states:

| State   | Button Text         |
| ------- | ------------------- |
| Idle    | Analyze Requirement |
| Loading | Analyzing...        |
| Success | Analyze Requirement |
| Error   | Try Again           |

---

## Success State Plan

When the backend returns a successful response, the frontend should display:

* Success message
* API summary
* Workflow steps
* Extracted business rules
* Database mappings
* Error scenarios
* Next actions

The response should be readable and easy to review.

For Day 31, JSON display is acceptable.

Future improvements can include:

* Cards
* Tables
* Step-by-step workflow visualization
* Download as Markdown
* Export as Excel
* Generate Node-RED flow

---

## Component Responsibilities

### Requirement Form Component

Responsible for:

* Capturing user input
* Validating required fields
* Calling the API client
* Managing loading and error states
* Passing response data to output component

---

### Factory Output Component

Responsible for:

* Displaying successful backend response
* Showing structured output
* Rendering workflow steps
* Rendering JSON when detailed formatting is not yet available

---

### Error Message Component

Responsible for:

* Displaying user-friendly error messages
* Keeping technical details hidden unless needed for debugging

---

## Recommended Folder Structure

```text
src/
  app/
    page.tsx
  components/
    RequirementForm.tsx
    FactoryOutput.tsx
    ErrorMessage.tsx
  lib/
    apiClient.ts
  types/
    factory.ts
```

---

## TypeScript Type Planning

The frontend should define shared TypeScript types for request and response structures.

Recommended file:

```text
src/types/factory.ts
```

Recommended types:

```ts
export type RequirementAnalyzeRequest = {
  apiName: string;
  description: string;
  method: string;
  endpoint: string;
  headers?: RequirementHeader[];
  requestBody?: string;
  responseBody?: string;
  businessRules?: string;
  databaseNotes?: string;
  downstreamNotes?: string;
  errorHandlingNotes?: string;
};

export type RequirementHeader = {
  name: string;
  required: boolean;
  description?: string;
};

export type RequirementAnalyzeResponse = {
  success: boolean;
  message: string;
  data?: FactoryOutput;
};

export type FactoryOutput = {
  apiSummary?: Record<string, unknown>;
  workflowSteps?: WorkflowStep[];
  requestSchema?: Record<string, unknown>;
  responseSchema?: Record<string, unknown>;
  businessRules?: unknown[];
  databaseMappings?: unknown[];
  errorScenarios?: unknown[];
  nextActions?: unknown[];
};

export type WorkflowStep = {
  stepNumber: number;
  stepName: string;
  stepType: string;
  description: string;
};
```

---

## Day 31 Acceptance Criteria

Day 31 is complete when:

* A reusable frontend API client is planned.
* The requirement input page has a clear backend submission strategy.
* The backend request payload is defined.
* The expected backend response shape is defined.
* Loading, success, and error states are documented.
* Environment-based backend URL configuration is planned.
* Future components have clear responsibilities.
* The implementation can proceed without guessing the integration structure.

---

## Out of Scope for Day 31

The following items are not part of Day 31:

* Full backend implementation
* Production authentication
* File upload support
* Excel generation
* Node-RED flow generation
* Database persistence
* Advanced response visualization
* User login or role-based access
* Deployment configuration

These will be handled in later phases.

---

## Next Step

After this plan is completed, the next step is to implement the reusable API client in:

```text
src/lib/apiClient.ts
```

This API client will become the foundation for connecting the UI to backend factory services.
