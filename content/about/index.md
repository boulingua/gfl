---
title: About this course
author: S. Le Boulanger
date: 2026-09-07
description: "What the Greek course is, who it is for, the language decisions behind it, and how CEFR conformance, materials, audio and sources are handled."
---

`gfl` is the Modern Greek course of the [boulingua](https://github.com/boulingua)
platform. It is written and maintained by S. Le Boulanger, published free of
charge, and licensed for reuse: content under **CC BY-SA 4.0**, the site code
under **MIT**. There is no login, no advertising, no tracking of individuals,
and no paid tier.

## Who it is for

Two audiences, served by the same pages.

**Learners in the German *Gesamtschule* system**, whose teachers need material
that can be printed, projected and adapted in a single lesson: every unit ships
an editable slide deck, a printable worksheet, an answer key and a teacher note
with timings, social forms and differentiation.

**Independent adult self-learners**, who need the same content to work without a
teacher: objectives stated up front, answer keys on the page, audio for every
dialogue and vocabulary list, and a self-assessment step at the end of each unit.

## How a unit is built

Every unit — in Level 0 and at A1, A2 and B1 — follows the same five-step model:

1. **Activate** — a short scene with the recurring cast that raises the target
   language before it is taught.
2. **Input** — the vocabulary, the model text or dialogue, and the grammar,
   presented once and clearly.
3. **Practise** — guided, closed tasks that rehearse the form.
4. **Apply** — one open output task, the part that differentiation scaffolds up
   and down.
5. **Reflect** — self-assessment against the unit's can-do statements.

The shape is deliberately monotonous. A learner who has done three units knows
where to find the answer key in the fourth, and a teacher who has taught one
unit can plan the next in ten minutes.

## The language decisions

Greek forces a small number of editorial decisions that a course in a
Latin-script language never has to make. They are settled here, once, and hold
for every page.

**Script is taught, not assumed.** The Greek alphabet is the largest barrier a
beginner meets. It gets its own stage — [Level 0](/level-0/), six units on
letters, digraphs, the tonos and reading fluency — placed before any
communicative content. A1 then assumes it completely.

**Transliteration is a scaffold with an end date.** Latin transliteration
(ELOT 743) appears inside Level 0 and in glossary headwords. It stops at the end
of Level 0 Unit 6 and does not return. Learners who are allowed to keep reading
Latinised Greek stop learning to read Greek.

**The variety is Standard Modern Greek (Dimotiki).** Not Katharevousa, not
Cypriot Greek, not the northern dialects. Those are treated in a cultural note
among the [appendices](/appendices/); keeping them out of the core prevents the
variant drift that shows up in beginners' output.

**The orthography is monotonic.** One accent — the tonos — plus the diaeresis.
The polytonic system appears only where a quoted historical text requires it.
This is a hard rule that reaches further than it looks: it governs every glyph
typed into a unit, every string handed to the speech synthesiser, and the choice
of typeface for the site and for the printed materials.

**The typography carries Greek properly.** The site is set in Source Sans 3 and
JetBrains Mono, both extended here with their Greek subsets so that the tonos,
the diaeresis, the combined ΐ and ΰ, and final sigma render correctly rather
than falling back to whatever the reader's system happens to have. The LaTeX
templates behind the slide decks and worksheets are held to the same standard.

## CEFR conformance

The course targets **`core` conformance — A1 to B1** — against the shared
[boulingua curriculum framework](https://github.com/boulingua/curriculum), plus
the pre-A1 Level 0 stage. Conformance is declared in a machine-readable
`conformance.yml` at the repository root: the framework version, the language,
the declared level, and for every unit can-do the descriptor ID it realises, in
the form `{LEVEL}.{DOMAIN}.{SCALE}.{SEQ}`.

Two commitments follow from that file, and they matter more than the claim
itself:

- **Every ID must resolve.** The framework's audit script checks format, global
  uniqueness, and that each ID names a real scale at a real level. An ID that
  does not resolve is a build failure, not a footnote.
- **Gaps are declared, not hidden.** The Companion Volume leaves many cells
  empty at A1–B1 — much of mediation, the plurilingual scales, several strategy
  scales. Those are recorded explicitly as `no-official-descriptor`. A silently
  missing scale would be a conformance failure; a declared gap is honest.

**B2 is a stretch goal**, published only if and when it actually ships. **C1 and
C2 are out of scope**: sustaining them at the quality this course claims is not
realistic for a freely maintained open resource, and claiming a level one has
not met is worse than not offering it.

## Materials and audio

Slide decks (`.odp` and PDF) and worksheets (PDF) are generated locally from the
shared branded LaTeX templates and **committed** to the repository. Continuous
integration builds and verifies the site; it does not build materials, so a
missing TeX installation can never break a deploy, and what you download is
exactly what was reviewed.

Audio is synthesised with the openly licensed Greek Piper voice
`el_GR-rapunzelina-low` and committed as OGG/Opus. It is worth stating the
limitation plainly: this is currently the only open Greek voice available to the
project, it is a single speaker, and it is a low-tier model. Dialogues therefore
distinguish their speakers typographically rather than acoustically, and
tonos-stressed words are checked against the synthesised output before a unit's
audio is committed. If a better open Greek voice appears, the audio is
re-synthesised — the pipeline is idempotent by design.

## Sources

Cultural, historical and factual content is drawn from **public-domain or
openly-licensed sources only**, cited on the page with author, access date and
licence. No copyrighted textbook material, no scanned pages, no exam papers from
a commercial provider. All dialogues, stimulus texts and exercises are original
work by the author.

Exams in this course are the author's own didactic constructions. They follow
the publicly documented shape of CEFR-aligned examinations, but they stand in no
official relationship to any examination board, and passing one certifies
nothing beyond the learner's own progress.

## Corrections

Errors in Greek — a dropped tonos, a wrong gender, a mistranslated gloss — are
the kind of mistake that propagates through a course if it is not caught. The
fastest way to have one fixed is an issue in the
[repository](https://github.com/boulingua/gfl/issues); the
[Impressum](/impressum/) gives an e-mail address for anything that does not
belong in public.
