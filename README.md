# BioNLP-AIPatient

# AIPatient Reproduction: Simulating Patients with EHRs and LLM Agents
![image](https://github.com/user-attachments/assets/83d9e76d-38fc-40ad-b50c-a49feec73114)
This repository is a faithful **reproduction** of the research project titled:
**"[AIPatient: Simulating Patients with EHRs and LLM Powered Agentic Workflow](https://arxiv.org/abs/2409.18924)" (Yu et al., 2024)**, originally published on arXiv.

Our implementation replicates the core architecture and methodology described in the paper using real-world electronic health records (EHRs) and large language models (LLMs), while also integrating several improvements and adaptations.
![image](https://github.com/user-attachments/assets/6f681b8e-0feb-4ff9-aa28-14176278294d)

---

## Dataset

We use the **MIMIC-III** dataset, more specifically the `NOTEEVENTS.csv` file. From this file, we extract and utilize **Discharge Summaries** as the primary data source to simulate realistic patient cases.

- **Dataset Source**: MIMIC-III v1.4 via PhysioNet
- **Access**: Credentialed data access required
- **Focus**: Discharge summaries only (free-text clinical narratives)

---

## Knowledge Graph Construction (AIPatient KG)

We construct a **Neo4j-based medical knowledge graph** from discharge summaries using an LLM-driven Named Entity Recognition (NER) pipeline:

### NER + Refinement Prompts (Custom)
- We use our **custom prompts** to extract medical entities including:
  - Symptoms
  - Vitals
  - Allergies
  - Social History
  - Family History
  - Medical History
- Our pipeline uses a **two-stage prompt system**:
  1. **Extraction Prompt** – identifies entities from the raw discharge note.
  2. **Self-Refinement Prompt** – verifies and corrects hallucinations or span errors.

### Knowledge Graph (Neo4j AuraDB)
- Graph database schema mimics the original AIPatient KG
- Relationships like `HAS_SYMPTOM`, `HAS_DURATION`, `HAS_MEDICAL_HISTORY` are captured.
- Tools: `neo4j`, `openai`, `tenacity`, `pandas`, `re`

![image](https://github.com/user-attachments/assets/16cc8d87-f5be-4d85-b202-f396f551d09e)


## LLM Agentic Workflow (Reasoning RAG)

We implement the **Reasoning RAG** framework using the **same prompts as the original paper** for agent behavior. This multi-agent system includes:

### Agents
- **Retrieval Agent**: Finds relevant KG nodes based on query
- **Abstraction Agent**: Generalizes specific queries
- **KG Query Generator**: Converts natural language into Cypher queries
- **Checker Agent**: Verifies alignment between retrieved info and the query
- **Rewrite Agent**: Generates realistic, personality-aligned responses
- **Summarization Agent**: Updates conversation history
![image](https://github.com/user-attachments/assets/b6f4a6f9-9be8-45d9-9e78-48b2edb30b91)

### Personality Simulation
- Patients are simulated using **Big Five personality traits**.
- Personality-aware responses are generated via prompt conditioning.

## Evaluation Metrics
- Readability: Flesch Reading Ease and Flesch-Kincaid Grade
- Manual Validation
## Setup & Installation

### Requirements
- Python ≥ 3.9
- OpenAI API Key (GPT-4 preferred)
- Neo4j AuraDB or local instance



> Yu et al. "AIPatient: Simulating Patients with EHRs and LLM Powered Agentic Workflow" arXiv preprint arXiv:2409.18924 (2024).

---

## License

This project is for academic, educational, and research purposes only. Please follow MIMIC-III data sharing and usage guidelines. All credit for the original architecture and prompts belongs to the authors of the referenced paper.
