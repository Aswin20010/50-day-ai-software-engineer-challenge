# Day 19 — Baseline Comparison Design

## Purpose

This document defines the baseline comparison design for the 50-Day AI Software Engineer Challenge.

The purpose of baseline comparison is to evaluate whether the AI-assisted workflow generation approach provides measurable improvement compared to a traditional manual process.

The project is not only trying to generate workflows using AI. It is also trying to prove that AI-assisted workflow synthesis can reduce manual effort, improve repeatability, and preserve requirement correctness when applied to enterprise API development.

---

## Background

The project focuses on transforming enterprise API requirements into structured workflow artifacts.

Earlier days defined the foundation for:

* requirement extraction
* workflow representation
* workflow schema
* validation framework
* validation checklist
* workflow quality metrics
* validation failure categories
* evaluation dataset design
* ground truth workflow guidelines

Day 19 extends the evaluation strategy by defining how the AI-assisted process will be compared against a manual baseline.

This comparison is important because a research project needs measurable evidence.

Without a baseline, the project can only say:

```text
The AI system generated a workflow.
```

With a baseline, the project can say:

```text
The AI-assisted process generated workflow artifacts faster and with comparable or better requirement coverage than the manual process.
```

---

## What Baseline Comparison Means

Baseline comparison means evaluating two different approaches on the same requirement samples.

The two approaches are:

```text
Manual Baseline Process
vs
AI-Assisted Workflow Generation Process
```

Both approaches should use the same source requirement samples from the evaluation dataset.

The outputs from both approaches should be compared against the approved ground truth workflow.

This allows the project to measure whether the AI-assisted process is better, equal, or worse than the manual process across defined metrics.

---

## What Is Being Compared

The comparison focuses on how workflows are created from source requirements.

The main comparison is:

```text
Manual creation of workflow artifacts
        vs
AI-assisted creation of workflow artifacts
```

The comparison should evaluate:

* time required
* requirement coverage
* workflow completeness
* validation accuracy
* business rule coverage
* step ordering correctness
* downstream mapping accuracy
* database mapping accuracy
* response mapping accuracy
* error handling completeness
* number of missed steps
* number of review corrections
* hallucination or unsupported logic rate
* artifact readiness
* repeatability

---

## Manual Baseline

The manual baseline represents the traditional approach where a developer, analyst, or engineer reads the requirement and manually creates workflow artifacts.

The manual process may include:

* reading the requirement document
* understanding endpoint purpose
* identifying headers
* identifying request body fields
* extracting validation rules
* identifying business rules
* identifying downstream services
* identifying database interactions
* defining workflow steps
* mapping responses
* identifying error scenarios
* reviewing and correcting gaps
* preparing artifacts for later implementation

This process depends heavily on human understanding, experience, and review effort.

---

## AI-Assisted Process

The AI-assisted process represents the proposed project approach.

In this process, the source requirement is provided to an AI system, and the AI generates structured requirement and workflow artifacts.

The AI-assisted process may include:

* providing source requirement to AI
* extracting structured requirement output
* generating workflow representation
* applying workflow validation checklist
* calculating workflow quality metrics
* identifying validation failures
* classifying missing or incorrect logic
* correcting the generated workflow
* preparing artifact-ready output

This process still includes human review, but the goal is to reduce manual effort and improve repeatability.

---

## Why Baseline Comparison Is Needed

Baseline comparison is needed because the project must prove practical value.

AI-generated artifacts are useful only if they improve or support the traditional development process.

The comparison helps answer:

* Does AI reduce the time needed to create workflow artifacts?
* Does AI preserve important requirement details?
* Does AI capture business rules correctly?
* Does AI reduce repetitive manual documentation effort?
* Does AI produce consistent workflow structures?
* Does AI make review easier?
* Does AI introduce unsupported assumptions?
* Does AI output become artifact-ready faster than manual output?

These questions are important for both the technical project and the research paper.

---

## Comparison Strategy

The comparison strategy should follow a controlled evaluation process.

The same dataset item should be processed through both manual and AI-assisted approaches.

Recommended flow:

```text
Select Dataset Item
        ↓
Create Manual Workflow
        ↓
Generate AI-Assisted Workflow
        ↓
Compare Both Against Ground Truth Workflow
        ↓
Apply Workflow Quality Metrics
        ↓
Record Time and Correction Effort
        ↓
Analyze Results
```

This ensures that both approaches are evaluated fairly.

---

## Evaluation Inputs

The baseline comparison uses the following inputs.

### 1. Source Requirement

The original requirement text from the evaluation dataset.

This is the same input for both manual and AI-assisted processes.

### 2. Ground Truth Workflow

The approved expected workflow for the dataset item.

This is used as the reference answer.

### 3. Manual Workflow Output

The workflow created manually from the source requirement.

This represents the traditional baseline.

### 4. AI-Generated Workflow Output

The workflow generated by the AI-assisted process.

This represents the proposed approach.

### 5. Review and Scoring Results

The validation checklist and quality metrics applied to both outputs.

---

## Evaluation Outputs

The baseline comparison should produce the following outputs:

* manual process time
* AI-assisted process time
* manual workflow quality score
* AI-generated workflow quality score
* manual workflow correction count
* AI workflow correction count
* missed requirement count
* hallucinated logic count
* validation failure categories
* artifact readiness comparison
* final comparison summary

These outputs will be used to support research findings.

---

## Recommended Comparison Table

Each dataset item can be evaluated using a comparison table.

| Evaluation Area             | Manual Workflow | AI-Assisted Workflow | Ground Truth Reference |
| --------------------------- | --------------: | -------------------: | ---------------------- |
| Time Taken                  |                 |                      |                        |
| Requirement Coverage        |                 |                      |                        |
| Business Rule Coverage      |                 |                      |                        |
| Validation Accuracy         |                 |                      |                        |
| Step Completeness           |                 |                      |                        |
| Step Ordering Accuracy      |                 |                      |                        |
| Downstream Mapping Accuracy |                 |                      |                        |
| Database Mapping Accuracy   |                 |                      |                        |
| Response Mapping Accuracy   |                 |                      |                        |
| Error Handling Completeness |                 |                      |                        |
| Traceability                |                 |                      |                        |
| Hallucination Control       |                 |                      |                        |
| Artifact Readiness          |                 |                      |                        |
| Review Corrections Needed   |                 |                      |                        |
| Final Decision              |                 |                      |                        |

---

## Example Comparison Scenario

### Dataset Item

```text
Dataset Item ID: REQ-WF-001
API Name: Create Application Message API
Category: Simple Create API
Complexity: MEDIUM
```

### Manual Process

The reviewer manually reads the requirement and creates a workflow.

Possible result:

```text
Time Taken: 45 minutes
Workflow Quality Score: 88%
Corrections Needed: 3
```

### AI-Assisted Process

The AI generates the workflow from the same source requirement.

Possible result:

```text
Time Taken: 8 minutes
Workflow Quality Score: 82%
Corrections Needed: 5
```

### Interpretation

The AI-assisted workflow may require more correction, but it significantly reduces initial workflow creation time.

After validation and correction, it may become comparable to the manual workflow.

This helps support the idea that AI can accelerate early workflow creation while still requiring validation.

---

## Key Research Comparison Claim

The project should avoid claiming that AI fully replaces developers or analysts.

A more realistic and stronger claim is:

```text
AI-assisted workflow synthesis can reduce the initial effort required to convert enterprise API requirements into structured workflow artifacts, while validation and human review help preserve correctness and reduce unsupported logic.
```

This claim is more defensible because it acknowledges the role of validation and human review.

---

## Baseline Comparison Dimensions

The baseline comparison should evaluate three major dimensions.

---

## 1. Efficiency

Efficiency measures how much time and effort each approach requires.

Questions:

* How long did manual workflow creation take?
* How long did AI-assisted workflow creation take?
* How much review time was needed?
* How many corrections were required?
* How much repetitive documentation effort was reduced?

Efficiency is important because one of the project goals is time reduction.

---

## 2. Correctness

Correctness measures how accurately each approach represents the source requirement.

Questions:

* Were required headers captured?
* Were request fields captured?
* Were validations included?
* Were business rules preserved?
* Were downstream calls mapped correctly?
* Were database operations mapped correctly?
* Was response mapping correct?
* Were error scenarios included?

Correctness is important because faster output is not useful if the workflow is wrong.

---

## 3. Repeatability

Repeatability measures whether the approach produces consistent workflow structure across multiple requirement samples.

Questions:

* Does the workflow follow the same schema each time?
* Are step names consistent?
* Are validation sections consistently included?
* Are response mappings consistently represented?
* Are traceability references consistently captured?
* Can the output be reviewed using the same checklist?

Repeatability is important because enterprise automation requires consistent artifacts.

---

## Baseline Comparison Method

For each dataset item, the following method should be used.

### Step 1 — Select Dataset Item

Choose one approved dataset item from the workflow evaluation dataset.

Example:

```text
REQ-WF-001
```

---

### Step 2 — Create Manual Workflow

Create the workflow manually by reading the source requirement.

Record:

* start time
* end time
* notes
* assumptions
* missed or unclear areas
* corrections made during review

---

### Step 3 — Generate AI-Assisted Workflow

Provide the same source requirement to the AI system.

Generate the structured workflow output.

Record:

* time taken
* prompt used
* AI output version
* corrections needed
* validation results

---

### Step 4 — Compare Against Ground Truth

Compare both manual and AI-generated workflows against the approved ground truth workflow.

Apply the same scoring method to both outputs.

---

### Step 5 — Record Metrics

Record metrics such as:

* time taken
* coverage score
* business rule score
* validation accuracy
* response mapping accuracy
* error handling completeness
* correction count
* hallucination count
* artifact readiness

---

### Step 6 — Analyze Results

Analyze which approach performed better and where.

The analysis should include:

* where manual workflow was stronger
* where AI workflow was stronger
* where AI missed details
* where manual work took longer
* how validation improved AI output
* whether AI reduced initial effort

---

## Fair Comparison Rules

To keep the comparison fair, follow these rules:

1. Use the same source requirement for both approaches.
2. Use the same approved ground truth workflow for scoring.
3. Apply the same validation checklist to both outputs.
4. Apply the same quality metrics to both outputs.
5. Record time consistently.
6. Do not improve the AI output before scoring the initial AI version.
7. Do not improve the manual output before scoring the initial manual version.
8. Record corrected scores separately from initial scores.
9. Document assumptions and ambiguity.
10. Avoid using confidential data in evaluation results.

---

## Initial Score vs Corrected Score

The comparison should distinguish between initial output and corrected output.

This is important because both manual and AI-assisted workflows may improve after review.

Recommended scoring categories:

```text
Manual Initial Workflow Score
Manual Corrected Workflow Score
AI Initial Workflow Score
AI Corrected Workflow Score
```

This allows the project to show how validation improves output quality.

Example:

| Output Type               | Score |
| ------------------------- | ----: |
| Manual Initial Workflow   |   86% |
| Manual Corrected Workflow |   95% |
| AI Initial Workflow       |   78% |
| AI Corrected Workflow     |   93% |

This gives a more honest evaluation.

---

## Time Measurement Rules

Time should be measured consistently.

Recommended time categories:

| Time Category          | Description                              |
| ---------------------- | ---------------------------------------- |
| Reading Time           | Time spent understanding the requirement |
| Workflow Creation Time | Time spent creating workflow steps       |
| Review Time            | Time spent validating the workflow       |
| Correction Time        | Time spent fixing issues                 |
| Total Time             | Sum of all time spent                    |

For AI-assisted workflow generation, record:

* prompt preparation time
* AI generation time
* validation time
* correction time
* total time

---

## Expected Research Findings

The baseline comparison may show results such as:

```text
AI-assisted workflow generation reduced initial workflow drafting time by 60–80%.
```

```text
Manual workflows had slightly higher initial correctness, but AI workflows improved significantly after validation.
```

```text
AI-generated workflows were more structurally consistent across dataset samples.
```

```text
AI workflows required human review to remove unsupported assumptions and fill missing business rules.
```

These findings are realistic and useful for the research paper.

---

## Risks in Baseline Comparison

The baseline comparison may have several risks.

### 1. Manual Baseline Bias

If the same person creates both the ground truth and manual workflow, the manual workflow may appear stronger.

Mitigation:

```text
Separate ground truth creation from manual workflow timing when possible.
```

---

### 2. AI Prompt Bias

A better prompt can make AI output stronger, while a weak prompt can make it worse.

Mitigation:

```text
Use a consistent prompt template across all dataset items.
```

---

### 3. Dataset Bias

If the dataset contains only simple requirements, AI may appear stronger than it really is.

Mitigation:

```text
Use a balanced dataset across simple, medium, and complex categories.
```

---

### 4. Scoring Subjectivity

Some workflow quality scores may involve judgment.

Mitigation:

```text
Use defined quality metrics, scoring rules, and failure categories.
```

---

### 5. Correction Time Ambiguity

It may be unclear whether correction time belongs to generation or review.

Mitigation:

```text
Track generation time, review time, and correction time separately.
```

---

## Relationship to Day 17 and Day 18

Day 19 builds directly on Day 17 and Day 18.

### Day 17

Defined:

* validation checklist
* workflow quality metrics
* failure categories

These are used to score manual and AI-assisted workflows.

### Day 18

Defined:

* evaluation dataset
* sample requirement categories
* ground truth workflow guidelines
* dataset review process

These provide the inputs for baseline comparison.

### Day 19

Defines:

* how to compare manual and AI-assisted workflow creation
* what metrics to record
* how to interpret the results

Together, these days create the core research evaluation framework.

---

## How This Supports the Research Paper

The baseline comparison design supports the research paper by providing a way to measure the project’s main claim.

The paper can report:

* manual workflow creation time
* AI-assisted workflow creation time
* quality score comparison
* correction effort comparison
* coverage comparison
* failure category comparison
* improvement after validation

This helps convert the project from a technical prototype into a research-backed evaluation.

---

## Practical Value

Baseline comparison also has practical value for enterprise API teams.

It can help answer whether AI-assisted workflow generation is useful in real software delivery.

Practical questions include:

* Can AI reduce requirement analysis time?
* Can AI help create consistent workflow documentation?
* Can AI reduce repeated manual Excel/spec creation work?
* Can AI help identify missing details earlier?
* Can AI support modernization from older systems to structured APIs?
* Can AI generate reviewable artifacts before implementation begins?

This makes the project useful beyond the 50-day challenge.

---

## Final Baseline Comparison Model

The final comparison model is:

```text
Approved Dataset Item
        ↓
Manual Workflow Creation
        ↓
Manual Workflow Score
        ↓
AI-Assisted Workflow Generation
        ↓
AI Workflow Score
        ↓
Validation and Correction
        ↓
Corrected Workflow Scores
        ↓
Manual vs AI Comparison
        ↓
Research Findings
```

---

## Summary

Baseline comparison is the evaluation method used to compare the traditional manual process with the AI-assisted workflow generation process.

The comparison evaluates:

* efficiency
* correctness
* repeatability
* review effort
* correction effort
* artifact readiness

This design ensures that the project can make a clear and defensible research claim.

Day 19 Phase 1 establishes the overall strategy for comparing manual workflow creation against AI-assisted workflow synthesis.
