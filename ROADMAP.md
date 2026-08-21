# GFL — Greek course roadmap

`code: gfl` · signature accent `#C74A23` (light) / `#E7977E` (dark) · status: **coming soon → building**

This roadmap turns the empty `gfl` scaffold (LICENSE + README + brand icons) into a
complete boulingua course: Modern Greek content **and** the published Hugo site,
conforming to the `kit` platform, the `curriculum` framework, and the binding
VG Wort standard. It is written to be executed top-to-bottom.

---

## 1. Overview

**What GFL will be.** A free, openly-licensed (CC BY-SA 4.0 content / MIT code) Modern
Greek course for two audiences the sister sites already serve: learners in the German
*Gesamtschule* system and independent adult self-learners. It follows the boulingua
five-step unit model (**Activate → Input → Practise → Apply → Reflect**), ships editable
`.odp`/PDF materials and native-voice audio, and maps every unit to CEFR Companion-Volume
descriptor IDs.

**Realistic CEFR scope.** `core` conformance (A1–B1) as the shipping target, preceded by a
**Level 0 script-onboarding stage** unique to non-Latin languages. B2 is a stretch goal
after `core` is live; C1/C2 are explicitly declared out of reach for freely-available OER
(consistent with the framework's "honest note on instantiation").

**The 3–5 defining challenges (from the language context):**

1. **Non-Latin script.** The Greek alphabet is the single biggest beginner barrier. Reading
   readiness cannot be assumed on page one — it must be *taught* as Level 0 before any A1
   communicative content lands.
2. **Orthography = monotonic.** Modern Greek uses the **monotonic** system (one tonos accent
   + diaeresis), not polytonic. This is a hard editorial rule affecting every glyph typed,
   every TTS input, and font selection.
3. **Web font with Greek coverage.** hugo-coder's default stack and the LaTeX material
   templates must render Greek (including tonos + diaeresis combinations) correctly; not all
   bundled fonts have complete Greek coverage.
4. **TTS is single-speaker, but not low-tier.** Upstream ships three Greek voices, and
   `el_GR-rapunzelina-medium` — the same speaker as `-low`, equally CC0, at 22.05 kHz
   against `-low`'s 16 kHz — is the one to use; the tier this document once accepted was
   a free upgrade left on the table. The third voice, `el_GR-joy-medium`, is CC BY-NC 4.0
   and cannot go inside a CC BY-SA 4.0 course, so Greek still has exactly **one** usable
   speaker and dialogue turns are still marked typographically, not acoustically.
5. **Phonics vs. spelling.** Greek spelling→sound is fairly regular, but sound→spelling is
   not (ι/η/υ/ει/οι all = /i/). Level 0 and the LING scales must handle this explicitly.

---

## 2. Language & localisation decisions

Each decision below is a recommendation, not an open question.

- **Script handling → teach it, don't gate it.** Add a **Level 0 "Το αλφάβητο" onboarding
  stage** (`content/level-0/`) covering the 24 letters, digraphs (ει, ου, γγ/γκ, μπ, ντ,
  τσ/τζ), stress/tonos, and reading fluency. All Greek from Level 0 onward is shown in Greek
  script; **transliteration (ELOT 743-style) is a scaffold used only in Level 0 and glossary
  headwords**, then dropped. Rationale: learners must read Greek, not Latinised Greek.
- **Variant → Standard Modern Greek (Dimotiki), monotonic.** No Katharevousa, no polytonic,
  no Cypriot dialect in the core (a Cyprus/culture note can live in an appendix). `hugo.toml`
  `languageCode = "el"`, `defaultContentLanguage = "el"`.
- **RTL → not applicable.** Greek is LTR; no bidi layout work. (Flagged only to close it.)
- **Web fonts → pin a Greek-complete stack.** Verify hugo-coder's font renders tonos +
  diaeresis; if not, take the Greek subset from the kit. Fonts are **not**
  self-hosted per course: F3 builds per-script subsets from full upstream faces and
  ships them in the module, so this course declares its script tier and receives the
  right subset. A course repo has no `static/fonts/` and no `assets/css/`, and adding
  either re-opens the drift the module closed. Same check for the kit's LaTeX templates (`fontspec` +
  a Greek-covering OpenType font; confirm `\setmainfont` handles combining tonos).
  §1's caution at challenge 3 is measured, not hedged: the bundled
  `kit/fonts/SourceSans3-Regular.ttf` carries 781 codepoints with **none** in
  U+0370–U+03FF, so it renders tofu for every Greek word. **F3** is where this is
  fixed org-wide — full upstream Source Sans 3, self-hosted, with the Greek subset
  built by `build_fonts.py` rather than hand-picked here — so do the verification, and
  take the subset from F3 when it lands instead of pinning a one-off face.
- **TTS → decided: `el_GR-rapunzelina-medium` (CC0), medium tier, single speaker.** Use
  it via the kit's Piper workflow (`kit/audio/`) for vocab, dialogues and texts. The
  medium-tier voice this document was waiting for already exists and is the same speaker
  at 22.05 kHz instead of 16 kHz, under the same CC0 terms — so the audio quality target
  rises with no licence cost and no re-audition of a new speaker. What does **not**
  change: `el_GR-joy-medium`, the only other Greek speaker upstream, is **CC BY-NC 4.0**,
  and a NonCommercial model cannot be used to synthesise audio that ships inside a
  CC BY-SA 4.0 work at all. Greek therefore stays **single-voice**, dialogues cannot
  alternate speakers acoustically, and speaker turns are marked typographically. The
  fallback ladder if `-medium` disappoints is `el_GR-rapunzelina-low` (same speaker,
  also CC0), then transcript-only — never a neighbouring language's voice. Verify
  pronunciation of tonos-stressed words and final-ς before committing audio.
- **Voice IDs are never hand-typed.** The ID, tier and licence above are read from
  `kit/audio/voices.yml`, which is the single source of truth and must never be
  hand-copied into prose and keeps `-low` and the rejected `joy` row with their licences so the
  decision stays auditable. A voice ID retyped from memory is how a 404 gets written into
  a download script.
- **Level 0 stage** — see above; it is a first-class content section, its long-form
  explanatory pages carry VG Wort marks like any unit.

---

## 3. Instantiation from the kit

Stand up the buildable site first, before authoring content. Nothing shared is
copied into `gfl/` — it is imported.

1. **Create the repo around the kit** (preserve the existing `LICENSE`,
   `README.md`, `brand/`). `gfl` holds a short `hugo.toml` — baseURL, title,
   languageCode, `[params]`, menus, and the module import

   ```toml
   [module]
     [[module.imports]]
       path = "github.com/boulingua/kit"
   ```

   — plus `go.mod`/`go.sum` requiring `github.com/boulingua/kit` at the pinned
   tag (`v1.0.0`), `boulingua.yml` (the per-course config the gate battery
   reads), the twelve-line `.github/workflows/deploy.yml` calling the org's
   reusable workflow, an empty `content/` skeleton, the legal pages,
   `lychee.toml`, `.gitignore`, `.gitattributes`, `.nojekyll`. `layouts/`,
   `assets/`, `i18n/`, `archetypes/` and `scripts/` are **not** copied and must
   never appear in this repo: they are the drift surface, and a file that is not
   here cannot fork. Hugo resolves the first four from the module; CI checks the
   kit out at the pinned tag so `scripts/` and the gate battery sit on disk
   before Hugo runs.
2. **Vendor `_materials/`.** Run `kit materials sync`: it assembles the kit's
   `latex/`, `fonts/` and `brand/icons/` into the flat `_materials/` tree the
   LaTeX build expects, and the result is committed. This is the only vendored
   surface — XeLaTeX cannot read a Hugo module — and it is hash-gated against
   `kit.lock`, so an edited file fails the build instead of forking quietly.
   (Earlier drafts said to copy `_materials/` from the template; `pagegen` never
   had one.)
3. **Edit the marked `hugo.toml` values (only these):**
   - `baseURL = "https://boulingua.github.io/gfl/"`
   - `title = "Ελληνικά — Greek course — S. Le Boulanger"`
   - `languageCode = "el"`, `defaultContentLanguage = "el"`
   - `[params].navTitle = "Ελληνικά"`, `description`, `keywords`
   - `[params].code = "gfl"` ← selects the accent from the kit's
     `data/accents.yaml`. Set it. The kit's own default is the neutral graphite
     `template` accent, so an unconfigured course looks obviously wrong rather
     than looking like DaF.
   - `[params.plausible].domain = "boulingua.github.io/gfl"`
   - `[[params.social]].url = "https://github.com/boulingua/gfl"`
   - `[[menu.main]]` sections → mirror `content/` (Level 0, A1, A2, B1, Materials, About,
     Legal). Keep `[params.plausible]` **last** under `[params]` (TOML sub-table trap).
4. **Accent + icon.** The kit's `data/accents.yaml` already carries `gfl` (`#C74A23` /
   hover `#A03B1C` / dark `#E7977E`) and arrives with the module — confirm, do **not**
   edit CSS and do not keep a copy here. Run the kit's `brand/make_icon.py` to regenerate
   this repo's pentagon mark + favicons from the accent.
5. **Legal pages.** Fill the ⟨…⟩ placeholders in `impressum.md` / `datenschutz.md` /
   `haftungsausschluss.md`; the Datenschutz page must include the VG Wort METIS disclosure.
6. **First green build.** `hugo --minify --gc --panicOnWarning` clean locally; push;
   confirm `.github/workflows/deploy.yml` runs the gate battery and deploys to GitHub
   Pages. Enable Pages (source: GitHub Actions). Verify the accent renders and the
   pentagon favicon loads. **This is the MVP-0 gate: an empty-but-correct site is live
   before content.**

---

## 4. Curriculum conformance target

- **Declared level: `core` (A1–B1).** Publish `full` (adds B2/C1) only if B2 later ships;
  never claim a level not met.
- **Scope manifest.** Publish a machine-readable `conformance.yml` (mirroring
  `curriculum/examples/de-a1/`) at the repo root or `data/`, declaring
  `framework: boulingua-curriculum`, `framework_version`, `language: el`,
  `declared_conformance: core`, and a `realizations:` list mapping each unit's can-dos to
  descriptor IDs `{LEVEL}.{DOMAIN}.{SCALE}.{SEQ}`.
- **In-scope scales to implement.** At A1–B1, cover every in-scope scale that has an official
  descriptor at that level — prioritise the activity domains learners exercise: **REC**
  (oral/reading comprehension), **PROD** (oral/written production), **INT** (conversation,
  information-exchange, transactions, correspondence, notes/messages), and the **LING**
  scales — including **`LING.orthographic-control`** and **`LING.phonological-control`**,
  which Level 0 directly serves — plus **SOC** and the **PRAG** scales.
- **Scales to declare, not invent.** For cells the CV leaves empty at A1–B1 (much of
  **MED** mediation, **PLUR** plurilingual, several strategy scales, all C-level cells),
  record **`no-official-descriptor`** — a silently missing scale is a conformance failure; a
  declared gap is not. `SIGN` receives no IDs (out of scope).
- **Gate.** Every `implements_id` must resolve — format, global uniqueness, and
  resolution to a real (scale, level) in `scale-registry.yml`. The reusable workflow
  `boulingua/.github/.github/workflows/course-build.yml@v1` runs
  `python .curriculum/scripts/conformance_audit.py resolve --manifest conformance.yml
  --content content`. `id-audit.sh` audits the framework's *own* level files and
  **cannot** validate this repo. Do not wire it here.

---

## 5. Content creation plan

Recurring cast + theme give continuity across levels: a small ensemble (e.g. **Ελένη,
Νίκος, Μαρία, Γιώργος**) moving through Greek daily life — neighbourhood, agora/laïki,
kafeneío, ferries and islands, festivals. Effort tags: **S** ≤ 2 days, **M** ≤ 1 week,
**L** > 1 week.

**Phase 0 — Το αλφάβητο (Level 0 script onboarding).** ~6 units. **L.**
Letters & sounds, digraphs, stress/tonos, handwriting, reading fluency drills. Acceptance:
a learner can decode any Greek word aloud and type monotonic Greek; each unit has audio and
a worksheet; no Latin transliteration past this stage.

**Phase 1 — A1.** ~12 units + exams. **L.**
Greetings/introductions, alphabet-to-words consolidation, numbers/time, family, café &
food, shopping at the laïki, directions, daily routine, present tense of common verbs,
nouns/gender/articles, basic cases (nom/acc). Acceptance per unit: five-step structure,
answer key, teacher note, committed slide deck + worksheet + audio, can-dos mapped to A1 IDs.

**Phase 2 — A2.** ~12 units + exams. **L.**
Past tenses (aorist), future (θα), genitive case, comparatives, travel/ferries, health,
work, weather, invitations, telephoning, short narratives. Acceptance as A1, mapped to A2 IDs.

**Phase 3 — B1.** ~12–14 units + exams. **L.**
Subjunctive, conditionals, relative clauses, opinion/argument, media & news, culture &
history, formal vs. informal register, longer reading texts. Acceptance as above, B1 IDs;
introduces `SOC`/`PRAG` register work.

**Exams — first-class siblings.** Each level gets exam bundles authored as
`…-exam/index.md` (`page_type: exam`, shared `unit_nr`, `duration_min`, `total_points`,
`notenschluessel`), with the PDF under `static/downloads/<level>/`. **M** each.

**Appendices** (`page_type: appendix`): Greek alphabet & pronunciation reference, monotonic
accentuation rules, noun/verb paradigm tables, thematic glossary (Greek headword +
transliteration + gloss), CEFR self-assessment grid, common-errors sheet. **M** each.

**Editorial:** `about/` course overview, acknowledgements, sources/references (open/
public-domain only, cited). **S–M.**

---

## 6. Website & materials

- **Section landings** via shortcodes only (`hero`, `kicker`, `lead`, `card-grid`, `card`,
  `downloads`, `callout`) — never raw HTML — for Level 0, A1, A2, B1, Materials, About.
- **Materials pipeline.** Generate decks (beamer) and worksheets from the kit's LaTeX
  templates (`kit/latex/`, vendored into `_materials/`) locally; **commit** the `.odp`/PDF outputs under
  `static/materials/` + `static/downloads/`. CI only *verifies* (no TeX Live in deploy path).
  Both LaTeX templates must be confirmed Greek-capable (fontspec + Greek OpenType face) as a
  one-time setup task before the first deck.
- **Native-voice audio.** Run the kit's Piper workflow with `el_GR-rapunzelina-medium` (CC0);
  output OGG/Opus committed under `static/`; idempotent re-synthesis on text change. The
  `.onnx` path comes from the `gfl` row in `kit/audio/voices.yml` and is never typed in
  by hand. Medium tier, so the audio quality target is the
  same as the sister sites'; single-voice caveat per §2 still applies, and it is a licence
  constraint, not a catalogue gap.
- **Thumbnails & downloads.** `scripts/render_thumbs.py` for deck/worksheet thumbnails;
  `verify_downloads.py` gate ensures every referenced download exists and is attributed.

---

## 7. VG Wort — pixel assignment for ALL content pages (required, non-skippable)

Every editorial page of **≥ 1800 rendered characters** — every Level 0 unit, A1/A2/B1 unit,
every exam, every appendix, and the About/overview prose — gets **exactly one** VG Wort
Zählmarke, per `kit/docs/vgwort-standard.md`. Procedure:

1. **Draw fresh public codes** (32-hex `Öffentlicher Identifikationscode`) from the author's
   **T.O.M.** account — one per work, never reused, never invented. Private codes never touch
   the repo.
2. **Register** each in `data/vgwort.yaml` keyed by `url:` (or `path:`) with `public_id`,
   `pixel_url` (`https://vg09.met.vgwort.de/na/<code>`), `min_chars: 1800`, `author`,
   `registered_at`.
3. **Render** via the single resolver partial `layouts/_partials/vgwort/url.html` → `<head>`
   preload + eager `<body>` `<img>` (loading=eager, off-screen, no JS, no consent gate,
   direct `met.vgwort.de`).
4. **Record** each mark in the private usage registry (kept outside the repo).
5. **Gates:** coverage audit (warns on ≥1800-char pages without a mark), render-verify
   (blocking: each `pixel_url` present, one URL site-wide), hub-guard (blocking: no pixel on
   `/materials/`). Never mark navigation, tag/category indexes, `/page/N/`, or the three
   templated legal pages.

**Estimated marks needed:** ~6 (Level 0) + ~36 (A1+A2+B1 units) + ~6–8 (exams) + ~6
(appendices) + ~2 (about/overview) ≈ **56–60 Zählmarken** for the complete `core` course
(draw incrementally, per phase, as pages are authored).

---

## 8. Milestones & sequencing

| Milestone | Contents | Dep. | Effort |
|---|---|---|---|
| **M0 — Site live (empty)** | §3 instantiation, green build, Pages deploy, accent+icon, legal filled | — | M |
| **M1 — Level 0 live** | 6 script-onboarding units, alphabet appendix, audio, fonts verified | M0 | L |
| **M2 — A1 MVP (flip candidate)** | 12 A1 units + exam, materials, audio, conformance.yml (A1), VG Wort A1 | M1 | L |
| **M3 — A2 complete** | 12 A2 units + exam, IDs, materials, marks | M2 | L |
| **M4 — B1 complete → `core`** | 12–14 B1 units + exam, full appendix set, §4 gate green | M3 | L |
| **M5 — polish** | thumbnails, glossary completeness, a11y pass, link-check | M4 | M |

**Dependencies:** font/LaTeX Greek verification blocks all materials; the TTS voice is
decided (§2) and blocks nothing beyond its own pronunciation spot-check; `data/accents.yaml` (done) blocks the icon; T.O.M. code draw blocks each
phase's render-verify gate.

**Definition of done (flip from "coming soon" to active on the world map):** site builds
green with the full gate battery passing; **Level 0 + A1 complete** with materials + audio
committed and thumbnails rendered; `conformance.yml` declares `core` and the §4 gate is
green; every ≥1800-char page carries a registered, render-verified Zählmarke; legal pages
filled and Datenschutz discloses METIS; README updated from "scaffold" to shipped. (A1 is the
minimum public flip; A2/B1 land as rolling updates toward full `core`.)

---

## 9. Open decisions & risks (language-specific)

- **TTS quality (risk, downgraded).** The tier problem is gone: `el_GR-rapunzelina-medium`
  is the same speaker as `-low` at medium quality and the same CC0 terms, so the "low tier"
  this document accepted was never necessary. The **single-speaker** constraint stands, and
  it is a licence constraint rather than a thin catalogue — upstream's second Greek speaker,
  `el_GR-joy-medium`, is CC BY-NC 4.0 and unusable in a CC BY-SA 4.0 course. Residual risk:
  mispronounced tonos-stressed words and no acoustic dialogue contrast. Mitigation: verify a
  sample set before bulk synthesis; typographic speaker turns; revisit only if a Greek voice
  lands with an allow-listed licence.
- **Font coverage.** hugo-coder web font and both LaTeX templates must render tonos +
  diaeresis + final-ς. If any gap, self-host an OFL Greek face. Verify once, early.
- **Transliteration scheme.** Confirm ELOT 743 for glossary headwords; decide the hard cutoff
  where transliteration disappears (recommended: end of Level 0).
- **Stress-mark input discipline.** Every authored word needs correct monotonic tonos; add a
  lint/spot-check so accents aren't dropped in typing or copy-paste.
- **Mediation/plurilingual gap.** Much MED/PLUR has no A1–B1 descriptor — ensure these are
  *declared* `no-official-descriptor`, not silently omitted (would fail conformance).
- **Cultural scope.** Keep Cyprus/dialect content in an appendix note, out of the Standard
  Modern Greek core, to avoid variant drift.
