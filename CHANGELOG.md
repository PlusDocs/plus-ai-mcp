# Changelog

All notable changes to this plugin are documented here.

## 1.0.1

- No API key needed: the MCP server now signs you in with your Plus AI account alone. The README and the `create-presentation` skill drop the API-key setup step. Uploading a deck to revise still uses an API key.
- The skill's error guidance matches the server's current messages: plan required, expired sign-in, and out of credits.

## 1.0.0 — initial release

- Portable [Agent Plugin](https://agent-plugins.org): root `plugin.json` and `mcp.json`, so it works in Cursor, Claude Code, Cline, Codex, and other compatible clients.
- Claude Code, Cursor and Codex plugin manifests (`.claude-plugin/`, `.cursor-plugin/`, `.codex-plugin/`) sharing one `.mcp.json`.
- `server.json` listing for the official MCP Registry, with a manually run GitHub Actions publish workflow.
- `plus-ai` MCP server pointing at Plus AI's hosted Streamable HTTP endpoint (`https://mcp.plusai.com/`), with OAuth sign-in.
- `create-presentation` skill (also a `/plus-ai:create-presentation` command) covering prompt writing, long-running agent sessions, deck edits, and error handling.
