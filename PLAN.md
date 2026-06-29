# PLAN — decodable-readers

> Status: Draft · Version: 0.1.0 · Last updated: 2026-06-28 · Owner: J. Carter (acting maintainer) · Lane: donated

## Executive summary

`decodable-readers` is an Elyos good-deed project that authors **open, phonics-aligned decodable
storybooks** for early reading — short, illustrated texts written so a beginning reader can
**sound out almost every word** using only the letter–sound correspondences (GPCs) they have
already been taught, plus a small, pre-taught set of high-frequency "tricky" words. The books are
designed to **slot directly into the `literacy-from-zero` flagship** as the "now read a whole
story" practice tier that sits on top of letter-sound instruction, and to be usable on their own by
teachers, tutors, parents, and adult emergent readers in low-resource settings.

The defining constraint of this project — its identity — is **provable decodability**. A book that
*claims* to be decodable at a given level but actually contains words the reader cannot yet sound
out is not a neutral mistake: it silently teaches the child to **guess from pictures and context**
instead of decoding, which is the exact habit structured-literacy instruction exists to prevent. So
decodability here is treated the way `vital-info-translations` treats licensing: **encoded as
checkable data and enforced by an automated gate**. Every book is checked against a transparent,
published **scope-and-sequence** by a **decodability checker**; a book that does not provably meet
its claimed level **does not ship**. This is the project's headline gate.

The work runs in the **donated lane**: a human runs their own coding/writing agent interactively to
draft a story, illustrations brief, and metadata, then opens a PR; the Elyos CLI only prepares the
workspace and opens PRs. The project is **low risk tier** overall (original children's fiction over
controlled vocabulary), but it carries three care obligations that are non-negotiable and are gated
in CI/review: (1) **decodability correctness** (above); (2) **child-content safeguarding +
inclusive representation** review of every book; and (3) for any **non-English** language, a
**native-speaker literacy/linguistics reviewer**, because phonics is language-specific and a book's
decodability claim cannot be trusted across an orthography the reviewer doesn't read. The third
obligation makes non-English work *effectively medium-risk in practice* even though the project tier
is low.

Honest status note: this is a **candidate** project (per `planning/ROADMAP.md`); **no partner
literacy organization or named requesting program is yet secured.** Until one is, `verifiedNeed` is
recorded as `false` on tasks whose value depends on a named beneficiary, and the project's
Definition of Shipped (a book *actually used with real learners*) cannot be fully met. M0/M1 are
deliberately built to produce real, reusable value (the checker, the open scope-and-sequence, the
first verified books) **without** a partner; securing a first partner and confirming integration
with `literacy-from-zero` is the top open dependency (see Open questions and Roadmap M2).

## Problem & beneficiaries

**Who is helped.** The ultimate beneficiaries are **people learning to read from the very
beginning** — primarily young children, and equally the **unschooled or low-literacy adults** that
`literacy-from-zero` targets — who need to *practice decoding on connected text* but for whom no
suitable book exists. Intermediate beneficiaries are the **teachers, tutors, volunteers, and
parents** who teach them, often in **low-resource or under-served-language settings** where
commercial decodable readers are expensive, copyrighted, English-only, or simply unavailable.

**The need (category-level, well-evidenced).** Systematic phonics / structured-literacy
instruction teaches letter-sound correspondences in a deliberate order, and learners need texts
they can **actually decode at each step** to build accuracy and confidence before they meet
unrestricted text. Two gaps make this hard:
- **Most "early readers" are not decodable.** Predictable / leveled texts are written to be *guessed*
  from pictures and repeated sentence frames; using them during the phonics phase rewards guessing
  over decoding.
- **Good decodables are scarce, costly, and overwhelmingly English.** Open, freely-redistributable
  decodable sets aligned to a transparent scope-and-sequence are rare; for most of the world's
  languages they barely exist. Naively *translating* English decodables does **not** work, because
  decodability is a property of a specific language's spelling system and teaching order (see
  Non-goals).

**Verified need: TO BE SECURED.** The *category* need is strong and well-documented in the reading-
science literature and in the existence of `literacy-from-zero` as a selected flagship. But a
**specific requesting program, target learner population, language set, and the exact
scope-and-sequence it teaches to are not yet secured.** We record this honestly: foundation tasks
(checker, sequence data, first verified books) proceed without a partner; **adoption/delivery tasks
are blocked** and marked `verifiedNeed: false` until a partner and target are confirmed.

**Partner org: TO BE SECURED.** Candidate partner / distribution-channel types (none committed as
of this draft): `literacy-from-zero` itself (the most natural internal "requestor"); open-content
literacy repositories such as **StoryWeaver (Pratham Books)**, the **Global Digital Library /
Norad**, the **African Storybook initiative (Saide)**, and **Worldreader / Library For All**;
structured-literacy / "science of reading" educator communities and Decoding-Dyslexia groups; and
university education / speech-language departments for reviewer support and small effectiveness
studies.

## Goals and non-goals

**Goals**
- Publish **original, openly-licensed, illustrated decodable storybooks** that **provably** match a
  **transparent, published scope-and-sequence**, level by level, cumulatively.
- Make decodability **machine-checkable**: ship a **decodability checker** + a curated, segmented
  **lexicon** + scope-and-sequence-as-data, so every book's level claim is verified, not asserted.
- Be **genuinely multilingual the right way**: each language gets its **own native
  scope-and-sequence and natively-authored books** (not translations), starting with languages
  where we have reviewer coverage.
- Make every book **accessible**: clean print/EPUB/HTML outputs, a **dyslexia-friendly** font
  option, large-print and low-ink variants, and structured text suitable for screen readers and for
  `read-aloud-audio` alignment later.
- Guarantee **safeguarding + inclusive-representation review** and (for non-English)
  **native-speaker linguistic review** of every book before it ships.
- Deliver book sets that a literacy program / partner **actually uses with real learners**, and
  that **slot into `literacy-from-zero`**.

**Non-goals**
- **Not** a phonics *curriculum* or a method war. We publish *texts aligned to* a scope-and-sequence;
  we do not prescribe how to teach, and we support **more than one** published sequence rather than
  declaring one "correct."
- **Not** predictable/leveled "guessing" readers, look-and-say sight-word primers, or generic
  picture books. (Those overlap with the separate `open-childrens-books` project.)
- **Not machine-translated decodables.** We never produce a non-English book by translating an
  English one and re-tagging it; decodability does not survive translation. Each language is authored
  natively against its own sequence. (This is the single most important non-goal.)
- **Not** an assessment, screening, or diagnostic tool, and **not** a dyslexia/clinical instrument.
  We provide practice texts; we do not test, label, or diagnose learners.
- **Not** a reading app, LMS, hosting platform, or account system. We produce open files; partners
  and existing open repos handle distribution.
- **Not** audio/narration (that is `read-aloud-audio`), handwriting practice (`handwriting-practice`),
  or original non-decodable picture books (`open-childrens-books`) — we coordinate with, but do not
  duplicate, those projects.
- **Not** content that is unsafe, frightening, or inappropriate for young children, and not content
  that stereotypes or excludes any group (enforced by the safeguarding/representation gate).

## Success metrics (outcomes)

Outcome-based and beneficiary-centric. Baselines are ~0 (new project). **Outcome** targets are for
the first ~6 months **after a partner/program is secured**; **interim foundation metrics** are
tracked from M0/M1 so progress is visible *before* a partner exists. We explicitly **do not** count
"PRs merged," "books drafted," or "words written" as success.

**Outcome metrics (post-partner)**

| Metric | Baseline | Target | How measured |
|---|---|---|---|
| Decodable books **adopted/used with real learners** by a program/partner | 0 | ≥ 1 full set (≥ 6 books) in ≥ 1 language | Partner written confirmation in PR/receipt |
| Languages covered **end-to-end** (sequence → verified books → used) | 0 | ≥ 2 | Project registry |
| **Integration with `literacy-from-zero`** (books referenced as its decodable practice tier) | none | linked + used in ≥ 1 lesson flow | Cross-project link + maintainer confirmation |
| Partner-reported **usefulness** of a delivered set (qualitative) | n/a | Positive from ≥ 1 program | Partner feedback log |
| **Learner decoding signal** (aspirational, partner-run, observational — *not* an Elyos claim) | n/a | Partner reports learners can decode the set with ≥ 90% word accuracy | Partner's own informal running-record / report |

**Interim foundation metrics (M0/M1, partner-independent)**

| Metric | Baseline | Target | How measured |
|---|---|---|---|
| Books passing the **decodability gate** (100% coverage vs claimed level) | n/a | **100%** of published books (hard gate; **automated from M1**, manual+checker in M0) | Decodability checker report per book |
| Books passing **safeguarding + representation** review | n/a | **100%** (hard gate) | Review sign-off in PR |
| Non-English books passing **native-speaker linguistic** review | n/a | **100%** (hard gate) | Reviewer sign-off in PR |
| **License/provenance completeness** of published artifacts (text + illustrations) | n/a | **100%** (hard gate) | License-check task / CI |
| Verified books published end-to-end (except adoption) | 0 | ≥ 1 by end of M0; ≥ 6 (a coherent set) by end of M1 | Repo + checker reports |
| Tricky-word load within policy (see below) | n/a | 100% of books within the published density cap | Checker report |
| Reviewers qualified (pedagogy / safeguarding / per-language) | 0 | ≥ 2 by end of M1 | Reviewer roster |

**Defining "decodability" precisely (so the gate is enforceable).** For a book tagged to
scope-and-sequence *S* at **level/step *k***, every running word must be **either** (a)
**decodable** — i.e., spellable using only the GPCs introduced in *S* at or before step *k*, per the
word's segmentation in the lexicon — **or** (b) a **pre-taught tricky/heart word** on *S*'s
cumulative tricky-word list at step *k*. We track two numbers:
- **Decodability coverage** = (decodable words + listed tricky words) ÷ total running words.
  **Hard gate: coverage = 100%.** *Zero* off-sequence words. (A single un-decodable, non-listed word
  fails the book.)
- **Tricky-word density** = listed tricky words ÷ total running words. **Quality cap (policy):** at
  the earliest levels, **≤ 1 *new* tricky word per book** and **≤ ~10–15% tricky words overall**
  (exact caps set per-level in the content policy). Density above the cap fails review even though
  coverage is 100%, because an over-loaded "decodable" is functionally a sight-word text.

**Sample rule for any rate reported as a percentage.** Percentages are reported only once the
denominator is **≥ 10 published books**; below that we report raw counts (e.g., "7/8 books passed")
to avoid small-sample noise.

## Scope

**Definition — "scope-and-sequence" (the contract a book is checked against).** An ordered list of
**steps**; each step declares the **GPCs introduced** at that step and the **tricky words
pre-taught** at that step. A book names exactly one sequence + step and is decodable *cumulatively*
(everything up to and including that step). We **publish the sequence as data** and support **more
than one** (English alone has several widely-used orders); we do not invent a novel sequence unless a
partner needs one.

**Definition — "low-resource language" (for prioritization).** A target language qualifies if
**openly-licensed decodable readers aligned to a published scope-and-sequence are scarce or absent**
for it, and it has a meaningful learner population. **Reviewer availability is a hard precondition,
not a qualifier:** we only schedule a language for which a qualified native-speaker
literacy/linguistics reviewer can be sourced.

**Prioritization rule (among candidate languages/levels).** (1) a **secured partner's** stated
target language and the sequence it teaches; (2) **`literacy-from-zero`'s** launch language(s);
(3) largest learner population with the **weakest existing open decodable material**; (4) **reviewer
coverage already available**; (5) **transparent (shallow) orthographies first** as the cheapest path
to a correct second language (e.g., a language where letter→sound is highly regular), since they let
us prove the multilingual model with lower segmentation risk. Ties break toward reusing existing
lexicon/sequence work.

**In scope**
- **Scope-and-sequence data** (`sequences/<lang>/<id>.yaml`) for each supported language, sourced
  from or aligned to a transparent, openly-documented order; license/provenance recorded.
- A **curated, segmented lexicon** (`lexicon/<lang>.yaml`) giving each word's grapheme→phoneme
  segmentation so decodability is computed deterministically.
- **Original decodable storybooks**: story text, level metadata, illustration brief + open-licensed
  illustrations, computed decodability report, reviews, provenance, attribution.
- The **decodability checker** + content **JSON Schemas** + **CI gate**.
- **Accessible exports**: HTML/EPUB/print-PDF, dyslexia-friendly font option, large-print/low-ink
  variants; structured text for screen readers and future audio alignment.
- **Safeguarding + inclusive-representation policy**, **content-rating** per book, **review rubric**,
  and per-book **license/provenance verification**.

**Out of scope**
- Translating existing decodables across languages (a non-goal, see above).
- Teaching method, lesson plans, assessment/screening, or diagnosis.
- Audio narration, handwriting sheets, non-decodable picture books (sibling projects).
- A website, app, account system, or hosting platform.
- Books in a language with **no qualified reviewer** available (we do not ship unreviewed reading
  material for children).
- Adapting third-party stories whose **license or public-domain status is unverified or
  incompatible** (see Data, licensing & compliance).

## Solution approach & architecture

This is primarily a **content/data pipeline** project (deliverables are books + data artifacts) with
**one focused software component** (the decodability checker + schemas). It rides existing Elyos
donated-lane mechanics (CLI prepares workspace, human runs agent, PR opened, human/expert review
gates "done"). Per CLAUDE.md, the checker is **project tooling living in this repo** and is
**agent-neutral** — no vendor/agent-specific logic; nothing goes into `packages/core`.

**Pipeline (per book)**
1. **Pick sequence + level** — choose the target scope-and-sequence and step *k*; this fixes the
   allowed GPCs and tricky words.
2. **Author story** — agent drafts a short, engaging, age-appropriate story constrained to the
   allowed GPCs + listed tricky words; flags any word it had to reach for.
3. **Segment new words** — any word not already in the lexicon is segmented (grapheme→phoneme) and
   added; this is the human-reviewed step that keeps the checker honest.
4. **Run the decodability checker** — compute coverage (must be 100%) and tricky-word density (must
   be within the level cap); emit `decodability.json`. Off-sequence words **block**.
5. **Illustration brief + assets** — produce an illustration brief and source/create **open-licensed
   or original** illustrations; record image provenance + license.
6. **Review** — (a) **pedagogy/decodability** reviewer confirms level fit and the checker report;
   (b) **safeguarding + representation** reviewer confirms child-appropriateness and inclusive,
   non-stereotyped content; (c) for **non-English**, a **native-speaker linguistics/literacy**
   reviewer confirms the sequence, segmentation, and naturalness. Sign-offs recorded in `review.yaml`.
7. **License & provenance check** — text license, illustration license(s), any adapted-source
   provenance, attribution strings all present and compatible.
8. **Export** — render accessible HTML/EPUB/PDF (+ dyslexia font, large-print variants).
9. **Deliver** — hand the set to the program/partner; record **actual use** (Definition of Shipped).

**Artifacts / data model**
- `sequences/<lang>/<id>.yaml` — `{ id, language, sourceName, sourceUrl, licenseName, licenseUrl,
  steps: [ { step, gpcs: [...], trickyWords: [...] } ], notes, verifiedBy, verifiedDate }`.
- `lexicon/<lang>.yaml` — entries `{ word, segmentation: [grapheme→phoneme pairs], syllables,
  notes }`; the **single source of truth** the checker uses to judge decodability.
- `books/<lang>/<book-id>/`:
  - `book.yaml` — `{ id, title, language, sequenceId, step, decodabilityTargetCoverage: 100,
    trickyDensityCap, ageBand, themes, contentRating, authors, illustrators }`.
  - `text.md` — the story (UTF-8 Markdown).
  - `decodability.json` — checker output `{ coverage, trickyDensity, offSequenceWords: [...],
    newTrickyWords: [...], pass: bool }`.
  - `review.yaml` — `{ pedagogySignoff, safeguardingSignoff, linguisticSignoff (if non-EN),
    coiDeclarations, agentFlags, status }`.
  - `provenance.yaml` — `{ originalOrAdapted, adaptedSource?, sourceLicense?, retrievalDate?,
    sequenceVersion, lexiconVersion, authors, illustrators }`.
  - `illustrations/` — image files + `illustrations/provenance.yaml` (per-image license/source).
  - `attribution.txt` — required attributions for any adapted text and for illustrations.
- `templates/illustration-brief.md`, `templates/review-checklist.md`.
- `policy/content-and-representation.md`, `policy/content-rating.md`.

**The decodability checker (the one software component).** Input: `text.md` + `sequenceId` + `step`
+ `lexicon/<lang>.yaml`. It tokenizes the text, looks up each word's segmentation in the lexicon,
and decides for each word: **decodable** (all graphemes' GPCs introduced ≤ step), **tricky-listed**
(on cumulative tricky list ≤ step), or **off-sequence** (fail). Words **not in the lexicon** are
reported as `needs-segmentation` and **block** until added+reviewed — the checker never guesses a
segmentation, which is what keeps the gate trustworthy. Output `decodability.json` + a non-zero exit
on any off-sequence/needs-segmentation word or density-cap breach, so **CI fails closed**.

**Why a curated lexicon (an honest engineering decision).** Robust automatic grapheme→phoneme
*alignment* is unreliable, especially for deep orthographies like English. Early decodables, however,
use a **small, controlled vocabulary** (low hundreds of words at the first levels), so maintaining a
hand-segmented, human-reviewed lexicon is **tractable and far more trustworthy** than auto-G2P. The
lexicon grows only as books need it. (Open pronunciation resources — e.g. public-domain CMUdict for
English — may *seed* candidate segmentations, but every entry used in a published book is
human-reviewed; see licensing.)

**Content schemas & CI validation.** The Task JSON schema lives in `packages/schema/src/schemas.ts`
(AJV / JSON Schema **draft-07**). This project's content artifacts get their **own JSON Schemas in
the same place** — `sequenceSchema`, `lexiconSchema`, `bookSchema`, `decodabilityReportSchema`,
`reviewSchema`, `provenanceSchema` — compiled and exposed via `validate.ts` exactly like
`taskSchema`/`registrySchema`. A structural-check script wired into `pnpm test` **fails CI** on any
malformed artifact; the decodability checker and license check run in the same CI job so a book
cannot merge with a failing level claim or missing attribution.

**Formats & accessibility.** UTF-8 throughout; Markdown canonical for text; YAML for metadata; JSON
for machine reports. Exports: semantic HTML, EPUB 3 (reflowable, screen-reader friendly), print PDF,
plus a **dyslexia-friendly font** option and **large-print / low-ink** variants. Structured text is
kept clean so `read-aloud-audio` can align narration later.

**Key decisions**
- **Decodability-as-data, enforced by a fail-closed gate** — the project's core discipline.
- **Curated segmented lexicon over auto-G2P** — trust over coverage.
- **Multiple published sequences supported; none declared "correct"** — avoids method wars and
  maximizes reuse across programs.
- **Native authoring per language, never translation** — preserves decodability.
- **Originals by default; adaptation only from verified PD/open sources** — minimizes license risk.
- **Accessibility and `literacy-from-zero` integration are first-class**, not afterthoughts.

## Data, licensing & compliance

**This is a critical section. Be conservative; when rights or child-appropriateness are unclear, do
not publish.**

**Text content.**
- **Originals by default.** Stories are **authored originally** for this project and released under
  an open license (default **CC-BY-4.0**; **CC0** offered where a partner/repo prefers public-domain
  dedication for maximum reuse). Original authorship is the cleanest path and the default.
- **Adaptation only from verified sources.** If a story adapts an existing work (e.g., a folktale),
  the source must be **confirmed public-domain or openly-licensed**, with provenance recorded
  (source, edition/version, retrieval date, license name + URL + a **snapshot/hash** of the license
  text). PD status is **jurisdiction-dependent** and must be verified, not assumed; ambiguous status →
  do not adapt. Coordinate with `world-folktales-open` / `public-domain-translations` where relevant.

**Illustrations (a distinct rights surface — handled explicitly).**
- Illustrations are **original (commissioned/contributed) or sourced from openly-licensed / PD image
  collections**, with **per-image** license + source recorded in `illustrations/provenance.yaml` and
  attributed in `attribution.txt`. Share-alike and attribution terms are honored.
- **AI-generated illustration policy (explicit decision required):** because the copyright/licensing
  status of AI-generated images is **legally unsettled and varies by jurisdiction**, AI-generated
  illustrations are **off by default**; if ever used they must be **labeled as such**, carry a clear
  usage statement, and be approved by the maintainer per the content policy. Default path is original
  or openly-licensed human art. (This is an open question for governance — see Open questions.)

**Scope-and-sequence & lexicon data.**
- A scope-and-sequence may be **aligned to** a published order; we record the source and its license.
  Where an order is a copyrighted commercial program, we **do not copy it**; we either use an openly-
  documented order or author/derive a sequence with reviewer sign-off and record that provenance.
- Lexicon segmentations are **factual linguistic data** authored/reviewed by us; where seeded from an
  open resource (e.g., **CMUdict**, U.S. public domain) the seeding source is recorded. Output
  lexicon/sequence data is released **CC-BY-4.0** (or **CC0** on request).

**Output licensing summary.** Project **code/tooling: MIT**. **Books, sequences, lexicons,
templates: CC-BY-4.0 by default (CC0 where preferred)**. Adapted text and all illustrations carry
their **source-compatible** license + required attribution; if a source forbids derivatives or
relicensing, we do not use it.

**Privacy / PII — minimal by design.** Content is **fiction for children**. We ingest **no personal
data**, collect **no learner data**, and store **no images of real identifiable people** unless
explicitly PD/consented and reviewed. No analytics on learners. Partner contacts are handled
out-of-band and never committed. Per CLAUDE.md, **no secrets/tokens/keys** in logs, receipts, or
committed files.

**Provenance model.** Every book ships `provenance.yaml` linking it to its sequence version, lexicon
version, original-or-adapted status (+ source license if adapted), authors, illustrators, and image
provenance. Provenance is **part of the license gate** — incomplete provenance blocks shipping.

## Quality, review & risk gates

**Risk tier: low** (original children's fiction over a controlled vocabulary). **But three hard gates
apply to every book, and non-English work is treated with medium-risk rigor in practice** because its
correctness depends on a reviewer who reads the language.

**Required review before a book is "done"**
1. **Decodability gate (automated + human).** The decodability checker reports **100% coverage** and
   **tricky density within the level cap**, with **zero** off-sequence / needs-segmentation words; a
   **pedagogy/decodability reviewer** confirms the level fit and that any new lexicon segmentations
   are correct. *Fail → not shipped.*
2. **Safeguarding + inclusive-representation gate (human).** A reviewer confirms the book is
   age-appropriate, non-frightening, free of unsafe modeling, and **inclusive/non-stereotyped**
   (gender, race/ethnicity, disability, family structure, religion, region), with a content rating
   recorded. *Fail → not shipped.*
3. **Linguistic gate (human, non-English only).** A **native-speaker literacy/linguistics reviewer**
   confirms the scope-and-sequence, the lexicon segmentations, and that the language is natural and
   correct. *No qualified reviewer → the language is not shipped.*
4. **License & provenance gate (automated + human).** Text + illustration + adapted-source licenses,
   attributions, and provenance are present and compatible. *Fail → not shipped.*
5. **CI green** for the checker, schema structural checks, and any tooling code; **maintainer
   approval** of the PR.

**Reviewer independence & two-reviewer rule.** Reviewers must be **independent of the drafting
step**: the human who ran the authoring agent may **not** be the sole reviewer for that book, and
each reviewer records a **conflict-of-interest declaration**. The **decodability** and
**safeguarding** sign-offs must be by **different people** (a single person may not clear both for the
same book). For **non-English** books the **linguistic** reviewer is **mandatory and independent**.

**Disagreement / conflict resolution.** If reviewers disagree, the book **cannot be signed off**
until resolved: (1) reconcile against the sequence, lexicon, and content policy, recording the
rationale; (2) unresolved issues escalate to the **maintainer** (or a third reviewer); (3) **when in
doubt, the more conservative reading wins** — a contested word is replaced and a contested
safeguarding concern is removed/softened rather than shipped. Resolution is recorded in `review.yaml`.

**Agent uncertainty self-check (operationalized).** The authoring agent must emit explicit flags —
`UNCERTAIN: <location> | <type: off-sequence|segmentation|age-appropriateness|cultural|adapted-source>
| <note>` — for any word it had to reach for, any new segmentation it is unsure of, and any content it
is unsure is appropriate. Flags are copied into `review.yaml` as `agentFlags`; **no sign-off may be
recorded while any flag is unresolved** (each must be `resolved` or `accepted-as-is` with reasoning).

**Definition of Shipped (project-specific).** A book/set is *shipped* only when: acceptance criteria
met **and** the **decodability gate** passes (100% coverage, density within cap) **and**
**safeguarding/representation** sign-off recorded **and** (non-English) **linguistic** sign-off
recorded **and** **license/provenance** verified **and** CI green **and** the set is **delivered to a
program/partner and actually used with real learners** (or adopted as the decodable practice tier in
`literacy-from-zero`). **Merged-but-unused is not shipped.**

## Roadmap & milestones

**M0 — Foundation & cold-start (no partner required).**
Goal: stand up the decodability machinery + policies and prove the pipeline on **one** English book
end-to-end except adoption.
Exit criteria: one **scope-and-sequence published as data** (English, from an openly-documented
order, license recorded); **lexicon schema + seed entries** for the first levels; the
**decodability checker** + content JSON Schemas working and runnable locally; **review rubric**
(decodability + safeguarding/representation) and **content + representation + content-rating policy**
merged; **license/provenance policy** (incl. illustration + AI-image stance) merged; **one original
English decodable book** authored, **checker-passed (100% coverage, density within cap)**, with
**pedagogy + safeguarding** sign-off and a passing license/provenance check; an **illustration brief
+ at least one open-licensed/original illustration** with recorded provenance. `verifiedNeed`
honestly `false` (no partner). *License/decodability gate note:* M0 runs the checker + a **minimal
automated structural check** (schemas + presence of required metadata/attribution) so the "100%"
gates are honest from day one; **full CI enforcement is automated in M1**.

**M1 — A coherent set + repeatability (no partner required).**
Goal: prove this scales to a *teachable set*, not a one-off.
Exit criteria: a **coherent set of ≥ 6 English books** covering the first N steps cumulatively, every
book checker-passed and reviewed; **decodability + schema + license checks enforced in CI**
(fail-closed); **accessible export pipeline** (HTML/EPUB/PDF + dyslexia font + large-print) producing
all set books; **reviewer qualification criteria** published + **≥ 2 qualified reviewers** engaged
(pedagogy + safeguarding); **authoring runbook** merged. Dependency: reviewer sourcing.

**M2 — Multilingual foundations + first partner (needs partner / reviewer).**
Goal: prove the multilingual model **the right way** and deliver an adopted set.
Exit criteria: a **second language's native scope-and-sequence + lexicon** authored with
**native-speaker linguistic review** (prefer a transparent orthography first); a **native-authored
set in that language** (not translated), checker-passed and fully reviewed; **partner/program secured
(`verifiedNeed = true`)** and target language/sequence agreed; **integration with
`literacy-from-zero` confirmed** (books referenced as its decodable practice tier); **≥ 1 set
delivered and confirmed used with real learners** (first true Definition-of-Shipped event).
Dependency: M0/M1 + partner + per-language reviewer.

**M3 — Scale & sustain.**
Goal: scale languages/levels with sustained quality and tracked outcomes.
Exit criteria: ≥ 2 languages with multi-level sets in use; **publication to open repos**
(e.g. StoryWeaver / Global Digital Library) with correct attribution; **reviewer rotation**
established; **outcome tracking** (usage, partner feedback, any defects) operating; named
**sustainability owner**; documented **maintenance cadence** for sequences/lexicons.

## Work breakdown

The itemized, sized backlog lives in **[TASKS.md](./TASKS.md)**, organized by the milestones above
(M0–M3) plus a Backlog/future section. Each task maps to an Elyos Task JSON (schema in
`packages/schema/src/schemas.ts`) with id, type, lane, risk tier, deliverable, acceptance criteria,
and license fields. M0/M1 tasks are partner-independent foundations; M2+ tasks are gated on a secured
partner / per-language reviewer and are marked `verifiedNeed: false` until then.

## Governance, roles & stakeholders

- **Maintainer (Owner): J. Carter (acting)** — assigned now; accepts/sequences tasks, approves PRs,
  owns the scope-and-sequence + lexicon integrity, the decodability gate, and the license/provenance
  gate. Acts as interim license/compliance reviewer until a dedicated reviewer is named.
- **Pedagogy / decodability reviewers: TO BE SECURED** — structured-literacy-literate reviewers
  (teacher, reading specialist, or SLP) who confirm level fit + lexicon segmentation. Rotation
  defined in M1.
- **Safeguarding / inclusive-representation reviewers: TO BE SECURED** — confirm child-appropriate,
  inclusive, non-stereotyped content + content rating; **independent** of the decodability reviewer.
- **Native-speaker linguistic reviewers (per non-English language): TO BE SECURED** — **mandatory**
  for any non-English book; a language with no reviewer is not shipped.
- **Steward (last-mile owner): TBD — named by end of M1** (acting maintainer holds these duties
  meanwhile). Owns the partner/`literacy-from-zero` relationship and confirms **actual use**
  (without which nothing reaches "shipped"). Naming a steward is a **prerequisite for entering M2**.
- **Partner / requestor: TO BE SECURED** — literacy program / `literacy-from-zero` / open repo that
  defines target language + sequence and confirms use.
- **Expert reviewers** — for any drift toward assessment/diagnosis or clinical dyslexia claims (which
  are out of scope), a credentialed expert is required (and the item should be rejected as
  out-of-scope instead).

## Dependencies & integrations

- **Elyos donated lane**: `packages/cli` (workspace prep + PR), `packages/core`, `packages/schema`
  (Task JSON + this project's content schemas). No funded-lane / API-key execution.
- **`literacy-from-zero`** (sibling flagship) — the primary internal consumer/integration target;
  this project supplies its decodable practice tier.
- **Sibling projects** — `read-aloud-audio` (future narration alignment), `open-childrens-books`
  (non-decodable picture books; coordinate to avoid overlap), `open-accessible-fonts`
  (dyslexia/under-served-script fonts), `world-folktales-open` / `public-domain-translations`
  (verified source material for adaptation).
- **Open scope-and-sequence references** and **open pronunciation resources** (e.g. PD CMUdict for
  English seeding) — used under their licenses; every used entry human-reviewed.
- **Open content repos / distribution channels** — StoryWeaver (Pratham Books), Global Digital
  Library/Norad, African Storybook (Saide), Worldreader/Library For All — **external, not yet
  secured**; targets for partner + publication.
- **Human reviewers** (pedagogy, safeguarding, per-language linguistics) — **external dependency, not
  yet secured.**

## Risks & mitigations

| Risk | Likelihood | Impact | Mitigation | Owner |
|---|---|---|---|---|
| A book labeled decodable isn't (off-sequence words) → teaches guessing, harms learners | Medium | High | Fail-closed decodability checker (100% coverage gate); curated reviewed lexicon (no guessed segmentations); pedagogy reviewer confirms; CI fails on any off-sequence word | Maintainer / Pedagogy reviewers |
| Wrong/auto segmentation makes the checker lie (false "pass") | Medium | High | Checker never guesses — unknown words **block** as `needs-segmentation`; every used segmentation human-reviewed; non-EN linguistic reviewer mandatory | Pedagogy / Linguistic reviewers |
| Naive translation of English books breaks decodability in other languages | Medium | High | Hard non-goal; each language authored natively against its own sequence; per-language reviewer gate | Maintainer / Linguistic reviewers |
| Inappropriate, frightening, or stereotyped content reaches children | Low | High | Mandatory independent safeguarding/representation gate + content-rating policy + reviewer checklist; conservative-reading-wins rule | Safeguarding reviewers |
| Illustration/source license violation (esp. images, adapted folktales, AI-image ambiguity) | Medium | High | Originals by default; per-image + per-source provenance/license recorded + checked; AI-images off by default & labeled; "if unclear, don't use" | License reviewer / Maintainer |
| No partner / program secured → nothing reaches "shipped" | High | Medium | M0/M1 build partner-independent value; concrete outreach plan (Open questions) incl. `literacy-from-zero` as the natural first requestor; `verifiedNeed=false` until secured; pause/decision point if none by end of M1 | Acting maintainer → Steward |
| No qualified reviewer for a target language | High | High | Reviewer availability is a hard precondition; don't ship unreviewed; partner with literacy orgs / universities; transparent orthographies first | Steward / Maintainer |
| Method-war / "wrong sequence" disputes stall the project | Medium | Medium | Support multiple published sequences as data; don't declare one "correct"; align to a partner's sequence when one exists | Maintainer |
| Scope creep into assessment/diagnosis/curriculum | Medium | Medium | Explicit non-goals; reject such tasks; require expert + likely out-of-scope | Maintainer |
| Lexicon/sequence quality erodes as it grows | Medium | Medium | Schemas + CI structural checks; review on every new entry; maintenance cadence (M3) | Maintainer |
| Single-person dependence (acting maintainer wears many hats) | Medium | Medium | Reviewer roster + rotation (M1/M3); name Steward before M2; document runbook | Maintainer |

## Security & privacy

- **Threat surface is small**: public, original text + images in a public repo. Main risks are
  **integrity** (a book that fails its decodability claim; an inappropriate-content slip) and
  **license/provenance**, not data exfiltration.
- **No secrets** in the normal flow (donated lane uses the human's own agent; no API keys/escrow).
  Per CLAUDE.md, never write secrets/tokens into logs, receipts, or committed files.
- **PII: none.** No learner data, no analytics, no images of real identifiable people (unless
  explicitly PD/consented + reviewed). Partner contacts kept out-of-band, uncommitted.
- **Child-safety as security.** The safeguarding gate is treated as a release-blocking control, not a
  style note; the content + representation policy is the spec it's checked against.
- **Abuse/misuse prevention.** Refusal guardrails apply: refuse content that is unsafe for children,
  discriminatory/stereotyping, deceptive, or that violates a source license/privacy. Reject any
  attempt to repurpose the checker or texts for harmful ends.
- **Supply-chain / integrity.** Pin sequence + lexicon versions in each book's provenance; hash/
  snapshot any adapted-source license; CI validates all content artifacts; reproducible exports.

## Sustainability & maintenance

- **After delivery**, the maintainer + steward keep sequences and lexicons current, re-verify any
  adapted-source licenses, and re-run the checker if a sequence changes (a sequence edit can change
  *every* dependent book's decodability — so changes are versioned and trigger re-validation).
- **Outcome tracking** continues post-delivery: a usage/feedback log per delivered set, partner
  check-ins on continued use, and a defect log (e.g., a slipped off-sequence word found in the field).
  Outcomes (use, learner decoding signal, defects) — not merge counts — are the maintained metrics.
- **Reviewer rotation** (M1/M3) avoids single-person dependence; **Steward** owns the last mile.
- **Decommissioning / correction.** If an adapted source's license changes or a quality defect is
  found, provenance makes impact assessment possible; affected books are corrected, re-checked, or
  withdrawn and downstream repos notified.

## Open questions

1. **Partner / first requestor.** Which program or repo is the first to *use* a set? **Outreach plan
   (concrete):**
   - **Targets (named):** `literacy-from-zero` (internal, most natural first requestor); StoryWeaver
     (Pratham Books), Global Digital Library/Norad, African Storybook (Saide), Worldreader / Library
     For All; structured-literacy educator communities; university education/SLP departments.
   - **Owner:** acting maintainer until a Steward is named (by end of M1), who then takes it over.
   - **Timeline:** outreach begins in parallel with M0; aim for **≥ 3 serious conversations by end of
     M1** and an **agreed first partner/integration during M2**.
   - **Pause/decision point:** if **no partner/integration is secured by end of M1**, the maintainer
     makes an explicit **go / pivot / hold** decision (e.g., ship the open set + checker as a
     standalone public good and integrate into `literacy-from-zero` directly) rather than authoring
     more books no program has committed to use.
2. **Which scope-and-sequence(s)?** Adopt one openly-documented English order for M0; which? And do we
   support a second English order before going multilingual, or go to a second language first?
   (Default: prove the set in one English order, then a transparent second language.)
3. **Second language choice.** Which transparent-orthography language gives the best
   need × reviewer-availability for M2?
4. **AI-generated illustrations.** Governance decision on the policy (default: off, labeled if ever
   used). Who decides per book?
5. **Lexicon seeding.** Use PD CMUdict (English) to *seed* candidate segmentations (all human-
   reviewed), or hand-author from scratch? (Default: seed-then-review for speed, never auto-accept.)
6. **Effectiveness evidence.** Can a university partner run a small observational study on decoding
   accuracy/confidence? (Aspirational; not an Elyos-made claim without it.)
7. **Funded lane?** Project is donated; is there ever a case for metered authoring under escrow for a
   surge of partner-requested levels? (Out of scope for v0.1; would require `fundedBudgetUsd`.)

## References

- `C:\code\elyos\CLAUDE.md` — Elyos work rules, lanes, quality bar, refusal guardrails.
- `C:\code\elyos\docs\good-deed-definition.md` — good-deed criteria and risk tiers.
- `C:\code\elyos\packages\schema\src\schemas.ts` — Task JSON schema.
- `C:\code\elyos\planning\ROADMAP.md` — portfolio roadmap (decodable-readers is a Track 3 candidate;
  `literacy-from-zero` is the flagship it integrates with).
- `C:\code\elyos\planning\projects\vital-info-translations\PLAN.md` — exemplar for license/provenance
  + reviewer-gate rigor.
- Reading-science / structured-literacy literature on decodable text and scope-and-sequence (to be
  cited per adopted sequence).
- Open content repositories: StoryWeaver (Pratham Books), Global Digital Library (Norad), African
  Storybook (Saide), Worldreader / Library For All — candidate partners + distribution channels.
- CMUdict (U.S. public-domain pronunciation dictionary) — optional English lexicon seeding source.

---

## Appendix A — Improvements applied

The following 25 specific improvements were identified against an initial draft and **applied** in
the plan and tasks above (this is not a wish-list — each is reflected in the text).

1. **Made decodability machine-enforceable, not asserted.** Defined a **decodability checker** that
   fails closed in CI, instead of relying on author claims. (Solution approach; gates.)
2. **Defined "decodability" with two precise numbers** — 100% **coverage** (hard gate) + a
   **tricky-word density cap** (quality gate) — so the gate is unambiguous and testable. (Metrics.)
3. **Made the checker refuse to guess.** Unknown words **block** as `needs-segmentation` rather than
   being auto-segmented, closing the "false pass" hole. (Checker design; risks.)
4. **Chose a curated, reviewed lexicon over auto-G2P**, with an honest rationale (auto-alignment is
   unreliable for English; early vocab is small). (Solution approach.)
5. **Elevated the "no machine translation" rule to a headline non-goal**, with the linguistic reason
   (decodability is language-specific). (Non-goals; risks.)
6. **Added a mandatory, independent safeguarding + inclusive-representation gate** with a content
   policy and per-book content rating — child-safety treated as a release control. (Gates; security.)
7. **Added a mandatory native-speaker linguistic gate for all non-English books**, and stated no
   reviewer ⇒ language not shipped. (Gates; scope.)
8. **Enforced reviewer independence** (drafting human ≠ sole reviewer; decodability ≠ safeguarding
   sign-off by same person) with COI declarations. (Gates.)
9. **Treated illustrations as a separate rights surface** with per-image provenance/license, not
   lumped with text. (Licensing.)
10. **Added an explicit AI-generated-illustration policy** (off by default, labeled, governance
    decision) given unsettled image-copyright law. (Licensing; open questions.)
11. **Made `verifiedNeed=false` honest throughout** and marked partner/requestor **TO BE SECURED**,
    matching the schema and the real candidate status. (Status note; tasks.)
12. **Named `literacy-from-zero` as the natural first internal requestor** and made integration with
    it a tracked outcome + M2 exit criterion. (Beneficiaries; metrics; roadmap.)
13. **Replaced vanity metrics with outcomes** (books *used with learners*, languages end-to-end) and
    added partner-independent **interim foundation metrics** so progress is visible pre-partner.
14. **Added a sample-size rule** (no percentages below 10 books) to avoid small-sample noise. (Metrics.)
15. **Supported multiple published scope-and-sequences** instead of declaring one correct, defusing
    method-war risk. (Non-goals; key decisions; risks.)
16. **Versioned sequences/lexicons and made a sequence edit trigger re-validation** of dependent
    books — a real footgun made explicit. (Sustainability; risks.)
17. **Operationalized the agent uncertainty self-check** with a concrete `UNCERTAIN:` flag format
    that blocks sign-off until resolved. (Gates.)
18. **Added a concrete partner-outreach plan with owner, timeline, and a pause/decision point** if no
    partner by end of M1. (Open questions; risks.)
19. **Phased multilingual deliberately** — transparent (shallow) orthographies first as the
    lowest-risk path to a correct second language. (Scope prioritization; roadmap M2.)
20. **Made accessibility first-class** — EPUB/HTML/PDF, dyslexia font, large-print/low-ink, clean
    structured text for future audio alignment. (Goals; solution approach.)
21. **Drew explicit boundaries with sibling projects** (`open-childrens-books`, `read-aloud-audio`,
    `open-accessible-fonts`, `world-folktales-open`) to prevent duplication. (Non-goals; deps.)
22. **Defined output licensing precisely** — MIT code; CC-BY-4.0 (CC0 optional) content; source-
    compatible for adapted/illustration material. (Licensing.)
23. **Kept PII surface at zero** explicitly (no learner data, no analytics, no real-person images).
    (Privacy.)
24. **Added a fail-closed CI gate uniting** decodability + schema + license checks, phased (manual+
    checker in M0 → automated CI in M1) so the 100% gates are honest from day one. (Roadmap; tasks.)
25. **Added a correction/decommissioning path** (defect log, withdraw/notify downstream) for issues
    found in the field, plus reviewer rotation to remove single-person dependence. (Sustainability.)

## Review sign-off

**Reviewed against the PLAN_SPEC robustness bar and Elyos guardrails on 2026-06-28.**

- **Structure & depth:** all 17 required H2 sections present and in order; depth matches the
  exemplars (Ofelia product-vision clarity + vital-info license/gate rigor). ✔
- **Measurable metrics:** outcome + interim tables carry baseline, target, and a measurement method;
  decodability is defined as two enforceable numbers; a sample-size rule prevents small-sample noise. ✔
- **Enforceable gates:** four hard gates (decodability, safeguarding/representation, non-English
  linguistic, license/provenance) are fail-closed and tied to Definition of Shipped; CI enforcement
  phased M0→M1. ✔
- **Risks:** table has Likelihood, Impact, Mitigation, **and a named Owner** for every row;
  highest-severity rows (false-decodable, bad segmentation, child-content, license) all mitigated. ✔
- **Guardrails:** license/provenance is conservative (originals default; verify PD/open; per-image
  rights; AI-image stance); **PII = none**; child-safeguarding is a release control; non-English work
  gated on a native reviewer; refusal guardrails referenced. ✔
- **Honesty:** project is correctly stated as a **candidate** with **no partner**; `verifiedNeed`
  is `false` on partner-dependent tasks; partner/requestor marked **TO BE SECURED**. ✔
- **Sequencing:** M0 thin cold-start (one verified book) → M1 coherent set + CI enforcement →
  M2 multilingual + partner + `literacy-from-zero` integration → M3 scale/sustain; dependencies are
  explicit and realistic. ✔
- **Schema validity:** TASKS.md maps every field of `taskSchema`; the example Task JSON was checked
  against the draft-07 schema (all required fields present, enums valid, `verifiedNeed=false`, real
  `outputLicense`). ✔

**Issues found & fixed during review:** (a) tightened the metric denominator/sample rule; (b) made
the sequence-edit re-validation footgun explicit in Sustainability and Risks; (c) added the
independence rule that decodability and safeguarding sign-offs must be different people; (d) ensured
the M0 license/decodability gate is honest (checker + structural check now, full CI in M1).
**No blocking issues remain.** Items needing a **human decision** before execution: choose the M0
English scope-and-sequence; ratify the AI-illustration policy; secure the first partner / confirm
`literacy-from-zero` integration; source pedagogy/safeguarding/linguistic reviewers.
