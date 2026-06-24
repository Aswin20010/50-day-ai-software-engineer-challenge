# Day 22 — Article 1 Final Version

## Article Title

Why AI Code Generation Fails Without Requirement Understanding

## Subtitle

AI can write code, but enterprise software needs something more important first: structured requirement understanding.

---

# Why AI Code Generation Fails Without Requirement Understanding

AI code generation has become one of the most exciting areas in software engineering.

Today, developers can ask an AI tool to generate an API endpoint, create DTOs, write unit tests, scaffold service logic, or explain an existing codebase within minutes.

That is powerful.

But in enterprise software engineering, the biggest challenge is often not whether AI can write code.

The bigger question is:

**Can AI understand the requirement well enough to build the right system?**

That is the problem I am exploring through my **50-Day AI Software Engineer Challenge**.

The larger research direction behind this work is:

**Automating Enterprise API Development using AI-Driven Requirement and Workflow Synthesis.**

The goal is not just to generate code faster.

The goal is to understand how AI can take messy enterprise requirements and convert them into structured, reviewable engineering artifacts.

---

## The Problem With Direct AI Code Generation

A common AI-assisted development approach is simple:

Give the model a requirement document and ask it to generate code.

This can work for small examples.

But enterprise API development is very different.

In real projects, requirements are rarely clean or complete.

They may be spread across:

* Confluence pages
* Screenshots
* Sample payloads
* Downstream API notes
* Database mapping documents
* Product discussions
* Business rule explanations
* Engineering clarification threads

One document may describe the happy path clearly but leave out error handling.

A sample response may not match the written field mapping.

A business rule may mention a downstream service without defining the exact contract.

A response field may appear in an example but not in the database mapping.

A required header may be missing from the sample request.

When this type of requirement is passed directly to an LLM, the model may still generate code confidently.

That is the risk.

The output may look structured, but it may be based on hidden assumptions.

If the requirement is unclear, the generated code will also be unreliable.

---

## Enterprise APIs Are Workflows, Not Just Endpoints

Many simple API examples focus on a route, a request, and a response.

Enterprise APIs usually involve much more.

A real enterprise API may include:

* Required headers
* Path parameters
* Query parameters
* Request body validation
* Authentication and authorization rules
* Downstream service calls
* Database reads and writes
* Business rule orchestration
* Data transformation
* Error mapping
* Audit fields
* Logging and observability
* Environment-specific configuration

This means the API is not just a controller method.

It is a workflow.

For example, an API may need to validate the incoming request, call a downstream system, read database records, apply business rules, transform values, handle fallback behavior, map errors, and return a response in a specific contract.

If one of those steps is missing or misunderstood, the generated API may compile but still fail the actual business requirement.

That is why enterprise API automation needs more than code generation.

It needs workflow understanding.

---

## The Missing Layer: Requirement Understanding

Before generating code, an AI system should be able to answer basic engineering questions.

What is the API supposed to do?

What are the required inputs?

Which fields are optional?

Where does each response field come from?

Which downstream systems are involved?

Which database tables are used?

What business rules must be applied?

What happens when data is missing?

What happens when a downstream call fails?

What assumptions are still unresolved?

These questions are usually answered by engineers during design discussions.

But if we want AI to participate meaningfully in enterprise software development, these answers must be captured in a structured way.

That is the missing layer.

AI should not move directly from requirement document to code.

It should move from requirement document to workflow first.

---

## Workflow Representation Before Code

An intermediate workflow representation acts as the bridge between human requirements and generated engineering artifacts.

Instead of asking an LLM to directly create code, the system should first extract a structured workflow.

That workflow should capture:

* API endpoint and method
* Request headers
* Path parameters
* Query parameters
* Request body fields
* Validation rules
* Business rule steps
* Downstream calls
* Database interactions
* Transformation rules
* Success response mappings
* Error response mappings
* Known gaps and assumptions

A better pipeline looks like this:

```text
Requirement Document
        ↓
Requirement Extraction
        ↓
Workflow Representation
        ↓
Schema Validation
        ↓
Artifact Generation
        ↓
Reviewable Engineering Outputs
```

This approach makes the process more reliable.

The workflow becomes something that engineers can review before code is generated.

It also gives the AI system a structured input for generating downstream artifacts such as Node-RED flows, Excel intake specifications, API documentation, test plans, gap analysis reports, and eventually service code.

The goal is not to remove engineers from the process.

The goal is to reduce manual translation work and make the generated output easier to review.

---

## Why Validation Matters

Even after a workflow is extracted, it should not be trusted blindly.

The workflow must be validated before generating artifacts.

Validation should check whether the extracted workflow is complete, consistent, and safe to use.

Some important validation questions are:

* Are all required request fields present?
* Are headers, path params, query params, and body fields clearly separated?
* Are response fields mapped to real sources?
* Are data types consistent?
* Are business rules complete?
* Are downstream systems defined clearly?
* Are database operations described properly?
* Are error scenarios documented?
* Are there conflicts between examples and written requirements?
* Are assumptions marked instead of silently accepted?

This step matters because LLMs can produce outputs that sound confident even when the input is incomplete.

Validation creates a checkpoint.

It prevents the factory from generating confident but incorrect artifacts.

---

## What My 50-Day Challenge Is Exploring

Through my **50-Day AI Software Engineer Challenge**, I am exploring how AI can automate major parts of enterprise API development from requirement documents.

The project is focused on generating reviewable engineering artifacts, including:

* Requirement extraction outputs
* Workflow representations
* Node-RED flows
* Excel intake specifications
* Gap analysis reports
* API generation inputs
* Go service structures

The first part of the challenge is intentionally focused on the non-coding foundation.

Before jumping into code generation, I am working on the structure required to make generation reliable.

That includes:

* Research problem definition
* System architecture
* Dataset strategy
* Requirement extraction design
* Workflow representation
* Schema planning
* Validation thinking
* Evaluation strategy

This foundation matters because AI-assisted software engineering should not only be about producing more code.

It should be about producing better engineering decisions.

---

## Lessons From the First 20 Days

The first 20 days of this challenge have made one thing clear:

**AI code generation is not the hardest problem.**

Requirement understanding is the real bottleneck.

If the input is weak, the output will be weak.

If the workflow model is incomplete, the generated API will miss important logic.

If error handling is not captured, the generated service will not behave correctly in real scenarios.

If assumptions are not detected, the system may generate artifacts that look complete but are actually wrong.

That is why intermediate representations matter.

They create a structured layer where requirements can be reviewed, validated, corrected, and then used for generation.

---

## Final Takeaway

The future of AI software engineering is not only about generating code faster.

It is about understanding the problem better before generating anything.

Reliable AI-assisted software engineering starts before code.

It starts with better requirement understanding.

---

## Publishing Notes

This article is ready to be published on:

* Medium
* Dev.to
* Hashnode
* LinkedIn Articles

Suggested tags:

* AI Engineering
* Generative AI
* Software Engineering
* API Development
* Enterprise Automation
* LLM
* Backend Development
* System Design
* Go
* Node-RED

---

## Day 22 Phase 4 Status

Phase 4 created the final publish-ready version of Article 1.

The next phase will create the Day 22 completion report.
