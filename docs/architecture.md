# Architecture notes

## System boundary

The prototype accepts only fictional documents and fictional call notes. It does not connect to a production CRM, send customer emails, or make autonomous sales decisions.

## Ingestion workflow

1. An n8n form receives a PDF.
2. The default document loader parses the binary file.
3. The recursive character splitter creates chunks of 700 characters with 200 characters of overlap.
4. OpenAI embeddings convert chunks into vectors.
5. Pinecone stores them in the `sales-v1` namespace.

## Retrieval workflow

1. An n8n form collects the company, sales representative, and raw call notes.
2. An information extractor produces customer needs, concerns, confirmed actions, and missing information.
3. Pinecone retrieves relevant playbook chunks using the original notes as the semantic query.
4. The workflow aggregates the retrieved items.
5. A second model creates a handoff of no more than 250 words.
6. Google Sheets stores the input, extraction, sources, output, and `Pending` review status.

## Information boundaries

| Data class | Source | Rule |
|---|---|---|
| Customer facts | Original call notes | Never invent or upgrade uncertainty into certainty |
| Confirmed actions | Explicit statements in the notes | Preserve owner, deadline, and supporting language |
| Company guidance | Retrieved knowledge-base content | Abstain when supporting information is missing |
| Suggested next steps | Model-generated | Label clearly as suggestions and limit to three |

## Security and privacy

- Published workflow exports contain no API keys or OAuth secrets.
- n8n credential references and private service identifiers have been removed.
- The Google Sheet identifier has been replaced with a placeholder.
- Use synthetic data unless your environment has an approved data-processing policy.
- Restrict access to n8n, Pinecone, OpenAI, and the review sheet in any real deployment.

