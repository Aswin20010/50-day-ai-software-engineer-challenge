# Day 35 — Output Utility Test Notes

## Purpose

This document defines manual unit-style test cases for the Day 35 output utility refactor.

Day 35 moved generated output formatting and download behavior out of `src/app/page.tsx` and into reusable utility files.

The goal of these notes is to verify that the new helpers preserve the exact previous behavior.

---

## Files Under Test

The Day 35 utility logic is located in:

```text
src/lib/day35/generatedOutputHelpers.ts
src/lib/day35/downloadHelpers.ts
```

---

## Main Functions Under Test

The main utility functions are:

```ts
formatGeneratedFlowOutput
formatGenerationErrorOutput
downloadTextFile
downloadJsonText
```

---

## 1. Generated Flow Formatting Tests

## Test 1 — Format Object Output

### Input

```ts
const flow = {
  id: "flow-1",
  label: "Generated Flow",
  nodes: [
    {
      id: "node-1",
      type: "http in"
    }
  ]
};
```

### Function

```ts
formatGeneratedFlowOutput(flow)
```

### Expected Output

```json
{
  "id": "flow-1",
  "label": "Generated Flow",
  "nodes": [
    {
      "id": "node-1",
      "type": "http in"
    }
  ]
}
```

### Expected Result

The helper should return `JSON.stringify(flow, null, 2)`.

---

## Test 2 — Format Array Output

### Input

```ts
const flow = [
  {
    id: "node-1",
    type: "http in"
  },
  {
    id: "node-2",
    type: "function"
  }
];
```

### Function

```ts
formatGeneratedFlowOutput(flow)
```

### Expected Result

The helper should return a formatted JSON array with two-space indentation.

---

## Test 3 — Format Null Output

### Input

```ts
const flow = null;
```

### Function

```ts
formatGeneratedFlowOutput(flow)
```

### Expected Output

```text
null
```

### Expected Result

The helper should behave exactly like `JSON.stringify(null, null, 2)`.

---

## Test 4 — Format Undefined Output

### Input

```ts
const flow = undefined;
```

### Function

```ts
formatGeneratedFlowOutput(flow)
```

### Expected Output

```text
undefined
```

### Expected Result

The helper should preserve the existing JavaScript behavior of `JSON.stringify(undefined, null, 2)`.

Important note:

```text
This is acceptable because the previous page behavior used JSON.stringify(data.flow, null, 2) directly.
```

---

## 2. Generation Error Formatting Tests

## Test 5 — Format Standard Error

### Input

```ts
const error = new Error("Invalid JSON in requestReplacements");
```

### Function

```ts
formatGenerationErrorOutput(error)
```

### Expected Output

```json
{
  "error": "Invalid form input",
  "details": "Invalid JSON in requestReplacements"
}
```

### Expected Result

The helper should use `error.message` when the input is an instance of `Error`.

---

## Test 6 — Format String Error

### Input

```ts
const error = "Something went wrong";
```

### Function

```ts
formatGenerationErrorOutput(error)
```

### Expected Output

```json
{
  "error": "Invalid form input",
  "details": "Something went wrong"
}
```

### Expected Result

The helper should use `String(error)` when the input is not an `Error`.

---

## Test 7 — Format Unknown Object Error

### Input

```ts
const error = {
  reason: "Invalid payload"
};
```

### Function

```ts
formatGenerationErrorOutput(error)
```

### Expected Output

```json
{
  "error": "Invalid form input",
  "details": "[object Object]"
}
```

### Expected Result

The helper should preserve the previous behavior by using `String(error)`.

---

## Test 8 — Format Null Error

### Input

```ts
const error = null;
```

### Function

```ts
formatGenerationErrorOutput(error)
```

### Expected Output

```json
{
  "error": "Invalid form input",
  "details": "null"
}
```

### Expected Result

The helper should preserve the previous behavior by using `String(null)`.

---

## Test 9 — Format Undefined Error

### Input

```ts
const error = undefined;
```

### Function

```ts
formatGenerationErrorOutput(error)
```

### Expected Output

```json
{
  "error": "Invalid form input",
  "details": "undefined"
}
```

### Expected Result

The helper should preserve the previous behavior by using `String(undefined)`.

---

## 3. Download Helper Tests

## Test 10 — Download JSON with Default Filename

### Input

```ts
const content = "{ \"test\": true }";
```

### Function

```ts
downloadJsonText(content)
```

### Expected Result

The browser should download a file named:

```text
generated-flow.json
```

The MIME type should be:

```text
application/json
```

---

## Test 11 — Download JSON with Custom Filename

### Input

```ts
const content = "{ \"test\": true }";
const filename = "custom-flow.json";
```

### Function

```ts
downloadJsonText(content, filename)
```

### Expected Result

The browser should download a file named:

```text
custom-flow.json
```

The MIME type should still be:

```text
application/json
```

---

## Test 12 — Download Text File

### Input

```ts
const content = "hello world";
const filename = "notes.txt";
const mimeType = "text/plain";
```

### Function

```ts
downloadTextFile(content, filename, mimeType)
```

### Expected Result

The browser should download:

```text
notes.txt
```

with MIME type:

```text
text/plain
```

---

## Test 13 — Empty Content Download

### Input

```ts
const content = "";
```

### Function

```ts
downloadJsonText(content)
```

### Expected Result

The helper itself can download an empty file.

Important note:

```text
The page-level downloadFlow function still prevents downloading when generated is empty.
```

---

## 4. Page Integration Tests

## Test 14 — Successful Generate Flow Output

### UI Flow

```text
Upload Confluence export file
→ Extract requirements
→ Generate flow
```

### Expected Result

The generated output textarea should still show formatted JSON.

The output should match the previous behavior from:

```ts
JSON.stringify(data.flow, null, 2)
```

---

## Test 15 — Generation Error Output

### UI Flow

Enter invalid JSON in a JSON textarea such as:

```text
requestReplacements
successFields
failureRules
headers
pathParams
queryParams
businessCodeMap
```

Then click:

```text
Generate Flow
```

### Expected Result

The generated output textarea should show:

```json
{
  "error": "Invalid form input",
  "details": "<specific error message>"
}
```

This should match the previous behavior exactly.

---

## Test 16 — Download Generated Flow

### UI Flow

```text
Generate flow
→ Click Download JSON
```

### Expected Result

The browser should download:

```text
generated-flow.json
```

The downloaded file content should match the textarea content exactly.

---

## Test 17 — Download Does Nothing When Output Is Empty

### UI Flow

Before generating any flow, click:

```text
Download JSON
```

### Expected Result

No file should download.

The page-level guard should still work:

```ts
if (!generated) {
  return;
}
```

---

## 5. Regression Checks

Confirm the following behavior remains unchanged:

| Behavior                      | Expected Result                           |
| ----------------------------- | ----------------------------------------- |
| Successful flow output        | Formatted JSON with two-space indentation |
| Error output                  | JSON with `error` and `details`           |
| Error title                   | `Invalid form input`                      |
| Error detail for Error object | Uses `error.message`                      |
| Error detail for non-Error    | Uses `String(error)`                      |
| Default download filename     | `generated-flow.json`                     |
| Download MIME type            | `application/json`                        |
| Empty generated output        | Download does nothing                     |
| Downloaded content            | Matches textarea content                  |

---

## 6. Build Validation

Run:

```bash
npm run build
```

Expected result:

```text
Build passes after the Day 35 utility refactor.
```

---

## 7. Final Manual Smoke Test

Run the full UI flow:

```text
Upload Confluence export file
→ Extract requirements
→ Confirm rules populate
→ Generate flow
→ Confirm generated JSON appears
→ Download generated-flow.json
→ Open downloaded file
→ Confirm downloaded content matches the UI textarea
```

Expected result:

```text
The output and download behavior works exactly as before Day 35.
```

---

## Day 35 Phase 5 Completion Criteria

Phase 5 is complete when:

* Generated flow formatting test cases are documented.
* Generation error formatting test cases are documented.
* Download helper test cases are documented.
* Page integration test cases are documented.
* Regression checks are documented.
* Build validation command is documented.
* Final manual smoke test is documented.
