# Day 41 — Functional Requirements

## Document Information

| Field        | Value                                                                                           |
| ------------ | ----------------------------------------------------------------------------------------------- |
| Project      | AI-Assisted API Migration Analyzer                                                              |
| Roadmap Day  | Day 41                                                                                          |
| Phase        | Phase 2 — Functional Requirements                                                               |
| Status       | Ready for technical design                                                                      |
| Primary Goal | Define the complete MVP behavior, validations, analysis rules, outputs, and acceptance criteria |

---

## 1. Purpose

This document defines the functional requirements for the AI-Assisted API Migration Analyzer.

The application will collect structured and free-form information about an existing API or legacy integration, analyze its modernization readiness, and generate an explainable migration report.

The first version will use deterministic analysis rules so that:

* Results remain reproducible.
* Scores are explainable.
* Missing information is visible.
* Recommendations can be traced to specific findings.
* The MVP does not depend on a paid external AI service.

The application will not migrate source code automatically.

---

## 2. Functional Scope

The MVP must support the following complete workflow:

```text
Open application
      ↓
Enter system and API information
      ↓
Validate the input
      ↓
Submit the migration assessment
      ↓
Normalize the current-state information
      ↓
Detect missing information
      ↓
Calculate readiness scores
      ↓
Identify and prioritize risks
      ↓
Recommend a migration strategy
      ↓
Generate target-state recommendations
      ↓
Generate a phased migration roadmap
      ↓
Review the structured report
      ↓
Reset and analyze another system
```

---

## 3. Functional Modules

The application will contain these functional modules:

1. Assessment input.
2. Input validation.
3. Current-state normalization.
4. Missing-information detection.
5. Readiness scoring.
6. Risk analysis.
7. Migration-strategy recommendation.
8. Target-architecture recommendation.
9. Migration-roadmap generation.
10. Executive-summary generation.
11. Results presentation.
12. Workflow reset.
13. Error and loading-state handling.

---

# 4. Assessment Input Requirements

## FR-001 — Display the Assessment Form

The application must display a structured input form when the page loads.

The form must be available before any analysis is performed.

The initial page must not display stale results from a previous browser session unless persistence is implemented deliberately.

### Acceptance Criteria

* [ ] The form appears on initial load.
* [ ] Required and optional fields are distinguishable.
* [ ] The Analyze control is visible.
* [ ] The Reset control is visible or becomes available when state exists.
* [ ] No migration report appears before submission.
* [ ] No loading indicator is active initially.
* [ ] No error message appears initially.

---

## FR-002 — Collect Basic System Information

The application must collect:

* System name.
* API or integration name.
* Business purpose.
* Current environment.
* Technical owner, optional.
* Business owner, optional.
* Migration goal.

### Required Fields

At minimum:

* System name.
* Business purpose.
* Migration goal.

### Example Migration Goals

* Move to cloud.
* Replace SOAP with REST.
* Upgrade runtime.
* Split a monolith.
* Improve security.
* Improve scalability.
* Replace synchronous integration.
* Retire the application.
* Standardize deployment.
* Improve observability.

### Acceptance Criteria

* [ ] Required fields are validated.
* [ ] Empty required fields are reported.
* [ ] Whitespace-only values are rejected.
* [ ] Entered values remain visible after validation errors.
* [ ] Optional values may remain empty.

---

## FR-003 — Collect Interface Information

The application must allow the user to describe the current interfaces.

Supported information should include:

* Interface type.
* Endpoint or operation name.
* HTTP method, when applicable.
* Protocol.
* Request format.
* Response format.
* Known status codes.
* Headers.
* Number of endpoints or operations.
* Free-form interface notes.

### Interface Types

Suggested values:

```text
REST
SOAP
GraphQL
gRPC
Messaging
File Transfer
Batch
Database Integration
Other
Unknown
```

### Protocol Values

Suggested values:

```text
HTTP
HTTPS
SOAP over HTTP
JMS
Kafka
AMQP
SFTP
FTP
TCP
Database
Other
Unknown
```

### Acceptance Criteria

* [ ] The user can select an interface type.
* [ ] Unknown is a valid selection.
* [ ] The user is not forced to invent information.
* [ ] Free-form interface notes are supported.
* [ ] Interface data is included in the normalized analysis.

---

## FR-004 — Collect Technology Information

The application must allow entry of:

* Programming language.
* Framework.
* Runtime version.
* Build tool.
* Hosting platform.
* Deployment model.
* Source-control availability.
* Technology age or last major upgrade.
* Free-form technology notes.

### Example Values

Programming language:

```text
Java
Go
Node.js
Python
C#
COBOL
PL/I
Other
Unknown
```

Deployment model:

```text
Virtual Machine
Bare Metal
Container
Kubernetes
Serverless
Mainframe
Managed Platform
Unknown
```

### Acceptance Criteria

* [ ] Unknown values are supported.
* [ ] Runtime and framework values can be entered freely.
* [ ] The application does not fail on uncommon technologies.
* [ ] Technology information influences readiness and risk analysis.
* [ ] Missing technology details are reported when relevant.

---

## FR-005 — Collect Dependency Information

The application must collect information about:

* Internal APIs.
* External APIs.
* Databases.
* Message brokers.
* File systems.
* Third-party vendors.
* Authentication providers.
* Batch processes.
* Shared libraries.
* Manual operational dependencies.

The first version may collect:

* Dependency count.
* Critical dependency count.
* Known or unknown ownership.
* Free-form dependency details.

### Acceptance Criteria

* [ ] The user can indicate that dependencies are unknown.
* [ ] Dependency complexity affects the score.
* [ ] Critical unknown dependencies generate a risk.
* [ ] Single-system input is supported.
* [ ] Many dependencies do not break the UI.

---

## FR-006 — Collect Security Information

The form must allow the user to describe:

* Authentication mechanism.
* Authorization model.
* Transport encryption.
* Data-at-rest encryption.
* Secret-management approach.
* Certificate usage.
* Sensitive-data handling.
* Audit logging.
* Compliance requirements.
* Known security findings.

### Authentication Values

Suggested values:

```text
OAuth 2.0
OIDC
JWT
API Key
HMAC
Mutual TLS
Basic Authentication
Session
Custom
None
Unknown
```

### Acceptance Criteria

* [ ] None and Unknown are separate values.
* [ ] Missing authentication information can generate a risk.
* [ ] Unencrypted transport generates a high or critical concern where applicable.
* [ ] Hardcoded secrets, when reported, generate a high-severity risk.
* [ ] Security findings contribute to the readiness score.

---

## FR-007 — Collect Operational Information

The form must support:

* Structured logging.
* Metrics.
* Distributed tracing.
* Health checks.
* Alerting.
* Retry behavior.
* Timeout behavior.
* Circuit breaking.
* Rate limiting.
* Support ownership.
* Incident-response documentation.
* Rollback process.

Each capability may use:

```text
Implemented
Partially Implemented
Not Implemented
Unknown
Not Applicable
```

### Acceptance Criteria

* [ ] Operational capabilities can be assessed individually.
* [ ] Unknown values reduce confidence.
* [ ] Missing timeouts or retries can generate risks.
* [ ] No rollback process affects deployment readiness.
* [ ] Missing observability affects observability readiness.

---

## FR-008 — Collect Testing Information

The application must collect:

* Unit-test availability.
* Unit-test coverage, optional.
* Integration-test availability.
* Contract-test availability.
* End-to-end tests.
* Performance tests.
* Security tests.
* Regression-test process.
* Test-data availability.
* Known production defects.

### Acceptance Criteria

* [ ] Test availability affects the testing score.
* [ ] Missing test information is reported.
* [ ] Zero or very low coverage can create a high risk.
* [ ] Unknown coverage is not treated as zero automatically.
* [ ] Contract-test absence matters more when multiple consumers exist.

---

## FR-009 — Collect Data Information

The application should support:

* Primary database type.
* Number of databases.
* Data ownership clarity.
* Shared-table usage.
* Data volume.
* Transaction requirements.
* Data-retention requirements.
* Sensitive data.
* Data migration requirement.
* Schema documentation.
* Free-form data notes.

### Acceptance Criteria

* [ ] Unknown data ownership generates a risk.
* [ ] Shared databases reduce architecture readiness.
* [ ] Missing retention requirements may generate a compliance or data risk.
* [ ] Data migration complexity affects strategy recommendations.
* [ ] No database is a valid value.

---

## FR-010 — Collect Documentation Information

The application must allow users to indicate the presence or quality of:

* API specification.
* Request and response examples.
* Architecture diagram.
* Dependency diagram.
* Business-rule documentation.
* Runbook.
* Deployment guide.
* Rollback guide.
* Support ownership.
* Known-issue register.

Suggested values:

```text
Complete
Partial
Missing
Unknown
Not Applicable
```

### Acceptance Criteria

* [ ] Documentation quality affects the readiness score.
* [ ] Missing API contracts generate a risk.
* [ ] Missing business-rule documentation generates a risk.
* [ ] Missing ownership information appears in missing-information results.
* [ ] Not Applicable does not create a penalty automatically.

---

## FR-011 — Support Free-Form Technical Description

The form must contain a large text area for additional technical details.

The text may include:

* Architecture description.
* Existing behavior.
* Known problems.
* Migration constraints.
* Business rules.
* Integration details.
* Incident history.
* Requirements not captured elsewhere.

### Acceptance Criteria

* [ ] Multi-line input is supported.
* [ ] Large text does not break the page.
* [ ] The field may be optional when enough structured data exists.
* [ ] Free-form text is preserved in the current-state result.
* [ ] The input is not logged unnecessarily.

---

# 5. Input Validation Requirements

## FR-012 — Validate Required Fields

Before analysis begins, the application must validate required fields.

Minimum required information:

* System name.
* Business purpose.
* Migration goal.
* At least one meaningful current-state detail.

A meaningful current-state detail may include:

* Interface.
* Technology.
* Dependency.
* Security.
* Operational.
* Testing.
* Data.
* Documentation.
* Free-form description.

### Acceptance Criteria

* [ ] Analysis does not start when required input is absent.
* [ ] Every invalid required field has an understandable message.
* [ ] All validation messages can appear in one submission.
* [ ] Entered valid data is preserved.
* [ ] Focus or navigation to the first invalid field is supported when practical.

---

## FR-013 — Reject Whitespace-Only Values

Text fields must trim leading and trailing whitespace.

Whitespace-only values must be treated as empty.

### Acceptance Criteria

* [ ] `"   "` does not pass required validation.
* [ ] Stored normalized text is trimmed.
* [ ] Internal spaces are preserved.
* [ ] Multi-line descriptions remain readable.

---

## FR-014 — Validate Numeric Values

Numeric fields may include:

* Number of endpoints.
* Number of dependencies.
* Number of critical dependencies.
* Runtime age.
* Test coverage.
* Data volume estimate.

### Rules

* Counts must not be negative.
* Coverage must be between `0` and `100`.
* Critical dependencies must not exceed total dependencies.
* Blank numeric fields must remain unknown rather than becoming `0`, unless zero was explicitly entered.

### Acceptance Criteria

* [ ] Invalid ranges are rejected.
* [ ] Clear messages explain allowed ranges.
* [ ] Blank and zero remain distinguishable.
* [ ] Decimal values are accepted only where meaningful.
* [ ] Invalid numbers do not start analysis.

---

## FR-015 — Validate Logical Consistency

The application should detect obvious contradictory input.

Examples:

* Authentication is `None`, but authorization is marked fully implemented.
* Total dependencies are zero, but critical dependencies are greater than zero.
* Deployment is marked manual, while deployment automation is marked complete.
* No database is selected, but shared-table usage is marked yes.
* Transport is HTTP, but transport encryption is marked complete without explanation.

### Acceptance Criteria

* [ ] Contradictions generate a warning or validation error.
* [ ] Non-blocking inconsistencies may allow analysis.
* [ ] Contradiction messages identify the affected fields.
* [ ] The application does not silently correct user input.

---

# 6. Analysis Execution Requirements

## FR-016 — Start Analysis from User Action

Analysis must begin only after the user selects the Analyze control.

### Acceptance Criteria

* [ ] Analysis does not run automatically during normal typing.
* [ ] Analyze is disabled or guarded when the form is invalid.
* [ ] One click starts one analysis.
* [ ] Rapid repeated clicks do not create duplicate reports.
* [ ] The application indicates that analysis is in progress.

---

## FR-017 — Display Analysis Loading State

The application must display a loading state during analysis.

The loading state may show stages such as:

```text
Validating input
Normalizing current state
Calculating readiness
Identifying risks
Generating recommendations
Preparing report
```

### Acceptance Criteria

* [ ] Loading state appears immediately after valid submission.
* [ ] Analyze is disabled during processing.
* [ ] Loading state ends after success.
* [ ] Loading state ends after failure.
* [ ] Duplicate submissions are prevented.
* [ ] The page remains responsive.

---

## FR-018 — Normalize the Current-State Input

The application must convert the form input into a consistent domain structure.

Normalization must:

* Trim text.
* Preserve unknown values.
* Standardize status values.
* Standardize severity-related inputs.
* Convert numeric fields safely.
* Group related information.
* Preserve free-form notes.
* Avoid inventing missing details.

### Acceptance Criteria

* [ ] Normalized values use consistent types.
* [ ] Unknown information remains explicitly unknown.
* [ ] Blank optional values do not become false facts.
* [ ] Normalized output belongs only to the active submission.
* [ ] The normalized result can be reused by all analysis modules.

---

# 7. Missing-Information Requirements

## FR-019 — Identify Missing Critical Information

The analyzer must identify missing information that blocks or weakens migration planning.

Examples:

* Unknown system owner.
* Unknown business owner.
* Missing API contract.
* Unknown authentication.
* Unknown database ownership.
* Unknown dependencies.
* Missing rollback process.
* Missing test information.
* Missing deployment process.
* Missing business rules.
* Missing data-retention requirements.

### Acceptance Criteria

* [ ] Missing items are derived from the active input.
* [ ] Present information is not falsely reported missing.
* [ ] Duplicate missing items are avoided.
* [ ] Every missing item includes a category.
* [ ] Every missing item includes a reason.
* [ ] Critical missing information is prioritized.

---

## FR-020 — Assign Missing-Information Priority

Missing information must be classified as:

```text
Critical
High
Medium
Low
```

Suggested examples:

| Missing Information               | Priority |
| --------------------------------- | -------- |
| Unknown production authentication | Critical |
| Unknown database ownership        | High     |
| Missing rollback process          | High     |
| Missing architecture diagram      | Medium   |
| Missing technical owner           | Medium   |
| Missing optional example payload  | Low      |

### Acceptance Criteria

* [ ] Priority is based on migration impact.
* [ ] Priority is visible in the report.
* [ ] The same missing item receives a consistent priority.
* [ ] Missing items can influence confidence and readiness.

---

# 8. Readiness Scoring Requirements

## FR-021 — Calculate Category Scores

The analyzer must calculate scores for:

* Documentation.
* Architecture.
* Security.
* Dependencies.
* Testing.
* Observability.
* Deployment.
* Data.
* Operations.
* Business rules.

Each category must receive a score from `0` to `10`.

### Acceptance Criteria

* [ ] Every category has a score.
* [ ] Scores remain within `0–10`.
* [ ] Scores are based on visible criteria.
* [ ] Unknown values reduce score confidence.
* [ ] Not Applicable values are excluded appropriately.
* [ ] Repeating the same input produces the same score.

---

## FR-022 — Calculate the Overall Readiness Score

The analyzer must calculate an overall score from `0` to `100`.

The initial MVP may use equal weighting or explicit fixed category weights.

Suggested weighted model:

| Category       | Weight |
| -------------- | -----: |
| Documentation  |    10% |
| Architecture   |    15% |
| Security       |    15% |
| Dependencies   |    10% |
| Testing        |    10% |
| Observability  |    10% |
| Deployment     |    10% |
| Data           |    10% |
| Operations     |     5% |
| Business Rules |     5% |

Total:

```text
100%
```

### Acceptance Criteria

* [ ] Overall score remains within `0–100`.
* [ ] Category weights total `100%`.
* [ ] The calculation is deterministic.
* [ ] Critical blockers may apply a documented penalty.
* [ ] The score is rounded consistently.
* [ ] The report explains important score drivers.

---

## FR-023 — Assign a Readiness Label

The application must assign a readiness label.

|  Score | Label                      |
| -----: | -------------------------- |
|   0–29 | Not Ready                  |
|  30–49 | Early Discovery            |
|  50–69 | Partially Ready            |
|  70–84 | Migration Ready            |
| 85–100 | Strong Migration Candidate |

Critical blockers may prevent a high readiness label even when the numeric score is high.

### Acceptance Criteria

* [ ] Every overall score receives one label.
* [ ] Boundary values map correctly.
* [ ] Critical blockers can cap the label where defined.
* [ ] The label is visible beside the score.

---

## FR-024 — Calculate Assessment Confidence

The analyzer should produce a confidence indicator based on input completeness.

Suggested values:

```text
Low
Medium
High
```

Possible interpretation:

* High: most important categories contain known data.
* Medium: several unknowns remain, but major areas are covered.
* Low: many important areas are unknown.

### Acceptance Criteria

* [ ] Confidence is separate from readiness.
* [ ] Strong readiness with low confidence is possible but clearly explained.
* [ ] Missing critical information lowers confidence.
* [ ] Confidence calculation is deterministic.

---

## FR-025 — Explain Score Drivers

The report must identify:

* Strongest categories.
* Weakest categories.
* Critical penalties.
* Missing information affecting confidence.
* Main reasons for the assigned readiness label.

### Acceptance Criteria

* [ ] At least one positive driver is displayed when available.
* [ ] At least one limiting driver is displayed.
* [ ] Explanations reference actual input.
* [ ] Generic explanations are avoided where specific evidence exists.

---

# 9. Risk Analysis Requirements

## FR-026 — Generate Risks from the Current State

The analyzer must generate risks based on normalized input.

Risk categories include:

* Architecture.
* Security.
* Dependencies.
* Testing.
* Observability.
* Deployment.
* Data.
* Operations.
* Documentation.
* Performance.
* Compliance.
* Business rules.

### Acceptance Criteria

* [ ] Risks belong only to the active assessment.
* [ ] Each risk is tied to an input condition or missing fact.
* [ ] Duplicate risks are avoided.
* [ ] Risks are grouped by category.
* [ ] No unsupported risk is presented as confirmed.

---

## FR-027 — Assign Risk Severity

Every risk must have one severity:

```text
Critical
High
Medium
Low
```

### Acceptance Criteria

* [ ] Severity follows documented rules.
* [ ] Critical risks are visually prominent.
* [ ] Severity is not represented by color alone.
* [ ] Risk sorting places higher severity first.
* [ ] Identical conditions produce consistent severity.

---

## FR-028 — Include Complete Risk Details

Every risk should contain:

* Risk ID.
* Category.
* Title.
* Description.
* Severity.
* Evidence.
* Impact.
* Recommendation.
* Priority.
* Whether it is a blocker.

### Acceptance Criteria

* [ ] Risk IDs are unique within the assessment.
* [ ] Evidence references the active input.
* [ ] Impact explains why the risk matters.
* [ ] Recommendation provides an actionable response.
* [ ] Blockers are clearly identified.

---

## FR-029 — Identify Migration Blockers

The analyzer must identify conditions that may prevent migration from proceeding safely.

Examples:

* Unknown critical dependencies.
* No valid authentication.
* Unencrypted sensitive traffic.
* No API contract for a critical interface.
* No rollback process for a high-risk cutover.
* Undefined data ownership.
* Unsupported runtime without upgrade path.
* No test environment for a core transaction flow.

### Acceptance Criteria

* [ ] Blockers are separated from general risks.
* [ ] The report states why each blocker must be resolved.
* [ ] Blockers influence readiness status.
* [ ] A report with blockers cannot be labeled fully migration-ready without explanation.

---

# 10. Migration Strategy Requirements

## FR-030 — Recommend a Primary Migration Strategy

The analyzer must recommend one primary strategy:

```text
Rehost
Replatform
Refactor
Rewrite
Replace
Retire
Further Discovery Required
```

### Acceptance Criteria

* [ ] One primary strategy is selected.
* [ ] The strategy is based on current-state evidence.
* [ ] The rationale is displayed.
* [ ] Missing information can produce `Further Discovery Required`.
* [ ] The strategy does not imply automatic execution.

---

## FR-031 — Provide Alternative Strategies

The application may provide one or two alternative strategies where appropriate.

Each alternative should include:

* When it becomes appropriate.
* Main advantage.
* Main drawback.

### Acceptance Criteria

* [ ] Alternatives do not duplicate the primary recommendation.
* [ ] Alternatives are relevant to the input.
* [ ] No alternative is required when one clear path exists.
* [ ] The report distinguishes primary and secondary options.

---

## FR-032 — Apply Strategy Decision Rules

Example rules:

### Rehost

Consider when:

* Architecture is acceptable.
* Main issue is hosting.
* Low application change is desired.
* Technology remains supportable.

### Replatform

Consider when:

* Runtime or hosting needs modernization.
* Business logic is reusable.
* Moderate changes are acceptable.

### Refactor

Consider when:

* Coupling is high.
* Business rules are valuable.
* Architecture limits maintainability.
* Modern interfaces are required.

### Rewrite

Consider when:

* Technology is unsupported.
* Testability is extremely poor.
* Documentation is weak.
* Architecture risk is high.
* Business capability must remain.

### Replace

Consider when:

* Capability is non-differentiating.
* A suitable enterprise platform exists.
* Custom maintenance cost is high.

### Retire

Consider when:

* Capability is unused.
* Another system already provides it.
* Business value is low.

### Further Discovery Required

Use when:

* Critical input is missing.
* Confidence is low.
* Unknown dependencies or ownership prevent a safe recommendation.

### Acceptance Criteria

* [ ] Strategy rules are implemented outside UI components.
* [ ] Strategy output is deterministic.
* [ ] The rationale names relevant findings.
* [ ] Critical unknowns can override other strategy signals.

---

# 11. Target Architecture Requirements

## FR-033 — Generate a Target Architecture Summary

The analyzer must generate a recommended target-state summary.

The output should cover:

* API style.
* Service boundary.
* Hosting model.
* Authentication.
* Authorization.
* Data ownership.
* Dependency handling.
* Observability.
* Deployment.
* Testing.
* Resilience.
* Migration pattern.

### Acceptance Criteria

* [ ] Recommendations align with identified risks.
* [ ] Recommendations do not require unsupported technology.
* [ ] Unknown details are identified as design decisions.
* [ ] The target state is understandable without viewing source code.
* [ ] Recommendations remain advisory.

---

## FR-034 — Recommend API Interface Modernization

Depending on input, the analyzer may recommend:

* Preserve REST.
* Introduce REST façade.
* Replace SOAP with REST.
* Introduce asynchronous messaging.
* Adopt event-driven integration.
* Standardize schemas.
* Introduce versioning.
* Add contract-first development.
* Add backward compatibility.

### Acceptance Criteria

* [ ] Interface recommendation reflects the existing protocol.
* [ ] SOAP is not automatically considered invalid.
* [ ] Asynchronous recommendations require a suitable use case.
* [ ] Contract-first recommendations appear when contract quality is weak.
* [ ] Versioning recommendations appear when interface evolution risk exists.

---

## FR-035 — Recommend Security Modernization

Possible recommendations include:

* OAuth 2.0 or OIDC.
* Mutual TLS.
* API gateway enforcement.
* Secret-manager integration.
* Encryption in transit.
* Encryption at rest.
* Least-privilege authorization.
* Audit-event standardization.
* Certificate rotation.
* Sensitive-data masking.

### Acceptance Criteria

* [ ] Security recommendations map to security findings.
* [ ] No specific product is required unless selected intentionally.
* [ ] Missing security information produces discovery actions.
* [ ] Critical security risks appear in the migration roadmap.

---

## FR-036 — Recommend Observability Modernization

Possible recommendations:

* Structured logging.
* Correlation IDs.
* Distributed tracing.
* Metrics.
* Health and readiness endpoints.
* Alerting.
* Error-rate dashboards.
* Dependency monitoring.

### Acceptance Criteria

* [ ] Missing capabilities produce specific recommendations.
* [ ] Existing capabilities are not incorrectly reported missing.
* [ ] Observability recommendations appear in the target architecture and roadmap.
* [ ] Correlation and trace recommendations are tied to distributed integrations.

---

## FR-037 — Recommend Deployment Modernization

Possible recommendations:

* Containerization.
* Kubernetes or managed container platform.
* Serverless deployment.
* CI/CD.
* Infrastructure as code.
* Blue-green deployment.
* Canary deployment.
* Automated rollback.
* Environment parity.
* Artifact versioning.

### Acceptance Criteria

* [ ] Deployment recommendations consider the current model.
* [ ] Complex platforms are not recommended without justification.
* [ ] Rollback gaps generate roadmap work.
* [ ] Deployment automation affects readiness and recommendation priority.

---

# 12. Migration Roadmap Requirements

## FR-038 — Generate a Phased Migration Roadmap

The application must generate a roadmap with these phases:

```text
Phase 1 — Discovery and blocker resolution
Phase 2 — Contract and architecture stabilization
Phase 3 — Security and platform preparation
Phase 4 — Implementation and integration
Phase 5 — Testing and migration validation
Phase 6 — Deployment and cutover
Phase 7 — Monitoring and decommissioning
```

The application may omit phases that are clearly not applicable, but the default report should remain comprehensive.

### Acceptance Criteria

* [ ] Roadmap items are based on findings.
* [ ] Critical blockers appear in Phase 1.
* [ ] Security gaps appear before production cutover.
* [ ] Testing work occurs before deployment.
* [ ] Decommissioning appears only where relevant.
* [ ] Roadmap tasks are not duplicated.

---

## FR-039 — Prioritize Roadmap Actions

Each roadmap item must have:

* Priority.
* Phase.
* Title.
* Description.
* Reason.
* Related risk IDs.
* Expected outcome.
* Dependency information.

Suggested priorities:

```text
P0 — Migration blocker
P1 — Required before implementation
P2 — Required before production
P3 — Recommended improvement
```

### Acceptance Criteria

* [ ] P0 items correspond to blockers.
* [ ] Higher-priority actions appear first.
* [ ] Related risks are traceable.
* [ ] Expected outcomes are actionable.
* [ ] Dependencies between tasks are understandable.

---

## FR-040 — Generate Discovery Questions

The roadmap or missing-information section must include questions for unresolved areas.

Examples:

* Who owns the downstream service?
* Is the current authentication approved for the target platform?
* Which consumers depend on the existing response contract?
* What are the peak transaction volumes?
* Is rollback supported for partial migration?
* Are there regulatory retention requirements?
* Which business rules are authoritative?

### Acceptance Criteria

* [ ] Questions are based on missing information.
* [ ] Questions are specific.
* [ ] Duplicate questions are avoided.
* [ ] Critical questions are prioritized.
* [ ] Questions are separated from confirmed findings.

---

# 13. Executive Summary Requirements

## FR-041 — Generate an Executive Summary

The application must generate a concise summary containing:

* System being assessed.
* Migration goal.
* Overall readiness.
* Confidence.
* Primary strategy.
* Main strengths.
* Main risks.
* Critical blockers.
* Immediate next steps.

### Acceptance Criteria

* [ ] The summary fits within a readable short section.
* [ ] The summary uses active assessment data.
* [ ] The summary does not contradict detailed results.
* [ ] The summary clearly distinguishes readiness and confidence.
* [ ] The summary names blockers when present.

---

# 14. Results Presentation Requirements

## FR-042 — Display a Structured Results Dashboard

The results view should include:

* Executive summary.
* Overall readiness score.
* Readiness label.
* Confidence.
* Category scores.
* Current-state summary.
* Missing information.
* Risk register.
* Migration strategy.
* Target architecture.
* Migration roadmap.
* Discovery questions.

### Acceptance Criteria

* [ ] Results appear only after successful analysis.
* [ ] Sections are clearly labeled.
* [ ] High-severity items are easy to identify.
* [ ] Long content remains readable.
* [ ] Empty optional sections are hidden or clearly marked.
* [ ] Results belong only to the active assessment.

---

## FR-043 — Sort Risks and Missing Information

Default sorting:

1. Critical.
2. High.
3. Medium.
4. Low.

Within the same severity, blockers should appear first.

### Acceptance Criteria

* [ ] Sorting is deterministic.
* [ ] Severity labels are visible.
* [ ] Color is not the only indicator.
* [ ] Sorting remains correct after repeated analysis.

---

## FR-044 — Display Score Explanations

Each category score should show:

* Score.
* Status label.
* Main positive factor.
* Main limiting factor.
* Related risks.
* Related missing information.

### Acceptance Criteria

* [ ] Users can understand why the score was assigned.
* [ ] Explanations are based on active input.
* [ ] No empty explanation is displayed.
* [ ] Unknown information is identified.

---

## FR-045 — Support Copyable Report Content

The report content should be easy to select and copy.

Optional MVP functionality may include:

* Copy executive summary.
* Copy risk register.
* Copy full report.

### Acceptance Criteria

* [ ] Text selection is not blocked.
* [ ] Code or structured sections preserve readable formatting.
* [ ] Copy controls, when implemented, display success feedback.
* [ ] Copy failure does not crash the page.

---

## FR-046 — Support Report Export When Feasible

Report export is optional for the Day 41 requirements but recommended by Day 45.

Possible formats:

* Markdown.
* JSON.
* Plain text.

PDF export is not required for the MVP.

### Acceptance Criteria

* [ ] Export uses the active report.
* [ ] File names are safe.
* [ ] Exported content includes the system name.
* [ ] No previous analysis data appears.
* [ ] Export failure is handled clearly.

---

# 15. Workflow Reset Requirements

## FR-047 — Reset the Complete Workflow

The Reset action must clear:

* All form inputs.
* Validation errors.
* Normalized current-state data.
* Missing information.
* Category scores.
* Overall score.
* Readiness label.
* Confidence.
* Risks.
* Blockers.
* Migration strategy.
* Target architecture.
* Roadmap.
* Executive summary.
* Export state.
* Loading state.
* Error state.
* Success messages.

### Acceptance Criteria

* [ ] Reset returns the application to the initial state.
* [ ] No browser refresh is required.
* [ ] No prior score remains.
* [ ] No prior report remains.
* [ ] Analyze is guarded until valid data is entered again.
* [ ] No console error appears.

---

## FR-048 — Support Sequential Assessments

The user must be able to analyze System A, reset, and then analyze System B.

### Acceptance Criteria

* [ ] System B results contain no System A values.
* [ ] Scores are recalculated from System B only.
* [ ] Risks are regenerated from System B only.
* [ ] Strategy is regenerated from System B only.
* [ ] Export uses System B only.
* [ ] No manual page refresh is required.

---

# 16. Error Handling Requirements

## FR-049 — Handle Analysis Failure

When analysis fails:

* Loading must stop.
* An error message must appear.
* Partial results must not appear as complete.
* Retry must remain possible.
* Reset must remain possible.

### Acceptance Criteria

* [ ] Error state is visible.
* [ ] Success state is not visible simultaneously.
* [ ] Previous completed report is not presented as the failed result.
* [ ] A successful retry clears the prior error.
* [ ] The page remains usable.

---

## FR-050 — Display Actionable Error Messages

Error messages should explain:

* What failed.
* Why it may have failed.
* What the user can do next.

Examples:

```text
Analysis could not be completed because the input was incomplete. Review the highlighted fields and try again.
```

```text
The readiness score could not be calculated. Reset the assessment or retry with the current input.
```

### Acceptance Criteria

* [ ] Raw stack traces are not shown to users.
* [ ] Error text is understandable.
* [ ] Error text does not expose sensitive data.
* [ ] The message refers to the current operation.
* [ ] Errors clear after successful retry or reset.

---

# 17. State Management Requirements

## FR-051 — Maintain a Single Active Assessment

The application must have one active assessment at a time.

### Acceptance Criteria

* [ ] Every result is tied to the latest valid submission.
* [ ] Old async results cannot overwrite a newer assessment.
* [ ] Reset invalidates prior results.
* [ ] Duplicate submissions are prevented or safely ignored.
* [ ] Current input and current output remain synchronized.

---

## FR-052 — Centralize Reset Behavior

Reset logic should be reusable and centralized.

### Acceptance Criteria

* [ ] Reset state is not duplicated across unrelated components.
* [ ] All result fields are cleared consistently.
* [ ] New state fields are added to reset behavior deliberately.
* [ ] Reset can be tested independently.

---

# 18. Accessibility Requirements

## FR-053 — Provide Accessible Form Controls

All form controls must have:

* Visible labels.
* Meaningful names.
* Keyboard access.
* Clear required-state indication.
* Associated validation messages.

### Acceptance Criteria

* [ ] Every input has a label.
* [ ] Tab order is logical.
* [ ] Focus is visible.
* [ ] Disabled controls are semantic.
* [ ] No keyboard trap exists.

---

## FR-054 — Provide Accessible Severity and Status Indicators

Severity and readiness must not rely only on color.

Indicators should include text such as:

```text
Critical
High
Medium
Low
Migration Ready
Low Confidence
```

### Acceptance Criteria

* [ ] Text labels accompany visual indicators.
* [ ] Contrast remains readable.
* [ ] Screen-reader users can understand status.
* [ ] Headings follow a reasonable hierarchy.

---

# 19. Performance Requirements

## FR-055 — Complete Local Analysis Promptly

For normal input, deterministic analysis should complete without noticeable delay.

A short artificial loading stage may be used for UX, but analysis should not block the browser unnecessarily.

### Acceptance Criteria

* [ ] Normal input completes in a reasonable local time.
* [ ] The browser remains responsive.
* [ ] Long descriptions do not freeze the UI.
* [ ] Repeated calculations are avoided where practical.
* [ ] Analysis functions remain independent from rendering.

---

# 20. Security and Privacy Requirements

## FR-056 — Avoid Unnecessary Sensitive Logging

The application must not log full technical descriptions or secret-like values unnecessarily.

### Acceptance Criteria

* [ ] No routine `console.log` prints full assessment input.
* [ ] Error logging avoids credentials.
* [ ] Example data contains no real secrets.
* [ ] Browser-visible configuration is safe.
* [ ] The README states that sensitive production credentials should not be pasted.

---

## FR-057 — Treat User Input as Untrusted

Free-form input must be rendered safely.

### Acceptance Criteria

* [ ] User input is not rendered as executable HTML.
* [ ] Script content is displayed as text or sanitized.
* [ ] File names, when export is added, are sanitized.
* [ ] No dynamic code execution is used.
* [ ] The rules engine does not use `eval`.

---

# 21. Deterministic Analysis Requirements

## FR-058 — Separate Analysis Rules from UI

Readiness, risk, strategy, and roadmap logic must exist in reusable functions or modules.

### Acceptance Criteria

* [ ] UI components do not contain the full scoring model.
* [ ] Analysis functions accept typed input.
* [ ] Analysis functions return typed output.
* [ ] Functions can be tested independently.
* [ ] The same input returns the same output.

---

## FR-059 — Preserve Explainability

Each analysis result should be traceable to:

* An input field.
* A missing field.
* A documented rule.
* A derived category result.

### Acceptance Criteria

* [ ] Risk evidence is available.
* [ ] Score explanations are available.
* [ ] Strategy rationale is available.
* [ ] Roadmap items reference findings.
* [ ] Unsupported claims are avoided.

---

# 22. Suggested Analysis Rules

The following rules provide the initial MVP direction.

## 22.1 Documentation Rules

Examples:

```text
API specification missing
→ Documentation score penalty
→ High documentation risk
→ Add contract-definition roadmap task
```

```text
Business rules partially documented
→ Moderate score penalty
→ Medium risk
→ Add business-rule discovery task
```

## 22.2 Architecture Rules

```text
Shared database across multiple services
→ Architecture score penalty
→ High coupling risk
→ Recommend data-ownership separation
```

```text
Unsupported runtime
→ High or critical technology risk
→ Recommend replatform, refactor, or rewrite
```

## 22.3 Security Rules

```text
Authentication is None
→ Critical security risk for protected business capability
→ Security score penalty
```

```text
Transport encryption is not implemented
→ High risk
→ Recommend HTTPS or mutual TLS
```

## 22.4 Testing Rules

```text
No unit or integration tests
→ High migration-validation risk
→ Testing score penalty
→ Add characterization-test phase
```

```text
Coverage unknown
→ Missing information
→ Confidence penalty
```

## 22.5 Observability Rules

```text
No logs, metrics, or health checks
→ High operational risk
→ Add observability foundation task
```

## 22.6 Deployment Rules

```text
Manual deployment and no rollback
→ High release risk
→ Recommend CI/CD and rollback automation
```

## 22.7 Dependency Rules

```text
Critical dependencies have unknown ownership
→ Migration blocker
→ Further discovery required
```

## 22.8 Data Rules

```text
Database ownership unknown
→ High data risk
→ Discovery action required
```

```text
Shared-table access
→ Architecture and data penalties
→ Recommend data-boundary analysis
```

---

# 23. Functional Acceptance Workflow

The MVP must pass this end-to-end scenario.

## Scenario A — Complete Assessment

1. Open the application.
2. Enter a complete legacy API description.
3. Submit the analysis.
4. Review normalized data.
5. Review category scores.
6. Review overall readiness.
7. Review confidence.
8. Review risks.
9. Review migration strategy.
10. Review target architecture.
11. Review roadmap.
12. Review executive summary.

Expected result:

* The analysis completes.
* Results are internally consistent.
* No missing critical data is falsely reported.
* No stale report appears.
* No browser error occurs.

---

## Scenario B — Incomplete Assessment

1. Enter only required fields and limited technical data.
2. Submit the analysis.

Expected result:

* Analysis completes when minimum requirements are met.
* Missing information is prominent.
* Confidence is reduced.
* Unsupported assumptions are avoided.
* Strategy may become `Further Discovery Required`.

---

## Scenario C — Invalid Assessment

1. Leave required fields empty.
2. Enter invalid numeric data.
3. Submit.

Expected result:

* Analysis does not begin.
* Validation messages appear.
* Entered valid data remains.
* No report appears.

---

## Scenario D — Sequential Assessment

1. Analyze System A.
2. Reset.
3. Analyze System B.

Expected result:

* System B contains no System A data.
* All scores and findings are recalculated.
* No manual refresh is needed.

---

# 24. MVP Completion Criteria

The functional MVP is complete when:

* [ ] All required input fields exist.
* [ ] Input validation works.
* [ ] Current-state normalization works.
* [ ] Missing-information detection works.
* [ ] Ten category scores are generated.
* [ ] Overall score is generated.
* [ ] Readiness label is generated.
* [ ] Confidence is generated.
* [ ] Risks are generated and sorted.
* [ ] Blockers are identified.
* [ ] Migration strategy is recommended.
* [ ] Target architecture is generated.
* [ ] Migration roadmap is generated.
* [ ] Executive summary is generated.
* [ ] Results are displayed clearly.
* [ ] Reset clears all workflow state.
* [ ] Sequential assessments remain isolated.
* [ ] Errors are recoverable.
* [ ] Lint passes.
* [ ] TypeScript validation passes.
* [ ] Production build passes.

---

# 25. Out-of-Scope Functional Requirements

The initial MVP will not include:

* Automatic source-code migration.
* GitHub or GitLab repository analysis.
* Live API calls.
* Cloud-resource discovery.
* Persistent database storage.
* User authentication.
* Multi-user workspaces.
* Team comments.
* Approval workflows.
* Automated deployment.
* Infrastructure provisioning.
* Cost calculation.
* Formal compliance certification.
* Production monitoring.
* Automatic architecture diagrams.
* Autonomous final migration decisions.

These items may be added to the backlog.

---

# 26. Functional Risks

| Risk                                  | Impact                       | Mitigation                                         |
| ------------------------------------- | ---------------------------- | -------------------------------------------------- |
| Too many form fields                  | Users may abandon input      | Group fields into clear sections and allow Unknown |
| Scoring appears arbitrary             | Results may not be trusted   | Show score explanations and rule evidence          |
| Recommendations become generic        | Low portfolio and user value | Tie every recommendation to findings               |
| Missing data creates false confidence | Unsafe migration conclusions | Separate confidence from readiness                 |
| Scope expands into code migration     | Day 47 completion risk       | Enforce out-of-scope boundaries                    |
| UI contains analysis logic            | Hard to test and maintain    | Keep deterministic rules in reusable modules       |
| Sequential results mix                | Incorrect report             | Centralize state and reset behavior                |
| Unsupported technology causes errors  | Poor reliability             | Use flexible text and Other/Unknown options        |

---

# 27. Functional Requirement Traceability

| Requirement Group       | Planned Implementation Area                      |
| ----------------------- | ------------------------------------------------ |
| Input form              | `src/components/input/`                          |
| Validation              | `src/lib/validation/`                            |
| Normalization           | `src/lib/analysis/normalizeAssessment.ts`        |
| Missing information     | `src/lib/analysis/findMissingInformation.ts`     |
| Readiness scoring       | `src/lib/analysis/calculateReadiness.ts`         |
| Risk generation         | `src/lib/analysis/generateRisks.ts`              |
| Strategy recommendation | `src/lib/analysis/recommendStrategy.ts`          |
| Target architecture     | `src/lib/analysis/generateTargetArchitecture.ts` |
| Migration roadmap       | `src/lib/analysis/generateRoadmap.ts`            |
| Executive summary       | `src/lib/analysis/generateExecutiveSummary.ts`   |
| Workflow orchestration  | `src/hooks/useMigrationAnalysis.ts`              |
| Results display         | `src/components/results/`                        |
| Shared types            | `src/types/`                                     |

The final file names may change during architecture design, but responsibilities should remain separated.

---

# 28. Day 41 Phase 2 Deliverables

Phase 2 produces:

1. Complete input requirements.
2. Validation requirements.
3. Normalization requirements.
4. Missing-information requirements.
5. Scoring requirements.
6. Risk-generation requirements.
7. Migration-strategy requirements.
8. Target-architecture requirements.
9. Roadmap requirements.
10. Results-display requirements.
11. Reset and error-handling requirements.
12. Accessibility and security requirements.
13. End-to-end acceptance scenarios.
14. MVP completion criteria.

---

# 29. Phase 2 Completion Criteria

Phase 2 is complete when:

* All required input categories are defined.
* Validation rules are documented.
* Analysis execution behavior is defined.
* Readiness categories and weights are defined.
* Risk structure and severity are defined.
* Migration strategy behavior is defined.
* Target-state outputs are defined.
* Roadmap output is defined.
* Reset requirements are defined.
* Error handling is defined.
* Accessibility requirements are defined.
* Security requirements are defined.
* End-to-end acceptance scenarios are documented.
* MVP completion criteria are documented.

---

# 30. Phase 2 Completion Statement

The functional requirements for the AI-Assisted API Migration Analyzer have been defined.

The MVP will accept structured and free-form legacy API information, validate and normalize the input, identify missing information, calculate explainable readiness scores, generate categorized risks, recommend a migration strategy, describe a target architecture, and produce a prioritized migration roadmap.

The next document is:

```text
docs/day41/technical_architecture.md
```

This document will define Phase 3 — Technical Architecture.
