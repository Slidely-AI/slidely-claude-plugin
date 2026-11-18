---
name: working-with-slidely
description: Create, import, review, and edit PowerPoint presentations with Slidely. Use this whenever the user wants a deck built from a topic or outline, wants an existing .pptx improved or redesigned, or asks to continue work on a presentation they already have in Slidely.
---

## Slidely is an AI agent programmed to be a stateful presentation collaborator

Slidely is a presentation-specialist AI agent, not a collection of independent tools with definitive actions.

You collaborate with Slidely on an existing or a new presentation (called the working presentation), and you do so via a chat. A Slidely chat is stateful. Within the same `chatId`, Slidely remembers the conversation, uploaded files, design system and layout selections, plans, artifacts, generated slides, rejected alternatives, and earlier feedback. It can also inspect the live working presentation when it needs slides' content/visuals. And it can create/edit slides and offer to add newly created slides to the working presentation, or replace existing slides in the working presentation. The actual operation is approval gated.

Use `message_slidely_v1` to continue this conversation. A message to Slidely may result in questions, suggestions, layout selection, slide preview images, slide candidates etc. Do not assume that sending a message about creating or editing slides will cause immediate execution. Slidely decides what the next useful step is and may ask for clarification first.

The other tools packaged with the Slidely MCP are deterministic helpers: they set up the working presentation and chat, set a design from a file, answer cards on the user's behalf, and read a turn's result.

## Decide when to involve Slidely

When the user is clear about how they want to work, follow their lead.

If the user wants to discuss the argument, evidence, narrative, or slide content before beginning presentation work, continue that discussion with the user. Do not set Slidely up merely because the eventual output may become a presentation.

If the user wants Slidely involved from the beginning, set up the presentation and allow Slidely to help develop the storyline, ask questions, structure the content, and create the slides.

### Default when developing a presentation

When the user asks to create a presentation but provides only a topic, goal, or rough idea, treat the request as permission to begin the presentation workflow — not as approval to choose the presentation's argument or storyline.

Unless the user has explicitly delegated narrative decisions:

1. Discuss or research the topic as needed.
2. Develop a concise proposed storyline or slide outline.
3. Show the proposed storyline to the user.
4. Ask the user to approve or revise it.
5. Stop and wait for the user's response.

Do not ask Slidely to create slide-wise data, design previews, or actual slides until the storyline has been approved.

You may skip storyline approval only when:

- the user provides an already-approved outline or slide structure; or
- the user explicitly delegates narrative decisions with instructions such as "use your judgment," "you decide," "just create it," "create all the slides directly," or "don't ask me questions."

A general request such as "create a presentation about X" is not explicit delegation. Choosing a design system or preferred layouts does not count as storyline approval.

Before approval, you may research the topic and draft a proposed storyline yourself. Consult Slidely about the storyline only when the user has asked for Slidely's involvement or agreed to involve Slidely at that stage.

After the storyline is approved or narrative authority is explicitly delegated, engage Slidely to develop slide-wise data, choose layouts, and create the actual slides. The user may want to create one slide, several core slides, or the entire deck. The reference file `references/presentation-creation-scenarios.md` shows some common scenarios that may occur when the user wants to create a new presentation.

When Slidely comes back with slide-wise data or proposed creation batches, that decision belongs to the user. Show it to them and wait for their answer. Do not approve it and tell Slidely to create the slides yourself unless the user explicitly said to create the slides directly or not to ask them anything. An earlier approval of the storyline, the design, or the layouts does not count as approval of this.

### Default when editing an existing presentation

When the user uploads a pptx file they want to edit, the default is that you handle the discussion related to the flow of slides content, ideas about how to concretely improve the content. Slidely should handle the proposals of new designs for one or more slides, bringing the new re-designs to life into actual slides, making specific edits to the slides. You can orchestrate the process, review Slidely's outputs, while discussing with the user. The reference file `references/presentation-editing-scenarios.md` shows some common scenarios that may occur when the user wants to edit a presentation they already have.

## The concept of working presentation, chat, design system, and layouts

`working_presentation`: The presentation the user is working on. A new one contains no slides; as slides get created they are added to it after review. When editing an existing presentation, that presentation is the working presentation. Each has its own unique id (a uuid).

`chat`: The thread of conversation with Slidely, with its own unique id (a uuid). Within the same chat, Slidely has access to all the messages it received previously. `message_slidely_v1` is the only way to say something to Slidely.

`design_system`: The collection of slide masters, master layouts, colour scheme, font scheme and shape components that slides are built from. Every chat needs one before any slide work. If the user asks for a different design later, setting it again changes the design of the same chat: slides already built keep their look and new slides use the new design. Design systems are created by the setup tools, never by Slidely itself — do not send a design .pptx over `message_slidely_v1`.

`layouts`: Every slide Slidely creates follows a master layout from the design system. A design system may have many layouts (sometimes >200), so Slidely may at some point ask which layouts the user prefers. This shows up as a card the user picks from.

## The Slidely panel

On hosts with the panel (`open_deck_panel` is among your tools), the panel is the one place the user sees and acts on everything: it lets them start or pick a deck and choose its design, it shows the deck's slides live, it shows Slidely's progress while a turn runs, and it holds every card Slidely raises — the questions, the layout picker, the slide plan, the design inspirations, the infographic browser, the proposed slides, the delete or reorder confirmations. The user answers those cards in the panel. The panel keeps itself current, and it posts into this conversation when a turn finishes or the user answers a card, so you never poll. On hosts where the panel's hidden notes do not reach you, its messages end with the Slidely chat id in brackets, and the turn id when a turn finished: use that chatId for `message_slidely_v1`, and read a finished turn with `get_turn_result_v1` before replying.

## Setting up: the presentation, the chat and the design

Two things are needed before Slidely can work: a working presentation and a chat on it, and the chat needs a design system before any slide work.

### Hosts with the Slidely panel

Set the deck up first and open the panel after, so it opens ready to work on:

1. Unless the user already said, ask in one short message what they are starting from: a .pptx deck of theirs to edit, a .pptx template of their own to build in, or a new deck. Ask them to attach any file to their message here; the panel cannot take files. A request for a presentation on a topic does not answer this, so ask before creating anything.
2. Set it up:
   - A deck to edit: `initialize_working_presentation_v1` with the file in `files`. It keeps its own design.
   - A template, or a design they named: `initialize_working_presentation_v1` with no file, then `set_design_system_v1` on the chat it returned, with the template in `files` or the name.
   - A deck they @-mentioned: `start_deck` with the presentationId at the end of its `presentation://slidely/<id>` link.
   - A new deck with no template: `initialize_working_presentation_v1` with no file.
3. Call `open_deck_panel`. Its result says what the conversation has and what to do next; follow it. A new deck without a design opens on the design picker: tell the user in one sentence to choose a design there, and end your reply. To continue a Slidely deck they did not @-mention, open the panel without setting anything up; they pick the deck there.
4. When they pick a design in the panel, it says so in a message from them ("I've chosen the design … in the Slidely panel.") carrying a hidden note with the presentation id, the chat id and the design. The design is set; do not call a tool to set or confirm it. Use that `chatId` for `message_slidely_v1`, and carry on with the deck.

Open the panel once in a conversation. It stays open and keeps itself current — the slides, Slidely's progress and every card — so do not open it again after a turn, a reply or a deck change; open it again only when the user asks to see it. Never answer with "see the panel"; it may not be open.

### Hosts without the panel

Use `initialize_working_presentation_v1` to create a working presentation, either empty or from the user's pptx file; it also creates a chat and returns its id. To continue a deck the user already has in Slidely, do not initialize — that would create a second deck. Call `list_presentations_v1`, show the user the names, and call `start_deck` with the presentationId they pick; it opens the chat and returns its id.

As soon as the presentation is set up, set the design. `initialize_working_presentation_v1` says whether the deck already has one: a deck imported from the user's pptx usually does, and then you leave it alone unless the user explicitly asks for a different design. When it has none, call `list_design_systems_v1`, show the user the names, and set their pick with `set_design_system_v1`. When they attached a .pptx to use as the design, pass it to `set_design_system_v1`; that build is slow, so wait for it rather than messaging Slidely in between.

Do not storyline with Slidely, ask it for slide-wise data, or ask for any slides until the design is set.

## Communicating with Slidely

Everything Slidely says comes back from `message_slidely_v1` as a message, sometimes carrying cards. The user sees and acts on them in the Slidely panel — the questions, the layout selection, the design inspirations, the infographic browser, the batch of proposed slides, a delete or reorder confirmation. A turn can raise several; each in the result's `cards` has a `card` name and a `kind`. Follow the instructions returned with them; they say what each card is offering.

When the user acts in the panel, you are told, as their own message in their words. The card is already answered and settled; never answer it again with `answer_cards`. Sort the event before calling anything:

1. **The panel already handled it.** Answers Slidely was waiting on — questions, layouts, infographic picks, design inspiration picks, a redesign to create, feedback or comments on proposed slides — were sent to Slidely by the panel itself, with a note to you saying what the user chose; Slidely is already working on it. Slides added, replaced, deleted or reordered from the panel, and a design picked in the panel, are already done. In every one of these, do not send anything to Slidely and do not call a tool to redo it. Reply in one short sentence; after a deck change, verify the deck (below); after a design pick, carry on with the step you were on.
2. **A message to relay.** Free-text feedback that is not about slides on an open card reaches Slidely only through you: relay the user's message with `message_slidely_v1`, verbatim. So does a layout the user changed on the slide data card ("Layout updated for slide 3 to …").
3. **Mixed.** An acknowledgement plus a new instruction: send only the new instruction.

Feedback about slides on an open proposed-slides card is a verdict, not a message. Asking for 3 of 10 to be changed is one `answer_cards` call revising those 3, never a relay: a relay adds nothing to the deck and leaves the card waiting. The rows you leave out stay open in the panel for the user.

Only when the user has explicitly handed the decision to you — in this conversation, in so many words: "just do it", "don't ask me, go ahead", "you have full control" — or named the choice themselves, answer a card yourself rather than waiting for them. The handover holds for the rest of the conversation until they ask to be consulted again. A request is not a handover: "delete slide 2" produces the confirmation card for the user to press, and only an explicit handover lets you press it. Without one, every card is the user's.

`answer_cards` answers cards exactly as the user would in the panel, and the panel shows them answered from the chat. It takes the `chatId` and `turnId` of the turn that raised them and one answer per card, named by its `card` and `kind`: the questions, the layout selection (omit the ids to let Slidely choose), the design options by `optionId`, the proposed slides row by row (accept, add_beside, or revise with a comment), and a delete or reorder confirmation (confirm applies exactly what the card shows, cancel leaves the deck alone). It applies the choices and sends Slidely what it needs in one message, so do not relay anything afterwards. It carries out only what a card already proposed, and slides a card takes out of the deck are archived in the Slidely chat, not erased. Answer every card of a turn in one call: once Slidely starts on the answers, the turn's other cards close. Saying in a message that you accept slides adds nothing; only this tool or the panel puts them in the deck. Infographic picks stay with the user.

A card takes one answer, whether the user's or yours — and never answer one the user is still deciding on.

## Seeing and showing the deck

To see the deck as it actually is rather than as the conversation described it, use `get_slide_details_v1` — ids and positions as plain data, and the slides themselves as images when you pass `images: true` with their positions. After any add, replace, delete, or reorder, check it before describing the outcome or requesting another edit: an add grows the count and the expected new ids are present; a replace keeps the count and the old id is gone; a delete shrinks the count and the removed id is absent; a reorder keeps the same ids in the new order. If the deck does not match, say so plainly — never describe an appended slide as a replacement.

Show the deck, don't only describe it. With the panel, the user already sees it live once you have opened it. Without it, `get_slide_details_v1` is the deck.

If the user asks to see an earlier step again, it is in the panel's Activity tab; say so rather than calling a tool.

If the user asks to stop something Slidely is working on, call `abort_chat`. Finished work stays; only the running operation ends.

## User vs your decision authority

The user owns the presentation. You may make decisions on the user's behalf only when the user has made the decision clear or delegated it.

By default:

- The user chooses the design system.
- The user may choose preferred layouts.
- The user approves consequential storyline changes.
- The user reviews generated slide candidates before they are added to the working presentation.

You may decide these matters when the user has identified the answer unambiguously or has said things such as "you decide," "use your judgment," or "just do it."

Delegation removes unnecessary approval loops, but it does not remove review. When acting independently, inspect the available review artifact on the user's behalf before accepting slides.

Use the current request and clearly established user preferences to judge the intended level of autonomy. Do not treat permission from an unrelated presentation task as permanent authority for future work.

If a decision is subjective and the user has not delegated it, ask the user. If a generated slide has an objective error — such as incorrect content, unreadable contrast, a misleading chart, broken hierarchy, or a visual relationship that contradicts the narrative — request a revision rather than asking the user whether the error is acceptable.

## Communicate incrementally

Slidely cannot see the surrounding conversation between you and the user unless relevant information is sent to it.

When it's time to engage with Slidely, in the first substantive message, give Slidely the user's goal and the context it needs but has not seen. This may include the audience, purpose, source material, approved storyline, even slide wise data, important constraints, and the way the user wants to collaborate.

Upload the relevant files once. Slidely retains them in the chat. This may include xlsx, docx, txt, png/jpeg/jpg/webp, pptx files.

With every message you send to Slidely, communicate incrementally. Send new information, corrections, feedback, or the next request. No need to restate the entire deck brief on every turn. Within the same chat id, it is valid to refer to earlier messages, uploaded files, proposed slide previews, created slides naturally.

If the user gives you new information that Slidely has not received, relay that information before expecting Slidely to use it.

### Let Slidely lead the flow once you start engaging

Once you start engaging with Slidely — at the very start, after a storyline is approved, after deciding on slide wise data, or when editing a presentation — let Slidely lead the flow. Tell Slidely what you want and let its response determine the next step; it may ask for clarifications, discuss a plan, or ask for layouts first.

Say what the user wants in their own terms and let Slidely choose the method. Prescribing an output shape — how many previews to make, images rather than slides, what to generate instead of what to show — forecloses the interactive cards Slidely would otherwise open for the user, such as the layout picker, the design inspirations, and the infographic browser. "Show me some infographics for slide 4" is a request to browse and pick, not a request to generate previews.

Allow Slidely to decide how slides should be grouped for creation. Slides with repeated structures may benefit from being produced together, while unrelated slides can be created independently or in parallel. Do not force all slides into one batch merely for visual consistency; the design system supplies that consistency.

## Handle active Slidely turns

After sending Slidely a message, tell the user only what you sent, then end your reply. Say nothing about the work beyond what Slidely itself has said: it chooses its own next step. The panel follows the work live and, when the turn finishes, posts the result into this conversation as a message: the status, Slidely's reply, the card's contents and any guidance. Act on that message as you would on a tool result. When the turn rendered slides or design options, their images come with that message where the host allows; when they do not, call `get_turn_result_v1` to see them. Never call it while the turn runs.

For example:

> I've sent the approved outline to Slidely. It's working on it now; the Slidely panel will show the result when it's done.

Never say where the panel is — above, below, beside, or "here".

The user can send a message while Slidely is working, to discuss something or change direction. Respond to their new request; Slidely keeps working in the background and the panel still posts when it finishes.

Treat returned states according to their meaning:

- If Slidely needs input, present or answer the question according to the user's delegation.
- If it presents an artifact for selection, let the user choose unless you already have authority and enough information to decide.
- If it presents candidates for review, inspect them before accepting or revising.
- If it returns slide-wise data or proposed creation batches, show them to the user and wait. Approve them yourself only if the user explicitly said to create the slides directly or not to ask them anything.
- If it is still processing, end your reply and wait for the panel's message carrying the result.
- If it fails, use the returned explanation and the existing chat history to recover. Do not restart the entire workflow unless the presentation or chat is unusable.

Because Slidely retains its work, most mistakes can be corrected through another message in the same chat.

## Slidely's capabilities

Slidely can:

- Analyze uploaded excel files
- Read uploaded pdf, word files, extract images from them
- Extract all details from uploaded pptx files — individual slide pptx, layout pptx, colour scheme, shapes etc.
- Create new slides from given content and selected master layout. Has the capability to
    - Add company logos
    - Add icons
    - Add relevant images
    - Add editable world/regions/country maps (like choropleth maps)
    - Add infographics like funnels, cycles, sector diagrams, sunburst etc
    - Add editable PowerPoint charts
    - Add tables
    - Add gradients, shape effects
- Make specific edits to slides ("change table to chart", "Change title to <xxx>", "Increase font sizes of..", "Change the bar chart to a donut chart with total in centre")
- Generate multiple design previews (image previews, not actual slides) from given content + layout, or for re-designing existing slides (faster and more credit efficient when the user wants to see multiple design options). This is for whole-slide design options; when the user wants to see or choose infographics, ask for the infographic browser instead
- Open a searchable infographic browser for the user — they search Slidely's infographic library themselves, pick what they like, and the panel sends their picks to Slidely
- Make consistent edits to multiple slides — adding breadcrumbs, making corner radius same etc.

Any of these might not be executed immediately if asked for, Slidely may ask for clarification, may discuss, it's an agent.
