# Day 6 — Prompt Design Strategy

## Purpose

This document defines the prompt design strategy for the future Requirement Extraction Agent.

The agent will eventually read Confluence-style enterprise API requirements and extract structured implementation details such as endpoint metadata, headers, request fields, response fields, business rules, downstream services, database references, error handling, and mapping logic.

This is a non-coding planning document. Actual implementation will begin from Day 25.

---

## Prompt Design Objective

The prompt should guide the AI model to produce extraction output that is:

1. Structured
2. Traceable
3. Conservative
4. Evaluation-friendly
5. Useful for downstream agents

The prompt must prevent the model from inventing missing technical details.

---

## Core Prompt Principles

| Principle                       | Description                                                     |
| ------------------------------- | --------------------------------------------------------------- |
| Extract, do not invent          | Only extract what is present in the source requirement          |
| Preserve evidence               | Keep source wording or source section references where possible |
| Use consistent schema           | Always return the same output structure                         |
| Mark ambiguity                  | Add unclear details to `open_questions`                         |
| Support downstream use          | Output should help Node-RED, Excel, and gap-analysis agents     |
| Separate facts from assumptions | Never silently assume implementation details                    |

---

## Future Prompt Structure

The future extraction prompt should follow this structure:

```text
You are a Requirement Extraction Agent for enterprise API modernization.

Your task is to extract structured implementation details from the provided Confluence-style requirement document.

Follow these rules:
1. Extract only information explicitly present in the requirement.
2. Do not invent missing fields, systems, tables, or business logic.
3. If a value is missing, use "unknown" or null.
4. If a detail is ambiguous, add it to open_questions.
5. Preserve exact endpoint paths, header names, enum values, table names, and field names.
6. Return only valid JSON using the required schema.
7. Make the output useful for Node-RED flow generation, Excel generation, and gap analysis.

Input:
<REQUIREMENT_DOCUMENT>

Output:
<STRUCTURED_JSON_SCHEMA>
```

---

## Prompt Sections

The future prompt should include the following sections.

---

## 1. Role Definition

The role definition tells the model what it is supposed to act as.

Example:

```text
You are a Requirement Extraction Agent that converts enterprise API requirements into structured JSON for implementation planning and evaluation.
```

Purpose:

1. Keeps the model focused.
2. Reduces generic summarization.
3. Frames the task as extraction, not generation.

---

## 2. Extraction Rules

The prompt should clearly state strict extraction rules.

Example:

```text
Extract only facts directly supported by the requirement document.
Do not infer missing endpoints, tables, fields, headers, or business rules.
If a value is unclear, add it to open_questions.
If a value is absent, use "unknown" or null.
```

Purpose:

1. Reduces hallucination.
2. Improves traceability.
3. Helps evaluation against ground truth.

---

## 3. Required Output Schema

The prompt must include the expected output schema.

The schema should match:

```text
docs/day6/extraction_output_schema.md
```

Top-level structure:

```json
{
  "requirement_id": "",
  "source_document": "",
  "api_metadata": {},
  "endpoint": {},
  "headers": [],
  "request_body": [],
  "response_body": [],
  "business_rules": [],
  "external_services": [],
  "database_references": [],
  "error_handling": [],
  "mapping_logic": [],
  "security_requirements": [],
  "non_functional_notes": [],
  "open_questions": [],
  "extraction_metadata": {}
}
```

Purpose:

1. Ensures stable output.
2. Makes downstream processing easier.
3. Supports automated scoring later.

---

## 4. Evidence Requirement

The prompt should ask the model to include evidence where possible.

Example:

```text
For every extracted field, include source or source_section when possible.
The source should describe the section of the requirement where the detail came from.
```

Purpose:

1. Helps manual review.
2. Supports research traceability.
3. Helps explain extraction decisions.

---

## 5. Ambiguity Handling

The prompt should explicitly define ambiguity handling.

Example:

```text
If the requirement says something is used but does not define whether it is required, add an open question.
If a field name appears in multiple inconsistent forms, preserve the original forms and add an open question.
If a downstream system is mentioned without endpoint details, extract the system name and mark endpoint_or_resource as unknown.
```

Purpose:

1. Avoids hidden assumptions.
2. Produces useful review questions.
3. Improves reliability.

---

## 6. Output Format Constraint

The prompt should require JSON-only output later during implementation.

Example:

```text
Return only valid JSON.
Do not include Markdown.
Do not include explanation outside the JSON.
```

Purpose:

1. Makes output machine-readable.
2. Supports automated evaluation.
3. Prevents parsing problems.

---

## Prompt Template v1

```text
You are a Requirement Extraction Agent for enterprise API modernization.

Your task is to extract structured implementation details from the provided Confluence-style API requirement.

You must follow these rules:

1. Extract only information explicitly present in the requirement.
2. Do not invent endpoints, headers, fields, tables, services, mappings, or business rules.
3. Preserve exact names for endpoints, headers, JSON fields, enum values, table names, and downstream services.
4. If a value is not present, use "unknown" or null.
5. If a detail is ambiguous or incomplete, add it to open_questions.
6. Include source or source_section wherever possible.
7. Return output using the required JSON schema.
8. Return only valid JSON. Do not include Markdown or explanatory text.

Required JSON schema:

{
  "requirement_id": "",
  "source_document": "",
  "api_metadata": {
    "api_name": "",
    "api_domain": "",
    "api_description": "",
    "owner": "",
    "system_context": ""
  },
  "endpoint": {
    "method": "",
    "path": "",
    "version": "",
    "operation_name": "",
    "request_content_type": "",
    "response_content_type": ""
  },
  "headers": [],
  "request_body": [],
  "response_body": [],
  "business_rules": [],
  "external_services": [],
  "database_references": [],
  "error_handling": [],
  "mapping_logic": [],
  "security_requirements": [],
  "non_functional_notes": [],
  "open_questions": [],
  "extraction_metadata": {
    "extraction_version": "v1",
    "confidence_notes": ""
  }
}

Requirement document:

<PASTE_REQUIREMENT_DOCUMENT_HERE>
```

---

## Prompt Template v1 Expected Behavior

The model should:

1. Read the full requirement.
2. Identify explicit technical details.
3. Convert those details into the target JSON schema.
4. Preserve exact technical names.
5. Avoid unsupported assumptions.
6. Add unresolved ambiguity to `open_questions`.
7. Produce JSON only.

---

## Prompt Testing Strategy

Later, from Day 25 onward, this prompt should be tested against:

1. FEED Discount requirement
2. Interrogate Account requirement
3. Customize Fabrics requirement
4. SOAP wrapper requirement
5. Clienteling Create Message requirement

Each result should be compared against a ground truth file.

---

## Prompt Evaluation Criteria

| Criterion            | Question                                            |
| -------------------- | --------------------------------------------------- |
| Completeness         | Did the model extract all required fields?          |
| Accuracy             | Are extracted values correct?                       |
| Traceability         | Are source sections included where possible?        |
| Consistency          | Is the output schema stable across requirements?    |
| Conservativeness     | Did the model avoid hallucinating missing details?  |
| Downstream usability | Can the output support Node-RED and Excel planning? |

---

## Known Prompt Risks

| Risk                                   | Mitigation                                   |
| -------------------------------------- | -------------------------------------------- |
| Model invents missing fields           | Use strict extraction-only instruction       |
| Model summarizes instead of extracting | Provide exact JSON schema                    |
| Model produces invalid JSON            | Use JSON-only output constraint              |
| Model loses header or field casing     | Explicitly preserve exact names              |
| Model ignores ambiguity                | Require `open_questions`                     |
| Model overfits to one example          | Test across multiple enterprise API examples |
