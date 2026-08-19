---
document_type: decisions
authority_class: decisional
status: completed
---

# Decisions

This file records current project-level decisions. Formal ADRs may be added later only when a decision needs deeper context or durable alternatives analysis.

## D-001 — CAL-004 is evaluation only

**Status:** Accepted

CAL-004 reports conformance and violations with evidence and citations. It does not recommend changes, generate corrected architecture, produce code or Terraform, or deploy infrastructure.

**Why:** This preserves a clear progression from CAL-003 knowledge retrieval to CAL-004 evaluation and leaves remediation to CAL-005.

## D-002 — Mermaid source is the sole initial artifact format

**Status:** Accepted

The evaluator accepts Mermaid (`.mmd`) source. Exported images, OCR, Terraform, and arbitrary diagram formats are excluded.

**Why:** Mermaid is machine-readable Architecture-as-Code and keeps the first case study focused.

## D-003 — CAL knowledge, not the model, is authoritative

**Status:** Accepted

Conformance claims require governed CAL sources and their authority metadata. The model performs comparison and reasoning but cannot establish a requirement from general memory when an authoritative CAL source is required.

**Why:** Grounding and authority make findings auditable and connect CAL-004 directly to CAL-003.

## D-004 — Use three finding statuses

**Status:** Accepted

Findings use `PASS`, `FAIL`, or `NOT_EVALUABLE`.

**Why:** The third state prevents incomplete evidence from being forced into a false binary conclusion.

## D-005 — Severity is simple engineering-impact metadata

**Status:** Accepted

Failed findings use `LOW`, `MEDIUM`, or `HIGH`. Severity describes likely impact if the finding is correct; it is not confidence, prioritization, remediation, or numeric risk.

**Why:** A small rubric improves communication without introducing a risk engine.

## D-006 — Validate with CAL-002 known-good and known-bad cases

**Status:** Completed

The approved CAL-002 Mermaid architecture established the baseline. Five separate copies contained one controlled mutation each so that every observed result could be traced to a single test condition.

**Why:** A familiar, governed architecture provides meaningful test evidence without inventing a second domain.

## D-007 — Isolate the answer key

**Status:** Accepted

Expected findings are human test truth and shall remain outside retrieval, prompts, and model context.

**Why:** Giving the evaluator the answer key would test retrieval of expected answers rather than grounded architectural reasoning.

## D-008 — Keep evaluation quality lightweight

**Status:** Accepted

The case study records detection rate, false positives, false negatives, and citation correctness. Ambiguous semantic matches receive human review.

**Why:** These measures create credible evidence without a benchmark platform, large corpus, dashboard, or scoring framework.

## D-009 — Treat artifact content as untrusted data

**Status:** Accepted

Mermaid labels and comments cannot issue model instructions or override policy and governed sources. A prompt-injection comment exercised this boundary; the resulting false negative is recorded as correlation, not proof that the injected text caused the behavior.

**Why:** Architecture artifacts cross a trust boundary and can contain adversarial text.

## D-010 — Preserve read-only operation

**Status:** Accepted

CAL-004 may read inputs and knowledge and write evaluation output. It has no architecture, repository, Terraform, deployment, or cloud-resource mutation path.

**Why:** Read-only capability reduces risk and matches the evaluator-only scope.

## D-011 — Separate deterministic facts from generative judgment

**Status:** Accepted

Explicit components, properties, relationships, and absence checks should use deterministic comparison where practical. Language models remain useful for interpreting governed requirements, resolving semantic conditions, explaining findings, and producing cited review output.

**Why:** The structured observed-state experiment preserved an incorrect graph relationship exactly, yet the retrieval-grounded evaluator still reconstructed a compliant topology and returned a false negative. Structured input improved evidence clarity but did not make generative comparison deterministic.
