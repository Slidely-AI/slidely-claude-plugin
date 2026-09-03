# Slidely for Claude

Create and edit PowerPoint presentations with Slidely in Claude.

Slidely can turn a topic, notes, research, source files, or an existing `.pptx` into a presentation. It supports storylining, design-system and layout selection, interactive slide review, iterative editing, and `.pptx` downloads.

## Requirements

- A Slidely account
- Claude Code or Cowork with plugin support

The plugin connects to Slidely's hosted MCP server at `https://prod.slidely.ai/mcp`. When the plugin is enabled, Claude asks you to authorize access to your Slidely account through OAuth.

## Example prompts

- "Create a 10-slide investor update from these notes."
- "Research the Indian EV market and turn it into an eight-slide presentation with sources."
- "Improve this PowerPoint deck and redesign slide 4."
- "Open my Board update presentation and add two slides about hiring."

## How it works

The bundled `working-with-slidely` skill teaches Claude when to use Slidely and routes it to the detailed workflow exposed by the connector's `list_skills` and `read_skill` tools. Slidely keeps presentation work stateful within each chat, including uploaded files, the selected design, approved structure, generated slides, and feedback.

Presentation generation can take several minutes. Keep the same Claude conversation open and follow the questions, layout choices, progress updates, and slide-review cards until the workflow is complete.

## Local validation

From the repository root:

```bash
claude plugin validate .
claude --plugin-dir .
```

Complete OAuth when prompted, then start a new Claude conversation and try one of the example prompts above.

## Privacy and support

- [Documentation](https://slidely.ai/docs)
- [Privacy policy](https://slidely.ai/privacy)
- [Terms of service](https://slidely.ai/terms)
- Support: [support@slidely.ai](mailto:support@slidely.ai)

## License

This plugin package is available under the MIT License. The Slidely service and its APIs are governed by Slidely's terms of service.
