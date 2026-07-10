# Day 37 — Extraction Workflow Hook Testing Checklist

## Purpose

This checklist verifies that the Day 37 extraction workflow hook refactor is complete, type-safe, behavior-preserving, and ready for final acceptance.

Day 37 moved requirement-extraction workflow logic out of:

```text
src/app/page.tsx
```

and into:

```text
src/hooks/useExtractionWorkflow.ts
```

The refactor must preserve existing extraction, rule mapping, generation, and error-handling behavior.

---

## 1. Required File Checklist

Confirm that the required Day 37 files exist.

* [ ] `docs/day37/extraction_workflow_hook_plan.md`
* [ ] `src/types/day37.ts`
* [ ] `src/hooks/useExtractionWorkflow.ts`
* [ ] `docs/day37/extraction_workflow_hook_test_notes.md`
* [ ] `docs/day37/testing_checklist.md`
* [ ] `docs/day37/day37_completion_report.md`

Confirm that the page was updated.

* [ ] `src/app/page.tsx`

---

## 2. Hook File Validation

Review:

```text
src/hooks/useExtractionWorkflow.ts
```

Confirm:

* [ ] The file begins with `"use client"`.
* [ ] React hooks are imported correctly.
* [ ] The hook name is `useExtractionWorkflow`.
* [ ] The hook returns extraction-specific state and actions.
* [ ] No JSX exists in the hook.
* [ ] No UI component is imported into the hook.
* [ ] No generation-output component logic exists in the hook.
* [ ] No download logic exists in the hook.
* [ ] No unrelated page state is managed by the hook.
* [ ] No `any` type was introduced.
* [ ] All hook callbacks have explicit return types where needed.
* [ ] Hook dependencies are complete.
* [ ] No infinite render loop is introduced.

---

## 3. Hook State Checklist

Confirm that the hook manages:

* [ ] `rawExtracted`
* [ ] `normalizedExtraction`
* [ ] `extractionMetadata`
* [ ] `missingFields`
* [ ] `extractStatus`
* [ ] `isExtracting`

Confirm that the hook does not manage:

* [ ] `flowName` as permanent editable page state
* [ ] `rules` as permanent editable page state
* [ ] `generated`
* [ ] `generationMetadata`
* [ ] `includeSoapSubflow`
* [ ] `requirementFile`
* [ ] `showNormalizedJson`

---

## 4. Initial State Checklist

Open the application before selecting a requirement file.

* [ ] `rawExtracted` is `null`.
* [ ] `normalizedExtraction` is `null`.
* [ ] `extractionMetadata` is `null`.
* [ ] `missingFields` is empty.
* [ ] `extractStatus` is empty.
* [ ] `isExtracting` is `false`.
* [ ] No stale extraction output is visible.
* [ ] The default flow name is visible.
* [ ] The rules list is empty or uses the expected initial state.
* [ ] Generated output is empty.
* [ ] Generation metadata is `null`.

---

## 5. Page Import Checklist

Review:

```text
src/app/page.tsx
```

Confirm that the page imports:

* [ ] `useExtractionWorkflow`
* [ ] `RequirementExtractionPanel`
* [ ] `RuleBuilderPanel`
* [ ] `GenerationOutputPanel`
* [ ] Existing flow-generation services
* [ ] Existing download helper
* [ ] Existing generated-output formatters
* [ ] Existing `BusinessRule` type
* [ ] Existing `GenerationMetadata` type

Confirm that the page no longer imports:

* [ ] `extractRequirements`
* [ ] `buildRulesFromExtraction`
* [ ] `getInitialExtractionWorkflowState`
* [ ] `mapExtractionResponseToState`
* [ ] Extraction-specific types no longer needed by the page

---

## 6. Removed Page State Checklist

Confirm that these extraction-specific state declarations were removed from `page.tsx`:

* [ ] `rawExtracted`
* [ ] `normalizedExtraction`
* [ ] `extractionMetadata`
* [ ] `missingFields`
* [ ] `extractStatus`
* [ ] `isExtracting`

Confirm that these page-level states remain:

* [ ] `flowName`
* [ ] `rules`
* [ ] `generated`
* [ ] `includeSoapSubflow`
* [ ] `generationMetadata`
* [ ] `requirementFile`
* [ ] `showNormalizedJson`

---

## 7. Day 36 Helper Reuse Checklist

Confirm that the hook uses:

```text
src/lib/day36/extractionStateHelpers.ts
```

Verify:

* [ ] `getInitialExtractionWorkflowState` is reused.
* [ ] `mapExtractionResponseToState` is reused.
* [ ] Reset defaults are not duplicated unnecessarily.
* [ ] The hook does not redefine Day 36 reset behavior incorrectly.
* [ ] The normalized extraction is mapped through the existing helper.
* [ ] Existing Day 36 behavior remains unchanged.

---

## 8. Extraction Service Checklist

Confirm that the hook uses:

```text
src/services/day33/requirementExtractionService.ts
```

Verify:

* [ ] `extractRequirements` is called inside the hook.
* [ ] File text is passed correctly.
* [ ] `useLLM: false` remains unchanged.
* [ ] The extraction service itself was not modified unnecessarily.
* [ ] No duplicate service call is introduced.
* [ ] The extraction request runs only after a file is available.
* [ ] Empty files do not call the service.

---

## 9. File Reading Checklist

Confirm that the hook reads the file using:

```ts
await file.text();
```

Verify:

* [ ] The selected file is used.
* [ ] File reading is asynchronous.
* [ ] Whitespace-only files are detected.
* [ ] File-read errors are caught.
* [ ] File reading does not occur when no file is selected.
* [ ] File content is not modified before extraction except for validation.

---

## 10. File Selection Checklist

Select a supported requirement file.

* [ ] The page stores the selected file.
* [ ] Existing extraction state is reset.
* [ ] Normalized JSON visibility is disabled.
* [ ] Generated output is cleared.
* [ ] Generation metadata is cleared.
* [ ] Extraction does not start automatically.
* [ ] The page remains responsive.
* [ ] No console error appears.

---

## 11. Missing File Checklist

Attempt to run extraction without a selected file.

* [ ] Extraction does not run.
* [ ] No file-read call occurs.
* [ ] No extraction-service call occurs.
* [ ] No runtime error occurs.
* [ ] The page remains usable.

---

## 12. Extraction Start Checklist

Start extraction with a valid file.

* [ ] `isExtracting` becomes `true`.
* [ ] `extractStatus` becomes `Extracting requirements...`.
* [ ] Previous raw extraction is cleared.
* [ ] Previous normalized extraction is cleared.
* [ ] Previous metadata is cleared.
* [ ] Previous missing fields are cleared.
* [ ] Normalized JSON visibility is disabled.
* [ ] Generated output is cleared.
* [ ] Generation metadata is cleared.
* [ ] The extraction-start callback runs once.

---

## 13. Empty File Checklist

Use an empty or whitespace-only file.

* [ ] The file is detected as empty.
* [ ] The extraction service is not called.
* [ ] A clear error message is shown.
* [ ] Raw extraction remains `null`.
* [ ] Normalized extraction remains `null`.
* [ ] Metadata remains `null`.
* [ ] Missing fields remain empty.
* [ ] `isExtracting` returns to `false`.
* [ ] A new file can be selected afterward.

Expected message format:

```text
The selected file "<filename>" is empty.
```

---

## 14. Successful Extraction Checklist

Run extraction using a valid requirement file.

* [ ] File text is read successfully.
* [ ] The extraction service returns successfully.
* [ ] Raw extraction is stored.
* [ ] Normalized extraction is stored.
* [ ] Extraction metadata is stored.
* [ ] Missing fields are stored.
* [ ] Extracted flow name is processed.
* [ ] Suggested nodes are processed.
* [ ] Business rules are populated.
* [ ] Completion status is displayed.
* [ ] `isExtracting` returns to `false`.
* [ ] No stale error status remains.

Expected completion message:

```text
Requirement extraction completed. Review normalized extraction, gaps, and highlighted missing fields.
```

---

## 15. Raw Extraction Checklist

Inspect the raw extraction output.

* [ ] Object-shaped raw extraction is preserved.
* [ ] Invalid raw extraction values are handled safely.
* [ ] Missing raw extraction becomes `null`.
* [ ] A new extraction replaces the previous result.
* [ ] Raw extraction remains compatible with `RequirementExtractionPanel`.
* [ ] No unsafe cast hides incompatible data.

---

## 16. Normalized Extraction Checklist

Inspect the normalized extraction output.

* [ ] The Day 36 mapping helper is used.
* [ ] The result matches `NormalizedExtraction`.
* [ ] Missing normalized data becomes `null`.
* [ ] A new extraction replaces the previous result.
* [ ] No normalization logic is duplicated in the page.
* [ ] No old normalized output remains after failure.

---

## 17. Metadata Checklist

Inspect extraction metadata.

* [ ] Metadata is stored from the service response.
* [ ] Missing metadata becomes `null`.
* [ ] Metadata is cleared before re-extraction.
* [ ] Metadata is cleared after failure.
* [ ] Metadata is passed unchanged to the extraction panel.
* [ ] Previous metadata does not remain visible.

---

## 18. Missing Fields Checklist

Test with complete and incomplete requirement files.

### Complete File

* [ ] `missingFields` is empty.
* [ ] No incorrect warning appears.

### Incomplete File

* [ ] Expected missing fields are returned.
* [ ] Missing fields are displayed.
* [ ] Missing fields are passed to `RuleBuilderPanel`.
* [ ] Previous missing fields are cleared.
* [ ] Missing fields are replaced, not appended.
* [ ] No unrelated fields are reported.

---

## 19. Flow Name Checklist

Use a response containing `extracted.flowName`.

* [ ] The flow name is validated as a string.
* [ ] Leading and trailing spaces are removed.
* [ ] Empty flow names are ignored.
* [ ] Valid flow names update the page.
* [ ] The flow name remains editable by the user.
* [ ] Extraction does not create duplicate flow-name state.
* [ ] A missing flow name leaves the current page value unchanged.

---

## 20. Rule Mapping Checklist

Confirm that the hook uses:

```text
src/mappers/day34/extractedRequirementRuleMapper.ts
```

Verify:

* [ ] `buildRulesFromExtraction` is called.
* [ ] `suggestedNodes` is passed.
* [ ] `fallbackRules` is passed.
* [ ] `extracted` data is passed.
* [ ] Mapped rules are returned through the callback.
* [ ] Page-level rules are updated.
* [ ] Users can edit mapped rules afterward.
* [ ] No duplicate mapping occurs in `page.tsx`.

---

## 21. Fallback Rule Checklist

Confirm that the latest page rules are available to the hook.

* [ ] `useRef` stores the latest fallback rule array.
* [ ] Rule edits are reflected before extraction.
* [ ] `extractFromFile` is not recreated after every rule edit.
* [ ] No stale rule list is used.
* [ ] Rule mapping remains deterministic.
* [ ] The ref is updated on every render safely.

---

## 22. SOAP Subflow Checklist

Test rule lists with and without `SOAP-SUBFLOW`.

* [ ] Extracted SOAP subflow rules enable `includeSoapSubflow`.
* [ ] Non-SOAP rule lists disable it.
* [ ] Manually adding `SOAP-SUBFLOW` enables it.
* [ ] Removing the last SOAP subflow rule disables it.
* [ ] Flow-generation payload receives the correct value.
* [ ] SOAP state remains synchronized with the rule list.

---

## 23. Rule Editing Checklist

After extraction:

* [ ] A rule can be added.
* [ ] A rule can be updated.
* [ ] A rule can be removed.
* [ ] Existing rule values are preserved.
* [ ] Rule order is preserved.
* [ ] No extracted rule becomes read-only.
* [ ] Updating rules does not reset extraction state.
* [ ] Removing rules does not alter normalized extraction.

---

## 24. Extraction Failure Checklist

Trigger a service failure.

* [ ] The error is caught inside the hook.
* [ ] The error is logged.
* [ ] Raw extraction is cleared.
* [ ] Normalized extraction is cleared.
* [ ] Metadata is cleared.
* [ ] Missing fields are cleared.
* [ ] The error message appears through `extractStatus`.
* [ ] Unknown errors use the fallback message.
* [ ] `isExtracting` returns to `false`.
* [ ] The page remains responsive.

Fallback message:

```text
Requirement extraction failed.
```

---

## 25. Recovery After Failure Checklist

After a failed extraction, run a valid extraction.

* [ ] The old error status is replaced.
* [ ] Loading begins normally.
* [ ] The file is read.
* [ ] The service is called.
* [ ] Raw extraction is populated.
* [ ] Normalized extraction is populated.
* [ ] Metadata is populated when available.
* [ ] Missing fields are populated correctly.
* [ ] Flow name updates when available.
* [ ] Rules are populated.
* [ ] `isExtracting` returns to `false`.
* [ ] No failed state remains visible.

---

## 26. Re-Extraction Checklist

Run extraction multiple times.

* [ ] Previous raw extraction is cleared before a new attempt.
* [ ] Previous normalized extraction is cleared.
* [ ] Previous metadata is cleared.
* [ ] Previous missing fields are cleared.
* [ ] Previous generated output is cleared.
* [ ] Previous generation metadata is cleared.
* [ ] Latest extraction results replace earlier results.
* [ ] Results do not accumulate.
* [ ] Rules are recalculated using the latest response.
* [ ] No duplicate status message appears.

---

## 27. Reset Extraction Checklist

Call `resetExtraction`.

* [ ] Raw extraction becomes `null`.
* [ ] Normalized extraction resets.
* [ ] Metadata becomes `null`.
* [ ] Missing fields become empty.
* [ ] Extraction status becomes empty.
* [ ] Loading becomes `false`.
* [ ] Generated output remains page-owned.
* [ ] Flow name remains page-owned.
* [ ] Editable rules remain page-owned unless file removal clears them.
* [ ] The hook remains usable afterward.

---

## 28. File Removal Checklist

Remove the selected file.

* [ ] `requirementFile` becomes `null`.
* [ ] Hook extraction state is reset.
* [ ] Normalized JSON visibility is disabled.
* [ ] Generated output is cleared.
* [ ] Generation metadata is cleared.
* [ ] Rules are cleared.
* [ ] SOAP subflow state becomes `false`.
* [ ] Default flow name is restored.
* [ ] No stale extraction output remains.
* [ ] A new file can be selected afterward.

---

## 29. Normalized JSON Checklist

Test the normalized JSON toggle.

* [ ] The toggle remains page-owned.
* [ ] The user can show normalized JSON.
* [ ] The user can hide normalized JSON.
* [ ] Starting extraction hides the previous view.
* [ ] Selecting a new file hides the previous view.
* [ ] Removing a file hides the previous view.
* [ ] The hook does not manage presentation-only visibility state.

---

## 30. Flow Generation Regression Checklist

Generate a flow after successful extraction.

* [ ] Current `flowName` is used.
* [ ] Current editable `rules` are used.
* [ ] Current `normalizedExtraction` is used.
* [ ] Current `includeSoapSubflow` is used.
* [ ] Existing generation service is called.
* [ ] Generation metadata is stored.
* [ ] Generated flow output is formatted.
* [ ] Extraction state remains unchanged.
* [ ] No Day 37 regression is observed.

---

## 31. Generation Failure Checklist

Trigger a generation failure.

* [ ] Generation metadata is cleared.
* [ ] The existing error formatter is used.
* [ ] Generated output displays the error.
* [ ] Extraction state is not reset.
* [ ] Extraction status is not overwritten.
* [ ] The hook does not handle generation errors.
* [ ] The page remains usable.

---

## 32. Download Checklist

Test flow download.

* [ ] No download occurs when output is empty.
* [ ] Generated output is passed to `downloadJsonText`.
* [ ] Existing download behavior remains unchanged.
* [ ] The extraction hook does not manage downloads.
* [ ] Downloaded JSON remains valid.

---

## 33. Callback Stability Checklist

Review callbacks passed into the hook.

* [ ] Flow-name callback uses `useCallback`.
* [ ] Rules callback uses `useCallback`.
* [ ] Extraction-start callback uses `useCallback`.
* [ ] Callback dependency arrays are correct.
* [ ] The hook does not reinitialize because of unstable callbacks.
* [ ] No repeated extraction side effect occurs.
* [ ] No render loop occurs.

---

## 34. Hook Dependency Checklist

Review `extractFromFile` dependencies.

* [ ] `onExtractionStart` is included.
* [ ] `onFlowNameExtracted` is included.
* [ ] `onRulesExtracted` is included.
* [ ] Stable helper imports do not need dependencies.
* [ ] State setters are not added unnecessarily.
* [ ] `fallbackRules` is accessed through the ref.
* [ ] ESLint hook rules pass.

---

## 35. Type Safety Checklist

Confirm that the implementation uses:

* [ ] `BusinessRule`
* [ ] `NormalizedExtraction`
* [ ] `ExtractionMetadata`
* [ ] `GenerationMetadata`

Confirm:

* [ ] No `any` is used.
* [ ] `unknown` is narrowed safely.
* [ ] `rawExtracted` is validated before casting if needed.
* [ ] Nullable values use explicit `null`.
* [ ] Callback arguments are typed.
* [ ] Hook return values are typed.
* [ ] Page props remain compatible.
* [ ] No duplicate domain type was created unnecessarily.

---

## 36. Day 37 Type File Checklist

Review:

```text
src/types/day37.ts
```

Confirm one of the following:

* [ ] It is aligned with the final project-specific hook contract.

or

* [ ] It is retained only for documented future use and does not conflict with the current implementation.

Confirm:

* [ ] No invalid imports exist.
* [ ] No unused exported types cause lint failures.
* [ ] No generic type conflicts with `day32` types.
* [ ] The final codebase has one clear active hook contract.

---

## 37. Lint Validation Checklist

Run:

```powershell
npm run lint
```

Confirm:

* [ ] No unused import remains.
* [ ] No unused variable remains.
* [ ] No hook dependency warning appears.
* [ ] No React hook rule violation appears.
* [ ] No explicit `any` warning appears.
* [ ] No Day 37 lint error appears.
* [ ] Existing unrelated warnings are documented separately.

Lint result:

```text
[ ] PASS
[ ] FAIL
```

Notes:

```text
Add lint notes here.
```

---

## 38. TypeScript Build Checklist

Run:

```powershell
npm run build
```

Confirm:

* [ ] TypeScript compilation succeeds.
* [ ] `page.tsx` imports resolve.
* [ ] Hook imports resolve.
* [ ] Day 36 helper imports resolve.
* [ ] Extraction service import resolves.
* [ ] Rule mapper import resolves.
* [ ] Component props remain compatible.
* [ ] No invalid callback type appears.
* [ ] No raw extraction type error appears.
* [ ] No nullable assignment error appears.

Build result:

```text
[ ] PASS
[ ] FAIL
```

Notes:

```text
Add build notes here.
```

---

## 39. Browser Validation Checklist

Run:

```powershell
npm run dev
```

Confirm:

* [ ] The page loads.
* [ ] No blank screen appears.
* [ ] No hydration error appears.
* [ ] No runtime import error appears.
* [ ] File selection works.
* [ ] Extraction works.
* [ ] Loading status appears.
* [ ] Completion status appears.
* [ ] Raw extraction appears.
* [ ] Normalized extraction appears.
* [ ] Metadata appears.
* [ ] Missing fields appear.
* [ ] Flow name updates.
* [ ] Rules populate.
* [ ] Rules remain editable.
* [ ] Flow generation works.
* [ ] Download works.
* [ ] Failure recovery works.
* [ ] No browser-console error appears.

---

## 40. Regression Checklist

Confirm that unrelated behavior remains unchanged.

* [ ] Existing page layout is unchanged.
* [ ] Existing styling is unchanged.
* [ ] Existing extraction panel behavior is unchanged.
* [ ] Existing rule-builder behavior is unchanged.
* [ ] Existing generation output behavior is unchanged.
* [ ] Existing flow-generation payload is unchanged.
* [ ] Existing download behavior is unchanged.
* [ ] Existing error messages are unchanged.
* [ ] Existing default flow name is unchanged.
* [ ] Existing SOAP subflow behavior is unchanged.
* [ ] No unrelated file was modified unnecessarily.

---

## 41. Code Quality Checklist

* [ ] The page is smaller than before.
* [ ] Extraction logic is centralized.
* [ ] State ownership is clear.
* [ ] The hook has one clear responsibility.
* [ ] Page-level editable state remains in the page.
* [ ] Repeated reset code is reduced.
* [ ] Day 36 helpers are reused.
* [ ] No dead code remains.
* [ ] No commented-out implementation remains.
* [ ] No temporary debug code remains.
* [ ] No unnecessary abstraction was introduced.
* [ ] The implementation is easier to maintain.

---

## 42. Documentation Checklist

* [ ] The Day 37 plan matches the final implementation.
* [ ] Test notes describe actual behavior.
* [ ] This checklist matches the final hook API.
* [ ] Known warnings are documented.
* [ ] Unverified tests are not marked as passed.
* [ ] Failed checks are recorded honestly.
* [ ] The completion report will reflect final results.

---

## 43. Known Warning Review

If the build displays the existing multiple-lockfile warning:

* [ ] Confirm the warning is unrelated to Day 37.
* [ ] Confirm the build still completes.
* [ ] Record the warning in test notes.
* [ ] Do not remove lockfiles without understanding their purpose.
* [ ] Do not classify the warning as a Day 37 regression.

Example:

```text
Next.js inferred your workspace root, but it may not be correct.
Multiple lockfiles were detected.
```

---

## 44. Final Acceptance Checklist

Day 37 can be marked complete only when:

* [x] The hook file exists.
* [x] The page uses the hook.
* [x] Extraction state was removed from the page.
* [x] Extraction service calls moved into the hook.
* [x] Rule mapping moved into the hook.
* [x] Day 36 state helpers are reused.
* [x] Flow-name editing still works.
* [x] Rule editing still works.
* [x] SOAP subflow behavior still works.
* [x] Flow generation still works.
* [x] Download still works.
* [x] Error recovery works.
* [x] Re-extraction works.
* [x] Lint passes.
* [x] Build passes.
* [x] Browser validation passes.
* [x] No functional regression is identified.
* [x] Test notes are complete.
* [x] Completion report is ready.

---

## Final Result

```text
DAY 37 TESTING STATUS

[ ] PASS
[ ] PASS WITH NON-BLOCKING WARNINGS
[ ] FAIL
```

## Final Notes

```text
Record the final Day 37 validation summary here.
```
