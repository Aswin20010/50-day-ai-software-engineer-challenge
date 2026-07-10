# Day 36 — Extraction State Refactor Testing Checklist

## Purpose

This checklist verifies that the Day 36 extraction-state refactor is complete, type-safe, behavior-preserving, and ready to be accepted.

The refactor moves reusable extraction-state defaults and reset values out of `src/app/page.tsx` into shared types and helper functions.

The checklist must be completed before Day 36 is marked finished.

---

## 1. File Creation Checklist

Confirm that the required Day 36 files exist.

* [ ] `docs/day36/extraction_state_refactor_plan.md`
* [ ] `src/types/day36.ts`
* [ ] `src/lib/day36/extractionStateHelpers.ts`
* [ ] `docs/day36/extraction_state_test_notes.md`
* [ ] `docs/day36/testing_checklist.md`
* [ ] `docs/day36/day36_completion_report.md`

Confirm that the existing page file was updated.

* [ ] `src/app/page.tsx`

---

## 2. Shared Type Validation

Review `src/types/day36.ts`.

* [ ] Extraction-state types are clearly named.
* [ ] Types represent all state fields handled by the helper.
* [ ] Existing application types are reused where possible.
* [ ] No unnecessary duplicate domain types were created.
* [ ] No `any` types were introduced.
* [ ] Nullable values are represented explicitly.
* [ ] Optional values are represented correctly.
* [ ] Array fields use the correct item types.
* [ ] Object fields use the correct existing models.
* [ ] The type definitions match the React state setter types in `page.tsx`.
* [ ] Type exports are used consistently.
* [ ] No React-specific logic exists in the shared types file.

---

## 3. Helper Module Validation

Review:

`src/lib/day36/extractionStateHelpers.ts`

* [ ] The helper module imports the correct Day 36 types.
* [ ] Helper functions have explicit parameter types.
* [ ] Helper functions have explicit return types.
* [ ] Helper functions are pure.
* [ ] Helper functions do not update React state directly.
* [ ] Helper functions do not import React hooks.
* [ ] Helper functions do not read files.
* [ ] Helper functions do not call APIs.
* [ ] Helper functions do not call `extractRequirements`.
* [ ] Helper functions do not generate business rules.
* [ ] Helper functions do not contain UI logic.
* [ ] Helper functions do not access browser APIs.
* [ ] Helper functions do not mutate their inputs.
* [ ] Helper functions return new objects.
* [ ] Helper functions return new arrays.
* [ ] No shared mutable default object is reused.
* [ ] Default values match the original values from `page.tsx`.
* [ ] Function names clearly describe their purpose.
* [ ] The helper code is small and readable.
* [ ] No unrelated functionality was added.

---

## 4. Initial State Checklist

Open the application before selecting a requirement file.

* [ ] Raw extraction output is empty.
* [ ] Normalized extraction output is empty.
* [ ] Extraction metadata is empty.
* [ ] Missing fields are empty.
* [ ] Flow name is empty.
* [ ] Generated rules are empty.
* [ ] No stale extraction result is visible.
* [ ] No extraction error is visible.
* [ ] Loading state is inactive.
* [ ] The initial UI matches the behavior before the refactor.

---

## 5. Valid File Selection Checklist

Select a valid supported requirement file.

* [ ] The file is accepted.
* [ ] The selected filename is displayed correctly.
* [ ] Previous extraction output is cleared.
* [ ] Previous metadata is cleared.
* [ ] Previous missing fields are cleared.
* [ ] Previous flow name is cleared.
* [ ] Previous rules are cleared.
* [ ] Previous error state is cleared.
* [ ] The application remains responsive.
* [ ] No console error appears.

---

## 6. Successful Extraction Checklist

Run extraction using a valid requirement file.

* [ ] The file content is read successfully.
* [ ] `extractRequirements` is called.
* [ ] The original input text is passed correctly.
* [ ] `useLLM: false` remains unchanged.
* [ ] Raw extraction is populated.
* [ ] Normalized extraction is populated.
* [ ] Extraction metadata is populated.
* [ ] Missing fields are calculated.
* [ ] Flow name is populated when available.
* [ ] Business rules are generated.
* [ ] The extraction result appears in the UI.
* [ ] Loading state returns to inactive.
* [ ] No unexpected error is displayed.
* [ ] The result matches the pre-refactor behavior.

---

## 7. Raw Extraction Checklist

Inspect the raw extraction result.

* [ ] The raw result matches the extraction function output.
* [ ] The helper does not alter the raw structure.
* [ ] No raw fields are removed.
* [ ] No unexpected raw fields are added.
* [ ] Null values remain unchanged.
* [ ] Optional values remain unchanged.
* [ ] A new extraction replaces the previous raw result.
* [ ] Raw results are not accidentally merged.

---

## 8. Normalized Extraction Checklist

Inspect the normalized extraction result.

* [ ] The normalized structure is unchanged.
* [ ] Existing normalization logic still runs in the same location.
* [ ] The helper does not perform normalization.
* [ ] The helper only stores or resets normalized output.
* [ ] Missing optional values are handled safely.
* [ ] A new extraction replaces the previous normalized result.
* [ ] Normalized results are not accidentally merged.
* [ ] No type assertion hides an incompatible value.

---

## 9. Metadata Checklist

Inspect extraction metadata.

* [ ] Metadata is stored after a successful extraction.
* [ ] Metadata values match the extraction response.
* [ ] The helper does not alter metadata.
* [ ] Metadata is cleared when a new file is selected.
* [ ] Metadata is cleared when the selected file is removed.
* [ ] Metadata is replaced during re-extraction.
* [ ] Metadata from an earlier extraction is not retained.

---

## 10. Missing Fields Checklist

Test with both complete and incomplete requirement files.

### Complete File

* [ ] Missing-fields collection is empty.
* [ ] No missing-field warning is displayed.

### Incomplete File

* [ ] Missing required fields are identified.
* [ ] Expected field names are included.
* [ ] No unrelated fields are reported.
* [ ] The missing-fields UI is displayed correctly.
* [ ] Previous missing fields are cleared before a new extraction.
* [ ] Missing fields are replaced, not appended unintentionally.

---

## 11. Flow Name Checklist

Test flow-name behavior.

* [ ] A valid extracted flow name is displayed.
* [ ] Existing flow-name fallback logic remains unchanged.
* [ ] Empty or missing flow names are handled safely.
* [ ] The flow name is cleared when the file changes.
* [ ] The flow name is cleared when the file is removed.
* [ ] Re-extraction replaces the previous flow name.
* [ ] No previous flow name remains visible.

---

## 12. Business Rule Checklist

Inspect generated business rules.

* [ ] Existing rule-generation logic still runs.
* [ ] Rule generation remains outside the state helper.
* [ ] Generated rules use the expected structure.
* [ ] Rules match the extracted requirement.
* [ ] Rules are cleared when the file changes.
* [ ] Rules are cleared when the file is removed.
* [ ] Rules are replaced during re-extraction.
* [ ] Rules are not duplicated.
* [ ] Rules from different files are not combined.
* [ ] Empty rule results are handled safely.

---

## 13. File Change Reset Checklist

Extract one requirement file and then select a different file.

* [ ] Raw extraction from the first file is cleared.
* [ ] Normalized extraction from the first file is cleared.
* [ ] Metadata from the first file is cleared.
* [ ] Missing fields from the first file are cleared.
* [ ] Flow name from the first file is cleared.
* [ ] Rules from the first file are cleared.
* [ ] Error state from the first file is cleared.
* [ ] The second file can be extracted successfully.
* [ ] No stale data remains in the UI.

---

## 14. File Removal Checklist

Select and extract a file, then remove it.

* [ ] The selected file is removed.
* [ ] Raw extraction is cleared.
* [ ] Normalized extraction is cleared.
* [ ] Metadata is cleared.
* [ ] Missing fields are cleared.
* [ ] Flow name is cleared.
* [ ] Rules are cleared.
* [ ] Error state is cleared where expected.
* [ ] The page returns to its initial extraction state.
* [ ] No stale result remains visible.

---

## 15. Re-Extraction Checklist

Run extraction more than once.

* [ ] The same file can be extracted again.
* [ ] A different file can be extracted after the first one.
* [ ] Previous state does not accumulate.
* [ ] Arrays are replaced instead of appended.
* [ ] Objects are replaced instead of mutated.
* [ ] The latest raw result is displayed.
* [ ] The latest normalized result is displayed.
* [ ] The latest metadata is displayed.
* [ ] The latest missing-fields result is displayed.
* [ ] The latest flow name is displayed.
* [ ] The latest rule set is displayed.
* [ ] No duplicate output appears.

---

## 16. Extraction Failure Checklist

Trigger an extraction failure using invalid, empty, or unsupported input.

* [ ] The existing error handler runs.
* [ ] The existing user-facing error behavior is preserved.
* [ ] Loading state returns to inactive.
* [ ] Partial output is not shown as successful.
* [ ] Stale output is cleared according to existing behavior.
* [ ] No uncaught exception appears in the browser.
* [ ] No unhandled promise rejection appears.
* [ ] The user can retry after the failure.
* [ ] The application remains responsive.

---

## 17. Recovery After Failure Checklist

After a failed extraction, process a valid file.

* [ ] The previous error state is cleared.
* [ ] The valid file is read.
* [ ] Extraction completes.
* [ ] Raw extraction is populated.
* [ ] Normalized extraction is populated.
* [ ] Metadata is populated.
* [ ] Missing fields are populated correctly.
* [ ] Flow name is populated.
* [ ] Rules are generated.
* [ ] No failed-state output remains.
* [ ] The application returns to normal operation.

---

## 18. Shared Reference Safety Checklist

Review state object creation.

* [ ] Each helper call returns a new top-level object.
* [ ] Each empty array is newly created.
* [ ] Each empty object is newly created where needed.
* [ ] One extraction-state instance cannot modify another.
* [ ] No module-level mutable default state is exported.
* [ ] No input array is modified.
* [ ] No input object is modified.
* [ ] Spread operators or equivalent safe copies are used where required.

---

## 19. `page.tsx` Integration Checklist

Review:

`src/app/page.tsx`

* [ ] The new helper functions are imported correctly.
* [ ] The new Day 36 types are imported correctly.
* [ ] Duplicate reset values have been reduced.
* [ ] React state setters remain inside the page component.
* [ ] File-reading logic remains inside the page workflow.
* [ ] `extractRequirements` usage remains unchanged.
* [ ] Rule-generation logic remains unchanged.
* [ ] Existing event handlers still work.
* [ ] Existing UI rendering still works.
* [ ] Existing loading behavior is preserved.
* [ ] Existing error behavior is preserved.
* [ ] No unrelated refactor was introduced.
* [ ] No extraction state field was omitted from reset logic.
* [ ] State updates occur in the correct order.
* [ ] Successful extraction applies the complete new result.
* [ ] Failed extraction does not leave an inconsistent mixed state.

---

## 20. Import and Path Checklist

* [ ] Import paths use the project alias conventions.
* [ ] File and folder name casing is correct.
* [ ] No circular dependency was introduced.
* [ ] No unused import remains.
* [ ] No duplicate import remains.
* [ ] The helper module resolves during the build.
* [ ] The Day 36 type module resolves during the build.
* [ ] Windows and case-sensitive build environments are considered.

---

## 21. TypeScript Validation Checklist

Run:

```powershell
npm run build
```

Confirm:

* [ ] TypeScript compilation succeeds.
* [ ] Helper parameter types are correct.
* [ ] Helper return types are correct.
* [ ] React state setters accept helper values.
* [ ] Nullable values are handled.
* [ ] Optional values are handled.
* [ ] No implicit `any` error appears.
* [ ] No invalid assignment error appears.
* [ ] No missing property error appears.
* [ ] No incorrect import error appears.

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

## 22. Lint Validation Checklist

Run:

```powershell
npm run lint
```

Confirm:

* [ ] No Day 36 lint error appears.
* [ ] No unused variable was introduced.
* [ ] No unused import was introduced.
* [ ] No React hook dependency warning was introduced.
* [ ] No unsafe explicit `any` was introduced.
* [ ] No formatting issue blocks the build.
* [ ] Any pre-existing warning is documented separately.

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

## 23. Production Build Checklist

Run:

```powershell
npm run build
```

Confirm:

* [ ] Next.js compilation succeeds.
* [ ] Application routes compile.
* [ ] Static analysis completes.
* [ ] No helper module error appears.
* [ ] No type module error appears.
* [ ] No client/server boundary issue appears.
* [ ] No unsupported browser API was added to the helper.
* [ ] The production build completes successfully.

Production build result:

```text
[ ] PASS
[ ] FAIL
```

---

## 24. Browser Validation Checklist

Start the application using the project’s development command.

Example:

```powershell
npm run dev
```

Confirm:

* [ ] The page loads.
* [ ] No blank screen appears.
* [ ] No runtime module error appears.
* [ ] No hydration error appears.
* [ ] File selection works.
* [ ] Extraction works.
* [ ] Reset behavior works.
* [ ] Error behavior works.
* [ ] Re-extraction works.
* [ ] No unexpected browser-console error appears.

---

## 25. Regression Checklist

Verify that unrelated functionality remains unchanged.

* [ ] Existing requirement upload UI is unchanged.
* [ ] Existing extraction button behavior is unchanged.
* [ ] Existing result sections are unchanged.
* [ ] Existing rule display is unchanged.
* [ ] Existing generated artifact behavior is unchanged.
* [ ] Existing navigation is unchanged.
* [ ] Existing styling is unchanged.
* [ ] Existing labels and messages are unchanged.
* [ ] No unrelated state was moved.
* [ ] No unrelated component was modified unnecessarily.

---

## 26. Code Quality Checklist

* [ ] The helper names are understandable.
* [ ] The type names are understandable.
* [ ] The helper functions have one clear responsibility.
* [ ] The implementation avoids duplicated default values.
* [ ] The implementation avoids unnecessary abstraction.
* [ ] The code is easier to maintain than before.
* [ ] Future extraction fields can be added safely.
* [ ] Comments explain only non-obvious behavior.
* [ ] No dead code remains.
* [ ] No temporary debug logging remains.
* [ ] No commented-out implementation remains.

---

## 27. Documentation Checklist

* [ ] The refactor plan matches the completed implementation.
* [ ] Test notes describe the actual validation performed.
* [ ] This checklist reflects the final code.
* [ ] Known warnings are documented.
* [ ] No unverified claim is included.
* [ ] Failed tests, if any, are recorded honestly.
* [ ] The completion report will include final validation results.

---

## 28. Known Warning Review

If the build displays a multiple-lockfile or workspace-root warning:

* [ ] Confirm that the warning existed independently of Day 36.
* [ ] Confirm that the build still completes successfully.
* [ ] Record the warning in the test notes.
* [ ] Do not treat the warning as a Day 36 functional regression.
* [ ] Do not remove lockfiles unless their purpose is understood.

Example warning:

```text
Next.js inferred your workspace root, but it may not be correct.
Multiple lockfiles were detected.
```

---

## 29. Final Acceptance Checklist

Day 36 can be marked complete only when:

* [ ] All required files exist.
* [ ] Shared extraction-state types are implemented.
* [ ] Reusable extraction-state helpers are implemented.
* [ ] `page.tsx` uses the helpers.
* [ ] Duplicate reset values are reduced.
* [ ] Initial state works.
* [ ] Successful extraction works.
* [ ] Missing-field behavior works.
* [ ] Flow-name behavior works.
* [ ] Rule generation works.
* [ ] File-change reset works.
* [ ] File-removal reset works.
* [ ] Re-extraction works.
* [ ] Failure recovery works.
* [ ] TypeScript validation passes.
* [ ] Lint validation passes.
* [ ] Production build passes.
* [ ] No functional regression is identified.
* [ ] Test notes are completed.
* [ ] Completion report is ready.

---

## Final Result

```text
DAY 36 TESTING STATUS

[ ] PASS
[ ] PASS WITH NON-BLOCKING WARNINGS
[ ] FAIL
```

## Final Notes

```text
Record the final Day 36 validation summary here.
```
