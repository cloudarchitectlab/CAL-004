# Unintended public exposure evaluation run metadata

- Run date: 2026-08-18
- AWS Region: `us-east-1`
- Knowledge Base ID: `2MHJF4FLXN`
- Model: `US Anthropic Claude Haiku 4.5`
- Model ARN: `arn:aws:bedrock:us-east-1:<ACCOUNT_ID>:inference-profile/us.anthropic.claude-haiku-4-5-20251001-v1:0`
- API: `bedrock-agent-runtime RetrieveAndGenerate`
- Preserved-run session ID: `f9efae13-f169-4bba-ba60-a19c66c3f956`
- Initial successful but display-truncated session ID: `cd3e9130-bb80-418d-84f0-cc6eab56d135`
- Retrieval/generation configuration: Knowledge Base ID and model ARN only; service defaults otherwise
- Prompt/retrieval/generation tuning: none
- Preserved-run outcome: 4 PASS markers, 2 FAIL markers

## Controlled mutation

Exactly one Mermaid edge target was changed:

```diff
-    PatientIGW -->|"Public application ingress"| PatientPublicNACL
+    PatientIGW -->|"Public application ingress"| PHINACL
```

No evaluator hint identifies the mutation. The known-good fixture was not modified.

## Capture note

The initial successful invocation used the same input and configuration, but its citation envelope was clipped by the command display before it could be preserved. The request was repeated without tuning or input changes. This directory preserves the complete second response envelope and identifies both sessions.

## Preserved evidence

- `prompt-prefix.txt`: evaluator prefix used for the run
- `cal-002-unintended-public-exposure.mmd`: submitted architecture artifact
- `raw-response.json`: complete Bedrock response envelope
- `generated-response.txt`: exact `output.text` extracted from the preserved response

## Retrieved references

- `s3://cal-003-ai-knowledge-assistant-knowledge-4c546b83/knowledge/CAL-002/docs/architecture.md | descriptive | architecture | a6ce5334-f56a-4951-8dd0-8fd2202d54f9`
- `s3://cal-003-ai-knowledge-assistant-knowledge-4c546b83/knowledge/CAL-002/docs/requirements.md | normative | requirement | 252cf38d-8f15-4dd5-8afd-140433a52115`
- `s3://cal-003-ai-knowledge-assistant-knowledge-4c546b83/knowledge/CAL-002/docs/security-design.md | descriptive | security_design | a1746060-f150-4033-9279-388b09ff834b`
- `s3://cal-003-ai-knowledge-assistant-knowledge-4c546b83/knowledge/CAL-002/docs/security-design.md | descriptive | security_design | 533b2a03-7b37-497f-9992-00a7b6a08e29`
- `s3://cal-003-ai-knowledge-assistant-knowledge-4c546b83/knowledge/CAL-002/docs/validation.md | evidentiary | validation | 2796cc54-2a6b-4b60-b11f-1456fdeacb30`
