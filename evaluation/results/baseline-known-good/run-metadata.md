# Untuned model-reasoning baseline run metadata

- Run date: 2026-08-18
- AWS Region: `us-east-1`
- Knowledge Base ID: `2MHJF4FLXN`
- Knowledge Base name: `cal-003-ai-knowledge-assistant-knowledge-base`
- Data source ID observed in citations: `CIPAGFBPAH`
- Model: `US Anthropic Claude Haiku 4.5`
- Model ARN: `arn:aws:bedrock:us-east-1:<ACCOUNT_ID>:inference-profile/us.anthropic.claude-haiku-4-5-20251001-v1:0`
- API: `bedrock-agent-runtime RetrieveAndGenerate`
- Session ID: `ee3efaed-f8a4-4e73-8fdd-dc613a8dd997`
- Retrieval/generation configuration: Knowledge Base ID and model ARN only; service defaults otherwise
- Prompt tuning before run: none
- Result counts: 14 `PASS`, 0 `FAIL`, 1 `NOT EVALUABLE`
- Similarity scores: not returned by the `RetrieveAndGenerate` response

## Preserved inputs and output

- `prompt-prefix.txt` is the exact evaluator prompt preceding the artifact.
- `cal-002-known-good.mmd` is the unchanged architecture artifact appended directly after that prefix.
- `generated-response.txt` is the complete text from the response `output.text` field.

## Retrieved source references observed

The response cited approved CAL-002 content from:

- `s3://cal-003-ai-knowledge-assistant-knowledge-4c546b83/knowledge/CAL-002/docs/requirements.md` (`authority_class=normative`, `document_type=requirement`, observed chunk `252cf38d-8f15-4dd5-8afd-140433a52115`)
- `s3://cal-003-ai-knowledge-assistant-knowledge-4c546b83/knowledge/CAL-002/docs/architecture.md` (`authority_class=descriptive`, `document_type=architecture`, observed chunk `a6ce5334-f56a-4951-8dd0-8fd2202d54f9`)
- `s3://cal-003-ai-knowledge-assistant-knowledge-4c546b83/knowledge/CAL-002/docs/security-design.md` (`authority_class=descriptive`, `document_type=security_design`, observed chunks `a1746060-f150-4033-9279-388b09ff834b` and `533b2a03-7b37-497f-9992-00a7b6a08e29`)

Each observed reference reported `case_study=CAL-002`, `status=approved`, and data source `CIPAGFBPAH`.

## Capture limitation

The command display retained the complete generated response and session ID but truncated the middle of the raw citation envelope before it was written to a file. The run was intentionally not repeated because this document records the first untuned result. Consequently, the generated answer is complete, while the citation inventory above contains every unique source and chunk identifier that remained observable rather than a byte-for-byte copy of the full API envelope.
