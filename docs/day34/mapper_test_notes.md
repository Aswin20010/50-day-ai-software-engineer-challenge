# Day 34 — Mapper Test Notes

## Purpose

This document defines manual unit-style test cases for the Day 34 rule mapping refactor.

Day 34 moved extracted requirement-to-business-rule mapping logic out of `src/app/page.tsx` and into dedicated mapper files.

The goal of these notes is to verify that the new mapper behavior matches the previous inline behavior exactly.

---

## Files Under Test

The Day 34 mapper logic is located in:

```text
src/mappers/day34/suggestedNodesMapper.ts
src/mappers/day34/extractedRequirementRuleMapper.ts
src/types/day34.ts
```

---

## Main Functions Under Test

The main mapper functions are:

```ts
mapSuggestedNodesToRules
mapExtractedFieldsToRules
buildRulesFromExtraction
```

---

## 1. Suggested Nodes Mapper Tests

## Test 1 — Valid Suggested Nodes

### Input

```ts
const suggestedNodes = ["HTTP-IN", "BR-002", "MAP-001"];
const fallbackRules = [];
```

### Expected Output

```ts
[
  { brId: "HTTP-IN" },
  { brId: "BR-002" },
  { brId: "MAP-001" }
]
```

### Expected Result

Valid suggested node IDs should become `BusinessRule` objects.

---

## Test 2 — Invalid Suggested Nodes Are Ignored

### Input

```ts
const suggestedNodes = ["HTTP-IN", "", null, 123, "BR-002"];
const fallbackRules = [];
```

### Expected Output

```ts
[
  { brId: "HTTP-IN" },
  { brId: "BR-002" }
]
```

### Expected Result

Only non-empty string values should be converted into rules.

---

## Test 3 — Suggested Node IDs Are Trimmed

### Input

```ts
const suggestedNodes = [" HTTP-IN ", " BR-002 "];
const fallbackRules = [];
```

### Expected Output

```ts
[
  { brId: "HTTP-IN" },
  { brId: "BR-002" }
]
```

### Expected Result

Whitespace around suggested node IDs should be removed.

---

## Test 4 — Fallback Rules Used When Suggested Nodes Are Missing

### Input

```ts
const suggestedNodes = undefined;
const fallbackRules = [
  { brId: "HTTP-IN", httpPath: "/existing" },
  { brId: "BR-002", requiredHeaders: "content-type" }
];
```

### Expected Output

```ts
[
  { brId: "HTTP-IN", httpPath: "/existing" },
  { brId: "BR-002", requiredHeaders: "content-type" }
]
```

### Expected Result

If `suggestedNodes` is not an array, existing rules should be preserved.

---

## 2. Extracted Field Mapper Tests

## Test 5 — HTTP-IN Mapping

### Input

```ts
const baseRules = [{ brId: "HTTP-IN" }];

const extracted = {
  httpMethod: "get",
  httpPath: "/api/v1/cdt/trans/upc/:upcNumber"
};
```

### Expected Output

```ts
[
  {
    brId: "HTTP-IN",
    httpMethod: "get",
    httpPath: "/api/v1/cdt/trans/upc/:upcNumber"
  }
]
```

### Expected Result

`HTTP-IN` should receive `httpMethod` and `httpPath`.

---

## Test 6 — BR-002 Header Mapping

### Input

```ts
const baseRules = [{ brId: "BR-002" }];

const extracted = {
  requiredHeaders: "content-type, accept, x-transaction-id",
  jsonHeaders: "content-type, accept"
};
```

### Expected Output

```ts
[
  {
    brId: "BR-002",
    requiredHeaders: "content-type, accept, x-transaction-id",
    jsonHeaders: "content-type, accept"
  }
]
```

### Expected Result

`BR-002` should receive `requiredHeaders` and `jsonHeaders`.

---

## Test 7 — BR-002 Default JSON Headers

### Input

```ts
const baseRules = [{ brId: "BR-002" }];

const extracted = {
  requiredHeaders: "content-type, accept"
};
```

### Expected Output

```ts
[
  {
    brId: "BR-002",
    requiredHeaders: "content-type, accept",
    jsonHeaders: "content-type, accept"
  }
]
```

### Expected Result

If `jsonHeaders` is missing, the mapper should default it to:

```text
content-type, accept
```

---

## Test 8 — BR-003 Required Fields

### Input

```ts
const baseRules = [{ brId: "BR-003" }];

const extracted = {
  requiredFields: "divisionNumber, storeNumber"
};
```

### Expected Output

```ts
[
  {
    brId: "BR-003",
    requiredFields: "divisionNumber, storeNumber"
  }
]
```

---

## Test 9 — BR-004 Numeric Fields

### Input

```ts
const baseRules = [{ brId: "BR-004" }];

const extracted = {
  numericFields: "upcNumber"
};
```

### Expected Output

```ts
[
  {
    brId: "BR-004",
    numericFields: "upcNumber"
  }
]
```

---

## Test 10 — BR-005 Defaults Mapping

### Input

```ts
const baseRules = [{ brId: "BR-005" }];

const extracted = {
  optionalHeaderDefaults: "{\"x-device-type\":{\"target\":\"device_type\",\"default\":\"PDA\"}}",
  soapDefaults: "{\"hdr_version\":\"1\",\"seq_number\":\"1\"}"
};
```

### Expected Output

```ts
[
  {
    brId: "BR-005",
    optionalHeaderDefaults: "{\"x-device-type\":{\"target\":\"device_type\",\"default\":\"PDA\"}}",
    soapDefaults: "{\"hdr_version\":\"1\",\"seq_number\":\"1\"}"
  }
]
```

---

## Test 11 — BR-006 Output Payload Mappings

### Input

```ts
const baseRules = [{ brId: "BR-006" }];

const extracted = {
  outputPayloadMappings: "[{\"source\":\"hdr_version\",\"target\":\"hdr_version\"}]"
};
```

### Expected Output

```ts
[
  {
    brId: "BR-006",
    outputPayloadMappings: "[{\"source\":\"hdr_version\",\"target\":\"hdr_version\"}]"
  }
]
```

---

## Test 12 — BR-007 Endpoint and Operation Mapping

### Input

```ts
const baseRules = [{ brId: "BR-007" }];

const extracted = {
  httpPath: "/api/v1/cdt/trans/upc/:upcNumber",
  operation: "XCDTS005",
  requestHeaderMappings: "{\"x-transaction-id\":\"transactionId\"}"
};
```

### Expected Output

```ts
[
  {
    brId: "BR-007",
    endpoint: "/api/v1/cdt/trans/upc/:upcNumber",
    operation: "XCDTS005",
    requestHeaderMappings: "{\"x-transaction-id\":\"transactionId\"}"
  }
]
```

### Expected Result

`BR-007.endpoint` should use `extracted.httpPath`.

---

## Test 13 — BR-007 Operation Fallback from Flow Name

### Input

```ts
const baseRules = [{ brId: "BR-007" }];

const extracted = {
  httpPath: "/api/v1/cdt/trans/upc/:upcNumber",
  flowName: "XCDTS005 Generated Flow"
};
```

### Expected Output

```ts
[
  {
    brId: "BR-007",
    endpoint: "/api/v1/cdt/trans/upc/:upcNumber",
    operation: "XCDTS005",
    requestHeaderMappings: "{\n  \"x-transaction-id\": \"transactionId\",\n  \"x-source-system\": \"sourceSystem\"\n}"
  }
]
```

### Expected Result

If `operation` is missing, it should fall back to `flowName` with `" Generated Flow"` removed.

---

## Test 14 — BR-007 Default Request Header Mappings

### Input

```ts
const baseRules = [{ brId: "BR-007" }];

const extracted = {
  httpPath: "/api/v1/example",
  operation: "ExampleOperation"
};
```

### Expected Result

`requestHeaderMappings` should default to:

```json
{
  "x-transaction-id": "transactionId",
  "x-source-system": "sourceSystem"
}
```

---

## Test 15 — BR-008 Runtime Config

### Input

```ts
const baseRules = [{ brId: "BR-008" }];

const extracted = {
  runtimeConfig: "{\"endpoint\":\"https://example.com\"}"
};
```

### Expected Output

```ts
[
  {
    brId: "BR-008",
    runtimeConfig: "{\"endpoint\":\"https://example.com\"}"
  }
]
```

---

## Test 16 — BR-009 Required Payload Fields

### Input

```ts
const baseRules = [{ brId: "BR-009" }];

const extracted = {
  requiredPayloadFields: "hdr_version, seq_number, retry_flag"
};
```

### Expected Output

```ts
[
  {
    brId: "BR-009",
    requiredPayloadFields: "hdr_version, seq_number, retry_flag"
  }
]
```

---

## Test 17 — BR-012 Path Parameters

### Input

```ts
const baseRules = [{ brId: "BR-012" }];

const extracted = {
  pathParams: "[{\"name\":\"upcNumber\",\"required\":true}]"
};
```

### Expected Output

```ts
[
  {
    brId: "BR-012",
    pathParams: "[{\"name\":\"upcNumber\",\"required\":true}]"
  }
]
```

---

## Test 18 — BR-013 Query Parameters

### Input

```ts
const baseRules = [{ brId: "BR-013" }];

const extracted = {
  queryParams: "[{\"name\":\"includeProduct\",\"required\":false}]"
};
```

### Expected Output

```ts
[
  {
    brId: "BR-013",
    queryParams: "[{\"name\":\"includeProduct\",\"required\":false}]"
  }
]
```

---

## Test 19 — BR-014 Record Builder Config

### Input

```ts
const baseRules = [{ brId: "BR-014" }];

const extracted = {
  recordBuilderConfig: "{\"sourceArray\":\"items\",\"targetField\":\"difu_records\"}"
};
```

### Expected Output

```ts
[
  {
    brId: "BR-014",
    recordBuilderConfig: "{\"sourceArray\":\"items\",\"targetField\":\"difu_records\"}"
  }
]
```

---

## Test 20 — MAP-001 SOAP Mapping

### Input

```ts
const baseRules = [{ brId: "MAP-001" }];

const extracted = {
  mappingName: "Mapping - xcdts005SoapRequest - Input",
  method: "POST",
  endpointSource: "runtime.endpoint",
  headers: "{\"Content-Type\":\"application/soap+xml\"}",
  requestReplacements: "[{\"tag\":\"hdr_version\",\"source\":\"hdr_version\"}]",
  successFields: "[{\"target\":\"returnCodeOut\",\"fromTag\":\"return_code_out\"}]",
  failureRules: "[{\"fromTag\":\"return_code_out\",\"equals\":\"0301\"}]",
  xmlTemplate: "<soap:Envelope>...</soap:Envelope>"
};
```

### Expected Output

```ts
[
  {
    brId: "MAP-001",
    mappingName: "Mapping - xcdts005SoapRequest - Input",
    method: "POST",
    endpointSource: "runtime.endpoint",
    headers: "{\"Content-Type\":\"application/soap+xml\"}",
    requestReplacements: "[{\"tag\":\"hdr_version\",\"source\":\"hdr_version\"}]",
    successFields: "[{\"target\":\"returnCodeOut\",\"fromTag\":\"return_code_out\"}]",
    failureRules: "[{\"fromTag\":\"return_code_out\",\"equals\":\"0301\"}]",
    xmlTemplate: "<soap:Envelope>...</soap:Envelope>"
  }
]
```

---

## Test 21 — MAP-GENERIC Response Mapping

### Input

```ts
const baseRules = [{ brId: "MAP-GENERIC" }];

const extracted = {
  returnCodePaths: "[\"return_code_out\",\"returnCode\"]",
  messagePaths: "[\"message_out\",\"message\"]",
  successCode: "0",
  successResponseMapping: "{\"code\":{\"from\":\"$returnCode\"}}",
  failureResponseMapping: "{\"code\":{\"from\":\"$returnCode\"}}",
  requiredSuccessFields: "[\"cdt_number\",\"destination_division\"]",
  businessCodeMap: "{\"0301\":{\"httpStatus\":404,\"code\":301}}"
};
```

### Expected Output

```ts
[
  {
    brId: "MAP-GENERIC",
    returnCodePaths: "[\"return_code_out\",\"returnCode\"]",
    messagePaths: "[\"message_out\",\"message\"]",
    successCode: "0",
    successResponseMapping: "{\"code\":{\"from\":\"$returnCode\"}}",
    failureResponseMapping: "{\"code\":{\"from\":\"$returnCode\"}}",
    requiredSuccessFields: "[\"cdt_number\",\"destination_division\"]",
    businessCodeMap: "{\"0301\":{\"httpStatus\":404,\"code\":301}}"
  }
]
```

---

## 3. Preservation Tests

## Test 22 — Existing Rule Values Are Preserved

### Input

```ts
const baseRules = [
  {
    brId: "HTTP-IN",
    httpMethod: "post",
    httpPath: "/existing/path"
  }
];

const extracted = {};
```

### Expected Output

```ts
[
  {
    brId: "HTTP-IN",
    httpMethod: "post",
    httpPath: "/existing/path"
  }
]
```

### Expected Result

When extracted values are missing, existing rule values should remain.

---

## Test 23 — Empty String Fallback

### Input

```ts
const baseRules = [{ brId: "BR-003" }];

const extracted = {};
```

### Expected Output

```ts
[
  {
    brId: "BR-003",
    requiredFields: ""
  }
]
```

### Expected Result

When both extracted and existing values are missing, the same empty-string fallback should be used.

---

## Test 24 — Unknown Rule Is Preserved

### Input

```ts
const baseRules = [
  {
    brId: "CUSTOM-RULE",
    customField: "custom-value"
  } as any
];

const extracted = {
  httpPath: "/api/v1/test"
};
```

### Expected Output

```ts
[
  {
    brId: "CUSTOM-RULE",
    customField: "custom-value"
  }
]
```

### Expected Result

Unknown rule types should be returned unchanged.

---

## 4. Combined Mapper Tests

## Test 25 — Build Rules from Extraction with Suggested Nodes

### Input

```ts
const suggestedNodes = ["HTTP-IN", "BR-002"];
const fallbackRules = [];
const extracted = {
  httpMethod: "get",
  httpPath: "/api/v1/test",
  requiredHeaders: "content-type, accept"
};
```

### Expected Output

```ts
[
  {
    brId: "HTTP-IN",
    httpMethod: "get",
    httpPath: "/api/v1/test"
  },
  {
    brId: "BR-002",
    requiredHeaders: "content-type, accept",
    jsonHeaders: "content-type, accept"
  }
]
```

---

## Test 26 — Build Rules from Extraction with Fallback Rules

### Input

```ts
const suggestedNodes = undefined;
const fallbackRules = [
  {
    brId: "HTTP-IN",
    httpMethod: "post",
    httpPath: "/old/path"
  }
];

const extracted = {
  httpMethod: "get",
  httpPath: "/new/path"
};
```

### Expected Output

```ts
[
  {
    brId: "HTTP-IN",
    httpMethod: "get",
    httpPath: "/new/path"
  }
]
```

### Expected Result

Fallback rules should be used when suggested nodes are missing, then extracted values should still populate over them.

---

## 5. Manual UI Validation

After the mapper files are connected to `page.tsx`, validate this UI flow:

```text
Upload Confluence export file
→ Click Extract Requirements from Confluence
→ Confirm rules are created from suggested nodes
→ Confirm extracted values populate the correct rule cards
→ Confirm missing fields still highlight
→ Edit one populated field manually
→ Generate flow
→ Confirm generated JSON still appears
```

Expected result:

```text
The UI behaves exactly the same as before the Day 34 mapper refactor.
```

---

## 6. Build Validation

Run:

```bash
npm run build
```

Expected result:

```text
Build passes with the mapper files connected.
```

---

## 7. Day 34 Mapper Test Completion Criteria

The mapper refactor is valid when:

* Suggested nodes convert into `BusinessRule[]`.
* Invalid suggested nodes are ignored.
* Fallback rules are preserved when suggested nodes are unavailable.
* Extracted fields populate the correct rule fields.
* Existing rule values are preserved when extracted values are missing.
* Default JSON headers remain `content-type, accept`.
* Default request header mappings remain unchanged.
* Unknown rules are preserved.
* `buildRulesFromExtraction` returns the same output that the old inline mapping produced.
* `npm run build` passes.
* The UI smoke test passes.
