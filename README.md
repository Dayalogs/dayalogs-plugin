# Dayalogs plugin

Connect your AI agent to [Dayalogs](https://www.dayalogs.com), the AI-native survey
platform. Design and validate surveys, preview them for review, manage audiences,
links and quotas, run email campaigns and export responses through the hosted
Dayalogs MCP server.

The same repository works in **Grok Build**, **Grok Bot**, **Cursor** and **Claude Code**.

## Installation

- **Grok Build:** open `/plugin`, search for **Dayalogs** and install. Or run
  `grok plugin install dayalogs --trust`.
- **Cursor / Grok Bot:** open **Settings → Plugins**, search for **Dayalogs** and
  install. In Cursor you can also run `/add-plugin dayalogs` in chat.
- **Claude Code:** `/plugin marketplace add dayalogs/dayalogs-plugin`, then
  `/plugin install dayalogs`.

On first connection, the agent opens the Dayalogs sign-in in your browser. Authorize
the Dayalogs account you want the agent to work with. No API key is needed and nothing
should be pasted into chat.

## What you get

- **MCP server** `dayalogs` at `https://www.dayalogs.com/mcp` (Streamable HTTP,
  OAuth 2.1).
- **Skill** `dayalogs`: how to use the tools well. Read before writing, validate
  before importing, preview before publishing, test before sending a campaign, and
  ask before any action that reaches respondents or deletes data.

Tools cover survey authoring and versioning, preview and review rounds, translations,
styles and branding, audiences and segments, share links, link sets, embeds and quotas,
email campaigns, response quality and exports. The live tool list is served by the MCP
server; see the [MCP setup guide](https://www.dayalogs.com/help/mcp-setup) for the
full matrix.

Every tool carries MCP annotations (`readOnlyHint`, `destructiveHint`), so the agent
can tell a lookup from a send or a deletion before calling it.

## Example prompts

- "Create a 10-question customer satisfaction survey in Catalan and Spanish and give me
  a preview link."
- "Add a 'Don't know' option to question 4 and show me the change."
- "How many responses does the NPS survey have, and how many look low quality?"
- "Draft an email campaign for the NPS survey and send a test to me."
- "Export last month's responses as CSV."

## Authentication and network

The plugin connects only to `https://www.dayalogs.com`. Authentication is OAuth 2.1
authorization code with PKCE; the agent handles the flow.

Network endpoints:

- `https://www.dayalogs.com/mcp`: hosted MCP server (Streamable HTTP)
- `https://www.dayalogs.com`: OAuth authorization, discovery metadata, and the human
  sign-in and consent screen

Credentials: a Dayalogs account. The OAuth grant is scoped to that account and its
permissions. The plugin stores no API key, ships no hooks or scripts, and requests no
filesystem or shell access. Tools act only on data the signed-in user can already
access in Dayalogs.

## License

MIT for the files in this plugin. Use of the hosted Dayalogs service is governed by the
Dayalogs terms of service and privacy policy.
