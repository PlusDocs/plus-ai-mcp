# Plus AI MCP

Turn a prompt into a professionally designed, fully editable PowerPoint presentation from any AI agent. This is the official plugin for [Plus AI](https://plusai.com)'s remote [MCP](https://modelcontextprotocol.io/) server, for Claude Code, Cursor, Codex, Cline and any other MCP client.

```
You:    Make a 10-slide pitch deck for an AI bookkeeping startup.
Agent:  Your deck is ready: pitch-deck.pptx · PDF · preview
```

## Why Plus AI

- **Native, editable PowerPoint.** Every slide is real PowerPoint: text, tables, charts and images you can edit, not screenshots pasted onto slides. Agent runs also return a PDF and a thumbnail.
- **Designed, not just written.** The Plus AI Presentation Agent plans the storyline, writes the content, designs every slide, and reviews its own work before handing the deck back.
- **Two speeds.** Use the Presentation Agent for the most polished result, or quick template-based generation when you need a deck right away.
- **Any language.** Generate a deck in the language your audience needs.
- **Secure.** OAuth sign-in, and Plus AI is SOC 2 Type II compliant.

See real output on [example presentations](https://plusai.com/example-presentations).

## Requirements

A Plus AI Pro, Team or Max plan. New customers can start with a [7-day free trial](https://plusai.com/pricing) that includes 1,000 AI credits. A presentation typically uses 40–170 credits.

You don't need an API key: you sign in with your Plus AI account the first time you use the server.

## Install

**Claude Code.** Install the plugin, which adds the MCP server and the `create-presentation` skill:

```
/plugin marketplace add PlusDocs/plus-ai-mcp
/plugin install plus-ai@plus-ai
```

Then ask for a deck, or run `/plus-ai:create-presentation 10-slide Q3 launch deck for the sales team`. To add only the MCP server: `claude mcp add --transport http plus-ai https://mcp.plusai.com/`

**Cursor.** Settings → Plugins, search **Plus AI**, Install.

**Codex.** Add this repository as a plugin marketplace, then install **Plus AI**.

**Any other MCP client.** Add the server to the client's MCP config:

```json
{
  "mcpServers": {
    "plus-ai": {
      "type": "streamable-http",
      "url": "https://mcp.plusai.com/"
    }
  }
}
```

On first use, the client opens a Plus AI sign-in (Google or Microsoft).

## Tools

| Tool | What it does | Speed |
| --- | --- | --- |
| `create_presentation_with_agent` | The Plus AI Presentation Agent: plans, writes and designs the deck from a prompt, or revises an existing deck. Returns PowerPoint, PDF and a thumbnail. | A few minutes |
| `create_presentation` | Quick, template-based generation from a prompt, a slide count (1–30) and a language. Returns a PowerPoint. | Faster |
| `get_presentation_agent_session` | Checks on an agent run that was still going when its call returned, and returns the files when it's done. | Instant |

To revise a deck you already have, upload it through the [Plus AI API](https://plusai.com/features/presentation-agent-api) and pass its file id to `create_presentation_with_agent`. The upload is the one step that uses an API key, which you can create at [app.plusdocs.com/api-settings](https://app.plusdocs.com/api-settings).

## Example prompts

- "Make a 10-slide pitch deck for an AI bookkeeping startup raising a seed round."
- "Turn these meeting notes into a 6-slide board update. Concise and data-driven."
- "Create a 45-minute lesson on photosynthesis for 8th graders, in Spanish."
- "Build a QBR deck for Acme Corp from this data: …"

## Notes

- Tools run as the Plus AI user who signs in, in that user's workspace.
- Each presentation uses AI credits from that user's plan, so clients may ask you to confirm each call.
- Plus AI hosts the server. This plugin runs no local code.

## Links

- [MCP server guide](https://guide.plusai.com/apis-for-presentations/mcp-server)
- [Pricing](https://plusai.com/pricing) · [Privacy policy](https://plusai.com/privacy-policy) · [Terms](https://plusai.com/terms) · [Security](https://plusai.com/security)
- Support: support@plusai.com · Security reports: see [SECURITY.md](SECURITY.md)

<details>
<summary>What's in this repository</summary>

| Path | Purpose |
| --- | --- |
| `.mcp.json` | MCP server config used by Claude Code, Cursor and Codex |
| `plugin.json`, `mcp.json` | Portable [Agent Plugins](https://agent-plugins.org) manifest and MCP config |
| `.claude-plugin/` | Claude Code plugin manifest and marketplace entry |
| `.cursor-plugin/` | Cursor plugin manifest |
| `.codex-plugin/`, `.agents/plugins/` | Codex plugin manifest and marketplace entry |
| `server.json` | Listing for the official [MCP Registry](https://registry.modelcontextprotocol.io), published by running `.github/workflows/publish-mcp.yml` from the Actions tab |
| `skills/create-presentation/` | Skill with guidance on using the tools well |
| `assets/` | Logo |

</details>

## License

MIT
