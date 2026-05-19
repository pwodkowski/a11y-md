---
name: a11y-annotate
description: Use this skill on every UI turn that touches composition — building, modifying, restructuring, or annotating any page, component, route, form, layout, navigation, modal, focus behaviour, or live region. Make sure to use this before any UI code change, and whenever the user asks what an a11y.md captures. Triggers on phrases like "build a checkout page," "add a modal," "iterate on the form," "restructure the nav," "create a component," "annotate this prototype," "update the a11y.md," "what's the focus order," "describe the live regions."
---

# a11y-annotate

How to read, write, and maintain `a11y.md` — the markdown file that captures composition-level accessibility intent for a page or prototype.

## The loop

On every UI turn that touches composition (whether or not this skill is explicitly invoked):

1. **Read.** Walk from the file being edited up to the repo root. Open every `a11y.md` found. The union of claims is ground truth.
2. **Apply.** Make sure new code satisfies every claim relevant to the change. If the code contradicts a rationale-bearing claim the user hasn't asked to change, stop and surface it. Don't silently overwrite.
3. **Update.** After the change, reflect any new composition-level intent in `a11y.md`. Preserve every `Rationale:` line verbatim. Flag claim changes in the turn summary, not just the diff.

## Sections

`a11y.md` has nine sections in fixed order. A minimal WCAG-target header sits above section 1. All sections are always present in every file; no frontmatter, no YAML, no schema.

Empty is meaningful — when a section has no claims for this page, write a single bullet saying so, with a brief reason where helpful:

```
## Reading order
*Empty when DOM = visual. <visual> → <DOM> for any mismatch.*

- Empty — DOM matches visual.
```

The bullet signals the section was considered, not forgotten.

The sections layer from wrapper to escape:

- **Wrapper:** Landmarks.
- **Content:** Headings, Images.
- **Interaction:** Forms, Controls.
- **Consequences:** Live regions.
- **Reconciliation:** Reading order, Focus (written last; depend on everything above).
- **Escape:** Notes (last resort).

When writing claims on a new route, don't attempt Reading order or Focus before the sections they integrate have been established.

Header:

```
**WCAG target:** WCAG 2.2 AA
```

Each `##` header is followed — on the immediately next line, no blank in between — by a one-line italic prompt describing what belongs in the section. The prompts are part of the file format: preserved across edits, kept in empty sections as well as filled ones.

Sections, in order:

1. **Landmarks** — `role: <label>`. Name the page's landmark regions. Multiple of one role need labels.
2. **Headings** — `hX: <Text>`. Outline the heading structure. Note deviations from default document-order DOM.
3. **Images** — `<image>: <alt strategy>`. Decorative vs informative. Complex images get longer treatment.
4. **Forms** — `<field>: label, validation, error`. The form's internal structure: field labels, label-control pairing convention, validation timing, error association, multi-step flow. Action buttons go in Controls.
5. **Controls** — `<control>: role = <role>, name = <text>`. Interactive surfaces whose role or accessible name is a per-page composition decision: icon-only buttons, disclosure toggles, button-vs-link choices, aria-label overrides, submit / reset / cancel buttons (including inside forms).
6. **Live regions** — `<region>: polite|assertive on <trigger>`. What gets announced when. Covers errors, success, status — anything dynamic.
7. **Reading order** — `<visual> → <DOM>` only when they differ. Empty when DOM order matches visual order.
8. **Focus** — `On <event>: focus → <target>`. Where focus moves on route change, modal open, action completion, error surfacing.
9. **Notes** — Free-form. Escape hatch — populated only when no other section fits.

## Voice

- **Imperative for behaviour.** "Focus moves to the first invalid field on submit." Not "the focus should move to the first invalid field."
- **Declarative for structure.** "h1: Checkout." Not "The h1 is set to 'Checkout'."
- **Roles, relationships, rationale.** Not selectors, IDs, or line numbers. Claims survive refactors; selectors break on rename.
- **Backticks** when a literal selector is genuinely unavoidable.
- **Rationale: lines** mark deliberate deviations from defaults. Defaults don't need rationale; deviations always do.
- **Bulleted claims.** Each claim is a bullet — one line if it fits, multi-sentence if the claim warrants depth. `Rationale:` hangs as its own bullet below the claim it qualifies.

## Add, supersede, deprecate

**Add** a claim when the page introduces composition-level intent that isn't yet captured — new focus management, a new live region, a heading structure that deviates from default.

**Supersede** a claim when intent has changed deliberately. Write the new claim with a `Rationale:` line that references the change. Never delete the old `Rationale:` silently.

**Deprecate** a claim when the component or page it describes no longer exists. Mark it deprecated with a `Rationale:` line explaining why. Keep it visible for a revision or two before removing entirely.

**Never** delete a `Rationale:` line verbatim. Supersede or deprecate with new rationale that acknowledges the change.

## Examples

### Headings

**Good:**

```
- h1: Account settings
- h2s: Profile, Email, Password, Notifications.
```

Names the outline. No `Rationale:` line — H2s mirror visible section labels, which is the default; deviations need a rationale, defaults don't.

**Bad:**

```
The page has a main heading and several sub-headings for the settings categories.
```

Vague paraphrase. Names nothing — reader can't reconstruct the outline.

### Controls

**Good:**

```
- "Save changes" (per section): role = button, name = "Save <section>" — e.g., "Save profile".
- "Change avatar" (opens picker modal): role = button (opens modal), name = "Change avatar".
- Logo (clickable, returns home): role = link, name = "Home" (overrides image alt).
- Rationale: Save buttons named per section so screen reader users hear which save fires. Modal-open uses button role because it changes UI state, not route.
```

Names each control, its role, its accessible name. Calls out role/name choices that override defaults.

**Bad:**

```
Each section's save button has a clear label, and the avatar dialog opens correctly when clicked.
```

Sounds descriptive but is unfalsifiable. "Clear label" and "opens correctly" — testable how? Doesn't name the role, name, or any decision.

### Focus

**Good:**

```
- On modal open (avatar picker): focus → close button.
- On modal close (Esc or close button): focus → invoking control ("Change avatar").
- On validation error at save: focus → first invalid field within that section.
- On save success: focus stays in the saved section; status message announces separately (see Live regions).
- Rationale: Moving focus on success would steal it from the user's flow when they're likely about to keep editing. Polite live region carries the confirmation instead.
```

Each beat names event and target. The save-success beat documents a deliberate non-move with its reasoning.

**Bad:**

```
Focus moves around the settings page as needed and the avatar modal handles it correctly.
```

"As needed" and "correctly" are unfalsifiable. Names no event, no target.

### Live regions

**Good:**

```
- Save success: polite ("Profile saved.", "Password changed.").
- Validation errors: assertive on save attempt.
- Rationale: Errors interrupt because the user can't move on without acting. Save confirmations don't interrupt — the user can keep working.
```

Names what, when, how, why.

**Bad:**

```
The page announces saves and validation errors when they happen.
```

No politeness level, no trigger, no actual message text.

### Superseding (not deleting) a Rationale: line

**Wrong:**

- Remove `h1: Profile. Rationale: matches the visible page title.`
- Replace with `h1: Account settings.`

**Right:**

```
- h1: Account settings
- Rationale: Renamed from "Profile" — supersedes prior claim. Page now spans profile, email, password, and notifications, not just the profile fields.
```

Old rationale acknowledged; new rationale carries the change.
