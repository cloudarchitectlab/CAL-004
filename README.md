# CAL-004 — AI Architecture Evaluator

CAL-004 explores treating architecture itself as an input to engineering automation.

The project evaluates a Mermaid Architecture-as-Code artifact against governed Cloud Architect Lab (CAL) standards and architectural knowledge. Building on the retrieval and knowledge-authority mechanisms established in CAL-003, it identifies architectural conformance and violations and returns structured, cited findings.

CAL-004 is an evaluator, not an engineer. It reports what it observes, which governed requirement applies, and why the evidence passes, fails, or cannot be evaluated. It does not modify architecture, recommend remediation, generate code or Terraform, or deploy infrastructure.

## Project objective

Demonstrate that an AI-assisted evaluator can apply authoritative CAL knowledge to a machine-readable engineering artifact in a repeatable, auditable way.

The initial case study uses the approved CAL-002 Mermaid architecture as a known-good input and a deliberately altered copy as a known-bad input. The expected defects remain isolated from the evaluator as human-maintained test truth.

## Scope

The first implementation is intentionally narrow:

- Mermaid (`.mmd`) source is the only architecture input.
- Governed CAL documents are the source of requirements and decisions.
- The foundation model compares artifact evidence with retrieved CAL knowledge.
- Output consists of structured findings with status, severity, evidence, rationale, and citations.
- CAL-002 supplies one known-good and one known-bad validation case.

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

Severity expresses the engineering impact of a finding, not model confidence and not an instruction to remediate. CAL-004 uses only `LOW`, `MEDIUM`, and `HIGH` classification metadata.

## Validation approach

Before fixtures are implemented, their proposed role and example contents are documented in [docs/validation.md](docs/validation.md). In brief:

- the known-good Mermaid file establishes the approved baseline;
- the known-bad copy contains about five deliberate defects;
- a human answer key records the expected findings and is never available to the evaluator;
- actual structured output is compared with that answer key.

Evaluation quality remains lightweight: detection rate, false positives, false negatives, and citation correctness. There is no benchmark platform, dashboard, scoring model, or fixture framework.

## Documentation

- [Requirements](docs/requirements.md)
- [Architecture](docs/architecture.md)
- [AI design](docs/ai-design.md)
- [Security design](docs/security-design.md)
- [Validation](docs/validation.md)
- [Decisions](docs/decisions.md)
- [Lessons learned](docs/lessons-learned.md)

## Roadmap boundary

CAL-003 asks whether AI can find and understand governed CAL knowledge. CAL-004 asks whether AI can apply that knowledge to an architecture artifact. Recommendations belong to CAL-005; Terraform generation and deployment remain later concerns.
