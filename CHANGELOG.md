# Changelog

All notable changes to this plugin are documented here.

## 1.0.3

- New server tool, `upload_file`, in the `create-presentation` skill and README. Agents that can run shell commands (Claude Code, Codex, Cursor) upload a .pptx from the user's computer with a `curl` command the tool returns. In ChatGPT, a .pptx the user attaches to the chat is uploaded for them. The agent then revises it through `pptx_file_id`.
- The skill explains each way to get a deck to the agent, and when to fall back to the web app.

## 1.0.2

- Two new server tools in the `create-presentation` skill and README: `continue_presentation_agent_session` for follow-up changes to a deck the agent made, and `list_files` for finding a PowerPoint in your Plus AI workspace to revise.
- The agent tools now return within about 45 seconds, usually with status `RUNNING`, and `get_presentation_agent_session` waits on its own. The skill tells the agent to call it right away instead of waiting a minute or two, and not to start the request again.
- The skill passes on the agent's own summary of each run (`lastAssistantMessage`), and explains how decks get into the workspace (the web app's "Start with an existing PowerPoint", or the API).

## 1.0.1

- No API key needed: the MCP server now signs you in with your Plus AI account alone. The README and the `create-presentation` skill drop the API-key setup step. Uploading a deck to revise still uses an API key.
- The skill's error guidance matches the server's current messages: plan required, expired sign-in, and out of credits.

## 1.0.0 — initial release

- Portable [Agent Plugin](https://agent-plugins.org): root `plugin.json` and `mcp.json`, so it works in Cursor, Claude Code, Cline, Codex, and other compatible clients.
- Claude Code, Cursor and Codex plugin manifests (`.claude-plugin/`, `.cursor-plugin/`, `.codex-plugin/`) sharing one `.mcp.json`.
- `server.json` listing for the official MCP Registry, with a manually run GitHub Actions publish workflow.
- `plus-ai` MCP server pointing at Plus AI's hosted Streamable HTTP endpoint (`https://mcp.plusai.com/`), with OAuth sign-in.
- `create-presentation` skill (also a `/plus-ai:create-presentation` command) covering prompt writing, long-running agent sessions, deck edits, and error handling.
