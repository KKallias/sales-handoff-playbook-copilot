# Security and responsible use

## Published artifacts

The n8n exports in this repository have been sanitized. They do not contain API keys, OAuth tokens, n8n credential IDs, webhook IDs, the original n8n instance identifier, or the private Google Sheet identifier.

## Before using the workflows

- Connect credentials through n8n's credential manager; never paste secrets into node parameters or commit them to Git.
- Use synthetic data for demonstrations.
- Review your organization's policies before processing customer data.
- Limit access to workflow executions, vector indexes, source documents, and review records.
- Require human review of any content that could affect a customer commitment.

## Reporting an issue

If you find a credential, personal record, or unsafe automation path in this repository, open a GitHub issue without reproducing the sensitive value.

