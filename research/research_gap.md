# Research Gap Definition

## 1. Introduction

Enterprise API development remains highly manual even though modern software teams now use AI coding assistants, low-code platforms, workflow automation tools, and code generation frameworks.

Most existing tools improve only one part of the software development lifecycle. For example, AI coding assistants help developers write source code, low-code platforms help users design applications visually, and workflow tools help orchestrate services. However, enterprise API modernization requires a connected pipeline that begins with business requirements and ends with implementation-ready artifacts.

This research focuses on the gap between semi-structured enterprise requirements and executable software artifacts.

The proposed work investigates whether AI can automate the transformation of requirement documents into:

1. Structured requirement metadata
2. Node-RED orchestration workflows
3. Go Factory Excel intake sheets
4. Requirement gap reports
5. Code-generation-ready specifications

---

## 2. Current State of Practice

In many enterprise environments, requirements are documented in tools such as Confluence, Jira, SharePoint, or internal documentation systems.

A typical enterprise API development process follows this pattern:

```text
Business Requirement Document
        ↓
Manual Requirement Analysis
        ↓
Manual API Design
        ↓
Manual Workflow / Integration Design
        ↓
Manual Factory Intake Sheet Creation
        ↓
Code Generation or Manual Development
        ↓
Testing and Validation
```

This workflow depends heavily on experienced engineers who understand:

* API contracts
* Enterprise header standards
* Request and response DTOs
* Validation rules
* REST and SOAP integrations
* Database mappings
* Security requirements
* Error handling standards
* Factory intake formats
* Deployment expectations

Because the process is manual, it is slow, inconsistent, and difficult to scale.

---

## 3. Limitations of Existing AI Coding Tools

AI coding assistants such as GitHub Copilot, Cursor, and ChatGPT have improved developer productivity, especially for function-level and file-level code generation.

However, these tools have several limitations in enterprise API modernization.

### 3.1 Limited Requirement-to-System Understanding

Most AI coding tools can generate code from prompts but do not automatically transform business requirement documents into full system artifacts.

They usually require the developer to manually define:

* API endpoint structure
* Required headers
* Request fields
* Response fields
* Business rules
* Downstream services
* Database tables
* Error mappings

### 3.2 Lack of Workflow Synthesis

Enterprise APIs often involve orchestration workflows such as:

* Validate headers
* Validate request body
* Build downstream request
* Invoke REST service
* Invoke SOAP service
* Query database
* Transform response
* Handle errors

Current AI coding tools do not reliably generate visual workflow structures such as Node-RED flow JSON from enterprise requirement documents.

### 3.3 Lack of Factory Intake Artifact Generation

Many enterprise modernization programs use structured intake formats such as Excel sheets to generate code through internal factories.

Existing AI coding assistants do not natively generate these factory-specific artifacts.

This is a major limitation because enterprise code generation depends not only on source code, but also on structured metadata.

### 3.4 Lack of Requirement Gap Detection

Enterprise requirements are often incomplete or ambiguous.

Common gaps include:

* Missing timeout values
* Missing retry policy
* Missing downstream error mapping
* Missing header propagation rules
* Missing validation constraints
* Missing database query details
* Missing response examples

Existing tools may answer questions about gaps if prompted manually, but they do not provide systematic gap analysis as part of an automated pipeline.

---

## 4. Limitations of Low-Code and Workflow Platforms

Low-code platforms such as Mendix, OutSystems, and Microsoft Power Platform reduce the amount of code developers write.

Workflow tools such as Node-RED and Camunda help teams model and execute business processes.

However, these platforms still require manual design.

### Key Limitations

* They do not automatically parse Confluence requirement documents.
* They do not generate complete API orchestration flows from natural language requirements.
* They do not create Go Factory Excel intake sheets.
* They do not detect missing requirement details before implementation.
* They require engineers or analysts to manually model workflows.

As a result, low-code and workflow tools reduce implementation effort but do not eliminate the manual translation step between requirements and technical artifacts.

---

## 5. Limitations of Model-Driven Engineering

Model-Driven Engineering attempts to generate software from structured models.

This approach is powerful when requirements are already converted into formal models.

However, enterprise requirements are often semi-structured, inconsistent, and scattered across documents.

### Key Limitations

* Requires upfront formal modeling.
* Requires domain-specific languages or standardized diagrams.
* Does not work directly from messy enterprise requirement documents.
* Does not naturally support Node-RED flow generation.
* Does not naturally support Excel-based code factory intake.
* Requires significant adoption effort across teams.

The proposed research differs by using AI to extract structured models directly from semi-structured enterprise documents.

---

## 6. Identified Research Gap

The core research gap is:

```text
Existing tools do not provide an end-to-end AI-driven pipeline that transforms semi-structured enterprise requirement documents into workflow, factory intake, and gap analysis artifacts for API modernization.
```

More specifically, existing tools do not simultaneously support:

1. Requirement extraction from semi-structured enterprise documents
2. API metadata extraction
3. Business rule extraction
4. Node-RED workflow generation
5. Go Factory Excel intake generation
6. Requirement gap detection
7. Code-generation readiness evaluation

This gap is important because enterprise modernization depends on more than just writing code. It requires accurate translation of requirements into multiple interconnected artifacts.

---

## 7. Proposed Research Direction

This research proposes an AI-driven Enterprise API Factory that acts as a bridge between business requirements and implementation-ready artifacts.

The proposed pipeline is:

```text
Confluence Requirement Document
        ↓
AI Requirement Extractor
        ↓
Structured Requirement JSON
        ↓
Node-RED Flow Generator
        ↓
Node-RED Workflow JSON
        ↓
Go Factory Excel Generator
        ↓
Factory Intake Sheet
        ↓
Gap Analyzer
        ↓
Requirement Gap Report
```

This approach combines:

* Natural language processing
* Large language models
* Workflow synthesis
* Template-based generation
* Enterprise API pattern recognition
* Gap analysis
* Factory-ready metadata generation

---

## 8. Research Questions

The following research questions guide this work:

### RQ1: Requirement Extraction

How accurately can an AI system extract API metadata, headers, request fields, response fields, validations, business rules, downstream dependencies, and database mappings from semi-structured enterprise requirement documents?

### RQ2: Workflow Generation

How accurately can an AI system generate Node-RED orchestration workflows from extracted requirement metadata?

### RQ3: Factory Intake Generation

How accurately can an AI system generate Go Factory Excel intake sheets from requirement and workflow artifacts?

### RQ4: Gap Detection

How effectively can an AI system detect missing, ambiguous, or incomplete requirement details before implementation?

### RQ5: Productivity Improvement

How much development preparation time can be reduced by using the proposed AI Enterprise API Factory compared with manual requirement analysis and artifact creation?

---

## 9. Hypothesis

The central hypothesis of this research is:

```text
An AI-driven pipeline that combines requirement extraction, workflow synthesis, Excel intake generation, and gap analysis can significantly reduce enterprise API development preparation time while maintaining high artifact accuracy.
```

Expected target outcomes:

| Metric                          | Target |
| ------------------------------- | -----: |
| Requirement Extraction Accuracy |   85%+ |
| Workflow Generation Accuracy    |   80%+ |
| Excel Generation Accuracy       |   90%+ |
| Gap Detection Precision         |   90%+ |
| Preparation Time Reduction      |   70%+ |

---

## 10. Novelty of the Research

The novelty of this research lies in connecting multiple automation layers into one enterprise-focused pipeline.

Existing tools usually focus on one of the following:

* Code generation
* Requirement summarization
* Workflow design
* Low-code application development
* Model-to-code transformation

The proposed system combines all of these ideas into a practical API modernization pipeline.

### Novel Contributions

1. A requirement-to-workflow generation approach for enterprise APIs
2. A Node-RED-based workflow synthesis method
3. A Go Factory Excel intake generation strategy
4. A requirement gap detection framework
5. A realistic enterprise API evaluation dataset
6. A measurable evaluation framework for enterprise software automation

---

## 11. Why This Gap Matters

This research gap matters because enterprises are under pressure to modernize legacy systems, migrate services, and accelerate API development.

Manual requirement translation creates several risks:

* Delayed delivery
* Inconsistent implementation
* Missed validation rules
* Incorrect downstream mappings
* Poor traceability
* Rework during testing
* Dependency on senior engineers

Automating this process can improve:

* Developer productivity
* Requirement clarity
* Artifact consistency
* Code generation readiness
* Testing readiness
* Enterprise modernization speed

---

## 12. Research Contribution Statement

This research contributes an AI-assisted enterprise software engineering approach that transforms semi-structured requirement documents into structured implementation artifacts.

The contribution is not limited to source code generation. Instead, it addresses the upstream engineering process by generating workflows, factory intake sheets, and gap reports before code is produced.

This positions the research as a bridge between requirement engineering, workflow automation, low-code development, and AI-assisted software modernization.

---

## 13. Summary

The research gap is clear: existing tools improve coding or workflow design, but they do not provide a complete pipeline from enterprise requirements to implementation-ready artifacts.

The proposed AI Enterprise API Factory fills this gap by automating requirement extraction, Node-RED workflow generation, Go Factory Excel intake generation, and gap analysis.

This gap definition establishes the foundation for the research methodology, dataset design, evaluation strategy, and eventual conference paper.
