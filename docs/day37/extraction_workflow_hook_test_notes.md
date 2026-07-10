# Day 37 — Extraction Workflow Hook Test Notes

## Purpose

This document records the validation performed for the Day 37 extraction workflow hook refactor.

Day 37 moved requirement-extraction workflow logic out of `src/app/page.tsx` and into:

```text
src/hooks/useExtractionWorkflow.ts
```

The goal of testing is to confirm that the new hook preserves the existing extraction behavior while reducing the responsibility of the page component.

---

## Files Validated

The following files were reviewed:

```text
src/hooks/useExtractionWorkflow.ts
src/app/page.tsx
src/types/day37.ts
```

The hook also depends on the existing project files:

```text
src/services/day33/requirementExtractionService.ts
src/mappers/day34/extractedRequirementRuleMapper.ts
src/lib/day36/extractionStateHelpers.ts
src/types/day32.ts
```

---

## Test Objective

The main validation objective is to confirm that the extraction workflow behaves the same after being moved into a custom React hook.

The following behavior must remain unchanged:

* Requirement-file selection
* Requirement-file reading
* Requirement extraction
* Raw extraction storage
* Normalized extraction storage
* Extraction metadata storage
* Missing-field handling
* Flow-name population
* Business-rule population
* Extraction status messages
* Generated output reset behavior
* Re-extraction behavior
* Error handling
* Recovery after failure

---

## Hook Responsibility Validation

The new hook was reviewed to confirm that it owns extraction-specific behavior.

The hook manages:

* Raw extraction state
* Normalized extraction state
* Extraction metadata
* Missing fields
* Extraction status
* Extraction loading state
* File reading
* Requirement extraction
* Extraction-state reset
* Rule mapping callbacks
* Flow-name callbacks
* Extraction failure handling

Result:

```text
PASS
```

The extraction workflow is now centralized inside the hook.

---

## Page Responsibility Validation

`src/app/page.tsx` was reviewed to confirm that it retains page-level and generation-related responsibilities.

The page continues to manage:

* Flow-name editing
* Business-rule editing
* Generated output
* Generation metadata
* SOAP subflow selection
* Requirement-file selection
* Normalized JSON visibility
* Flow generation
* Flow download
* UI rendering

Result:

```text
PASS
```

The page no longer contains the complete extraction workflow.

---

## Initial Hook State Test

The application was opened before selecting a requirement file.

Expected behavior:

* `rawExtracted` is `null`.
* `normalizedExtraction` is `null`.
* `extractionMetadata` is `null`.
* `missingFields` is empty.
* `extractStatus` is empty.
* `isExtracting` is `false`.
* No stale extraction output is visible.
* The default flow name remains visible.
* The business-rule list uses the existing page state.

Result:

```text
PASS
```

The hook starts with the expected empty extraction state.

---

## Requirement File Selection Test

A supported requirement file was selected.

Expected behavior:

* The page stores the selected file.
* Existing extraction output is reset.
* Normalized JSON visibility is disabled.
* Existing generated output is cleared.
* Existing generation metadata is cleared.
* Extraction does not run automatically.
* The selected file can be passed to the hook when the user clicks Extract.

Result:

```text
PASS
```

File selection and reset behavior remain functional.

---

## Missing Requirement File Test

The extraction action was reviewed when no file was available.

Expected behavior:

* Extraction is not attempted.
* No file-read operation occurs.
* No request is sent to the extraction service.
* The page remains responsive.

Result:

```text
PASS
```

The workflow safely prevents extraction without a file.

---

## Extraction Start Test

A valid file was selected and extraction was started.

Expected behavior:

* `isExtracting` becomes `true`.
* `extractStatus` becomes `Extracting requirements...`.
* Existing raw extraction is cleared.
* Existing normalized extraction is cleared.
* Existing metadata is cleared.
* Existing missing fields are cleared.
* Normalized JSON visibility is disabled.
* Generated flow output is cleared.
* Generation metadata is cleared.

Result:

```text
PASS
```

The hook correctly prepares the application for a new extraction attempt.

---

## File Reading Test

The hook reads the selected file using:

```ts
await file.text();
```

Expected behavior:

* The complete text content is read.
* The current `File` object is used.
* No additional browser dependency is introduced.
* File reading remains asynchronous.
* Read failures are handled by the extraction error path.

Result:

```text
PASS
```

File reading remains compatible with the previous implementation.

---

## Empty File Test

An empty file was used.

Expected behavior:

* Empty or whitespace-only text is detected.
* The extraction service is not called.
* A clear error message is displayed.
* Extraction state remains empty.
* `isExtracting` returns to `false`.
* The page remains usable for another attempt.

Expected message format:

```text
The selected file "<filename>" is empty.
```

Result:

```text
PASS
```

Empty files are handled safely.

---

## Extraction Service Call Test

A valid requirement file was processed.

Expected behavior:

* The existing `extractRequirements` service is called.
* The request includes the file text.
* `useLLM: false` remains unchanged.
* The service import remains unchanged.
* No duplicate extraction request is introduced.

Expected request shape:

```ts
{
  text,
  useLLM: false,
}
```

Result:

```text
PASS
```

The hook preserves the existing extraction-service behavior.

---

## Raw Extraction Test

The service returned a raw extraction result.

Expected behavior:

* `data.rawExtracted` is stored.
* Object-shaped raw extraction data is preserved.
* Missing raw extraction data results in `null`.
* A new extraction replaces the previous raw result.
* Raw extraction remains compatible with `RequirementExtractionPanel`.

Result:

```text
PASS
```

Raw extraction output is managed correctly by the hook.

---

## Normalized Extraction Test

The extraction response was mapped using:

```ts
mapExtractionResponseToState(data)
```

Expected behavior:

* The existing Day 36 mapper remains in use.
* Normalized extraction is stored using the existing `NormalizedExtraction` type.
* The hook does not duplicate normalization logic.
* A new extraction replaces the previous normalized result.
* `null` is used when no normalized result is available.

Result:

```text
PASS
```

Normalized extraction behavior remains unchanged.

---

## Extraction Metadata Test

The extraction response included metadata.

Expected behavior:

* Metadata is stored from `data.extractionMetadata`.
* Missing metadata results in `null`.
* Metadata from a previous extraction is cleared before a new attempt.
* Metadata is passed unchanged to `RequirementExtractionPanel`.

Result:

```text
PASS
```

Extraction metadata continues to work correctly.

---

## Missing Fields Test

Files with complete and incomplete requirements were reviewed.

Expected behavior for complete requirements:

* `missingFields` is empty.

Expected behavior for incomplete requirements:

* Missing fields are returned from the extraction service.
* The field names are stored in the hook.
* The same values are passed to both extraction and rule-builder panels.
* Previous missing fields are cleared before re-extraction.

Result:

```text
PASS
```

Missing-field behavior remains unchanged.

---

## Flow Name Test

The extraction response contained an extracted flow name.

Expected behavior:

* `data.extracted.flowName` is read.
* The value is validated as a string.
* Leading and trailing whitespace is removed.
* Empty flow names are ignored.
* Valid flow names are passed to `onFlowNameExtracted`.
* The page updates its editable flow-name state.
* The user can still manually edit the flow name afterward.

Result:

```text
PASS
```

Extracted and manually editable flow-name behavior is preserved.

---

## Business Rule Population Test

The hook used the existing rule mapper:

```ts
buildRulesFromExtraction({
  suggestedNodes,
  fallbackRules,
  extracted,
})
```

Expected behavior:

* Suggested nodes are passed to the mapper.
* Existing page rules are available as fallback rules.
* Extracted requirement data is passed unchanged.
* Populated rules are returned through `onRulesExtracted`.
* The page updates its editable rule state.
* Users can continue adding, editing, and removing rules after extraction.

Result:

```text
PASS
```

Business-rule mapping remains compatible with the existing rule builder.

---

## Fallback Rule Reference Test

The hook stores the latest fallback rules using a React ref.

Expected behavior:

* The latest page-level rules are available during extraction.
* `extractFromFile` does not need to be recreated after every rule edit.
* Rule edits made before extraction are preserved as fallback input.
* No stale fallback-rule array is used.

Result:

```text
PASS
```

The hook uses the current editable rule state without unnecessary callback recreation.

---

## SOAP Subflow State Test

Extracted rules were reviewed for the `SOAP-SUBFLOW` rule.

Expected behavior:

* When extracted rules include `SOAP-SUBFLOW`, `includeSoapSubflow` becomes `true`.
* When extracted rules do not include it, the value becomes `false`.
* Manually adding the rule enables the option.
* Removing the last SOAP subflow rule disables the option.

Result:

```text
PASS
```

SOAP subflow state remains aligned with the editable rule list.

---

## Extraction Success Status Test

A valid extraction completed successfully.

Expected behavior:

* `extractStatus` displays the existing completion message.
* `isExtracting` returns to `false`.
* Raw extraction is available.
* Normalized extraction is available.
* Metadata is available when returned.
* Missing fields are available.
* Flow name is updated when returned.
* Rules are populated.
* No error status remains.

Expected status:

```text
Requirement extraction completed. Review normalized extraction, gaps, and highlighted missing fields.
```

Result:

```text
PASS
```

Successful extraction completes with the expected state.

---

## Extraction Failure Test

The extraction service was allowed to fail.

Expected behavior:

* The error is caught inside the hook.
* The error is logged for development diagnostics.
* Raw extraction is cleared.
* Normalized extraction is cleared.
* Metadata is cleared.
* Missing fields are cleared.
* The error message is displayed through `extractStatus`.
* Unknown errors use the existing fallback message.
* `isExtracting` returns to `false`.

Fallback message:

```text
Requirement extraction failed.
```

Result:

```text
PASS
```

Extraction failures remain controlled and recoverable.

---

## Recovery After Failure Test

A failed extraction was followed by a valid extraction.

Expected behavior:

* The previous error status is replaced.
* Extraction loading state starts normally.
* The valid file is read.
* The extraction service is called.
* Raw extraction is populated.
* Normalized extraction is populated.
* Metadata is populated when returned.
* Missing fields are populated.
* Flow name and rules are updated.
* `isExtracting` returns to `false`.

Result:

```text
PASS
```

The workflow successfully recovers after an extraction failure.

---

## Re-Extraction Test

The same file and different files were extracted more than once.

Expected behavior:

* Previous extraction output is cleared before each new attempt.
* Results do not accumulate.
* Missing fields are replaced.
* Normalized extraction is replaced.
* Metadata is replaced.
* Raw extraction is replaced.
* Rules are repopulated from the latest response.
* Generated flow output is cleared before extraction.
* Generation metadata is cleared before extraction.

Result:

```text
PASS
```

Re-extraction replaces the previous result correctly.

---

## Reset Extraction Test

The hook’s `resetExtraction` action was reviewed.

Expected behavior:

* Day 36 reset helpers are reused.
* Raw extraction becomes `null`.
* Normalized extraction is reset.
* Metadata becomes `null`.
* Missing fields are cleared.
* Extraction status is cleared.
* Loading state becomes `false`.
* Page-level generated output is reset separately.
* Page-level flow name and editable rules remain controlled by the page.

Result:

```text
PASS
```

The hook reset behavior is predictable and appropriately scoped.

---

## File Removal Test

A selected file was removed.

Expected behavior:

* `requirementFile` becomes `null`.
* Hook extraction state is reset.
* Generated output is cleared.
* Generation metadata is cleared.
* Normalized JSON visibility is disabled.
* Rules are cleared.
* SOAP subflow state is disabled.
* The default flow name is restored.
* No stale extraction output remains visible.

Result:

```text
PASS
```

File removal restores the page to a clean state.

---

## Normalized JSON Toggle Test

The normalized JSON display toggle was reviewed.

Expected behavior:

* The visibility flag remains page-owned.
* The user can show or hide normalized JSON.
* Starting a new extraction hides the previous normalized JSON view.
* Selecting or removing a file also hides it.
* The hook does not contain UI visibility state.

Result:

```text
PASS
```

UI-only visibility state remains correctly separated from the hook.

---

## Flow Generation Regression Test

A flow was generated after successful extraction.

Expected behavior:

* The page uses the current `flowName`.
* The page uses the current editable `rules`.
* The page uses the current `normalizedExtraction`.
* The page uses the current `includeSoapSubflow` value.
* The generation service remains unchanged.
* Generation metadata is stored.
* Generated output is formatted using the existing helper.
* Extraction-hook changes do not alter flow generation.

Result:

```text
PASS
```

The Node-RED flow generation workflow remains functional.

---

## Generation Error Regression Test

A flow-generation failure was reviewed.

Expected behavior:

* Generation metadata is cleared.
* The existing generation error formatter is used.
* The error is displayed in the generation output.
* Extraction state is not modified.
* The extraction hook does not handle generation errors.

Result:

```text
PASS
```

Extraction and generation error responsibilities remain separate.

---

## Download Regression Test

The generated flow download behavior was reviewed.

Expected behavior:

* Download is skipped when generated output is empty.
* Existing generated JSON text is passed to `downloadJsonText`.
* Extraction-hook changes do not alter download behavior.

Result:

```text
PASS
```

Download behavior remains unchanged.

---

## Callback Stability Test

The page callbacks passed into the hook were reviewed.

Expected behavior:

* Flow-name callback uses `useCallback`.
* Rule callback uses `useCallback`.
* Extraction-start callback uses `useCallback`.
* Stable callbacks reduce unnecessary hook callback recreation.
* The hook dependency list remains accurate.
* No infinite render loop is introduced.

Result:

```text
PASS
```

Hook callback dependencies are stable.

---

## Type Validation

The implementation uses the existing project types:

```text
BusinessRule
NormalizedExtraction
ExtractionMetadata
GenerationMetadata
```

Expected behavior:

* The hook uses `BusinessRule[]` for mapped rules.
* The hook uses `NormalizedExtraction | null`.
* The hook uses `ExtractionMetadata | null`.
* The page and hook share compatible types.
* Generic `unknown`-based Day 37 types do not conflict with the actual workflow.
* No unnecessary `any` type is introduced.

Result:

```text
PASS
```

The extraction hook is aligned with the project’s real data types.

---

## Lint Validation

Run:

```powershell
npm run lint
```

Expected behavior:

* No unused imports remain.
* No unused variables remain.
* Hook dependency arrays pass validation.
* No React hook rule violation appears.
* No Day 37 lint error is reported.

Result:

```text
PASS
```

Any unrelated pre-existing warning should be documented separately.

---

## Production Build Validation

Run:

```powershell
npm run build
```

Expected behavior:

* TypeScript compilation succeeds.
* The hook module resolves.
* The page imports the hook correctly.
* Client-component boundaries remain valid.
* Existing services and mappers resolve.
* The production build completes successfully.

Result:

```text
PASS
```

The Day 37 hook refactor is compatible with the production build.

---

## Browser Validation

Run:

```powershell
npm run dev
```

Validate the following browser behavior:

* Page loads normally.
* Requirement file can be selected.
* Extract button works.
* Loading status appears.
* Extraction result appears.
* Missing fields appear.
* Flow name is populated.
* Rules are populated.
* Rules remain editable.
* Flow generation works.
* Download works.
* Extraction failure is displayed.
* A valid retry succeeds.
* No runtime exception appears.
* No hydration error appears.

Result:

```text
PASS
```

The hook works correctly in the browser.

---

## Behavior Comparison

| Area                    | Before Refactor | After Refactor       | Status |
| ----------------------- | --------------- | -------------------- | ------ |
| File selection          | Page-managed    | Page-managed         | PASS   |
| File reading            | Page workflow   | Hook workflow        | PASS   |
| Extraction service call | Page workflow   | Hook workflow        | PASS   |
| Raw extraction state    | Page-owned      | Hook-owned           | PASS   |
| Normalized state        | Page-owned      | Hook-owned           | PASS   |
| Metadata state          | Page-owned      | Hook-owned           | PASS   |
| Missing fields          | Page-owned      | Hook-owned           | PASS   |
| Flow-name update        | Direct setter   | Hook callback        | PASS   |
| Rule mapping            | Page workflow   | Hook workflow        | PASS   |
| Rule editing            | Page-owned      | Page-owned           | PASS   |
| Loading state           | Implicit status | Hook-owned           | PASS   |
| Extraction status       | Page-owned      | Hook-owned           | PASS   |
| Generated output        | Page-owned      | Page-owned           | PASS   |
| Flow generation         | Page-owned      | Page-owned           | PASS   |
| Reset behavior          | Page workflow   | Hook plus page reset | PASS   |
| Error recovery          | Working         | Working              | PASS   |

---

## Architectural Verification

The following boundaries were confirmed:

* The hook contains no JSX.
* The hook does not render components.
* The hook does not generate Node-RED flows.
* The hook does not download files.
* The hook does not manage normalized JSON visibility.
* The hook does not manage generation metadata.
* The page no longer directly calls the extraction service.
* The page no longer directly maps extraction responses.
* The page no longer stores core extraction result state.
* Day 36 state helpers are reused.
* Existing extraction service and rule mapper remain unchanged.

---

## Known Non-Blocking Notes

The Next.js build may continue to display the existing multiple-lockfile workspace-root warning.

Example:

```text
Next.js inferred your workspace root, but it may not be correct.
Multiple lockfiles were detected.
```

This warning is unrelated to the Day 37 extraction hook refactor when the build otherwise completes successfully.

---

## Final Test Result

```text
DAY 37 — EXTRACTION WORKFLOW HOOK REFACTOR

STATUS: PASS
```

The requirement-extraction workflow was successfully moved into a reusable React hook while preserving the existing extraction, rule mapping, flow generation, and error-handling behavior.
