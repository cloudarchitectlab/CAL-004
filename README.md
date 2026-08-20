---
document_type: case-study
authority_class: descriptive
status: completed
---

# CAL-004 — AI Architecture Evaluator

CAL-004 evaluates architecture itself as an input to engineering automation through **grounded architecture conformance evaluation**.

The project evaluates a Mermaid Architecture-as-Code artifact against governed Cloud Architect Lab (CAL) standards and architectural knowledge. Building on the retrieval and knowledge-authority mechanisms established in CAL-003, it identifies architectural conformance and violations and returns structured findings grounded in authoritative evidence and citations.

CAL-004 is an evaluator, not an engineer. It reports what it observes, which governed requirement applies, and why the evidence passes, fails, or cannot be evaluated. It does not modify architecture, recommend remediation, generate code or Terraform, or deploy infrastructure.

## Project objective

Demonstrate that an AI-assisted evaluator can perform grounded architecture conformance evaluation by applying authoritative CAL knowledge to a machine-readable engineering artifact in a repeatable, auditable way.

The completed case study used the approved CAL-002 Mermaid architecture as a known-good baseline and five independently mutated copies as controlled defect inputs. It also tested a two-stage flow that extracted structured observed state before retrieval-grounded evaluation.

## Scope

The implementation is intentionally narrow:

- Mermaid (`.mmd`) source is the only architecture input.
- Governed CAL documents are the source of requirements and decisions.
- The foundation model compares artifact evidence with retrieved CAL knowledge.
- Output consists of structured findings with status, severity, evidence, rationale, and citations.
- CAL-002 supplies one known-good baseline and five isolated defect evaluations.
- One follow-up experiment separates observed-state extraction from generative evaluation.

See [docs/requirements.md](docs/requirements.md) for the full boundary.

## Evaluation workflow

```text
Mermaid architecture + governed CAL knowledge
                       |
                       v
                 CAL-004 evaluator
                       |
                       v
       structured, cited engineering findings
```

The architecture artifact answers **what exists**. Governed CAL knowledge answers **what should exist**. The model performs the comparison; it is not itself the authority.

Each applicable condition is reported as:

- `PASS` — sufficient evidence shows conformance.
- `FAIL` — sufficient evidence shows a violation.
- `NOT_EVALUABLE` — the artifact or governed sources do not provide enough evidence.

Severity expresses the engineering impact of a finding, not model confidence and not an instruction to remediate. CAL-004 uses only `LOW`, `MEDIUM`, and `HIGH` classifications.

## Validation approach

The grounded architecture conformance evaluation methodology was tested through controlled defect injection and known-good/known-bad fixtures. The completed validation evidence is documented in [docs/validation.md](docs/validation.md).

In brief:

- the known-good Mermaid fixture establishes the approved baseline;
- five known-bad Mermaid fixtures each contain one deliberately injected defect;
- expected outcomes remain outside the evaluator context;
- captured output is compared with the controlled mutation and governed CAL sources; and
- a structured observed-state experiment tests whether separating extraction from judgment improves reliability.

One fixture placed prompt-injection text inside the Mermaid artifact to adversarially test whether the evaluator continued to treat artifact content as untrusted data. Citation review checked that findings were supported by retrieved, authoritative CAL evidence rather than generic model judgment.

The evaluator retrieved relevant authoritative knowledge and produced a clean known-good baseline, but it was not reliable as a deterministic conformance engine.

Across the defect suite it produced false negatives, confused descriptive text with graph relationships, reconstructed required components that were absent from the artifact, and inconsistently followed the requested output contract.

The prompt-injection run correlated with a changed result, but the single run does not establish causation.

The structured observed-state experiment preserved the tested graph defect explicitly and still produced a false negative.

The resulting design boundary is practical:

> Use deterministic checks for deterministic architecture facts where possible, and use language models for semantic interpretation, judgment, citations, and explanation under human review.

## Documentation

- [Requirements](docs/requirements.md)
- [Architecture](docs/architecture.md)
- [AI design](docs/ai-design.md)
- [Security design](docs/security-design.md)
- [Validation](docs/validation.md)
- [Decisions](docs/decisions.md)
- [Lessons learned](docs/lessons-learned.md)

## Roadmap boundary

CAL-004 ends at evaluation:

> Determine what is wrong, why it is wrong, and which authoritative evidence supports that conclusion.

Recommendations and proposed architecture or code changes are intentionally deferred to **CAL-005 — Architecture / Code Recommendations**.
