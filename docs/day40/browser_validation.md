# Day 40 — Browser Validation

## Document Information

| Field           | Value                                                                                                                |
| --------------- | -------------------------------------------------------------------------------------------------------------------- |
| Project         | API Factory Generator                                                                                                |
| Application     | API Factory UI                                                                                                       |
| Roadmap Day     | Day 40                                                                                                               |
| Phase           | Phase 4 — Browser Validation                                                                                         |
| Status          | Ready for execution                                                                                                  |
| Validation Type | End-to-end browser workflow validation                                                                               |
| Primary Goal    | Confirm that the production build behaves correctly for complete, incomplete, invalid, repeated, and reset workflows |

---

## 1. Purpose

The purpose of this phase is to validate the API Factory UI through the browser after the application has passed production validation.

This phase confirms that users can complete the intended workflow reliably from beginning to end.

The browser validation covers:

* Initial page load.
* Requirement-file selection.
* Requirement extraction.
* Raw and normalized result display.
* Extraction metadata.
* Missing-field detection.
* Flow-name generation and editing.
* Business-rule generation.
* API artifact generation.
* Loading-state behavior.
* Error handling.
* Retry behavior.
* Workflow reset.
* Sequential file processing.
* Browser console health.
* Network request behavior.
* Responsive desktop layout.
* Keyboard accessibility.
* Final browser-readiness decision.

This phase must be performed against the production build whenever possible.

---

## 2. Validation Environment

Record the environment used for browser testing.

```text
Operating system:

Browser:

Browser version:

Screen resolution:

Application URL:

Git branch:

Node.js version:

Validation date:

Validated by:
```

Recommended application URL:

```text
http://localhost:3000
```

Start the production application using:

```powershell
npm run build
npm run start
```

When port `3000` is unavailable:

```powershell
npm run start -- -p 3001
```

Use the actual port consistently throughout validation.

---

## 3. Supported Browser Scope

At minimum, complete the full validation using one Chromium-based browser.

Recommended primary browser:

```text
Google Chrome or Microsoft Edge
```

When available, perform a basic smoke test using an additional browser:

```text
Mozilla Firefox
```

The primary objective is functional stability rather than exhaustive cross-browser certification.

Record tested browsers:

| Browser | Version | Full Validation or Smoke Test | Result |
| ------- | ------- | ----------------------------- | ------ |
|         |         |                               |        |

---

## 4. Browser Validation Result Statuses

Use the following statuses:

| Status            | Meaning                                                         |
| ----------------- | --------------------------------------------------------------- |
| PASS              | Behavior matches the expected result                            |
| PASS WITH WARNING | Behavior works, but a minor non-blocking issue exists           |
| FAIL              | Behavior is incorrect or prevents workflow completion           |
| BLOCKED           | Validation cannot be completed because of an external condition |
| NOT APPLICABLE    | Scenario does not apply to the current application              |

A failed critical workflow prevents project freeze.

---

## 5. Validation Test Data

Prepare at least four requirement files.

## 5.1 Test File A — Complete Requirement

This file should contain:

* API name.
* Business purpose.
* Endpoint.
* HTTP method.
* Required headers.
* Request structure.
* Validation rules.
* Business rules.
* Success response.
* Error responses.
* Expected generated output information.

Expected behavior:

* File selection succeeds.
* Extraction succeeds.
* Normalized content is populated.
* Missing fields are empty or minimal.
* Flow name is generated.
* Business rules are generated.
* API artifacts can be generated.
* No error message remains.

---

## 5.2 Test File B — Incomplete Requirement

This file should intentionally omit important information, such as:

* Endpoint.
* HTTP method.
* Required request field.
* Success response.
* Error response.
* Business-rule details.

Expected behavior:

* Extraction completes where possible.
* Missing fields are clearly reported.
* The application remains stable.
* Generation is blocked, guarded, or clearly marked incomplete when required data is absent.
* No fabricated data is silently presented as confirmed input.

---

## 5.3 Test File C — Minimal or Poorly Structured Requirement

This file may contain only a short paragraph with limited structure.

Expected behavior:

* Available information is extracted where possible.
* Unsupported assumptions are avoided.
* Missing fields are reported.
* The user receives actionable feedback.
* The application does not crash.
* Reset remains available.

---

## 5.4 Test File D — Invalid or Unsupported Input

Use one of the following:

* Empty file.
* Unsupported file type.
* Corrupt file.
* File containing irrelevant text.
* File that cannot be parsed.

Expected behavior:

* The application rejects or safely handles the input.
* A clear validation or error message appears.
* Loading state ends.
* Previous successful data is not shown as the result of the failed input.
* Retry or reset remains possible.

---

## 6. Browser Setup

Before starting the tests:

1. Open the browser.
2. Open Developer Tools.
3. Select the **Console** tab.
4. Select the **Network** tab.
5. Enable preservation of logs when useful.
6. Clear the browser console.
7. Clear the network history.
8. Open the application URL.
9. Confirm the production server is running.
10. Do not use browser extensions that alter application behavior when avoidable.

Recommended Developer Tools checks:

```text
Console:
- Errors
- Warnings
- Repeated messages
- React runtime issues

Network:
- Failed requests
- Duplicate requests
- Long-running requests
- Incorrect status codes
- Missing static files
```

---

## 7. Scenario 1 — Initial Application Load

### Steps

1. Open the production application URL.
2. Wait for the page to finish loading.
3. Observe the visible sections.
4. Review the browser console.
5. Review the network panel.
6. Refresh the page once.
7. Repeat the review.

### Expected Result

* The application loads successfully.
* The page title is correct.
* No blank screen appears.
* No uncaught exception appears.
* No hydration error appears.
* No required asset returns `404`.
* No repeated request loop occurs.
* Initial controls are visible.
* Extraction and generation output areas are empty or in their intended initial state.
* No previous workflow data appears.
* Loading indicators are inactive.
* Error messages are absent.

### Evidence

```text
Scenario 1 result:

Initial page status:

Console errors:

Console warnings:

Failed network requests:

Notes:
- 
```

---

## 8. Scenario 2 — Initial Control-State Validation

### Steps

1. Load the application in a fresh browser session.
2. Review the file-selection control.
3. Review the extraction button.
4. Review the generation button.
5. Review reset controls.
6. Review any editable flow-name field.
7. Attempt to trigger actions before selecting a file.

### Expected Result

* File selection is available.
* Extraction is disabled or guarded before a file is selected.
* API generation is disabled or guarded before extraction.
* Flow-name editing is unavailable or empty before extraction, where appropriate.
* Reset does not create an error when used in the initial state.
* Invalid actions display a clear message or remain disabled.
* No operation begins with missing prerequisites.

### Evidence

```text
Scenario 2 result:

Extraction control state:

Generation control state:

Reset behavior:

Unexpected behavior:
- 
```

---

## 9. Scenario 3 — Select a Valid Requirement File

Use Test File A.

### Steps

1. Select the file-input control.
2. Choose Test File A.
3. Confirm the selected file name is displayed.
4. Confirm the extraction action becomes available.
5. Do not start extraction yet.
6. Replace Test File A with Test File B.
7. Confirm the displayed file name updates.
8. Re-select Test File A for the next scenario.

### Expected Result

* The selected file is accepted.
* The correct file name is displayed.
* Replacing the selected file updates the UI.
* No extraction starts automatically unless designed to do so.
* Previous extracted output remains empty before extraction.
* No error is shown for a supported file.
* The extraction control reflects the valid selection.

### Evidence

```text
Scenario 3 result:

Selected file displayed correctly:

Replacement file displayed correctly:

Unexpected extraction:

Console or network issues:
- 
```

---

## 10. Scenario 4 — Complete Requirement Extraction

Use Test File A.

### Steps

1. Clear the browser console and network history.
2. Select Test File A.
3. Start extraction.
4. Observe the loading indicator.
5. Attempt to click extraction again while processing.
6. Wait for extraction to complete.
7. Review all extraction sections.
8. Review browser console and network activity.

### Expected Result

* Extraction begins once.
* A clear loading state appears.
* Duplicate extraction is prevented or ignored safely.
* The loading state ends after completion.
* Raw extraction content is populated.
* Normalized extraction content is populated.
* Extraction metadata is populated.
* Missing-field results are empty or appropriate.
* Flow name is generated.
* Business rules are generated.
* Error state is empty.
* No duplicate result section appears.
* No console error appears.
* No unexpected duplicate network request appears.

### Evidence

```text
Scenario 4 result:

Extraction completed:

Duplicate action prevented:

Raw extraction displayed:

Normalized extraction displayed:

Metadata displayed:

Missing fields:

Flow name:

Business rules displayed:

Loading state cleared:

Console errors:

Network issues:

Notes:
- 
```

---

## 11. Scenario 5 — Validate Raw Extraction Output

### Steps

1. Review the raw extraction area after Test File A completes.
2. Compare the output with the original requirement file.
3. Scroll through the entire content.
4. Review long lines and formatting.
5. Resize the browser window.

### Expected Result

* The raw content corresponds to the selected file.
* Content from another file is not present.
* Long content is readable.
* Text wraps or scrolls intentionally.
* The output does not overflow the page unexpectedly.
* No `undefined`, `[object Object]`, or accidental `null` text appears.
* Special characters render correctly.
* The section remains usable at supported desktop widths.

### Evidence

```text
Scenario 5 result:

Raw output accurate:

Formatting acceptable:

Overflow behavior:

Unexpected values:
- 
```

---

## 12. Scenario 6 — Validate Normalized Extraction Output

### Steps

1. Review every normalized field.
2. Compare each value against Test File A.
3. Identify values that were transformed.
4. Review absent optional fields.
5. Confirm field labels are understandable.

### Expected Result

* Normalized values match the requirement.
* Required fields are represented.
* Optional missing values are handled clearly.
* No value from a previous test appears.
* Transformation does not change business meaning.
* Arrays and nested structures render correctly.
* Empty values are not misleadingly presented as confirmed data.

### Validation Table

| Field            | Expected Value | Actual Value | Result |
| ---------------- | -------------- | ------------ | ------ |
| API name         |                |              |        |
| Endpoint         |                |              |        |
| HTTP method      |                |              |        |
| Headers          |                |              |        |
| Request fields   |                |              |        |
| Business rules   |                |              |        |
| Success response |                |              |        |
| Error responses  |                |              |        |

### Evidence

```text
Scenario 6 result:

Incorrect values:
- 

Unexpected missing values:
- 

Unexpected generated assumptions:
- 
```

---

## 13. Scenario 7 — Validate Extraction Metadata

### Steps

1. Locate the extraction metadata section.
2. Review available metadata fields.
3. Confirm the values apply to Test File A.
4. Repeat extraction once using Test File B.
5. Confirm metadata updates.

### Expected Result

* Metadata belongs to the active extraction.
* Metadata updates after a new extraction.
* Previous metadata does not remain.
* Metadata formatting is readable.
* Missing metadata does not crash the UI.
* Internal-only information is not exposed unnecessarily.

Possible metadata may include:

* Extraction mode.
* Source type.
* Processing timestamp.
* Confidence indicator.
* Parser or provider.
* Field count.
* Warning information.

### Evidence

```text
Scenario 7 result:

Metadata accurate:

Metadata updated after second extraction:

Stale metadata found:

Notes:
- 
```

---

## 14. Scenario 8 — Validate Missing-Field Detection

Use Test File B.

### Steps

1. Reset the workflow.
2. Select Test File B.
3. Run extraction.
4. Review the missing-field section.
5. Compare reported fields with the intentionally omitted values.
6. Check generation controls.

### Expected Result

* Extraction completes without crashing.
* Known omitted fields are reported.
* Fields that are present are not incorrectly reported missing.
* Missing-field labels are understandable.
* Duplicate missing-field entries do not appear.
* Old missing fields from Test File A are absent.
* Critical missing data prevents or safely guards generation.
* The user understands what must be supplied.

### Validation Table

| Intentionally Missing Field | Reported? | Correct Label? | Result |
| --------------------------- | --------- | -------------- | ------ |
|                             |           |                |        |
|                             |           |                |        |
|                             |           |                |        |

### Evidence

```text
Scenario 8 result:

Expected missing fields:

Actual missing fields:

False positives:

False negatives:

Generation behavior:
- 
```

---

## 15. Scenario 9 — Minimal Requirement Handling

Use Test File C.

### Steps

1. Reset the workflow.
2. Select Test File C.
3. Run extraction.
4. Observe loading and result behavior.
5. Review normalized output.
6. Review missing fields.
7. Attempt generation only when the UI allows it.

### Expected Result

* The application remains responsive.
* Extraction returns only available information.
* Missing fields are clearly identified.
* Unsupported values are not fabricated.
* Generation is blocked or produces an explicitly incomplete result according to application design.
* The user can reset and continue.
* No uncaught exception appears.
* Loading state clears.

### Evidence

```text
Scenario 9 result:

Extraction outcome:

Missing fields outcome:

Generation outcome:

Recovery available:

Console errors:
- 
```

---

## 16. Scenario 10 — Invalid or Unsupported Input

Use Test File D.

### Steps

1. Reset the workflow.
2. Select Test File D.
3. Start extraction.
4. Observe loading behavior.
5. Review the error message.
6. Review previous output areas.
7. Attempt retry.
8. Use reset.

### Expected Result

* Invalid input is rejected or handled safely.
* The error message identifies the operation that failed.
* The loading state ends.
* No success message appears.
* Previous successful extraction is not displayed as current output.
* Generated artifacts are not shown as new results.
* Retry is available where appropriate.
* Reset returns the application to its initial state.
* No browser crash occurs.

### Evidence

```text
Scenario 10 result:

Validation or error message:

Loading state cleared:

Stale data visible:

Retry result:

Reset result:

Console errors:
- 
```

---

## 17. Scenario 11 — Flow-Name Generation

Use Test File A.

### Steps

1. Reset the workflow.
2. Extract Test File A.
3. Record the generated flow name.
4. Confirm it reflects the extracted requirement.
5. Review capitalization and separators.
6. Review for invalid characters.
7. Refresh the page and repeat the extraction.

### Expected Result

* A non-empty flow name is generated.
* The flow name corresponds to the current requirement.
* Leading and trailing whitespace are absent.
* Unexpected line breaks are absent.
* Unsafe path characters are absent.
* Extremely long uncontrolled values are avoided.
* Repeating the same extraction produces a consistent or acceptably equivalent name.
* A previous flow name is not reused for a different requirement.

### Unsafe Characters to Review

```text
/
\
..
:
*
?
"
<
>
|
```

### Evidence

```text
Scenario 11 result:

Generated flow name:

Consistency result:

Invalid characters:

Stale flow name:
- 
```

---

## 18. Scenario 12 — Flow-Name Editing

Perform this scenario only when manual editing is supported.

### Steps

1. Extract Test File A.
2. Edit the generated flow name.
3. Enter a valid custom name.
4. Generate the API artifacts.
5. Confirm generated output uses the edited name.
6. Enter only whitespace.
7. Enter a value with invalid characters.
8. Observe validation behavior.

### Expected Result

* Valid manual edits are retained.
* Generated artifacts use the final accepted flow name.
* Empty or whitespace-only names are rejected.
* Invalid characters are rejected, removed, or clearly warned about.
* Editing does not reset unrelated extraction data.
* Validation messages are understandable.
* The original name is not unexpectedly restored during generation.

### Evidence

```text
Scenario 12 result:

Valid edit retained:

Generated output uses edit:

Whitespace handling:

Invalid-character handling:

Unexpected reset:
- 
```

---

## 19. Scenario 13 — Business-Rule Generation

Use Test File A.

### Steps

1. Extract Test File A.
2. Review the generated business rules.
3. Compare them against the source requirement.
4. Check rule order.
5. Check for duplicates.
6. Repeat using Test File B.
7. Confirm rules update.

### Expected Result

* Rules represent the current requirement.
* Validations, transformations, and response behavior are understandable.
* Rules do not duplicate unnecessarily.
* Rules from Test File A disappear when Test File B is extracted.
* No stale entity names remain.
* Missing source information is not presented as a confirmed rule.
* Formatting remains readable for long rule lists.

### Validation Table

| Source Rule | Generated Rule Present? | Accurate? | Result |
| ----------- | ----------------------- | --------- | ------ |
|             |                         |           |        |
|             |                         |           |        |
|             |                         |           |        |

### Evidence

```text
Scenario 13 result:

Missing rules:

Incorrect rules:

Duplicate rules:

Stale rules:

Formatting notes:
- 
```

---

## 20. Scenario 14 — API Artifact Generation

Use Test File A.

### Steps

1. Reset the workflow.
2. Extract Test File A.
3. Confirm required extraction data is available.
4. Start API generation.
5. Observe the loading indicator.
6. Attempt to trigger generation again while processing.
7. Wait for completion.
8. Review all generated sections.
9. Review console and network activity.

### Expected Result

* Generation begins only after valid extraction.
* The loading state is visible.
* Duplicate generation is prevented or handled safely.
* Loading ends after completion.
* Generated artifacts appear once.
* Generated output corresponds to Test File A.
* The current flow name is used.
* Required artifact sections are present.
* No stale values appear.
* No `undefined`, unintended `null`, or placeholder value appears.
* Error state is empty after success.
* No unexpected browser error occurs.

### Evidence

```text
Scenario 14 result:

Generation completed:

Duplicate action prevented:

Generated sections:

Flow name used correctly:

Placeholder values found:

Stale values found:

Loading state cleared:

Console errors:

Network issues:
- 
```

---

## 21. Scenario 15 — Generated Artifact Content Review

Review every generated artifact.

Potential artifacts may include:

* API specification.
* Route definition.
* Request model.
* Response model.
* Validation rules.
* Business-rule section.
* Service scaffold.
* Handler scaffold.
* Generated documentation.
* Configuration output.

### Steps

1. Review artifact names.
2. Review file names.
3. Review endpoint and HTTP method.
4. Review request fields.
5. Review response fields.
6. Review validations.
7. Review business rules.
8. Search generated output for placeholders.
9. Search for values belonging to previous tests.

### Search Terms

```text
undefined
null
TODO
FIXME
placeholder
example-value
Test File B entity name
Previous flow name
```

### Expected Result

* Artifact naming is consistent.
* Endpoint and method are accurate.
* Request and response structures match the requirement.
* Business rules are included.
* Required validations are included.
* No previous workflow value appears.
* No duplicate route appears.
* No malformed formatting appears.
* Output is readable and copyable.
* Generated file names are safe.

### Evidence

| Artifact | Accurate? | Complete? | Stale Data? | Result |
| -------- | --------- | --------- | ----------- | ------ |
|          |           |           |             |        |
|          |           |           |             |        |
|          |           |           |             |        |

---

## 22. Scenario 16 — Generation Before Extraction

### Steps

1. Reset the application.
2. Attempt to trigger API generation immediately.
3. Observe control state and messages.
4. Select a file but do not extract it.
5. Attempt generation again.

### Expected Result

* Generation does not start.
* The generation control remains disabled or validation prevents the action.
* A clear message explains the missing prerequisite when needed.
* No empty artifact is generated.
* No runtime error appears.
* No network generation request occurs.

### Evidence

```text
Scenario 16 result:

Generation blocked before file:

Generation blocked before extraction:

Validation message:

Unexpected request:
- 
```

---

## 23. Scenario 17 — Repeated Extraction Without Reset

### Steps

1. Extract Test File A.
2. Without using reset, select Test File B.
3. Extract Test File B.
4. Compare every output section.
5. Review metadata, missing fields, flow name, and rules.

### Expected Result

* Test File B becomes the active source.
* Raw extraction updates.
* Normalized extraction updates.
* Metadata updates.
* Missing fields update.
* Flow name updates.
* Business rules update.
* Generated output from Test File A is cleared or clearly marked as belonging to the previous run according to application design.
* No mixed data appears.
* No duplicate output appears.

### Evidence

```text
Scenario 17 result:

Raw output updated:

Normalized output updated:

Metadata updated:

Missing fields updated:

Flow name updated:

Rules updated:

Old generated artifacts cleared:

Mixed data found:
- 
```

---

## 24. Scenario 18 — Complete Workflow Reset

### Steps

1. Complete extraction and generation using Test File A.
2. Record all populated sections.
3. Trigger reset.
4. Review every workflow state.
5. Review controls.
6. Review browser console.

### Expected Result

Reset clears:

* Selected requirement file.
* Raw extraction.
* Normalized extraction.
* Extraction metadata.
* Missing fields.
* Flow name.
* Business rules.
* Generated artifacts.
* Validation errors.
* Extraction errors.
* Generation errors.
* Success messages.
* Loading indicators.

After reset:

* The application matches the initial state.
* File selection remains available.
* Extraction is guarded.
* Generation is guarded.
* No console error appears.
* A browser refresh is not required.

### Reset Checklist

* [ ] Selected file cleared.
* [ ] Raw extraction cleared.
* [ ] Normalized extraction cleared.
* [ ] Metadata cleared.
* [ ] Missing fields cleared.
* [ ] Flow name cleared.
* [ ] Business rules cleared.
* [ ] Generated output cleared.
* [ ] Errors cleared.
* [ ] Success messages cleared.
* [ ] Loading state cleared.
* [ ] Initial controls restored.

### Evidence

```text
Scenario 18 result:

Reset completed:

State values not cleared:
- 

Console errors:

Manual refresh required:
- 
```

---

## 25. Scenario 19 — New Workflow After Reset

### Steps

1. Complete Test File A extraction and generation.
2. Reset the workflow.
3. Select Test File B.
4. Extract Test File B.
5. Review all outputs.
6. Attempt generation according to missing-field behavior.

### Expected Result

* Test File B starts from a clean state.
* No Test File A value appears.
* Missing fields reflect Test File B only.
* Flow name reflects Test File B.
* Business rules reflect Test File B.
* Generated artifacts, when allowed, reflect Test File B.
* No manual browser refresh is required.
* No duplicate result sections appear.

### Evidence

```text
Scenario 19 result:

Clean second workflow:

Stale values found:

Mixed values found:

Generation outcome:
- 
```

---

## 26. Scenario 20 — Extraction Failure Followed by Successful Retry

This scenario requires an input or condition that causes extraction to fail safely.

### Steps

1. Reset the application.
2. Trigger a controlled extraction failure.
3. Confirm the error state.
4. Select Test File A.
5. Retry extraction.
6. Review the successful output.

### Expected Result

* The failure is shown clearly.
* Loading stops after failure.
* A second extraction can begin.
* The previous error clears after successful retry.
* Test File A output appears correctly.
* No failed-run partial data remains.
* No browser refresh is required.

### Evidence

```text
Scenario 20 result:

Failure displayed:

Loading stopped:

Retry completed:

Previous error cleared:

Partial stale data found:
- 
```

---

## 27. Scenario 21 — Generation Failure Followed by Successful Retry

This scenario applies when a safe generation-failure condition can be reproduced.

### Steps

1. Produce a valid extraction.
2. Trigger a controlled generation failure.
3. Review the error state.
4. Correct the condition.
5. Retry generation.
6. Review generated artifacts.

### Expected Result

* The generation failure is visible.
* Loading stops after failure.
* Failed output is not shown as successful.
* Retry is possible.
* Successful retry clears the previous error.
* Artifacts appear once.
* No duplicated partial output remains.

### Evidence

```text
Scenario 21 result:

Failure displayed:

Loading stopped:

Retry completed:

Error cleared:

Duplicate or partial output:
- 
```

---

## 28. Scenario 22 — Rapid User Interaction

### Steps

1. Select Test File A.
2. Click extraction rapidly several times.
3. During extraction, attempt reset.
4. During generation, attempt another generation action.
5. Attempt file replacement during active processing when permitted.

### Expected Result

* Duplicate operations are prevented or safely serialized.
* The UI does not crash.
* State does not become mixed.
* Reset behavior during processing is predictable.
* Loading indicators remain accurate.
* No repeated uncontrolled request loop occurs.
* Generated output appears at most once per successful operation.
* The final state is internally consistent.

### Evidence

```text
Scenario 22 result:

Duplicate extraction requests:

Duplicate generation requests:

Reset during processing:

State consistency:

Console errors:
- 
```

---

## 29. Scenario 23 — Page Refresh During the Workflow

### Steps

1. Extract Test File A.
2. Refresh the page before generation.
3. Observe the state.
4. Repeat and refresh during a loading operation when safe.
5. Reopen the application.

### Expected Result

The application should behave according to its documented persistence design.

When state persistence is not implemented:

* Refresh returns the UI to its initial state.
* No corrupted partial state appears.
* No runtime error occurs.

When state persistence is implemented:

* Restored state is complete and internally consistent.
* Sensitive file data is not retained unexpectedly.
* Loading state is not restored indefinitely.

### Evidence

```text
Scenario 23 result:

State after refresh:

Behavior matches design:

Corrupted state found:

Console errors:
- 
```

---

## 30. Scenario 24 — Browser Back and Forward Navigation

### Steps

1. Open the application.
2. Navigate to another page or tab where applicable.
3. Use browser Back.
4. Use browser Forward.
5. Observe application state.

### Expected Result

* Navigation does not produce a blank page.
* No route error appears.
* Application state follows the intended design.
* Controls remain usable.
* No uncaught exception appears.

When the application contains only one route, this scenario may be marked `NOT APPLICABLE` after confirming refresh behavior.

### Evidence

```text
Scenario 24 result:

Status:

Notes:
- 
```

---

## 31. Scenario 25 — Desktop Responsive Validation

Test these viewport sizes:

```text
1920 × 1080
1440 × 900
1366 × 768
1280 × 720
```

### Steps

For each viewport:

1. Load the initial page.
2. Select a file.
3. Complete extraction.
4. Complete generation where possible.
5. Scroll through all sections.
6. Open long output areas.
7. Review buttons and labels.

### Expected Result

* Primary controls remain visible.
* Text does not overlap.
* Cards do not cover one another.
* Buttons remain accessible.
* Long file names do not break the layout.
* Error messages wrap correctly.
* Generated code scrolls intentionally.
* Horizontal page scrolling is not required unless designed.
* Loading indicators remain visible.
* Content remains readable.

### Validation Table

| Viewport    | Initial Page | Extraction Results | Generated Output | Result |
| ----------- | ------------ | ------------------ | ---------------- | ------ |
| 1920 × 1080 |              |                    |                  |        |
| 1440 × 900  |              |                    |                  |        |
| 1366 × 768  |              |                    |                  |        |
| 1280 × 720  |              |                    |                  |        |

---

## 32. Scenario 26 — Long File Name and Long Content

### Steps

1. Use a supported requirement file with a long file name.
2. Select the file.
3. Confirm the file name display.
4. Use a requirement containing long descriptions.
5. Run extraction and generation.
6. Review layout and scrolling.

### Expected Result

* Long file names wrap, truncate, or scroll safely.
* The layout does not expand uncontrollably.
* Long requirement text remains readable.
* Generated content remains accessible.
* Buttons are not pushed off-screen.
* Copying output remains possible.

### Evidence

```text
Scenario 26 result:

Long file-name behavior:

Long content behavior:

Layout issue:
- 
```

---

## 33. Scenario 27 — Keyboard Navigation

### Steps

1. Reload the application.
2. Do not use the mouse.
3. Press `Tab` through interactive controls.
4. Use `Shift + Tab` to move backward.
5. Activate buttons with `Enter` or `Space`.
6. Open the file chooser using the keyboard.
7. Observe focus indicators.
8. Navigate generated output areas.

### Expected Result

* Interactive elements receive focus.
* Focus order is logical.
* Focus is visibly indicated.
* Buttons can be activated by keyboard.
* Disabled controls are skipped or correctly announced.
* The file-input control is accessible.
* No keyboard trap occurs.
* Reset remains reachable.

### Evidence

```text
Scenario 27 result:

Logical focus order:

Visible focus indicator:

Keyboard activation:

Keyboard traps:

Inaccessible controls:
- 
```

---

## 34. Scenario 28 — Basic Screen-Reader Semantics Review

A full accessibility audit is outside the immediate Day 40 scope, but basic semantics must be reviewed.

### Confirm

* The page contains a meaningful primary heading.
* Form controls have accessible labels.
* Buttons have descriptive names.
* Error messages are visible as text.
* Status changes are understandable.
* Sections use meaningful headings.
* Images have appropriate alternative text.
* Decorative images do not create noise.
* Disabled states are semantic rather than visual only.

Record:

```text
Scenario 28 result:

Missing labels:

Unclear control names:

Heading issues:

Status-message issues:
- 
```

---

## 35. Scenario 29 — Browser Console Validation

Complete the main workflow and review the console continuously.

### Confirm There Are No

* Uncaught exceptions.
* Hydration errors.
* Infinite update warnings.
* Missing React keys.
* State-update warnings.
* Failed dynamic imports.
* Repeated identical errors.
* Sensitive requirement content logged unnecessarily.
* Debug-only logging left in production.

### Console Findings

| Message | Type | Scenario | Severity | Decision |
| ------- | ---- | -------- | -------- | -------- |
|         |      |          |          |          |

### Evidence

```text
Scenario 29 result:

Error count:

Warning count:

Accepted warnings:
- 

Blocking messages:
- 
```

---

## 36. Scenario 30 — Network Validation

Use the Network panel during extraction and generation.

### Confirm

* Requests occur only when expected.
* Duplicate requests do not occur from one user action.
* Requests complete or fail cleanly.
* Failed requests produce user-visible feedback.
* No request remains pending indefinitely.
* Required static assets load.
* Response status codes are appropriate.
* Sensitive values are not unnecessarily placed in URLs.
* Request payloads belong to the current file.
* Retry creates a new controlled request.

### Network Findings

| Request | Trigger | Status | Duplicate? | Duration | Result |
| ------- | ------- | -----: | ---------- | -------: | ------ |
|         |         |        |            |          |        |

### Evidence

```text
Scenario 30 result:

Failed requests:

Duplicate requests:

Long-running requests:

Unexpected payload:
- 
```

---

## 37. Scenario 31 — Error Message Quality

Review every error produced during the invalid and failure scenarios.

### A Good Error Message Should Explain

* What failed.
* Why it may have failed.
* What the user can do next.
* Whether retry is safe.
* Whether reset is needed.

### Confirm Error Messages Do Not

* Expose internal stack traces unnecessarily.
* Display raw object output.
* Contain only generic text such as `Something went wrong` when more context is available.
* Remain after a later successful action.
* Appear simultaneously with success messages.
* Refer to a previous file.

### Error Review Table

| Scenario | Error Text | Actionable? | Clears After Success? | Result |
| -------- | ---------- | ----------- | --------------------- | ------ |
|          |            |             |                       |        |

---

## 38. Scenario 32 — Success Message Quality

When success messages are implemented, confirm:

* The message reflects the completed operation.
* Extraction success and generation success are distinguishable.
* A new action clears outdated success messages.
* Reset clears success messages.
* Success is not shown after a failed operation.
* Messages do not block controls.

Record:

```text
Scenario 32 result:

Success messages accurate:

Stale success messages:

Conflicting messages:
- 
```

---

## 39. Scenario 33 — Cross-Browser Smoke Test

Perform a basic test in a second browser when available.

### Minimum Steps

1. Load the application.
2. Select Test File A.
3. Run extraction.
4. Review normalized output.
5. Generate artifacts.
6. Reset the workflow.
7. Review the console.

### Expected Result

* Primary workflow remains usable.
* No browser-specific layout failure appears.
* File selection works.
* Generated content is readable.
* Reset works.
* No browser-specific exception appears.

### Evidence

```text
Scenario 33 result:

Browser:

Version:

Primary workflow:

Layout:

Console findings:

Notes:
- 
```

---

## 40. Scenario 34 — Final Clean-Session Validation

This is the final browser test.

### Steps

1. Stop the production server.
2. Close the browser.
3. Start the production server again.
4. Open a new browser session.
5. Open the application URL.
6. Complete Test File A extraction.
7. Generate API artifacts.
8. Reset the workflow.
9. Complete Test File B extraction.
10. Review the final state.

### Expected Result

* The application starts cleanly.
* The first workflow completes.
* Generation succeeds.
* Reset clears all state.
* The second workflow contains no first-workflow data.
* No uncaught exception occurs.
* No critical warning appears.
* The application is ready for project freeze.

### Evidence

```text
Scenario 34 result:

First workflow:

Generation:

Reset:

Second workflow:

Stale data:

Console health:

Network health:

Final notes:
- 
```

---

## 41. Defect Recording Template

Record every browser defect using this format.

```text
Defect ID:

Title:

Scenario:

Test file:

Browser:

Steps to reproduce:
1.
2.
3.

Expected result:

Actual result:

Console output:

Network output:

Severity:

Reproducible:
[Yes / No / Intermittent]

Workaround:

Resolution decision:
```

---

## 42. Browser Defect Severity

### Severity 1 — Critical

Examples:

* Application crashes.
* Primary workflow cannot complete.
* Generated artifacts use another file’s data.
* Reset produces corrupted state.
* Production page does not load.
* Browser becomes unresponsive.

Decision:

```text
Must be fixed before freeze.
```

### Severity 2 — High

Examples:

* Extraction or generation fails for valid input.
* Missing-field logic is materially incorrect.
* Retry does not work.
* Loading remains active indefinitely.
* A refresh is required after a normal error.
* Duplicate requests produce duplicate output.

Decision:

```text
Should be fixed before freeze.
```

### Severity 3 — Medium

Examples:

* Recoverable UI inconsistency.
* Minor generated formatting problem.
* Confusing but usable error message.
* Issue at one supported viewport.
* Non-critical keyboard accessibility issue.

Decision:

```text
Fix when low risk or document for follow-up.
```

### Severity 4 — Low

Examples:

* Cosmetic spacing issue.
* Minor wording issue.
* Non-blocking console warning.
* Future usability enhancement.

Decision:

```text
Document unless correction is trivial and safe.
```

---

## 43. Browser Validation Checklist

### Initial Load

* [ ] Production page loads.
* [ ] Page title is correct.
* [ ] No blank screen.
* [ ] No uncaught exception.
* [ ] No hydration error.
* [ ] Required assets load.
* [ ] Initial state is clean.

### File Selection

* [ ] Supported file can be selected.
* [ ] File name displays correctly.
* [ ] File replacement works.
* [ ] Unsupported file is handled.
* [ ] Long file name does not break layout.

### Extraction

* [ ] Extraction starts once.
* [ ] Loading indicator appears.
* [ ] Duplicate extraction is prevented.
* [ ] Raw extraction is populated.
* [ ] Normalized extraction is populated.
* [ ] Metadata is populated.
* [ ] Missing fields are accurate.
* [ ] Loading ends.
* [ ] Error state clears after success.

### Flow Name

* [ ] Flow name is generated.
* [ ] Flow name matches current requirement.
* [ ] Invalid characters are handled.
* [ ] Manual edit works when supported.
* [ ] Generated output uses the final name.
* [ ] Reset clears the flow name.

### Business Rules

* [ ] Rules match the requirement.
* [ ] Rule order is understandable.
* [ ] No duplicate rules.
* [ ] No stale rules.
* [ ] Reset clears rules.

### API Generation

* [ ] Generation is guarded before extraction.
* [ ] Generation starts once.
* [ ] Loading indicator appears.
* [ ] Duplicate generation is prevented.
* [ ] Required artifacts appear.
* [ ] Output matches the current requirement.
* [ ] No placeholder values.
* [ ] No stale values.
* [ ] Loading ends.
* [ ] Retry works after failure.

### Reset

* [ ] File selection clears.
* [ ] Raw extraction clears.
* [ ] Normalized extraction clears.
* [ ] Metadata clears.
* [ ] Missing fields clear.
* [ ] Flow name clears.
* [ ] Business rules clear.
* [ ] Generated artifacts clear.
* [ ] Errors clear.
* [ ] Success messages clear.
* [ ] Loading state clears.
* [ ] New workflow starts cleanly.

### Error Handling

* [ ] Empty input is handled.
* [ ] Invalid input is handled.
* [ ] Extraction failure is handled.
* [ ] Generation failure is handled.
* [ ] Error messages are actionable.
* [ ] Errors clear after success.
* [ ] No stale success message remains.
* [ ] Retry is possible.
* [ ] Reset recovers the workflow.

### Layout and Accessibility

* [ ] Common desktop widths pass.
* [ ] Long content is readable.
* [ ] Buttons remain accessible.
* [ ] Keyboard navigation works.
* [ ] Focus is visible.
* [ ] No keyboard trap.
* [ ] Inputs have labels.
* [ ] Headings are meaningful.
* [ ] Status messages are understandable.

### Browser Health

* [ ] No blocking console error.
* [ ] No repeated request loop.
* [ ] No unintended duplicate request.
* [ ] No required request remains pending.
* [ ] Failed requests produce feedback.
* [ ] No sensitive content is logged unnecessarily.

---

## 44. Browser Validation Evidence Summary

Complete this section after testing.

### Scenario Results

| Scenario                     | Result | Defect ID | Notes |
| ---------------------------- | ------ | --------- | ----- |
| Initial application load     |        |           |       |
| Initial control state        |        |           |       |
| Valid file selection         |        |           |       |
| Complete extraction          |        |           |       |
| Raw extraction review        |        |           |       |
| Normalized extraction review |        |           |       |
| Metadata review              |        |           |       |
| Missing-field validation     |        |           |       |
| Minimal requirement          |        |           |       |
| Invalid input                |        |           |       |
| Flow-name generation         |        |           |       |
| Flow-name editing            |        |           |       |
| Business-rule generation     |        |           |       |
| API artifact generation      |        |           |       |
| Artifact content review      |        |           |       |
| Generation before extraction |        |           |       |
| Repeated extraction          |        |           |       |
| Complete reset               |        |           |       |
| New workflow after reset     |        |           |       |
| Extraction retry             |        |           |       |
| Generation retry             |        |           |       |
| Rapid user interaction       |        |           |       |
| Refresh behavior             |        |           |       |
| Browser navigation           |        |           |       |
| Responsive desktop layout    |        |           |       |
| Long file and content        |        |           |       |
| Keyboard navigation          |        |           |       |
| Basic semantics              |        |           |       |
| Console validation           |        |           |       |
| Network validation           |        |           |       |
| Error message quality        |        |           |       |
| Success message quality      |        |           |       |
| Cross-browser smoke test     |        |           |       |
| Final clean session          |        |           |       |

---

## 45. Defect Summary

| Severity | Open | Resolved | Deferred |
| -------- | ---: | -------: | -------: |
| Critical |      |          |          |
| High     |      |          |          |
| Medium   |      |          |          |
| Low      |      |          |          |

### Blocking Defects

```text
- None
```

Replace `None` when blocking defects are discovered.

### Accepted Warnings

```text
- 
```

### Deferred Improvements

```text
- 
```

---

## 46. Final Browser Validation Result

Select one result.

### PASS — Ready for Project Freeze

Use when:

* Complete extraction succeeds.
* API generation succeeds.
* Reset succeeds.
* A second workflow starts cleanly.
* No critical or high defect remains.
* Console contains no blocking error.
* Network behavior is stable.
* Supported desktop widths are usable.

```text
Browser validation result: PASS
```

### PASS WITH WARNINGS — Ready with Documented Conditions

Use when:

* The primary workflow passes.
* Reset passes.
* No critical or high defect remains.
* Medium or low issues are documented.
* Remaining issues do not affect required functionality.

```text
Browser validation result: PASS WITH WARNINGS
```

### FAIL — Not Ready for Project Freeze

Use when:

* Valid extraction fails.
* Generation fails.
* Reset leaves stale state.
* Sequential workflows mix data.
* The production application crashes.
* A critical or high defect remains.
* Browser behavior is unreliable.

```text
Browser validation result: FAIL
```

---

## 47. Final Results Section

Complete after executing browser validation.

```text
Final browser validation status:

Primary browser:

Secondary browser:

Complete requirement extraction:

Incomplete requirement extraction:

Invalid input handling:

Flow-name validation:

Business-rule validation:

API generation:

Reset validation:

Sequential workflow validation:

Console validation:

Network validation:

Responsive validation:

Keyboard validation:

Critical defects:

High-severity defects:

Medium-severity defects:

Low-severity defects:

Blocking issues:
- 

Accepted warnings:
- 

Deferred improvements:
- 
```

---

## 48. Phase 4 Completion Criteria

Phase 4 is complete when:

* The production application has been opened in a browser.
* Initial load has been validated.
* Complete requirement extraction has been validated.
* Incomplete requirement behavior has been validated.
* Invalid input handling has been validated.
* Raw and normalized output have been reviewed.
* Metadata has been reviewed.
* Missing-field behavior has been reviewed.
* Flow-name behavior has been reviewed.
* Business-rule generation has been reviewed.
* API artifact generation has been reviewed.
* Reset has been validated.
* A second workflow has been completed after reset.
* Retry behavior has been reviewed.
* Console and network activity have been reviewed.
* Supported desktop widths have been reviewed.
* Keyboard accessibility has been reviewed.
* Defects have been documented.
* A final browser-validation result has been assigned.

---

## 49. Phase 4 Completion Statement

The API Factory UI has completed the planned production-browser and end-to-end workflow validation.

The final result must be recorded as:

```text
PASS
PASS WITH WARNINGS
FAIL
```

When the result is `PASS` or `PASS WITH WARNINGS`, proceed to:

```text
docs/day40/project_freeze_checklist.md
```

This document will define the Phase 5 project-freeze checks.
