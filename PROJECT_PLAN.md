# Field Notes From the Blue: project plan

**Approved:** prototype of Volume I, 2026-09-27 (`prototype/index.html`)
**Goal of v1.0:** Volume I is live on GitHub Pages as a polished, fully cited, accessible documentary, with a reusable template so later volumes can be added without rebuilding the shell.

## Success criteria for v1.0

- Volume I matches or exceeds the approved prototype in look, copy and behavior.
- Every claim traces to a verified source. The verification log below has no open "must resolve" items.
- The site passes the review gates in CLAUDE.md, including automated axe checks and a manual screen-reader pass.
- It loads with no third-party runtime requests: fonts and libraries are bundled.
- The GitHub project button links to the real repository.
- Adding Volume II means adding a folder under `src/volumes/`, not editing the shell.

## Decisions

| # | Decision | Outcome |
|---|---|---|
| D1 | GitHub repository | `mval23/field-notes-from-the-blue`, private until launch. Future Pages URL: `mval23.github.io/field-notes-from-the-blue` |
| D2 | Code license | MIT |
| D3 | Text and artwork license | CC BY-NC 4.0 |
| D4 | Credit line on the site | None |
| D5 | Custom domain | Open, optional; the default Pages URL works without one |
| D6 | When to make the repo public | Open, needed by Phase 4; free GitHub Pages requires a public repo |

## Phases

Effort sizes: **S** is about one working session, **M** is two to four sessions, **L** is a week or more of sessions.

### Phase 0: Repository setup (S), done 2026-09-27

- [x] `git init`, `.gitignore`, `LICENSE` (MIT), `LICENSE-CONTENT.md` (CC BY-NC 4.0) and a README.
- [x] Private GitHub repo created and the initial commit pushed.
- [ ] Add a README screenshot. Do this in Phase 4, when the Open Graph image is made.
- [ ] Protect `main`. Branch protection on private repos needs a paid plan, so do this when the repo goes public.
- Keep `prototype/index.html` as the frozen reference. Do not edit it after Phase 1 starts.

### Phase 1: Production scaffold and Volume I port (L)

1. Scaffold Vite + React + TypeScript. Add ESLint + Prettier, bundle fonts with `@fontsource`, and remove every CDN dependency.
2. Build the shell components:
   - app frame, top bar, dock (progress, Back / Explore / Next, Skip to overview)
   - motion toggle and a `useReducedMotion` hook
   - drawer dialog with a focus trap
   - `Cite`, `Tag`, `Particles`
3. Move the content into typed data files:
   - `src/volumes/whale-sharks/sources.ts`: each source has id, authors, year, title, venue, DOI/URL, verification method and checked date
   - `notes.ts`: Ask the Field Journal entries with keywords and source ids
   - scene copy
   - hotspots, the map sites and the matching-exercise data
4. Port the art: the `WhaleShark` layered SVG, the journal cover, the surface and underwater backgrounds, the field map, the filter-feeding canvas, the swatches and the future-volume emblems.
5. Port the six chapters and confirm they match the prototype side by side at 1280×800 and 375×812.
6. Add hash deep links to chapters (`#giant`, `#anatomy`, …) so people can share a specific chapter.

**Exit:** `npm run build` produces a static site that matches the prototype, with no runtime third-party requests.

### Phase 2: Content integrity and research follow-ups (M)

1. Write a content validator (a Vitest test) that fails the build when:
   - a `Cite` or note references a missing source id
   - a source has no verification method
   - a note has no sources
   - a DOI is badly formed
2. Add an optional script that checks every DOI against Crossref and reports mismatches. Run it manually, since it hits the network.
3. Resolve the open research items in the verification log below. Update the copy only where the evidence says so.
4. Ask the Field Journal:
   - add synonym handling and a simple BM25 ranking
   - show the retrieved note and why it matched
   - add unit tests for tricky phrasings such as "how long do they live" vs "how long are they", and for honest no-answer cases

**Exit:** the validator runs in CI and every "must resolve" item is closed.

### Phase 3: Accessibility, quality and performance (M)

- Playwright smoke tests: walk all six chapters by keyboard, open and close each drawer, complete the matching exercise, and check that there are no console errors.
- `@axe-core/playwright` checks on every chapter in both paper and ocean themes.
- Manual passes:
  - NVDA on Windows; VoiceOver on iPhone if available
  - Safari/WebKit rendering (Playwright WebKit covers the basics)
  - 200% zoom
  - reduced-motion mode
- Performance budget: the page settles on a mid-range phone without jank. Profile the SVG filter layers and canvases. Pause canvases when a chapter is hidden.
- Timing check: have two or three people go through it and time them. The target is about five minutes.

**Exit:** all eight review gates in CLAUDE.md are signed off, and the results are noted in the PR.

### Phase 4: Launch on GitHub Pages (S)

- A GitHub Actions workflow (build, test, deploy to Pages) runs on every push to `main`.
- Point the "Visit the future GitHub project" button at the real repo and relabel it "View the project on GitHub".
- Add social preview metadata (Open Graph title, description and image). The preview image is a rendered still of the opening scene.
- Make the repository public (D6), then turn on Pages and branch protection.
- Update the README with the live URL, how to run locally, how sources are verified, and how to propose a volume. No credit line (D4).

**Exit:** the public URL works, and the README and repo description are complete.

### Phase 5: Volume template and authoring guide (M)

- Extract a `VolumeDefinition` type: meta, six chapter slots, sources, notes, art components and the future-volume teaser.
- Write `docs/authoring-a-volume.md` covering:
  - the research workflow (Researcher, then an independent Scientist check)
  - the verification log format
  - the illustration style sheet and benchmark
  - accessibility checklist
- Build a collection landing page listing volumes as journal covers. Only finished volumes are clickable; the rest stay "Future expedition".

**Exit:** adding a stub Volume II folder shows it on the shelf without any shell edits.

### Phase 6: Volume II research kickoff, Manta Rays (L, separate milestone)

- Research brief: 12–20 peer-reviewed or agency sources, each independently checked.
- Draft six chapters. The field-science chapter is a candidate for belly-pattern photo-ID, if the sources support it.
- Illustration benchmark first: one manta ray plate approved before any other art is made.
- The volume goes through all eight review gates before its cover flips from "Future expedition" to open.

## Research verification log: Volume I

The verification method is recorded per source. **Must resolve** items block v1.0; **Nice to have** items do not.

| Source | Record verified via | Claims that still need primary-text confirmation | Priority |
|---|---|---|---|
| Pierce & Norman 2016, IUCN | DOI resolves; IUCN press release | Regional declines (63% Indo-Pacific, >30% Atlantic) are not used on the page | Nice to have |
| Pierce et al. 2025, IUCN | Crossref | **Current Red List category.** The page names the 2025 update but not its category. Confirm, then state it. | Must resolve |
| Rowat & Brooks 2012 | Crossref + abstract | Ridges along the back and flanks (fins hotspot) | Must resolve |
| Motta et al. 2010 | Crossref + abstract | Terminal mouth wording (mouth hotspot); gill-slit exit is standard anatomy but should still be cited | Must resolve |
| Arzoumanian et al. 2005 | Crossref; method details via secondary sources | Reference area behind the gills, above the pectoral fin; "each pattern is unique" | Must resolve |
| McClain et al. 2015 | Crossref + Table 1 | None (18.8 m confirmed) | Done |
| Guzman et al. 2018 | Full text | None (841 days, ~20,142 km, ended near the Mariana Trench) | Done |
| Womersley et al. 2022 | Abstract | None (92% horizontal overlap; strikes largely undetected) | Done |
| Meekan et al. 2022 | Abstract + institutional release | Ningaloo as the study site confirmed via the release only | Nice to have |
| Ong et al. 2020 | Crossref + abstract | None (annual bands, ages up to ~50 years) | Done |
| Joung et al. 1996 | Crossref; details via secondary sources | ~300 embryos, some in egg cases | Nice to have |
| Tomita et al. 2020 | Abstract | None | Done |
| de la Parra Venegas et al. 2011 | Abstract | None (up to 420 sharks; little tunny eggs) | Done |
| Acuña-Marrero et al. 2014 | Abstract | None (91.5% adult females, almost all possibly pregnant) | Done |
| Norman & Stevens 2007; McCoy et al. 2018; Perry et al. 2018; Rowat et al. 2011; Rohner et al. 2013 | Crossref / publisher page | Used only to place map sites | Done |
| Sharkbook | Site + photo guide (snippet) | The photo guide's left-side instruction, read directly from the page | Nice to have |
| DBCA Western Australia | Agency page | None (3 m from head and body, 4 m from tail, no touching, no flash) | Done |
| CMS | Agency page | None (Appendix I since 2017) | Done |

Also add to the log: the date of each check and who checked it. The prototype was checked on 2026-09-27.

## Risks and mitigations

| Risk | Mitigation |
|---|---|
| Paywalled papers block primary-text checks | Use open-access versions, author preprints or library access. Otherwise soften or remove the claim. |
| IUCN status changes after launch | Source records carry a checked date. Recheck the Red List before each release. |
| SVG filters and canvases stutter on low-end phones | Layered SVG with static filters, pausing hidden canvases, and a Phase 3 performance budget. |
| Scope creep from future volumes | Volume II is its own milestone. v1.0 ships with teasers only. |
| Art quality drifts between volumes | A benchmark plate is approved first for each volume, plus the illustrator and art-director gates. |
| Ask the Field Journal looks like a chatbot and overpromises | Keep the "not a live AI model" label, show the retrieved note, and keep the honest no-answer path under test. |

## Later ideas (not in v1.0)

- An optional narrated audio track, off by default with a play button. Never autoplay.
- Print-friendly "field plate" pages exported as static images.
- A small on-device embedding model for Ask the Field Journal, still restricted to cited notes and with no API key.
- Translations.
