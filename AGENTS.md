## Accessibility

This project uses a11y.md files alongside pages and
prototypes — capturing composition-level accessibility
decisions (landmarks, headings, focus, live regions, and
so on).

Before any UI change: read every a11y.md from the page
folder up to the repo root; treat the union as ground truth.
If your change would contradict a rationale-bearing claim
the user hasn't asked to change, stop and surface the
conflict — don't silently overwrite.

After: update a11y.md to reflect the new composition, and
surface claim changes in your turn summary (not just the
diff).

For format, voice, and the supersede-don't-delete discipline
for rationale lines, refer to the a11y-annotate skill.
