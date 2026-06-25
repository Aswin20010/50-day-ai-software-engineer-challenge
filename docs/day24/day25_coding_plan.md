# Day 24 — Phase 4: Day 25 Coding Plan

## Purpose

This document defines the exact coding plan for Day 25 of the 50-Day AI Software Engineer Challenge.

Day 25 is the first implementation day after completing the planning, architecture, workflow schema, prompt strategy, Excel structure, and readiness phases.

The goal of Day 25 is not to build the entire automation system. The goal is to build the first working version of the requirement extraction pipeline.

---

## Day 25 Main Objective

The main objective for Day 25 is:

```text
Build a simple requirement extraction pipeline that reads a markdown API requirement file and produces a structured JSON extraction output.
```

This will be the first working implementation foundation for the larger system.

---

## Why Day 25 Starts With Requirement Extraction

Requirement extraction is the first implementation layer because every later artifact depends on it.

The future pipeline depends on this order:

```text
Raw Requirement
      ↓
Extracted Requirement JSON
      ↓
Workflow JSON
      ↓
Excel Intake Workbook
      ↓
Gap Analysis Report
      ↓
Factory LLM Code Generation
```

If the extraction layer is weak, every later output will also be weak.

Therefore, Day 25 should focus on building a clean, stable, and extendable extraction foundation.

---

# 1. Day 25 Implementation Scope

## In Scope

Day 25 will implement:

* Project implementation folder structure
* Requirement markdown file loader
* Basic rule-based requirement extractor
* Structured extraction JSON schema
* JSON output writer
* One sample test run
* Day 25 notes document

---

## Out of Scope

Day 25 will not implement:

* Full LLM integration
* Excel generation
* Workflow JSON generation
* Gap analysis engine
* Node-RED flow generation
* Go API generation
* UI or frontend
* Database storage
* Multi-file batch processing
* Advanced parsing accuracy
* Automated evaluation scoring

These will be handled in later days.

---

# 2. Recommended Day 25 Folder Structure

Create the following structure:

```text
src/
  requirement_extraction/
    __init__.py
    extractor.py

datasets/
  confluence_requirements/
    REQ-001-feed-discount.md

outputs/
  day25/
    extracted_requirements/

docs/
  day25/
    day25_notes.md
```

---

## Folder Purpose

| Folder/File                             | Purpose                                     |
| --------------------------------------- | ------------------------------------------- |
| `src/requirement_extraction/`           | Holds requirement extraction implementation |
| `extractor.py`                          | Main extraction logic                       |
| `datasets/confluence_requirements/`     | Stores sample requirement markdown files    |
| `outputs/day25/extracted_requirements/` | Stores generated extraction JSON files      |
| `docs/day25/day25_notes.md`             | Documents what was built and tested         |

---

# 3. File 1 — `src/requirement_extraction/__init__.py`

## Purpose

This file marks the `requirement_extraction` folder as a Python package.

## Expected Content

```python
"""
Requirement extraction package.

This package contains the first implementation layer for converting
raw API requirement documents into structured extraction JSON.
"""
```

---

# 4. File 2 — `src/requirement_extraction/extractor.py`

## Purpose

This file will contain the first version of the requirement extraction pipeline.

It should include:

* File loading logic
* Requirement ID detection
* Section extraction logic
* Basic endpoint and method extraction
* Structured output creation
* JSON output writing

---

## Recommended Functions

```text
load_requirement_file(file_path)
extract_requirement_id(file_path, raw_text)
extract_endpoint(raw_text)
extract_method(raw_text)
extract_section_lines(raw_text, keywords)
build_extraction_result(file_path, raw_text)
write_extraction_output(output_path, extraction_result)
main()
```

---

## Function Responsibilities

### `load_requirement_file(file_path)`

Reads the markdown requirement file.

Responsibilities:

* Accept file path
* Check whether the file exists
* Read text content
* Return raw requirement text
* Raise clear error if file is missing

---

### `extract_requirement_id(file_path, raw_text)`

Extracts the requirement ID.

Recommended behavior:

1. Try to find a pattern like `REQ-001` inside the text
2. If not found, derive it from the filename
3. If still not found, return `UNKNOWN_REQUIREMENT`

---

### `extract_endpoint(raw_text)`

Extracts the API endpoint path.

Recommended behavior:

* Search for text patterns that look like `/api/...`
* Return the first matched endpoint
* Return empty string if not found

---

### `extract_method(raw_text)`

Extracts the HTTP method.

Recommended behavior:

* Search for known HTTP methods:

  * `GET`
  * `POST`
  * `PUT`
  * `PATCH`
  * `DELETE`
* Return the method if clearly found near endpoint or method section
* Return empty string if not found

---

### `extract_section_lines(raw_text, keywords)`

Extracts lines related to specific requirement categories.

Example keyword groups:

```python
business_rule_keywords = [
    "business rule",
    "process",
    "logic",
    "rule",
    "calculate",
    "determine",
    "apply"
]
```

This function should be simple for Day 25. It does not need perfect NLP.

---

### `build_extraction_result(file_path, raw_text)`

Builds the structured JSON-compatible dictionary.

The output should follow this shape:

```json
{
  "requirement_id": "",
  "source_file": "",
  "api_name": "",
  "endpoint": "",
  "method": "",
  "description": "",
  "request_headers": [],
  "request_body_fields": [],
  "response_fields": [],
  "business_rules": [],
  "validation_rules": [],
  "downstream_dependencies": [],
  "database_dependencies": [],
  "error_scenarios": [],
  "assumptions": [],
  "open_questions": [],
  "extraction_metadata": {
    "extraction_version": "v1",
    "extraction_method": "rule_based",
    "confidence_level": "initial"
  }
}
```

---

### `write_extraction_output(output_path, extraction_result)`

Writes the extraction result to JSON.

Responsibilities:

* Create parent output folder if missing
* Write formatted JSON
* Use indentation for readability
* Preserve field order where possible

---

### `main()`

Runs the first manual extraction test.

For Day 25, it can use fixed sample paths:

```text
Input:
datasets/confluence_requirements/REQ-001-feed-discount.md

Output:
outputs/day25/extracted_requirements/REQ-001-extraction.json
```

---

# 5. Day 25 Extraction Output Schema

The generated JSON should follow this exact top-level structure:

```json
{
  "requirement_id": "REQ-001",
  "source_file": "datasets/confluence_requirements/REQ-001-feed-discount.md",
  "api_name": "",
  "endpoint": "",
  "method": "",
  "description": "",
  "request_headers": [],
  "request_body_fields": [],
  "response_fields": [],
  "business_rules": [],
  "validation_rules": [],
  "downstream_dependencies": [],
  "database_dependencies": [],
  "error_scenarios": [],
  "assumptions": [],
  "open_questions": [],
  "extraction_metadata": {
    "extraction_version": "v1",
    "extraction_method": "rule_based",
    "confidence_level": "initial"
  }
}
```

---

# 6. Keyword Extraction Strategy

Day 25 should use a simple keyword-based approach.

This is acceptable because the purpose is to create a working foundation, not a perfect extraction engine.

---

## Request Header Keywords

```text
header
headers
requestorinfo
clientid
subclientid
traceid
authorization
apikey
hmac
```

---

## Request Body Keywords

```text
request
request body
payload
input
field
account
transaction
encryptInfo
```

---

## Response Field Keywords

```text
response
response body
result
output
discountDetails
message
code
status
```

---

## Business Rule Keywords

```text
business rule
process
logic
rule
calculate
determine
apply
eligible
discount
cap
classification
```

---

## Validation Rule Keywords

```text
required
mandatory
validate
validation
enum
missing
invalid
must
should
```

---

## Downstream Dependency Keywords

```text
downstream
external
call
service
api
REST
SOAP
GraphQL
PIIDLookup
```

---

## Database Dependency Keywords

```text
database
table
db
select
insert
update
delete
query
repository
row-level lock
```

---

## Error Scenario Keywords

```text
error
failure
exception
timeout
not found
bad request
unauthorized
invalid
missing
```

---

# 7. Open Question Strategy

The extractor should add open questions when important details are missing.

Example open questions:

```text
Endpoint path is not clearly mentioned in the requirement document.
HTTP method is not clearly mentioned in the requirement document.
Request body schema is incomplete or missing.
Response body schema is incomplete or missing.
Error handling behavior is not fully documented.
Downstream dependency details are incomplete or missing.
Database mapping details are incomplete or missing.
```

---

## Rule

The extractor must not invent missing details.

If something is unclear, it should be captured in `open_questions`.

---

# 8. Day 25 Manual Test Plan

After implementation, run the extractor against the sample requirement file.

## Test Input

```text
datasets/confluence_requirements/REQ-001-feed-discount.md
```

## Expected Output

```text
outputs/day25/extracted_requirements/REQ-001-extraction.json
```

---

## Manual Verification Checklist

After running the script, verify:

* [ ] Script runs without error
* [ ] Input markdown file is read successfully
* [ ] Output folder is created automatically
* [ ] JSON output file is created
* [ ] JSON is valid
* [ ] `requirement_id` is populated
* [ ] `source_file` is populated
* [ ] `endpoint` is extracted if present
* [ ] `method` is extracted if present
* [ ] Lists are present even if empty
* [ ] Missing items are added to `open_questions`
* [ ] Metadata is included

---

# 9. Recommended Command to Run

From the project root:

```bash
python src/requirement_extraction/extractor.py
```

Expected result:

```text
Extraction completed successfully.
Output written to outputs/day25/extracted_requirements/REQ-001-extraction.json
```

---

# 10. Day 25 Completion Criteria

Day 25 is complete when:

* [ ] Folder structure is created
* [ ] `extractor.py` is implemented
* [ ] Sample requirement markdown file exists
* [ ] Script runs successfully
* [ ] Extraction JSON is generated
* [ ] Output follows the agreed schema
* [ ] Missing fields are captured as open questions
* [ ] Day 25 notes are updated
* [ ] Changes are committed to GitHub

---

# 11. Git Commit Plan

After Day 25 implementation is complete, use a commit message like:

```text
Add Day 25 requirement extraction foundation
```

Recommended files to commit:

```text
src/requirement_extraction/__init__.py
src/requirement_extraction/extractor.py
datasets/confluence_requirements/REQ-001-feed-discount.md
outputs/day25/extracted_requirements/REQ-001-extraction.json
docs/day25/day25_notes.md
```

---

# 12. Risks and Controls

## Risk 1 — Extractor misses important details

Control:

```text
Keep output transparent and reviewable. Missing fields should be captured in open_questions.
```

---

## Risk 2 — Rule-based extraction is not accurate enough

Control:

```text
Accept this limitation for Day 25. Later days can improve extraction using better parsing or LLM-assisted extraction.
```

---

## Risk 3 — Output schema changes too often

Control:

```text
Keep the Day 25 schema stable and version it using extraction_metadata.extraction_version.
```

---

## Risk 4 — Extractor invents details

Control:

```text
Do not generate unsupported values. Use empty fields and open_questions instead.
```

---

# 13. Day 25 Success Statement

Day 25 will be successful if the project has its first working implementation foundation.

The expected result is a simple but functional pipeline that reads a requirement markdown file and writes a structured JSON extraction output.

This gives the project a real implementation base for workflow generation, Excel generation, gap analysis, and factory LLM code generation in the next stages.

---

# Final Summary

Day 25 begins the coding phase of the 50-Day AI Software Engineer Challenge.

The implementation should stay focused and small:

```text
Read requirement markdown → Extract key requirement details → Write structured JSON
```

This first version does not need to be perfect. It needs to be clean, traceable, extendable, and ready for future improvement.

---
