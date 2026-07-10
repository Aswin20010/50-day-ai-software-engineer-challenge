# Day 36 — Extraction State Test Notes

## Purpose

This document records the validation performed for the Day 36 extraction-state refactor.

The purpose of the testing is to confirm that moving extraction-state defaults and reset values out of `src/app/page.tsx` did not change the existing requirement-extraction behavior.

The refactor must only improve code organization and state consistency.

---

## Files Validated

The following files were reviewed as part of Day 36 testing:

```text
src/types/day36.ts
src/lib/day36/extractionStateHelpers.ts
src/app/page.tsx
```

The following documentation files are also part of the Day 36 implementation:

```text
docs/day36/extraction_state_refactor_plan.md
docs/day36/extraction_state_test_notes.md
docs/day36/testing_checklist.md
docs/day36/day36_completion_report.md
```

---

## Test Objective

The main test objective is to confirm that the extraction workflow behaves exactly as it did before the refactor.

The following behavior must remain unchanged:

* Requirement-file selection
* Requirement-file reading
* Requirement extraction
* Raw extraction storage
* Normalized extraction storage
* Extraction metadata storage
* Missing-field detection
* Flow-name population
* Business-rule generation
* Extraction error handling
* Extraction reset behavior
* Re-extraction behavior

---

## Helper Function Validation

The extraction-state helper functions were reviewed to confirm that they:

* Return predictable default values
* Return new arrays and objects
* Do not mutate input values
* Do not directly update React state
* Do not contain UI logic
* Do not perform file operations
* Do not perform API or network calls
* Use explicit TypeScript types
* Avoid the use of `any`
* Preserve the existing state value shapes

The helper module should remain independent from React so that it can be reused and tested separately.

---

## Initial State Validation

The application was reviewed on initial page load.

Expected behavior:

* No raw extraction result is displayed.
* No normalized extraction result is displayed.
* No extraction metadata is displayed.
* The missing-fields collection is empty.
* The flow name is empty.
* Generated rules are empty.
* No previous extraction error is displayed.
* No stale requirement-file output is visible.

Result:

```text
PASS
```

The extraction state starts with the expected empty values.

---

## Valid Requirement File Test

A valid requirement file was selected and processed.

Expected behavior:

1. The file content is read successfully.
2. `extractRequirements` is called with the expected input.
3. The existing `useLLM: false` configuration is preserved.
4. Raw extraction output is stored.
5. Normalized extraction output is stored.
6. Extraction metadata is stored.
7. Missing required fields are calculated.
8. The flow name is populated when available.
9. Business rules are generated from the extracted requirement.
10. The UI displays the latest extraction result.

Result:

```text
PASS
```

The successful extraction flow continues to populate all expected values.

---

## Raw Extraction Validation

The raw extraction output was reviewed after processing a valid requirement file.

Expected behavior:

* The raw result matches the result returned by the extraction function.
* The refactor does not modify the raw extraction structure.
* No raw extraction fields are removed.
* No additional fields are introduced by the state helper.
* The previous raw extraction result is replaced during re-extraction.

Result:

```text
PASS
```

The raw extraction behavior remains unchanged.

---

## Normalized Extraction Validation

The normalized extraction output was reviewed after a successful extraction.

Expected behavior:

* The normalized result uses the same structure as before the refactor.
* Existing normalization logic remains unchanged.
* The helper only stores or resets the result.
* The helper does not perform requirement normalization.
* The latest normalized result replaces any previous result.

Result:

```text
PASS
```

The normalized extraction behavior remains unchanged.

---

## Extraction Metadata Validation

The extraction metadata was reviewed after processing a requirement file.

Expected behavior:

* Metadata returned by the extraction workflow is stored correctly.
* The metadata is cleared when extraction state is reset.
* Metadata from a previous file does not remain after a new file is selected.
* The helper does not alter metadata values.

Result:

```text
PASS
```

The extraction metadata is handled consistently.

---

## Missing Fields Validation

A requirement file with complete data and a requirement file with incomplete data were reviewed.

Expected behavior for complete data:

* The missing-fields collection is empty.

Expected behavior for incomplete data:

* Missing required fields are identified.
* The missing-fields collection contains the expected field names.
* The UI continues to display the missing-field information.
* Missing fields from an earlier extraction are cleared before a new result is stored.

Result:

```text
PASS
```

Missing-field detection and reset behavior remain unchanged.

---

## Flow Name Validation

The flow-name behavior was reviewed for requirement files with and without a usable flow name.

Expected behavior:

* A valid extracted flow name is displayed.
* The existing flow-name selection or fallback logic is preserved.
* The flow name is cleared when extraction state is reset.
* A flow name from a previous file does not remain visible after selecting another file.

Result:

```text
PASS
```

The flow-name behavior remains consistent.

---

## Business Rule Validation

Generated business rules were reviewed after successful extraction.

Expected behavior:

* Rules are generated using the existing rule-generation function.
* The state helper does not generate business rules.
* Generated rules are stored in the same structure as before.
* Previous rules are cleared before or during a new extraction attempt.
* Rules from a previous requirement file are not retained.
* Re-extraction replaces the complete rule collection.

Result:

```text
PASS
```

Business-rule generation and storage remain unchanged.

---

## Requirement File Change Test

A valid requirement file was processed, and then a different requirement file was selected.

Expected behavior:

* Raw extraction output from the first file is cleared.
* Normalized extraction output from the first file is cleared.
* Metadata from the first file is cleared.
* Missing fields from the first file are cleared.
* The flow name from the first file is cleared.
* Generated rules from the first file are cleared.
* The second file can be extracted normally.
* No stale output from the first file remains visible.

Result:

```text
PASS
```

The new reset helper consistently clears file-dependent extraction state.

---

## Requirement File Removal Test

A requirement file was selected and extracted, and then the selected file was removed.

Expected behavior:

* File-dependent extraction output is cleared.
* Raw extraction data is cleared.
* Normalized extraction data is cleared.
* Metadata is cleared.
* Missing fields are cleared.
* The flow name is cleared.
* Generated rules are cleared.
* The page returns to an empty extraction state.

Result:

```text
PASS
```

Removing the selected file clears the related extraction state.

---

## Re-Extraction Test

The same requirement file was extracted more than once.

Expected behavior:

* The previous extraction result does not accumulate with the new result.
* Arrays are replaced instead of reused or appended unintentionally.
* The latest raw extraction result is displayed.
* The latest normalized extraction result is displayed.
* The latest metadata is displayed.
* The latest missing-fields result is displayed.
* The latest flow name is displayed.
* The latest generated rules are displayed.

Result:

```text
PASS
```

Re-extraction replaces the previous result correctly.

---

## Extraction Failure Test

An invalid or unsupported requirement file was used to trigger extraction failure behavior.

Expected behavior:

* The error is handled by the existing error path.
* The existing error message behavior is preserved.
* Loading or processing state is reset.
* Partial extraction results are not displayed as successful output.
* Stale results from a previous successful extraction are cleared where required by the existing behavior.
* The user can select another file and retry extraction.

Result:

```text
PASS
```

The extraction failure flow remains functional after the refactor.

---

## Recovery After Failure Test

After triggering an extraction failure, a valid requirement file was selected and processed.

Expected behavior:

* The previous error state is cleared.
* The valid file is read successfully.
* Extraction completes successfully.
* Raw extraction output is populated.
* Normalized extraction output is populated.
* Metadata is populated.
* Missing fields are populated correctly.
* The flow name is populated.
* Business rules are generated.
* No failed extraction state remains visible.

Result:

```text
PASS
```

The extraction workflow successfully recovers after an error.

---

## Shared Mutable State Check

The default extraction state was reviewed for shared array or object references.

Expected behavior:

* Each helper call returns a new state object.
* Empty arrays are newly created for each state instance.
* Empty objects are newly created where required.
* Updating one state result does not modify another state result.
* Default values are not stored in a mutable shared singleton.

Result:

```text
PASS
```

The helper returns safe state values without shared mutable defaults.

---

## TypeScript Validation

The project was checked for TypeScript compatibility.

Validation points:

* Day 36 types are imported correctly.
* Helper return types match the React state setter types.
* Existing extraction models remain compatible.
* No unnecessary type assertions were introduced.
* No `any` types were introduced.
* Nullable values are handled explicitly.
* Optional extraction values are handled safely.

Command:

```powershell
npm run build
```

Result:

```text
PASS
```

The project compiles successfully with the Day 36 changes.

---

## Lint Validation

The project was checked using the configured lint command.

Command:

```powershell
npm run lint
```

Result:

```text
PASS
```

No Day 36 lint errors were identified.

If the project reports existing warnings unrelated to Day 36, they should be documented separately and should not be treated as failures caused by this refactor.

---

## Production Build Validation

A Next.js production build was executed.

Command:

```powershell
npm run build
```

Expected behavior:

* TypeScript compilation completes.
* Application routes compile.
* No invalid imports are reported.
* No helper module resolution errors are reported.
* No extraction-state type errors are reported.
* The production build completes successfully.

Result:

```text
PASS
```

The Day 36 refactor is compatible with the production build.

---

## Behavior Comparison

The extraction workflow was compared before and after the refactor.

| Area                    | Before Refactor        | After Refactor            | Status |
| ----------------------- | ---------------------- | ------------------------- | ------ |
| File selection          | Working                | Working                   | PASS   |
| File removal            | Working                | Working                   | PASS   |
| Raw extraction          | Working                | Working                   | PASS   |
| Normalized extraction   | Working                | Working                   | PASS   |
| Metadata storage        | Working                | Working                   | PASS   |
| Missing-field detection | Working                | Working                   | PASS   |
| Flow-name population    | Working                | Working                   | PASS   |
| Rule generation         | Working                | Working                   | PASS   |
| Error handling          | Working                | Working                   | PASS   |
| State reset             | Repeated setter values | Centralized helper values | PASS   |
| Re-extraction           | Working                | Working                   | PASS   |
| Production build        | Working                | Working                   | PASS   |

---

## Refactor Verification

The following implementation boundaries were confirmed:

* React state remains owned by `src/app/page.tsx`.
* The helper module only constructs extraction-state values.
* Requirement extraction remains inside the existing extraction workflow.
* File-reading logic was not moved into the helper.
* Rule-generation logic was not moved into the helper.
* UI rendering logic was not moved into the helper.
* Existing extraction behavior was preserved.
* Repeated reset values were reduced.
* State defaults are now centralized.

---

## Known Non-Blocking Notes

The Next.js build may display an existing workspace-root warning when multiple lockfiles are present.

Example:

```text
Next.js inferred your workspace root, but it may not be correct.
Multiple lockfiles were detected.
```

This warning is not caused by the Day 36 extraction-state refactor.

It can be handled separately by:

* Removing an unnecessary lockfile, or
* Configuring the expected Turbopack root in the Next.js configuration

The warning does not invalidate the Day 36 implementation when the build otherwise completes successfully.

---

## Final Test Result

```text
DAY 36 EXTRACTION STATE REFACTOR: PASS
```

The extraction-state helper refactor successfully centralizes extraction defaults and reset values while preserving the existing requirement-extraction behavior.

No functional regression was identified during validation.
