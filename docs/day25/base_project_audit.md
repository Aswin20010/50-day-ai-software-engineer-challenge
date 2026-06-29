# Day 25 — Base Project Audit

## Purpose

This document audits the existing API Factory Generator code base before continuing Day 25 implementation.

Originally, Day 25 was planned as the first coding day for a simple requirement extraction pipeline. However, the current project already contains a working TypeScript and Next.js base that supports requirement extraction, business-rule configuration, SOAP wrapper support, and Node-RED flow generation.

Therefore, Day 25 has been realigned from starting a new implementation to auditing, cleaning, and aligning the existing base.

---

## Day 25 Realignment

### Original Day 25 Plan

The original Day 25 plan was:

```text
Read markdown requirement
        ↓
Extract key requirement details
        ↓
Write structured JSON output
```

### Actual Current Base

The current project already supports a more advanced flow:

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

This means the current code base is ahead of the original Day 25 plan.

---

## Current Project Stack

The current implementation uses:

* Next.js
* TypeScript
* React UI
* Next.js API routes
* Rule-based requirement extraction
* Optional Ollama-based enhancement
* Business-rule templates
* Node-RED flow JSON generation
* SOAP subflow templates

---

## Current Project Structure

The active implementation is under:

```text
api-factory-ui/
```

Relevant active files:

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

## Active Implementation Files

### `src/app/page.tsx`

This file provides the main UI for the API Factory Generator.

It supports:

* Requirement text input
* Requirement file upload
* Extracting requirements from Confluence-style text
* Reviewing extracted configuration
* Editing generated fields
* Generating Node-RED flow JSON
* Downloading generated flow output

---

### `src/app/api/extract-requirements/route.ts`

This API route handles requirement extraction.

It accepts requirement text from the UI and returns structured extraction data.

Expected Day 25 behavior:

* Accept requirement text
* Run rule-based extraction
* Optionally use Ollama enhancement
* Return extracted API factory configuration
* Return suggested Node-RED nodes
* Return missing fields
* Return extraction metadata

---

### `src/app/api/generate/route.ts`

This API route handles Node-RED flow generation.

It accepts structured business-rule configuration and returns a generated Node-RED flow JSON artifact.

Expected Day 25 behavior:

* Accept flow configuration
* Generate Node-RED-compatible JSON
* Return generated flow
* Return generation metadata

---

### `src/lib/business-rule-templates.ts`

This file contains reusable business-rule templates.

These templates generate Node-RED function-node JavaScript for common rule types such as:

* Request payload validation
* Required header validation
* Required body field validation
* Numeric field validation
* Default header value handling
* SOAP request normalization
* Request metadata setup

This file is important because it standardizes generated business-rule logic.

---

### `src/lib/business-rule-generator.ts`

This file acts as the business-rule generation layer.

It converts structured business-rule configuration into executable Node-RED function-node logic using the available templates.

---

### `src/lib/node-red-flow-generator.ts`

This file generates the final Node-RED flow JSON.

It is responsible for:

* Creating the Node-RED tab
* Creating function nodes
* Wiring nodes in order
* Applying generated business-rule logic
* Producing importable Node-RED flow JSON

---

### `src/lib/ollama-extractor.ts`

This file supports optional Ollama-based requirement extraction enhancement.

For Day 25, Ollama should remain optional. The system should still work even if Ollama is not running locally.

Recommended Day 25 setting:

```text
useLLM: false
```

Ollama can be improved later as part of extraction enhancement.

---

### `src/lib/soap-subflow-template.ts`

This file contains reusable SOAP wrapper/subflow template logic.

It supports the long-term project goal of generating enterprise API workflow artifacts that may need to call SOAP downstream services.

---

## Existing Capabilities

The current base already supports the following capabilities:

1. Accepting Confluence-style requirement input
2. Uploading requirement files through the UI
3. Extracting structured requirement details
4. Running rule-based extraction
5. Optionally enhancing extraction with Ollama
6. Populating UI fields from extracted data
7. Defining business rules
8. Generating Node-RED function nodes
9. Supporting reusable business-rule templates
10. Supporting SOAP wrapper patterns
11. Producing Node-RED-compatible JSON
12. Downloading generated flow output

---

## Comparison With Original Roadmap

The planned project pipeline is:

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

The current code base already covers:

| Planned Capability          | Current Status      |
| --------------------------- | ------------------- |
| Requirement input           | Available           |
| Requirement extraction      | Available           |
| UI-based review             | Available           |
| Business-rule generation    | Available           |
| Node-RED flow generation    | Available           |
| SOAP wrapper support        | Available           |
| Generated JSON artifact     | Available           |
| Excel intake generation     | Not yet implemented |
| Gap analysis                | Not yet implemented |
| Evaluation metrics          | Not yet implemented |
| Factory LLM code generation | Not yet implemented |

---

## Implementation Maturity

The original roadmap positioned Day 25 as the start of implementation.

However, based on the existing base, the project is closer to:

```text
Day 31 to Day 32 implementation maturity
```

Realistic completed implementation work:

| Area                          | Status            |
| ----------------------------- | ----------------- |
| UI foundation                 | Mostly complete   |
| Extraction API                | Started           |
| Ollama integration            | Started           |
| Business-rule template system | Mostly complete   |
| Business-rule generator       | Started           |
| Node-RED generator            | Mostly complete   |
| SOAP template support         | Started           |
| Sample input/output           | Available         |
| Documentation                 | Needs improvement |
| Testing and validation        | Needs improvement |

---

## Day 25 Cleanup Decisions

Day 25 will not introduce a separate Python implementation.

The project will continue with the existing TypeScript and Next.js stack.

The following cleanup decisions were made:

1. Keep active implementation under `api-factory-ui/src/`
2. Move sample JSON files into `api-factory-ui/samples/`
3. Archive duplicate root-level test files
4. Add extraction metadata
5. Add generation metadata
6. Make Ollama optional by default
7. Add traceability metadata to generated business-rule nodes
8. Create Day 25 documentation

---

## Sample File Organization

Sample files should be organized as:

```text
api-factory-ui/
  samples/
    input-xcdts005.json
    generated-flow-xcdts005.json
    rollback.json
```

This keeps test artifacts separate from active source code.

---

## Duplicate File Cleanup

The real app code should remain under:

```text
api-factory-ui/src/
```

Any old root-level test files should be archived under:

```text
archive/day25-root-test-files/
```

This prevents confusion between active app code and older test scripts.

---

## Day 25 Required Code Updates

### 1. Add Extraction Metadata

The extraction API response should include:

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

The generate API response should include:

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

This ensures the app works without requiring a local Ollama server during Day 25 validation.

---

### 4. Add Generated Node Traceability

Generated business-rule nodes should include:

```js
generatedBy: "api-factory-generator",
day: "Day25"
```

This improves traceability from generated node back to the project milestone.

---

## Current Strengths

The current base has several strong points:

1. It already uses the correct long-term technology direction.
2. It supports UI-based requirement processing.
3. It already separates API routes and library logic.
4. It supports reusable business-rule templates.
5. It generates Node-RED-compatible JSON.
6. It includes SOAP wrapper support, which is important for enterprise APIs.
7. It can be extended toward Excel intake and gap analysis later.

---

## Current Gaps

The following gaps remain after the audit:

1. Extraction schema needs to be standardized.
2. Requirement-to-business-rule traceability needs improvement.
3. Generated Node-RED output needs validation checks.
4. Excel intake generation is not implemented yet.
5. Gap analysis module is not implemented yet.
6. Evaluation metrics are not implemented yet.
7. More sample requirements are needed.
8. README and architecture documentation need to be improved.
9. Test coverage is not yet available.
10. Generated flow quality needs repeatable validation.

---

## Risk Assessment

| Risk                                           | Impact | Control                                    |
| ---------------------------------------------- | ------ | ------------------------------------------ |
| Ollama dependency may block local testing      | Medium | Make `useLLM` false by default             |
| Duplicate files may confuse active source path | Medium | Archive duplicate root-level files         |
| Generated flow may not be fully validated      | Medium | Add validation checklist and sample output |
| Extraction may produce inconsistent schema     | High   | Standardize schema on Day 26               |
| Missing traceability may weaken research value | High   | Add metadata fields and rule IDs           |

---

## Day 25 Validation Checklist

Day 25 is considered valid when:

* [ ] App starts with `npm run dev`
* [ ] App builds with `npm run build`
* [ ] Lint passes or known lint issues are documented
* [ ] Requirement extraction API returns structured output
* [ ] Extraction response includes metadata
* [ ] Generate API returns Node-RED flow JSON
* [ ] Generate response includes metadata
* [ ] Generated business-rule nodes include traceability metadata
* [ ] Ollama is optional by default
* [ ] Sample files are moved into `samples/`
* [ ] Duplicate root-level files are archived
* [ ] Day 25 documentation is added

---

## Final Audit Result

The existing base is valid and aligned with the original project plan.

The project is not starting from zero on Day 25. It already has a meaningful implementation foundation that supports requirement extraction, business-rule configuration, and Node-RED flow generation.

Day 25 is therefore completed by aligning the existing base, adding metadata, organizing files, making the system easier to validate, and documenting the current foundation.

---

## Final Status

```text
Existing base is valid.
Project direction is correct.
Implementation is ahead of the original Day 25 plan.
Day 25 can be completed through cleanup, metadata, validation, and documentation.
```
