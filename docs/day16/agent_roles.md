# Day 16 — Phase 2: Agent Roles

## Purpose

This document defines the AI agent roles for the future enterprise API artifact generation and gap analysis system.

The purpose of defining agent roles is to separate responsibilities across the pipeline so that each agent has a clear task, input, output, and behavior boundary.

The future system should not depend on one generic AI prompt doing everything.

Instead, the system should use specialized agent roles for:

```text
Requirement extraction
Workflow modeling
Excel intake generation
Gap analysis
Evaluation scoring
Human review support
```

This makes the pipeline more reliable, repeatable, and easier to evaluate.

---

## Why Agent Roles Are Needed

Enterprise API artifact generation involves multiple different tasks.

Each task requires a different kind of reasoning.

For example:

* Extracting requirements from Confluence requires careful reading.
* Modeling workflows requires step-by-step process understanding.
* Generating Excel intake content requires structured formatting.
* Performing gap analysis requires comparison across artifacts.
* Evaluating results requires metric calculation.
* Reviewing output requires evidence checking and risk classification.

If one agent handles all tasks without separation, the output may become inconsistent or hard to validate.

Agent roles solve this by assigning clear responsibilities.

---

## Agent Role Overview

The future system will use the following primary agents:

```text
Requirement Extraction Agent
Workflow Modeling Agent
Excel Intake Generation Agent
Gap Analysis Agent
Evaluation Agent
Reviewer Agent
```

Optional future agents may include:

```text
Node-RED Flow Generation Agent
Go Code Readiness Agent
Documentation Agent
Research Reporting Agent
```

Each agent is defined below.

---

# 1. Requirement Extraction Agent

## Role

The Requirement Extraction Agent reads Confluence-style requirement documents and extracts structured API requirement information.

This agent treats the requirement document as the primary source of truth.

---

## Primary Input

```text
Confluence requirement document
API requirement notes
Sample request and response payloads
Business process description
Validation descriptions
Downstream service details
Database references
```

---

## Primary Output

The agent should produce a structured requirement extraction output.

The output may include:

```text
Endpoint path
HTTP method
Operation name
Required headers
Optional headers
Request body fields
Response body fields
Validation rules
Enum values
Business rules
Downstream services
Database references
Error handling expectations
Technology assumptions
Open questions
```

---

## Responsibilities

The Requirement Extraction Agent should:

```text
Identify API endpoints
Extract request and response contracts
Extract required and optional fields
Extract validation rules
Extract enum values
Extract business rules
Extract downstream service references
Extract database references
Identify unclear or missing details
Separate confirmed facts from assumptions
```

---

## Guardrails

The Requirement Extraction Agent must not:

```text
Invent endpoints not present in the source
Invent request fields
Invent response fields
Invent database tables
Invent downstream services
Assume optional fields are required without evidence
Assume business rules not stated in the source
```

If information is missing, the agent should mark it as:

```text
Not Found
Missing
Ambiguous
Needs Manual Review
```

---

## Example Output Summary

```text
Endpoint: POST /api/v1/discount/interrogateAccount
Required Headers: requestorInfo-clientId, requestorInfo-subclientId, x-macys-apikey
Request Fields: account.encryptInfo.encryptedData, account.encryptInfo.keyType, account.entryMode
Downstream Service: PIIDLookup
Open Question: Timeout behavior is not defined in the requirement.
```

---

# 2. Workflow Modeling Agent

## Role

The Workflow Modeling Agent converts extracted requirements into a step-by-step workflow representation.

This agent focuses on process sequence and execution logic.

---

## Primary Input

```text
Structured requirement extraction output
Business process description
Validation rules
Service dependencies
Database dependencies
Response mapping rules
```

---

## Primary Output

The agent should produce a workflow representation.

The output may include:

```text
Workflow ID
Workflow name
Endpoint reference
Ordered workflow steps
Step type
Step input
Step output
Decision points
Service calls
Database queries
Response mapping
Error handling steps
```

---

## Responsibilities

The Workflow Modeling Agent should:

```text
Convert requirements into ordered workflow steps
Identify validation steps
Identify mapping steps
Identify downstream service call steps
Identify database query steps
Identify business decision points
Identify response construction steps
Identify error handling paths
Preserve the original requirement intent
```

---

## Guardrails

The Workflow Modeling Agent must not:

```text
Add workflow steps not supported by requirements
Skip required validation steps
Change business rule order without reason
Assume a service call exists unless stated
Assume a database lookup exists unless stated
Replace required logic with generic CRUD behavior
```

If the workflow cannot be fully modeled, the agent should mark the missing part as:

```text
Needs Manual Review
Missing Source Detail
Ambiguous Workflow Step
```

---

## Example Workflow

```text
HTTP Request Received
        ↓
Validate Required Headers
        ↓
Validate Request Body
        ↓
Validate Enum Fields
        ↓
Build Downstream Request
        ↓
Invoke PIIDLookup Service
        ↓
Validate Downstream Response
        ↓
Map Final Response
        ↓
Return HTTP Response
```

---

# 3. Excel Intake Generation Agent

## Role

The Excel Intake Generation Agent converts structured requirements and workflow representations into Excel-ready intake content for the Go code generation factory.

This agent focuses on producing structured sheet-level content.

---

## Primary Input

```text
Requirement extraction output
Workflow representation
Service mapping details
Database query details
Business rule definitions
Technical requirements
```

---

## Primary Output

The agent should produce Excel-ready content for sheets such as:

```text
technical_requirements
api_endpoints
entities
external_services
business_requirements
endpoint_service_mapping
queries
```

---

## Responsibilities

The Excel Intake Generation Agent should:

```text
Create technical requirement rows
Create API endpoint rows
Create external service rows
Create business requirement rows
Create endpoint-service mapping rows
Create entity definitions when needed
Create query definitions when needed
Preserve field names and data types
Represent validations clearly
Represent business logic clearly
Represent downstream dependencies clearly
```

---

## Guardrails

The Excel Intake Generation Agent must not:

```text
Add unsupported sheets
Add unsupported services
Add unsupported database tables
Change PostgreSQL to SQLite unless source says so
Convert business logic into generic CRUD assumptions
Drop required headers or fields
Drop validation rules
```

If a required Excel value cannot be determined, the agent should mark it as:

```text
TBD
Needs Manual Review
Not Found in Source
```

---

## Example Output Summary

```text
Sheet: api_endpoints
Endpoint ID: EP-001
Method: POST
Path: /api/v1/discount/interrogateAccount
Required Headers: requestorInfo-clientId, requestorInfo-subclientId, x-macys-apikey
Request Body: account.encryptInfo, account.entryMode
Validation Rules: keyType enum, entryMode enum
```

---

# 4. Gap Analysis Agent

## Role

The Gap Analysis Agent compares Confluence requirements, Node-RED workflows, and Excel intake workbook content to detect missing, incorrect, inconsistent, or unsupported information.

This agent is responsible for quality control.

---

## Primary Input

```text
Confluence requirement extraction
Node-RED workflow representation
Excel intake workbook summary
Gap category definitions
Severity rules
Gap output schema
```

---

## Primary Output

The agent should produce a structured gap report.

Each gap should include:

```text
gap_id
gap_type
gap_category_group
source_of_truth
compared_source
missing_from
affected_artifact
affected_section
affected_field
severity
confidence
description
evidence
recommendation
status
```

---

## Responsibilities

The Gap Analysis Agent should:

```text
Compare Confluence to Node-RED
Compare Confluence to Excel
Compare Node-RED to Excel
Identify missing endpoints
Identify missing headers
Identify missing request and response fields
Identify missing validations
Identify missing business rules
Identify missing service mappings
Identify missing database queries
Identify technology mismatches
Identify unsupported assumptions
Assign severity using predefined rules
Provide actionable recommendations
```

---

## Guardrails

The Gap Analysis Agent must not:

```text
Treat every naming difference as a real gap
Ignore documented field mappings
Invent source-of-truth requirements
Assign Critical severity without impact evidence
Report ambiguous issues as confirmed defects
```

Ambiguous findings should be marked as:

```text
Informational
Low Confidence
Needs Manual Review
```

---

## Example Gap

```json
{
  "gap_id": "GAP-001",
  "gap_type": "Missing Header Gap",
  "source_of_truth": "Confluence",
  "missing_from": "Excel",
  "affected_artifact": "api_endpoints",
  "affected_field": "x-macys-apikey",
  "severity": "High",
  "confidence": "High",
  "description": "The required header x-macys-apikey exists in Confluence but is missing from the Excel api_endpoints sheet.",
  "recommendation": "Add x-macys-apikey to required_headers and validation_rules in api_endpoints."
}
```

---

# 5. Evaluation Agent

## Role

The Evaluation Agent compares AI-detected gaps against the manual baseline and calculates evaluation metrics.

This agent is responsible for research measurement.

---

## Primary Input

```text
expected_gaps.json
detected_gaps.json
manual_review_time.json
ai_review_time.json
evaluation metric definitions
matching rules
scoring model
```

---

## Primary Output

The agent should produce:

```text
evaluation_result.json
evaluation_summary.md
gap_comparison_table.md
readiness_score.json
```

---

## Responsibilities

The Evaluation Agent should:

```text
Compare manual and AI gap reports
Identify true positives
Identify false positives
Identify false negatives
Identify partial matches
Identify severity matches
Identify severity mismatches
Calculate gap detection accuracy
Calculate false positive rate
Calculate false negative rate
Calculate severity assignment accuracy
Calculate manual review reduction
Calculate code generation readiness score
Produce research-ready result summaries
```

---

## Guardrails

The Evaluation Agent must not:

```text
Change metric formulas during evaluation
Hide false positives
Hide false negatives
Claim results that were not measured
Treat ambiguous matches as exact matches without note
Overstate research conclusions
```

If a match is unclear, the agent should mark it as:

```text
Partial Match
Needs Manual Review
Ambiguous Match
```

---

## Example Evaluation Summary

```json
{
  "benchmark_id": "api_001_interrogate_account",
  "expected_gaps": 8,
  "detected_gaps": 9,
  "true_positives": 7,
  "false_positives": 2,
  "false_negatives": 1,
  "gap_detection_accuracy": "87.5%",
  "manual_review_reduction": "71.4%"
}
```

---

# 6. Reviewer Agent

## Role

The Reviewer Agent acts as a final quality-control layer.

This agent reviews generated outputs, identifies risk areas, and prepares items for human confirmation.

The Reviewer Agent does not replace a human reviewer.

It supports the human reviewer.

---

## Primary Input

```text
Requirement extraction output
Workflow representation
Excel intake content
Gap analysis report
Evaluation result
Open questions
Low-confidence findings
```

---

## Primary Output

The agent should produce:

```text
review_summary.md
open_questions.md
risk_items.md
approval_recommendation.md
```

---

## Responsibilities

The Reviewer Agent should:

```text
Review high-risk outputs
Flag unsupported assumptions
Flag Critical and High gaps
Summarize open questions
Identify low-confidence findings
Prepare human review checklist
Recommend whether the artifact is ready for the next stage
```

---

## Guardrails

The Reviewer Agent must not:

```text
Approve artifacts with unresolved Critical gaps
Ignore low-confidence findings
Treat AI output as final authority
Remove human review from high-risk decisions
Claim production readiness without evidence
```

---

## Example Reviewer Output

```text
Review Status: Needs Review

Reason:
One Critical gap remains unresolved. The Excel workbook references SQLite, but the target technology is PostgreSQL.

Required Action:
Update technical_requirements to PostgreSQL and re-run gap analysis.
```

---

# Optional Future Agents

The following agents may be added later after the core pipeline is stable.

---

# 7. Node-RED Flow Generation Agent

## Role

This agent generates or updates Node-RED flow JSON from workflow representations.

## Primary Responsibility

```text
Convert workflow schema into Node-RED-compatible flow structure.
```

## Guardrail

This agent should not invent workflow logic that was not produced by the Workflow Modeling Agent.

---

# 8. Go Code Readiness Agent

## Role

This agent checks whether the Excel intake workbook contains enough information to support Go code generation.

## Primary Responsibility

```text
Review workbook completeness, technology alignment, service mappings, entities, and queries before Go factory execution.
```

## Guardrail

This agent should not generate Go code during the no-code planning phase.

---

# 9. Documentation Agent

## Role

This agent generates project documentation from structured artifacts.

## Primary Responsibility

```text
Create README updates, architecture notes, completion reports, and research documentation.
```

## Guardrail

This agent should not modify technical facts without source support.

---

# 10. Research Reporting Agent

## Role

This agent converts evaluation results into paper-ready, Medium-ready, LinkedIn-ready, and resume-ready summaries.

## Primary Responsibility

```text
Summarize measured results clearly and honestly.
```

## Guardrail

This agent must separate planned targets from actual measured results.

---

# Agent Interaction Summary

The core agent flow is:

```text
Requirement Extraction Agent
        ↓
Workflow Modeling Agent
        ↓
Excel Intake Generation Agent
        ↓
Gap Analysis Agent
        ↓
Evaluation Agent
        ↓
Reviewer Agent
```

Optional future flow:

```text
Reviewer Agent
        ↓
Node-RED Flow Generation Agent
        ↓
Go Code Readiness Agent
        ↓
Documentation Agent
        ↓
Research Reporting Agent
```

---

# Agent Responsibility Matrix

| Agent                          | Primary Task                             | Main Output                   |
| ------------------------------ | ---------------------------------------- | ----------------------------- |
| Requirement Extraction Agent   | Extract structured requirements          | Requirement extraction schema |
| Workflow Modeling Agent        | Convert requirements into workflow steps | Workflow representation       |
| Excel Intake Generation Agent  | Generate Excel-ready sheet content       | Excel intake summary          |
| Gap Analysis Agent             | Detect cross-artifact gaps               | Gap report                    |
| Evaluation Agent               | Compare AI output to baseline            | Evaluation metrics            |
| Reviewer Agent                 | Review risks and open questions          | Review summary                |
| Node-RED Flow Generation Agent | Generate Node-RED flow JSON              | Node-RED flow                 |
| Go Code Readiness Agent        | Check Go generation readiness            | Readiness review              |
| Documentation Agent            | Produce documentation                    | Markdown docs                 |
| Research Reporting Agent       | Produce research summaries               | Paper/report summaries        |

---

# General Agent Behavior Rules

All agents should follow these rules:

```text
Use source artifacts as authority
Do not invent missing details
Mark ambiguity clearly
Use structured outputs
Include evidence when possible
Follow the assigned schema
Respect the current project phase
Separate confirmed facts from assumptions
Escalate Critical and High risk findings
Support human review instead of replacing it
```

---

# Summary

This document defines the agent roles for the future AI-assisted enterprise API artifact generation and gap analysis system.

The core agents are:

```text
Requirement Extraction Agent
Workflow Modeling Agent
Excel Intake Generation Agent
Gap Analysis Agent
Evaluation Agent
Reviewer Agent
```

Each agent has a clear input, output, responsibility, and guardrail.

This role separation makes the future system more reliable, easier to evaluate, and better aligned with the research goal of automating enterprise API development using AI-driven requirement and workflow synthesis.
