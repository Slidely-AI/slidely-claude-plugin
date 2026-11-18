# About

This reference explains some common scenarios that occur when a user is trying to make a presentation. It tells you how to best navigate those scenarios using Slidely.

# Scenario 1: User has a vague topic, no source material 

A common scenario is that the user has a vague topic, a rough idea in their brain what they want to present, and no source material.

You should discuss with the user first about the topic, about the narrative, ask them the right questions to get the best out of them, do research and analysis. Ask the user if they'd like you to consult with Slidely regarding the narrative, what questions to ask, what framework to use for the storyline. If the user wants to use Slidely at this stage, pass Slidely all the relevant context regarding the presentation user wants to create, and tell Slidely what you want -- a good narrative, or questions, or which framework for storyline should you use.

Once the storyline is discussed, you can tell user that you can help them create the data for all the slides at once, or first for a few core slides, and develop those slides.

# Scenario 2: Developing a few slides

The user might want to take their presentation creation slowly and very meticulously.

Imagine a scenario where the user has developed a storyline with your help, and they want to first nail down their core slides. You can use Slidely in two ways here:

## 1. For creating design previews (images, not actual slides) for those slides

If the user wants granular control over the design of the slide, you may pass Slidely the data for the few slides user wants to see design previews for and tell Slidely to generate the design previews for each of the slide. Also tell slidely about any constraints, any design ideas that you discussed with the user regarding these. Slidely can itself come up with design ideas so you don't need to tell it if not already discussed anything with the user. Slidely may ask the user (or you on user's behalf if user relayed that responsibility to you) to choose preferred layouts for the slides.

## 2. Creating those slides directly

This is the more common flow where the user wants to get to v1 of their slides quicker.

In this case, you pass the data for the few slides to be developed, give slidely the context about the entire presentation and that you're giving it a few slides' data first, and tell Slidely to use the data to create the slides. Again, slidely may ask the user to choose the layouts, may even confirm the slide wise data + layout choice for that slide with the user.

Once the slides are created, Slidely will show the user a card in the Slidely panel to add those slides to their working_presentation, or to provide feedback on what to revise. Slidely agent is quite fluid in that sense.

But once approved, make sure that the slides are added to the working_presentation (check the working_presentation state with `get_slide_details_v1` tool). Basically just inform which slides are accepted, what to revise in which slides. If the user does it from the panel instead, the panel has already applied it and told Slidely; you only hear what they did.

# Scenario 3: Creating the entire presentation at once

Pass slidely the storyline and the slide wise data (if the user wants slidely to develop the slide wise data, you can ask slidely to do that as well, just give it the context of the presentation and what you already have discussed with the user). Slidely may ask you for preferred layouts before laying out the layout choice for each slide and its final "to be presented" content. Slidely may share a few choices for the slide creation plan -- if the user wants to see some sample slides first, if the user wants to create all the slides then which slides to be created together in a batch etc.

Once Slidely has created the slides, it will send them as a card in the Slidely panel for the user to review (or for you to review on user's behalf). Each slide will have an id or a label that you can use to refer to it. The user can accept the slides or give feedback on them via the UI or via you. You can use `answer_cards` to decide for each row whether to accept it into the working presentation, or revise it. It does two things -- adding accepted slides to the working presentation (or replacing existing slide with the newly created one in case of some edit/re-design), and sending your feedback to slidely via normal chat message.

# Scenario 4: User wants to download all the slides created so far

The user may want to download the working_presentation (where all the accepted slides live), or all the slides that have been created so far and not necessarily accepted. Slidely keeps track of all the slides, and just by sending a message to it, you can get any of these that you want.

# Scenario 5: Creating a new presentation from a given pptx, and a template

It is quite common for a lot of consulting companies/agencies that they have a standard presentation where all data and assets are fixed, but they need to replicate it in their client's template. Slidely can directly help with this.

Initialize the working presentation from the content presentation they already have (`initialize_working_presentation_v1` with the file — this is the import path on every host, with or without the Slidely panel), and set the design system as the template presentation (`set_design_system_v1` takes a pptx file). Then you can tell Slidely that you want to convert the content presentation into the client's template preserving all details and data as is.

Slidely may ask the user which layouts do they prefer from the client's template if it has a lot of them.

You can ask Slidely to propose which layouts from the client's template would be most suitable for each slide in the presentation, and once agreed upon that, you can tell slidely to redesign the slides into the new template which is set as the design system. Tell it to use the client's templates colorscheme and fonts, and verify the same when slidely is done.

# Scenario 6: Brainstorming

When the user is trying to present to some very important stakeholders and wants a lot of discussion about the story, the data to put on, the sources, you can also use Slidely as a brainstorming partner. Slidely is a smart agent, and it is guided in a way to do think in terms of presentation frameworks, do analysis from data, do research accordingly. You can use Slidely to find out specific data, analyze pdfs, excels, take out images from documents etc.