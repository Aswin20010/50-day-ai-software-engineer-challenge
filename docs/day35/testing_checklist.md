# Day 35 — Output Utility Refactor Testing Checklist

## Purpose

This checklist validates the Day 35 output utility refactor.

Day 35 moved generated flow output formatting and JSON download behavior out of `src/app/page.tsx` and into reusable helper files.

The goal is to confirm that the refactor did not change generated output formatting, error output formatting, download behavior, filename, MIME type, or the full API Factory UI workflow.

---

## Testing Scope

This checklist covers:

* Day 35 file structure
* Generated output helper validation
* Download helper validation
* `page.tsx` integration
* Build validation
* Successful generation output
* Generation error output
* Download behavior
* Full regression smoke test

---

## 1. File Structure Validation

Confirm these files exist:

```text
src/lib/day35/generatedOutputHelpers.ts
src/lib/day35/downloadHelpers.ts
docs/day35/output_utility_refactor_plan.md
docs/day35/output_utility_test_notes.md
docs/day35/testing_checklist.md
```

Expected result:

```text
All Day 35 output utility files are present.
```

---

## 2. Generated Output Helper Validation

Open:

```text
src/lib/day35/generatedOutputHelpers.ts
```

Confirm it exports:

```ts
formatGeneratedFlowOutput
formatGenerationErrorOutput
```

Confirm the implementation preserves successful output formatting:

```ts
export function formatGeneratedFlowOutput(flow: unknown): string {
  return JSON.stringify(flow, null, 2);
}
```

Expected result:

```text
Generated flow output still uses two-space JSON formatting.
```

---

## 3. Generation Error Helper Validation

In the same file, confirm the error helper uses:

```ts
export function formatGenerationErrorOutput(error: unknown): string {
  return JSON.stringify(
    {
      error: "Invalid form input",
      details: error instanceof Error ? error.message : String(error),
    },
    null,
    2
  );
}
```

Expected result:

```text
Generation error output keeps the same error/details JSON structure.
```

---

## 4. Download Helper Validation

Open:

```text
src/lib/day35/downloadHelpers.ts
```

Confirm it exports:

```ts
downloadTextFile
downloadJsonText
```

Confirm `downloadTextFile` still performs:

```text
Blob creation
Object URL creation
Anchor creation
Anchor click
Object URL revoke
```

Expected result:

```text
Download behavior was moved into a helper without changing browser behavior.
```

---

## 5. Default JSON Download Filename Check

Confirm `downloadJsonText` defaults to:

```ts
filename = "generated-flow.json"
```

Expected result:

```text
The default download filename remains generated-flow.json.
```

---

## 6. JSON MIME Type Check

Confirm `downloadJsonText` uses:

```ts
downloadTextFile(content, filename, "application/json");
```

Expected result:

```text
Downloaded generated flow still uses application/json MIME type.
```

---

## 7. `page.tsx` Import Check

Open:

```text
src/app/page.tsx
```

Confirm it imports:

```ts
import { downloadJsonText } from "@/lib/day35/downloadHelpers";
import {
  formatGeneratedFlowOutput,
  formatGenerationErrorOutput,
} from "@/lib/day35/generatedOutputHelpers";
```

Expected result:

```text
page.tsx uses Day 35 output helpers.
```

---

## 8. `page.tsx` Success Output Check

Confirm `generateFlow` uses:

```ts
setGenerated(formatGeneratedFlowOutput(data.flow));
```

Expected result:

```text
Successful flow output formatting is delegated to the helper.
```

---

## 9. `page.tsx` Error Output Check

Confirm `generateFlow` catch block uses:

```ts
setGenerated(formatGenerationErrorOutput(err));
```

Expected result:

```text
Generation error output formatting is delegated to the helper.
```

---

## 10. `page.tsx` Download Check

Confirm `downloadFlow` now uses:

```ts
function downloadFlow() {
  if (!generated) {
    return;
  }

  downloadJsonText(generated);
}
```

Expected result:

```text
page.tsx keeps the empty-output guard but delegates download behavior to the helper.
```

---

## 11. Inline Logic Removal Check

Search inside:

```text
src/app/page.tsx
```

Confirm these are no longer present:

```ts
JSON.stringify(data.flow, null, 2)
```

```ts
new Blob([generated]
```

```ts
URL.createObjectURL(blob)
```

```ts
anchor.download = "generated-flow.json"
```

Expected result:

```text
Generated output formatting and Blob download logic are no longer inline in page.tsx.
```

---

## 12. Build Validation

Run:

```bash
npm run build
```

Expected result:

```text
Build completes successfully.
```

If build fails, check:

* Missing Day 35 helper imports
* Incorrect helper file paths
* Incorrect export names
* Unused imports blocked by lint
* Browser-only download helper accidentally called during server rendering
* Syntax errors in `downloadHelpers.ts`

---

## 13. Dev Server Validation

Run:

```bash
npm run dev
```

Open the app in the browser.

Expected result:

```text
API Factory UI loads successfully.
```

Check the browser console for:

* Module not found errors
* Runtime download helper errors
* Undefined helper function errors
* Hydration errors

---

## 14. Successful Generation Output Test

Run this UI flow:

```text
Upload Confluence export file
→ Extract requirements
→ Generate flow
```

Expected result:

```text
Generated flow JSON appears in the output textarea.
```

The output should be formatted with two-space indentation.

---

## 15. Generated Output Content Check

After generation, confirm the textarea content is valid JSON.

Expected result:

```text
The output is the same formatted JSON previously produced by JSON.stringify(data.flow, null, 2).
```

---

## 16. Generation Error Output Test

Enter invalid JSON in one of the JSON-based rule fields, such as:

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

Expected result:

```json
{
  "error": "Invalid form input",
  "details": "<specific error message>"
}
```

Expected result:

```text
Generation error formatting is unchanged.
```

---

## 17. Error Detail Preservation Check

Confirm the error details follow the same behavior:

| Error Type             | Expected `details` |
| ---------------------- | ------------------ |
| `new Error("message")` | `message`          |
| string error           | `String(error)`    |
| object error           | `[object Object]`  |
| `null`                 | `null`             |
| `undefined`            | `undefined`        |

Expected result:

```text
Error detail formatting matches the previous page behavior.
```

---

## 18. Download Before Generation Test

Before generating any flow, click:

```text
Download JSON
```

Expected result:

```text
No file downloads.
```

Reason:

```ts
if (!generated) {
  return;
}
```

Expected result:

```text
The page-level empty output guard is preserved.
```

---

## 19. Download After Generation Test

After successfully generating a flow, click:

```text
Download JSON
```

Expected result:

```text
A file downloads successfully.
```

Expected filename:

```text
generated-flow.json
```

---

## 20. Downloaded Content Match Test

Open the downloaded file and compare it to the generated output textarea.

Expected result:

```text
The downloaded file content matches the textarea content exactly.
```

---

## 21. Download MIME Type Check

Confirm the helper uses:

```text
application/json
```

Expected result:

```text
Downloaded JSON file remains a JSON file.
```

---

## 22. Custom Helper Safety Check

Although the UI uses the default filename, confirm the helper supports a custom filename:

```ts
downloadJsonText(content, "custom-flow.json");
```

Expected result:

```text
The helper can be reused later for custom JSON exports.
```

---

## 23. Full API Factory Regression Test

Run the complete workflow:

```text
Upload Confluence export file
→ Click Extract Requirements from Confluence
→ Confirm normalized extraction appears
→ Confirm suggested nodes populate rule cards
→ Edit one rule field
→ Remove one unnecessary rule
→ Generate flow
→ Confirm generated JSON appears
→ Confirm generation metadata appears
→ Expand full metadata JSON
→ Download generated-flow.json
```

Expected result:

```text
The full API Factory workflow behaves the same as before Day 35.
```

---

## 24. Regression Checklist

Use this table during validation:

| Feature                                           | Pass / Fail |
| ------------------------------------------------- | ----------- |
| App loads                                         |             |
| Build passes                                      |             |
| Dev server starts                                 |             |
| Requirement extraction works                      |             |
| Normalized extraction displays                    |             |
| Suggested nodes populate rules                    |             |
| Rule editing works                                |             |
| Rule removal works                                |             |
| Generate flow works                               |             |
| Generated JSON formatting unchanged               |             |
| Generation error formatting unchanged             |             |
| Metadata rendering works                          |             |
| Download button does nothing when output is empty |             |
| Download button downloads after generation        |             |
| Filename is `generated-flow.json`                 |             |
| Downloaded content matches textarea               |             |

---

## 25. Day 35 Phase 6 Completion Criteria

Phase 6 is complete when:

* `npm run build` passes.
* `npm run dev` starts.
* Generated output helper exists and is used.
* Download helper exists and is used.
* `page.tsx` no longer contains inline generated output formatting.
* `page.tsx` no longer contains Blob/object URL download logic.
* Successful generated flow output is unchanged.
* Generation error output is unchanged.
* Download filename remains `generated-flow.json`.
* Downloaded file content matches the textarea.
* Full UI smoke test passes.

---

## Final Validation Commands

Run:

```bash
npm run build
```

Then run:

```bash
npm run dev
```

Perform the full regression test from Section 23.

If all checks pass, Day 35 Phase 6 is complete.
