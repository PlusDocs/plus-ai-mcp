# Changelog

All notable changes to this plugin are documented here.

## 1.0.0 — initial release

- Portable [Agent Plugin](https://agent-plugins.org): root `plugin.json` and `mcp.json`, so it works in Cursor, Claude Code, Cline, Codex, and other compatible clients.
- Claude Code, Cursor and Codex plugin manifests (`.claude-plugin/`, `.cursor-plugin/`, `.codex-plugin/`) sharing one `.mcp.json`.
- `server.json` listing for the official MCP Registry, with a manually run GitHub Actions publish workflow.
- `plus-ai` MCP server pointing at Plus AI's hosted Streamable HTTP endpoint (`https://mcp.plusai.com/`), with OAuth sign-in.
- `create-presentation` skill (also a `/plus-ai:create-presentation` command) covering prompt writing, long-running agent sessions, deck edits, and error handling.
