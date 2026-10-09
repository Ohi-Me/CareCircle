# Product feedback (draft for the Devpost form)

These are first-hand notes from building CareCircle. **Please review and edit before submitting**: the form asks for *your* genuine feedback.

## Model Context Protocol: TypeScript SDK (`@modelcontextprotocol/sdk` 1.32.1)

**Worked well**
- `McpServer.registerTool` with zod input schemas made strict validation nearly free. Invalid arguments are rejected before our code runs.
- `elicitInput` validates the human's answer against the requested schema, which is exactly what a safety-critical approval needs.
- The same SDK on the server, the Node agent and the browser kept the protocol behaviour consistent everywhere.

**Friction**
- It isn't obvious up front that **elicitation is impossible in stateless / `enableJsonResponse` mode**. Server-to-client requests need a session and an SSE response stream so the answer (a separate POST) can be routed back. We built stateless first and had to rework the endpoint. A short "stateless vs stateful: what you lose" table in the docs would save time.
- `elicitInput` needs `relatedRequestId` (from the tool handler's `extra`) to send the request on the in-flight call's stream. That is easy to miss, and without it the request goes nowhere visible.
- `elicitInput` checks `capabilities.elicitation.form`, so clients have to declare `{ elicitation: { form: {} } }` explicitly. Examples showing the 2025-11-25 capability shape would help.
- The default client request timeout is short compared with how long a human takes to answer an elicitation. We pass a per-call timeout; a note in the elicitation docs would help.

**Feature requests**
- A documented pattern for **relaying elicitation from a server-side agent to a human** on another channel (voice or web). Agents in the middle are common.
- A first-class "this tool requires human approval" annotation that clients could enforce, alongside `destructiveHint`.

## Amazon Bedrock (Converse API, Amazon Nova Pro)

*(Fill in after the Bedrock evaluation run.)*
- Converse's `toolConfig` accepts MCP tool JSON schemas directly (`inputSchema.json`). Mapping MCP tools to Bedrock tools was a few lines.
- Friction: the getting-started path leads to running as the **root** account through `aws login`. A guided "create a least-privilege Bedrock dev identity" flow would help newcomers.
- Note the difference between on-demand model ids (`amazon.nova-pro-v1:0`) and cross-region inference profiles (`us.amazon.nova-pro-v1:0`). They need different IAM resources, and that is easy to trip over.

## Alexa+ developer experience

- The Add-on tools (Category SDK, MCP Toolkit, Web Simulator) were gated, so we built an independent web simulator, as the FAQ allows. Feature request: a public, self-serve **MCP Toolkit sandbox** that points at a developer's own Streamable HTTP server. Even a text-only simulator would let builders test real Alexa+ orchestration.
- Feature request: guidance on how Alexa+ will surface **MCP elicitation** (spoken yes/no confirmation) and **server-initiated announcements**. Both are central to safety-critical household skills like this one.
