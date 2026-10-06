# Composway for Claude Code

Composway's hosted [MCP](https://modelcontextprotocol.io) server plus the skills
that drive it: search-data analytics across Google Search Console and Bing
Webmaster Tools, and the content tools that act on what it finds.

## Install

```sh
/plugin marketplace add voluminoushq/claude-plugins
/plugin install composway@composway
```

The first time a session uses a Composway tool, your browser opens and you sign in
with your Composway account. There is no API key. The server acts as you — the same
workspaces, permissions and quotas as the web app.

You need a Composway account and at least one workspace first; sign up at
[composway.ai](https://composway.ai). An assistant with no workspace can read very
little.

## What you get

**The MCP server** (`https://mcp.composway.ai/mcp`) — 48 curated tools in four
groups: diagnostics, workspaces & campaigns, search analytics, and writes. Every
write asks you to confirm before it runs. Full reference at
[composway.ai/hub/mcp/tools](https://composway.ai/hub/mcp/tools).

**Skills** that turn those tools into a workflow:

| Skill | What it does |
| --- | --- |
| `search-triage` | Works out what to fix and what to do about it — resolves the site, prices one ranked queue, confirms each finding with the read that can settle it, classifies click losses by cause, and hands off into content work. |

Skills are model-invoked: Claude reaches for one when the task calls for it. You can
also invoke it directly as `/composway:search-triage`.

## Not using Claude Code?

The same server works with Claude desktop, Cursor, Codex, VS Code, Zed and anything
else that speaks MCP over Streamable HTTP — see
[composway.ai/hub/mcp/connect](https://composway.ai/hub/mcp/connect). This plugin
is the Claude Code path; it just bundles the connection and the skills into one
install.

## Development

Edits happen in the monorepo (see **Source of truth** below), from
`workers/mcp-server/`. Test locally without installing:

```sh
claude --plugin-dir ./plugin
```

Then `/reload-plugins` to pick up edits, and `claude plugin validate ./plugin`
plus `yarn plugin:check` before opening a PR.

## Source of truth

This repository is **mirrored** from the Composway monorepo, where the skills live
next to the MCP server whose tools they describe — so CI can fail a change that
leaves a skill referencing a tool the server no longer exposes. Don't send patches
here; they'll be overwritten on the next publish. File an issue instead, or write to
support@composway.ai.

## License

MIT — see [LICENSE](LICENSE).
