---
document_type: architecture
authority_class: descriptive
status: completed
---

# Architecture

## Overview

CAL-004 combines three components with distinct responsibilities:

| Component | Question answered | Role |
|---|---|---|
| Mermaid architecture | What exists? | Untrusted engineering evidence |
| Governed CAL knowledge | What should exist? | Authoritative requirements and decisions |
| Foundation model | How do they compare? | Constrained reasoning engine |
| Structured findings | Where do they conform or differ? | Auditable evaluation output |

## Logical architecture

```mermaid
flowchart LR
    USER["Engineer"]

    subgraph INPUT["Architecture Input — Untrusted"]
        MERMAID["Mermaid Architecture-as-Code<br/>.mmd"]
    end

    subgraph EVALUATOR["CAL-004 Architecture Evaluator"]
        LOAD["Artifact Loader"]
        CONTEXT["Evaluation Context Builder"]
        EVAL["AI Architecture Evaluator"]
        FORMAT["Structured Finding Generator"]
    end

    subgraph KNOWLEDGE["Governed CAL Knowledge"]
        CORPUS["CAL Standards<br/>Requirements<br/>ADRs<br/>Architecture and Security Docs"]
        INDEX["Governed Retrieval Index"]
    end

    subgraph MODEL["Foundation Model"]
        LLM["Compare and Reason"]
    end

    subgraph OUTPUT["Read-only Evaluation Output"]
        FINDINGS["PASS / FAIL / NOT_EVALUABLE"]
        EVIDENCE["Architecture Evidence<br/>Rationale + CAL Citations"]
        REPORT["Structured Evaluation Report"]
    end

    USER --> MERMAID
    MERMAID --> LOAD
    LOAD --> CONTEXT
    CORPUS --> INDEX
    CONTEXT --> EVAL
    EVAL <--> INDEX
    EVAL <--> LLM
    EVAL --> FORMAT
    FORMAT --> FINDINGS
    FORMAT --> EVIDENCE
    FINDINGS --> REPORT
    EVIDENCE --> REPORT
```

The diagram is logical rather than a commitment to a particular cloud deployment. CAL-003 mechanisms such as an Amazon Bedrock Knowledge Base, S3-backed vector storage, and a Bedrock foundation model may be reused, but CAL-004 documentation does not expand the project into infrastructure implementation.

## Component responsibilities

### Artifact loader

Accepts one Mermaid source file and treats its complete raw source as the baseline retrieval and evaluation input. The baseline did not parse, decompose, or extract concepts before retrieval. A later experiment added a constrained observed-state extraction stage to test whether explicit component and relationship facts improved evaluation. The loader does not render images, execute directives, or modify the file.

### Evaluation context builder

Keeps evaluator policy, governed sources, and untrusted artifact content distinct. It requests relevant CAL knowledge and preserves source authority and citation metadata. It must not include fixture answer keys.

### AI architecture evaluator

Compares observed evidence with retrieved authoritative requirements. It selects a status, explains the comparison, and classifies failure impact. It does not propose a target state.

### Structured finding generator

Produces stable machine-readable records for human review and lightweight validation. Formatting does not change the substance or severity of a finding.

### Governed retrieval

Returns relevant CAL passages with source identity and authority. Retrieval relevance is necessary but not sufficient; the evaluator must determine whether a source is authoritative for the condition.

## Validation architecture

The test arrangement remains separate from the evaluator:

```mermaid
flowchart LR
    FIXTURE["Known-good or known-bad<br/>Mermaid fixture"] --> EVALUATOR["CAL-004 evaluator"]
    KNOWLEDGE["Governed CAL knowledge"] --> EVALUATOR
    EVALUATOR --> ACTUAL["Actual structured result"]
    ACTUAL --> REVIEW["Lightweight comparison"]
    ANSWER["Human answer key<br/>isolated from evaluator"] --> REVIEW
    REVIEW --> QUALITY["Detection, false positives,<br/>false negatives, citations"]
```

The completed fixture and result records preserve this separation. See [validation.md](validation.md) for the five defect evaluations and structured observed-state experiment.

## Data flow and trust

1. An engineer supplies Mermaid source.
2. The loader treats the complete raw artifact as untrusted data and passes it directly as the retrieval query and evaluation input.
3. The context builder retrieves applicable governed CAL sources without prior artifact parsing, decomposition, or concept extraction.
4. The model compares evidence and requirements under evaluator policy.
5. The formatter emits cited findings to a result destination.
6. During validation only, a separate human process compares results with isolated expected findings.

There is no data flow from findings into architecture, code generation, Terraform, Git, or cloud deployment.

The first experiment intentionally tested the simplest viable path. Raw Mermaid retrieved relevant authority, so preprocessing was not required for retrieval quality. The later observed-state experiment was introduced to isolate artifact interpretation from judgment after defect tests exposed graph reasoning and expected-state contamination failures.
