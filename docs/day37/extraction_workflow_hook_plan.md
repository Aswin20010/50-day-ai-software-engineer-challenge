# Day 37 — Extraction Workflow Hook Refactor Plan

## Purpose

Day 37 improves the structure of the API Factory UI by moving the requirement-extraction workflow out of `src/app/page.tsx` and into a reusable custom React hook.

Day 36 centralized extraction-state defaults and reset behavior. Day 37 builds on that work by separating extraction workflow coordination from page rendering.

The refactor must preserve all existing application behavior.

---

## Primary Goal

Create a reusable hook that manages the requirement-extraction workflow while allowing `src/app/page.tsx` to focus on:

* Rendering the user interface
* Passing user actions into the hook
* Displaying extraction results
* Displaying errors and loading states
* Coordinating unrelated page features

The new hook should manage extraction-specific state and workflow logic.

---

## Current Problem

The page component currently owns both UI behavior and requirement-extraction workflow logic.

This may include:

* Requirement-file validation
* File-content reading
* Extraction loading state
* Extraction errors
* Calls to `extractRequirements`
* Raw extraction state
* Normalized extraction state
* Extraction metadata
* Missing-field state
* Flow-name state
* Business-rule generation
* Extraction reset behavior
* Re-extraction behavior

Keeping all of this inside `src/app/page.tsx` increases the size and responsibility of the page component.

This creates several risks:

1. The page becomes harder to read.
2. Extraction behavior becomes harder to test independently.
3. State updates may become inconsistent.
4. Future extraction features may add more complexity to the page.
5. Reusing the extraction workflow in another component becomes difficult.
6. UI changes may unintentionally affect workflow logic.

---

## Refactor Scope

Day 37 will introduce a custom hook that coordinates the extraction workflow.

The planned hook is:

```text
src/hooks/useExtractionWorkflow.ts
```

The hook will use the Day 36 extraction-state helpers where appropriate.

The planned shared hook types are:

```text
src/types/day37.ts
```

The existing page will be updated:

```text
src/app/page.tsx
```

---

## Responsibilities to Move Into the Hook

The following extraction-specific responsibilities will be reviewed for movement into the hook:

* Extraction loading state
* Extraction error state
* Raw extracted data
* Normalized extraction data
* Extraction metadata
* Missing-field output
* Flow-name output
* Generated business rules
* Extraction reset behavior
* File-dependent result cleanup
* Extraction workflow coordination
* Re-extraction behavior
* Failure recovery behavior

The exact implementation must match the existing codebase and existing state types.

---

## Responsibilities to Keep in `page.tsx`

The page component should continue to own:

* Page layout
* Rendering
* File-input elements
* Button rendering
* User-interface labels
* Result sections
* Navigation
* Unrelated page state
* Unrelated generation workflows
* Visual styling
* Passing selected file or file content into the hook where appropriate

The hook should not contain JSX or direct rendering logic.

---

## Hook Design Principles

The custom hook must follow these principles:

1. Use explicit TypeScript types.
2. Avoid `any`.
3. Keep extraction behavior unchanged.
4. Reuse Day 36 state helpers.
5. Avoid duplicated extraction defaults.
6. Return a clear and stable hook API.
7. Keep browser and file logic limited to what the current workflow requires.
8. Avoid unrelated page state.
9. Avoid hidden side effects.
10. Keep reset behavior predictable.
11. Preserve current error behavior.
12. Preserve current state-update order where behavior depends on it.

---

## Planned Hook API

The final hook API may be adjusted to match the actual application, but it should follow a clear structure.

Conceptual example:

```ts
const {
  rawExtracted,
  normalizedExtraction,
  extractionMetadata,
  missingFields,
  flowName,
  rules,
  extractionError,
  isExtracting,
  extractFromFile,
  resetExtraction,
} = useExtractionWorkflow();
```

The hook may accept callbacks or dependencies when the current page owns related behavior.

Conceptual example:

```ts
const extraction = useExtractionWorkflow({
  buildRules,
  onFlowNameChange,
});
```

The final design must avoid passing unnecessary page implementation details into the hook.

---

## Planned Type Definitions

The following types may be introduced in:

```text
src/types/day37.ts
```

Possible types include:

* `UseExtractionWorkflowOptions`
* `UseExtractionWorkflowResult`
* `ExtractionWorkflowState`
* `ExtractionWorkflowActions`
* `ExtractionSuccessInput`
* `ExtractionFailureResult`
* Callback types for flow-name or rule updates

The exact types must reuse existing domain models where possible.

---

## Existing Workflow to Preserve

The current extraction workflow generally follows this sequence:

1. Confirm that a requirement file is available.
2. Clear stale extraction output where required.
3. Set extraction loading state.
4. Read the requirement file.
5. Call `extractRequirements`.
6. Preserve `useLLM: false`.
7. Store the raw extraction result.
8. Store the normalized extraction result.
9. Store extraction metadata.
10. Determine missing fields.
11. Populate the flow name.
12. Generate business rules.
13. Clear any previous extraction error.
14. Mark extraction as complete.
15. Handle failures using the existing error behavior.
16. Reset loading state.

The Day 37 hook must preserve this sequence and all observable behavior.

---

## Behavioral Constraints

The refactor must not change:

* The extraction service
* The extraction request payload
* The `useLLM: false` setting
* Supported file types
* File-reading behavior
* Requirement normalization
* Extraction metadata
* Missing-field validation
* Flow-name fallback behavior
* Business-rule generation
* Existing error messages
* Existing loading behavior
* Existing button behavior
* Existing UI layout
* Existing generated artifacts
* Existing success output
* Existing failure output

Day 37 is an architectural refactor only.

---

## Reset Scenarios

The hook must correctly support all current reset scenarios.

### Initial Page Load

The extraction workflow must begin with predictable empty values.

### Requirement File Change

State from the previous requirement file must be cleared when required.

### Requirement File Removal

File-dependent extraction output must be cleared.

### New Extraction Attempt

State that should not persist between attempts must be reset.

### Extraction Failure

Partial or stale success data must not remain unless the current behavior intentionally preserves it.

### Successful Re-Extraction

The latest result must completely replace the previous result.

### Manual Reset

The hook should expose a clear reset action when the page needs to reset extraction output.

---

## Loading-State Requirements

The hook must preserve the existing extraction loading behavior.

Expected behavior:

* Loading begins before extraction processing.
* Loading remains active while file reading and extraction are in progress.
* Loading stops after success.
* Loading stops after failure.
* Loading does not remain active after an exception.
* Duplicate extraction actions should be handled according to the existing behavior.

The hook should use `try`, `catch`, and `finally` where appropriate.

---

## Error-State Requirements

The hook must preserve the current extraction error behavior.

Expected behavior:

* Previous errors are cleared before a valid new attempt when appropriate.
* Extraction failures produce the same user-facing error behavior.
* Unknown errors are handled safely.
* Error state does not remain after a successful retry.
* Errors do not expose unsupported internal details.
* The hook does not introduce new error messages unless necessary.

---

## Business Rule Handling

Business-rule generation must remain behaviorally unchanged.

The implementation must determine whether the rule builder should:

* Remain outside the hook and be passed in as a dependency, or
* Be imported directly by the hook if it is already a stable extraction dependency

The selected approach should minimize unnecessary coupling.

The hook must not redesign or alter rule-generation logic.

---

## Flow Name Handling

Flow-name behavior must remain unchanged.

The hook must preserve:

* Extracted flow-name values
* Existing fallback behavior
* Reset behavior
* Replacement during re-extraction
* Empty-value handling

The implementation should avoid creating conflicting flow-name state in both the page and the hook.

---

## Day 36 Integration

The Day 37 hook should reuse the extraction-state helpers created during Day 36.

Relevant file:

```text
src/lib/day36/extractionStateHelpers.ts
```

The hook should not duplicate default values already centralized by Day 36.

The Day 36 helpers may be used for:

* Initial state construction
* Full reset values
* Success-state construction
* Failure-state cleanup
* File-change reset behavior

If the Day 36 helper API requires a small compatibility adjustment, the change must remain behavior-preserving and clearly documented.

---

## Dependency Rules

The hook may depend on:

* React hooks
* Existing extraction services
* Existing extraction models
* Day 36 state helpers
* Existing rule-generation helpers
* Existing validation helpers

The hook should not depend on:

* Page JSX
* UI components
* CSS modules
* DOM rendering details
* Navigation logic
* Unrelated artifact-generation workflows
* Unrelated page state

---

## Planned Files

### New Files

```text
docs/day37/extraction_workflow_hook_plan.md
src/types/day37.ts
src/hooks/useExtractionWorkflow.ts
docs/day37/extraction_workflow_hook_test_notes.md
docs/day37/testing_checklist.md
docs/day37/day37_completion_report.md
```

### Existing File to Update

```text
src/app/page.tsx
```

### Existing Day 36 File to Reuse

```text
src/lib/day36/extractionStateHelpers.ts
```

---

## Implementation Phases

### Phase 1 — Refactor Plan

Create this planning document.

### Phase 2 — Hook Types

Create the shared TypeScript contracts for the hook.

### Phase 3 — Hook Implementation

Create `useExtractionWorkflow.ts` and move extraction-specific workflow logic into it.

### Phase 4 — Page Integration

Update `src/app/page.tsx` to consume the hook.

### Phase 5 — Test Notes

Document successful extraction, failures, resets, loading states, and recovery.

### Phase 6 — Testing Checklist

Create a complete technical and browser validation checklist.

### Phase 7 — Completion Report

Summarize the implementation, validation, and final status.

---

## Validation Strategy

The Day 37 refactor will be validated using static, build, and browser testing.

### Static Validation

* TypeScript compilation
* Import validation
* Hook return-type validation
* React hook rule validation
* ESLint validation

### Build Validation

* Next.js production build
* Client component compatibility
* Module resolution
* Type compatibility

### Functional Validation

* Initial extraction state
* Valid file extraction
* Invalid file extraction
* Loading behavior
* Error behavior
* Missing-field output
* Flow-name output
* Business-rule output
* File-change reset
* File-removal reset
* Re-extraction
* Recovery after failure
* Manual reset behavior

---

## Suggested Commands

```powershell
npm run lint
npm run build
npm run dev
```

If the project includes automated tests, run the relevant test command defined in `package.json`.

---

## Regression Risks

The following risks must be reviewed carefully:

* State updates occurring in a different order
* Loading state not resetting after failure
* Errors persisting after a successful retry
* Duplicate flow-name state
* Duplicate rule state
* Stale extraction results after file change
* Partial state updates after an exception
* Hook dependency-array issues
* Unnecessary re-renders
* New callback identity issues
* Client/server component boundary errors
* File-reading behavior changing unintentionally

---

## Success Criteria

Day 37 will be considered successful when:

* The hook types are defined.
* The extraction workflow hook is implemented.
* Extraction-specific workflow logic is reduced in `page.tsx`.
* Day 36 state helpers are reused.
* Existing extraction behavior is preserved.
* Loading behavior is preserved.
* Error behavior is preserved.
* Reset behavior is preserved.
* Re-extraction behavior is preserved.
* The project passes lint and build validation.
* Manual browser testing identifies no regression.
* Test notes and completion documentation are created.

---

## Expected Outcome

After completing Day 37:

* `src/app/page.tsx` will be smaller and easier to read.
* Extraction state and workflow coordination will be centralized.
* Extraction behavior will be easier to test.
* Reset behavior will remain consistent with Day 36.
* Future extraction features can be added with less page-level complexity.
* The extraction workflow can be reused by other components if needed.
* Existing user-visible behavior will remain unchanged.
