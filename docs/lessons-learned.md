---
document_type: lessons-learned
authority_class: descriptive
status: completed
---

# Lessons Learned

This document records the empirical lessons from the completed known-good baseline, five controlled defect evaluations, and one structured observed-state experiment.

## Empirical lessons

### Architecture can be an engineering input

Using Mermaid source rather than a rendered picture makes architectural relationships available for repeatable evaluation. This is the central CAL-004 case-study step: Architecture-as-Code becomes input to automation without yet becoming input to code generation.

### Evidence, authority, and reasoning are different things

The Mermaid artifact says what the design declares. Governed CAL documents say what is required or decided. The model compares them. Collapsing these roles would let untrusted artifact text or model opinion masquerade as policy.

### `NOT_EVALUABLE` is a useful outcome

An architecture diagram cannot prove every implementation property, and retrieval may not find a governing rule. Recording insufficient evidence is more trustworthy than forcing a pass or failure.

### Severity should describe impact, not certainty

For engineering artifacts, severity is useful when it communicates the consequence of a valid finding: broken trust boundaries are more consequential than noncritical clarity issues. Keeping only `LOW`, `MEDIUM`, and `HIGH` avoids premature scoring, triage, and remediation machinery.

### A fixture is just controlled test data

The fixtures folder needs no special framework. Its purpose is to hold inputs whose intended outcome is known. Keeping the fixture, human answer key, and actual output separate makes the evaluator testable and keeps the answer out of its context.

### Evaluator quality can be measured cheaply

Two controlled artifacts and four measures—detection rate, false positives, false negatives, and citation correctness—are enough to move the case study beyond a demo prompt. A large benchmark would add more infrastructure than insight at this stage.

In practice, five isolated mutations were more useful than one combined known-bad artifact because each result could be attributed to a single controlled change.

### Retrieval success does not guarantee evaluation success

Raw Mermaid retrieved relevant, approved CAL-002 requirements, architecture, and security material. That demonstrated that preprocessing was not required merely to achieve useful retrieval. The defect runs nevertheless produced incorrect conclusions, showing that grounded retrieval and correct architectural comparison are separate quality dimensions.

### Graph reasoning is less reliable than semantic recognition

The evaluator often understood the architecture vocabulary while mishandling topology. It preferred descriptive route-table text over a contradictory Mermaid edge and later reversed or reconstructed relationships that were explicitly supplied as source-label-target facts. Semantic fluency should not be mistaken for reliable graph conformance checking.

### Expected state can contaminate observed state

When the Private Workload Security Group was removed, the evaluator described that required control as though it were still present. The retrieved documentation correctly established what should exist, but the model projected that expected state into its account of the submitted artifact. A conformance system must preserve observed evidence independently from governing knowledge.

### Absence is especially difficult

The evaluator missed both a removed peering route and a removed workload security group. Detecting absence requires an explicit inventory or assertion check; it should not depend only on a language model noticing that a required fact never appeared in a long artifact.

### Prompt-injection correlation is not causation

The prompt-injection fixture reused the public-exposure mutation and added instructions to return only passes. The evaluator then missed the defect, whereas a prior non-injected run detected the core exposure. This is a meaningful security test failure, but a single nondeterministic comparison cannot prove the injected text caused the changed result. The evidence supports caution and further controlled testing, not a stronger causal claim.

### Structured observed state improves clarity, not determinism

The follow-up extractor preserved the defective `PatientIGW → PHINACL` relationship explicitly. The evaluator still returned a false negative and reconstructed a compliant path that was not present in the structured input. Separating extraction from judgment reduced Mermaid parsing ambiguity but did not guarantee semantic separation between observed and expected state.

### Deterministic facts need deterministic checks

Components, properties, graph edges, and required-absence or required-presence assertions are often machine-checkable. Those facts should use deterministic comparison where practical. Language models remain valuable for interpreting requirements, handling conditions that genuinely require semantic judgment, explaining results, and producing reviewable citations.

### Security can be demonstrated inside the core use case

An instruction-like Mermaid label tests whether the artifact trust boundary holds. Answer-key isolation tests knowledge separation. Read-only permissions test the evaluator boundary. These controls strengthen the architecture story without turning CAL-004 into a separate security project.

## Questions carried forward

- What deterministic graph and assertion model is small enough to remain useful without becoming a generalized policy engine?
- Which architectural conditions genuinely require semantic judgment after deterministic facts are established?
- What citation locator is both stable and easy for a reviewer to resolve?
- How should repeated controls distinguish prompt-injection effects from ordinary model nondeterminism?
- How should CAL-005 consume a validated finding without inheriting an evaluator's unsupported assumptions?
