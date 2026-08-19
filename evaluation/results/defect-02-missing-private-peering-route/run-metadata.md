# Defect 02 — missing Patient Platform private peering route

- Run date: 2026-08-18
- AWS Region: `us-east-1`
- Knowledge Base ID: `2MHJF4FLXN`
- Model: `US Anthropic Claude Haiku 4.5`
- Model ARN: `arn:aws:bedrock:us-east-1:<ACCOUNT_ID>:inference-profile/us.anthropic.claude-haiku-4-5-20251001-v1:0`
- API: `bedrock-agent-runtime Retrieve` and `RetrieveAndGenerate`
- RetrieveAndGenerate session ID: `b8a6f1bc-5ed4-4064-be69-d58d5afcc25b`
- Standalone retrieval configuration: 10 results; service defaults otherwise
- RetrieveAndGenerate configuration: Knowledge Base ID and model ARN only; service defaults otherwise
- Prompt/retrieval/generation tuning: none

## Controlled mutation

The fixture is a fresh copy of the preserved known-good CAL-002 Mermaid. Exactly one route-table entry was removed:

```diff
-        PatientPrivateRT["Patient Platform Private Route Table<br/><br/>10.20.0.0/16 → local<br/>10.10.11.0/24 → VPC Peering<br/>0.0.0.0/0 → NAT Gateway<br/><br/>No direct route to Internet Gateway"]
+        PatientPrivateRT["Patient Platform Private Route Table<br/><br/>10.20.0.0/16 → local<br/>0.0.0.0/0 → NAT Gateway<br/><br/>No direct route to Internet Gateway"]
```

The defect-01 public-exposure edge is not present. The known-good public-ingress edge remains:

```mermaid
PatientIGW -->|"Public application ingress"| PatientPublicNACL
```

## Standalone retrieval outcome

- Returned results: `10`
- Score range: `0.8429103195667267` to `0.8862704932689667`
- All 10 retrieved chunks report `case_study=CAL-002` and `status=approved`.
- The result set includes architecture and security-design text stating that the approved private-management path traverses the Patient Platform private route table.
- It also includes the normative requirement to use explicit route-table entries for local, peering, and approved internet paths.

The first successful invocation was displayed for inspection. The identical untuned command was immediately repeated so its complete JSON could be preserved in `retrieve-response.json`.

## RetrieveAndGenerate outcome

The evaluator returned six `PASS` findings, zero `FAIL` findings, and did not detect the missing return route.

Most notably, it claimed:

> Patient Platform Private Route Table includes route to peer only when destined for Shared Services private subnet

That route is absent from the submitted artifact. The generated answer therefore both missed defect #2 and asserted architecture evidence that the Mermaid does not contain.

## Preserved evidence

- `cal-002-missing-private-peering-route.mmd`: submitted defect-02 artifact
- `prompt-prefix.txt`: unchanged evaluator instructions
- `retrieve-command.txt`: exact standalone retrieval command
- `retrieve-query.json`: exact standalone retrieval query
- `retrieve-response.json`: complete standalone retrieval response, including scores and metadata
- `rag-command.txt`: exact RetrieveAndGenerate command
- `rag-input.json`: exact evaluator instructions plus artifact input
- `raw-response.json`: complete RetrieveAndGenerate response envelope
- `generated-response.txt`: exact generated `output.text`
