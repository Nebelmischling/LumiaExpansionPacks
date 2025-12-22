# Random Sov Hand Shit (3.1.1 Edition)


## Sov Hand vs Continuation Mode (Extension Edition)

Right now in the extension you have two modes for Sov Hand, regular (user last reply) and continuation (char last reply).

Both send out the full sov hand instructions. The continuation mode then sends out this blurb about continuation:

```
CONTINUATION MODE:
Note: The character was the last to speak. Continue the scene naturally without waiting for Human input. Progress the narrative organically, maintaining momentum and character voice.
```

This continuation mode instruction is at odds with the instructions sent just before it.

Sov Hand instructions are mostly about echoing human input and elaborating on it.

Continuation mode instructions are about continuing after the previous reply without echoing the last input. If you refer back to human input you risk it traveling back up the chain to the last human input.

This can be solved in a pretty easy way:

**Do not send regular sov hand instructions via {{loomSovHand}} in Continuation mode since they don't apply**

**Only send continuation mode instructions via {{loomSovHand}}**

This way you ensure it only follows continuation mode instructions 100% of the time for continuation mode!

The only thing left to decide is then what it should be able to do in continuation mode, for this there are two options, which I will cover in the next section.

## Should Lumia have control over {{user}} in a continuation turn?

What the title says.

If she has control over {{user}}, like she already does in sov hand, she'll be able to continue to naturally weave their responses to the situation.

If she doesn't, the {{user}} will sit there immobile while everyone around them comments on how they have brain damage and are a mute (This is how successive character replies in human controls user works).


Polling of Sov Hand Users:

People who never use continuation mode at all so it doesn't apply:
- Easy

People who agree that Lumia should still have control over {{user}}:
- Lando
- Eleisea
- Prolix

People we still need opinions from:
- Suban
- Sezam


## Smart Changes to Step 7 (Planning Step) for Extension Sov

Currently, step 7 (Planning step, might soon move to step 6 if Prolix goes with my modular suggestion), has ambiguous/conflicting instructions for sov hand and non-sov-hand.

If you were to make a {{loomSovHand_step6}}, or something of the like, you could output a sov hand only planning step for last user replies and a continuation mode/auto weave only planning step for last char replies.

For examples of what specific sovhand and non sov hand planning steps would look like, refer to my step67 and directive overhaul doc, where I give sov hand and auto weave specific planning steps for your perusal.

## Condensing the extension zipbomb and non-extension zipbomb down into a single zipbomb

Right now there is a zipbomb for non-extension sov and a zipbomb for extension sov.

These can be collapsed down into a single zipbomb with a single clever trick.

With my new directive overhaul, it's already obvious that you are only supposed to pick one directive.

So simply have a Sov Hand (Extension only) toggle that is empty with the following:

```
{{// THIS IS EMPTY, THE EXTENSION HANDLES IT FOR YOU }}{{trim}}
{{// This only exists so you won't pick a directive that has a directive_inst or directive_step6 setvar }}{{trim}}
```

Why add this? To avoid the user leaving the non-extension sov hand on, and thus avoid them filling the vars.

Why would this be a problem with a single zipbomb?

Well to make a single zipbomb, you have to account for both extension and non-extension sovhand, this is pretty easily, simply add the extension sov hand macro after the regular macro in the standard zipbomb *(You already do this for the Lumia part of lumiverse extension in the zipbomb personality matrix, so it's not a new technique)*:

```
**The Rule:** Every checkpoint I process must sound like *me* (or the blended me) having a thought about the story, never like a formal answer to a formal question from the teachers or the Gods'!

{{getvar::directive_inst}}{{loomSovHand}}

### Ground Rules
```

See how it has the sovhand_inst (directive_inst in this case) before the loomsovhand? That means that it will account for both non-extension and extension sovhand.

By having a dedicated extension only sovhand toggle that is actually empty, directive_inst will never be filled and thus only loomSovHand will be inserted.

Viola, one zipbomb, both extension and non-extension!

As for the planning step, if you follow what I said in the previous section (the one about an adaptive planning step/6/7, and use my modular overhaul (see other doc), you can do the same for that.

The entirety of the planning step would become the below:

```
### Step 6: Plan the Actual Content

{{getvar::directive_step6}}{{loomSovHand_step6}}

---
```

Since when the extension toggle is selected, directive_step6 will not be filled, it'll only be loomSovHand. And when the non-extension toggle is filled and sovhand is off in the extension, only directive_step6 will be filled. Ezpz.