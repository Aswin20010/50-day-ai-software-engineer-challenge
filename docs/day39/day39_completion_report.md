# Day 39 — Page Workflow Composition Completion Report

## Overview

Day 39 focused on simplifying the API Factory UI page by moving the remaining workflow state and cross-workflow coordination out of:

```text
src/app/page.tsx
```

and into a dedicated composition hook:

```text
src/hooks/useApiFactoryWorkflow.ts
```

The new hook composes:

```text
src/hooks/useExtractionWorkflow.ts
src/hooks/useFlowGeneration.ts
```

and centralizes editable workflow state, rule actions, file actions, extraction triggers, generation triggers, and reset coordination.

The result is a smaller, presentation-focused page component with clearer state ownership and improved workflow reuse.

---

## Day 39 Goal

The primary goal was to make `src/app/page.tsx` primarily responsible for rendering while preserving all existing user-visible behavior.

The refactor needed to preserve:

* Default flow name
* Requirement-file selection
* Requirement-file removal
* Requirement extraction
* `useLLM: false`
* Raw extraction output
* Normalized extraction output
* Extraction metadata
* Missing-field handling
* Extracted flow-name population
* Rule mapping
* Manual rule editing
* SOAP subflow synchronization
* Flow generation
* Generation metadata
* Generation error formatting
* Repeated generation
* Generated-flow download
* Cross-workflow reset behavior
* Failure recovery
* Existing layout and styling

Day 39 was an orchestration refactor only.

---

## Completed Phases

### Phase 1 — Workflow Composition Plan

Created:

```text
docs/day39/page_workflow_composition_plan.md
```

The plan documented:

* Existing page responsibilities
* Responsibilities to move into the composition hook
* Responsibilities to retain in the page
* Proposed hook API
* State ownership
* Rule and SOAP behavior
* File-change behavior
* Extraction and generation coordination
* Validation strategy
* Completion criteria

---

### Phase 2 — Page Workflow Types

Created:

```text
src/types/day39.ts
```

The Day 39 types define the active composition-hook contracts.

The implementation reuses established project types where possible, including:

* `BusinessRule`
* `NormalizedExtraction`
* `ExtractionMetadata`
* `GenerationMetadata`
* Existing extraction-hook contracts
* Existing generation-hook contracts

This avoids introducing duplicate domain models.

---

### Phase 3 — API Factory Workflow Hook

Created:

```text
src/hooks/useApiFactoryWorkflow.ts
```

The composition hook centralizes:

* Editable flow-name state
* Editable business-rule state
* SOAP subflow state
* Requirement-file state
* Normalized JSON visibility
* Rule addition
* Rule updates
* Rule removal
* Extracted flow-name handling
* Extracted rule handling
* File-change behavior
* File-removal behavior
* Extraction trigger coordination
* Generation trigger coordination
* Cross-workflow reset coordination
* Download delegation

---

### Phase 4 — Page Workflow Integration

Updated:

```text
src/app/page.tsx
```

Created:

```text
docs/day39/page_workflow_integration.md
```

The page now calls:

```ts
const workflow = useApiFactoryWorkflow();
```

and uses the returned values and actions to render the existing panels.

The page no longer directly manages:

* React workflow state
* Extraction-hook composition
* Generation-hook composition
* Rule-management logic
* File-change logic
* Extraction trigger logic
* Generation trigger logic
* Cross-workflow resets
* Service calls
* Payload construction
* Output formatting
* Download behavior

---

### Phase 5 — Workflow Composition Test Notes

Created:

```text
docs/day39/page_workflow_composition_test_notes.md
```

The test notes document validation for:

* Initial state
* Default flow name
* File selection
* File changes
* File removal
* Extraction start
* Extraction success
* Extraction failure
* Extraction recovery
* Flow-name handling
* Rule mapping
* Rule editing
* SOAP synchronization
* Normalized JSON visibility
* Generation success
* Generation failure
* Generation recovery
* Re-extraction
* Repeated generation
* Download
* Cross-workflow resets
* Page simplification
* Component compatibility
* State ownership
* Type validation
* Build validation
* Browser validation

---

### Phase 6 — Testing Checklist

Created:

```text
docs/day39/testing_checklist.md
```

The checklist provides final validation for:

* Required files
* Composition-hook structure
* Hook initialization order
* Callback stability
* State ownership
* Rule actions
* SOAP behavior
* File actions
* Extraction behavior
* Generation behavior
* Cross-workflow resets
* Page import cleanup
* Page logic removal
* Component prop compatibility
* Type safety
* Lint validation
* Production build validation
* Browser validation
* Regression safety

---

### Phase 7 — Completion Report

Created:

```text
docs/day39/day39_completion_report.md
```

This document records the final Day 39 implementation summary and completion status.

---

## Files Created

The following files were created for Day 39:

```text
docs/day39/page_workflow_composition_plan.md
src/types/day39.ts
src/hooks/useApiFactoryWorkflow.ts
docs/day39/page_workflow_integration.md
docs/day39/page_workflow_composition_test_notes.md
docs/day39/testing_checklist.md
docs/day39/day39_completion_report.md
```

---

## File Updated

The following existing file was updated:

```text
src/app/page.tsx
```

---

## Existing Hooks Reused

The composition hook reuses:

```text
src/hooks/useExtractionWorkflow.ts
src/hooks/useFlowGeneration.ts
```

The lower-level hooks remain focused on their own responsibilities.

### Extraction Hook

Continues to manage:

* File reading
* Requirement extraction
* Extraction response mapping
* Raw extraction state
* Normalized extraction state
* Extraction metadata
* Missing fields
* Extraction status
* Extraction loading
* Flow-name callback
* Rule callback
* Extraction reset behavior

### Generation Hook

Continues to manage:

* Generation payload construction
* Generation service execution
* Generated output
* Generation metadata
* Generation loading
* Success formatting
* Error formatting
* Download behavior
* Generation reset behavior

---

## Composition Hook Responsibilities

The final `useApiFactoryWorkflow` hook manages page-level workflow coordination.

### Editable State

* `flowName`
* `rules`
* `includeSoapSubflow`

### File and Presentation State

* `requirementFile`
* `showNormalizedJson`

### Rule Actions

* `addRule`
* `updateRule`
* `removeRule`

### File Actions

* `handleRequirementFileChange`
* File-removal reset behavior

### Extraction Coordination

* Extracted flow-name callback
* Extracted rules callback
* Extraction-start callback
* Extraction trigger
* Duplicate extraction protection
* Generation reset before extraction

### Generation Coordination

* Generation trigger
* Current-input assembly
* Duplicate generation protection
* Download delegation

---

## Page Responsibilities

The final `src/app/page.tsx` is responsible for:

* JSX
* Page layout
* Styling
* Header rendering
* Flow-name input rendering
* Panel placement
* Prop wiring

The page no longer contains workflow state or service coordination.

---

## Final State Ownership

### `useExtractionWorkflow`

Owns extraction-specific state and behavior.

### `useFlowGeneration`

Owns generation-specific state and behavior.

### `useApiFactoryWorkflow`

Owns editable workflow state and cross-workflow coordination.

### `src/app/page.tsx`

Owns presentation and layout.

This separation gives every state value one clear owner.

---

## Initial Workflow State

The composed workflow starts with:

```text
flowName: XCDTS005 Generated Flow
rules: []
includeSoapSubflow: false
requirementFile: null
showNormalizedJson: false

rawExtracted: null
normalizedExtraction: null
extractionMetadata: null
missingFields: []
extractStatus: ""
isExtracting: false

generated: ""
generationMetadata: null
isGenerating: false
```

---

## Default Flow Name Preservation

The default flow name remains:

```text
XCDTS005 Generated Flow
```

Expected behavior remains:

* It appears on initial load.
* The user can edit it.
* A valid extracted value replaces it.
* Empty extracted values are ignored.
* File removal restores it.
* Generation uses the latest current value.

---

## Requirement File Workflow

The composition hook owns the current requirement file.

### File Selection

When a file is selected:

1. The file becomes the current `requirementFile`.
2. Extraction state resets.
3. Generation state resets.
4. Normalized JSON is hidden.
5. Extraction does not start automatically.

### File Change

When another file is selected:

* Previous extraction output is cleared.
* Previous generated output is cleared.
* Previous metadata is cleared.
* Loading states are inactive.
* The new file becomes active.

### File Removal

When the file becomes `null`:

* Extraction state resets.
* Generation state resets.
* Normalized JSON is hidden.
* Default flow name is restored.
* Rules are cleared.
* SOAP subflow is disabled.
* No stale output remains.

---

## Extraction Workflow Preservation

The composition hook delegates extraction to:

```text
useExtractionWorkflow
```

It does not directly call the extraction service.

The extraction request remains:

```ts
{
  text,
  useLLM: false,
}
```

The following behavior remains unchanged:

* File reading
* Empty-file validation
* Raw extraction
* Normalized extraction
* Metadata
* Missing fields
* Extraction status
* Loading state
* Error handling
* Retry behavior

---

## Extracted Flow Name Handling

The composition hook receives the extracted flow name through a stable callback.

The callback:

1. Accepts the extracted string.
2. Trims whitespace.
3. Ignores empty values.
4. Updates editable flow-name state.

The user can still edit the value after extraction.

---

## Extracted Rule Handling

The composition hook receives mapped rules from the extraction hook.

The callback:

1. Replaces the editable rule list.
2. Preserves rule order.
3. Recalculates SOAP subflow state.
4. Leaves the resulting rules editable.

No duplicate rule state remains inside the page.

---

## Rule Editing Preservation

The composition hook provides:

```text
addRule
updateRule
removeRule
```

### Add Rule

* Appends a new rule using the requested `brId`.
* Preserves existing rules.
* Enables SOAP state when adding `SOAP-SUBFLOW`.

### Update Rule

* Updates only the selected rule.
* Preserves other rules.
* Preserves rule order.
* Makes the latest value available for generation.

### Remove Rule

* Removes only the selected rule.
* Preserves remaining rule order.
* Recalculates SOAP state.

---

## SOAP Subflow Synchronization

SOAP subflow state remains synchronized with the editable rule list.

The value becomes `true` when:

* The user adds `SOAP-SUBFLOW`.
* Extracted rules contain `SOAP-SUBFLOW`.

The value becomes `false` when:

* Extracted rules do not contain `SOAP-SUBFLOW`.
* The last SOAP rule is removed.
* The requirement file is removed.

Generation receives the current value unchanged.

---

## Normalized JSON Visibility

The composition hook owns:

```text
showNormalizedJson
```

It exposes:

```text
toggleNormalizedJson
```

The view is hidden when:

* A new extraction starts.
* The selected file changes.
* The selected file is removed.

This state remains outside the lower-level extraction hook because it is presentation-oriented.

---

## Generation Workflow Preservation

The composition hook delegates generation to:

```text
useFlowGeneration
```

It passes the current:

* Flow name
* Editable rules
* SOAP subflow state
* Normalized extraction

The composition hook does not:

* Build the payload
* Call the generation service
* Format successful output
* Format errors
* Manage generation metadata directly
* Perform downloads directly

---

## Current Input Usage

Every generation request uses the current workflow state.

This includes:

* Latest flow-name edits
* Added rules
* Updated rules
* Removed rules
* Current SOAP state
* Latest normalized extraction

No stale duplicate input state is retained inside the generation hook.

---

## Cross-Workflow Reset Coordination

When a new extraction starts, the composition hook calls:

```text
resetGeneration
```

This clears:

* Generated output
* Generation metadata
* Generation loading state

The extraction then proceeds normally.

This prevents stale generated flows from remaining visible after requirements change.

---

## Failure Isolation

Extraction and generation failures remain independent.

### Extraction Failure

* Existing extraction status behavior is preserved.
* Extraction loading returns to inactive.
* Generation remains reset.
* A valid retry can succeed.

### Generation Failure

* Existing formatted error output is preserved.
* Generation metadata is cleared.
* Generation loading returns to inactive.
* Extraction state remains unchanged.
* A valid retry can succeed.

The composition hook does not introduce a shared error state.

---

## Repeated Workflow Behavior

The composed workflow continues to support:

* Re-extraction
* Repeated generation
* Editing after extraction
* Editing after generation
* Retry after extraction failure
* Retry after generation failure
* Download after successful generation
* Selecting another file
* Starting over after file removal

The latest current values are used for each action.

---

## Download Preservation

Download remains delegated to `useFlowGeneration`.

Expected behavior remains:

* Empty output does not download.
* The latest generated output is downloaded.
* Download does not trigger generation.
* Download does not clear state.
* Downloaded content matches displayed content.

---

## Page Simplification

After Day 39, `src/app/page.tsx` no longer needs:

```text
useState
useCallback
useExtractionWorkflow
useFlowGeneration
BusinessRule
Extraction services
Generation services
Rule mappers
State helpers
Payload builders
Output formatters
Download helpers
```

The page imports:

```text
RequirementExtractionPanel
RuleBuilderPanel
GenerationOutputPanel
useApiFactoryWorkflow
```

and renders the existing interface.

---

## Component Compatibility

The composition-hook actions remain compatible with the existing component props.

### Requirement Extraction Panel

Receives:

* Extraction status
* Missing fields
* Normalized extraction
* Raw extraction
* Extraction metadata
* JSON visibility
* File-change action
* Extraction action
* JSON-toggle action

### Rule Builder Panel

Receives:

* Rules
* Missing fields
* Add action
* Update action
* Remove action

### Generation Output Panel

Receives:

* Generated output
* Generation metadata
* Generate action
* Download action

No component redesign was required.

---

## Validation Commands

Run:

```powershell
npm run lint
npm run build
npm run dev
```

---

## Validation Results

Complete these fields using the actual command and browser results.

### Lint

```text
[ ] PASS
[ ] PASS WITH NON-BLOCKING WARNINGS
[ ] FAIL
```

### Production Build

```text
[ ] PASS
[ ] PASS WITH NON-BLOCKING WARNINGS
[ ] FAIL
```

### Browser Validation

```text
[ ] PASS
[ ] PASS WITH NON-BLOCKING WARNINGS
[ ] FAIL
```

---

## Known Non-Blocking Warning

The Next.js production build may continue to show the existing multiple-lockfile workspace-root warning.

Example:

```text
Next.js inferred your workspace root, but it may not be correct.
Multiple lockfiles were detected.
```

Known lockfiles may include:

```text
C:\Users\BH31021\api-factory-generator\package-lock.json
C:\Users\BH31021\api-factory-generator\api-factory-ui\package-lock.json
```

This warning is unrelated to the Day 39 workflow-composition refactor when the build otherwise succeeds.

---

## Code Quality Improvements

Day 39 improves the codebase through:

### Smaller Page Component

The page now focuses on rendering rather than workflow orchestration.

### Centralized Page Workflow

Editable state and cross-workflow actions have one owner.

### Focused Lower-Level Hooks

Extraction and generation hooks remain responsible only for their own workflows.

### Clear State Ownership

No unnecessary duplicate workflow state remains in the page.

### Centralized Rule Logic

Add, update, remove, and SOAP synchronization behavior are grouped together.

### Centralized Reset Logic

File changes and extraction starts coordinate both workflows consistently.

### Improved Reusability

The complete API Factory workflow can be reused by another page or component.

### Improved Testability

The complete workflow can be validated through the composition hook without depending on page markup.

---

## Risks Reduced

The refactor reduces the risk of:

* Page-component growth
* Duplicate state ownership
* Stale rules during generation
* Stale flow names during generation
* Unsynchronized SOAP state
* Incomplete file-change resets
* Stale generated output after extraction
* Callback logic being duplicated in the page
* Service behavior leaking into rendering code
* UI refactors accidentally changing workflow behavior

---

## Future Improvements

Potential future enhancements include:

* Dedicated unit tests for `useApiFactoryWorkflow`
* React Testing Library integration tests
* Grouped return objects for extraction, generation, and editable state
* Reducer-based editable workflow state
* Derived SOAP state instead of stored SOAP state
* Stronger rule-field typing
* File-type and file-size validation
* Status-message components
* Disable-state integration for extraction and generation buttons
* End-to-end browser tests
* Resolving the multiple-lockfile workspace warning

These improvements are outside the Day 39 scope.

---

## Completion Criteria

The Day 39 implementation criteria are:

* [x] Workflow composition plan created
* [x] Day 39 type file created
* [x] `useApiFactoryWorkflow` created
* [x] Extraction hook composed
* [x] Generation hook composed
* [x] Flow-name state moved out of the page
* [x] Rule state moved out of the page
* [x] SOAP state moved out of the page
* [x] Requirement-file state moved out of the page
* [x] Normalized JSON state moved out of the page
* [x] Rule actions moved out of the page
* [x] File actions moved out of the page
* [x] Extraction trigger moved out of the page
* [x] Generation trigger moved out of the page
* [x] Cross-workflow resets moved out of the page
* [x] Page integration documented
* [x] Test notes created
* [x] Testing checklist created
* [x] Completion report created

Update after final validation:

* [ ] Lint passed
* [ ] Production build passed
* [ ] Browser validation passed
* [ ] No functional regression identified

---

## Final Status

Use the status matching the actual validation results:

```text
DAY 39 — PAGE WORKFLOW COMPOSITION REFACTOR

STATUS:
[ ] COMPLETE
[ ] COMPLETE WITH NON-BLOCKING WARNINGS
[ ] INCOMPLETE — VALIDATION OR FIXES REQUIRED
```

Day 39 is ready for final validation and commit after the lint, production build, and browser results are recorded.
