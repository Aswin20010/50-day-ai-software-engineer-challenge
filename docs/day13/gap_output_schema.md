# Day 13 — Phase 4: Gap Output Schema

## Purpose

This document defines the output schema for the Gap Analysis Framework.

The goal of the schema is to store every detected gap in a consistent and structured format.

A gap is any missing, incorrect, inconsistent, unsupported, or mismatched detail found when comparing:

1. Confluence requirement documents
2. Node-RED workflow implementations
3. Excel intake workbooks

The output schema makes the gap analysis result easy to review, measure, export, and improve.

---

## Why a Gap Output Schema Is Needed

Gap analysis can produce many findings.

Without a structured schema, the findings may become difficult to understand or compare.

A schema helps standardize:

* What issue was found
* Where the issue was found
* Which artifact is the source of truth
* Which artifact is incomplete or incorrect
* How serious the issue is
* What should be done to fix it
* Which requirement, workflow step, or Excel sheet is affected

This allows the framework to produce repeatable and measurable results.

---

## Gap Record Definition

Each detected gap should be stored as one gap record.

A gap record represents a single issue found during comparison.

Each record should answer the following questions:

```text
What is the gap?
Where was it found?
What artifact is correct?
What artifact is missing or incorrect?
How serious is the gap?
What should be done to fix it?
```

---

## Core Gap Output Schema

The following JSON structure defines the standard format for one gap record.

```json
{
  "gap_id": "GAP-001",
  "gap_type": "Missing Validation Gap",
  "gap_category_group": "Validation Gap",
  "source_of_truth": "Confluence",
  "compared_source": "Excel",
  "missing_from": "Excel",
  "affected_artifact": "api_endpoints",
  "affected_section": "validation_rules",
  "affected_field": "Division-id",
  "severity": "High",
  "confidence": "High",
  "description": "Division-id numeric validation exists in Confluence but is missing from the Excel api_endpoints sheet.",
  "evidence": {
    "confluence": "Division-id must be numeric.",
    "node_red": "Validate Headers node checks Division-id numeric format.",
    "excel": "No numeric validation is defined for Division-id."
  },
  "recommendation": "Add numeric validation for Division-id to the api_endpoints validation_rules column.",
  "status": "Open"
}
```

---

## Schema Fields

## 1. gap_id

### Definition

A unique identifier for the detected gap.

### Format

```text
GAP-001
GAP-002
GAP-003
```

### Example

```json
"gap_id": "GAP-001"
```

### Purpose

The gap ID helps track each finding during review and correction.

---

## 2. gap_type

### Definition

The specific type of gap detected.

### Allowed Values

```text
Missing Endpoint Gap
Missing Header Gap
Missing Request Field Gap
Missing Response Field Gap
Missing Validation Gap
Missing Enum Validation Gap
Missing Business Rule Gap
Missing Downstream Service Gap
Missing Database Query Gap
Missing Entity Gap
Incorrect Field Mapping Gap
Incorrect Response Mapping Gap
Incorrect Service Mapping Gap
Incorrect Data Type Gap
Incorrect Required or Optional Status Gap
Technology Mismatch Gap
Error Handling Gap
Extra Artifact Gap
Naming Mismatch Gap
Workflow Step Gap
```

### Example

```json
"gap_type": "Missing Header Gap"
```

### Purpose

The gap type classifies the exact issue.

---

## 3. gap_category_group

### Definition

The broader group to which the gap belongs.

### Allowed Values

```text
Contract Gap
Validation Gap
Business Logic Gap
Integration Gap
Consistency Gap
```

### Example

```json
"gap_category_group": "Contract Gap"
```

### Purpose

This field helps group similar gaps for reporting and metrics.

---

## 4. source_of_truth

### Definition

The artifact that contains the correct requirement or expected behavior.

### Allowed Values

```text
Confluence
Node-RED
Excel
Manual Review
```

### Example

```json
"source_of_truth": "Confluence"
```

### Purpose

This field identifies the artifact that should be trusted for correction.

For most cases, Confluence will be the source of truth.

---

## 5. compared_source

### Definition

The artifact being compared against the source of truth.

### Allowed Values

```text
Confluence
Node-RED
Excel
Generated Code
```

### Example

```json
"compared_source": "Excel"
```

### Purpose

This field shows which artifact was checked and found to be missing, incorrect, or inconsistent.

---

## 6. missing_from

### Definition

The artifact where the required information is missing.

### Allowed Values

```text
Confluence
Node-RED
Excel
Generated Code
Not Applicable
```

### Example

```json
"missing_from": "Excel"
```

### Purpose

This field directly identifies where the gap exists.

For incorrect mappings or mismatches, this field can be set to the artifact that needs correction.

---

## 7. affected_artifact

### Definition

The specific file, sheet, workflow node, or artifact affected by the gap.

### Examples

```text
api_endpoints
external_services
business_requirements
endpoint_service_mapping
queries
Node-RED Validate Headers node
Node-RED REST Wrapper node
Confluence API Contract section
```

### Example

```json
"affected_artifact": "api_endpoints"
```

### Purpose

This field tells the reviewer where to make the correction.

---

## 8. affected_section

### Definition

The smaller section, column, node property, or document section affected by the gap.

### Examples

```text
required_headers
request_body
response_body
validation_rules
business_rule_description
service_url
query_text
output_mapping
```

### Example

```json
"affected_section": "validation_rules"
```

### Purpose

This helps locate the issue more precisely.

---

## 9. affected_field

### Definition

The exact field, header, service, entity, query, or rule affected by the gap.

### Examples

```text
Division-id
requestorInfo-clientId
account.entryMode
PIIDLookup
Emp_Disc_Profile
SQL-DISC-001
BR-001
```

### Example

```json
"affected_field": "account.entryMode"
```

### Purpose

This field identifies the exact requirement element that needs attention.

---

## 10. severity

### Definition

The seriousness of the gap.

### Allowed Values

```text
Critical
High
Medium
Low
Informational
```

### Example

```json
"severity": "High"
```

### Purpose

Severity helps prioritize fixes.

The detailed rules for severity are defined in Phase 5.

---

## 11. confidence

### Definition

The confidence level of the gap detection.

### Allowed Values

```text
High
Medium
Low
```

### Example

```json
"confidence": "High"
```

### Purpose

Confidence indicates how certain the framework is that the issue is a real gap.

A low-confidence gap may require manual review.

---

## 12. description

### Definition

A clear explanation of the detected gap.

### Example

```json
"description": "The required header x-macys-apikey is defined in Confluence but missing from the Excel api_endpoints sheet."
```

### Purpose

The description should be understandable to both engineers and reviewers.

---

## 13. evidence

### Definition

A structured object showing what each source contains.

### Example

```json
"evidence": {
  "confluence": "x-macys-apikey is listed as a required header.",
  "node_red": "Header validation node checks x-macys-apikey.",
  "excel": "api_endpoints sheet does not include x-macys-apikey."
}
```

### Purpose

Evidence explains why the gap was detected.

This is important for transparency and manual verification.

---

## 14. recommendation

### Definition

The suggested action required to fix the gap.

### Example

```json
"recommendation": "Add x-macys-apikey to the required_headers field in the api_endpoints sheet."
```

### Purpose

This field makes the gap report actionable.

---

## 15. status

### Definition

The current lifecycle status of the gap.

### Allowed Values

```text
Open
In Review
Resolved
Accepted Risk
False Positive
```

### Example

```json
"status": "Open"
```

### Purpose

This helps track gap correction progress over time.

---

# Optional Fields

Some gap records may include extra fields when needed.

---

## related_requirement_id

### Definition

The ID of the related requirement, if available.

### Example

```json
"related_requirement_id": "REQ-001"
```

---

## related_business_rule_id

### Definition

The ID of the related business rule.

### Example

```json
"related_business_rule_id": "BR-001"
```

---

## related_endpoint_id

### Definition

The ID of the related API endpoint.

### Example

```json
"related_endpoint_id": "EP-001"
```

---

## related_query_id

### Definition

The ID of the related database query.

### Example

```json
"related_query_id": "SQL-DISC-001"
```

---

## related_service_id

### Definition

The ID of the related downstream service.

### Example

```json
"related_service_id": "SRV-REST-PIIDLOOKUP"
```

---

## detected_by

### Definition

The method used to detect the gap.

### Allowed Values

```text
Rule-Based Comparison
LLM-Assisted Review
Manual Review
Hybrid Review
```

### Example

```json
"detected_by": "Rule-Based Comparison"
```

---

## created_at

### Definition

The date when the gap was detected.

### Example

```json
"created_at": "2026-06-13"
```

---

## resolved_at

### Definition

The date when the gap was resolved.

### Example

```json
"resolved_at": null
```

---

# Complete Gap Record Example

```json
{
  "gap_id": "GAP-001",
  "gap_type": "Missing Header Gap",
  "gap_category_group": "Contract Gap",
  "source_of_truth": "Confluence",
  "compared_source": "Excel",
  "missing_from": "Excel",
  "affected_artifact": "api_endpoints",
  "affected_section": "required_headers",
  "affected_field": "x-macys-apikey",
  "severity": "High",
  "confidence": "High",
  "description": "The required header x-macys-apikey is defined in Confluence but missing from the Excel api_endpoints sheet.",
  "evidence": {
    "confluence": "x-macys-apikey is listed as a required request header.",
    "node_red": "Validate Headers node checks for x-macys-apikey.",
    "excel": "api_endpoints required_headers does not include x-macys-apikey."
  },
  "recommendation": "Add x-macys-apikey to the required_headers column in the api_endpoints sheet.",
  "status": "Open",
  "related_endpoint_id": "EP-001",
  "detected_by": "Rule-Based Comparison",
  "created_at": "2026-06-13",
  "resolved_at": null
}
```

---

# Example 2: Technology Mismatch Gap

```json
{
  "gap_id": "GAP-002",
  "gap_type": "Technology Mismatch Gap",
  "gap_category_group": "Consistency Gap",
  "source_of_truth": "Confluence",
  "compared_source": "Excel",
  "missing_from": "Excel",
  "affected_artifact": "technical_requirements",
  "affected_section": "database",
  "affected_field": "database_type",
  "severity": "Critical",
  "confidence": "High",
  "description": "The target database should be PostgreSQL, but the Excel workbook refers to SQLite.",
  "evidence": {
    "confluence": "Requirement expects enterprise PostgreSQL persistence.",
    "node_red": "Prototype may use SQLite only for local testing.",
    "excel": "technical_requirements database_type is set to SQLite."
  },
  "recommendation": "Update the technical_requirements sheet to use PostgreSQL as the target database.",
  "status": "Open",
  "detected_by": "Hybrid Review",
  "created_at": "2026-06-13",
  "resolved_at": null
}
```

---

# Example 3: Missing Workflow Step Gap

```json
{
  "gap_id": "GAP-003",
  "gap_type": "Workflow Step Gap",
  "gap_category_group": "Business Logic Gap",
  "source_of_truth": "Node-RED",
  "compared_source": "Excel",
  "missing_from": "Excel",
  "affected_artifact": "endpoint_service_mapping",
  "affected_section": "workflow_sequence",
  "affected_field": "Validate Enum Fields",
  "severity": "High",
  "confidence": "High",
  "description": "The Node-RED workflow contains an enum validation step, but the Excel endpoint_service_mapping sheet does not include this step.",
  "evidence": {
    "confluence": "entryMode must be validated against allowed enum values.",
    "node_red": "Validate Enum Fields node exists before downstream service invocation.",
    "excel": "endpoint_service_mapping does not include enum validation in the workflow sequence."
  },
  "recommendation": "Add the enum validation step to endpoint_service_mapping before the downstream service call.",
  "status": "Open",
  "related_endpoint_id": "EP-001",
  "detected_by": "Rule-Based Comparison",
  "created_at": "2026-06-13",
  "resolved_at": null
}
```

---

# Gap Report Output Format

The full gap analysis output should be stored as a list of gap records.

```json
{
  "analysis_id": "GA-001",
  "project_name": "Confluence Node-RED Excel Gap Analysis",
  "analysis_date": "2026-06-13",
  "sources_compared": [
    "Confluence",
    "Node-RED",
    "Excel"
  ],
  "total_gaps": 3,
  "critical_count": 1,
  "high_count": 2,
  "medium_count": 0,
  "low_count": 0,
  "informational_count": 0,
  "gaps": [
    {
      "gap_id": "GAP-001",
      "gap_type": "Missing Header Gap",
      "severity": "High",
      "status": "Open"
    },
    {
      "gap_id": "GAP-002",
      "gap_type": "Technology Mismatch Gap",
      "severity": "Critical",
      "status": "Open"
    },
    {
      "gap_id": "GAP-003",
      "gap_type": "Workflow Step Gap",
      "severity": "High",
      "status": "Open"
    }
  ]
}
```

---

# CSV or Excel-Friendly Format

The same schema can also be represented in tabular format.

| Column Name              | Description                                     |
| ------------------------ | ----------------------------------------------- |
| gap_id                   | Unique gap identifier                           |
| gap_type                 | Specific gap type                               |
| gap_category_group       | Broader gap group                               |
| source_of_truth          | Artifact treated as correct                     |
| compared_source          | Artifact being checked                          |
| missing_from             | Artifact where the issue exists                 |
| affected_artifact        | Sheet, node, file, or document section affected |
| affected_section         | Specific column, node property, or section      |
| affected_field           | Exact field, header, rule, service, or query    |
| severity                 | Priority level                                  |
| confidence               | Detection confidence                            |
| description              | Explanation of the gap                          |
| evidence_confluence      | Evidence from Confluence                        |
| evidence_node_red        | Evidence from Node-RED                          |
| evidence_excel           | Evidence from Excel                             |
| recommendation           | Suggested fix                                   |
| status                   | Current gap status                              |
| related_endpoint_id      | Related endpoint                                |
| related_business_rule_id | Related business rule                           |
| related_service_id       | Related service                                 |
| related_query_id         | Related query                                   |
| detected_by              | Detection method                                |
| created_at               | Detection date                                  |
| resolved_at              | Resolution date                                 |

---

# Rules for Writing Gap Descriptions

Each gap description should be:

* Specific
* Actionable
* Traceable to evidence
* Easy for engineers to understand
* Linked to the affected artifact

A weak description would be:

```text
Header missing.
```

A better description would be:

```text
The required header x-macys-apikey is defined in Confluence and validated in Node-RED, but it is missing from the Excel api_endpoints required_headers column.
```

---

# Rules for Writing Recommendations

Each recommendation should tell exactly what should be changed.

A weak recommendation would be:

```text
Fix Excel.
```

A better recommendation would be:

```text
Add x-macys-apikey to the api_endpoints required_headers column and include it in the validation_rules field as a required header.
```

---

# Summary

The Gap Output Schema defines how every gap should be stored and reported.

Each gap record includes the gap type, affected artifact, severity, evidence, recommendation, and status.

This schema makes the gap analysis output structured, measurable, and actionable.

It also prepares the project for future implementation, where the system can automatically produce gap reports from Confluence, Node-RED, and Excel comparisons.
