# Day 20 — Expected Results and Limitations

## Purpose

This document defines the expected results and limitations for the research paper in the 50-Day AI Software Engineer Challenge.

The purpose of this document is to clearly describe what the project expects to demonstrate and what it does not claim to solve.

This is important because the research paper should be realistic, honest, and defensible.

The project should avoid overclaiming that AI can fully replace engineers or generate production-ready enterprise APIs without review. Instead, the expected result should focus on AI-assisted workflow synthesis, validation, review, and artifact readiness.

---

## Research Context

The project proposes an AI-assisted workflow synthesis framework for enterprise API development.

The framework converts unstructured or semi-structured API requirements into structured workflow representations.

The workflow representations are then validated using:

* validation checklists
* workflow quality metrics
* failure categories
* ground truth workflows
* manual baseline comparison
* human review and correction

The expected results should show whether this approach can reduce manual effort and improve workflow structure while still requiring validation.

---

## Main Expected Result

The main expected result is:

```text
AI-assisted workflow synthesis can reduce the initial effort required to convert enterprise API requirements into structured workflow artifacts, while validation and human review help preserve correctness and reduce unsupported logic.
```

This result is realistic because it does not claim full automation.

It positions AI as an assistant that speeds up early workflow drafting and creates reviewable artifacts.

---

## Expected Result 1 — Reduced Initial Workflow Drafting Time

## Description

The AI-assisted process is expected to reduce the time required to create the first workflow draft.

In the manual process, the reviewer must read the requirement, extract details, organize workflow steps, identify mappings, and document error handling manually.

In the AI-assisted process, the AI can generate a structured initial draft faster once the requirement is provided.

---

## Expected Evidence

The project expects to compare:

```text
Manual Initial Drafting Time
vs
AI Initial Drafting Time
```

The AI-assisted process is expected to show a reduction in initial workflow drafting time.

---

## Example Expected Finding

```text
The AI-assisted process reduced initial workflow drafting time by 50–80% compared to the manual process across selected evaluation samples.
```

This number should be updated later based on actual evaluation results.

---

## Why This Matters

Time reduction is one of the most practical benefits of the project.

Enterprise teams often spend significant effort converting requirement documents into structured artifacts.

If AI can reduce this early drafting effort, engineers can spend more time reviewing, validating, and refining the workflow instead of starting from scratch.

---

## Expected Result 2 — Improved Structure Consistency

## Description

The AI-assisted process is expected to produce workflow outputs with more consistent structure when guided by a fixed prompt and schema.

Manual workflows may vary based on the reviewer’s style, experience, and documentation habits.

AI-generated workflows can be prompted to follow the same sections every time.

---

## Expected Evidence

The project expects to compare:

* structure consistency score
* presence of required workflow sections
* consistency of step IDs
* consistency of validation sections
* consistency of response mapping sections
* consistency of ambiguity notes

---

## Example Expected Finding

```text
AI-generated workflows followed the expected workflow structure more consistently than manually drafted workflows, especially in metadata, validation, response mapping, and artifact readiness sections.
```

---

## Why This Matters

Consistency is important because the workflow is intended to support downstream artifact generation.

A workflow that follows a predictable structure is easier to validate, compare, and transform into Excel, Node-RED, tests, documentation, or code later in the roadmap.

---

## Expected Result 3 — Comparable Corrected Workflow Quality

## Description

The initial AI-generated workflow may not always be as accurate as a manually created workflow, especially for complex business rules.

However, after applying the validation framework and human correction, the corrected AI workflow is expected to become close to the corrected manual workflow in quality.

---

## Expected Evidence

The project expects to compare:

```text
Manual Corrected Workflow Score
vs
AI Corrected Workflow Score
```

The corrected AI workflow is expected to improve significantly after validation.

---

## Example Expected Finding

```text
Initial AI workflows required correction for missing error handling, incomplete traceability, or business rule gaps. After validation and correction, AI-assisted workflows reached quality scores comparable to manually corrected workflows.
```

---

## Why This Matters

This result supports the idea that AI can provide a strong starting point, but validation is required before using the output for downstream artifacts.

The research claim should focus on the full AI-assisted process, not raw AI output alone.

---

## Expected Result 4 — Strong Requirement Coverage for Well-Structured Inputs

## Description

The AI-assisted process is expected to perform better when the source requirement is clear and well-structured.

If the requirement clearly defines endpoint details, headers, body fields, validations, business rules, downstream services, and response expectations, the AI should capture most of the important elements.

---

## Expected Evidence

The project expects to measure:

* requirement coverage score
* validation accuracy score
* response mapping accuracy score
* business rule coverage score

---

## Example Expected Finding

```text
AI-generated workflows achieved higher requirement coverage when source requirements contained clear endpoint, request, response, and business rule sections.
```

---

## Why This Matters

This shows that source requirement quality affects AI output quality.

It also supports the practical recommendation that enterprise teams should provide better requirement templates when using AI-assisted workflow generation.

---

## Expected Result 5 — Higher Risk on Ambiguous Requirements

## Description

The AI-assisted process is expected to struggle more with ambiguous or incomplete requirements.

If the requirement says things like:

```text
Use standard headers.
Handle errors appropriately.
Call the customer service.
Return customer details.
```

without defining exact fields, errors, or mappings, the AI may either produce vague output or guess unsupported behavior.

---

## Expected Evidence

The project expects to measure:

* ambiguity handling score
* hallucination control score
* unsupported logic count
* review correction count

---

## Example Expected Finding

```text
Ambiguous requirement samples had a higher rate of unsupported assumptions and required more human review than clearly structured requirement samples.
```

---

## Why This Matters

This reinforces the importance of ambiguity detection.

The framework should mark unclear requirements instead of silently converting assumptions into workflow logic.

---

## Expected Result 6 — Validation Improves AI Workflow Reliability

## Description

The validation framework is expected to improve the reliability of AI-generated workflows.

Initial AI outputs may have gaps, but validation checklists, quality metrics, and failure categories can help identify those gaps.

---

## Expected Evidence

The project expects to compare:

```text
AI Initial Workflow Score
vs
AI Corrected Workflow Score
```

The corrected AI score should be higher after validation and review.

---

## Example Expected Finding

```text
Applying the validation framework improved AI-generated workflow quality by identifying missing validation rules, incomplete error handling, incorrect mappings, and unsupported assumptions.
```

---

## Why This Matters

This is a key research point.

The project is not simply using AI to generate artifacts.

It is proposing a validation-driven AI-assisted process.

The validation layer is what makes the approach safer and more defensible.

---

## Expected Result 7 — Human Review Remains Necessary

## Description

The project is expected to show that human review remains necessary.

AI can assist in workflow drafting, but humans are still needed to resolve ambiguity, verify business logic, approve mappings, and confirm artifact readiness.

---

## Expected Evidence

The project expects to record:

* correction count
* ambiguity notes
* hallucinated logic count
* business rule gap count
* reviewer approval status

---

## Example Expected Finding

```text
Human review was necessary to resolve ambiguous requirements, remove unsupported logic, and confirm business rule correctness before workflows could be marked artifact-ready.
```

---

## Why This Matters

This prevents overclaiming.

The paper should clearly state that the framework supports engineers rather than replacing them.

---

## Expected Result 8 — Better Reviewability Through Structured Workflow Artifacts

## Description

The AI-assisted workflow representation is expected to make review easier because it organizes requirement details into predictable sections.

Instead of reviewing a long paragraph, reviewers can inspect:

* endpoint
* headers
* request body
* validations
* business rules
* downstream services
* database interactions
* workflow steps
* response mapping
* error handling
* traceability
* ambiguity notes

---

## Expected Evidence

The project expects to measure:

* reviewability score
* structure consistency score
* traceability score
* validation checklist completion

---

## Example Expected Finding

```text
Structured workflow artifacts made it easier to identify missing validations, response mapping gaps, and unsupported assumptions during review.
```

---

## Why This Matters

Reviewability is practical enterprise value.

Even when AI output is imperfect, a structured draft can reduce the effort needed to inspect and correct it.

---

## Expected Result 9 — Artifact Readiness Improves After Correction

## Description

The initial AI-generated workflow may not always be ready for downstream artifact generation.

However, after validation and correction, the workflow is expected to become more artifact-ready.

Artifact-ready means the workflow has enough structure to support:

* Excel intake generation
* Node-RED flow generation
* test generation
* documentation generation
* Go code generation after Day 25

---

## Expected Evidence

The project expects to compare:

```text
AI Initial Artifact Readiness Score
vs
AI Corrected Artifact Readiness Score
```

---

## Example Expected Finding

```text
After validation and correction, AI-assisted workflows contained enough structured detail to support downstream artifact generation planning.
```

---

## Why This Matters

Artifact readiness is one of the main goals of the project.

The validated workflow is the bridge between requirement understanding and future artifact generation.

---

## Expected Result 10 — Common AI Failure Patterns Can Be Identified

## Description

The project expects to identify recurring types of AI workflow generation failures.

These may include:

* missing error handling
* incomplete traceability
* generic validation logic
* missing business rules
* incorrect response mapping
* unsupported authorization assumptions
* vague downstream service mapping
* weak ambiguity handling

---

## Expected Evidence

The project expects to classify failures using the Day 17 failure categories.

---

## Example Expected Finding

```text
The most common AI workflow generation failures were incomplete error handling, weak traceability, and missing business rule details in complex requirements.
```

---

## Why This Matters

Failure analysis can be used to improve prompts, schemas, validation rules, and future automation.

This makes the system iterative and research-ready.

---

# Limitations

The project should clearly explain its limitations.

Honest limitations make the research stronger.

---

## Limitation 1 — Dataset Size May Be Limited

## Description

The evaluation dataset for the 50-day project may contain a limited number of curated API requirement samples.

A small dataset can show early evidence, but it may not represent every enterprise API scenario.

---

## Impact

The results may not generalize to all enterprise environments.

---

## Mitigation

The paper should clearly report the dataset size and scope.

Future work can expand the dataset with more API patterns, domains, and complexity levels.

---

## Limitation 2 — Dataset Samples Are Curated

## Description

The dataset samples may be curated or sanitized versions of enterprise API patterns.

They may not fully capture the complexity of production systems.

---

## Impact

The evaluation may be useful for controlled research, but real-world production requirements may introduce additional challenges.

---

## Mitigation

The project should explain that the dataset is intended as an initial benchmark.

Future work can include more production-like and independently reviewed samples.

---

## Limitation 3 — AI Output Depends on Prompt Quality

## Description

The quality of AI-generated workflows depends heavily on the prompt used.

A strong prompt may produce better outputs, while a weak prompt may produce incomplete or inconsistent workflows.

---

## Impact

Results may vary if prompt templates change.

---

## Mitigation

The project should record prompt versions and use consistent prompts during evaluation.

Prompt sensitivity can be explored in future work.

---

## Limitation 4 — Human Review Is Still Required

## Description

The framework does not remove the need for human review.

Humans are still needed to verify business logic, resolve ambiguity, approve mappings, and confirm final workflow correctness.

---

## Impact

The system should not be treated as a fully autonomous API development tool.

---

## Mitigation

The paper should position the system as AI-assisted, not AI-autonomous.

---

## Limitation 5 — Ambiguous Requirements Remain Difficult

## Description

AI cannot reliably resolve missing or unclear business requirements without additional context.

If the source requirement is ambiguous, the AI may guess incorrectly unless instructed to mark ambiguity.

---

## Impact

Ambiguous requirements may produce lower quality workflows or require more human correction.

---

## Mitigation

The framework includes ambiguity notes and hallucination detection.

Future work can add clarification-question generation or requirement gap reports.

---

## Limitation 6 — Domain Knowledge May Be Needed

## Description

Some enterprise APIs rely on domain-specific business knowledge.

The AI may not understand internal business rules, legacy system behavior, or organization-specific conventions unless they are provided in the requirement.

---

## Impact

AI-generated workflows may miss implicit domain logic.

---

## Mitigation

The framework should require explicit source references and avoid unsupported assumptions.

Domain-specific context can be added through controlled requirement templates or knowledge bases in future work.

---

## Limitation 7 — Ground Truth Creation Requires Manual Effort

## Description

Ground truth workflows must be manually created or reviewed.

This requires time and subject matter knowledge.

---

## Impact

The evaluation process depends on the quality of the ground truth workflows.

If the ground truth is incomplete or biased, the evaluation results may be unreliable.

---

## Mitigation

Ground truth workflows should follow the review process defined on Day 18 and should be marked approved before evaluation.

---

## Limitation 8 — Manual Baseline May Be Biased

## Description

The manual workflow baseline may be influenced by reviewer experience or prior knowledge of the requirement.

If the same person creates the ground truth and manual workflow, the manual workflow may appear stronger.

---

## Impact

Manual vs AI comparison may not be fully independent.

---

## Mitigation

The paper should acknowledge this limitation.

Future work can include independent reviewers or blind evaluation.

---

## Limitation 9 — The Framework Does Not Yet Prove Code Correctness

## Description

The current research focus is workflow synthesis and validation.

It does not yet prove that generated code will be correct.

Code generation is planned later in the roadmap after workflow validation foundations are completed.

---

## Impact

The paper should not claim end-to-end production code generation correctness.

---

## Mitigation

The paper should clearly state that workflow validation is an intermediate step toward future artifact generation.

Future work can evaluate generated Excel, Node-RED, tests, and Go code.

---

## Limitation 10 — Evaluation Is Domain-Specific

## Description

The project focuses on enterprise API development and modernization patterns.

The results may not directly apply to other software engineering domains such as frontend UI generation, mobile apps, embedded systems, or machine learning pipelines.

---

## Impact

The findings are strongest for backend API workflow synthesis.

---

## Mitigation

The paper should define the scope clearly.

Future work can test the method on additional software domains.

---

# Expected Results Summary Table

| Expected Result                              | Measurement Area                    |
| -------------------------------------------- | ----------------------------------- |
| Reduced initial drafting time                | Time reduction percentage           |
| Improved structure consistency               | Structure consistency score         |
| Comparable corrected quality                 | Corrected workflow quality score    |
| Strong coverage for clear inputs             | Requirement coverage score          |
| Higher risk on ambiguous inputs              | Ambiguity and hallucination metrics |
| Validation improves reliability              | Initial vs corrected AI score       |
| Human review remains necessary               | Correction count and review notes   |
| Better reviewability                         | Reviewability score                 |
| Improved artifact readiness after correction | Artifact readiness score            |
| Common AI failure patterns can be identified | Failure category analysis           |

---

# Limitations Summary Table

| Limitation                      | Impact                                             | Mitigation                                    |
| ------------------------------- | -------------------------------------------------- | --------------------------------------------- |
| Limited dataset size            | Results may not generalize broadly                 | Report dataset size and expand in future work |
| Curated samples                 | May not fully represent production complexity      | Add more production-like samples later        |
| Prompt sensitivity              | AI output may vary                                 | Use consistent prompt versions                |
| Human review required           | Not fully autonomous                               | Frame as AI-assisted                          |
| Ambiguous requirements          | AI may guess incorrectly                           | Mark ambiguity and use validation             |
| Domain knowledge needed         | Implicit rules may be missed                       | Require explicit source references            |
| Ground truth effort             | Evaluation requires manual review                  | Use approval process                          |
| Manual baseline bias            | Comparison may be influenced by reviewer knowledge | Use blind review later                        |
| Code correctness not proven yet | Cannot claim full implementation correctness       | Limit claim to workflow synthesis             |
| Domain-specific evaluation      | May not apply to all software domains              | Define scope clearly                          |

---

## Safe Interpretation of Results

The paper should interpret results carefully.

A safe interpretation is:

```text
The AI-assisted process is expected to reduce initial workflow drafting effort and improve structure consistency. However, validation and human review remain necessary to ensure correctness, resolve ambiguity, and remove unsupported assumptions.
```

Avoid saying:

```text
AI fully automates enterprise API development.
```

Use:

```text
AI assists enterprise API workflow synthesis by creating structured drafts that can be validated, corrected, and prepared for downstream artifact generation.
```

---

## Expected Research Conclusion

The expected research conclusion is:

```text
The proposed framework demonstrates that AI-assisted workflow synthesis can support enterprise API development by reducing early-stage documentation and workflow drafting effort. The framework is most effective when AI generation is paired with validation, quality metrics, failure classification, ground truth comparison, and human review.
```

---

## How This Supports LinkedIn and Medium Content

This document also helps prepare the public content planned for Day 21 and Day 22.

The public message should be:

```text
I am not trying to make AI replace engineers.

I am building a validation-driven workflow synthesis process where AI helps convert messy enterprise API requirements into structured, reviewable workflow artifacts before code generation.
```

This is a clear and professional story for LinkedIn and Medium.

---

## Summary

This document defines the expected results and limitations for the research paper.

The expected results focus on:

* reduced drafting time
* improved structure consistency
* comparable corrected workflow quality
* stronger reviewability
* improved artifact readiness
* validation-driven reliability
* identifiable failure patterns

The limitations clarify that:

* the dataset may be limited
* prompt quality matters
* human review remains necessary
* ambiguous requirements remain difficult
* code correctness is not yet proven
* the evaluation is focused on enterprise API workflows

Day 20 Phase 4 establishes the honest and defensible results framing needed for the research paper.
