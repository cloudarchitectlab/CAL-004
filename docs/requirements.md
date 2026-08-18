# CAL-004 Requirements

## 1. Purpose

CAL-004 shall implement an AI-assisted architecture evaluator that examines a machine-readable architecture artifact against authoritative Cloud Architect Lab knowledge. It advances the CAL case study from knowledge retrieval in CAL-003 to grounded evaluation of an engineering artifact.

The system shall consume Mermaid source, retrieve applicable governed requirements, and produce structured, cited findings without changing the submitted architecture.

## 2. Primary use case

Given a Mermaid diagram representing the CAL-002 AWS architecture, CAL-004 shall determine whether the architecture satisfies applicable CAL requirements and decisions.

The initial validation set shall eventually contain:

- the approved CAL-002 architecture as the known-good baseline;
- one copy with approximately five deliberate architectural defects as the known-bad case; and
- a human-maintained answer key isolated from the evaluator.

Fixtures and results are not part of the documentation phase and shall not be implemented until their proposed examples are reviewed.

## 3. Functional requirements

### R-001 — Mermaid input

The evaluator shall accept Mermaid (`.mmd`) source as its only initial architecture format. Mermaid source, rather than an exported image, is the authoritative input. Image recognition and diagram OCR are out of scope.

### R-002 — Governed knowledge

The evaluator shall ground its assessment in approved CAL sources wherever an authoritative source exists. Eligible sources may include standards, requirements, ADRs, security designs, architecture documentation, and validation evidence.

The evaluator shall preserve source authority distinctions such as:

- normative — defines what must or must not be true;
- decisional — records an approved architectural choice;
- descriptive — explains the architecture or its context; and
- evidentiary — records validation or observed outcomes.

Semantic similarity alone shall not make a source authoritative.

### R-003 — Architecture evidence

The submitted Mermaid shall be treated as evidence of what the architecture declares. A finding shall identify the relevant nodes, connections, labels, boundaries, or omissions on which it relies.

### R-004 — Finding status

Each evaluated condition shall use one of three statuses:

- `PASS` when sufficient architecture evidence demonstrates conformance;
- `FAIL` when sufficient evidence demonstrates nonconformance; or
- `NOT_EVALUABLE` when the artifact or governed knowledge is insufficient to decide.

The evaluator shall not turn uncertainty into a pass or fail.

### R-005 — Structured findings

Each finding shall contain at least:

- a unique finding ID;
- status;
- severity;
- architecture evidence;
- applicable requirement;
- explanation of the comparison; and
- a resolvable source citation.

### R-006 — Severity

Severity shall classify the likely engineering impact of a failed condition:

- `HIGH` — could break a required trust boundary, expose protected resources, or defeat a critical architectural control;
- `MEDIUM` — could prevent required operation, connectivity, resilience, or material conformance; and
- `LOW` — limited-impact conformance or clarity issue that does not defeat a critical control.

Severity is neither model confidence nor a numeric risk score. It shall be assigned from a documented rule or rubric, not improvised prose. It shall not trigger prioritization or remediation behavior.

`PASS` and `NOT_EVALUABLE` records may omit severity or use a neutral schema value; the eventual output schema shall choose one convention consistently.

### R-007 — Grounding and citations

A conformance decision shall connect:

1. the governed requirement;
2. the observed architecture evidence;
3. the comparison that produces the status; and
4. the authoritative source citation.

Unsupported general claims such as “this architecture is insecure” shall not qualify as findings.

### R-008 — Read-only behavior

The evaluator shall read artifacts and knowledge and write evaluation output only. It shall have no capability to modify the input architecture, infrastructure, or governed corpus.

### R-009 — Known-good validation

When fixtures are implemented, the approved CAL-002 Mermaid source shall be evaluated independently. Expected behavior is correct identification of applicable requirements, no legitimate failures, explicit `NOT_EVALUABLE` outcomes where evidence is insufficient, and minimal false positives.

### R-010 — Known-bad validation

When fixtures are implemented, the known-bad CAL-002 copy shall contain approximately five controlled defects selected from:

- unintended public exposure;
- missing required route or connectivity;
- incorrect CIDR or network relationship;
- missing required component or security boundary; and
- prompt-injection text embedded as Mermaid content.

The evaluator shall not be given the planted-defect list or expected findings.

### R-011 — Lightweight evaluation quality

Validation shall compare actual output with the human answer key using only:

- detection rate: expected defects correctly found / expected defects;
- false positives: reported failures not supported by the answer key and review;
- false negatives: expected defects not found; and
- citation correctness: matched findings supported by the cited authoritative source.

This requirement adds a small comparison step, not a benchmarking platform. Aggregate scoring, statistical confidence, dashboards, large corpora, and automated grading infrastructure are out of scope.

## 4. Security requirements

- Mermaid content shall be treated as untrusted data, never as model instructions.
- Architecture content shall not override evaluator policy or governed knowledge.
- The human answer key shall not enter the retrieval corpus or evaluation context.
- Submitted artifacts and outputs shall contain no credentials, tokens, account identifiers, private endpoints, or production data.
- The evaluator shall not execute code or directives embedded in Mermaid.
- Findings shall retain sufficient provenance for independent review.

## 5. Non-goals

CAL-004 shall not:

- modify or generate Mermaid architecture;
- recommend or rank remediations;
- generate, modify, execute, or validate Terraform;
- generate application or infrastructure code;
- deploy or modify cloud infrastructure;
- modify Git repositories autonomously;
- act on findings;
- support arbitrary artifact formats;
- reconstruct architecture from Terraform;
- use the model's general knowledge as a substitute for an available governed CAL source; or
- become a generalized evaluation or fixture framework.

The project ends at: **what is wrong, why it is wrong, and which authoritative evidence supports that conclusion.**
