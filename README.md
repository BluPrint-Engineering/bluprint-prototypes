# BluPrint prototypes

Approved screen prototypes for [bluprint-app](https://github.com/BluPrint-Engineering/bluprint-app), one folder per screen and one immutable folder per version (`nova-obra/v1/`). A change is a new version folder; an existing version is never edited, so a link to it always shows what was approved.

Each `*.html` page is self-contained: it embeds React, the design system, the Geist fonts and its sibling screens, and runs offline. Open it from a clone, from a downloaded file, or from the [browsing index](https://bluprint-engineering.github.io/bluprint-prototypes/).

A version folder holds:

- `<screen>.html`: the interactive screen.
- `qa*.html`: state galleries, every state in mobile 390 × 844 and desktop 1280 × 800, light and dark, with an index.
- `tickets/<version>.html`: only the states one implementation ticket builds.
- `README.md`, `HANDOFF.md`: the visual spec and the handoff.
- `source/`: the files exactly as the refinement session left them.

Implementation issues link to their file at a pinned commit. Pages are built with `.claude/skills/bluprint-design/screen-artifacts.ts` in bluprint-app.
