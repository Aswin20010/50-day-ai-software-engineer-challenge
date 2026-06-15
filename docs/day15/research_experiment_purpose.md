# Day 15 — Phase 1: Research Experiment Purpose

## Purpose

This document defines the purpose of the research experiment for the 50-Day AI Software Engineer Challenge.

The goal of the experiment is to evaluate whether an AI-assisted enterprise API artifact generation pipeline can improve the process of converting requirements into structured implementation artifacts.

The research experiment focuses on the following pipeline:

```text
Confluence Requirement
        ↓
Node-RED Workflow
        ↓
Excel Intake Workbook
        ↓
Gap Analysis Report
        ↓
Go Code Generation Readiness
```

The purpose is not only to show that artifacts can be generated, but also to measure whether those artifacts are complete, accurate, reviewable, and ready for future code generation.

---

## Research Direction

The larger research direction of this project is:

```text
Automating Enterprise API Development using AI-Driven Requirement and Workflow Synthesis
```

This direction focuses on using AI to assist with enterprise API development tasks such as:

* Understanding requirement documents
* Extracting API contract details
* Representing workflow logic
* Generating structured Excel intake files
* Detecting gaps across artifacts
* Measuring readiness for code generation

The experiment is designed to provide measurable evidence for this research direction.

---

## Why an Experiment Is Needed

A project can claim that AI helps generate artifacts, but without an experiment, the claim is difficult to prove.

An experiment is needed to answer questions such as:

```text
Did AI reduce manual review effort?
Did AI improve requirement coverage?
Did AI detect missing information?
Did AI generate more complete Excel intake files?
Did AI help identify code generation blockers earlier?
```

The experiment provides a structured way to measure the value of the system.

It turns the project from a simple prototype into an evidence-based engineering and research project.

---

## Main Research Problem

Enterprise API development often requires converting unstructured or semi-structured requirements into multiple implementation artifacts.

These artifacts may include:

```text
Requirement documents
Workflow diagrams
Node-RED flows
Excel intake workbooks
Database query definitions
External service mappings
Business rule definitions
Generated Go service code
```

This transformation is usually manual, slow, repetitive, and error-prone.

Important details can be lost between artifacts.

For example:

* A required header may be missing from the Excel workbook.
* A validation rule may exist in the requirement but not in the workflow.
* A downstream service may be called in Node-RED but missing from the structured intake sheet.
* A database query may be required by business logic but not documented for code generation.
* The generated artifact may use the wrong technology assumption.

The research experiment is designed to evaluate whether AI-assisted artifact generation and gap analysis can reduce these issues.

---

## Main Research Question

The primary research question is:

```text
Can an AI-assisted framework improve the completeness, accuracy, and review efficiency of enterprise API artifact generation?
```

This question can be broken into smaller questions.

---

## Supporting Research Questions

### Question 1: Requirement Coverage

```text
How much of the original Confluence requirement can be captured in the generated workflow and Excel intake workbook?
```

This question measures whether important requirement elements are preserved.

---

### Question 2: Gap Detection Accuracy

```text
How accurately can the framework detect missing, incorrect, or inconsistent information across Confluence, Node-RED, and Excel artifacts?
```

This question measures the quality of the gap analysis framework.

---

### Question 3: Excel Completeness

```text
How complete is the generated Excel intake workbook for Go code generation?
```

This question measures whether the structured workbook contains enough information for the Go factory.

---

### Question 4: Manual Review Reduction

```text
How much manual review effort can be reduced using AI-assisted gap analysis?
```

This question measures practical engineering productivity.

---

### Question 5: Code Generation Readiness

```text
Can the framework identify whether an artifact set is ready for future Go code generation?
```

This question connects the research experiment to practical implementation readiness.

---

## Experiment Objective

The main objective of the experiment is to compare manual artifact review against AI-assisted artifact review.

The experiment should measure whether the AI-assisted approach can:

```text
Increase requirement coverage
Improve Excel intake completeness
Detect known gaps
Reduce false positives
Reduce false negatives
Assign useful severity levels
Provide actionable recommendations
Reduce manual review time
Improve code generation readiness
```

The experiment should produce measurable results that can be used in the research paper, portfolio project, Medium article, and LinkedIn posts.

---

## Experiment Scope

The experiment will focus on enterprise-style API examples.

The planned benchmark APIs include:

```text
API-001 — Interrogate Account API
API-002 — Evaluate Transaction Discount API
API-003 — Fabrics Wrapper API
API-004 — Clienteling Create Message API
```

These APIs represent different levels of complexity.

Some focus on API contract and validation.

Some focus on downstream service integration.

Some focus on database queries and business rules.

Some focus on workflow-to-Excel mapping.

---

## Artifacts Used in the Experiment

Each experiment case should include the following artifacts:

```text
confluence_requirement.md
node_red_flow.json
excel_intake_summary.md
expected_gaps.json
detected_gaps.json
evaluation_result.json
```

The Confluence requirement acts as the source of truth.

The Node-RED flow represents workflow logic.

The Excel intake workbook represents structured input for the Go code generation factory.

The expected gaps file represents the manual baseline.

The detected gaps file represents the AI-assisted output.

The evaluation result file stores the measured performance.

---

## Manual Review Baseline

The experiment requires a manual review baseline.

A manual baseline is a human-created reference answer.

The reviewer manually compares the artifacts and identifies expected gaps.

The manual baseline is important because it provides the ground truth for evaluation.

The AI-assisted output can then be compared against this baseline to measure:

```text
True positives
False positives
False negatives
Severity assignment accuracy
Recommendation usefulness
Manual review reduction
```

Without a manual baseline, the experiment would not have a reliable way to measure correctness.

---

## AI-Assisted Review

The AI-assisted review represents the output produced by the framework.

It should detect gaps between:

```text
Confluence and Node-RED
Confluence and Excel
Node-RED and Excel
Excel and Confluence
Node-RED and Confluence
```

The AI-assisted output should include:

* Gap type
* Gap category
* Source of truth
* Missing artifact
* Affected field or section
* Severity
* Evidence
* Recommendation
* Status

The AI output should be structured so that it can be compared with the manual baseline.

---

## Metrics Used in the Experiment

The experiment will use the metrics defined in Day 14.

Key metrics include:

```text
Requirement Coverage
Workflow Completeness Score
Excel Completeness Score
Gap Detection Accuracy
False Positive Rate
False Negative Rate
Severity Assignment Accuracy
Recommendation Usefulness Score
Manual Review Reduction
Code Generation Readiness Score
Artifact Consistency Score
Blocking Gap Count
Gap Resolution Rate
```

These metrics make the experiment measurable and repeatable.

---

## Expected Research Claims

The experiment should support claims such as:

```text
The framework captured 85%+ of requirement elements.
The framework achieved 85%+ gap detection accuracy.
The framework reduced manual review effort by 70%.
The generated Excel intake workbook reached 90%+ completeness.
The code generation readiness score exceeded 85%.
```

These claims will only be made later if the measured results support them.

For now, these are target outcomes, not final proven results.

---

## Hypothesis

The initial hypothesis for the experiment is:

```text
An AI-assisted artifact generation and gap analysis framework can reduce manual review effort while maintaining high requirement coverage and gap detection accuracy for enterprise API development.
```

This hypothesis will be tested using benchmark API examples and evaluation metrics.

---

## Independent and Dependent Variables

At a high level, the experiment compares two approaches.

### Independent Variable

The review approach:

```text
Manual review
AI-assisted review
```

### Dependent Variables

Measured outcomes:

```text
Review time
Requirement coverage
Detected gaps
False positives
False negatives
Severity accuracy
Excel completeness
Code generation readiness
```

These variables will be defined in more detail in a later phase.

---

## Success Criteria

The experiment is considered successful if the AI-assisted framework can achieve the following target results:

| Metric                          | Target        |
| ------------------------------- | ------------- |
| Requirement Coverage            | 85% or higher |
| Gap Detection Accuracy          | 85% or higher |
| Excel Completeness Score        | 90% or higher |
| False Positive Rate             | 15% or lower  |
| False Negative Rate             | 15% or lower  |
| Manual Review Reduction         | 70% or higher |
| Code Generation Readiness Score | 85% or higher |

These targets may be refined later as the project matures.

---

## Importance to the 50-Day Challenge

Day 15 is important because it connects the technical project to research validation.

Earlier days focused on:

```text
Requirement extraction
Workflow representation
Schema design
Gap analysis
Evaluation metrics
```

Day 15 begins shaping these pieces into a research experiment.

This makes the project stronger because it shows not just what was built, but how it will be evaluated.

---

## Importance to Resume and Portfolio

This experiment design can be explained in interviews as:

```text
I designed an AI-assisted enterprise API artifact generation pipeline and created an evaluation methodology to compare manual review against AI-assisted gap analysis using benchmark API cases.
```

This sounds stronger than simply saying:

```text
I used AI to generate documentation.
```

The experiment shows engineering maturity, measurement thinking, and research discipline.

---

## What This Phase Does Not Include

This phase does not include:

* Running the experiment
* Creating the actual benchmark files
* Implementing gap detection logic
* Calculating live metrics
* Writing code
* Parsing Excel files
* Parsing Node-RED JSON
* Generating Go code

Those tasks will come later in the challenge.

This phase only defines the purpose and motivation of the research experiment.

---

## Summary

The purpose of the research experiment is to evaluate whether AI-assisted artifact generation and gap analysis can improve enterprise API development.

The experiment will compare manual review against AI-assisted review using benchmark APIs, manual baselines, structured gap reports, and evaluation metrics.

This gives the project a measurable foundation for research, resume storytelling, technical writing, and future implementation.
