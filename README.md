# Slidely

Create and edit slides in Claude.

Slidely is a stateful presentation agent. You work with it through a chat that
remembers the uploaded files, the chosen design, the approved storyline, and the
slides it has proposed. It proposes, you approve: slides only enter the deck
through an explicit review.

## What's included

| Component | Name | Purpose |
| --- | --- | --- |
| Skill | `working-with-slidely` | Tells Claude how to drive Slidely: read the server-side working instructions first, then initialize, design, build, and review a deck. |
| MCP server | `slidely` | Hosted connector at `https://prod.slidely.ai/mcp` providing the presentation tools. |

## Triggering

The skill activates when you ask to:

- build a deck from a topic or an outline
- improve or redesign an existing `.pptx`
- continue work on a presentation you already have in Slidely

## Setup

1. Install this plugin.
2. Connect the `slidely` connector when prompted (OAuth, one time).

Once connected, tools such as `initialize_working_presentation`,
`list_design_systems`, `message_slidely`, `select_layouts`, and
`review_slides_batch` become available, along with the `list_skills` /
`read_skill` pair the skill instructs Claude to read before its first call.

Building a deck runs for minutes rather than seconds. Stay in the same
conversation and answer the questions, design and layout cards, and slide
reviews as they arrive.

## Requirements

- A Slidely account, free to create at [slidely.ai](https://slidely.ai)
- Claude Code or Cowork with plugin support

## Privacy and support

- [Documentation](https://slidely.ai/docs)
- [Privacy policy](https://slidely.ai/privacy)
- [Terms of service](https://slidely.ai/terms)
- Support: [support@slidely.ai](mailto:support@slidely.ai)

## License

This plugin package is available under the MIT License. The Slidely service and
its APIs are governed by Slidely's terms of service.
