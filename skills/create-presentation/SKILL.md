---
name: create-presentation
description: Create or edit PowerPoint presentations with the Plus AI MCP tools. Use when the user asks for a slide deck, presentation, pitch deck, or PowerPoint, or wants an existing Plus AI deck revised, and the plus-ai MCP server is connected.
argument-hint: [describe the presentation you need]
---

# Creating presentations with Plus AI

The `plus-ai` MCP server turns a prompt into a finished, designed presentation. When invoked as a command, the user's request is "$ARGUMENTS".

## Examples

- `/plus-ai:create-presentation 10-slide Q3 product launch deck for the sales team`
- `/plus-ai:create-presentation board update from the notes above, concise and data-driven`
- `/plus-ai:create-presentation lesson on photosynthesis for 8th graders, in Spanish`

## Tools

The server has five tools:

| Tool | Use it for | Time |
| --- | --- | --- |
| `create_presentation_with_agent` | The default, for the most polished deck. The Presentation Agent plans the storyline, writes the content, designs every slide, and reviews its own work. With `pptx_file_id` it revises a deck from the user's workspace. | Returns within about 45 seconds; the deck usually takes a few minutes |
| `continue_presentation_agent_session` | Every follow-up change to a deck the agent made ("shorten slide 3", "add a closing slide"). Edits the same deck in place. | Same as above |
| `get_presentation_agent_session` | Waiting for an agent session that returned status `RUNNING`. | Waits up to about 45 seconds per call |
| `list_files` | Finding a PowerPoint in the user's Plus AI workspace to revise. | Instant |
| `create_presentation` | Quick, template-based generation. Use it when the user needs the deck right away, or asks for a quick draft. Takes a slide count (1–30). | Faster |

## Write a good prompt

Plus AI only knows what you put in the prompt, so pass along everything useful from the conversation rather than a one-line topic:

- **Topic and goal** — what the deck is for and what the audience should take away.
- **Audience and tone** — for example, "board of directors, concise and data-driven".
- **Length and structure** — slide count and any sections the user asked for. With the agent, state these in the prompt; it has no separate slide-count argument.
- **Source material** — facts, figures, names, and quotes from the conversation or files the user shared. Plus AI can't see the conversation, so include the content itself, not a reference to it.

Set `language` only when the user wants a language different from the one the prompt is written in. The agent's default, `auto`, matches the prompt.

## Wait for the agent

`create_presentation_with_agent` and `continue_presentation_agent_session` return within about 45 seconds, so the client never times out. The agent usually needs a few minutes, so most calls return a `sessionId` with status `RUNNING`. That's expected, not a failure: the agent keeps working.

1. Tell the user the deck is being built.
2. Call `get_presentation_agent_session` with the `sessionId` right away. Each call waits up to about 45 seconds for the agent, so there's no need to pause between calls. Keep calling it while the status is `RUNNING`.
3. Don't call `create_presentation_with_agent` or `continue_presentation_agent_session` again for the same request. That starts another run and uses more of the user's credits.

## Change a deck the agent made

For any follow-up change to a deck the agent made in this conversation, call `continue_presentation_agent_session` with its `sessionId` and a prompt describing the change. The agent remembers the conversation so far and edits the same deck; don't start a new deck for an edit. The result comes back the same way as a new deck: wait on `RUNNING` as above.

- If the agent is still working on the previous change, the tool says so. Wait for it with `get_presentation_agent_session`, then send the next change.
- Only the person who started a session can continue it.

## Edit a deck from the user's workspace

To revise a PowerPoint the agent didn't make in this conversation, pass its file id as `pptx_file_id` to `create_presentation_with_agent`.

- Find the id with `list_files`. Use `name_contains` to search by name, and pass `nextCursor` back as `cursor` to look further back. Use only ids from `list_files` or from the user, and never invent one.
- Files get into the workspace when the user drops a .pptx on the New presentation page at https://app.plusdocs.com ("Start with an existing PowerPoint"), or uploads one through the Plus AI API (https://plusai.com/features/presentation-agent-api, which needs an API key). This server can't upload files itself. If the deck isn't listed, ask the user to add it there, or offer to create a new deck from its content instead.
- Decks the agent made don't appear in `list_files`. Continue those by `sessionId`.

## Present the result

A finished deck returns `presentationUrl` (the PowerPoint file). Agent sessions also return `pdfUrl`, `thumbnailUrl`, and `lastAssistantMessage`, the agent's own summary of what it did. Many clients show the thumbnail (the first slide) inline.

- Share the PowerPoint link and offer the PDF.
- Pass on the agent's summary, including anything it says it couldn't check.
- Mention that the file is a native, editable PowerPoint, so the user can open it in PowerPoint, Keynote or Google Slides and edit it.
- Offer to make changes; they go through `continue_presentation_agent_session`.

## Errors

- **"requires a Pro, Team or Max plan"** — the user's Plus AI organization isn't on a plan that includes the MCP server. Tell the user, and pass on the upgrade link from the error (new customers can start a 7-day free trial at https://plusai.com/pricing).
- **"sign-in has expired"** — the user needs to reconnect the Plus AI connector in their client, then retry.
- **"run out of AI Agent credits"** — the organization has used this billing cycle's credits. Tell the user; don't retry.
- **Anything else** — report the error message to the user. Every call uses Plus AI credits, so ask before retrying more than once.
