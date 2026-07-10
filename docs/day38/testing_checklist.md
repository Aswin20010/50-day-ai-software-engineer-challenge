# Day 38 — Flow Generation Hook Testing Checklist

## Purpose

This checklist verifies that the Day 38 flow-generation hook refactor is complete, type-safe, behavior-preserving, and ready for final acceptance.

Day 38 moves Node-RED flow generation and generated-flow download logic out of:

```text
src/app/page.tsx
```

and into:

```text
src/hooks/useFlowGeneration.ts
```

The refactor must preserve all existing generation, formatting, metadata, error, reset, extraction-integration, and download behavior.

---

## 1. Required File Checklist

Confirm that the required Day 38 files exist.

* [ ] `docs/day38/flow_generation_hook_plan.md`
* [ ] `src/types/day38.ts`
* [ ] `src/hooks/useFlowGeneration.ts`
* [ ] `docs/day38/flow_generation_hook_test_notes.md`
* [ ] `docs/day38/testing_checklist.md`
* [ ] `docs/day38/day38_completion_report.md`

Confirm that the page was updated.

* [ ] `src/app/page.tsx`

---

## 2. Hook File Validation

Review:

```text
src/hooks/useFlowGeneration.ts
```

Confirm:

* [ ] The file begins with `"use client"`.
* [ ] React hooks are imported correctly.
* [ ] The hook is named `useFlowGeneration`.
* [ ] The hook returns generation-specific state and actions.
* [ ] No JSX exists in the hook.
* [ ] No UI component is imported into the hook.
* [ ] No requirement-extraction logic exists in the hook.
* [ ] No rule-editing logic exists in the hook.
* [ ] No flow-name input logic exists in the hook.
* [ ] No normalized JSON visibility logic exists in the hook.
* [ ] No unrelated page state is managed by the hook.
* [ ] No `any` type was introduced.
* [ ] Hook callback dependencies are complete.
* [ ] No infinite render loop is introduced.

---

## 3. Hook State Checklist

Confirm that the generation hook manages:

* [ ] `generated`
* [ ] `generationMetadata`
* [ ] `isGenerating`

Confirm that the generation hook exposes:

* [ ] `generateFlow`
* [ ] `downloadFlow`
* [ ] `resetGeneration`

Confirm that the generation hook does not permanently manage:

* [ ] `flowName`
* [ ] `rules`
* [ ] `includeSoapSubflow`
* [ ] `normalizedExtraction`
* [ ] `requirementFile`
* [ ] `showNormalizedJson`
* [ ] Extraction status
* [ ] Extraction metadata
* [ ] Missing fields

---

## 4. Initial State Checklist

Open the application before generating a flow.

* [ ] `generated` is an empty string.
* [ ] `generationMetadata` is `null`.
* [ ] `isGenerating` is `false`.
* [ ] No generated flow is visible.
* [ ] No generation error output is visible.
* [ ] Download does nothing when output is empty.
* [ ] Extraction state remains independent.
* [ ] Editable flow name remains available.
* [ ] Editable rules remain available.
* [ ] SOAP subflow state remains page-owned.

---

## 5. Page Import Checklist

Review:

```text
src/app/page.tsx
```

Confirm that the page imports:

* [ ] `useFlowGeneration`
* [ ] `useExtractionWorkflow`
* [ ] `RequirementExtractionPanel`
* [ ] `RuleBuilderPanel`
* [ ] `GenerationOutputPanel`
* [ ] Existing `BusinessRule` type

Confirm that the page no longer imports directly:

* [ ] `buildFlowGenerationPayload`
* [ ] `generateNodeRedFlow`
* [ ] `downloadJsonText`
* [ ] `formatGeneratedFlowOutput`
* [ ] `formatGenerationErrorOutput`
* [ ] `GenerationMetadata`, if it is no longer needed outside the hook

---

## 6. Removed Page State Checklist

Confirm that these generation-specific state declarations were removed from `page.tsx`:

* [ ] `generated`
* [ ] `generationMetadata`
* [ ] `isGenerating`, if previously page-owned

Confirm that these page-level states remain:

* [ ] `flowName`
* [ ] `rules`
* [ ] `includeSoapSubflow`
* [ ] `requirementFile`
* [ ] `showNormalizedJson`

Confirm that extraction state remains owned by `useExtractionWorkflow`.

---

## 7. Removed Page Function Checklist

Confirm that these direct page functions were removed:

* [ ] Page-level `generateFlow`
* [ ] Page-level `downloadFlow`
* [ ] Direct generation error formatting
* [ ] Direct successful output formatting
* [ ] Direct generation service call
* [ ] Direct generation metadata setters
* [ ] Direct generated-output setters

The page should delegate generation and download to the hook.

---

## 8. Day 38 Type Validation

Review:

```text
src/types/day38.ts
```

Confirm that the file defines or exports the active contracts required by the hook.

Possible contracts include:

* [ ] `GenerateFlowInput`
* [ ] `FlowGenerationState`
* [ ] `FlowGenerationActions`
* [ ] `UseFlowGenerationResult`
* [ ] `FlowGenerationSuccessResult`
* [ ] `FlowGenerationFailureResult`

Confirm:

* [ ] Existing `BusinessRule` is reused.
* [ ] Existing `NormalizedExtraction` is reused.
* [ ] Existing `GenerationMetadata` is reused.
* [ ] No duplicate domain model is introduced.
* [ ] No invalid import exists.
* [ ] No unused type causes lint failure.
* [ ] Nullable metadata is represented explicitly.
* [ ] Generated output is typed as `string`.
* [ ] Action return types are explicit.

---

## 9. Existing Helper Reuse Checklist

Confirm that the hook reuses:

```text
src/services/day33/flowGenerationService.ts
src/lib/day35/generatedOutputHelpers.ts
src/lib/day35/downloadHelpers.ts
```

Verify that the hook uses:

* [ ] `buildFlowGenerationPayload`
* [ ] `generateNodeRedFlow`
* [ ] `formatGeneratedFlowOutput`
* [ ] `formatGenerationErrorOutput`
* [ ] `downloadJsonText`

Confirm:

* [ ] Existing helpers were not duplicated.
* [ ] Existing service behavior was not redesigned.
* [ ] Existing output format remains unchanged.
* [ ] Existing error format remains unchanged.
* [ ] Existing download behavior remains unchanged.

---

## 10. Generation Input Checklist

Confirm that the page passes current values when generation is requested.

Expected values:

* [ ] Current `flowName`
* [ ] Current `rules`
* [ ] Current `includeSoapSubflow`
* [ ] Current `normalizedExtraction`

Conceptual call:

```ts
await generateFlow({
  flowName,
  includeSoapSubflow,
  normalizedExtraction,
  rules,
});
```

Confirm:

* [ ] The hook does not retain stale editable inputs.
* [ ] The hook does not duplicate page state.
* [ ] The latest user edits are used.
* [ ] Input objects are not mutated.
* [ ] Rule order is preserved.

---

## 11. Payload Construction Checklist

Confirm that the hook calls:

```ts
buildFlowGenerationPayload({
  flowName,
  includeSoapSubflow,
  normalizedExtraction,
  rules,
});
```

Verify:

* [ ] Flow name is passed unchanged.
* [ ] SOAP subflow state is passed unchanged.
* [ ] Normalized extraction is passed unchanged.
* [ ] Business rules are passed unchanged.
* [ ] No payload field is renamed.
* [ ] No payload field is removed.
* [ ] No extra payload transformation is introduced.
* [ ] The existing helper remains the source of truth.

---

## 12. Generation Start Checklist

Start a valid generation request.

* [ ] `isGenerating` becomes `true`.
* [ ] Previous generation metadata is cleared.
* [ ] The payload is built once.
* [ ] The generation service is called once.
* [ ] Extraction state remains unchanged.
* [ ] Flow name remains unchanged.
* [ ] Rules remain unchanged.
* [ ] SOAP subflow state remains unchanged.
* [ ] The page remains responsive.

---

## 13. Generation Service Checklist

Confirm that the hook calls:

```ts
await generateNodeRedFlow(payload);
```

Verify:

* [ ] The existing service is reused.
* [ ] The helper-built payload is passed directly.
* [ ] The call remains asynchronous.
* [ ] No duplicate request is introduced.
* [ ] `page.tsx` no longer calls the service directly.
* [ ] Service errors are caught by the hook.
* [ ] The service file was not modified unnecessarily.

---

## 14. Successful Generation Checklist

Generate a flow using valid inputs.

* [ ] The service returns successfully.
* [ ] Generation metadata is stored.
* [ ] Generated flow output is formatted.
* [ ] Previous generated output is replaced.
* [ ] Previous error output is replaced.
* [ ] `isGenerating` returns to `false`.
* [ ] Extraction state remains unchanged.
* [ ] Flow name remains editable.
* [ ] Rules remain editable.
* [ ] The result is displayed in `GenerationOutputPanel`.

---

## 15. Successful Output Formatting Checklist

Confirm that success output uses:

```ts
formatGeneratedFlowOutput(data.flow);
```

Verify:

* [ ] No manual duplicate JSON formatting exists in the hook.
* [ ] The output remains a string.
* [ ] The displayed structure matches earlier behavior.
* [ ] The output remains compatible with download.
* [ ] Repeated generation replaces the old string.
* [ ] No unrelated wrapper text is added.

---

## 16. Generation Metadata Checklist

Inspect generation metadata.

* [ ] Metadata is stored from `data.generationMetadata`.
* [ ] Missing metadata results in `null`.
* [ ] Metadata is cleared before a new generation attempt.
* [ ] Metadata is cleared after failure.
* [ ] Metadata is passed unchanged to `GenerationOutputPanel`.
* [ ] Metadata from an older request is not retained.
* [ ] Resetting generation clears metadata.

---

## 17. Loading-State Checklist

Confirm that `isGenerating` behaves correctly.

* [ ] Initial value is `false`.
* [ ] It becomes `true` before generation.
* [ ] It remains `true` during the request.
* [ ] It becomes `false` after success.
* [ ] It becomes `false` after failure.
* [ ] It becomes `false` after reset.
* [ ] It does not remain active after an exception.
* [ ] Repeated clicks are prevented or ignored while active.

---

## 18. Generation Failure Checklist

Trigger a generation service failure.

* [ ] The error is caught inside the hook.
* [ ] Generation metadata is cleared.
* [ ] The error is passed to the existing formatter.
* [ ] Formatted error output is stored in `generated`.
* [ ] `isGenerating` returns to `false`.
* [ ] Extraction state remains unchanged.
* [ ] Flow name remains editable.
* [ ] Rules remain editable.
* [ ] SOAP subflow state remains unchanged.
* [ ] The page remains usable.

---

## 19. Error Formatting Checklist

Confirm that errors use:

```ts
formatGenerationErrorOutput(error);
```

Verify:

* [ ] Existing error output format is preserved.
* [ ] Unknown errors are handled by the helper.
* [ ] The hook does not create a second error-formatting system.
* [ ] The formatted error appears in the output panel.
* [ ] A later success replaces the error output.
* [ ] Resetting generation clears the error output.

---

## 20. Retry After Failure Checklist

After a failed request, run generation again with valid inputs.

* [ ] The previous error output is replaced.
* [ ] Metadata is cleared before retry.
* [ ] Loading starts normally.
* [ ] Current page inputs are used.
* [ ] The service is called again.
* [ ] Successful metadata is stored.
* [ ] Successful output is formatted.
* [ ] Loading returns to `false`.
* [ ] No failed state remains visible.

---

## 21. Repeated Generation Checklist

Run generation more than once.

* [ ] The same inputs can be generated again.
* [ ] Updated flow names are used.
* [ ] Updated rules are used.
* [ ] Updated SOAP subflow state is used.
* [ ] Updated normalized extraction is used.
* [ ] Previous metadata is cleared.
* [ ] Latest output replaces earlier output.
* [ ] Results do not accumulate.
* [ ] No duplicate service call occurs from one action.
* [ ] Download uses the latest output.

---

## 22. Flow Name Checklist

Edit the flow name before generation.

* [ ] The current edited value is passed into the hook.
* [ ] The payload contains the edited value.
* [ ] Generation does not reset the input.
* [ ] The hook does not maintain a conflicting flow-name state.
* [ ] A second edit is used by the next request.
* [ ] Extraction-populated flow names continue to work.

---

## 23. Rule Editing Checklist

Add, edit, and remove rules before generation.

* [ ] Added rules are included in the payload.
* [ ] Updated values are included.
* [ ] Removed rules are excluded.
* [ ] Rule order remains unchanged.
* [ ] Generation does not reset rules.
* [ ] Resetting generation does not clear rules.
* [ ] The hook does not maintain a duplicate rule list.
* [ ] A later generation uses the latest rules.

---

## 24. SOAP Subflow Checklist

Generate with SOAP subflow enabled and disabled.

### Enabled

* [ ] `includeSoapSubflow` is `true`.
* [ ] The payload receives `true`.
* [ ] SOAP-related generation behavior is preserved.

### Disabled

* [ ] `includeSoapSubflow` is `false`.
* [ ] The payload receives `false`.
* [ ] No stale enabled value remains.

Also confirm:

* [ ] Adding a SOAP rule enables the state.
* [ ] Removing the last SOAP rule disables the state.
* [ ] Resetting generation does not alter SOAP state.

---

## 25. Normalized Extraction Checklist

Generate using the latest extraction result.

* [ ] Current `normalizedExtraction` is passed.
* [ ] The generation hook does not duplicate extraction state.
* [ ] A new extraction updates the next generation input.
* [ ] Generation failure does not modify extraction.
* [ ] Resetting generation does not modify extraction.
* [ ] Missing normalized extraction is handled as before.
* [ ] Payload behavior remains unchanged.

---

## 26. Reset Generation Checklist

Call `resetGeneration`.

* [ ] `generated` becomes an empty string.
* [ ] `generationMetadata` becomes `null`.
* [ ] `isGenerating` becomes `false`.
* [ ] Extraction state remains unchanged.
* [ ] Flow name remains unchanged.
* [ ] Rules remain unchanged.
* [ ] SOAP subflow state remains unchanged.
* [ ] Requirement-file state remains unchanged.
* [ ] The hook remains usable afterward.

---

## 27. Extraction Start Reset Checklist

Generate a flow, then start extraction again.

* [ ] The extraction-start callback calls `resetGeneration`.
* [ ] Previous generated output is cleared.
* [ ] Previous metadata is cleared.
* [ ] Generation loading becomes inactive.
* [ ] Extraction begins normally.
* [ ] No stale generated flow remains visible.
* [ ] Editable flow name and rules follow existing behavior.

---

## 28. Requirement File Change Checklist

Change the selected requirement file after generation.

* [ ] Extraction state resets.
* [ ] Generation state resets.
* [ ] Generated output is cleared.
* [ ] Generation metadata is cleared.
* [ ] Loading becomes inactive.
* [ ] Normalized JSON visibility is disabled.
* [ ] No stale output remains.
* [ ] The new file can be extracted normally.

---

## 29. Requirement File Removal Checklist

Remove the selected requirement file after generation.

* [ ] `requirementFile` becomes `null`.
* [ ] Extraction state resets.
* [ ] Generation state resets.
* [ ] Generated output is empty.
* [ ] Metadata is `null`.
* [ ] Loading is `false`.
* [ ] Rules follow existing removal behavior.
* [ ] SOAP subflow follows existing removal behavior.
* [ ] Default flow name follows existing behavior.
* [ ] No stale output remains.

---

## 30. Download Without Output Checklist

Call download before successful generation.

* [ ] No download occurs.
* [ ] No error is thrown.
* [ ] Generation state remains unchanged.
* [ ] Extraction state remains unchanged.
* [ ] Page remains responsive.
* [ ] No empty file is created.

---

## 31. Successful Download Checklist

Generate and download a flow.

* [ ] The hook calls `downloadJsonText(generated)`.
* [ ] The displayed string is passed unchanged.
* [ ] No second generation request occurs.
* [ ] Download does not clear output.
* [ ] Download does not clear metadata.
* [ ] Download does not change loading state.
* [ ] Extraction state remains unchanged.
* [ ] Downloaded content matches displayed output.

---

## 32. Latest Output Download Checklist

Generate two different outputs and download after the second.

* [ ] The second output is downloaded.
* [ ] The first output is not used accidentally.
* [ ] No stale closure captures old output.
* [ ] Metadata remains associated with the latest request.
* [ ] Download remains available after repeated generation.

---

## 33. Duplicate Request Protection Checklist

Review overlapping generation protection.

* [ ] A second request is not intentionally started while one is active.
* [ ] `isGenerating` is available to the page.
* [ ] One click produces one service request.
* [ ] Loading state remains consistent.
* [ ] Reset behavior does not create an overlapping request.
* [ ] The final displayed output corresponds to the intended request.
* [ ] No accidental double submission occurs.

---

## 34. Generation Output Panel Checklist

Review:

```text
src/components/day32/GenerationOutputPanel.tsx
```

Confirm:

* [ ] `generated` comes from the hook.
* [ ] `generationMetadata` comes from the hook.
* [ ] Generate action calls the hook.
* [ ] Download action calls the hook.
* [ ] Existing labels remain unchanged.
* [ ] Existing layout remains unchanged.
* [ ] Existing formatting remains unchanged.
* [ ] `isGenerating` is passed if the panel supports it.
* [ ] Button disabling behavior remains correct.

---

## 35. Extraction Regression Checklist

Run requirement extraction after Day 38 integration.

* [ ] File selection still works.
* [ ] File reading still works.
* [ ] Extraction service still runs.
* [ ] `useLLM: false` remains unchanged.
* [ ] Raw extraction appears.
* [ ] Normalized extraction appears.
* [ ] Metadata appears.
* [ ] Missing fields appear.
* [ ] Flow name updates.
* [ ] Rules populate.
* [ ] Extraction failure recovery works.
* [ ] Day 38 does not alter extraction output.

---

## 36. Rule Builder Regression Checklist

Test rule editing before and after generation.

* [ ] Rules can be added.
* [ ] Rules can be updated.
* [ ] Rules can be removed.
* [ ] Rule order remains stable.
* [ ] SOAP state stays synchronized.
* [ ] Generation uses the latest list.
* [ ] Resetting generation does not clear rules.
* [ ] Generation failure does not clear rules.
* [ ] The rule builder remains editable.

---

## 37. State Ownership Checklist

Confirm final ownership.

### `useExtractionWorkflow`

* [ ] Raw extraction
* [ ] Normalized extraction
* [ ] Extraction metadata
* [ ] Missing fields
* [ ] Extraction status
* [ ] Extraction loading state

### `useFlowGeneration`

* [ ] Generated output
* [ ] Generation metadata
* [ ] Generation loading state
* [ ] Payload and service coordination
* [ ] Success and error formatting
* [ ] Download coordination
* [ ] Generation reset

### `page.tsx`

* [ ] Flow name
* [ ] Editable rules
* [ ] SOAP subflow state
* [ ] Requirement-file selection
* [ ] Normalized JSON visibility
* [ ] UI rendering

Confirm:

* [ ] No state is owned in two places unnecessarily.
* [ ] No duplicate setter remains.
* [ ] No circular state synchronization is introduced.

---

## 38. Hook Dependency Checklist

Review the generation hook callbacks.

* [ ] `generateFlow` has the correct dependencies.
* [ ] `downloadFlow` uses the latest generated output.
* [ ] `resetGeneration` is stable.
* [ ] State setters are not added unnecessarily.
* [ ] Imported helpers do not require dependencies.
* [ ] No stale input is captured.
* [ ] ESLint hook rules pass.
* [ ] No unnecessary callback recreation occurs.

---

## 39. Type Safety Checklist

Confirm the implementation uses:

* [ ] `BusinessRule`
* [ ] `NormalizedExtraction`
* [ ] `GenerationMetadata`
* [ ] Day 38 generation input and result types

Confirm:

* [ ] No `any` is used.
* [ ] `unknown` values are narrowed safely.
* [ ] Metadata uses explicit `null`.
* [ ] Generated output is a string.
* [ ] Hook actions have explicit return types.
* [ ] Page inputs are compatible.
* [ ] Service results are assigned safely.
* [ ] Existing domain types are reused.

---

## 40. Import Cleanup Checklist

Review `src/app/page.tsx`.

Confirm the following are removed when no longer needed:

* [ ] Generation service imports
* [ ] Payload-builder import
* [ ] Download-helper import
* [ ] Success-formatting helper import
* [ ] Error-formatting helper import
* [ ] `GenerationMetadata` type import
* [ ] Unused state setter imports
* [ ] Duplicate generation-related imports

Confirm:

* [ ] `useFlowGeneration` is imported.
* [ ] No unused import remains.
* [ ] No incorrect path remains.

---

## 41. Lint Validation Checklist

Run:

```powershell
npm run lint
```

Confirm:

* [ ] No unused import remains.
* [ ] No unused variable remains.
* [ ] No hook dependency warning appears.
* [ ] No React hook violation appears.
* [ ] No explicit `any` warning appears.
* [ ] No Day 38 lint error appears.
* [ ] Existing unrelated warnings are documented separately.

Lint result:

```text
[ ] PASS
[ ] PASS WITH NON-BLOCKING WARNINGS
[ ] FAIL
```

Notes:

```text
Record actual lint output here.
```

---

## 42. TypeScript Build Checklist

Run:

```powershell
npm run build
```

Confirm:

* [ ] TypeScript compilation succeeds.
* [ ] `useFlowGeneration` resolves.
* [ ] Day 38 types resolve.
* [ ] Generation-service imports resolve.
* [ ] Formatting-helper imports resolve.
* [ ] Download-helper import resolves.
* [ ] `page.tsx` hook usage is compatible.
* [ ] Output-panel props remain compatible.
* [ ] No nullable assignment error appears.
* [ ] No invalid return type appears.
* [ ] Production build completes successfully.

Build result:

```text
[ ] PASS
[ ] PASS WITH NON-BLOCKING WARNINGS
[ ] FAIL
```

Notes:

```text
Record actual build output here.
```

---

## 43. Browser Validation Checklist

Run:

```powershell
npm run dev
```

Confirm:

* [ ] Page loads normally.
* [ ] No blank screen appears.
* [ ] No hydration error appears.
* [ ] No runtime import error appears.
* [ ] Requirement extraction works.
* [ ] Flow name remains editable.
* [ ] Rules remain editable.
* [ ] Generate Flow works.
* [ ] Generation loading behavior works.
* [ ] Generated output appears.
* [ ] Generation metadata appears.
* [ ] Generation errors appear correctly.
* [ ] Retry after failure works.
* [ ] Repeated generation works.
* [ ] Download works.
* [ ] New extraction clears old generation output.
* [ ] File change clears generation state.
* [ ] File removal clears generation state.
* [ ] No unexpected console error appears.

Browser result:

```text
[ ] PASS
[ ] PASS WITH NON-BLOCKING WARNINGS
[ ] FAIL
```

Notes:

```text
Record actual browser validation here.
```

---

## 44. Regression Checklist

Confirm that unrelated behavior remains unchanged.

* [ ] Page layout is unchanged.
* [ ] Existing styling is unchanged.
* [ ] Requirement extraction panel is unchanged.
* [ ] Rule builder is unchanged.
* [ ] Generation output panel is unchanged.
* [ ] Flow-generation payload is unchanged.
* [ ] Generation service behavior is unchanged.
* [ ] Generated output format is unchanged.
* [ ] Error output format is unchanged.
* [ ] Downloaded content is unchanged.
* [ ] Default flow name is unchanged.
* [ ] SOAP subflow behavior is unchanged.
* [ ] No unrelated file was modified unnecessarily.

---

## 45. Code Quality Checklist

* [ ] `page.tsx` is smaller than before.
* [ ] Generation logic is centralized.
* [ ] State ownership is clear.
* [ ] The hook has one clear responsibility.
* [ ] Editable inputs remain page-owned.
* [ ] Generation resets use one action.
* [ ] Existing helpers are reused.
* [ ] No dead code remains.
* [ ] No commented-out implementation remains.
* [ ] No temporary debug logging remains.
* [ ] No unnecessary abstraction is introduced.
* [ ] The implementation is easier to maintain.
* [ ] The hook can be reused by another component.

---

## 46. Documentation Checklist

* [ ] The Day 38 plan matches the final implementation.
* [ ] Test notes describe actual behavior.
* [ ] This checklist matches the active hook API.
* [ ] Known warnings are documented.
* [ ] Unverified tests are not marked as passed.
* [ ] Failed checks are recorded honestly.
* [ ] The completion report will reflect actual results.

---

## 47. Known Warning Review

If the Next.js build displays the existing workspace-root warning:

* [ ] Confirm the warning is unrelated to Day 38.
* [ ] Confirm the build otherwise completes.
* [ ] Record the warning in test notes.
* [ ] Do not remove lockfiles without understanding their purpose.
* [ ] Do not classify the warning as a functional regression.

Example:

```text
Next.js inferred your workspace root, but it may not be correct.
Multiple lockfiles were detected.
```

---

## 48. Final Acceptance Checklist

Day 38 can be marked complete only when:

* [ ] `src/types/day38.ts` exists.
* [ ] `src/hooks/useFlowGeneration.ts` exists.
* [ ] `page.tsx` uses the generation hook.
* [ ] Generated output state was removed from the page.
* [ ] Generation metadata state was removed from the page.
* [ ] Payload construction moved into the hook.
* [ ] Generation service calls moved into the hook.
* [ ] Success formatting moved into the hook.
* [ ] Error formatting moved into the hook.
* [ ] Download coordination moved into the hook.
* [ ] Generation reset behavior is centralized.
* [ ] Extraction-start reset works.
* [ ] File-change reset works.
* [ ] File-removal reset works.
* [ ] Flow-name editing still works.
* [ ] Rule editing still works.
* [ ] SOAP subflow behavior still works.
* [ ] Normalized extraction integration still works.
* [ ] Repeated generation works.
* [ ] Failure recovery works.
* [ ] Download works.
* [ ] Lint passes.
* [ ] Production build passes.
* [ ] Browser validation passes.
* [ ] No functional regression is identified.
* [ ] Test notes are complete.
* [ ] Completion report is ready.

---

## Final Result

```text
DAY 38 TESTING STATUS

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

Overall:
[ ] PASS
[ ] PASS WITH NON-BLOCKING WARNINGS
[ ] FAIL
```

## Final Notes

```text
Record the final Day 38 validation summary here.
```
