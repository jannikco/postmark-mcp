# Postmark MCP server

Inspect outbound delivery statistics before explicitly submitting one transactional email.

A direct-provider MCP server with useful local execution. No PipMCP account is required for local use. Provider API access and any provider charges are your responsibility.

## Quick start

Requires Node.js22+. Clone this repository, then run:

```sh
npm ci --ignore-scripts
npm test
npm start
```

The default command inspects the tools offline. Configure your own credentials in your environment:

- `POSTMARK_SERVER_TOKEN`



Credential help: https://postmarkapp.com/swagger/server.yml

## Provider setup

Use a Server API Token, not an Account API Token. Sender signature/domain must be verified. Use an authorized test server/recipient; success does not prove delivery.

## Execute a real read

Replace example record IDs with records you are authorized to access.

```sh
npm start -- --call postmark_get_delivery_stats '{}'
```

## Use with an MCP client

Launch `node /absolute/path/postmark-mcp/server.mjs` using stdio transport; pass credentials through the client environment. Stdout is reserved for JSON-RPC.

## Tools

| Tool | Effect | Purpose |
| --- | --- | --- |
| postmark_get_delivery_stats | Read | Read outbound delivery statistics without exposing the server API token configuration. |
| postmark_send_email | Explicit approval required | Submit one transactional message; inspect ErrorCode/MessageID and subsequent delivery events. |

## Concrete workflow

Inspect outbound delivery statistics before explicitly submitting one transactional email.

1. postmark_get_delivery_stats
2. postmark_send_email

## Optional writes

Writes are disabled by default. Set CONNECTOR_ALLOW_WRITES=true and supply `_approval: {approved:true,idempotencyKey:"unique-request-id"}` on the specific call. Do not reuse a key for a different request. Uncertain outcomes are never automatically replayed; inspect the provider before another attempt.

## Capability evidence

Local: **Tested — HTTP fixtures and real stdio tools/call**. Managed: **Documented**. Provider authentication and live operations have not been established by these fixture tests. Evidence applies per operation and execution mode; setup discovery does not imply provider execution.

## Managed production

[Activate with PipMCP](https://pipmcp.com/github/start?app=postmark&integration=postmark-mcp&utm_source=github&utm_campaign=github-postmark-mcp&utm_content=readme) when you want the hosted service.  Local credentials are not uploaded by this code. Review provider permissions in the managed onboarding. [Directory and related projects](https://jannikco.github.io/pipmcp-connectors/integrations/postmark/).

## Provenance and maintenance

- https://postmarkapp.com/swagger/server.yml

Maintainer:jannikco. MIT license for this code. See connector.json for selected operations and integration.json for evidence. Runtime dependencies retain their own licenses. Report sensitive issues through https://pipmcp.com/contact; do not post credentials or customer data in public issues.
