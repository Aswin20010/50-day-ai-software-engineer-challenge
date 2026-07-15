# Day 45 — Migration Analyzer Browser Workflow Plan

## 1. Overview

Day 45 introduces the browser-based workflow for the API Migration Analyzer.

The migration-analysis engine developed during Days 41–44 can already inspect source artifacts, evaluate migration rules, identify gaps and risks, generate recommendations, and build migration tasks.

The goal for Day 45 is to expose that functionality through a complete browser experience so that users can configure, execute, and review a migration analysis without using terminal commands or test scripts.

The browser workflow must provide:

* sample source-project selection
* target-architecture selection
* analysis request validation
* execution and loading states
* recoverable error handling
* summary scorecards
* rule-result inspection
* migration-gap inspection
* migration-risk inspection
* recommendation inspection
* migration-task inspection
* supporting-evidence inspection
* responsive behavior across desktop, tablet, and mobile layouts

---

## 2. Day 45 Objective

Build a production-oriented browser workflow that allows a user to:

1. Select a source project.
2. Select a target architecture.
3. Configure the migration-analysis request.
4. Validate the request before execution.
5. Run the migration-analysis orchestrator.
6. View the complete analysis result.
7. Navigate between result categories.
8. Inspect supporting evidence.
9. Reset the workflow and run another analysis.
10. Recover from execution failures without refreshing the page.

---

## 3. User Journey

The intended browser journey is:

```text
Open Migration Analyzer
        ↓
Select Sample Source Project
        ↓
Review Source Project Metadata
        ↓
Select Target Architecture
        ↓
Review Target Architecture Description
        ↓
Configure Analysis Options
        ↓
Validate Request
        ↓
Run Migration Analysis
        ↓
Display Loading State
        ↓
Receive Success or Error Result
        ↓
Review Summary Scorecards
        ↓
Inspect Rules, Gaps, Risks, Recommendations, and Tasks
        ↓
Open Supporting Evidence
        ↓
Reset or Run Another Analysis
```

---

## 4. Browser Route

The Migration Analyzer should be available through a dedicated route.

Recommended route:

```text
/migration-analyzer
```

Recommended page file:

```text
src/app/migration-analyzer/page.tsx
```

The route should render the main browser workspace component.

Example structure:

```tsx
import {
  MigrationAnalyzerWorkspace,
} from "../../components/migration-analyzer/MigrationAnalyzerWorkspace";

export default function MigrationAnalyzerPage() {
  return <MigrationAnalyzerWorkspace />;
}
```

The page component should remain lightweight.

Browser workflow state, execution logic, form handling, and result rendering should be delegated to reusable components and hooks.

---

## 5. Page Structure

The browser page should contain the following main regions:

```text
Migration Analyzer Page
├── Page Header
├── Analysis Configuration Panel
│   ├── Sample Project Selector
│   ├── Source Project Summary
│   ├── Target Architecture Selector
│   ├── Target Architecture Summary
│   ├── Optional Analysis Settings
│   ├── Validation Messages
│   └── Form Actions
├── Execution Status Region
│   ├── Loading State
│   └── Error State
└── Analysis Results Dashboard
    ├── Summary Scorecards
    ├── Section Navigation
    ├── Rule Results
    ├── Migration Gaps
    ├── Migration Risks
    ├── Recommendations
    ├── Migration Tasks
    └── Evidence Viewer
```

---

## 6. Page Header

The page header should clearly communicate the purpose of the workflow.

Recommended content:

```text
API Migration Analyzer

Analyze an existing API project against a target architecture and generate
rule results, migration gaps, risks, recommendations, tasks, and evidence.
```

The header may also display:

* current workflow status
* selected source project
* selected target architecture
* last completed analysis time
* Reset Analysis action

The header should remain concise and should not compete visually with the analysis form or results.

---

## 7. Analysis Configuration Panel

The configuration panel is the starting point of the browser workflow.

It should collect all information required to create a valid `MigrationAnalysisRequest`.

The configuration panel should contain:

* sample project selector
* target architecture selector
* optional analysis settings
* validation feedback
* Run Migration Analysis button
* Reset button

The form must be accessible through keyboard navigation and must provide visible labels for every field.

---

## 8. Sample Source-Project Selection

The browser workflow should initially support sample projects stored inside the application.

Each sample-project option should provide enough information for the user to understand what is being analyzed.

Recommended sample-project option structure:

```ts
export interface MigrationSampleProjectOption {
  readonly id: string;
  readonly name: string;
  readonly description: string;
  readonly language: string;
  readonly framework?: string;
  readonly artifactCount: number;
  readonly artifacts: readonly SourceArtifact[];
}
```

Each option should display:

* project name
* short description
* primary language
* framework, when available
* number of source artifacts

Example selector labels:

```text
Legacy Node.js Express API
Spring Boot Order Service
Go Monolithic REST API
```

When a project is selected, the browser should display a compact project summary.

Example:

```text
Language: TypeScript
Framework: Express
Artifacts: 14
Primary API Style: REST
```

The selected project should supply the `sourceArtifacts` required by the migration-analysis request.

---

## 9. Target Architecture Selection

Target architectures should be loaded from the existing target-architecture registry.

Existing source:

```text
src/data/targetArchitectures.ts
```

Each target-architecture option should display:

* architecture name
* architecture description
* supported runtime or language
* major architectural layers
* expected capabilities

Example:

```text
Go Cloud-Native Layered API

A production-oriented Go API using handler, service, repository,
and external-client layers.
```

The selector value should store the target architecture identifier rather than duplicating the complete architecture object in form state.

Recommended form field:

```ts
targetArchitectureId: string;
```

The selected architecture object can be resolved from the target-architecture collection before analysis execution.

---

## 10. Optional Analysis Settings

The initial Day 45 workflow may include optional analysis settings when supported by the current orchestrator.

Potential settings include:

* include endpoint analysis
* include capability analysis
* include evidence extraction
* include recommendation generation
* include migration-task generation
* strict rule evaluation
* maximum evidence items per rule

These options should only be exposed when they directly map to supported engine behavior.

The browser must not display settings that are ignored by the analysis engine.

A minimal first version may run all available analysis stages automatically.

---

## 11. Form State Model

The browser form should use a dedicated state model.

Recommended type:

```ts
export interface MigrationAnalysisFormState {
  readonly sampleProjectId: string;
  readonly targetArchitectureId: string;
  readonly includeEndpointAnalysis: boolean;
  readonly includeCapabilityAnalysis: boolean;
  readonly includeEvidence: boolean;
}
```

The initial form state should contain safe defaults.

Example:

```ts
export const initialMigrationAnalysisFormState:
  MigrationAnalysisFormState = {
    sampleProjectId: "",
    targetArchitectureId: "",
    includeEndpointAnalysis: true,
    includeCapabilityAnalysis: true,
    includeEvidence: true,
  };
```

The form state should not contain generated analysis results or execution errors.

Those values belong to the workflow execution state.

---

## 12. Request Validation

The request must be validated before the migration-analysis orchestrator is called.

Minimum validation rules:

1. A sample source project must be selected.
2. A target architecture must be selected.
3. The selected source project must exist.
4. The selected source project must contain at least one source artifact.
5. The selected target architecture must exist.
6. Required analysis request fields must be populated.
7. A second submission must not start while an analysis is already running.

Recommended validation-error structure:

```ts
export interface MigrationAnalysisFormErrors {
  readonly sampleProjectId?: string;
  readonly targetArchitectureId?: string;
  readonly general?: string;
}
```

Example messages:

```text
Select a source project before running the analysis.

Select a target architecture before running the analysis.

The selected source project does not contain any analyzable artifacts.

The selected target architecture could not be found.
```

Field-level messages should appear near the relevant control.

General request errors should appear above the form actions.

---

## 13. Analysis Request Construction

After successful validation, the browser workflow should construct a `MigrationAnalysisRequest`.

Conceptual request construction:

```ts
const request: MigrationAnalysisRequest = {
  sourceArtifacts: selectedProject.artifacts,
  targetArchitectureId: selectedArchitecture.id,
};
```

The exact request fields must follow the current `MigrationAnalysisRequest` definition.

The browser should not introduce a second request format when the existing domain type can be reused safely.

Any browser-only values should be converted before the orchestrator is called.

---

## 14. Analysis Execution

The browser workflow should execute the existing migration-analysis entry point.

Expected integration:

```ts
runMigrationAnalysisArchitecture(request);
```

The browser layer should not duplicate migration-analysis logic.

Responsibilities of the browser workflow:

* validate form input
* construct the request
* call the orchestrator
* capture the result
* capture execution failures
* update the UI state

Responsibilities of the migration-analysis engine:

* inspect source artifacts
* extract capabilities
* extract endpoints
* evaluate rules
* generate gaps
* generate risks
* generate recommendations
* generate migration tasks
* return evidence
* calculate summary information

---

## 15. Workflow Execution State

The browser should represent execution using an explicit state model.

Recommended status values:

```ts
export type MigrationAnalysisExecutionStatus =
  | "idle"
  | "validating"
  | "running"
  | "success"
  | "error";
```

Recommended workflow state:

```ts
export interface MigrationAnalysisWorkflowState {
  readonly status: MigrationAnalysisExecutionStatus;
  readonly result: MigrationAnalysisResult | null;
  readonly errorMessage: string | null;
  readonly selectedSection: MigrationAnalysisSection;
  readonly selectedEvidenceId: string | null;
}
```

State meanings:

### `idle`

* no analysis is running
* no result is currently displayed
* form controls are enabled

### `validating`

* form values are being checked
* the orchestrator has not started
* submission controls should remain temporarily disabled

### `running`

* analysis execution is in progress
* duplicate submissions are blocked
* loading state is displayed
* form values should not change until execution completes

### `success`

* a valid analysis result is available
* summary scorecards and result sections are displayed
* the user may inspect evidence or run another analysis

### `error`

* analysis execution failed
* a recoverable error message is displayed
* the previous valid result may either remain visible or be cleared consistently
* the user can modify the form and retry

---

## 16. Custom Workflow Hook

Execution logic should be isolated inside a reusable custom hook.

Recommended file:

```text
src/hooks/useMigrationAnalysisWorkflow.ts
```

Recommended responsibilities:

* own form state
* own form validation errors
* resolve selected project
* resolve selected target architecture
* construct the analysis request
* execute the orchestrator
* maintain execution status
* store the analysis result
* store recoverable error details
* manage active result section
* manage selected evidence
* reset the workflow

Recommended hook interface:

```ts
export interface UseMigrationAnalysisWorkflowResult {
  readonly formState: MigrationAnalysisFormState;
  readonly formErrors: MigrationAnalysisFormErrors;
  readonly status: MigrationAnalysisExecutionStatus;
  readonly result: MigrationAnalysisResult | null;
  readonly errorMessage: string | null;
  readonly selectedSection: MigrationAnalysisSection;
  readonly selectedEvidenceId: string | null;
  readonly isRunning: boolean;

  readonly updateFormField: <
    Key extends keyof MigrationAnalysisFormState,
  >(
    field: Key,
    value: MigrationAnalysisFormState[Key],
  ) => void;

  readonly runAnalysis: () => Promise<void>;
  readonly resetAnalysis: () => void;
  readonly selectSection: (
    section: MigrationAnalysisSection,
  ) => void;
  readonly selectEvidence: (
    evidenceId: string | null,
  ) => void;
}
```

The hook should expose UI-ready values and actions without returning component-specific markup.

---

## 17. Main Workspace Component

Recommended file:

```text
src/components/migration-analyzer/MigrationAnalyzerWorkspace.tsx
```

The workspace component should coordinate the complete browser experience.

Responsibilities:

* call `useMigrationAnalysisWorkflow`
* render the page header
* render the request form
* render execution states
* render the results dashboard
* pass selected result and callbacks to child components
* maintain a consistent page layout

The workspace should not contain detailed rendering logic for every result category.

Each major result category should have a dedicated component.

---

## 18. Recommended Component Boundaries

Recommended component structure:

```text
src/components/migration-analyzer/
├── MigrationAnalyzerWorkspace.tsx
├── MigrationAnalysisForm.tsx
├── MigrationAnalysisLoading.tsx
├── MigrationAnalysisError.tsx
├── MigrationAnalysisResults.tsx
├── MigrationSummaryScorecards.tsx
├── MigrationAnalysisNavigation.tsx
├── MigrationRuleResults.tsx
├── MigrationGapsPanel.tsx
├── MigrationRisksPanel.tsx
├── MigrationRecommendationsPanel.tsx
├── MigrationTasksPanel.tsx
└── MigrationEvidenceViewer.tsx
```

### `MigrationAnalyzerWorkspace`

Coordinates the complete workflow.

### `MigrationAnalysisForm`

Renders selectors, validation messages, and form actions.

### `MigrationAnalysisLoading`

Displays the current execution state.

### `MigrationAnalysisError`

Displays recoverable execution failures.

### `MigrationAnalysisResults`

Coordinates scorecards, section navigation, and active result content.

### `MigrationSummaryScorecards`

Displays high-level analysis metrics.

### `MigrationAnalysisNavigation`

Allows switching between result categories.

### `MigrationRuleResults`

Displays rule evaluation results.

### `MigrationGapsPanel`

Displays identified migration gaps.

### `MigrationRisksPanel`

Displays migration risks and severity.

### `MigrationRecommendationsPanel`

Displays generated recommendations.

### `MigrationTasksPanel`

Displays actionable migration tasks.

### `MigrationEvidenceViewer`

Displays supporting source evidence.

---

## 19. Loading State

The loading state should communicate that the analysis is actively running.

Recommended content:

```text
Running migration analysis...

Inspecting source artifacts, evaluating architecture rules,
and generating migration guidance.
```

The loading state should:

* display a visible progress indicator
* disable the Run Migration Analysis button
* disable form selectors
* prevent duplicate submissions
* remain accessible to screen readers
* avoid displaying fake completion percentages

Optional stage labels may be displayed only when the orchestrator exposes meaningful progress information.

Example stage labels:

```text
Inspecting source artifacts
Extracting endpoints and capabilities
Evaluating architecture rules
Generating migration guidance
```

Without real progress events, the browser should use a single indeterminate loading state.

---

## 20. Error State

The error state must be recoverable.

It should include:

* a clear title
* a user-friendly message
* a retry action
* an option to modify the request
* technical details only when useful for development

Recommended title:

```text
Migration analysis could not be completed
```

Recommended fallback message:

```text
An unexpected error occurred while analyzing the selected project.
Review the request and run the analysis again.
```

The browser should avoid exposing raw stack traces directly in the main interface.

Errors may still be logged to the browser console during development.

---

## 21. Results Dashboard

After successful execution, the page should display the analysis results in a structured dashboard.

Recommended hierarchy:

```text
Analysis Results
├── Analysis Context
├── Summary Scorecards
├── Section Navigation
└── Active Result Section
```

Analysis context should display:

* selected source project
* selected target architecture
* analyzed artifact count
* analysis completion status

---

## 22. Summary Scorecards

The summary region should provide a fast assessment of migration readiness.

Recommended scorecards:

* overall migration score
* readiness classification
* source artifact count
* passed rules
* failed rules
* migration gaps
* migration risks
* recommendations
* migration tasks

Example layout:

```text
Overall Score       74 / 100
Readiness           Moderate
Artifacts           14
Rules Passed        9
Rules Failed        4
Migration Gaps      6
Risks               3
Recommendations     8
Migration Tasks     11
```

Scorecard values must be derived from the actual `MigrationAnalysisResult`.

The browser should not maintain independent duplicate totals when the engine already provides them.

---

## 23. Readiness Classification

When the analysis result does not already provide a readiness label, the browser may derive one from the overall score.

Suggested display classifications:

```text
90–100  Ready
75–89   Mostly Ready
50–74   Moderate Migration Effort
25–49   Significant Migration Effort
0–24    Major Redesign Required
```

This classification should be treated as a presentation-layer interpretation unless it becomes part of the official analysis domain model.

The exact thresholds should remain centralized and covered by tests.

---

## 24. Result Section Navigation

The dashboard should provide navigation between major result categories.

Recommended section type:

```ts
export type MigrationAnalysisSection =
  | "summary"
  | "rules"
  | "gaps"
  | "risks"
  | "recommendations"
  | "tasks"
  | "evidence";
```

Recommended navigation labels:

```text
Summary
Rule Results
Migration Gaps
Risks
Recommendations
Migration Tasks
Evidence
```

Each navigation item may display an item count.

Example:

```text
Rule Results (13)
Migration Gaps (6)
Risks (3)
Recommendations (8)
Migration Tasks (11)
Evidence (24)
```

Navigation should be implemented using accessible buttons or tabs.

The active section must have a visible selected state.

---

## 25. Rule Results Section

The rule-results section should display every evaluated migration rule.

Each rule result should show:

* rule identifier
* rule title
* rule description
* evaluation status
* severity or importance
* expected architecture behavior
* observed source-project behavior
* supporting evidence count
* related migration gaps
* related recommendations

Recommended statuses:

```text
Passed
Failed
Partially Satisfied
Not Applicable
```

Failed rules should be easy to distinguish without relying only on color.

Selecting a rule should allow the user to view its evidence.

---

## 26. Migration Gaps Section

The migration-gaps section should explain what is missing between the current source project and the target architecture.

Each migration gap should display:

* gap identifier
* title
* description
* category
* severity
* affected artifacts
* related rules
* supporting evidence
* recommended resolution

Potential categories:

```text
Architecture
Security
Observability
Reliability
Testing
Deployment
Data Access
API Design
Documentation
```

When no gaps exist, display a deliberate empty state.

Example:

```text
No migration gaps were identified for the selected architecture.
```

---

## 27. Migration Risks Section

The risks section should communicate migration concerns and their potential impact.

Each risk should display:

* risk identifier
* title
* description
* severity
* probability, when available
* migration impact
* affected artifacts
* related gaps
* mitigation guidance
* supporting evidence

Recommended severity labels:

```text
Critical
High
Medium
Low
```

Risk severity must be displayed through both text and visual styling.

---

## 28. Recommendations Section

The recommendations section should provide clear migration guidance.

Each recommendation should display:

* recommendation identifier
* title
* description
* priority
* rationale
* expected outcome
* related rules
* related gaps
* related risks
* supporting evidence

Recommendations should be written as actionable engineering guidance rather than generic observations.

Example:

```text
Introduce a service layer between HTTP handlers and repository operations
to isolate business rules and improve testability.
```

---

## 29. Migration Tasks Section

The migration-tasks section should convert recommendations into actionable implementation work.

Each task should display:

* task identifier
* title
* description
* priority
* estimated complexity, when available
* dependencies
* acceptance criteria
* related recommendation
* related gap
* related risk
* affected artifacts

Tasks may be grouped by:

* migration phase
* priority
* architecture layer
* dependency order

Recommended migration phases:

```text
Foundation
Architecture Refactor
Security
Observability
Testing
Deployment
Validation
```

---

## 30. Evidence Viewer

The evidence viewer is a critical part of the browser workflow.

It should allow users to verify why the analyzer produced a rule result, gap, risk, recommendation, or task.

Each evidence item should display:

* evidence identifier
* artifact path
* evidence type
* line number or source location, when available
* extracted content
* matched capability or endpoint
* related rule
* related gap
* related risk
* confidence or match information, when available

Recommended evidence categories:

```text
Endpoint
Capability
Configuration
Dependency
Architecture Pattern
Security Control
Testing Pattern
Deployment Pattern
```

The viewer should preserve formatting for code-like evidence.

Example:

```text
Artifact:
src/controllers/orderController.ts

Location:
Lines 18–42

Evidence Type:
Endpoint

Extracted Content:
router.post("/orders", createOrderHandler);
```

Long artifact paths should wrap or truncate safely without causing horizontal page overflow.

---

## 31. Evidence Selection Workflow

Evidence should be selectable from multiple result sections.

Examples:

* selecting a failed rule opens related evidence
* selecting a migration gap opens supporting artifacts
* selecting a risk opens the source evidence
* selecting a recommendation opens the evidence that justified it
* selecting a task opens its related recommendation and evidence

Recommended state:

```ts
selectedEvidenceId: string | null;
```

The evidence viewer may appear as:

* a side panel on desktop
* an inline panel on tablet
* a full-width section on mobile

The selected evidence should be cleared when:

* the workflow is reset
* a new analysis starts
* the selected evidence does not exist in the new result

---

## 32. Empty States

Every result section must support an explicit empty state.

Examples:

### No failed rules

```text
All applicable migration rules passed.
```

### No migration gaps

```text
No migration gaps were identified.
```

### No risks

```text
No migration risks were generated.
```

### No recommendations

```text
No additional migration recommendations are required.
```

### No tasks

```text
No migration tasks were generated.
```

### No evidence

```text
No supporting evidence is available for this result.
```

Empty sections should never appear broken or completely blank.

---

## 33. Reset Behavior

The Reset action should return the browser workflow to its initial state.

Reset should:

* restore initial form values
* clear validation errors
* clear execution errors
* clear the analysis result
* return the execution status to `idle`
* return navigation to the default section
* clear selected evidence

The Reset action should remain disabled while analysis is running unless cancellation is explicitly supported.

Day 45 does not require analysis cancellation.

---

## 34. Running Another Analysis

After a successful result, the user should be able to:

* change the source project
* change the target architecture
* update supported options
* run a new analysis

When a new analysis begins:

* old execution errors should be cleared
* selected evidence should be cleared
* navigation should return to the default result section
* duplicate submission should be prevented
* the previous result should either remain visible with a loading overlay or be cleared consistently

Recommended Day 45 behavior:

```text
Clear the previous result when a new valid analysis begins.
```

This prevents users from confusing old results with the currently running request.

---

## 35. Responsive Layout Strategy

The browser workflow must work across desktop, tablet, and mobile widths.

### Desktop

Recommended layout:

```text
Configuration Panel | Results Dashboard
                    |
                    | Evidence Side Panel
```

Possible behavior:

* configuration panel uses a fixed or limited width
* result dashboard uses the remaining space
* evidence viewer appears as a right-side panel
* scorecards use a multi-column grid

### Tablet

Recommended layout:

```text
Configuration Panel
Results Dashboard
Evidence Panel
```

Possible behavior:

* configuration form becomes full width
* scorecards use two or three columns
* result navigation becomes horizontally scrollable
* evidence appears below active content

### Mobile

Recommended layout:

```text
Page Header
Configuration Form
Execution State
Summary Cards
Section Navigation
Active Section
Evidence Viewer
```

Possible behavior:

* all regions use a single column
* buttons become full width
* scorecards stack vertically or use two columns
* long navigation labels wrap or scroll
* code and evidence blocks use contained horizontal scrolling

---

## 36. Overflow Protection

The UI must safely handle:

* long artifact paths
* long architecture names
* large rule descriptions
* long recommendation text
* long migration-task acceptance criteria
* code evidence
* large collections of results
* unbroken identifiers

Recommended CSS behavior:

```css
overflow-wrap: anywhere;
word-break: break-word;
min-width: 0;
```

Code evidence containers may use:

```css
overflow-x: auto;
white-space: pre;
```

The page itself should not develop unintended horizontal scrolling.

---

## 37. Accessibility Requirements

The Day 45 browser workflow should meet basic accessibility expectations.

Requirements:

* every form field has a visible label
* validation messages are associated with their fields
* buttons use descriptive text
* loading state uses an accessible live region
* error state is announced appropriately
* section navigation is keyboard accessible
* active navigation state is programmatically identifiable
* result severity is communicated through text
* focus remains visible
* evidence content can be accessed without a mouse
* disabled controls are visibly distinct

Color must not be the only method used to communicate:

* passed or failed status
* risk severity
* selected navigation state
* validation state

---

## 38. Browser Integration Strategy

The browser workflow should reuse existing domain types and engine functions wherever possible.

Preferred dependency direction:

```text
Browser Components
        ↓
Browser Workflow Hook
        ↓
Migration Analysis Orchestrator
        ↓
Domain Services and Helpers
```

The migration engine should not import React, browser components, or UI-specific state.

The browser layer may import:

* `MigrationAnalysisRequest`
* `MigrationAnalysisResult`
* `SourceArtifact`
* `TargetArchitecture`
* `targetArchitectures`
* `runMigrationAnalysisArchitecture`

The domain layer should remain usable in:

* unit tests
* command-line scripts
* future API routes
* browser workflows

---

## 39. Client and Server Boundary

The browser workspace will require client-side interaction.

Components using React state, event handlers, or custom hooks should include:

```tsx
"use client";
```

The route page may remain a server component when it only renders the client workspace.

Recommended boundary:

```text
page.tsx
└── MigrationAnalyzerWorkspace.tsx
    └── "use client"
```

The orchestrator must be reviewed for browser compatibility.

Potential browser incompatibilities include:

* Node.js filesystem access
* `path` module usage
* environment-only dependencies
* server-only package imports
* direct process access
* dynamic file reads

Sample source artifacts should be represented as imported application data rather than read from the local filesystem during browser execution.

If the orchestrator depends on Node.js-only functionality, a server API route should be introduced instead of importing the orchestrator directly into the client bundle.

---

## 40. Preferred Execution Approach

Use direct browser execution when:

* the orchestrator is fully browser compatible
* source artifacts are already available as application data
* no server-only dependencies are required
* analysis execution is lightweight

Use a server route when:

* the orchestrator uses Node.js-only modules
* analysis requires filesystem access
* analysis requires protected credentials
* source projects are uploaded dynamically
* analysis is computationally expensive
* engine implementation should remain outside the client bundle

Potential server route:

```text
src/app/api/migration-analysis/route.ts
```

Day 45 should use the simplest approach that preserves correct architecture and passes the production build.

---

## 41. Sample Project Data Strategy

Sample project definitions should be stored separately from React components.

Recommended file:

```text
src/data/migrationSampleProjects.ts
```

Recommended structure:

```ts
export const migrationSampleProjects:
  readonly MigrationSampleProjectOption[] = [
    {
      id: "legacy-express-orders-api",
      name: "Legacy Express Orders API",
      description:
        "A controller-heavy Express API with direct data access.",
      language: "TypeScript",
      framework: "Express",
      artifactCount: 4,
      artifacts: [
        // SourceArtifact entries
      ],
    },
  ];
```

The `artifactCount` should preferably be derived from `artifacts.length` rather than maintained manually.

---

## 42. Suggested Browser Types

Recommended file:

```text
src/types/migrationAnalyzerBrowser.ts
```

Potential types:

```ts
export type MigrationAnalysisExecutionStatus =
  | "idle"
  | "validating"
  | "running"
  | "success"
  | "error";

export type MigrationAnalysisSection =
  | "summary"
  | "rules"
  | "gaps"
  | "risks"
  | "recommendations"
  | "tasks"
  | "evidence";

export interface MigrationAnalysisFormState {
  readonly sampleProjectId: string;
  readonly targetArchitectureId: string;
  readonly includeEndpointAnalysis: boolean;
  readonly includeCapabilityAnalysis: boolean;
  readonly includeEvidence: boolean;
}

export interface MigrationAnalysisFormErrors {
  readonly sampleProjectId?: string;
  readonly targetArchitectureId?: string;
  readonly general?: string;
}

export interface MigrationSampleProjectOption {
  readonly id: string;
  readonly name: string;
  readonly description: string;
  readonly language: string;
  readonly framework?: string;
  readonly artifacts: readonly SourceArtifact[];
}

export interface MigrationSummaryScorecard {
  readonly id: string;
  readonly label: string;
  readonly value: string | number;
  readonly description?: string;
}
```

These types should remain browser-focused and should not replace existing migration-domain types.

---

## 43. Result View Models

Components should avoid repeatedly calculating totals and labels inside JSX.

A small browser view-model layer may transform the analysis result into display-ready data.

Potential helper file:

```text
src/lib/migration-analyzer/buildMigrationAnalysisViewModel.ts
```

Potential view model:

```ts
export interface MigrationAnalysisViewModel {
  readonly scorecards:
    readonly MigrationSummaryScorecard[];
  readonly readinessLabel: string;
  readonly ruleCount: number;
  readonly passedRuleCount: number;
  readonly failedRuleCount: number;
  readonly gapCount: number;
  readonly riskCount: number;
  readonly recommendationCount: number;
  readonly taskCount: number;
  readonly evidenceCount: number;
}
```

View-model helpers should be pure functions and covered by unit tests.

---

## 44. Styling Strategy

The Day 45 browser workflow should follow the existing project styling approach.

Possible options:

* existing global CSS
* CSS modules
* Tailwind CSS, when already configured
* reusable design-system components already present in the project

Do not introduce a second styling framework solely for Day 45.

Recommended visual hierarchy:

* clear page title
* bordered or elevated configuration panel
* compact summary cards
* prominent status labels
* readable result sections
* consistent spacing
* restrained use of severity colors
* clear selected states

---

## 45. Validation Strategy

Day 45 validation should include:

### Unit Validation

Test pure logic such as:

* form validation
* source-project resolution
* target-architecture resolution
* readiness classification
* scorecard generation
* result counts
* evidence selection

### Component Validation

Validate:

* form renders expected options
* validation messages appear
* Run button disables while running
* error state renders
* result sections render
* empty states render
* navigation changes active content
* evidence selection updates the viewer

### Browser Validation

Manually validate:

* initial page load
* valid analysis execution
* invalid form submission
* loading state
* error recovery
* result navigation
* evidence inspection
* Reset action
* rerunning analysis
* desktop layout
* tablet layout
* mobile layout

### Production Validation

Run:

```powershell
npm run lint
npm run test
npm run build
npm run dev
```

All commands should complete successfully before Day 45 is marked complete.

---

## 46. Error Scenarios to Validate

The browser workflow should be validated against the following scenarios:

1. No source project selected.
2. No target architecture selected.
3. Selected project identifier is invalid.
4. Selected architecture identifier is invalid.
5. Selected project contains no artifacts.
6. Orchestrator throws an error.
7. Orchestrator returns an incomplete result.
8. Result contains no failed rules.
9. Result contains no gaps.
10. Result contains no risks.
11. Result contains no recommendations.
12. Result contains no tasks.
13. Result contains no evidence.
14. User clicks Run multiple times.
15. User resets after a successful analysis.
16. User runs a second analysis with different selections.
17. Evidence references a missing artifact.
18. Large evidence content exceeds the available width.

---

## 47. Performance Considerations

The Day 45 workflow should avoid unnecessary recalculation and rendering.

Recommended practices:

* memoize derived scorecards when useful
* avoid storing values that can be derived safely
* keep sample-project data outside component bodies
* use stable keys for result collections
* avoid recreating large source-artifact arrays during every render
* render large result collections predictably
* defer advanced virtualization unless result size requires it

Day 45 does not require premature optimization.

Correctness, clarity, and browser stability have higher priority.

---

## 48. Out-of-Scope Items

The following items are outside the initial Day 45 scope unless the current project already supports them:

* uploading arbitrary repositories
* cloning Git repositories
* editing source code in the browser
* automatically applying migration changes
* exporting reports to PDF
* authentication
* multi-user collaboration
* persistent analysis history
* real-time progress streaming
* analysis cancellation
* cloud deployment
* external database storage

These may be considered in future roadmap days.

---

## 49. Proposed Day 45 Implementation Order

Recommended implementation sequence:

### Step 1

Create browser-specific types.

```text
src/types/migrationAnalyzerBrowser.ts
```

### Step 2

Create sample-project data.

```text
src/data/migrationSampleProjects.ts
```

### Step 3

Create form validation and view-model helpers.

```text
src/lib/migration-analyzer/
```

### Step 4

Create the migration-analysis workflow hook.

```text
src/hooks/useMigrationAnalysisWorkflow.ts
```

### Step 5

Create the analysis request form.

```text
MigrationAnalysisForm.tsx
```

### Step 6

Create loading and error states.

```text
MigrationAnalysisLoading.tsx
MigrationAnalysisError.tsx
```

### Step 7

Create summary scorecards and result navigation.

```text
MigrationSummaryScorecards.tsx
MigrationAnalysisNavigation.tsx
```

### Step 8

Create each result panel.

```text
MigrationRuleResults.tsx
MigrationGapsPanel.tsx
MigrationRisksPanel.tsx
MigrationRecommendationsPanel.tsx
MigrationTasksPanel.tsx
```

### Step 9

Create the evidence viewer.

```text
MigrationEvidenceViewer.tsx
```

### Step 10

Create the main workspace and route.

```text
MigrationAnalyzerWorkspace.tsx
src/app/migration-analyzer/page.tsx
```

### Step 11

Add responsive styling.

### Step 12

Run automated and browser validation.

---

## 50. Expected Day 45 File Structure

```text
api-migration-analyzer/
├── docs/
│   └── day45/
│       ├── migration_analyzer_browser_workflow_plan.md
│       ├── browser_validation_checklist.md
│       └── day45_completion_report.md
│
├── src/
│   ├── app/
│   │   └── migration-analyzer/
│   │       └── page.tsx
│   │
│   ├── components/
│   │   └── migration-analyzer/
│   │       ├── MigrationAnalyzerWorkspace.tsx
│   │       ├── MigrationAnalysisForm.tsx
│   │       ├── MigrationAnalysisLoading.tsx
│   │       ├── MigrationAnalysisError.tsx
│   │       ├── MigrationAnalysisResults.tsx
│   │       ├── MigrationSummaryScorecards.tsx
│   │       ├── MigrationAnalysisNavigation.tsx
│   │       ├── MigrationRuleResults.tsx
│   │       ├── MigrationGapsPanel.tsx
│   │       ├── MigrationRisksPanel.tsx
│   │       ├── MigrationRecommendationsPanel.tsx
│   │       ├── MigrationTasksPanel.tsx
│   │       └── MigrationEvidenceViewer.tsx
│   │
│   ├── data/
│   │   ├── migrationSampleProjects.ts
│   │   └── targetArchitectures.ts
│   │
│   ├── hooks/
│   │   └── useMigrationAnalysisWorkflow.ts
│   │
│   ├── lib/
│   │   └── migration-analyzer/
│   │       ├── buildMigrationAnalysisRequest.ts
│   │       ├── buildMigrationAnalysisViewModel.ts
│   │       ├── classifyMigrationReadiness.ts
│   │       └── validateMigrationAnalysisForm.ts
│   │
│   └── types/
│       └── migrationAnalyzerBrowser.ts
```

The final structure may be adjusted to match the existing repository organization, but responsibilities should remain separated.

---

## 51. Day 45 Completion Criteria

Day 45 is complete when all of the following conditions are satisfied:

* [ ] The `/migration-analyzer` route loads successfully.
* [ ] Sample source projects are available for selection.
* [ ] Target architectures are available for selection.
* [ ] The form blocks invalid submissions.
* [ ] The browser creates a valid migration-analysis request.
* [ ] The migration-analysis orchestrator runs from the browser workflow.
* [ ] Duplicate submissions are prevented.
* [ ] A visible loading state appears during execution.
* [ ] Recoverable errors are displayed clearly.
* [ ] Successful analysis results render without terminal usage.
* [ ] Summary scorecards use real analysis data.
* [ ] Rule results are inspectable.
* [ ] Migration gaps are inspectable.
* [ ] Migration risks are inspectable.
* [ ] Recommendations are inspectable.
* [ ] Migration tasks are inspectable.
* [ ] Supporting evidence is inspectable.
* [ ] Empty result sections display intentional messages.
* [ ] Reset returns the workflow to its initial state.
* [ ] A second analysis can run successfully.
* [ ] The desktop layout is usable.
* [ ] The tablet layout is usable.
* [ ] The mobile layout is usable.
* [ ] Long artifact paths do not break the page.
* [ ] Keyboard navigation works for primary controls.
* [ ] `npm run lint` passes.
* [ ] `npm run test` passes.
* [ ] `npm run build` passes.
* [ ] Manual browser validation is completed.

---

## 52. Definition of Done

Day 45 is considered complete when a user can perform the following workflow entirely through the browser:

```text
Select a sample source project
        ↓
Select a target architecture
        ↓
Run migration analysis
        ↓
Review readiness and scorecards
        ↓
Inspect rule results
        ↓
Inspect migration gaps and risks
        ↓
Review recommendations and migration tasks
        ↓
Open supporting source evidence
        ↓
Reset or run another analysis
```

The browser workflow must remain aligned with the existing migration-analysis domain model and must not duplicate engine logic inside React components.
