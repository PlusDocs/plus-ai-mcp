# Changelog

All notable changes to this plugin are documented here.

## 1.0.4

- New server tool, `open_presentation`, and a presentation panel. In clients that support MCP Apps (and in ChatGPT), a deck the agent creates or edits opens in an inline viewer: flip through the slides, download the PowerPoint or PDF, and select a slide to ask for a change. Called without a session ID, the tool opens a picker of your recent PowerPoints. Other clients still get the text result and thumbnail.
- The panel never starts an edit on its own: quick actions draft the request in your chat for you to send, so nothing is billed until you do.
- Every tool now declares its safety hints. `continue_presentation_agent_session` is marked destructive, because it edits the deck in place, and `open_presentation`, `list_files` and `get_presentation_agent_session` are read-only.
- The `create-presentation` skill and README now list seven tools, and tell the agent to show the panel's result rather than poll for it.

## 1.0.3

- New server tool, `upload_file`, in the `create-presentation` skill and README. Agents that can run shell commands (Claude Code, Codex, Cursor) upload a .pptx from the user's computer with a `curl` command the tool returns. In ChatGPT, a .pptx the user attaches to the chat is uploaded for them. The agent then revises it through `pptx_file_id`.
- The skill explains each way to get a deck to the agent, and when to fall back to the web app. It also says uploads need HTTPS access to `s3.us-east-2.amazonaws.com`, and how to allow it in Claude.ai's code sandbox.

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
