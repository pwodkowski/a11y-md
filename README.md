# a11y-md

The accessibility annotation layer your prototype's been missing. Markdown, colocated with code, read by every agent (or human) that touches it.

## The annotation problem we never actually solved.

Hard truth: as an industry, we never solved accessibility annotation. We tried annotation libraries, sticker sheets, plugins — all the same shape, all glued on top of a design that keeps changing.

It hasn't worked, not at any scale that matters. Every accessibility specialist has been through this. More than once. Every rollout, every evangelisation push — the same three complaints come back:
- Hard to start early, when you know the design is going to change.
- Hard to maintain through that change, when the team just wants to ship.
- And when you do persist, you get annotations on top of annotations, no way to tell which is true.

All three are real. We kept trying to solve this by changing the annotations. New kits, better stickers. More documentation, better examples. But the annotations were fine. They just had nowhere good to live.

### How do we annotate a hundred iterations?

Pick any vibe-coded prototype your team works on and count the prompts. Ninety-two. A hundred and four. Two hundred and thirty. Every prompt is an iteration. The problems multiply — and now there's a new one underneath them. The agent is writing code you don't read. You check what you can see, but you can't control what happened between dozens of rounds of changes.

## Meet a11y.md.

One file. Plain markdown, sitting in the repo with the code. A primitive — small, does one thing, plays well with the rest of your stack. The agent reads it before every UI change, and updates it after. It's human-readable. You're a human. The annotation rides with the work now.

## What's in the file

Accessibility annotations. Just that. Split across nine categories, always the same, from the outside in: Landmarks, Headings, Images, Forms, Controls, Live regions, Reading order, Focus, Notes.

Inside each: a list of small claims. When a claim isn't obvious, it carries a `Rationale:` line — the _why_. Here's one:

```markdown
## Focus

*Where focus lands, where it returns, what traps it.*

After submit, focus moves to the error summary
at the top of the form, not to the first invalid
field.
Rationale: screen-reader users need to hear what
failed before retrying. First-invalid-field skips
the count.
```

The italic line under the header is part of the file. It tells the next reader — human or agent — what belongs in this section, so nobody has to guess. Every section has one.

Sections with no claims for this page stay present, header only. Empty is meaningful — it says _no claims here, on purpose_.

For a full file populated end-to-end, see [`a11y-example.md`](a11y-example.md).

## The magic happens in the loop

Every turn that touches UI starts the same way. The agent opens [`a11y.md`](a11y.md) and reads it, walking up the tree if needed.

The file is ground truth, so if the code and the file disagree, we assume the code is wrong. Disagreements get reconciled deliberately, between you and the agent — never by drift.

When the agent hits a `Rationale:` line that contradicts what you've asked for, it stops. Surfaces the conflict. Asks which one wins.

Then the work happens. The agent updates the file before the turn ends, including the decisions you didn't see it make. The file keeps up with each iteration, and you can verify it just by reading.

The full loop closes: read before, work on UI, write after, verify.

## The recipe

To annotate your prototype, in any of the vibe-coding tools, you need two things plus a third optional one.

**The [`a11y-annotate` skill](skills/a11y-annotate/SKILL.md)** — the writing spec. Section definitions, voice, examples of good and bad claims, when to add a `Rationale:` line and when to leave it off. This is what teaches the agent how to write a paragraph that belongs in the file.

**Wiring** — a couple of lines in [`AGENTS.md`](AGENTS.md), or [`CLAUDE.md`](CLAUDE.md), or whatever your tool reads first. It points the agent at the skill, and at the file, and at the loop: read before, update after.

That's the recipe.

The [`a11y.md`](a11y.md) template is the optional third. An empty file with the nine section headers already in place, and a short italic prompt under each. Optional, but it's how you guarantee the skill writes against the right structure.

## Install

Three things to put in place — exact paths depend on your agent, but the shape is the same.

1. **The skill.** Copy `skills/a11y-annotate/` to wherever your agent loads skills from — globally for all projects, or scoped to a single one. Your tool's docs will name the path.

2. **The wiring.** Paste the contents of [`AGENTS.md`](AGENTS.md) into the file your agent reads first on session start (`AGENTS.md`, or your tool's equivalent). It's a few lines under an `## Accessibility` heading — safe to merge with existing instructions.

3. **The template** *(optional)*. Copy [`a11y.md`](a11y.md) into your repo. The agent can generate one from scratch, but starting with the scaffold guarantees the right structure.

## Five things we kept out.

The agent reads and writes this file constantly, so we kept it small on purpose. Five things we left out, and what we chose instead:

**WCAG requirements.** The file defines the WCAG target at the top, and that's all the agent needs to know which conformance level to aim for.

**Component behaviour.** The agent reads the design system directly, so it already knows how each component is meant to behave.

**Approval tracking.** Handled by git or other versioning systems.

**Structure.** Prose under named headings. No schema, no frontmatter, no pinned selectors or IDs. The file describes what should happen; the code is where the specifics live.

**Whole-codebase manifests.** Each `a11y.md` is scoped to a view or a route, so the file stays small enough to read on every turn.

## Start today, with one file.

Pick a prototype you're already iterating on. Add the skill, wire up the instructions, drop an empty `a11y.md` in the root. Then ask the agent to annotate it.

Or, even better: wire it all into your vibe-coding template once, and let it travel with every prototype you start.

Then watch the file. See what the agent writes. See what changes when you iterate. That's the whole loop.

If you try it, I'd like to hear what happened.

https://linkedin.com/in/pawelwodkowski

## License

MIT — see [LICENSE](LICENSE).
