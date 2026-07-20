# Day 50 – Phase 6

# Publication and Evaluation Execution Plan

## Research Project

**AI-Assisted API Factory and API Migration Analyzer**

## Working Paper Title

**AI-Assisted API Modernization: A Rule-Based Architecture for Requirement Extraction, API Generation, and Migration Analysis**

---

# 1. Phase 6 Objective

The objective of Day 50 Phase 6 is to define the post-roadmap execution plan required to move the current research preparation package toward:

* A reproducible empirical evaluation.
* A verified literature review.
* A completed research manuscript.
* A public technical preprint.
* A conference, workshop, or journal submission.
* A defensible portfolio and research profile.

The 50-Day Roadmap produced:

* Two connected software prototypes.
* A research problem.
* Research questions.
* Hypotheses.
* An evaluation methodology.
* Dataset templates.
* Ground-truth templates.
* Publication-asset plans.
* A literature-review plan.
* An initial manuscript draft.

The roadmap did not complete the empirical experiments or publication process.

This document defines the remaining work in an ordered and controlled manner.

---

# 2. Recommended File Location

```text
docs/day50/publication_execution_plan.md
```

---

# 3. Current Research Status

| Research Area                     | Status            |
| --------------------------------- | ----------------- |
| Research problem                  | Completed         |
| Research questions                | Completed         |
| Hypotheses                        | Completed         |
| Proposed architecture             | Completed         |
| API Factory prototype             | Implemented       |
| Migration Analyzer prototype      | Implemented       |
| Evaluation methodology            | Completed         |
| Dataset design                    | Completed         |
| Ground-truth templates            | Completed         |
| Literature-search strategy        | Completed         |
| Initial manuscript draft          | Completed         |
| Final repository validation       | Pending execution |
| Evaluation dataset implementation | Pending           |
| Ground-truth population           | Pending           |
| Experiment scripts                | Pending           |
| AI-only experiments               | Pending           |
| Hybrid experiments                | Pending           |
| Manual baseline                   | Pending           |
| Human reviewer study              | Pending           |
| Statistical analysis              | Pending           |
| Literature review                 | Pending execution |
| Final results section             | Pending           |
| Final figures and charts          | Pending           |
| Preprint                          | Pending           |
| Venue submission                  | Pending           |

---

# 4. Publication Readiness Definition

The research should be considered publication ready only when:

* The evaluated repository version is frozen.
* All selected evaluation cases are implemented.
* Ground truth is independently reviewed.
* Manual, AI-only, and hybrid experiments are completed.
* Failed runs are preserved.
* Results are calculated from versioned data.
* Human review is completed where required.
* The literature review is completed.
* Every factual claim is cited or experimentally supported.
* Figures and tables use actual data.
* Threats and limitations are documented.
* The paper is formatted for a selected venue.
* Author, license, privacy, and disclosure requirements are satisfied.

---

# 5. Execution Workstreams

The remaining work is divided into nine workstreams.

| Workstream | Name                                        |
| ---------- | ------------------------------------------- |
| W1         | Repository Freeze and Reproducibility       |
| W2         | Evaluation Dataset Implementation           |
| W3         | Ground Truth and Expert Review              |
| W4         | Experiment Automation and Pilot             |
| W5         | Main Experiment Execution                   |
| W6         | Human Reviewer Evaluation                   |
| W7         | Literature Review and Citation Verification |
| W8         | Results, Figures, and Manuscript Revision   |
| W9         | Preprint and Venue Submission               |

---

# 6. Workstream 1 – Repository Freeze and Reproducibility

## 6.1 Objective

Create a stable version of both projects that will be used for the research evaluation.

The evaluated implementation must not change during the main experiment unless a critical defect invalidates the study.

---

## 6.2 Required Actions

For both repositories:

1. Complete the Day 50 repository audit.
2. Run final technical validation.
3. Fix blocking lint, test, type, and build errors.
4. Record unresolved non-blocking limitations.
5. Remove confidential or proprietary content.
6. Confirm synthetic sample data.
7. Record dependency versions.
8. Record Node.js and npm versions.
9. Record the final Git commit.
10. Create a research evaluation tag.

Suggested tags:

```text
api-factory-research-v1
migration-analyzer-research-v1
```

---

## 6.3 Freeze Record Template

```md
# Research Implementation Freeze

## Project

API Factory or API Migration Analyzer

## Repository

TBD

## Branch

TBD

## Commit

TBD

## Tag

TBD

## Node.js Version

TBD

## npm Version

TBD

## Validation Status

TBD

## Known Limitations

TBD

## Freeze Date

TBD
```

---

## 6.4 Reproducibility Manifest

Recommended file:

```text
evaluation/manifests/implementation-manifest.json
```

Template:

```json
{
  "studyVersion": "1.0.0",
  "apiFactory": {
    "repository": "TBD",
    "commit": "TBD",
    "tag": "api-factory-research-v1"
  },
  "migrationAnalyzer": {
    "repository": "TBD",
    "commit": "TBD",
    "tag": "migration-analyzer-research-v1"
  },
  "runtime": {
    "node": "TBD",
    "npm": "TBD",
    "operatingSystem": "TBD"
  },
  "schemas": {
    "requirementSchemaVersion": "1.0.0",
    "migrationSchemaVersion": "1.0.0"
  },
  "ruleSetVersion": "1.0.0",
  "targetArchitectureVersion": "1.0.0"
}
```

---

## 6.5 Workstream 1 Completion Criteria

* [ ] Both repositories pass required release gates.
* [ ] Blocking defects are resolved.
* [ ] Non-blocking defects are recorded.
* [ ] Commit hashes are frozen.
* [ ] Research tags are created.
* [ ] Dependency versions are recorded.
* [ ] Implementation manifest is created.
* [ ] Public-safe sample data is confirmed.

---

# 7. Workstream 2 – Evaluation Dataset Implementation

## 7.1 Objective

Convert the Day 49 case designs into executable evaluation inputs.

---

## 7.2 Minimum Evaluation Cases

The recommended initial study uses:

* C1 – Simple Product API.
* C2 – Customer Validation API.
* C5 – Legacy Order API.
* C7 – Secure Layered Account API.
* C8 – Incomplete Shipment Requirement.

This combination provides:

* Basic extraction.
* Complex validation.
* Legacy modernization.
* False-positive control.
* Missing-information handling.

---

## 7.3 Full Evaluation Cases

The full study may include:

* C1 – Simple Product API.
* C2 – Customer Validation API.
* C3 – Wallet Transaction API.
* C4 – Identity Integration API.
* C5 – Legacy Order API.
* C6 – Partially Modernized Discount API.
* C7 – Secure Layered Account API.
* C8 – Incomplete Shipment Requirement.

---

## 7.4 Dataset Structure

```text
evaluation/
└── datasets/
    ├── requirements/
    │   ├── C1/
    │   ├── C2/
    │   └── C8/
    └── migration/
        ├── C3/
        ├── C4/
        ├── C5/
        ├── C6/
        └── C7/
```

Each case should contain:

```text
case-id/
├── manifest.json
├── input.md
├── source-artifacts/
├── expected-scope.md
└── notes.md
```

---

## 7.5 Requirement Case Manifest

```json
{
  "caseId": "C1",
  "name": "Simple Product API",
  "caseType": "requirement-extraction",
  "version": "1.0.0",
  "inputFile": "input.md",
  "expectedComplexity": "low",
  "includedCapabilities": [
    "endpoint-extraction",
    "field-extraction",
    "validation-extraction",
    "error-extraction"
  ],
  "excludedCapabilities": [],
  "containsIntentionalAmbiguity": false
}
```

---

## 7.6 Migration Case Manifest

```json
{
  "caseId": "C5",
  "name": "Legacy Order API",
  "caseType": "migration-analysis",
  "version": "1.0.0",
  "targetArchitectureId": "layered-cloud-api",
  "sourceArtifactDirectory": "source-artifacts",
  "expectedComplexity": "high",
  "includedCapabilities": [
    "endpoint-evidence",
    "layering",
    "validation",
    "configuration",
    "error-handling",
    "testing",
    "observability"
  ]
}
```

---

## 7.7 Dataset Quality Rules

Each case must:

* Use synthetic names.
* Avoid employer-owned code.
* Avoid confidential requirements.
* Be understandable without external context.
* Include sufficient detail to determine ground truth.
* Include intentional negative cases.
* Have stable file and artifact identifiers.
* Avoid accidental hints about expected findings.
* Be version controlled.
* Remain unchanged during the main experiment.

---

## 7.8 Dataset Validation

Create a dataset validator that checks:

* Required manifest exists.
* Declared files exist.
* Case ID is unique.
* Version is present.
* Source artifacts have unique IDs.
* Target architecture exists.
* Input checksum can be generated.
* No case contains forbidden confidential terms.
* No ground-truth file is exposed to the tested approach.

---

## 7.9 Workstream 2 Completion Criteria

* [ ] Minimum study cases are implemented.
* [ ] Case manifests are valid.
* [ ] Synthetic input files are complete.
* [ ] Source artifacts are complete.
* [ ] Dataset validator passes.
* [ ] Dataset version is frozen.
* [ ] Dataset checksums are recorded.

---

# 8. Workstream 3 – Ground Truth and Expert Review

## 8.1 Objective

Create an independently reviewed reference answer for every evaluation case.

---

## 8.2 Requirement Ground Truth

For requirement cases, record:

* Expected endpoints.
* Expected methods.
* Expected paths.
* Expected headers.
* Expected request fields.
* Expected response fields.
* Expected validation rules.
* Expected business rules.
* Expected errors.
* Expected missing information.
* Non-supported interpretations.

---

## 8.3 Migration Ground Truth

For migration cases, record:

* Expected endpoint evidence.
* Expected capability evidence.
* Expected architecture-rule outcomes.
* Expected gaps.
* Expected risks.
* Acceptable recommendation categories.
* Expected non-findings.
* Non-applicable rules.
* Expected readiness range.

---

## 8.4 Requirement Ground-Truth Template

```json
{
  "caseId": "C1",
  "version": "1.0.0",
  "endpoints": [],
  "fields": [],
  "validationRules": [],
  "businessRules": [],
  "errors": [],
  "missingInformation": [],
  "unsupportedInterpretations": []
}
```

---

## 8.5 Migration Ground-Truth Template

```json
{
  "caseId": "C5",
  "version": "1.0.0",
  "endpointEvidence": [],
  "capabilityEvidence": [],
  "ruleOutcomes": [],
  "gaps": [],
  "risks": [],
  "acceptableRecommendationCategories": [],
  "nonFindings": [],
  "nonApplicableRuleIds": [],
  "expectedReadinessRange": {
    "minimum": 0,
    "maximum": 100
  }
}
```

---

## 8.6 Review Procedure

Recommended review process:

1. Primary reviewer creates the initial ground truth.
2. Secondary reviewer independently reviews the case.
3. Differences are recorded.
4. Reviewers resolve disagreements.
5. Final ground truth is frozen.
6. Version and signatures are recorded.

Where only one expert is available:

* Perform the first review.
* Wait before the second review.
* Re-review from the documented criteria.
* Record that the same reviewer performed both passes.
* State the limitation in the paper.

---

## 8.7 Ground-Truth Decision Log

Recommended file:

```text
evaluation/ground-truth/decision-log.md
```

Template:

```md
## Ground-Truth Decision

- Decision ID:
- Case ID:
- Item:
- Initial interpretation:
- Alternative interpretation:
- Final decision:
- Rationale:
- Reviewers:
- Date:
```

---

## 8.8 Matching Rules

Matching rules must be frozen before the main experiment.

They should define:

* Case sensitivity.
* HTTP method normalization.
* Path normalization.
* Field-name normalization.
* Type equivalence.
* Rule semantic equivalence.
* Gap equivalence.
* Risk equivalence.
* Recommendation category matching.
* Duplicate handling.
* Partial-credit handling.

---

## 8.9 Workstream 3 Completion Criteria

* [ ] Ground truth exists for every selected case.
* [ ] Ground truth has been reviewed.
* [ ] Disagreements are documented.
* [ ] Matching rules are frozen.
* [ ] Ground-truth version is recorded.
* [ ] Evaluation approaches cannot access ground truth.
* [ ] Ground-truth checksums are recorded.

---

# 9. Workstream 4 – Experiment Automation and Pilot

## 9.1 Objective

Build the scripts required to execute, validate, compare, and record experiments reproducibly.

---

## 9.2 Required Experiment Tools

Implement tools for:

* Experiment manifest validation.
* Run directory creation.
* Prompt version loading.
* Input checksum generation.
* AI-only execution.
* Hybrid execution.
* Raw output preservation.
* Schema validation.
* Ground-truth comparison.
* Metric calculation.
* Failure recording.
* Result aggregation.
* Blind-review package creation.

---

## 9.3 Recommended Scripts

```text
evaluation/scripts/
├── validate-dataset.mjs
├── validate-ground-truth.mjs
├── create-run.mjs
├── run-ai-only.mjs
├── run-hybrid.mjs
├── validate-output.mjs
├── compare-requirements.mjs
├── compare-migration.mjs
├── calculate-metrics.mjs
├── create-review-package.mjs
└── aggregate-results.mjs
```

---

## 9.4 Experiment Manifest

```json
{
  "experimentId": "EXP-C1-HYBRID-001",
  "caseId": "C1",
  "approach": "hybrid",
  "runNumber": 1,
  "datasetVersion": "1.0.0",
  "groundTruthVersion": "1.0.0",
  "promptVersion": "1.0.0",
  "model": "TBD",
  "modelParameters": {
    "temperature": "TBD"
  },
  "schemaVersion": "1.0.0",
  "ruleSetVersion": "1.0.0",
  "targetArchitectureVersion": "1.0.0"
}
```

---

## 9.5 Run Directory

```text
evaluation/runs/
└── EXP-C1-HYBRID-001/
    ├── manifest.json
    ├── input/
    ├── raw-output/
    ├── normalized-output/
    ├── validation/
    ├── comparison/
    ├── timing.json
    ├── failure.json
    └── review-package/
```

---

## 9.6 Pilot Case

Use C1 as the pilot.

The pilot verifies:

* Dataset loading.
* Prompt loading.
* Output capture.
* Schema validation.
* Ground-truth comparison.
* Metric correctness.
* Timing capture.
* Failure handling.
* Review package generation.

---

## 9.7 Pilot Freeze Rule

After the pilot:

* Correct defects in scripts.
* Clarify matching rules.
* Finalize output schemas.
* Freeze the experiment process.
* Do not modify comparison logic during the main experiment unless a defect invalidates the study.

All post-pilot changes must be versioned and documented.

---

## 9.8 Workstream 4 Completion Criteria

* [ ] All experiment scripts exist.
* [ ] Scripts preserve nonzero exit codes.
* [ ] Raw outputs are preserved.
* [ ] Failure records are generated.
* [ ] Metrics are tested with known examples.
* [ ] C1 pilot completes.
* [ ] Comparison logic is reviewed.
* [ ] Experiment process is frozen.

---

# 10. Workstream 5 – Main Experiment Execution

## 10.1 Objective

Execute the manual, AI-only, and hybrid approaches under controlled conditions.

---

## 10.2 Minimum Execution Matrix

| Case      | Manual | AI-Only | Hybrid | Deterministic Rerun |
| --------- | -----: | ------: | -----: | ------------------: |
| C1        |      1 |       3 |      3 |                   1 |
| C2        |      1 |       3 |      3 |                   1 |
| C5        |      1 |       3 |      3 |                   2 |
| C7        |      1 |       3 |      3 |                   1 |
| C8        |      1 |       3 |      3 |                   1 |
| **Total** |  **5** |  **15** | **15** |               **6** |

Planned total:

```text
41 executions
```

---

## 10.3 Manual Baseline Procedure

The manual baseline should:

* Use the same source input.
* Use the same output schema.
* Be completed without viewing AI or hybrid output.
* Record start and end times.
* Record corrections.
* Preserve the completed artifact.
* Record reviewer experience level.

---

## 10.4 AI-Only Procedure

The AI-only approach should:

* Receive the same input.
* Receive the frozen prompt.
* Receive the required output schema.
* Not use deterministic rule evaluation beyond parsing required for recording.
* Run three times per case.
* Preserve raw outputs.
* Record invalid or incomplete runs.

---

## 10.5 Hybrid Procedure

The hybrid approach should:

* Use the same input.
* Use the frozen prompt where AI is involved.
* Normalize output.
* Validate output.
* Run deterministic analysis.
* Preserve evidence links.
* Record validation defects.
* Run three times per case.

---

## 10.6 Execution Order Control

To reduce ordering effects:

* Randomize or alternate approach order.
* Do not always run manual first or hybrid last.
* Record the actual order.
* Avoid reviewing previous outputs during a new run.
* Use separate run directories.

---

## 10.7 Timing Rules

Measure:

* Preparation time.
* Machine execution time.
* Review time.
* Correction time.
* Total completion time.

Pause the timer only for documented external interruptions.

---

## 10.8 Failed Runs

A failed run must remain in the dataset.

Examples:

* Timeout.
* Invalid JSON.
* Missing required output.
* Schema failure.
* Empty output.
* Rule-engine failure.
* Runtime exception.

Do not rerun and silently replace the failed result.

A replacement run may be executed, but the original failure must remain recorded.

---

## 10.9 Workstream 5 Completion Criteria

* [ ] All planned manual runs are complete.
* [ ] All AI-only runs are complete.
* [ ] All hybrid runs are complete.
* [ ] Deterministic reruns are complete.
* [ ] Timing data is complete.
* [ ] Failed runs are preserved.
* [ ] No approach accessed ground truth.
* [ ] Run manifests are valid.
* [ ] Result aggregation completes.

---

# 11. Workstream 6 – Human Reviewer Evaluation

## 11.1 Objective

Evaluate correctness, explainability, evidence quality, actionability, and confidence through human review.

---

## 11.2 Recommended Reviewer Profiles

Preferred reviewers:

* Software architect.
* Senior backend engineer.
* API platform engineer.
* Technical lead.
* Developer-tools engineer.
* Modernization consultant.

A reviewer should understand API architecture and software quality.

---

## 11.3 Minimum Reviewer Target

Recommended minimum:

```text
3 reviewers
```

Preferred:

```text
5–8 reviewers
```

When fewer reviewers are available, state the limitation clearly.

---

## 11.4 Blind Review

Where practical:

* Remove approach labels.
* Randomize output order.
* Use neutral output IDs.
* Do not identify manual, AI-only, or hybrid artifacts.
* Do not expose model names.

---

## 11.5 Review Dimensions

Use a five-point scale for:

* Correctness.
* Completeness.
* Explainability.
* Evidence quality.
* Recommendation actionability.
* Migration-task quality.
* Confidence.
* Perceived correction effort.

---

## 11.6 Reviewer Form

```md
# Review Form

## Review Package ID

TBD

## Case ID

TBD

## Ratings

- Correctness: 1–5
- Completeness: 1–5
- Explainability: 1–5
- Evidence quality: 1–5
- Recommendation actionability: 1–5
- Migration-task quality: 1–5
- Confidence: 1–5
- Correction effort: 1–5

## Unsupported Findings

TBD

## Missing Findings

TBD

## Recommended Corrections

TBD

## Overall Comments

TBD
```

---

## 11.7 Reviewer Instructions

Reviewers should:

* Judge the output against the supplied case.
* Mark unsupported findings.
* Mark missing important findings.
* Inspect evidence links.
* Evaluate recommendation usefulness.
* Avoid guessing which approach produced the output.
* Complete reviews independently.

---

## 11.8 Agreement Analysis

Calculate reviewer agreement where sample size permits.

Potential measures:

* Percentage agreement.
* Cohen’s kappa for two reviewers.
* Fleiss’ kappa for multiple reviewers.
* Intraclass correlation for rating scales.

The selected measure must match the review design.

---

## 11.9 Workstream 6 Completion Criteria

* [ ] Reviewers are recruited.
* [ ] Reviewer consent or acknowledgment is recorded.
* [ ] Blind packages are generated.
* [ ] Instructions are frozen.
* [ ] All assigned reviews are complete.
* [ ] Corrections are recorded.
* [ ] Agreement is calculated where appropriate.
* [ ] Reviewer limitations are documented.

---

# 12. Workstream 7 – Literature Review and Citation Verification

## 12.1 Objective

Complete the literature review defined during Day 49 Phase 6.

---

## 12.2 Research Areas

Search and review work on:

1. AI-assisted requirements engineering.
2. LLMs for software engineering.
3. AI-assisted API and code generation.
4. Model-driven and schema-first development.
5. Static analysis and architecture conformance.
6. Legacy modernization and cloud migration.
7. Explainable AI for developer tools.
8. Human-in-the-loop software engineering.

---

## 12.3 Literature Target

Minimum viable target:

```text
25–30 strong sources
```

Preferred target:

```text
30–45 verified references
```

---

## 12.4 Literature Execution Order

1. Search systematic reviews.
2. Search recent empirical studies.
3. Identify foundational papers.
4. Perform title screening.
5. Perform abstract screening.
6. Review full texts.
7. Complete quality scoring.
8. Perform backward snowballing.
9. Perform forward snowballing.
10. Populate the comparison matrix.
11. Verify every citation.
12. Rewrite the research-gap statement.

---

## 12.5 Source Quality Rules

Prefer:

* Peer-reviewed journals.
* Peer-reviewed conferences.
* Systematic reviews.
* Primary empirical studies.
* Official standards.

Use preprints when:

* The topic is highly recent.
* No peer-reviewed version exists.
* The source is labeled clearly as a preprint.

Avoid using vendor marketing as evidence for research performance.

---

## 12.6 Citation Verification

For each citation:

* Verify authors.
* Verify title.
* Verify year.
* Verify venue.
* Verify DOI or identifier.
* Read the relevant section.
* Confirm the source supports the claim.
* Record page or section.
* Record limitations.
* Add a verified BibTeX entry.

---

## 12.7 Literature Outputs

```text
docs/day49/literature/
├── search_log.md
├── candidate_papers.csv
├── selected_papers.csv
├── excluded_papers.csv
├── comparison_matrix.csv
├── citation_claims.csv
├── citation_verification.md
├── references.bib
└── citation_notes/
```

---

## 12.8 Workstream 7 Completion Criteria

* [ ] All eight research areas are searched.
* [ ] Candidate papers are tracked.
* [ ] Inclusion and exclusion decisions are recorded.
* [ ] Full-text notes exist for selected papers.
* [ ] Quality scores are complete.
* [ ] Comparison matrix is populated.
* [ ] Research gap is revised.
* [ ] Every manuscript claim has verified citations.
* [ ] Bibliography contains no uncited entries.

---

# 13. Workstream 8 – Results, Figures, and Manuscript Revision

## 13.1 Objective

Transform raw experiment and review data into accurate paper results.

---

## 13.2 Required Result Files

```text
evaluation/results/
├── accuracy/
│   ├── requirement-results.csv
│   ├── migration-results.csv
│   └── structural-validity.csv
├── validation/
│   └── defect-detection.csv
├── traceability/
│   └── traceability-results.csv
├── consistency/
│   └── consistency-results.csv
├── timing/
│   └── timing-results.csv
├── scoring/
│   └── scoring-results.csv
└── human-review/
    └── review-results.csv
```

---

## 13.3 Result Validation

Before generating charts:

* Recalculate totals.
* Verify denominators.
* Confirm failed runs are included.
* Confirm duplicate runs are not counted.
* Confirm missing values are represented.
* Confirm percentages use consistent scales.
* Verify timing units.
* Check result scripts with small known examples.
* Review outliers.

---

## 13.4 Statistical Analysis

Potential analysis includes:

* Mean.
* Median.
* Standard deviation.
* Range.
* Confidence intervals.
* Paired comparisons.
* Non-parametric tests for small samples.
* Effect sizes.

The exact method depends on sample size and study design.

Do not use significance tests that the dataset cannot support.

---

## 13.5 Required Charts

Generate charts for:

* Requirement precision and recall.
* Migration gap and risk performance.
* Structural validity.
* Traceability coverage.
* Workflow time.
* Human reviewer ratings.
* Repeated-run consistency.
* False positives.
* Validation defect detection.
* System versus expert readiness score.

---

## 13.6 Chart Generation Rules

* Use actual result CSV files.
* Preserve chart scripts.
* Use vector formats.
* Avoid three-dimensional charts.
* Use zero-based bar axes.
* State sample sizes.
* Define error bars.
* Ensure grayscale readability.
* Do not hide failed runs.
* Keep approach ordering consistent.

---

## 13.7 Manuscript Sections to Rewrite

After results are available, revise:

* Abstract.
* Introduction contribution wording.
* Related-work gap.
* Results.
* Discussion.
* Threats to validity.
* Limitations.
* Conclusion.

Remove all:

```text
TBD
[Results Pending]
[Citation Required]
```

before submission.

---

## 13.8 Results Writing Rules

The results section should:

* Present measured values.
* Avoid interpretation beyond the data.
* Report failures.
* Report variability.
* Report sample size.
* Separate primary and secondary findings.

The discussion should:

* Interpret results.
* Compare approaches.
* Explain failure patterns.
* Relate findings to prior work.
* Address hypotheses.
* Avoid universal claims.

---

## 13.9 Workstream 8 Completion Criteria

* [ ] Result files are generated.
* [ ] Metrics are independently checked.
* [ ] Statistical methods are appropriate.
* [ ] Charts are generated from versioned data.
* [ ] Tables match source CSV files.
* [ ] Results section is complete.
* [ ] Discussion is complete.
* [ ] Abstract is rewritten.
* [ ] Conclusion is rewritten.
* [ ] No placeholder remains.

---

# 14. Workstream 9 – Preprint and Venue Submission

## 14.1 Objective

Prepare the completed manuscript for public dissemination and formal submission.

---

## 14.2 Venue Categories

Potential venue types:

* Software-engineering conference.
* Software architecture conference.
* AI-for-software-engineering workshop.
* API or cloud engineering workshop.
* Developer-tools workshop.
* Applied AI conference.
* Software-engineering journal.
* Preprint repository.

Venue selection must be based on:

* Topic fit.
* Paper maturity.
* Page limit.
* Submission deadline.
* Review timeline.
* Open-access policy.
* Preprint policy.
* Artifact-evaluation option.
* Registration cost.
* Travel requirements.

---

## 14.3 Venue Selection Matrix

| Criterion              | Weight | Venue A | Venue B | Venue C |
| ---------------------- | -----: | ------: | ------: | ------: |
| Topic fit              |      5 |     TBD |     TBD |     TBD |
| Deadline feasibility   |      4 |     TBD |     TBD |     TBD |
| Page limit fit         |      3 |     TBD |     TBD |     TBD |
| Artifact track         |      4 |     TBD |     TBD |     TBD |
| Open-access policy     |      3 |     TBD |     TBD |     TBD |
| Reputation             |      4 |     TBD |     TBD |     TBD |
| Cost                   |      3 |     TBD |     TBD |     TBD |
| Travel requirement     |      2 |     TBD |     TBD |     TBD |
| Preprint compatibility |      3 |     TBD |     TBD |     TBD |

Do not select a venue only because it promises easy acceptance.

---

## 14.4 Preprint Preparation

Before posting a preprint:

* Confirm employer and ownership permissions.
* Remove confidential content.
* Confirm venue preprint policy.
* Include accurate author information.
* Include a version date.
* Include repository or artifact links where public.
* Label the work as a preprint.
* Avoid stating that it is peer reviewed.

---

## 14.5 Artifact Package

Recommended public artifact structure:

```text
research-artifact/
├── README.md
├── LICENSE
├── paper/
├── datasets/
├── ground-truth/
├── prompts/
├── schemas/
├── rule-sets/
├── experiment-scripts/
├── result-data/
├── chart-scripts/
└── reproducibility/
```

---

## 14.6 Reproducibility README

The artifact README should explain:

* System requirements.
* Installation.
* Dataset structure.
* Ground-truth structure.
* How to run the pilot.
* How to run an experiment.
* How to validate outputs.
* How to calculate results.
* How to generate charts.
* Expected execution costs.
* Known limitations.

---

## 14.7 Submission Checklist

* [ ] Venue scope matches the paper.
* [ ] Submission deadline is confirmed.
* [ ] Formatting template is applied.
* [ ] Page limit is satisfied.
* [ ] References are complete.
* [ ] Figures are readable.
* [ ] Tables fit.
* [ ] Author requirements are satisfied.
* [ ] Anonymization is correct where required.
* [ ] Conflict-of-interest information is complete.
* [ ] Artifact links comply with blind-review rules.
* [ ] AI-use disclosure is included if required.
* [ ] Ethics statement is included if required.
* [ ] Supplementary files are validated.
* [ ] Final PDF is reviewed.
* [ ] Submission confirmation is preserved.

---

# 15. Recommended Post-Roadmap Schedule

The following is a recommended eight-week execution plan.

## Week 1 – Repository Freeze

* Complete validation.
* Fix blockers.
* Freeze commits.
* Create research tags.
* Create implementation manifest.

## Week 2 – Dataset Implementation

* Implement minimum study cases.
* Validate manifests.
* Freeze dataset version.

## Week 3 – Ground Truth

* Populate ground truth.
* Complete secondary review.
* Freeze matching rules.

## Week 4 – Experiment Scripts and Pilot

* Implement scripts.
* Run C1 pilot.
* Verify metrics.
* Freeze experiment process.

## Week 5 – Main Experiments

* Complete manual runs.
* Complete AI-only runs.
* Complete hybrid runs.
* Preserve failures.

## Week 6 – Human Review and Literature

* Distribute blind review packages.
* Continue literature review.
* Populate citation matrix.

## Week 7 – Analysis and Writing

* Aggregate results.
* Generate tables and charts.
* Draft results and discussion.
* Rewrite abstract and conclusion.

## Week 8 – Preprint and Submission Package

* Proofread.
* Format for target venue.
* Prepare artifact package.
* Post preprint where permitted.
* Submit to the selected venue.

This schedule is adjustable. It is not a guarantee of completion or acceptance.

---

# 16. Minimum Publication Path

When resources are limited, use the following minimum path:

1. Freeze both repositories.
2. Implement C1, C2, C5, C7, and C8.
3. Complete one reviewed ground truth per case.
4. Execute the 41-run minimum study.
5. Recruit at least three reviewers.
6. Review at least 25 strong sources.
7. Report descriptive statistics.
8. Generate four primary charts.
9. Complete a workshop-length paper.
10. Release a preprint and reproducibility package where permitted.

---

# 17. Expanded Publication Path

For a stronger journal or conference paper:

* Use all eight cases.
* Add multiple real-world open-source API projects.
* Add AST-based source parsing.
* Add multiple target architectures.
* Recruit five to eight reviewers.
* Compare more than one AI model.
* Add prompt sensitivity analysis.
* Add ablation studies.
* Add runtime evidence.
* Add recommendation implementation validation.
* Perform stronger statistical analysis.

---

# 18. Ablation Study Plan

An ablation study can identify which hybrid components provide value.

Potential variants:

1. AI-only.
2. AI plus schema validation.
3. AI plus normalization and schema validation.
4. AI plus evidence extraction.
5. AI plus deterministic rule evaluation.
6. Complete hybrid system.

Possible measurements:

* Structural validity.
* False positives.
* Traceability.
* Consistency.
* Correction effort.

This study is optional for the first paper but valuable for a stronger follow-up.

---

# 19. Model Comparison Plan

A future study may compare:

* Different model providers.
* Different model sizes.
* Different prompting strategies.
* Deterministic-only processing.
* AI-assisted extraction with the same deterministic backend.

Required controls:

* Same cases.
* Same schemas.
* Same temperature policy.
* Same output limits.
* Same comparison logic.
* Same number of runs.

---

# 20. Cost Tracking

Record experiment costs when AI services are used.

Recommended fields:

```csv
run_id,model,input_tokens,output_tokens,estimated_cost,currency,notes
```

The paper may report:

* Cost per run.
* Cost per case.
* Total study cost.
* Cost versus human review time.

Do not report estimated costs as exact billing values unless verified.

---

# 21. Data Management Plan

Store:

* Synthetic datasets.
* Ground truth.
* Experiment manifests.
* Raw outputs.
* Validated outputs.
* Comparison results.
* Reviewer ratings.
* Aggregated data.
* Chart scripts.

Do not publicly store:

* Personal reviewer information.
* Confidential source code.
* Private credentials.
* Employer-owned documents.
* Non-redistributable papers.

---

# 22. Reviewer Privacy

When reviewers participate:

* Assign reviewer IDs.
* Do not publish personal information without consent.
* Store contact information separately.
* Publish only aggregated ratings where appropriate.
* Remove identifying free-text comments where necessary.

---

# 23. AI Use Disclosure

The final paper should disclose:

* Which models were used.
* Where AI was used in the system.
* Where AI was used in manuscript preparation.
* Which outputs were human reviewed.
* Which downstream processes were deterministic.
* Whether AI-generated text was edited by the author.

Follow the selected venue’s current AI-use policy.

---

# 24. Research Claim Controls

The final paper may claim only what is supported.

## Implementation-Supported Claims

* The prototypes were implemented.
* The architecture contains defined components.
* The analyzer generates configured artifact types.
* Evidence identifiers link findings to source artifacts.
* Tests exist for specified modules.

## Experiment-Supported Claims

Require measured evidence:

* Accuracy.
* Recall.
* False-positive reduction.
* Structural-validity improvement.
* Time reduction.
* Consistency.
* Reviewer confidence.
* Score agreement.

## Publication-Supported Claims

Require an actual record:

* Preprint posted.
* Paper submitted.
* Paper accepted.
* Paper published.
* Paper cited.

---

# 25. Risk Register

| Risk                        | Impact                 | Mitigation                                 |
| --------------------------- | ---------------------- | ------------------------------------------ |
| Dataset too small           | Weak generalization    | Add diverse cases and state scope          |
| Ground-truth bias           | Inflated results       | Secondary review and decision log          |
| Prompt tuning to test cases | Overfitting            | Freeze prompt before main runs             |
| Model changes               | Reproducibility loss   | Record exact model and date                |
| Failed reviewer recruitment | Weak human study       | Use minimum reviewers and state limitation |
| Playwright instability      | Limited UI evidence    | Use manual validation and document status  |
| Literature gap contradicted | Novelty claim weakened | Rewrite contribution honestly              |
| Confidential data exposure  | Legal and ethical risk | Use synthetic inputs and run privacy audit |
| Unsupported statistics      | Invalid conclusions    | Use descriptive analysis where appropriate |
| Venue mismatch              | Rejection risk         | Use venue selection matrix                 |
| Ownership uncertainty       | Blocks public release  | Confirm rights before publishing           |
| Experiment cost             | Incomplete runs        | Pilot costs and use minimum study first    |

---

# 26. Publication Decision Gates

## Gate 1 – Technical Freeze

Proceed only when:

* Required validation passes.
* Research commits are frozen.
* No confidential data remains.

## Gate 2 – Dataset Freeze

Proceed only when:

* Cases are complete.
* Manifests validate.
* Dataset version is frozen.

## Gate 3 – Ground-Truth Freeze

Proceed only when:

* Ground truth is reviewed.
* Matching logic is frozen.

## Gate 4 – Pilot Approval

Proceed only when:

* Pilot succeeds.
* Metrics are verified.
* Failure handling works.

## Gate 5 – Main Study Completion

Proceed only when:

* All planned runs are complete.
* Failed runs are preserved.
* Timing is complete.

## Gate 6 – Manuscript Readiness

Proceed only when:

* Literature review is complete.
* Results are complete.
* Placeholders are removed.
* Claims are supported.

## Gate 7 – Submission Readiness

Proceed only when:

* Venue formatting is complete.
* Ownership is confirmed.
* Final PDF and artifacts are reviewed.

---

# 27. Progress Tracker

| Workstream                | Status  | Owner | Target Date | Evidence |
| ------------------------- | ------- | ----- | ----------- | -------- |
| W1 Repository freeze      | Pending | TBD   | TBD         |          |
| W2 Dataset implementation | Pending | TBD   | TBD         |          |
| W3 Ground truth           | Pending | TBD   | TBD         |          |
| W4 Pilot automation       | Pending | TBD   | TBD         |          |
| W5 Main experiments       | Pending | TBD   | TBD         |          |
| W6 Human review           | Pending | TBD   | TBD         |          |
| W7 Literature review      | Pending | TBD   | TBD         |          |
| W8 Results and revision   | Pending | TBD   | TBD         |          |
| W9 Submission             | Pending | TBD   | TBD         |          |

---

# 28. Weekly Research Log

Recommended file:

```text
research-log.md
```

Template:

```md
# Research Log

## Week

TBD

## Completed

- TBD

## Evidence

- Commit:
- Dataset version:
- Run IDs:
- Documents:

## Problems

- TBD

## Decisions

- TBD

## Next Actions

- TBD
```

---

# 29. Publication Evidence Folder

Recommended structure:

```text
publication-evidence/
├── repository-freeze/
├── dataset-freeze/
├── ground-truth-review/
├── experiment-runs/
├── reviewer-materials/
├── literature-search/
├── result-validation/
├── preprint/
└── submissions/
```

Preserve:

* Tag and commit records.
* Dataset checksums.
* Ground-truth approvals.
* Experiment summaries.
* Reviewer assignment records.
* Search logs.
* Submission confirmations.
* Acceptance or rejection decisions.

---

# 30. Portfolio Integration

As research milestones are completed, update the portfolio accurately.

## After Repository Freeze

Allowed:

```text
Research implementation version frozen and reproducibility package prepared.
```

## After Experiments

Allowed:

```text
Completed a controlled evaluation across manual, AI-only, and hybrid workflows.
```

Add metrics only after validation.

## After Preprint

Allowed:

```text
Published a public technical preprint.
```

## After Submission

Allowed:

```text
Submitted the research manuscript to <venue>.
```

## After Acceptance

Allowed:

```text
Accepted for publication or presentation at <venue>.
```

Do not use later-stage wording before the milestone occurs.

---

# 31. O-1 Evidence Alignment

Potential future evidence from this plan includes:

* Original technical contribution.
* Public research preprint.
* Peer-reviewed publication.
* Conference presentation.
* Open-source repository impact.
* Technical article.
* External expert review.
* Independent project adoption.
* Citation or reuse by others.

The execution plan should preserve dated and independently verifiable records for each milestone.

The project and paper alone do not guarantee eligibility for any immigration category.

---

# 32. Phase 6 Acceptance Criteria

Day 50 Phase 6 is complete when:

* [x] The remaining research workstreams are defined.
* [x] Repository freeze requirements are defined.
* [x] Reproducibility manifests are defined.
* [x] Dataset implementation steps are defined.
* [x] Ground-truth review procedures are defined.
* [x] Matching-rule freeze requirements are defined.
* [x] Experiment automation scripts are planned.
* [x] Pilot execution is defined.
* [x] Main experiment procedures are defined.
* [x] Manual, AI-only, and hybrid controls are defined.
* [x] Failed-run handling is defined.
* [x] Human reviewer procedures are defined.
* [x] Literature-review execution is defined.
* [x] Result-analysis procedures are defined.
* [x] Chart-generation rules are defined.
* [x] Manuscript revision requirements are defined.
* [x] Venue-selection criteria are defined.
* [x] Preprint preparation is defined.
* [x] Artifact-package requirements are defined.
* [x] Submission checks are defined.
* [x] An eight-week recommended schedule is defined.
* [x] Minimum and expanded study paths are defined.
* [x] Ablation and model-comparison options are defined.
* [x] Cost tracking is defined.
* [x] Data management and reviewer privacy are defined.
* [x] AI-use disclosure requirements are defined.
* [x] Research claim controls are defined.
* [x] Risks and mitigation actions are defined.
* [x] Publication decision gates are defined.
* [x] Progress and evidence tracking are defined.

---

# 33. Phase 6 Completion Status

**Status:** Completed as the post-roadmap publication and evaluation execution plan

**Empirical evaluation status:** Pending

**Literature review status:** Pending execution

**Preprint status:** Not prepared

**Venue submission status:** Not submitted

**Recommended file:**

```text
docs/day50/publication_execution_plan.md
```

---

# 34. Next Phase

**Day 50 Phase 7 – Final 50-Day Roadmap Completion Report**

Phase 7 will summarize:

* Project 1 completion.
* Project 2 completion.
* Technical validation status.
* Research preparation.
* Portfolio readiness.
* Remaining limitations.
* Post-roadmap publication work.
* Final accomplishments.
* The correct final completion statement for the entire 50-Day Roadmap.
