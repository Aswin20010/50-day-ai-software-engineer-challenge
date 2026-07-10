# Day 39 — Page Workflow Composition Refactor Plan

## Purpose

Day 39 improves the architecture of the API Factory UI by extracting the remaining page-level workflow coordination from:

```text
src/app/page.tsx
```

into a dedicated composition hook:

```text
src/hooks/useApiFactoryWorkflow.ts
```

Day 37 moved requirement extraction into `useExtractionWorkflow`.

Day 38 moved Node-RED flow generation into `useFlowGeneration`.

Day 39 composes those two hooks and centralizes the remaining editable workflow state and cross-workflow coordination.

The page component should become primarily responsible for rendering the user interface.

---

## Primary Goal

Create a reusable page workflow hook that coordinates:

* Requirement extraction
* Flow generation
* Editable flow-name state
* Editable business-rule state
* SOAP subflow synchronization
* Requirement-file selection
* Normalized JSON visibility
* Extraction and generation resets
* File-change behavior
* File-removal behavior
* Extraction-to-generation integration

The refactor must preserve all current application behavior.

---

## Current Architecture

The current page already delegates major service workflows to two hooks.

### Extraction Hook

```text
src/hooks/useExtractionWorkflow.ts
```

This hook manages:

* Raw extracted data
* Normalized extraction
* Extraction metadata
* Missing fields
* Extraction status
* Extraction loading state
* File reading
* Requirement extraction
* Rule mapping callbacks
* Flow-name callbacks
* Extraction reset behavior

### Generation Hook

```text
src/hooks/useFlowGeneration.ts
```

This hook manages:

* Generated flow output
* Generation metadata
* Generation loading state
* Flow-generation payload construction
* Flow-generation service calls
* Success formatting
* Error formatting
* Download behavior
* Generation reset behavior

### Page Component

The page still manages:

* Editable flow name
* Editable rules
* SOAP subflow state
* Requirement-file selection
* Normalized JSON visibility
* Rule add behavior
* Rule update behavior
* Rule removal behavior
* File-change handling
* Extraction trigger handling
* Generation trigger handling
* Cross-hook reset coordination
* Page rendering

Day 39 will move these remaining workflow responsibilities into a composition hook.

---

## Current Problem

Although Day 37 and Day 38 reduced page complexity, `src/app/page.tsx` still coordinates multiple pieces of business workflow state.

This creates several risks:

1. The page still contains non-rendering workflow logic.
2. Rule synchronization logic is coupled to JSX.
3. SOAP subflow state must be updated in multiple handlers.
4. File-change resets require coordination across two hooks.
5. Extraction callbacks are declared inside the page.
6. Generation inputs are assembled inside the page.
7. The page must understand internal hook sequencing.
8. Future workflow additions would continue increasing page complexity.
9. Testing the complete workflow requires rendering the page.
10. Reusing the workflow in another page would require duplication.

---

## Refactor Scope

Day 39 will introduce:

```text
src/hooks/useApiFactoryWorkflow.ts
```

The hook will compose:

```text
useExtractionWorkflow
useFlowGeneration
```

Shared hook types will be created in:

```text
src/types/day39.ts
```

The existing page will be updated:

```text
src/app/page.tsx
```

The existing extraction and generation hooks must remain focused and reusable.

---

## Responsibilities Moving Into the Composition Hook

The following responsibilities will move from `page.tsx` into `useApiFactoryWorkflow`.

### Editable State

* `flowName`
* `rules`
* `includeSoapSubflow`
* `requirementFile`
* `showNormalizedJson`

### Rule Actions

* Adding a rule
* Updating a rule
* Removing a rule
* Synchronizing SOAP subflow state from rules

### File Actions

* Selecting a requirement file
* Removing a requirement file
* Resetting extraction state on file changes
* Resetting generation state on file changes
* Restoring the default flow name when required
* Clearing rules and SOAP state when required
* Hiding normalized JSON when required

### Extraction Coordination

* Handling extracted flow names
* Handling extracted rules
* Resetting generation when extraction starts
* Preventing duplicate extraction actions
* Triggering extraction for the current selected file

### Generation Coordination

* Building the current generation input
* Preventing duplicate generation actions
* Triggering generation with current workflow values
* Exposing download behavior

### Presentation Actions

* Toggling normalized JSON visibility
* Exposing state and actions needed by the page components

---

## Responsibilities Staying in `page.tsx`

The page should retain rendering-oriented responsibilities only.

These include:

* Page layout
* Header markup
* Flow-name input markup
* Rendering `RequirementExtractionPanel`
* Rendering `RuleBuilderPanel`
* Rendering `GenerationOutputPanel`
* Passing workflow state into components
* Passing workflow actions into components
* Styling and visual structure

The page should not directly call extraction or generation services.

---

## Proposed Hook Name

The new hook will be:

```text
useApiFactoryWorkflow
```

File:

```text
src/hooks/useApiFactoryWorkflow.ts
```

This name reflects that the hook coordinates the complete page workflow rather than a single service operation.

---

## Proposed Hook API

The final API may be adjusted to match the project, but it should expose a clear grouped contract.

Conceptual example:

```ts
const workflow = useApiFactoryWorkflow();
```

The returned object may contain:

```ts
{
  flowName,
  rules,
  includeSoapSubflow,
  requirementFile,
  showNormalizedJson,

  rawExtracted,
  normalizedExtraction,
  extractionMetadata,
  missingFields,
  extractStatus,
  isExtracting,

  generated,
  generationMetadata,
  isGenerating,

  setFlowName,
  addRule,
  updateRule,
  removeRule,
  handleRequirementFileChange,
  extractRequirementsFromFile,
  generateFlow,
  downloadFlow,
  toggleNormalizedJson,
}
```

The final names should remain readable and consistent with existing component props.

---

## Proposed State Grouping

The hook return value may be grouped into logical sections.

Conceptual example:

```ts
{
  editableState: {
    flowName,
    rules,
    includeSoapSubflow,
  },

  fileState: {
    requirementFile,
    showNormalizedJson,
  },

  extraction: {
    rawExtracted,
    normalizedExtraction,
    extractionMetadata,
    missingFields,
    extractStatus,
    isExtracting,
  },

  generation: {
    generated,
    generationMetadata,
    isGenerating,
  },

  actions: {
    setFlowName,
    addRule,
    updateRule,
    removeRule,
    handleRequirementFileChange,
    extractRequirementsFromFile,
    generateFlow,
    downloadFlow,
    toggleNormalizedJson,
  },
}
```

A flat return structure may be used if it keeps page integration simpler.

The design should avoid unnecessary nesting if it makes component props harder to read.

---

## Planned Type Definitions

The following types may be created in:

```text
src/types/day39.ts
```

Possible contracts include:

* `ApiFactoryWorkflowState`
* `ApiFactoryWorkflowActions`
* `UseApiFactoryWorkflowResult`
* `RuleUpdateInput`
* `RequirementFileChangeHandler`
* `ApiFactoryEditableState`
* `ApiFactoryExtractionState`
* `ApiFactoryGenerationState`

The implementation should reuse existing domain types from:

```text
src/types/day32.ts
src/types/day38.ts
```

Relevant existing types include:

* `BusinessRule`
* `NormalizedExtraction`
* `ExtractionMetadata`
* `GenerationMetadata`

The composition hook should also reuse the existing hook return types where practical.

---

## Default Flow Name Preservation

The current default flow name is:

```text
XCDTS005 Generated Flow
```

Day 39 must preserve this value.

The composition hook should define one reusable constant:

```ts
const DEFAULT_FLOW_NAME = "XCDTS005 Generated Flow";
```

Expected behavior:

* It appears on initial load.
* It remains editable.
* A valid extracted flow name replaces it.
* An empty extracted flow name is ignored.
* File removal restores the default value.
* Generation uses the latest current value.

---

## Editable Rule Preservation

Business rules must remain fully editable after extraction.

The composition hook must support:

* Adding a rule
* Updating a rule field
* Removing a rule
* Retaining rule order
* Replacing rules after extraction
* Using current rules during generation
* Keeping rules after generation failures
* Clearing rules during file removal according to current behavior

Rule editing must not be moved into the extraction or generation hooks.

---

## SOAP Subflow Synchronization

SOAP subflow state is derived from and affected by the editable rule list.

The composition hook must preserve current behavior.

### Adding a SOAP Rule

When the user adds:

```text
SOAP-SUBFLOW
```

the hook must set:

```ts
includeSoapSubflow = true;
```

### Extracted Rules

When extracted rules contain `SOAP-SUBFLOW`, the hook must enable SOAP subflow generation.

When extracted rules do not contain it, the hook must disable SOAP subflow generation.

### Removing Rules

When the last `SOAP-SUBFLOW` rule is removed, the hook must set:

```ts
includeSoapSubflow = false;
```

### Generation

The current SOAP subflow state must be passed unchanged into `useFlowGeneration`.

---

## Requirement File State

The composition hook will own:

```ts
requirementFile: File | null
```

Expected behavior:

* Selecting a file stores the current file.
* File selection resets stale extraction output.
* File selection resets stale generation output.
* File selection hides normalized JSON.
* Selecting a file does not automatically run extraction.
* Removing the file restores the clean page state.
* Extraction uses the current selected file.

---

## File Change Behavior

When a requirement file changes, the composition hook must:

1. Store the new file.
2. Call `resetExtraction`.
3. Call `resetGeneration`.
4. Hide normalized JSON.
5. Preserve or reset editable state according to current behavior.

The current implementation clears flow name, rules, and SOAP state only when the file is removed.

Day 39 must preserve that behavior unless the actual page currently behaves differently.

---

## File Removal Behavior

When the file becomes `null`, the composition hook must:

* Reset extraction state
* Reset generation state
* Hide normalized JSON
* Restore the default flow name
* Clear rules
* Disable SOAP subflow
* Leave the page ready for another file

No stale extraction or generation output should remain visible.

---

## Extraction Callback Composition

The composition hook must provide stable callbacks to `useExtractionWorkflow`.

### Flow Name Callback

The hook should:

1. Receive the extracted flow name.
2. Trim whitespace.
3. Ignore empty values.
4. Update editable flow-name state.

### Rule Callback

The hook should:

1. Receive mapped extracted rules.
2. Replace the current editable rules.
3. Recalculate SOAP subflow state.

### Extraction Start Callback

The hook should:

1. Hide normalized JSON.
2. Reset generation state.
3. Preserve current extraction behavior.

These callbacks should use `useCallback`.

---

## Extraction Trigger Behavior

The composition hook should expose an action similar to:

```ts
extractRequirementsFromFile
```

Expected behavior:

* If no file is selected, do nothing.
* If extraction is already active, do nothing.
* Otherwise, pass the current file to `extractFromFile`.
* Preserve the existing extraction status behavior.
* Do not call the service directly.

The composition hook should not duplicate logic already inside `useExtractionWorkflow`.

---

## Generation Trigger Behavior

The composition hook should expose an action similar to:

```ts
generateFlow
```

Expected behavior:

* If generation is already active, do nothing.
* Otherwise, call the generation hook with:

  * Current flow name
  * Current SOAP subflow state
  * Current normalized extraction
  * Current rules
* Do not build the payload directly.
* Do not call the service directly.
* Do not format output directly.

---

## Download Behavior

The composition hook should expose the generation hook’s download action.

Expected behavior:

* Empty output produces no download.
* Latest generated output is downloaded.
* Download does not change workflow state.
* Download does not trigger generation.
* Page code does not import the download helper.

---

## Normalized JSON Visibility

The composition hook will own:

```ts
showNormalizedJson
```

It should expose an action similar to:

```ts
toggleNormalizedJson
```

Expected behavior:

* The user can show normalized JSON.
* The user can hide normalized JSON.
* New extraction attempts hide the previous view.
* File changes hide the previous view.
* File removal hides the previous view.
* The extraction hook remains unaware of this presentation state.

---

## Hook Composition Order

The composition hook will likely initialize `useFlowGeneration` before `useExtractionWorkflow`.

This allows the extraction-start callback to call:

```ts
resetGeneration();
```

Conceptual order:

```ts
const generation = useFlowGeneration();

const extraction = useExtractionWorkflow({
  fallbackRules: rules,
  onFlowNameExtracted,
  onRulesExtracted,
  onExtractionStart: generation.resetGeneration,
});
```

The final dependency structure must follow React hook rules and avoid conditional hook calls.

---

## Callback Stability

The composition hook must use stable callbacks where they are passed into child hooks.

Expected callbacks include:

* `handleFlowNameExtracted`
* `handleRulesExtracted`
* `handleExtractionStart`
* `handleRequirementFileChange`
* `extractRequirementsFromFile`
* `handleGenerateFlow`
* `toggleNormalizedJson`

Stable callbacks reduce unnecessary hook action recreation and help avoid dependency warnings.

---

## State Ownership After Day 39

### `useExtractionWorkflow`

Owns:

* Raw extraction
* Normalized extraction
* Extraction metadata
* Missing fields
* Extraction status
* Extraction loading state
* Extraction service coordination
* Extraction response mapping

### `useFlowGeneration`

Owns:

* Generated output
* Generation metadata
* Generation loading state
* Generation service coordination
* Success formatting
* Error formatting
* Download behavior

### `useApiFactoryWorkflow`

Owns:

* Editable flow name
* Editable rules
* SOAP subflow state
* Requirement-file state
* Normalized JSON visibility
* Hook composition
* Rule actions
* File actions
* Extraction trigger
* Generation trigger
* Cross-workflow reset coordination

### `page.tsx`

Owns:

* JSX
* Page layout
* Component placement
* Styling
* Prop wiring

---

## Page Integration Plan

After the composition hook is implemented, `src/app/page.tsx` should:

1. Import `useApiFactoryWorkflow`.
2. Remove direct imports of `useExtractionWorkflow`.
3. Remove direct imports of `useFlowGeneration`.
4. Remove all `useState` declarations.
5. Remove all `useCallback` declarations.
6. Remove rule-management functions.
7. Remove file-change functions.
8. Remove extraction-trigger functions.
9. Remove generation-trigger functions.
10. Remove reset-coordination logic.
11. Read all workflow values from the composition hook.
12. Pass those values and actions into the existing components.

---

## Expected Simplified Page Shape

Conceptual structure:

```tsx
"use client";

import RequirementExtractionPanel from "../components/day32/RequirementExtractionPanel";
import RuleBuilderPanel from "../components/day32/RuleBuilderPanel";
import GenerationOutputPanel from "../components/day32/GenerationOutputPanel";
import { useApiFactoryWorkflow } from "../hooks/useApiFactoryWorkflow";

export default function HomePage() {
  const workflow = useApiFactoryWorkflow();

  return (
    <main>
      <RequirementExtractionPanel
        extractStatus={workflow.extractStatus}
        missingFields={workflow.missingFields}
        normalizedExtraction={workflow.normalizedExtraction}
        rawExtracted={workflow.rawExtracted}
        extractionMetadata={workflow.extractionMetadata}
        showNormalizedJson={workflow.showNormalizedJson}
        onFileChange={workflow.handleRequirementFileChange}
        onExtractRequirements={workflow.extractRequirementsFromFile}
        onToggleNormalizedJson={workflow.toggleNormalizedJson}
      />

      <input
        value={workflow.flowName}
        onChange={(event) =>
          workflow.setFlowName(event.target.value)
        }
      />

      <RuleBuilderPanel
        rules={workflow.rules}
        missingFields={workflow.missingFields}
        onAddRule={workflow.addRule}
        onUpdateRule={workflow.updateRule}
        onRemoveRule={workflow.removeRule}
      />

      <GenerationOutputPanel
        generated={workflow.generated}
        generationMetadata={workflow.generationMetadata}
        onGenerateFlow={workflow.generateFlow}
        onDownloadFlow={workflow.downloadFlow}
      />
    </main>
  );
}
```

The exact layout and existing styling must remain unchanged.

---

## Imports Expected to Leave `page.tsx`

After Day 39, the page should no longer need:

```ts
useState
useCallback
useExtractionWorkflow
useFlowGeneration
BusinessRule
```

The page should only import:

* Existing UI components
* `useApiFactoryWorkflow`

Other imports may remain only if directly needed for rendering.

---

## Behavioral Constraints

Day 39 must not change:

* The default flow name
* Extraction file reading
* Extraction request shape
* `useLLM: false`
* Extraction response mapping
* Missing-field behavior
* Extracted flow-name behavior
* Rule mapping behavior
* Manual rule editing
* SOAP subflow synchronization
* Generation request shape
* Generation service behavior
* Success output formatting
* Error output formatting
* Metadata handling
* Download behavior
* File-change reset behavior
* File-removal reset behavior
* Normalized JSON toggle behavior
* Existing component props
* Existing page labels
* Existing page layout
* Existing styling

Day 39 is an orchestration refactor only.

---

## Error and Recovery Requirements

The composed workflow must preserve independent failure handling.

### Extraction Failure

* Extraction status shows the existing error.
* Generation state remains reset if extraction had started.
* Editable flow name and rules remain available according to current behavior.
* A valid retry can succeed.

### Generation Failure

* Formatted generation error appears.
* Extraction state remains unchanged.
* Editable flow name remains available.
* Editable rules remain available.
* A valid retry can succeed.

The composition hook must not create one shared error state for both workflows.

---

## Repeated Workflow Requirements

The user must be able to:

* Select a file
* Extract requirements
* Edit the flow name
* Add rules
* Update rules
* Remove rules
* Generate a flow
* Download the flow
* Edit values again
* Generate again
* Select another file
* Extract again
* Recover from extraction failure
* Recover from generation failure

The composition hook must always use the latest current state.

---

## Planned Files

### New Files

```text
docs/day39/page_workflow_composition_plan.md
src/types/day39.ts
src/hooks/useApiFactoryWorkflow.ts
docs/day39/page_workflow_composition_test_notes.md
docs/day39/testing_checklist.md
docs/day39/day39_completion_report.md
```

### Existing File to Update

```text
src/app/page.tsx
```

### Existing Hooks to Reuse

```text
src/hooks/useExtractionWorkflow.ts
src/hooks/useFlowGeneration.ts
```

### Existing Types to Reuse

```text
src/types/day32.ts
src/types/day38.ts
```

---

## Implementation Phases

### Phase 1 — Workflow Composition Plan

Create this planning document.

### Phase 2 — Page Workflow Types

Create the composition-hook state, action, and return contracts.

### Phase 3 — API Factory Workflow Hook

Create `useApiFactoryWorkflow.ts` and compose the extraction and generation hooks.

### Phase 4 — Simplify `page.tsx`

Replace remaining page workflow logic with the composition hook.

### Phase 5 — Workflow Test Notes

Document extraction, generation, reset, editing, failure, and recovery behavior.

### Phase 6 — Testing Checklist

Create a complete static, browser, integration, state-ownership, and regression checklist.

### Phase 7 — Completion Report

Summarize the implementation, validation, known warnings, and final status.

---

## Validation Strategy

The Day 39 refactor will be validated through static, build, browser, and integration testing.

### Static Validation

* TypeScript compilation
* Hook return-type validation
* Callback parameter validation
* React hook dependency validation
* Import cleanup
* ESLint validation

### Extraction Validation

* Initial state
* File selection
* Extraction start
* Successful extraction
* Missing fields
* Flow-name population
* Rule population
* Extraction failure
* Retry after failure
* Re-extraction

### Generation Validation

* Successful generation
* Generation metadata
* Loading state
* Generation failure
* Retry after failure
* Repeated generation
* Download behavior

### Composition Validation

* File change resets both workflows
* File removal restores clean state
* New extraction clears generated output
* Rule edits affect generation
* SOAP state stays synchronized
* Flow-name edits affect generation
* Normalized JSON visibility resets correctly
* Latest workflow values are always used

---

## Suggested Commands

Run:

```powershell
npm run lint
npm run build
npm run dev
```

The known multiple-lockfile warning may still appear during the production build.

The warning is non-blocking when compilation and TypeScript validation succeed.

---

## Regression Risks

The following risks must be reviewed carefully:

* Circular callback dependencies
* Composition hook re-render loops
* Stale rules during extraction
* Stale flow name during generation
* Stale normalized extraction during generation
* SOAP state becoming unsynchronized
* File changes not resetting both hooks
* File removal not restoring the default flow name
* Generated output remaining after new extraction
* Normalized JSON remaining visible after resets
* Duplicate extraction requests
* Duplicate generation requests
* Page props becoming incompatible
* Existing hooks being duplicated instead of composed
* Too much state being moved into one lower-level hook
* Page layout changing accidentally

---

## Success Criteria

Day 39 will be considered successful when:

* `src/types/day39.ts` is created.
* `src/hooks/useApiFactoryWorkflow.ts` is created.
* The composition hook uses `useExtractionWorkflow`.
* The composition hook uses `useFlowGeneration`.
* Editable flow-name state moves out of `page.tsx`.
* Editable rule state moves out of `page.tsx`.
* SOAP subflow state moves out of `page.tsx`.
* Requirement-file state moves out of `page.tsx`.
* Normalized JSON visibility moves out of `page.tsx`.
* Rule actions move out of `page.tsx`.
* File actions move out of `page.tsx`.
* Extraction trigger coordination moves out of `page.tsx`.
* Generation trigger coordination moves out of `page.tsx`.
* Cross-workflow reset coordination moves out of `page.tsx`.
* `page.tsx` becomes primarily presentational.
* Existing extraction behavior is preserved.
* Existing generation behavior is preserved.
* Existing rule editing is preserved.
* SOAP subflow behavior is preserved.
* File-change and removal resets are preserved.
* Lint passes.
* Production build passes.
* Browser validation passes.
* No functional regression is identified.
* Test notes and completion documentation are created.

---

## Expected Outcome

After Day 39:

* `src/app/page.tsx` will be substantially smaller.
* Page workflow coordination will be centralized.
* Extraction and generation hooks will remain focused.
* Editable workflow state will have one clear owner.
* File resets will use one coordinated path.
* SOAP subflow synchronization will be centralized.
* Rule actions will be reusable.
* The complete workflow will be easier to test.
* Another page could reuse the same orchestration logic.
* Existing user-visible behavior will remain unchanged.
