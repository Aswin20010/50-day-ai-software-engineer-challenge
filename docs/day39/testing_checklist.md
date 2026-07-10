# Day 39 — Page Workflow Composition Testing Checklist

## Purpose

This checklist verifies that the Day 39 page workflow composition refactor is complete, type-safe, behavior-preserving, and ready for final acceptance.

Day 39 introduces:

```text
src/hooks/useApiFactoryWorkflow.ts
```

to compose:

```text
src/hooks/useExtractionWorkflow.ts
src/hooks/useFlowGeneration.ts
```

and move the remaining workflow state and coordination out of:

```text
src/app/page.tsx
```

The page should become primarily presentational while preserving all existing extraction, editing, generation, reset, and download behavior.

---

## 1. Required File Checklist

Confirm that the required Day 39 files exist.

* [ ] `docs/day39/page_workflow_composition_plan.md`
* [ ] `src/types/day39.ts`
* [ ] `src/hooks/useApiFactoryWorkflow.ts`
* [ ] `docs/day39/page_workflow_integration.md`
* [ ] `docs/day39/page_workflow_composition_test_notes.md`
* [ ] `docs/day39/testing_checklist.md`
* [ ] `docs/day39/day39_completion_report.md`

Confirm that the page was updated.

* [ ] `src/app/page.tsx`

---

## 2. Composition Hook File Validation

Review:

```text
src/hooks/useApiFactoryWorkflow.ts
```

Confirm:

* [ ] The file begins with `"use client"`.
* [ ] React hooks are imported correctly.
* [ ] The hook is named `useApiFactoryWorkflow`.
* [ ] The hook calls `useExtractionWorkflow`.
* [ ] The hook calls `useFlowGeneration`.
* [ ] Both lower-level hooks are called unconditionally.
* [ ] No JSX exists in the composition hook.
* [ ] No UI component is imported into the hook.
* [ ] No direct extraction-service call exists.
* [ ] No direct generation-service call exists.
* [ ] No direct payload construction exists.
* [ ] No direct output formatting exists.
* [ ] No direct download-helper call exists.
* [ ] No `any` type was introduced.
* [ ] No circular dependency was introduced.
* [ ] No render loop was introduced.

---

## 3. Composition Hook State Checklist

Confirm that `useApiFactoryWorkflow` manages:

* [ ] `flowName`
* [ ] `rules`
* [ ] `includeSoapSubflow`
* [ ] `requirementFile`
* [ ] `showNormalizedJson`

Confirm that the composition hook does not duplicate lower-level state such as:

* [ ] Raw extraction
* [ ] Normalized extraction
* [ ] Extraction metadata
* [ ] Missing fields
* [ ] Extraction loading state
* [ ] Generated output
* [ ] Generation metadata
* [ ] Generation loading state

These values should be read from the lower-level hooks.

---

## 4. Composition Hook Action Checklist

Confirm that the composition hook exposes:

* [ ] `setFlowName`
* [ ] `addRule`
* [ ] `updateRule`
* [ ] `removeRule`
* [ ] `handleRequirementFileChange`
* [ ] `extractRequirementsFromFile`
* [ ] `generateFlow`
* [ ] `downloadFlow`
* [ ] `toggleNormalizedJson`

Confirm that each action has an explicit type and clear responsibility.

---

## 5. Default Flow Name Checklist

Confirm the default flow name is:

```text
XCDTS005 Generated Flow
```

Verify:

* [ ] It is visible on initial load.
* [ ] It is defined in one reusable constant.
* [ ] The user can edit it.
* [ ] A valid extracted flow name replaces it.
* [ ] An empty extracted flow name is ignored.
* [ ] Removing the requirement file restores it.
* [ ] Generation uses the current edited value.

---

## 6. Initial Workflow State Checklist

Open the application before selecting a file.

Confirm:

* [ ] `flowName` contains the default value.
* [ ] `rules` is empty.
* [ ] `includeSoapSubflow` is `false`.
* [ ] `requirementFile` is `null`.
* [ ] `showNormalizedJson` is `false`.
* [ ] `rawExtracted` is `null`.
* [ ] `normalizedExtraction` is `null`.
* [ ] `extractionMetadata` is `null`.
* [ ] `missingFields` is empty.
* [ ] `extractStatus` is empty.
* [ ] `isExtracting` is `false`.
* [ ] `generated` is empty.
* [ ] `generationMetadata` is `null`.
* [ ] `isGenerating` is `false`.

---

## 7. Extraction Hook Composition Checklist

Confirm that the composition hook passes the correct values into `useExtractionWorkflow`.

Verify:

* [ ] Current rules are supplied as `fallbackRules`.
* [ ] Extracted flow-name callback is supplied.
* [ ] Extracted rules callback is supplied.
* [ ] Extraction-start callback is supplied.
* [ ] The callback names match the active extraction-hook contract.
* [ ] No duplicate extraction state is created.
* [ ] No direct extraction service import exists in the composition hook.

---

## 8. Generation Hook Composition Checklist

Confirm that the composition hook uses `useFlowGeneration`.

Verify:

* [ ] Generation state is read from the generation hook.
* [ ] `generateFlow` is delegated to the generation hook.
* [ ] `downloadFlow` is delegated to the generation hook.
* [ ] `resetGeneration` is used for coordinated resets.
* [ ] No duplicate generation state is created.
* [ ] No direct generation-service import exists in the composition hook.

---

## 9. Hook Initialization Order Checklist

Review hook composition order.

* [ ] `useFlowGeneration` is initialized before callbacks that use `resetGeneration`.
* [ ] `useExtractionWorkflow` is initialized after stable extraction callbacks are created.
* [ ] Hook calls remain unconditional.
* [ ] Hook order remains stable between renders.
* [ ] No hook is called inside a callback or condition.
* [ ] React hook rules pass.

---

## 10. Extracted Flow Name Callback Checklist

Review the flow-name callback.

Confirm:

* [ ] It uses `useCallback`.
* [ ] The input is typed as `string`.
* [ ] Leading and trailing whitespace is removed.
* [ ] Empty values are ignored.
* [ ] Valid values update `flowName`.
* [ ] It does not reset rules.
* [ ] It does not reset extraction state.
* [ ] It does not reset generation state directly.

---

## 11. Extracted Rule Callback Checklist

Review the extracted-rules callback.

Confirm:

* [ ] It uses `useCallback`.
* [ ] The input is typed as `BusinessRule[]`.
* [ ] Extracted rules replace the current rule list.
* [ ] Rule order is preserved.
* [ ] SOAP state is recalculated from the new list.
* [ ] `SOAP-SUBFLOW` detection uses the correct `brId`.
* [ ] No duplicate rule list is retained elsewhere.
* [ ] The rules remain editable afterward.

---

## 12. Extraction Start Callback Checklist

Review the extraction-start callback.

Confirm:

* [ ] It uses `useCallback`.
* [ ] Normalized JSON is hidden.
* [ ] `resetGeneration` is called.
* [ ] Previous generated output is cleared.
* [ ] Previous generation metadata is cleared.
* [ ] Generation loading becomes inactive.
* [ ] Extraction state is not reset incorrectly.
* [ ] Flow name and rules follow existing behavior.

---

## 13. Requirement File Selection Checklist

Select a supported requirement file.

Confirm:

* [ ] The selected file becomes `requirementFile`.
* [ ] Extraction state resets.
* [ ] Generation state resets.
* [ ] Normalized JSON is hidden.
* [ ] Extraction does not start automatically.
* [ ] Existing editable state follows current behavior.
* [ ] The page remains responsive.
* [ ] No console error appears.

---

## 14. Requirement File Change Checklist

Select a second file after extraction and generation results exist.

Confirm:

* [ ] The second file replaces the first.
* [ ] Raw extraction is cleared.
* [ ] Normalized extraction is cleared.
* [ ] Extraction metadata is cleared.
* [ ] Missing fields are cleared.
* [ ] Extraction status is cleared.
* [ ] Generated output is cleared.
* [ ] Generation metadata is cleared.
* [ ] Loading states are inactive.
* [ ] Normalized JSON is hidden.
* [ ] Extraction does not run automatically.

---

## 15. Requirement File Removal Checklist

Remove the active file.

Confirm:

* [ ] `requirementFile` becomes `null`.
* [ ] Extraction state resets.
* [ ] Generation state resets.
* [ ] Normalized JSON is hidden.
* [ ] Default flow name is restored.
* [ ] Rules are cleared.
* [ ] SOAP subflow is disabled.
* [ ] Generated output is empty.
* [ ] Metadata is cleared.
* [ ] No stale output remains.

---

## 16. Missing File Extraction Checklist

Trigger extraction with no selected file.

Confirm:

* [ ] No extraction request starts.
* [ ] No file read occurs.
* [ ] No extraction service call occurs.
* [ ] Extraction loading remains inactive.
* [ ] Generation state remains unchanged.
* [ ] The page remains usable.

---

## 17. Duplicate Extraction Protection Checklist

Start extraction and trigger the action again immediately.

Confirm:

* [ ] A second extraction request is not started.
* [ ] `isExtracting` is checked.
* [ ] One action produces one extraction request.
* [ ] Extraction status remains consistent.
* [ ] Loading state returns to inactive after completion.
* [ ] No duplicate result update occurs.

---

## 18. Successful Extraction Checklist

Run extraction with a valid file.

Confirm:

* [ ] The selected file is passed to `extractFromFile`.
* [ ] File reading remains inside the extraction hook.
* [ ] `useLLM: false` remains unchanged.
* [ ] Raw extraction is populated.
* [ ] Normalized extraction is populated.
* [ ] Metadata is populated when available.
* [ ] Missing fields are populated.
* [ ] Completion status appears.
* [ ] Extracted flow name updates the editable input.
* [ ] Extracted rules replace the editable list.
* [ ] SOAP state is recalculated.
* [ ] Extraction loading returns to inactive.
* [ ] Generation remains reset.

---

## 19. Extraction Failure Checklist

Trigger an extraction failure.

Confirm:

* [ ] The lower-level extraction hook catches the error.
* [ ] Existing error formatting or status behavior is preserved.
* [ ] Extraction loading returns to inactive.
* [ ] Failed extraction state is handled consistently.
* [ ] Generation remains reset.
* [ ] Flow name and rules remain usable according to current behavior.
* [ ] A later valid extraction can succeed.

---

## 20. Recovery After Extraction Failure Checklist

After failure, run a valid extraction.

Confirm:

* [ ] Previous error status is replaced.
* [ ] File reading succeeds.
* [ ] The extraction service runs.
* [ ] Raw extraction is populated.
* [ ] Normalized extraction is populated.
* [ ] Metadata is populated when available.
* [ ] Missing fields are updated.
* [ ] Flow name is refreshed.
* [ ] Rules are refreshed.
* [ ] SOAP state is recalculated.
* [ ] Loading returns to inactive.

---

## 21. Add Rule Checklist

Add a rule manually.

Confirm:

* [ ] A new rule is appended.
* [ ] The new rule uses the requested `brId`.
* [ ] Existing rules remain unchanged.
* [ ] Rule order is preserved.
* [ ] Adding `SOAP-SUBFLOW` enables SOAP state.
* [ ] Adding a non-SOAP rule does not enable SOAP state incorrectly.
* [ ] Extraction state remains unchanged.

---

## 22. Update Rule Checklist

Update a rule field.

Confirm:

* [ ] Only the selected rule is updated.
* [ ] Other rules remain unchanged.
* [ ] Rule order remains unchanged.
* [ ] The updated field and value are preserved.
* [ ] The latest value is used in generation.
* [ ] No duplicate rule state is updated in the page.
* [ ] Extraction state remains unchanged.

---

## 23. Remove Rule Checklist

Remove a rule.

Confirm:

* [ ] Only the selected rule is removed.
* [ ] Remaining rule order is preserved.
* [ ] SOAP state is recalculated from remaining rules.
* [ ] Removing the last SOAP rule disables SOAP state.
* [ ] Removing a non-SOAP rule does not disable SOAP state incorrectly.
* [ ] The latest rule list is used in generation.

---

## 24. SOAP Subflow Synchronization Checklist

Test all SOAP-state paths.

* [ ] Initial value is `false`.
* [ ] Adding `SOAP-SUBFLOW` sets it to `true`.
* [ ] Extracted rules containing `SOAP-SUBFLOW` set it to `true`.
* [ ] Extracted rules without `SOAP-SUBFLOW` set it to `false`.
* [ ] Removing the last SOAP rule sets it to `false`.
* [ ] Removing another rule leaves it unchanged when SOAP remains.
* [ ] File removal sets it to `false`.
* [ ] Generation reset does not alter it.
* [ ] Generation receives the current value.

---

## 25. Flow Name Editing Checklist

Edit the flow-name input.

Confirm:

* [ ] `setFlowName` updates the composition-hook state.
* [ ] The input remains controlled.
* [ ] The edited value is visible.
* [ ] Extraction can replace it with a valid extracted value.
* [ ] Empty extracted values are ignored.
* [ ] Generation uses the latest visible value.
* [ ] Generation failure does not reset it.
* [ ] File removal restores the default.

---

## 26. Normalized JSON Toggle Checklist

Toggle the normalized JSON view.

Confirm:

* [ ] `toggleNormalizedJson` changes the visibility state.
* [ ] The user can show the normalized JSON.
* [ ] The user can hide it.
* [ ] Starting extraction hides it.
* [ ] Changing files hides it.
* [ ] Removing the file hides it.
* [ ] Lower-level hooks do not manage this presentation state.

---

## 27. Successful Generation Checklist

Run flow generation after extraction.

Confirm:

* [ ] Current flow name is used.
* [ ] Current rule list is used.
* [ ] Current SOAP state is used.
* [ ] Current normalized extraction is used.
* [ ] The composition hook delegates to `useFlowGeneration`.
* [ ] Payload construction remains in the generation hook.
* [ ] The service is called once.
* [ ] Generated output appears.
* [ ] Generation metadata appears when available.
* [ ] Loading returns to inactive.
* [ ] Extraction state remains unchanged.

---

## 28. Duplicate Generation Protection Checklist

Start generation and trigger it again immediately.

Confirm:

* [ ] A second request is not started.
* [ ] `isGenerating` is checked.
* [ ] The lower-level ref guard remains effective.
* [ ] One action produces one service call.
* [ ] Loading state remains consistent.
* [ ] The intended response determines displayed output.

---

## 29. Current Input Generation Checklist

Edit workflow values and generate again.

Confirm:

* [ ] Latest flow name is used.
* [ ] Added rules are included.
* [ ] Updated rules are included.
* [ ] Removed rules are excluded.
* [ ] Latest SOAP state is used.
* [ ] Latest normalized extraction is used.
* [ ] No stale callback value is used.
* [ ] No stale rule array is used.

---

## 30. Generation Failure Checklist

Trigger a generation failure.

Confirm:

* [ ] The generation hook catches the error.
* [ ] Generation metadata is cleared.
* [ ] Existing error formatting is used.
* [ ] Formatted error output appears.
* [ ] Loading returns to inactive.
* [ ] Extraction state remains unchanged.
* [ ] Flow name remains editable.
* [ ] Rules remain editable.
* [ ] SOAP state remains unchanged.
* [ ] A later valid generation can succeed.

---

## 31. Recovery After Generation Failure Checklist

After failure, run generation again successfully.

Confirm:

* [ ] Previous error output is replaced.
* [ ] Current inputs are used.
* [ ] Metadata is refreshed.
* [ ] Successful output appears.
* [ ] Loading returns to inactive.
* [ ] Extraction state remains unchanged.
* [ ] No failed state remains visible.

---

## 32. Re-Extraction Checklist

Run extraction more than once.

Confirm:

* [ ] Previous extraction output is cleared.
* [ ] Previous generation output is cleared.
* [ ] Previous generation metadata is cleared.
* [ ] New extraction replaces the old result.
* [ ] New rules replace old extracted rules.
* [ ] Flow name updates when available.
* [ ] SOAP state is recalculated.
* [ ] Normalized JSON is hidden.
* [ ] Results do not accumulate.

---

## 33. Repeated Generation Checklist

Run generation multiple times.

Confirm:

* [ ] The same inputs can be generated again.
* [ ] Updated inputs are used.
* [ ] Latest output replaces previous output.
* [ ] Latest metadata replaces previous metadata.
* [ ] Results do not accumulate.
* [ ] Download uses the latest output.
* [ ] No duplicate request occurs from one action.

---

## 34. Download Without Output Checklist

Trigger download before generation.

Confirm:

* [ ] No download occurs.
* [ ] No empty file is created.
* [ ] No error is thrown.
* [ ] Workflow state remains unchanged.
* [ ] The page remains responsive.

---

## 35. Successful Download Checklist

Download a generated flow.

Confirm:

* [ ] The generation hook handles the action.
* [ ] The latest generated string is used.
* [ ] No new generation occurs.
* [ ] Output remains visible.
* [ ] Metadata remains unchanged.
* [ ] Extraction state remains unchanged.
* [ ] Downloaded content matches displayed content.

---

## 36. Cross-Workflow Reset Checklist

Generate a flow and then start a new extraction.

Confirm:

* [ ] `resetGeneration` is called.
* [ ] Generated output is cleared.
* [ ] Generation metadata is cleared.
* [ ] Generation loading is inactive.
* [ ] Extraction begins normally.
* [ ] Flow name and rules follow current behavior.
* [ ] No stale generated output remains visible.

---

## 37. Page Import Cleanup Checklist

Review:

```text
src/app/page.tsx
```

Confirm that the page imports only:

* [ ] Existing UI components
* [ ] `useApiFactoryWorkflow`

Confirm that the page no longer imports:

* [ ] `useState`
* [ ] `useCallback`
* [ ] `useExtractionWorkflow`
* [ ] `useFlowGeneration`
* [ ] `BusinessRule`
* [ ] Extraction services
* [ ] Generation services
* [ ] State helpers
* [ ] Rule mappers
* [ ] Output formatters
* [ ] Download helpers

---

## 38. Page Logic Removal Checklist

Confirm that `page.tsx` no longer contains:

* [ ] State declarations
* [ ] Rule add logic
* [ ] Rule update logic
* [ ] Rule removal logic
* [ ] File-change logic
* [ ] Extraction-trigger logic
* [ ] Generation-trigger logic
* [ ] Cross-workflow reset logic
* [ ] Service calls
* [ ] Payload construction
* [ ] Output formatting
* [ ] Download logic

---

## 39. Page Rendering Checklist

Confirm that `page.tsx` still renders:

* [ ] Existing header
* [ ] Existing descriptive text
* [ ] `RequirementExtractionPanel`
* [ ] Flow-name input
* [ ] `RuleBuilderPanel`
* [ ] `GenerationOutputPanel`

Confirm:

* [ ] Existing labels remain unchanged.
* [ ] Existing classes remain unchanged.
* [ ] Existing layout remains unchanged.
* [ ] Existing component order remains unchanged.
* [ ] Existing accessibility attributes remain unchanged.

---

## 40. Requirement Panel Prop Compatibility Checklist

Confirm that the page passes:

* [ ] `extractStatus`
* [ ] `missingFields`
* [ ] `normalizedExtraction`
* [ ] `rawExtracted`
* [ ] `extractionMetadata`
* [ ] `showNormalizedJson`
* [ ] `handleRequirementFileChange`
* [ ] `extractRequirementsFromFile`
* [ ] `toggleNormalizedJson`

Confirm all prop types remain compatible.

---

## 41. Rule Builder Prop Compatibility Checklist

Confirm that the page passes:

* [ ] `rules`
* [ ] `missingFields`
* [ ] `addRule`
* [ ] `updateRule`
* [ ] `removeRule`

Confirm:

* [ ] Callback signatures match.
* [ ] No adapter logic is required in the page.
* [ ] Rule editing remains functional.

---

## 42. Generation Panel Prop Compatibility Checklist

Confirm that the page passes:

* [ ] `generated`
* [ ] `generationMetadata`
* [ ] `generateFlow`
* [ ] `downloadFlow`

If supported:

* [ ] `isGenerating`

Confirm:

* [ ] Callback signatures match.
* [ ] Existing labels remain unchanged.
* [ ] Existing output behavior remains unchanged.

---

## 43. State Ownership Checklist

Confirm final ownership.

### `useExtractionWorkflow`

* [ ] Raw extraction
* [ ] Normalized extraction
* [ ] Extraction metadata
* [ ] Missing fields
* [ ] Extraction status
* [ ] Extraction loading
* [ ] Extraction service coordination

### `useFlowGeneration`

* [ ] Generated output
* [ ] Generation metadata
* [ ] Generation loading
* [ ] Generation service coordination
* [ ] Success and error formatting
* [ ] Download behavior

### `useApiFactoryWorkflow`

* [ ] Flow name
* [ ] Editable rules
* [ ] SOAP state
* [ ] Requirement file
* [ ] Normalized JSON visibility
* [ ] Rule actions
* [ ] File actions
* [ ] Extraction trigger
* [ ] Generation trigger
* [ ] Cross-workflow reset coordination

### `page.tsx`

* [ ] JSX
* [ ] Layout
* [ ] Styling
* [ ] Prop wiring

Confirm:

* [ ] No unnecessary duplicate state ownership exists.
* [ ] No circular synchronization exists.

---

## 44. Day 39 Type File Checklist

Review:

```text
src/types/day39.ts
```

Confirm:

* [ ] Active workflow contracts are defined.
* [ ] Existing domain types are reused.
* [ ] Existing lower-level hook types are reused where practical.
* [ ] `BusinessRule` is not duplicated.
* [ ] Extraction types are not duplicated.
* [ ] Generation types are not duplicated.
* [ ] Nullable values are explicit.
* [ ] Action signatures match component props.
* [ ] No invalid import exists.
* [ ] No unused export causes lint failure.

---

## 45. Callback Dependency Checklist

Review callbacks inside `useApiFactoryWorkflow`.

Confirm:

* [ ] Extracted flow-name callback dependencies are correct.
* [ ] Extracted-rules callback dependencies are correct.
* [ ] Extraction-start callback includes `resetGeneration`.
* [ ] File-change callback includes both reset actions.
* [ ] Extraction trigger includes current file and loading state.
* [ ] Generation trigger includes current generation inputs.
* [ ] Toggle callback is stable.
* [ ] No unnecessary dependency is added.
* [ ] No stale closure remains.
* [ ] ESLint hook rules pass.

---

## 46. Type Safety Checklist

Confirm that the implementation uses:

* [ ] `BusinessRule`
* [ ] `NormalizedExtraction`
* [ ] `ExtractionMetadata`
* [ ] `GenerationMetadata`
* [ ] `UseApiFactoryWorkflowResult`

Confirm:

* [ ] No `any` is used.
* [ ] Callback parameters are typed.
* [ ] Hook actions have explicit return types.
* [ ] Nullable values use `null`.
* [ ] Component props remain compatible.
* [ ] Lower-level hook results are forwarded safely.
* [ ] Dynamic rule field updates remain type-compatible.

---

## 47. Lint Validation Checklist

Run:

```powershell
npm run lint
```

Confirm:

* [ ] No unused import remains.
* [ ] No unused variable remains.
* [ ] No React hook dependency warning appears.
* [ ] No React hook rule violation appears.
* [ ] No explicit `any` warning appears.
* [ ] No Day 39 lint error appears.
* [ ] Existing unrelated warnings are documented separately.

Lint result:

```text
[ ] PASS
[ ] PASS WITH NON-BLOCKING WARNINGS
[ ] FAIL
```

Notes:

```text
Record the actual lint output here.
```

---

## 48. TypeScript Build Checklist

Run:

```powershell
npm run build
```

Confirm:

* [ ] TypeScript compilation succeeds.
* [ ] `useApiFactoryWorkflow` resolves.
* [ ] Day 39 types resolve.
* [ ] Lower-level hooks resolve.
* [ ] Component props remain compatible.
* [ ] No circular import appears.
* [ ] No nullable assignment error appears.
* [ ] No callback signature error appears.
* [ ] Production compilation succeeds.
* [ ] Page-data collection succeeds.
* [ ] Static-page generation succeeds.
* [ ] Route generation succeeds.

Build result:

```text
[ ] PASS
[ ] PASS WITH NON-BLOCKING WARNINGS
[ ] FAIL
```

Notes:

```text
Record the actual build output here.
```

---

## 49. Browser Validation Checklist

Run:

```powershell
npm run dev
```

Confirm:

* [ ] Page loads normally.
* [ ] No blank screen appears.
* [ ] No hydration error appears.
* [ ] No runtime import error appears.
* [ ] File selection works.
* [ ] File removal works.
* [ ] Extraction works.
* [ ] Extraction failure appears correctly.
* [ ] Retry after extraction failure works.
* [ ] Flow name updates.
* [ ] Flow name remains editable.
* [ ] Rules populate.
* [ ] Rules can be added.
* [ ] Rules can be updated.
* [ ] Rules can be removed.
* [ ] SOAP state remains synchronized.
* [ ] Normalized JSON toggle works.
* [ ] Generation works.
* [ ] Generation metadata appears.
* [ ] Generation errors appear correctly.
* [ ] Retry after generation failure works.
* [ ] Download works.
* [ ] New extraction clears generated output.
* [ ] File change clears both workflows.
* [ ] No unexpected console error appears.

Browser result:

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

## 50. Regression Checklist

Confirm that unrelated behavior remains unchanged.

* [ ] Page layout is unchanged.
* [ ] Styling is unchanged.
* [ ] Existing component interfaces are unchanged.
* [ ] Extraction service behavior is unchanged.
* [ ] `useLLM: false` is unchanged.
* [ ] Rule mapping is unchanged.
* [ ] Generation payload is unchanged.
* [ ] Generation service behavior is unchanged.
* [ ] Success formatting is unchanged.
* [ ] Error formatting is unchanged.
* [ ] Downloaded content is unchanged.
* [ ] Default flow name is unchanged.
* [ ] SOAP behavior is unchanged.
* [ ] File reset behavior is unchanged.
* [ ] No unrelated file was modified unnecessarily.

---

## 51. Code Quality Checklist

* [ ] `page.tsx` is substantially smaller.
* [ ] Page workflow logic is centralized.
* [ ] Lower-level hooks remain focused.
* [ ] Editable state has one clear owner.
* [ ] Cross-workflow resets use one coordinated path.
* [ ] Rule actions are centralized.
* [ ] SOAP synchronization is centralized.
* [ ] Existing helpers are reused.
* [ ] No dead code remains.
* [ ] No commented-out implementation remains.
* [ ] No temporary debug logging remains.
* [ ] No unnecessary abstraction was introduced.
* [ ] The composition hook is reusable.
* [ ] The page is easier to read and maintain.

---

## 52. Documentation Checklist

* [ ] The Day 39 plan matches the final implementation.
* [ ] The integration guide matches `page.tsx`.
* [ ] Test notes describe actual behavior.
* [ ] This checklist matches the active hook API.
* [ ] Known warnings are documented.
* [ ] Unverified tests are not marked as passed.
* [ ] Failed checks are recorded honestly.
* [ ] The completion report will reflect actual results.

---

## 53. Known Warning Review

If the production build displays the existing workspace-root warning:

* [ ] Confirm the warning is unrelated to Day 39.
* [ ] Confirm the build otherwise completes.
* [ ] Record the warning as non-blocking.
* [ ] Do not remove lockfiles without understanding their purpose.
* [ ] Do not classify the warning as a functional regression.

Example:

```text
Next.js inferred your workspace root, but it may not be correct.
Multiple lockfiles were detected.
```

---

## 54. Final Acceptance Checklist

Day 39 can be marked complete only when:

* [ ] `src/types/day39.ts` exists.
* [ ] `src/hooks/useApiFactoryWorkflow.ts` exists.
* [ ] The composition hook uses `useExtractionWorkflow`.
* [ ] The composition hook uses `useFlowGeneration`.
* [ ] Flow-name state moved out of the page.
* [ ] Rule state moved out of the page.
* [ ] SOAP state moved out of the page.
* [ ] Requirement-file state moved out of the page.
* [ ] JSON visibility moved out of the page.
* [ ] Rule actions moved out of the page.
* [ ] File actions moved out of the page.
* [ ] Extraction trigger moved out of the page.
* [ ] Generation trigger moved out of the page.
* [ ] Cross-workflow resets moved out of the page.
* [ ] The page is primarily presentational.
* [ ] Extraction behavior remains unchanged.
* [ ] Generation behavior remains unchanged.
* [ ] Flow-name editing works.
* [ ] Rule editing works.
* [ ] SOAP synchronization works.
* [ ] File-change resets work.
* [ ] File-removal resets work.
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
DAY 39 TESTING STATUS

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
Record the final Day 39 validation summary here.
```
