# About

This reference explains some common scenarios that occur when a user is trying to improve/edit their presentations.

Slidely can handle most of these scenarios directly, you can explain to slidely what the user wants and get it done.

# Scenario 1: User wants to make a specific edit on one slide

For example, change the bar chart to a pie chart, and add callouts on the biggest category.

Tell slidely the slide number in the working presentation that the user wants to edit, and what edit they want to make. Let slidely lead the flow from them, and review the outputs.

# Scenario 2: User wants to re-design their existing slide

Slidely can come up new design ideas for the user's existing slides. Just specify the constraints if the user has any -- don't use a lot of icons, use an infographic, use a chevron-based process diagram. Don't have to be overly specific, just simply mentioning should work.

Slidely may propose what it calls design previews, they are not actual slides, they are just image previews of how the slide may look like. The previews might have some minor issues like not following the fonts, or the layout to the dot, but those should be ironed out when the previews are created into actual slides.

The previews arrive as a card in the Slidely panel, where the user picks directions. The choice is theirs by default; when they have delegated it to you, send the review through `message_slidely_v1` in this exact shape — the agent matches picks by the labels it wrote:

```
Create slides from these design inspirations:

Slide ID <id>:
- <option label>
- <option label> — Comment : <what to keep or change>

Slide ID <id>: skipped
```

# Scenario 3: User wants to re-design their slide in a different layout

Some times, the user may want to change the master layout of their slide, for example from a simple  `Title and content` layout to a more specific `Two comparison`  layout. Slidely is capable of doing that. It may ask for clarification regarding which layou to use exactly, or what to not change etc.

# Scenario 4: User wants to make a consistent edit across slides (breadcrumbs, aligning titles etc.)

Slidely when instructed to can make consistent edits across multiple slides like adding the same styled breadcrumbs across all slides, making all titles 25 font sized, adding a slide number to all slides etc.

# Scenario 5: Coming up with new design ideas

When re-designing slides, you can see the slides, give rough ideas, or very detailed ascii structure to slidely, and it can use either. You (with discussion inputs from the user) can tell slidely to try a map instead of a table, or an infographic instead of a list. You yourself can create a rough sketch of the slide in ascii and give to Slidely.

Basically, slidely can accept everything from rough ideas to very detailed prompts for re-designing slides. Discuss with the user how they want it, and visualize the ideas using Slidely's previews (just ask slidely to give design previews) before creating the actual slides (creating slides takes more time and credits compared to just previews).