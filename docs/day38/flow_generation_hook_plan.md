# Day 38 — Flow Generation Hook Refactor Plan

## Purpose

Day 38 improves the architecture of the API Factory UI by moving the Node-RED flow-generation and download workflow out of:

```text
src/app/page.tsx
```

and into a reusable custom React hook.

The planned hook is:

```text
src/hooks/useFlowGeneration.ts
```

Day 37 moved requirement extraction into `useExtractionWorkflow`. Day 38 applies the same separation-of-concerns approach to flow generation while preserving all current application behavior.

---

## Primary Goal

Create a reusable hook that coordinates the Node-RED flow-generation workflow.

The hook should manage:

* Generated flow output
* Generation metadata
* Generation loading state
* Generation error handling
* Flow-generation request construction
* Flow-generation service calls
* Generated-output formatting
* Generated-flow download
* Generation-state reset behavior

The page component should continue to own the editable values used as generation inputs.

---

## Current Generation Workflow

The current flow-generation workflow is implemented directly inside:

```text
src/app/page.tsx
```

The page currently manages:

```ts
const [generated, setGenerated] = useState("");

const [generationMetadata, setGenerationMetadata] =
  useState<GenerationMetadata | null>(null);
```

The page also contains generation and download functions similar to:

```ts
async function generateFlow() {
  try {
    setGenerationMetadata(null);

    const payload = buildFlowGenerationPayload({
      flowName,
      includeSoapSubflow,
      normalizedExtraction,
      rules,
    });

    const data = await generateNodeRedFlow(payload);

    setGenerationMetadata(
      data.generationMetadata || null
    );

    setGenerated(
      formatGeneratedFlowOutput(data.flow)
    );
  } catch (err) {
    setGenerationMetadata(null);

    setGenerated(
      formatGenerationErrorOutput(err)
    );
  }
}

function downloadFlow() {
  if (!generated) {
    return;
  }

  downloadJsonText(generated);
}
```

This behavior works, but it increases the responsibilities of the page component.

---

## Current Problem

`src/app/page.tsx` currently coordinates multiple workflows:

* Requirement-file selection
* Requirement extraction
* Editable flow-name management
* Editable business-rule management
* SOAP subflow state
* Node-RED flow generation
* Generation error handling
* Generated output formatting
* Generated flow download
* Page rendering

Day 37 reduced this responsibility by moving requirement extraction into a hook.

The remaining generation logic creates several maintainability risks:

1. The page still coordinates service-level generation behavior.
2. Generation state is mixed with editable form state.
3. Error formatting remains coupled to page rendering.
4. Download behavior remains implemented in the page.
5. Reusing generation behavior in another component would require duplication.
6. Future generation features would continue increasing page size.
7. Generation loading behavior is not represented as a dedicated state.
8. Reset behavior is distributed between extraction callbacks and page handlers.

---

## Refactor Scope

Day 38 will introduce:

```text
src/hooks/useFlowGeneration.ts
```

The hook will coordinate the existing generation functions:

```text
buildFlowGenerationPayload
generateNodeRedFlow
formatGeneratedFlowOutput
formatGenerationErrorOutput
downloadJsonText
```

Shared hook types will be created in:

```text
src/types/day38.ts
```

The existing page will be updated:

```text
src/app/page.tsx
```

The existing services and helpers will be reused without changing their core behavior.

---

## Existing Files to Reuse

The Day 38 hook will reuse the existing files:

```text
src/services/day33/flowGenerationService.ts
src/lib/day35/generatedOutputHelpers.ts
src/lib/day35/downloadHelpers.ts
src/types/day32.ts
```

The hook must use the existing:

```ts
buildFlowGenerationPayload
generateNodeRedFlow
formatGeneratedFlowOutput
formatGenerationErrorOutput
downloadJsonText
```

Day 38 must not duplicate or redesign the logic already provided by these functions.

---

## Responsibilities to Move Into the Hook

The following responsibilities will move from `page.tsx` into `useFlowGeneration`:

* `generated` state
* `generationMetadata` state
* Generation loading state
* Generation error representation
* Generation request coordination
* Calls to `buildFlowGenerationPayload`
* Calls to `generateNodeRedFlow`
* Calls to `formatGeneratedFlowOutput`
* Calls to `formatGenerationErrorOutput`
* Generated-flow download coordination
* Generation-state reset behavior

The hook may expose actions such as:

```text
generateFlow
downloadFlow
resetGeneration
```

The final action names should be clear and consistent with the current project.

---

## Responsibilities to Keep in `page.tsx`

The page should continue to own the editable generation inputs:

* `flowName`
* `rules`
* `includeSoapSubflow`
* Requirement-file selection
* Normalized JSON visibility
* Page layout
* Component rendering

The normalized extraction result remains owned by `useExtractionWorkflow` and is passed into the generation hook when generation is requested.

The page should also remain responsible for:

* Flow-name input rendering
* Rule-builder rendering
* Extraction-panel rendering
* Generation-output panel rendering
* User interactions unrelated to generation service coordination

---

## State Ownership After Refactor

### `useExtractionWorkflow`

The extraction hook continues to manage:

* `rawExtracted`
* `normalizedExtraction`
* `extractionMetadata`
* `missingFields`
* `extractStatus`
* `isExtracting`

### `useFlowGeneration`

The generation hook will manage:

* `generated`
* `generationMetadata`
* `isGenerating`
* Generation failure output or error state
* Generation reset behavior

### `page.tsx`

The page will continue to manage:

* `flowName`
* `rules`
* `includeSoapSubflow`
* `requirementFile`
* `showNormalizedJson`

This separation keeps editable user inputs in the page while moving service workflow state into hooks.

---

## Proposed Hook Input

The generation hook should receive the current generation inputs when generation is requested.

A conceptual input type is:

```ts
type GenerateFlowInput = {
  flowName: string;
  includeSoapSubflow: boolean;
  normalizedExtraction: NormalizedExtraction | null;
  rules: BusinessRule[];
};
```

This avoids permanently duplicating editable page state inside the hook.

The page remains the source of truth for editable values.

---

## Proposed Hook API

The final API may be adjusted to match the existing project types, but it should follow a structure similar to:

```ts
const {
  generated,
  generationMetadata,
  isGenerating,
  generateFlow,
  downloadFlow,
  resetGeneration,
} = useFlowGeneration();
```

The page could call:

```ts
await generateFlow({
  flowName,
  includeSoapSubflow,
  normalizedExtraction,
  rules,
});
```

The hook should not require separate setters for each generation input.

---

## Planned Type Definitions

The following types may be created in:

```text
src/types/day38.ts
```

Possible contracts include:

* `GenerateFlowInput`
* `FlowGenerationState`
* `FlowGenerationActions`
* `UseFlowGenerationOptions`
* `UseFlowGenerationResult`
* `FlowGenerationSuccessResult`
* `FlowGenerationFailureResult`

The final types should reuse existing project models from:

```text
src/types/day32.ts
```

especially:

```text
BusinessRule
GenerationMetadata
NormalizedExtraction
```

The implementation should avoid duplicating established domain types.

---

## Existing Workflow to Preserve

The current generation sequence is:

1. The user extracts or manually enters generation inputs.
2. The user clicks the Generate Flow action.
3. Existing generation metadata is cleared.
4. A request payload is built using:

   * Flow name
   * SOAP subflow option
   * Normalized extraction
   * Business rules
5. `generateNodeRedFlow` is called.
6. The returned generation metadata is stored.
7. The generated flow is formatted as displayable JSON text.
8. The generated output is displayed.
9. If generation fails:

   * Generation metadata is cleared.
   * The error is formatted using the existing helper.
   * The formatted failure output is displayed.
10. The user can download the generated JSON output.

The Day 38 hook must preserve this observable behavior.

---

## Generation Request Preservation

The generation hook must continue to build requests using:

```ts
buildFlowGenerationPayload({
  flowName,
  includeSoapSubflow,
  normalizedExtraction,
  rules,
});
```

Day 38 must not change:

* Payload field names
* Payload data structure
* Rule mapping
* Flow-name handling
* SOAP subflow behavior
* Normalized extraction handling
* Service request behavior

The payload helper remains the source of truth for request construction.

---

## Generation Service Preservation

The hook must continue to call:

```ts
generateNodeRedFlow(payload);
```

The generation service must not be moved, renamed, duplicated, or redesigned during Day 38.

The hook coordinates the call but does not replace the service.

---

## Generated Output Preservation

Successful output must continue to use:

```ts
formatGeneratedFlowOutput(data.flow);
```

The hook must not manually duplicate JSON formatting that is already handled by this helper.

The resulting `generated` string must remain compatible with:

```text
GenerationOutputPanel
downloadJsonText
```

---

## Generation Error Preservation

Generation errors must continue to use:

```ts
formatGenerationErrorOutput(error);
```

The hook should not introduce a different user-facing generation error format.

On failure:

* `generationMetadata` must be cleared.
* Generated output must contain the formatted error.
* Loading state must return to `false`.
* Extraction state must remain unchanged.
* Editable flow name and rules must remain unchanged.
* The user must be able to retry.

---

## Download Behavior Preservation

Download must continue to use:

```ts
downloadJsonText(generated);
```

Expected behavior:

* No download occurs when generated output is empty.
* Existing generated output is downloaded without modification.
* The hook must not rebuild the flow solely for download.
* Download behavior must remain independent from requirement extraction.
* Download must not reset generation state.
* Download must not alter generation metadata.

---

## Generation Loading State

Day 38 should introduce explicit generation loading state:

```text
isGenerating
```

Expected behavior:

* `isGenerating` becomes `true` before request construction or service execution.
* It remains active during the generation request.
* It becomes `false` after success.
* It becomes `false` after failure.
* It does not remain active after an exception.
* Repeated generation actions should be guarded or disabled while generation is already running.

The final UI usage depends on the existing `GenerationOutputPanel` props.

If the panel does not yet accept `isGenerating`, the hook should still expose it for future integration and page-level guarding.

---

## Generation Reset Scenarios

The hook must expose a predictable reset action.

### Requirement File Change

When a different requirement file is selected:

* Previous generated output must be cleared.
* Previous generation metadata must be cleared.
* Loading state must be inactive.

### Requirement File Removal

When the selected file is removed:

* Generated output must be cleared.
* Generation metadata must be cleared.
* Loading state must be inactive.

### New Extraction Attempt

When requirement extraction starts:

* Previous generated output must be cleared.
* Previous generation metadata must be cleared.
* The previous generated flow must not remain visible while inputs are changing.

### Manual Generation Reset

The page should be able to reset generation state without changing extraction state or editable rules.

### New Generation Attempt

Before generating again:

* Previous generation metadata must be cleared.
* Existing generated output may remain or be cleared according to current behavior.
* The latest result must replace the previous result.

The implementation should preserve current user-visible behavior.

---

## Extraction Integration

Day 38 must remain compatible with `useExtractionWorkflow`.

The extraction-start callback currently clears generation state in `page.tsx`.

After Day 38, the callback should call:

```ts
resetGeneration();
```

instead of directly calling:

```ts
setGenerated("");
setGenerationMetadata(null);
```

Similarly, requirement-file changes should call both:

```ts
resetExtraction();
resetGeneration();
```

This keeps extraction and generation reset responsibilities separated.

---

## Page Integration Plan

After the hook is implemented, `src/app/page.tsx` should:

1. Import `useFlowGeneration`.
2. Remove the `generated` state declaration.
3. Remove the `generationMetadata` state declaration.
4. Remove direct generation-service imports.
5. Remove generated-output helper imports.
6. Remove the download helper import.
7. Remove the page-level `generateFlow` function.
8. Remove the page-level `downloadFlow` function.
9. Read generation state from the hook.
10. Pass the hook actions to `GenerationOutputPanel`.
11. Call `resetGeneration` when extraction starts.
12. Call `resetGeneration` when requirement files change or are removed.

---

## Imports Expected to Move Out of `page.tsx`

The following imports should move into the generation hook:

```ts
import {
  buildFlowGenerationPayload,
  generateNodeRedFlow,
} from "../services/day33/flowGenerationService";

import { downloadJsonText } from "../lib/day35/downloadHelpers";

import {
  formatGeneratedFlowOutput,
  formatGenerationErrorOutput,
} from "../lib/day35/generatedOutputHelpers";
```

After integration, the page should no longer need these imports.

---

## Imports Expected in the Hook

The hook will likely import:

```ts
import { useCallback, useState } from "react";

import {
  buildFlowGenerationPayload,
  generateNodeRedFlow,
} from "../services/day33/flowGenerationService";

import { downloadJsonText } from "../lib/day35/downloadHelpers";

import {
  formatGeneratedFlowOutput,
  formatGenerationErrorOutput,
} from "../lib/day35/generatedOutputHelpers";
```

It should also import the required types from:

```text
src/types/day32.ts
src/types/day38.ts
```

---

## Dependency Rules

The generation hook may depend on:

* React hooks
* Existing generation services
* Existing generation payload helper
* Existing output-formatting helpers
* Existing download helper
* Existing Day 32 types
* Day 38 hook contract types

The generation hook should not depend on:

* Requirement extraction service
* Requirement extraction mapper
* Extraction-state helper
* UI components
* JSX
* CSS
* DOM rendering details
* File-input state
* Rule-editor event handlers
* Flow-name input event handlers

---

## Behavioral Constraints

Day 38 must not change:

* Existing flow-generation request shape
* Existing generation endpoint behavior
* Existing generated-flow response handling
* Existing generation metadata handling
* Existing success formatting
* Existing error formatting
* Existing downloaded content
* Existing flow name
* Existing business rules
* Existing SOAP subflow state
* Existing normalized extraction
* Existing extraction hook
* Existing page layout
* Existing component labels
* Existing output panel behavior

Day 38 is an architectural refactor only.

---

## Error and Recovery Requirements

The hook must safely support generation failure and retry.

Expected behavior:

1. A generation error is caught.
2. Metadata is cleared.
3. The existing error formatter creates the output.
4. `isGenerating` becomes `false`.
5. Extraction state remains available.
6. Flow name remains editable.
7. Rules remain editable.
8. A subsequent generation request can succeed.
9. The successful output replaces the previous error output.
10. No stale error-specific state remains after success.

---

## Repeated Generation Requirements

The user must be able to generate more than once.

The hook must support:

* Generating with the same inputs
* Generating after editing the flow name
* Generating after adding a rule
* Generating after updating a rule
* Generating after removing a rule
* Generating after changing SOAP subflow state
* Generating after a new extraction
* Retrying after a generation failure

Each request must use the current inputs passed by the page.

---

## Concurrency Considerations

The generation action should avoid accidental duplicate requests.

Possible behavior:

```ts
if (isGenerating) {
  return;
}
```

However, React state captured inside callbacks can become stale.

The implementation may use a ref if a strict synchronous duplicate-request guard is required.

At minimum:

* The UI should not intentionally start overlapping requests.
* `isGenerating` should be exposed to the page.
* The generation button should be disabled where the current panel supports it.
* The latest successful result should replace the previous result.

Advanced request cancellation is outside the scope of Day 38.

---

## Planned Files

### New Files

```text
docs/day38/flow_generation_hook_plan.md
src/types/day38.ts
src/hooks/useFlowGeneration.ts
docs/day38/flow_generation_hook_test_notes.md
docs/day38/testing_checklist.md
docs/day38/day38_completion_report.md
```

### Existing File to Update

```text
src/app/page.tsx
```

### Existing Files to Reuse

```text
src/services/day33/flowGenerationService.ts
src/lib/day35/generatedOutputHelpers.ts
src/lib/day35/downloadHelpers.ts
src/types/day32.ts
src/hooks/useExtractionWorkflow.ts
```

---

## Implementation Phases

### Phase 1 — Generation Hook Refactor Plan

Create this planning document.

### Phase 2 — Generation Hook Types

Create typed state, input, action, success, failure, and hook return contracts.

### Phase 3 — Flow Generation Hook

Create `useFlowGeneration.ts` using the existing generation services and helpers.

### Phase 4 — Page Integration

Update `src/app/page.tsx` to use the generation hook and remove direct generation workflow logic.

### Phase 5 — Hook Test Notes

Document generation success, errors, reset behavior, repeated generation, download behavior, and extraction integration.

### Phase 6 — Testing Checklist

Create a complete static, browser, regression, and state-ownership validation checklist.

### Phase 7 — Completion Report

Summarize implementation details, files changed, validation results, known warnings, and final status.

---

## Validation Strategy

The Day 38 refactor will be validated through static, build, and browser testing.

### Static Validation

* TypeScript compilation
* Hook return-type validation
* Callback parameter validation
* React hook dependency validation
* Import validation
* ESLint validation

### Generation Validation

* Successful flow generation
* Generation metadata storage
* Generated output formatting
* Generation error formatting
* Retry after failure
* Repeated generation
* Latest input usage
* Loading-state behavior

### Download Validation

* No download for empty output
* Download after successful generation
* Downloaded content matches displayed output
* Download does not reset state
* Download remains available after repeated generation

### Integration Validation

* Extraction still works
* New extraction clears old generated output
* File change clears generation state
* File removal clears generation state
* Rule editing still works
* Flow-name editing still works
* SOAP subflow behavior remains correct
* Generation uses current normalized extraction

---

## Suggested Commands

Run:

```powershell
npm run lint
npm run build
npm run dev
```

If the project contains automated tests, also run the configured test command from `package.json`.

---

## Regression Risks

The following risks must be reviewed carefully:

* Generation inputs captured with stale values
* Generated output not resetting when extraction restarts
* Generation metadata remaining from a previous request
* Loading state remaining active after an exception
* Download using outdated output
* Error output being overwritten incorrectly
* Duplicate generation requests
* Hook dependencies causing unnecessary callback recreation
* Page still retaining duplicate generation state
* Generation service being called twice
* Current rules not being included in the payload
* SOAP subflow state becoming unsynchronized
* Normalized extraction being lost during page integration
* Output panel props becoming incompatible
* Existing extraction behavior being unintentionally modified

---

## Success Criteria

Day 38 will be considered successful when:

* `src/types/day38.ts` is created.
* `src/hooks/useFlowGeneration.ts` is created.
* Generated output state moves out of `page.tsx`.
* Generation metadata state moves out of `page.tsx`.
* Generation service coordination moves into the hook.
* Output formatting moves into the hook.
* Error formatting moves into the hook.
* Download coordination moves into the hook.
* `page.tsx` uses the hook.
* Extraction resets call `resetGeneration`.
* File changes call `resetGeneration`.
* Current flow name is used during generation.
* Current rules are used during generation.
* Current SOAP subflow state is used.
* Current normalized extraction is used.
* Repeated generation works.
* Generation failure recovery works.
* Download behavior remains unchanged.
* Lint passes.
* Production build passes.
* Browser validation passes.
* No functional regression is identified.
* Test notes and completion documentation are created.

---

## Expected Outcome

After Day 38:

* `src/app/page.tsx` will contain less workflow coordination.
* Flow generation will be centralized in a reusable hook.
* Generation state ownership will be clearer.
* Generated output and metadata will use one consistent reset path.
* Extraction and generation workflows will remain separated.
* Current editable inputs will remain page-owned.
* Generation failures will remain recoverable.
* Download behavior will remain unchanged.
* The generation workflow will be easier to test and reuse.
* Existing user-visible behavior will remain unchanged.
