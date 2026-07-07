# Day 35 — Output Utility Refactor Plan

## Purpose

The purpose of Day 35 is to move generated output formatting and JSON download behavior out of `src/app/page.tsx` and into reusable utility files.

Day 32 moved UI rendering into components.

Day 33 moved API calls into service files.

Day 34 moved extracted requirement-to-rule mapping into mapper files.

Day 35 continues the cleanup by extracting generated output helper logic.

---

## Day 35 Goal

The main goal is:

```text
Move generated output formatting and download logic into reusable utilities without changing behavior.
```

Currently, `page.tsx` still owns:

* Formatting successful generated flow JSON
* Formatting generation error JSON
* Creating a Blob for downloaded JSON
* Creating a temporary object URL
* Creating an anchor element
* Triggering the file download
* Revoking the object URL

This logic works, but it does not need to live directly inside the page.

---

## Current Output Logic in `page.tsx`

The generation success path currently formats output like this:

```ts
setGenerated(JSON.stringify(data.flow, null, 2));
```

The generation error path currently formats output like this:

```ts
setGenerated(
  JSON.stringify(
    {
      error: "Invalid form input",
      details: err instanceof Error ? err.message : String(err),
    },
    null,
    2
  )
);
```

The download logic currently looks like this:

```ts
function downloadFlow() {
  if (!generated) {
    return;
  }

  const blob = new Blob([generated], {
    type: "application/json",
  });

  const url = URL.createObjectURL(blob);
  const anchor = document.createElement("a");

  anchor.href = url;
  anchor.download = "generated-flow.json";
  anchor.click();

  URL.revokeObjectURL(url);
}
```

Day 35 will move this logic into utilities.

---

## What Should Move

## 1. Generated Output Formatting

Move formatting logic into:

```text
src/lib/day35/generatedOutputHelpers.ts
```

This file should provide helpers for:

```text
formatGeneratedFlowOutput
formatGenerationErrorOutput
```

### `formatGeneratedFlowOutput`

This helper should accept the generated flow object and return formatted JSON.

Expected behavior:

```ts
formatGeneratedFlowOutput(data.flow)
```

returns:

```text
JSON.stringify(data.flow, null, 2)
```

---

### `formatGenerationErrorOutput`

This helper should accept an unknown error and return the same formatted error JSON currently shown in the textarea.

Expected output structure:

```json
{
  "error": "Invalid form input",
  "details": "<error message>"
}
```

The helper should preserve the same error detail behavior:

```ts
err instanceof Error ? err.message : String(err)
```

---

## 2. JSON Download Logic

Move download logic into:

```text
src/lib/day35/downloadHelpers.ts
```

This file should provide:

```text
downloadTextFile
downloadJsonText
```

### `downloadTextFile`

This helper should:

* Accept text content
* Accept filename
* Accept MIME type
* Create a Blob
* Create an object URL
* Create an anchor
* Set anchor href
* Set anchor download filename
* Trigger click
* Revoke the object URL

---

### `downloadJsonText`

This helper should call `downloadTextFile` with:

```text
type: application/json
filename: generated-flow.json
```

---

## What Should Stay in `page.tsx`

The page should still own:

* React state
* Calling `generateNodeRedFlow`
* Setting `generationMetadata`
* Setting `generated`
* Deciding whether generated content exists before download
* Calling the download helper
* UI orchestration

The page should not directly create Blob or object URLs anymore.

---

## Proposed Folder Structure

Create:

```text
src/
  lib/
    day35/
      generatedOutputHelpers.ts
      downloadHelpers.ts

docs/
  day35/
    output_utility_refactor_plan.md
    output_utility_test_notes.md
    testing_checklist.md
    day35_completion_report.md
```

---

## Helper Function Design

## `generatedOutputHelpers.ts`

Recommended functions:

```ts
export function formatGeneratedFlowOutput(flow: unknown): string

export function formatGenerationErrorOutput(error: unknown): string
```

Expected implementation behavior:

```ts
JSON.stringify(flow, null, 2)
```

and:

```ts
JSON.stringify(
  {
    error: "Invalid form input",
    details: error instanceof Error ? error.message : String(error),
  },
  null,
  2
)
```

---

## `downloadHelpers.ts`

Recommended functions:

```ts
export function downloadTextFile(
  content: string,
  filename: string,
  mimeType: string
): void

export function downloadJsonText(
  content: string,
  filename?: string
): void
```

Expected behavior:

```ts
downloadJsonText(generated)
```

downloads:

```text
generated-flow.json
```

with MIME type:

```text
application/json
```

---

## Updated Page Behavior

After Day 35, the success path in `generateFlow` should change from:

```ts
setGenerated(JSON.stringify(data.flow, null, 2));
```

to:

```ts
setGenerated(formatGeneratedFlowOutput(data.flow));
```

The error path should change from:

```ts
setGenerated(
  JSON.stringify(
    {
      error: "Invalid form input",
      details: err instanceof Error ? err.message : String(err),
    },
    null,
    2
  )
);
```

to:

```ts
setGenerated(formatGenerationErrorOutput(err));
```

The download logic should change from direct Blob logic to:

```ts
function downloadFlow() {
  if (!generated) {
    return;
  }

  downloadJsonText(generated);
}
```

---

## Behavior That Must Not Change

The following behavior must remain unchanged:

* Successful generated flow output is still formatted with two-space JSON indentation.
* Error output still has the same JSON structure.
* Error message still uses `err.message` when the error is an `Error`.
* Error message still uses `String(err)` for non-Error values.
* Download still does nothing when `generated` is empty.
* Downloaded filename remains `generated-flow.json`.
* Download MIME type remains `application/json`.
* Generated JSON textarea still displays the same content.
* Downloaded file still contains the same content shown in the textarea.

---

## Refactor Strategy

Use this safe order:

1. Create generated output helper.
2. Create download helper.
3. Update `page.tsx` imports.
4. Replace output formatting calls.
5. Replace direct Blob download logic.
6. Run build.
7. Run generation and download smoke test.

---

## Phase Plan

## Phase 1 — Output Utility Refactor Plan

Create:

```text
docs/day35/output_utility_refactor_plan.md
```

---

## Phase 2 — Generated Output Helper

Create:

```text
src/lib/day35/generatedOutputHelpers.ts
```

---

## Phase 3 — Download Helper

Create:

```text
src/lib/day35/downloadHelpers.ts
```

---

## Phase 4 — Update Page

Update:

```text
src/app/page.tsx
```

Use:

```text
formatGeneratedFlowOutput
formatGenerationErrorOutput
downloadJsonText
```

---

## Phase 5 — Output Utility Test Notes

Create:

```text
docs/day35/output_utility_test_notes.md
```

---

## Phase 6 — Testing Checklist

Create:

```text
docs/day35/testing_checklist.md
```

---

## Phase 7 — Completion Report

Create:

```text
docs/day35/day35_completion_report.md
```

---

## Acceptance Criteria

Day 35 is complete when:

* Generated flow formatting is moved into a helper.
* Generation error formatting is moved into a helper.
* JSON download logic is moved into a helper.
* `page.tsx` no longer directly calls `JSON.stringify(data.flow, null, 2)`.
* `page.tsx` no longer directly builds the generation error JSON object.
* `page.tsx` no longer directly creates `Blob`, `URL.createObjectURL`, or anchor download logic.
* Generated output remains unchanged.
* Error output remains unchanged.
* Downloaded filename remains `generated-flow.json`.
* Downloaded content matches the textarea content.
* `npm run build` passes.
* Full smoke test passes.

---

## Final Validation Flow

Run:

```bash
npm run build
```

Then run:

```bash
npm run dev
```

Perform this smoke test:

```text
Upload Confluence export file
→ Extract requirements
→ Generate flow
→ Confirm generated JSON appears
→ Download generated-flow.json
→ Confirm downloaded file content matches textarea
```

If this works exactly as before, Day 35 is successful.
