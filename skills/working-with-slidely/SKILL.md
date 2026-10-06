---
name: working-with-slidely
description: Create, import, review, and edit PowerPoint presentations with Slidely. Use this whenever the user wants a deck built from a topic or outline, wants an existing .pptx improved or redesigned, or asks to continue work on a presentation they already have in Slidely.
---

## Slidely is a stateful presentation agent, not a set of one-shot tools

Slidely builds and edits PowerPoint decks. You work with it through a chat, and Slidely remembers everything in that chat — the uploaded files, the chosen design, the approved storyline, the slides it has proposed and the feedback you gave. A message asks it to do something; it decides the next useful step and may ask a question before acting.

The user owns the presentation. Slidely proposes, the user approves, and slides only enter the deck through an explicit review.

## Where the working instructions are

This text is only the overview. The detailed workflow — how to initialize a presentation, when to involve Slidely, how design systems and layouts are chosen, how to answer the cards Slidely raises, and who decides what — is in the skills served by two tools: `list_skills` lists them, and `read_skill` returns the one you name.

Those skills show how to use Slidely to create, edit, redesign and review presentations. Accepting slides, choosing layouts and setting a design each have their own tool, and a message to Slidely is not a substitute for any of them.

A skill may also list extra files under `files`. `read_skill` returns the whole skill, or only one of those files when given its `path`.
