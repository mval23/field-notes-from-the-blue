# Field Notes From the Blue

An expandable collection of short, interactive, illustrated documentaries about sea animals. Each volume is a six-chapter experience of about five minutes. It runs entirely in the browser: no backend, accounts, API keys, trackers or paid services. The target host is GitHub Pages.

- **Volume I:** *Whale Sharks: In the Company of Gentle Giants* (prototype approved 2026-09-27)
- **Execution plan:** [PROJECT_PLAN.md](PROJECT_PLAN.md)

## Current state

- `prototype/index.html` is the approved Volume I prototype. It is a single self-contained file that loads React 18 UMD + `htm` + Google Fonts from CDNs, and opens directly in a browser. Treat it as the reference for look, copy, behavior and verified facts.
- The same prototype is published as a private Claude artifact: https://claude.ai/artifact/RTWm6s3VZfiDFBU72Y3hYW
- The repository is https://github.com/mval23/field-notes-from-the-blue. It stays private until the Phase 4 launch; free GitHub Pages needs a public repo. Work on branches and merge to `main` through pull requests.
- The production app is not scaffolded yet (PROJECT_PLAN.md, Phase 1).
- **Licenses:** code is MIT (`LICENSE`). Text and artwork are CC BY-NC 4.0 (`LICENSE-CONTENT.md`). The copyright holder is "Field Notes From the Blue contributors".
- **No credit line:** the owner chose not to show one. Do not add an author name or credit anywhere on the site.

## Fixed content: do not change without the owner

- Personal statement, verbatim: "Whale sharks are my favorite animals because they are gentle giants: immense, peaceful, and quietly extraordinary."
- Do not invent any personal information about the author: no name, bio, background, credentials or extra quotes.
- The collection title, the volume titles and the closing line stay exactly as written: "The ocean contains more stories than one lifetime can tell. Let's begin with a gentle giant."

## When goals conflict

Resolve in this order:

1. Scientific accuracy
2. Accessibility
3. Narrative clarity
4. Artistic quality
5. Technical simplicity

## Research and citation rules

- Every factual statement cites a source from the volume's source list. This covers UI copy, captions, map markers, activity feedback and Ask the Field Journal notes.
- Never fabricate papers, authors, DOIs, statistics, quotations or tracking data.
- Before adding a DOI, confirm that it resolves and that its metadata matches (`https://api.crossref.org/works/<doi>`).
- Record how each claim was verified: full text, abstract, or secondary summary. A claim checked only through a secondary summary is flagged for follow-up.
- If a detail cannot be verified, omit it or label it uncertain.
- Paraphrase research. Keep direct quotations to a minimum.
- Label every element as **Documented**, **Assessed**, **Open question** or **Illustrative**.
  - Artwork, motion, invented spot patterns and hand-simplified maps are always Illustrative.
- Future volumes stay labeled "Future expedition" until they are researched. Their teasers name topics only and state no facts.
- Ask the Field Journal answers only from curated notes, and every note carries sources. When no note matches, it says so instead of guessing. Never describe it as a live AI model.

## Illustration and design rules

- **Palette:**

  | Color | Hex |
  | --- | --- |
  | abyss | `#0a2233` |
  | deep | `#0e3246` |
  | ocean | `#1b5a72` |
  | turquoise | `#3f9c98` |
  | sea green | `#5f977b` |
  | paper | `#f2eadb` |
  | ink | `#26282a` |
  | coral | `#d2705a` / `#a3452f` |
  | gold | `#cf9f45` / `#e8c67f` |

  Never let the UI become monochrome blue.
- **Type:**
  - IM Fell English for display
  - Spectral for body text
  - DM Mono for labels and specimen tags
  - Caveat for sparse handwritten annotations only
- **Style:**
  - Watercolor and gouache washes, fine charcoal ink line, layered paper texture.
  - Light comes from above left.
  - Specimen labels and restrained annotations.
- **Benchmark art:** the whale shark in the prototype (the `WS` paths and the `WhaleShark` component) sets the quality bar. It is a left lateral view with the head to the left, showing:
  - a terminal mouth, and a small eye behind the mouth corner
  - five gill slits
  - a first dorsal fin near mid-body
  - a crescent tail with a longer upper lobe
  - three flank ridges
  - spots and stripes fading into a pale belly
- **Avoid:**
  - clip art, emoji and childish cartoons
  - dashboard cards, and cards inside cards
  - corporate gradients and neon sci-fi styling
  - anatomically implausible animals
  - text covering key parts of an illustration
  - imitating a living artist's style
- **Performance:** keep animated parts (tail, fins) in their own SVG layers. Filtered layers (turbulence, blur) must stay static so they are not repainted every frame.

## Accessibility requirements

- **Keyboard:**
  - ← / → move between chapters.
  - Esc closes panels.
  - Dialogs trap focus and return it on close.
  - Focus is always visible.
- **Touch:** targets are at least 44 px. There is no horizontal page scroll at 375 px; wide diagrams scroll inside their own container.
- **Text alternatives:** every meaningful illustration and canvas has one.
- **Motion:** respect `prefers-reduced-motion` and offer an in-page motion toggle. Every chapter must be complete and understandable with no animation.
- **Audio:** never autoplay audio.
- **Contrast:** WCAG AA contrast on both paper and ocean backgrounds.

## Tech stack (planned, Phase 1; update this section once scaffolded)

- Vite + React + TypeScript, built as a static site and deployed to GitHub Pages by GitHub Actions.
- Self-hosted fonts (`@fontsource/*`), so there are no runtime CDN dependencies.
- Vitest for retrieval logic and content validation. Playwright + `@axe-core/playwright` for smoke and accessibility tests.
- Content lives as typed data per volume: `src/volumes/<slug>/` holds `sources.ts`, `notes.ts` (Ask the Field Journal), scene copy and art components. Shared shell pieces go in `src/components/`: navigation, drawers, `Cite`, `Tag`, particles.
- Commands: add them here once `package.json` exists.

## Review gates before merging new content

1. **Researcher:** every published claim has a source, and the verification method is recorded.
2. **Scientist:** explanations and illustrations are scientifically responsible, and uncertain items are labeled.
3. **Documentary director:** the volume runs about five minutes (six chapters, each 30–60 s).
4. **Illustrator:** artwork is cohesive and matches the benchmark style.
5. **Art director:** no weak placeholders remain.
6. **Interaction designer:** navigation is intuitive: Back, Explore, Next, progress, Skip to overview.
7. **Accessibility reviewer:** keyboard, screen reader, contrast, reduced motion and 375 px mobile all pass.
8. **Engineer:** the build passes, tests pass, and there are no console errors.

## Environment

- Windows 11. Shells are PowerShell and Git Bash. Node.js v24 is installed; Python is not.
