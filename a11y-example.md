**WCAG target:** WCAG 2.2 AA

## Landmarks
*Roles and boundaries. role: <label>. Multiple of one role need labels.*

- banner
- navigation: "primary"
- main
- footer

## Headings
*Outline. hX: <Text>. Note deviations from document-order DOM.*

- h1: Zumba with Pawel
- h2s: Classes, About Pawel, Stay in the loop.

## Images
*Decorative vs informative. <image>: <alt strategy>.*

- Hero photo: informative, alt = "Pawel leading a Zumba class, mid-step, arms raised."
- About photo (Pawel headshot): informative, alt = "Pawel, smiling, in a Zumba instructor shirt."
- Background pattern in hero: decorative
- Social icons in footer: decorative — names live on the enclosing links (see Controls).

## Forms
*Form structure: fields, validation, error association. <field>: label, validation, error.*

- Newsletter signup: single email field. Label "Email address — required" visible above the input (not placeholder). Field is aria-required; input declares the email autocomplete purpose so browsers can autofill.
- Validation: format on blur, presence on submit.
- Error association: input uses aria-describedby pointing to the error message below it. Error text names the field, states the problem, and tells the user how to fix it — e.g., "Email address is required." for empty submit; "Enter an email like name@example.com." for bad format.
- Rationale: Single-field form; spelling "required" into the label beats an asterisk that would need a separate legend.

## Controls
*Interactive surfaces. <control>: role = <role>, name = <text>. Submit / reset / cancel buttons included.*

- "Book a class" (hero CTA): role = link, name = "Book a class", target = /book.
- "Subscribe" (newsletter): role = button, name = "Subscribe to newsletter".
- Primary nav: role = link, names = "Classes", "About", "Contact".
- Social icons in footer: role = link, names = "Pawel on Instagram", "Pawel on Facebook", "Pawel on TikTok" (icons decorative; name via aria-label).
- Logo (clickable, returns home): role = link, name = "Home" (overrides image alt).
- Rationale: "Book a class" is a link because it navigates to a separate route, not a UI state change. Social icon links named by platform, not by glyph.

## Live regions
*What announces what. <region>: polite|assertive on <trigger>.*

- Newsletter success: polite ("Almost done — check your inbox to confirm your subscription.").
- Newsletter error: assertive on submit attempt.
- Rationale: Errors interrupt because the user can't subscribe without acting. Success doesn't interrupt — it just confirms.

## Reading order
*Empty when DOM = visual. <visual> → <DOM> for any mismatch. Written after the sections above.*

- Empty — DOM order matches the visual top-to-bottom flow.

## Focus
*Where focus moves. On <event>: focus → <target>. Written after the sections above.*

- First tab stop: skip link → main.
- On newsletter validation error: focus → first invalid field.
- On newsletter submit success: focus → the success message.
- Rationale: Moving focus to the success message confirms the action audibly and gives a clear next position, rather than leaving the user in a now-cleared input. (Different default from a settings/save flow, where the user is mid-edit and should not lose their place.)

## Notes
*Free-form. Escape hatch — populated only when no other section fits.*

- Reduced motion: any animation respects prefers-reduced-motion.