---
document_type: deployment
authority_class: descriptive
status: completed
---

# Deployment

CAL-004 did not deploy new infrastructure. It reused the existing CAL-003 Amazon Bedrock Knowledge Base and an available Claude Haiku 4.5 inference profile to perform read-only evaluation experiments.

## Operational boundary

- No Terraform or cloud-resource deployment was required.
- The evaluator used Bedrock `Retrieve` and `RetrieveAndGenerate` operations in `us-east-1`.
- Architecture artifacts were treated as untrusted, read-only inputs.
- Results were written only to local evaluation records.
- CAL-004 had no path to modify architecture, repositories, Terraform, or cloud resources.

Exact model, Knowledge Base, session, input, and capture details are recorded with the applicable runs under `evaluation/results/`.
