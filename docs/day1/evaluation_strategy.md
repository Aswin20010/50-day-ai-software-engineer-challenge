# Evaluation Strategy

## LLM Strategy

The system will use a hybrid AI approach:

1. **Ollama Local LLM**
   - Used for local development, experimentation, and reproducible testing.
   - Helps validate that the framework can run without depending on external APIs.

2. **Vertex AI Enterprise Access**
   - Used for enterprise-grade model execution, scalability, governance, and future production integration.
   - Supports comparison between local and enterprise AI performance.

---

## Dataset

The evaluation dataset will consist of real or sanitized enterprise-style artifacts.

### Input Artifacts

1. Confluence requirement documents
2. Node-RED flow JSON files
3. Reference Excel specifications

### Dataset Size for Initial Evaluation

| Artifact Type | Target Count |
|---|---:|
| Confluence Requirements | 10 |
| Node-RED Flow JSONs | 10 |
| Reference Excel Files | 10 |

---

## Evaluation Tasks

### Task 1: Requirement Extraction

Input:

- Confluence requirement

Output:

- Structured requirement JSON

Measured by:

- Correct endpoint extraction
- Correct method extraction
- Correct header extraction
- Correct validation extraction
- Correct mapping extraction
- Correct business rule extraction

Target Accuracy:

**85%+**

---

### Task 2: Node-RED Flow Generation

Input:

- Structured requirement JSON

Output:

- Node-RED flow JSON

Measured by:

- Correct HTTP In node
- Correct validation nodes
- Correct business rule nodes
- Correct mapping nodes
- Correct wrapper/subflow nodes
- Correct HTTP Response node
- Correct node wiring

Target Accuracy:

**80%+**

---

### Task 3: Excel Generation

Input:

- Confluence requirement
- Node-RED flow JSON

Output:

- Factory-ingestable Excel specification

Measured by:

- Correct sheet population
- Correct endpoint metadata
- Correct validations
- Correct business rules
- Correct mappings
- Correct service references
- Correct query/entity/service rows

Target Accuracy:

**90%+**

---

### Task 4: Gap Analysis

Input:

- Confluence requirement
- Node-RED flow JSON
- Generated Excel

Output:

- Gap analysis report

Measured by:

- Missing field detection
- Inconsistent mapping detection
- Validation mismatch detection
- Incomplete Excel row detection
- Unclear requirement detection

Target Accuracy:

**90%+**

---

## Engineering Time Reduction

Manual baseline:

- 4 hours per API requirement

AI-assisted target:

- 30 minutes per API requirement

Expected reduction:

- 70%+

---

## Final Evaluation Output

The final evaluation will produce:

1. Accuracy score table
2. Time comparison table
3. Gap detection report
4. Model comparison between Ollama and Vertex AI
5. Lessons learned for enterprise adoption