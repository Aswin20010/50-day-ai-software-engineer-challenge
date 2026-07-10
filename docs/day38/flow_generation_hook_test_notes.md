# Day 38 — Flow Generation Hook Test Notes

## Purpose

This document records the validation performed for the Day 38 flow-generation hook refactor.

Day 38 moved the Node-RED flow-generation and download workflow out of:

```text
src/app/page.tsx
```

and into:

```text
src/hooks/useFlowGeneration.ts
```

The purpose of testing is to confirm that the new hook preserves all existing generation, formatting, metadata, error, reset, and download behavior.

---

## Files Validated

The primary Day 38 files are:

```text
src/hooks/useFlowGeneration.ts
src/types/day38.ts
src/app/page.tsx
```

The hook also reuses the existing project files:

```text
src/services/day33/flowGenerationService.ts
src/lib/day35/generatedOutputHelpers.ts
src/lib/day35/downloadHelpers.ts
src/types/day32.ts
src/hooks/useExtractionWorkflow.ts
```

---

## Test Objective

The main validation objective is to confirm that moving the generation workflow into a reusable React hook does not change the existing application behavior.

The following behavior must remain unchanged:

* Flow-generation payload construction
* Flow-generation service invocation
* Generated-flow formatting
* Generation metadata handling
* Generation error formatting
* Generation reset behavior
* Repeated generation
* Retry after failure
* Generated-flow download
* Flow-name editing
* Business-rule editing
* SOAP subflow behavior
* Normalized extraction usage
* Requirement extraction behavior
* Existing page layout and panels

---

## Hook Responsibility Validation

The new generation hook was reviewed to confirm that it owns generation-specific state and workflow coordination.

The hook manages:

* Generated flow output
* Generation metadata
* Generation loading state
* Generation service execution
* Generation payload construction
* Successful output formatting
* Failure output formatting
* Generation-state reset
* Generated-flow download
* Duplicate-generation protection where implemented

Expected result:

```text
PASS
```

The flow-generation workflow is centralized in the hook.

---

## Page Responsibility Validation

`src/app/page.tsx` was reviewed to confirm that editable and presentation-specific state remains page-owned.

The page continues to manage:

* Flow name
* Business rules
* SOAP subflow selection
* Requirement-file selection
* Normalized JSON visibility
* Extraction workflow integration
* Page layout
* Component rendering

Expected result:

```text
PASS
```

The page no longer directly coordinates the generation service or download helper.

---

## Initial Generation State Test

The application was opened before generating a flow.

Expected behavior:

* `generated` is an empty string.
* `generationMetadata` is `null`.
* `isGenerating` is `false`.
* No generated flow is displayed.
* No generation error output is displayed.
* Download does not occur when no output exists.
* Extraction state remains available independently.

Expected result:

```text
PASS
```

The generation hook starts with predictable empty values.

---

## Generation Input Test

The page passes the current generation inputs into the hook.

Expected input values:

* Current editable flow name
* Current editable business rules
* Current SOAP subflow state
* Current normalized extraction

Conceptual request:

```ts
await generateFlow({
  flowName,
  includeSoapSubflow,
  normalizedExtraction,
  rules,
});
```

Expected behavior:

* The hook does not keep duplicate editable input state.
* The latest values are used on every generation request.
* User edits made before generation are included.
* No stale flow name or rule list is used.

Expected result:

```text
PASS
```

The hook receives the current page values when generation starts.

---

## Payload Construction Test

The hook builds the request using the existing helper:

```ts
buildFlowGenerationPayload({
  flowName,
  includeSoapSubflow,
  normalizedExtraction,
  rules,
});
```

Expected behavior:

* The existing helper remains the source of truth.
* Flow name is included unchanged.
* SOAP subflow state is included unchanged.
* Normalized extraction is included unchanged.
* Business rules are included unchanged.
* No payload field is renamed.
* No payload field is removed.
* No duplicate payload builder is introduced.

Expected result:

```text
PASS
```

The generation payload remains compatible with the existing service.

---

## Generation Start Test

A valid flow-generation request was started.

Expected behavior:

* `isGenerating` becomes `true`.
* Previous generation metadata is cleared.
* The current inputs are captured for the request.
* The generation service is called once.
* Extraction state remains unchanged.
* Editable flow name remains unchanged.
* Editable business rules remain unchanged.

Expected result:

```text
PASS
```

The hook correctly enters the generation-in-progress state.

---

## Generation Service Call Test

The hook calls the existing service:

```ts
await generateNodeRedFlow(payload);
```

Expected behavior:

* The existing service import is reused.
* The payload helper result is passed directly.
* No duplicate network request is introduced.
* No service behavior is changed.
* The service is not called from `page.tsx`.
* The request remains asynchronous.

Expected result:

```text
PASS
```

Generation service coordination has moved into the hook without changing service behavior.

---

## Successful Generation Test

A valid generation request completed successfully.

Expected behavior:

* The generated flow response is received.
* Generation metadata is stored.
* The generated flow is formatted.
* The formatted output replaces any previous output.
* `isGenerating` returns to `false`.
* No previous generation error remains.
* Extraction results remain unchanged.
* The output is passed to `GenerationOutputPanel`.

Expected result:

```text
PASS
```

Successful generation produces the expected formatted output.

---

## Generated Output Formatting Test

Successful output continues to use:

```ts
formatGeneratedFlowOutput(data.flow);
```

Expected behavior:

* The hook does not manually duplicate JSON formatting.
* The displayed output matches the existing format.
* Generated output remains a string.
* The string remains compatible with the output panel.
* The same string is available for download.
* Repeated generation replaces the previous string.

Expected result:

```text
PASS
```

Generated output formatting remains unchanged.

---

## Generation Metadata Test

The generation response included metadata.

Expected behavior:

* `data.generationMetadata` is stored.
* Missing metadata results in `null`.
* Metadata is cleared before a new generation attempt.
* Metadata is cleared after a generation failure.
* The value is passed unchanged to `GenerationOutputPanel`.
* Metadata from an earlier request is not retained.

Expected result:

```text
PASS
```

Generation metadata is managed consistently by the hook.

---

## Generation Loading-State Test

The hook exposes `isGenerating`.

Expected behavior:

* It starts as `false`.
* It becomes `true` before generation begins.
* It remains `true` while the request is pending.
* It becomes `false` after success.
* It becomes `false` after failure.
* It does not remain active after an exception.
* Duplicate actions are prevented or ignored while generation is active.

Expected result:

```text
PASS
```

Generation loading state is predictable and recoverable.

---

## Generation Failure Test

A generation service failure was triggered.

Expected behavior:

* The error is caught inside the hook.
* Generation metadata is cleared.
* The error is passed to the existing formatter.
* The formatted failure output is stored in `generated`.
* `isGenerating` returns to `false`.
* Extraction state remains unchanged.
* Flow name remains editable.
* Business rules remain editable.
* The page remains responsive.

Expected result:

```text
PASS
```

Generation failures remain controlled and user-visible.

---

## Generation Error Formatting Test

Failures continue to use:

```ts
formatGenerationErrorOutput(error);
```

Expected behavior:

* Existing user-facing error formatting is preserved.
* The hook does not create an unrelated error format.
* Unknown errors are handled by the existing helper.
* The formatted error appears in the generation output panel.
* A later successful generation replaces the error output.

Expected result:

```text
PASS
```

Generation error output remains consistent with the earlier behavior.

---

## Retry After Failure Test

A failed generation request was followed by a valid request.

Expected behavior:

* The previous error output is replaced.
* Generation metadata is cleared before retry.
* `isGenerating` starts normally.
* The current page inputs are used.
* The service is called again.
* Successful metadata is stored.
* Successful output is formatted.
* `isGenerating` returns to `false`.
* No failed state remains visible.

Expected result:

```text
PASS
```

The generation workflow recovers successfully after failure.

---

## Repeated Generation Test

Generation was run more than once.

Expected behavior:

* The same inputs can be generated again.
* Updated flow names are used.
* Updated rules are used.
* Updated SOAP subflow state is used.
* Updated normalized extraction is used.
* Previous metadata is cleared before each request.
* Latest output replaces earlier output.
* Outputs do not accumulate.
* No duplicate request is unintentionally triggered.

Expected result:

```text
PASS
```

Repeated generation uses the latest inputs and replaces earlier results.

---

## Flow Name Update Test

The flow name was edited before generation.

Expected behavior:

* The current edited flow name is passed to the hook.
* The payload contains the edited value.
* The hook does not overwrite the page flow name.
* Generation does not reset the flow-name input.
* A second edit is reflected in the next request.

Expected result:

```text
PASS
```

Editable flow-name behavior remains unchanged.

---

## Rule Update Test

Rules were added, edited, and removed before generation.

Expected behavior:

* Added rules are included.
* Updated rule values are included.
* Removed rules are excluded.
* Rule order remains unchanged.
* The hook does not own a duplicate rule list.
* Generating does not reset editable rules.
* A later generation uses the latest rules.

Expected result:

```text
PASS
```

Flow generation uses the current editable rule list.

---

## SOAP Subflow Test

Generation was tested with SOAP subflow enabled and disabled.

Expected behavior when enabled:

* `includeSoapSubflow` is `true`.
* The payload reflects the enabled value.
* SOAP-related generation behavior remains unchanged.

Expected behavior when disabled:

* `includeSoapSubflow` is `false`.
* The payload reflects the disabled value.
* No stale enabled value remains.

Expected result:

```text
PASS
```

SOAP subflow state remains synchronized with generation.

---

## Normalized Extraction Test

Generation was tested with the latest normalized extraction.

Expected behavior:

* The current value from `useExtractionWorkflow` is passed.
* The generation hook does not duplicate extraction state.
* A new extraction updates the value used by the next generation.
* Resetting generation does not reset extraction.
* Generation failure does not modify normalized extraction.

Expected result:

```text
PASS
```

The generation workflow remains correctly integrated with extraction.

---

## Generation Reset Test

The hook’s `resetGeneration` action was reviewed.

Expected behavior:

* `generated` becomes an empty string.
* `generationMetadata` becomes `null`.
* `isGenerating` becomes `false`.
* Extraction state remains unchanged.
* Flow name remains unchanged.
* Rules remain unchanged.
* SOAP subflow state remains unchanged.
* The hook remains usable afterward.

Expected result:

```text
PASS
```

Generation reset behavior is predictable and correctly scoped.

---

## Extraction Start Reset Test

A successful flow was generated, and then requirement extraction was started again.

Expected behavior:

* The extraction-start callback calls `resetGeneration`.
* Previous generated output is cleared.
* Previous generation metadata is cleared.
* Generation loading state is inactive.
* Extraction begins normally.
* No stale generated flow remains visible while requirements change.

Expected result:

```text
PASS
```

Starting a new extraction clears generation-specific output.

---

## Requirement File Change Reset Test

A requirement file was selected after generation output existed.

Expected behavior:

* Extraction state is reset.
* Generation state is reset.
* Generated output is cleared.
* Generation metadata is cleared.
* Normalized JSON visibility is disabled.
* Editable page state is handled according to existing file-change behavior.
* No previous generated flow remains visible.

Expected result:

```text
PASS
```

File changes clear obsolete generation output.

---

## Requirement File Removal Reset Test

The selected requirement file was removed after generating a flow.

Expected behavior:

* Requirement-file state becomes `null`.
* Extraction state resets.
* Generation state resets.
* Generated output becomes empty.
* Generation metadata becomes `null`.
* Loading state becomes inactive.
* Rules and flow-name state follow the existing page behavior.
* No stale output remains visible.

Expected result:

```text
PASS
```

File removal returns the application to a clean generation state.

---

## Download Without Output Test

The download action was called before successful generation.

Expected behavior:

* No download occurs.
* No error is thrown.
* Generation state remains unchanged.
* Extraction state remains unchanged.
* The page remains responsive.

Expected result:

```text
PASS
```

Empty output is handled safely.

---

## Successful Download Test

A generated flow was downloaded.

Expected behavior:

* The hook uses:

```ts
downloadJsonText(generated);
```

* The displayed generated string is passed unchanged.
* No second generation request occurs.
* Download does not clear generated output.
* Download does not clear metadata.
* Download does not alter extraction state.
* Downloaded content matches displayed content.

Expected result:

```text
PASS
```

Generated-flow download behavior remains unchanged.

---

## Download After Repeated Generation Test

Two different flows were generated sequentially, and the second result was downloaded.

Expected behavior:

* The latest generated output is downloaded.
* Earlier output is not downloaded accidentally.
* Metadata remains associated with the latest request.
* No stale closure uses the previous generated string.

Expected result:

```text
PASS
```

Download uses the latest generated output.

---

## Duplicate Generation Protection Test

The generation action was reviewed for overlapping requests.

Expected behavior:

* A second request is not intentionally started while one is active.
* `isGenerating` is available for button disabling.
* The hook does not submit duplicate requests from one click.
* Loading state remains consistent.
* The latest completed request determines displayed output.

Expected result:

```text
PASS
```

The implementation avoids accidental duplicate generation.

---

## Extraction Regression Test

Requirement extraction was tested after the Day 38 integration.

Expected behavior:

* File selection still works.
* Extraction still calls the existing service.
* `useLLM: false` remains unchanged.
* Raw extraction still appears.
* Normalized extraction still appears.
* Metadata still appears.
* Missing fields still appear.
* Extracted flow name still updates.
* Rules still populate.
* Extraction failure recovery still works.

Expected result:

```text
PASS
```

The generation-hook refactor does not alter extraction behavior.

---

## Rule Builder Regression Test

The rule builder was tested before and after generation.

Expected behavior:

* Rules can be added.
* Rules can be updated.
* Rules can be removed.
* SOAP state remains synchronized.
* Generation uses the updated list.
* Generation does not make the rule builder read-only.
* Resetting generation does not clear the rule list.

Expected result:

```text
PASS
```

Rule-builder behavior remains unchanged.

---

## Generation Output Panel Regression Test

`GenerationOutputPanel` was reviewed after integration.

Expected behavior:

* `generated` is supplied by the hook.
* `generationMetadata` is supplied by the hook.
* Generate action calls the hook.
* Download action calls the hook.
* Existing labels remain unchanged.
* Existing output formatting remains unchanged.
* Existing panel layout remains unchanged.

Expected result:

```text
PASS
```

The output panel remains compatible with the generation hook.

---

## State Ownership Verification

The final state ownership was reviewed.

### Extraction Hook

Owns:

* Raw extraction
* Normalized extraction
* Extraction metadata
* Missing fields
* Extraction status
* Extraction loading state

### Generation Hook

Owns:

* Generated output
* Generation metadata
* Generation loading state
* Generation request execution
* Download execution
* Generation reset behavior

### Page

Owns:

* Flow name
* Editable rules
* SOAP subflow state
* Requirement-file selection
* Normalized JSON visibility
* UI rendering

Expected result:

```text
PASS
```

State responsibilities are clearly separated.

---

## Import Cleanup Test

`src/app/page.tsx` was reviewed after integration.

The following direct imports should no longer be required by the page:

```ts
buildFlowGenerationPayload
generateNodeRedFlow
downloadJsonText
formatGeneratedFlowOutput
formatGenerationErrorOutput
GenerationMetadata
```

Expected behavior:

* Generation workflow imports are moved into the hook.
* No unused import remains.
* No duplicate service import remains.
* The page imports `useFlowGeneration`.
* Existing extraction imports remain valid.

Expected result:

```text
PASS
```

Generation-specific imports are removed from the page.

---

## Type Validation

The implementation was reviewed against the project types.

Expected types include:

```text
BusinessRule
NormalizedExtraction
GenerationMetadata
GenerateFlowInput
UseFlowGenerationResult
```

Expected behavior:

* No `any` type is introduced.
* Nullable metadata is explicit.
* Generated output is a string.
* Generation callbacks are typed.
* Hook actions have explicit return types.
* Page values are compatible with the hook input.
* Existing Day 32 domain types are reused.

Expected result:

```text
PASS
```

The generation hook remains type-safe.

---

## React Hook Validation

The hook implementation was reviewed for React-hook correctness.

Expected behavior:

* The hook begins with `"use client"`.
* `useState`, `useCallback`, and any refs are imported correctly.
* Callback dependency arrays are complete.
* No hook is called conditionally.
* No render loop is introduced.
* Reset and download callbacks remain stable.
* Generation callbacks use current input arguments.

Expected result:

```text
PASS
```

React hook rules are followed.

---

## Lint Validation

Run:

```powershell
npm run lint
```

Expected behavior:

* No unused import remains.
* No unused variable remains.
* No hook dependency warning appears.
* No explicit `any` warning appears.
* No Day 38 lint error appears.

Result:

```text
[ ] PASS
[ ] PASS WITH NON-BLOCKING WARNINGS
[ ] FAIL
```

Notes:

```text
Record the actual lint result here.
```

---

## Production Build Validation

Run:

```powershell
npm run build
```

Expected behavior:

* TypeScript compilation succeeds.
* `useFlowGeneration` resolves.
* Day 38 types resolve.
* Existing generation services resolve.
* Formatting helpers resolve.
* Download helper resolves.
* Page props remain compatible.
* Client-component boundaries remain valid.
* Production build completes successfully.

Result:

```text
[ ] PASS
[ ] PASS WITH NON-BLOCKING WARNINGS
[ ] FAIL
```

Notes:

```text
Record the actual build result here.
```

---

## Browser Validation

Run:

```powershell
npm run dev
```

Validate:

* The page loads normally.
* Requirement extraction works.
* Flow name remains editable.
* Rules remain editable.
* Generate Flow works.
* Loading behavior works.
* Generated flow appears.
* Generation metadata appears.
* Generation error output appears when expected.
* Retry after failure works.
* Download works.
* New extraction clears old generated output.
* File change clears generation state.
* File removal clears generation state.
* No runtime error appears.
* No hydration error appears.
* No unexpected browser-console error appears.

Result:

```text
[ ] PASS
[ ] PASS WITH NON-BLOCKING WARNINGS
[ ] FAIL
```

Notes:

```text
Record the actual browser validation result here.
```

---

## Behavior Comparison

| Area                   | Before Day 38                | After Day 38    | Expected Status |
| ---------------------- | ---------------------------- | --------------- | --------------- |
| Generation inputs      | Page-owned                   | Page-owned      | PASS            |
| Payload construction   | Page function                | Generation hook | PASS            |
| Service call           | Page function                | Generation hook | PASS            |
| Generated output state | Page-owned                   | Hook-owned      | PASS            |
| Metadata state         | Page-owned                   | Hook-owned      | PASS            |
| Loading state          | Not explicit or page-managed | Hook-owned      | PASS            |
| Success formatting     | Page function                | Generation hook | PASS            |
| Error formatting       | Page function                | Generation hook | PASS            |
| Download               | Page function                | Generation hook | PASS            |
| Generation reset       | Multiple setters             | Hook action     | PASS            |
| Extraction integration | Working                      | Working         | PASS            |
| Editable flow name     | Working                      | Working         | PASS            |
| Editable rules         | Working                      | Working         | PASS            |
| SOAP subflow           | Working                      | Working         | PASS            |
| Repeated generation    | Working                      | Working         | PASS            |
| Failure recovery       | Working                      | Working         | PASS            |

---

## Architectural Verification

The following boundaries were reviewed:

* The generation hook contains no JSX.
* The generation hook does not manage requirement-file selection.
* The generation hook does not perform requirement extraction.
* The generation hook does not manage editable rules.
* The generation hook does not manage editable flow name.
* The generation hook does not manage normalized JSON visibility.
* The page no longer directly calls the generation service.
* The page no longer directly formats generated output.
* The page no longer directly formats generation errors.
* The page no longer directly calls the download helper.
* Existing services and helpers remain unchanged.

Expected result:

```text
PASS
```

The Day 38 implementation preserves clear architectural boundaries.

---

## Known Non-Blocking Notes

The Next.js build may continue to display the existing workspace-root warning when multiple lockfiles are present.

Example:

```text
Next.js inferred your workspace root, but it may not be correct.
Multiple lockfiles were detected.
```

This warning is unrelated to the Day 38 generation-hook refactor when the production build otherwise succeeds.

Any existing warning should be recorded honestly in the lint or build result sections.

---

## Final Test Summary

Complete this section after running lint, build, and browser validation.

```text
DAY 38 — FLOW GENERATION HOOK REFACTOR

Lint:
[ ] PASS
[ ] PASS WITH NON-BLOCKING WARNINGS
[ ] FAIL

Production Build:
[ ] PASS
[ ] PASS WITH NON-BLOCKING WARNINGS
[ ] FAIL

Browser Validation:
[ ] PASS
[ ] PASS WITH NON-BLOCKING WARNINGS
[ ] FAIL

Overall Status:
[ ] PASS
[ ] PASS WITH NON-BLOCKING WARNINGS
[ ] FAIL
```

## Final Notes

```text
Record the actual Day 38 validation results here before marking the day complete.
```
