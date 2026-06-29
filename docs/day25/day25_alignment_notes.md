# Day 25 — Alignment Notes

## Purpose

This document explains how the existing API Factory Generator base aligns with the 50-Day AI Software Engineer Challenge roadmap.

Day 25 was originally planned as the first implementation day for building a simple requirement extraction pipeline. However, the current project already contains a working TypeScript and Next.js implementation that supports requirement input, extraction, business-rule configuration, SOAP wrapper support, and Node-RED flow generation.

Because of this, Day 25 has been realigned from starting implementation from scratch to aligning, cleaning, validating, and documenting the existing base.

---

## Original Project Goal

The overall goal of the project is to build an AI-assisted API factory that can transform enterprise API requirements into structured implementation artifacts.

The long-term goal is:

```text
Automate enterprise API development by converting requirements into structured artifacts and generated workflow/code outputs.
```

The project is intended to reduce manual effort in enterprise API development by standardizing how requirements are captured, extracted, validated, transformed, and prepared for implementation.

---

## Original Planned Pipeline

The original roadmap was based on the following pipeline:

```text
Confluence Requirement
        ↓
Requirement Extraction
        ↓
Structured Business Rules
        ↓
Workflow Representation
        ↓
Node-RED Flow Generation
        ↓
Excel Intake and Gap Analysis
        ↓
Factory LLM Code Generation
```

This pipeline remains valid.

The current implementation already covers the early and middle parts of this pipeline.

---

## Original Day 25 Plan

Day 25 was originally planned as:

```text
Read markdown requirement
        ↓
Extract key API requirement details
        ↓
Write structured JSON output
```

The expected Day 25 output was a basic requirement extraction foundation.

The original plan assumed that no implementation base existed yet.

---

## Current Project Reality

The current code base already contains a working implementation foundation.

The existing base supports:

```text
Confluence-style requirement input
        ↓
Rule-based extraction
        ↓
Optional Ollama enhancement
        ↓
Business-rule configuration
        ↓
Node-RED flow generation
        ↓
Generated flow JSON download
```

This means the current base is ahead of the original Day 25 plan.

Instead of creating a new Python or standalone extractor, the project should continue with the existing TypeScript and Next.js implementation.

---

## Current Project Stack

The current implementation uses:

* Next.js
* TypeScript
* React
* Next.js API routes
* Rule-based extraction
* Optional Ollama enhancement
* Business-rule template generation
* Node-RED flow JSON generation
* SOAP subflow template support

This stack is suitable for the planned project because it supports both a user-facing interface and backend artifact generation.

---

## Current Active Implementation Structure

The active implementation is located under:

```text
api-factory-ui/
```

The main implementation files are:

```text
api-factory-ui/
  src/
    app/
      api/
        extract-requirements/
          route.ts
        generate/
          route.ts
      page.tsx
      layout.tsx
      globals.css

    lib/
      business-rule-generator.ts
      business-rule-templates.ts
      node-red-flow-generator.ts
      ollama-extractor.ts
      soap-subflow-template.ts
```

---

## How Current Files Align With the Roadmap

### `src/app/page.tsx`

This file supports the UI layer.

It aligns with the roadmap by allowing users to provide requirement input, review extracted data, generate flow output, and download the generated artifact.

Roadmap alignment:

```text
Requirement Input
        ↓
Human Review
        ↓
Artifact Generation Trigger
```

---

### `src/app/api/extract-requirements/route.ts`

This file supports the requirement extraction layer.

It aligns with the roadmap by taking raw requirement text and converting it into structured information that can be used by the generator.

Roadmap alignment:

```text
Confluence Requirement
        ↓
Requirement Extraction
```

---

### `src/lib/ollama-extractor.ts`

This file supports optional LLM enhancement.

It aligns with the roadmap by allowing AI-assisted extraction, while still allowing the system to work without an LLM dependency.

Roadmap alignment:

```text
Requirement Extraction
        ↓
AI-Assisted Enhancement
```

For Day 25, Ollama should remain optional so the project can run even when the local Ollama server is not available.

---

### `src/lib/business-rule-templates.ts`

This file supports reusable rule templates.

It aligns with the roadmap by converting structured business-rule definitions into reusable executable Node-RED function logic.

Roadmap alignment:

```text
Structured Business Rules
        ↓
Reusable Rule Templates
```

---

### `src/lib/business-rule-generator.ts`

This file supports business-rule generation.

It aligns with the roadmap by converting structured business-rule configuration into generated logic.

Roadmap alignment:

```text
Structured Business Rules
        ↓
Generated Business Rule Logic
```

---

### `src/lib/node-red-flow-generator.ts`

This file supports Node-RED flow generation.

It aligns with the roadmap by converting structured business rules into importable Node-RED flow JSON.

Roadmap alignment:

```text
Workflow Representation
        ↓
Node-RED Flow Generation
```

---

### `src/lib/soap-subflow-template.ts`

This file supports reusable SOAP wrapper generation.

It aligns with the project because many enterprise APIs require REST-to-SOAP orchestration. This file supports the future goal of generating enterprise workflow wrappers.

Roadmap alignment:

```text
Downstream SOAP Dependency
        ↓
Reusable SOAP Subflow Pattern
```

---

### `src/app/api/generate/route.ts`

This file supports artifact generation.

It aligns with the roadmap by exposing a backend API that generates Node-RED flow JSON from the configured business rules.

Roadmap alignment:

```text
Structured Business Rules
        ↓
Node-RED Flow JSON Artifact
```

---

## Current Roadmap Coverage

The current project already covers the following roadmap areas:

| Roadmap Area                | Current Status                       |
| --------------------------- | ------------------------------------ |
| Requirement input           | Available through UI                 |
| Requirement extraction      | Available through API route          |
| LLM enhancement             | Available through Ollama integration |
| Business-rule structure     | Available through template system    |
| Node-RED flow generation    | Available through generator          |
| SOAP wrapper support        | Available through SOAP template      |
| UI review/edit              | Available through `page.tsx`         |
| Generated JSON output       | Available through flow generation    |
| Excel intake generation     | Not implemented yet                  |
| Gap analysis                | Not implemented yet                  |
| Evaluation metrics          | Not implemented yet                  |
| Factory LLM code generation | Not implemented yet                  |

---

## Day 25 Realignment Decision

The Day 25 implementation plan has been updated.

Instead of building a new extractor from scratch, Day 25 now focuses on:

1. Auditing the existing base
2. Cleaning duplicate or old files
3. Moving sample files into a dedicated `samples/` folder
4. Adding metadata to extraction output
5. Adding metadata to generation output
6. Making Ollama optional by default
7. Adding traceability metadata to generated business-rule nodes
8. Creating Day 25 documentation
9. Running local validation

---

## Why This Realignment Is Correct

This realignment is correct because the current base is already aligned with the project’s core research direction.

The original research and engineering goal was not simply to build a text extractor. The actual goal is to build a factory-style system that can convert API requirements into structured implementation artifacts.

The current base already supports that direction.

Therefore, restarting with a new extractor would duplicate effort and slow the project down.

The better approach is to continue from the existing TypeScript and Next.js base and strengthen it.

---

## Day 25 Required Code Alignment

The following updates are required to complete Day 25.

### 1. Add Extraction Metadata

The extraction API should return metadata that identifies the extraction version, method, project, target output, and timestamp.

Expected metadata:

```ts
extractionMetadata: {
  extractionVersion: "day25-v1",
  extractionMethod: source,
  project: "api-factory-generator",
  outputTarget: "node-red-flow",
  llmSafe: true,
  generatedAt: new Date().toISOString()
}
```

---

### 2. Add Generation Metadata

The generation API should return metadata that identifies the generation version, project, output type, and timestamp.

Expected metadata:

```ts
generationMetadata: {
  generationVersion: "day25-v1",
  project: "api-factory-generator",
  outputType: "node-red-flow-json",
  generatedAt: new Date().toISOString()
}
```

---

### 3. Make Ollama Optional

The UI should default to:

```ts
useLLM: false
```

This ensures Day 25 validation does not depend on a local Ollama server.

Ollama remains available as a future enhancement.

---

### 4. Add Traceability to Generated Nodes

Generated business-rule nodes should include:

```js
generatedBy: "api-factory-generator",
day: "Day25"
```

This improves traceability from generated Node-RED logic back to the project milestone.

---

### 5. Organize Sample Files

Sample files should be moved into:

```text
api-factory-ui/
  samples/
    input-xcdts005.json
    generated-flow-xcdts005.json
    rollback.json
```

This separates sample artifacts from active source code.

---

### 6. Archive Duplicate Root-Level Test Files

Old or duplicate root-level test files should be moved to:

```text
archive/day25-root-test-files/
```

This prevents confusion between active app code and earlier test scripts.

---

## Current Implementation Strengths

The existing base has the following strengths:

1. It already uses the correct technology stack for a UI-driven factory tool.
2. It separates UI, API routes, and reusable library logic.
3. It supports both rule-based and LLM-assisted extraction.
4. It supports reusable business-rule templates.
5. It generates Node-RED-compatible JSON.
6. It includes SOAP wrapper support.
7. It can be extended toward Excel generation and gap analysis.
8. It is suitable for demonstration, portfolio, article, and research-paper use.

---

## Current Implementation Gaps

The following gaps still remain:

1. Extraction schema needs to be standardized.
2. Business-rule traceability needs to be made stronger.
3. Generated flow validation needs to be automated.
4. Excel intake generation is not implemented yet.
5. Gap analysis is not implemented yet.
6. Evaluation metrics are not implemented yet.
7. More sample requirement datasets are needed.
8. README and architecture documentation need to be improved.
9. Tests need to be added.
10. Generated Node-RED output needs repeatable validation checks.

---

## Updated Roadmap Position

Original roadmap position:

```text
Day 25 — Start implementation
```

Actual implementation maturity:

```text
Around Day 31 to Day 32
```

This does not mean the roadmap is wrong. It means the project had an existing implementation base that already completed several planned future steps.

The roadmap should now shift from starting implementation to hardening and formalizing the existing base.

---

## Updated Day 26 Direction

Since Day 25 now aligns the existing implementation, Day 26 should focus on:

```text
Day 26 — Standardize Extraction Schema and Traceability
```

Day 26 should define a stable extraction schema that can support:

* Node-RED generation
* Excel intake generation
* Gap analysis
* Factory LLM code generation
* Evaluation metrics

---

## Day 25 Validation Checklist

Day 25 is complete when:

* [ ] Active implementation files are identified
* [ ] Sample files are moved into `samples/`
* [ ] Duplicate root-level files are archived
* [ ] Extraction API includes metadata
* [ ] Generate API includes metadata
* [ ] Ollama is optional by default
* [ ] Generated business-rule nodes include traceability metadata
* [ ] Day 25 docs are added
* [ ] `npm run lint` is executed
* [ ] `npm run build` is executed
* [ ] `npm run dev` starts the app
* [ ] A sample requirement can be extracted
* [ ] A Node-RED flow JSON can be generated
* [ ] Generated flow can be downloaded

---

## Final Alignment Status

The current project is aligned with the original roadmap.

The project is ahead of the original Day 25 plan because it already supports requirement extraction, business-rule generation, SOAP wrapper support, and Node-RED flow generation.

Day 25 is complete once the current base is cleaned, metadata is added, sample files are organized, validation is performed, and documentation is committed.

---

## Final Status

```text
The current project is aligned with the original 50-day roadmap.
The implementation is ahead of the original Day 25 plan.
Day 25 should continue with the existing TypeScript and Next.js base.
Next step: Day 26 — Standardize Extraction Schema and Traceability.
```
