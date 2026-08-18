# Lessons Learned

This document is intentionally a living record. At the documentation stage, the entries below capture design lessons and hypotheses. Empirical lessons will be added only after the evaluator and controlled fixtures have been run.

## Current design lessons

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

### Security can be demonstrated inside the core use case

An instruction-like Mermaid label tests whether the artifact trust boundary holds. Answer-key isolation tests knowledge separation. Read-only permissions test the evaluator boundary. These controls strengthen the architecture story without turning CAL-004 into a separate security project.

## Questions to answer during implementation

- What minimum Mermaid parsing is required to preserve useful architectural evidence?
- How reliably does retrieval return the authoritative source rather than a merely similar descriptive source?
- What citation locator is both stable and easy for a reviewer to resolve?
- When do two differently worded findings count as the same planted defect?
- Which properties of CAL-002 are visible in the diagram, and which must correctly remain `NOT_EVALUABLE`?
- Does the prompt-injection fixture leave evaluation behavior unchanged?

## Deferred lessons

The following claims require evidence and should not be written as conclusions yet:

- actual known-good false-positive behavior;
- actual known-bad detection rate;
- actual citation correctness;
- model sensitivity to Mermaid syntax and labeling;
- operational cost, latency, and repeatability; and
- whether the severity rubric needs refinement.

These will be recorded after fixture review, implementation, and repeatable evaluation runs.
