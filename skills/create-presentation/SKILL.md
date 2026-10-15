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

The server has three tools:

| Tool | Use it for | Time |
| --- | --- | --- |
| `create_presentation_with_agent` | The default, for the most polished deck. The Presentation Agent plans the storyline, writes the content, designs every slide, and reviews its own work. It can also revise an existing deck. | A few minutes |
| `create_presentation` | Quick, template-based generation. Use it when the user needs the deck right away, or asks for a quick draft. Takes a slide count (1–30). | Faster |
| `get_presentation_agent_session` | Checking on an agent session that was still running when `create_presentation_with_agent` returned. | Instant |

## Write a good prompt

Plus AI only knows what you put in the prompt, so pass along everything useful from the conversation rather than a one-line topic:

- **Topic and goal** — what the deck is for and what the audience should take away.
- **Audience and tone** — for example, "board of directors, concise and data-driven".
- **Length and structure** — slide count and any sections the user asked for. With the agent, state these in the prompt; it has no separate slide-count argument.
- **Source material** — facts, figures, names, and quotes from the conversation or files the user shared. Plus AI can't see the conversation, so include the content itself, not a reference to it.

Set `language` only when the user wants a language different from the one the prompt is written in. The agent's default, `auto`, matches the prompt.

## Handle long-running agent sessions

`create_presentation_with_agent` waits several minutes for the deck. If the agent isn't done by then, the tool returns a `sessionId` with status `RUNNING`. This is not a failure — the agent keeps working.

1. Tell the user the deck is still being built.
2. Call `get_presentation_agent_session` with the `sessionId` after a minute or two, and repeat until it returns download links or an error.
3. Do not call `create_presentation_with_agent` again for the same request. That starts a second deck and uses more of the user's credits.

## Edit an existing deck

Pass `pptx_file_id` to `create_presentation_with_agent` to have the agent revise a deck instead of starting over. File ids come from uploading a PowerPoint through the Plus AI API (the one step that needs an API key), so use only an id the user gives you, and never invent one. If the user wants to edit a deck and has no file id, explain that they can upload it through the Plus AI API (https://plusai.com/features/presentation-agent-api), or offer to create a new deck from its content instead. To take another pass at the same deck, call the tool again with the same `pptx_file_id` and a prompt describing the changes.

## Present the result

A finished deck returns `presentationUrl` (the PowerPoint file). Agent sessions also return `pdfUrl` and `thumbnailUrl`. Share the PowerPoint link, offer the PDF, and show the thumbnail if the client can display images. Summarize what the deck covers in a sentence or two. Mention that the file is a native, editable PowerPoint, so the user can open it in PowerPoint, Keynote or Google Slides and edit it.

## Errors

- **"requires a Pro, Team or Max plan"** — the user's Plus AI organization isn't on a plan that includes the MCP server. Tell the user, and pass on the upgrade link from the error (new customers can start a 7-day free trial at https://plusai.com/pricing).
- **"sign-in has expired"** — the user needs to reconnect the Plus AI connector in their client, then retry.
- **"run out of AI Agent credits"** — the organization has used this billing cycle's credits. Tell the user; don't retry.
- **Anything else** — report the error message to the user. Every call uses Plus AI credits, so ask before retrying more than once.
