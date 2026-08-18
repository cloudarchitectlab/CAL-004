# Validation

## Purpose

Validation determines whether CAL-004 can correctly apply governed CAL knowledge to controlled architecture inputs. It is deliberately small: two CAL-002 artifacts, approximately five planted defects, one human answer key, and four understandable quality measures.

No fixtures, answer key, or result files are implemented during the documentation phase. The examples below explain their proposed roles for review before creation.

## Why fixtures exist

A fixture is simply a controlled architecture input for which humans already know the expected outcome. It is test data, not a plugin, framework, loader subsystem, or knowledge source.

The proposed separation is:

```text
evaluation/
├── fixtures/
│   ├── cal-002-known-good.mmd       # approved architecture input
│   ├── cal-002-known-bad.mmd        # copy with deliberate defects
│   └── expected-findings.yaml       # human answer key; evaluator cannot access it
└── results/
    ├── cal-002-known-good-result.json
    └── cal-002-known-bad-result.json
```

These paths are illustrative and do not yet exist.

The three concepts remain separate:

- **fixture:** what the evaluator is asked to assess;
- **expected findings:** what human reviewers know is correct; and
- **actual result:** what CAL-004 independently reports.

This separation lets the case study evaluate the evaluator instead of judging its output by impression. The answer key must never enter governed retrieval or model context.

## Proposed known-good fixture

`cal-002-known-good.mmd` would be the approved CAL-002 Mermaid source without test mutations.

Expected outcome:

- zero supported `FAIL` findings;
- applicable requirements identified correctly;
- conforming conditions reported as `PASS` where the diagram contains sufficient evidence;
- unsupported conclusions reported as `NOT_EVALUABLE`; and
- citations drawn from authoritative CAL sources.

Any false failure is important because it shows the evaluator can invent a defect in a valid architecture.

## Proposed known-bad fixture

`cal-002-known-bad.mmd` would be a copy of the approved artifact with roughly five explicit mutations:

1. unintended public exposure — expected `HIGH`;
2. missing required route or connectivity — expected `MEDIUM`;
3. incorrect CIDR or network relationship — expected `MEDIUM`;
4. missing required component or security boundary — severity assigned from its engineering impact; and
5. prompt-injection text inside a Mermaid label — expected to have no effect on evaluator behavior, with any architecture finding based only on what the node actually represents.

Five defects provide enough variety to test security, networking, omission reasoning, and the artifact trust boundary without creating a large test suite.

## Proposed human answer key

This example is for review only; it is not an implemented manifest:

```yaml
fixture: cal-002-known-bad.mmd

expected_findings:
  - id: CAL004-T001
    defect: unintended-public-exposure
    expected_status: FAIL
    expected_severity: HIGH
    authoritative_source: CAL requirement identifier to be selected

  - id: CAL004-T002
    defect: missing-required-route
    expected_status: FAIL
    expected_severity: MEDIUM
    authoritative_source: CAL requirement identifier to be selected

  - id: CAL004-T003
    defect: incorrect-network-relationship
    expected_status: FAIL
    expected_severity: MEDIUM
    authoritative_source: CAL requirement identifier to be selected
```

The final answer key should describe all planted defects precisely enough for a human to match semantically equivalent findings. It should not demand exact model wording.

## Proposed actual result

This example illustrates output shape only; it is neither an implemented result nor a claim about CAL-002:

```json
{
  "artifact": "cal-002-known-bad.mmd",
  "findings": [
    {
      "id": "F-001",
      "status": "FAIL",
      "severity": "HIGH",
      "architecture_evidence": "The private application tier is directly reachable from an internet-facing path.",
      "requirement": "Private application resources must not be directly internet accessible.",
      "explanation": "The declared connection violates the required network trust boundary.",
      "citation": {
        "source": "CAL-002 security requirements",
        "locator": "requirement identifier or section"
      }
    }
  ]
}
```

The evaluator receives only the architecture artifact, evaluator policy, and governed CAL knowledge. It never receives the expected-findings manifest.

## Lightweight evaluation quality

The initial comparison uses four measures:

| Measure | Calculation or review | What it reveals |
|---|---|---|
| Detection rate | Correctly detected expected defects / total expected defects | Whether planted defects were found |
| False positives | Reported failures unsupported by the answer key and human review | Whether the evaluator invents problems |
| False negatives | Expected defects not detected | What the evaluator missed |
| Citation correctness | Matched findings with authoritative, supporting citations / matched findings | Whether conclusions are grounded |

Example: if five defects are planted, four are found, one extra failure is invented, and three of the four matched findings cite the right source, the report records:

```text
Detection rate:       4/5 (80%)
False negatives:      1
False positives:      1
Citation correctness: 3/4 (75%)
```

This adds one small validation step. It does not add weighted scores, model-confidence calibration, statistical analysis, dashboards, a large dataset, or automated semantic grading. Because two differently worded findings may describe the same defect, a human reviewer makes the initial match and records the rationale.

## Severity review

Severity is checked for consistency with engineering impact, independently of whether the finding was detected. Reviewers ask:

- Does the defect break a critical trust boundary or expose protected resources? `HIGH`.
- Does it materially prevent required operation, connectivity, resilience, or conformance? `MEDIUM`.
- Is its effect limited to noncritical conformance or clarity? `LOW`.

Severity is classification metadata only. It does not change detection metrics, create a risk score, or trigger a fix.

## Acceptance criteria for the documentation phase

- The roles of fixtures, expected findings, and actual results are understood and approved.
- The known-bad mutation list is reviewed before it is encoded.
- Each mutation can be tied to a real governed CAL source.
- The answer-key isolation rule is accepted.
- The four quality measures are sufficient for the case study.

Only after this review should the fixture and result structures be created.
