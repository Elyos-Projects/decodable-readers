# TASKS — decodable-readers

> Status: Draft · Version: 0.1.0 · Last updated: 2026-06-28 · Owner: J. Carter (acting maintainer) · Lane: donated

The backlog for the `decodable-readers` good-deed project. Read alongside [PLAN.md](./PLAN.md).
Milestones (M0–M3) match the roadmap there.

## How these tasks map to Elyos

Each task below becomes an **Elyos Task JSON** validated against `packages/schema/src/schemas.ts`
(AJV / JSON Schema draft-07). Field mapping:

- **id** — stable slug id, e.g. `decodable-readers-checker-001` (table column `ID`).
- **title** — the task title.
- **project** — always `decodable-readers`.
- **type** — one of `code | research | writing | data | design-spec | maintenance` (table `Type`).
- **lane** — `donated` for all current tasks (no funded/API execution). Funded tasks would need
  `fundedBudgetUsd`; none here.
- **priority** — `high | medium | low`.
- **domain** — array, e.g. `["education","literacy","phonics"]`.
- **riskTier** — `low | medium | high`. Project tier is **low**; **non-English** authoring/sequence/
  lexicon tasks are set **medium** because they require a native-speaker linguistic reviewer (table
  `Risk`).
- **urgent** — boolean (default `false`; no live emergency).
- **deliverable** — `pr | dataset | document | translation` (table `Deliverable`). Books/stories are
  authored content; we use **`document`** for a book and its metadata bundle, **`dataset`** for
  sequences/lexicons, **`pr`** for tooling/CI, and `design-spec` type for illustration specs.
  (Note: the schema's `deliverable` enum has no "book" value; `document` is the correct fit.)
- **tokenEstimate** — `small | medium | large` (table `Size`).
- **status** — `open | in-progress | review | delivered | done` (all start `open`).
- **context / objective** — why + what.
- **acceptanceCriteria[]** — checkable bullets (listed below tables for key tasks).
- **resources[]** — links/paths (PLAN, schemas, sequence sources, policies, templates).
- **output** — path/description of the produced artifact.
- **requestor** — partner/requestor; **`TO BE SECURED`** until a partner/program is confirmed.
- **verifiedNeed** — boolean; **`false`** wherever value depends on an unsecured partner.
- **outputLicense** — **MIT** for code/tooling; **CC-BY-4.0** (or **CC0** on request) for books,
  sequences, lexicons, templates; adapted text/illustrations carry their **source-compatible**
  license + attribution.

**Sizing:** small ≈ <1 day · medium ≈ 1–3 days · large ≈ 3–7 days of focused agent+human work.

---

## Milestone M0 — Foundation & cold-start (no partner required)

Goal: stand up the decodability machinery + policies and prove the pipeline on **one** English book
end-to-end except adoption.

| ID | Title | Type | Size | Risk | Deliverable | Depends on | Reviewer |
|---|---|---|---|---|---|---|---|
| decodable-readers-sequence-001 | Publish an English scope-and-sequence as data (open-documented order, license recorded) | data | small | low | dataset | — | Maintainer / Pedagogy reviewer |
| decodable-readers-policy-001 | Content + inclusive-representation + content-rating policy | writing | small | low | document | — | Maintainer / Safeguarding reviewer |
| decodable-readers-license-001 | License & provenance policy (text + per-image + adapted-source + AI-image stance) + per-book check procedure | research | small | low | document | — | License reviewer / Maintainer |
| decodable-readers-schema-001 | Content JSON Schemas (sequence/lexicon/book/decodability/review/provenance) + structural CI check | code | medium | low | pr | sequence-001 | Maintainer |
| decodable-readers-checker-001 | Decodability checker tool (coverage + density; blocks on off-sequence / needs-segmentation) | code | medium | low | pr | sequence-001, schema-001 | Maintainer / Pedagogy reviewer |
| decodable-readers-lexicon-001 | Curated segmented English lexicon (seed for first levels) | data | small | low | dataset | sequence-001, schema-001 | Pedagogy reviewer |
| decodable-readers-rubric-001 | Review rubric/checklist (decodability + safeguarding/representation + linguistic) | writing | small | low | document | policy-001 | Maintainer |
| decodable-readers-illus-001 | Illustration brief template + image provenance policy + first open-licensed/original illustration | design-spec | small | low | design-spec | license-001 | Maintainer / Safeguarding reviewer |
| decodable-readers-book-001 | Author + verify first English decodable book (level ~1–2), end-to-end except adoption | writing | medium | low | document | sequence-001, lexicon-001, checker-001, rubric-001, illus-001 | Pedagogy reviewer + Safeguarding reviewer (independent) |

**Acceptance criteria — key M0 tasks**

`sequence-001` (English scope-and-sequence as data)
- `sequences/en/<id>.yaml` lists ordered **steps**; each step declares `gpcs` introduced and
  `trickyWords` pre-taught, cumulatively interpretable.
- Records `sourceName`, `sourceUrl`, `licenseName`, `licenseUrl`, `verifiedBy`, `verifiedDate`; the
  order is from an **openly-documented** sequence (no copying of a copyrighted commercial program).
- Validates against `sequenceSchema`; passes the structural CI check.

`checker-001` (decodability checker)
- Given `text.md` + `sequenceId` + `step` + `lexicon/en.yaml`, classifies each running word as
  **decodable / tricky-listed / off-sequence / needs-segmentation** using only the lexicon's
  segmentations (the checker **never guesses** a segmentation).
- Emits `decodability.json` with `coverage`, `trickyDensity`, `offSequenceWords[]`,
  `newTrickyWords[]`, `pass`; **exits non-zero** on any off-sequence/needs-segmentation word or on a
  tricky-density-cap breach (fail-closed).
- Has unit tests with fixtures (a passing book, a book with one off-sequence word, a book over the
  density cap); CI runs them.

`book-001` (first decodable book)
- One **original** English story authored to a named `sequenceId` + `step`; engaging and
  age-appropriate.
- Decodability checker reports **100% coverage** and **density within the level cap**; **zero**
  off-sequence / needs-segmentation words. Any new words are added to `lexicon/en.yaml` **with
  reviewed segmentations**.
- Ships `book.yaml`, `text.md`, `decodability.json`, `review.yaml`, `provenance.yaml`,
  `illustrations/` (+ `illustrations/provenance.yaml`), `attribution.txt`.
- Agent `UNCERTAIN:` flags captured into `review.yaml` as `agentFlags`; **no sign-off while any flag
  is unresolved**.
- **Pedagogy/decodability sign-off** and **independent safeguarding/representation sign-off**
  recorded (different people; COI declared); content rating set per `policy-001`.
- License/provenance check (`license-001`) passes: text license = CC-BY-4.0 (or CC0); every
  illustration has a recorded license/source; attribution complete.

`policy-001` (content + representation + rating)
- Defines age bands, what is **not** allowed (frightening/unsafe/inappropriate content), **inclusive,
  non-stereotyped representation** requirements, and a **content-rating** scheme recorded per book.
- Sets per-level **tricky-word density caps** the checker enforces.

**M0 Definition of Done:** English scope-and-sequence published as data (license recorded); content/
license/provenance policies + review rubric merged; **decodability checker + content schemas + a
minimal automated structural check** runnable and green in CI; seed English lexicon merged; **one
original English decodable book authored, checker-passed (100% coverage, density within cap), with
independent pedagogy + safeguarding sign-off and a passing license/provenance check**, including at
least one open-licensed/original illustration with recorded provenance. Full fail-closed CI
enforcement is **effective from M1**; M0 verifies the first book via checker + structural check +
manual review. Partner adoption deferred to M2 (so M0 deliverables carry `verifiedNeed: false`).

---

## Milestone M1 — A coherent set + repeatability (no partner required)

Goal: prove this scales to a *teachable set*, with the gates enforced in CI and reviewers engaged.

| ID | Title | Type | Size | Risk | Deliverable | Depends on | Reviewer |
|---|---|---|---|---|---|---|---|
| decodable-readers-bookset-001 | Author a coherent set of ≥ 6 English books across the first N steps (cumulative) | writing | large | low | document | book-001 | Pedagogy + Safeguarding reviewers |
| decodable-readers-ci-001 | Enforce decodability + schema + license checks in CI (fail-closed) | code | small | low | pr | checker-001, schema-001, license-001 | Maintainer |
| decodable-readers-export-001 | Accessible export pipeline (HTML/EPUB/PDF + dyslexia font + large-print/low-ink) | code | medium | low | pr | book-001 | Maintainer |
| decodable-readers-reviewers-001 | Reviewer qualification criteria + onboarding (pedagogy, safeguarding, linguistic) + engage ≥ 2 | research | medium | low | document | rubric-001 | Maintainer / Steward |
| decodable-readers-runbook-001 | Authoring pipeline runbook (pick level → draft → segment → check → review → export) | writing | small | low | document | book-001, ci-001 | Maintainer |

**Acceptance criteria — key M1 tasks**

`bookset-001` (coherent set)
- ≥ 6 original English books covering the first **N** steps **cumulatively** (each book ≤ its step;
  later books may use earlier GPCs), forming a coherent reading progression.
- Every book passes the **decodability gate** (100% coverage, density within cap) via CI, and carries
  independent pedagogy + safeguarding sign-offs and complete license/provenance.
- The set's level coverage is documented (which steps each book targets).

`ci-001` (fail-closed CI gate)
- CI runs the decodability checker on every book, the schema structural checks on every artifact, and
  the license/provenance check; **any** off-sequence word, schema violation, density-cap breach, or
  missing attribution **fails the build**. This makes the "100%" gates **automated from M1**.

`reviewers-001` (reviewer network)
- Written, objective criteria for each reviewer role (pedagogy/decodability; safeguarding/
  representation; native-speaker linguistic), including a **conflict-of-interest declaration** and the
  **independence rule** (drafting human ≠ sole reviewer; decodability ≠ safeguarding sign-off by same
  person; non-English requires an independent linguistic reviewer) and the **disagreement-resolution**
  rule (reconcile → escalate to maintainer/third reviewer → conservative reading wins; recorded in
  `review.yaml`).
- **≥ 2 qualified reviewers** engaged (or a literacy-org/university partner providing them).

`export-001` (accessibility)
- Produces clean **HTML, EPUB 3 (reflowable, screen-reader friendly), and print PDF** for every set
  book, plus a **dyslexia-friendly font** option and a **large-print / low-ink** variant; output is
  reproducible and runs in CI.

**M1 Definition of Done:** a coherent ≥ 6-book English set, every book checker-passed and reviewed;
**decodability + schema + license checks enforced fail-closed in CI**; accessible export pipeline
producing all set books (incl. dyslexia font + large print); reviewer qualification criteria
published (incl. independence + conflict resolution) and **≥ 2 reviewers engaged**; authoring runbook
merged. **Steward named** (governance prerequisite for M2). Partner-dependent value still
`verifiedNeed: false`.

---

## Milestone M2 — Multilingual foundations + first partner (needs partner / reviewer)

Goal: prove the multilingual model the right way and deliver a set that is **actually used**. All
delivery tasks gated on a secured partner; `verifiedNeed` flips to `true` only when confirmed.

| ID | Title | Type | Size | Risk | Deliverable | Depends on | Reviewer |
|---|---|---|---|---|---|---|---|
| decodable-readers-partner-001 | Secure first partner/program; agree language + sequence; confirm `literacy-from-zero` integration | research | medium | low | document | reviewers-001 | Steward / Maintainer |
| decodable-readers-sequence-002 | Native scope-and-sequence + lexicon for a 2nd language (transparent orthography), linguistically reviewed | data | medium | medium | dataset | reviewers-001 | Native linguistic reviewer |
| decodable-readers-bookset-002 | Author a native decodable set in the 2nd language (NOT translated) | writing | large | medium | document | sequence-002, ci-001 | Native linguistic + Safeguarding reviewers |
| decodable-readers-integrate-001 | Integrate the English set into `literacy-from-zero` as its decodable practice tier | data | small | low | document | bookset-001 | Maintainer / literacy-from-zero owner |
| decodable-readers-delivery-001 | Package + deliver a set; confirm actual use with real learners | writing | small | low | document | bookset-001 or bookset-002, partner-001 | Steward |

**Acceptance criteria — key M2 tasks**

`partner-001`
- Outreach (acting maintainer → Steward) targets named candidates (`literacy-from-zero`, StoryWeaver,
  Global Digital Library/Norad, African Storybook, Worldreader/Library For All, educator/university
  groups), aiming for **≥ 3 serious conversations by end of M1**.
- **Pause/decision point:** if **no partner/integration is secured by end of M1**, the maintainer
  records an explicit **go / pivot / hold** decision before further authoring.
- A named program confirmed as requestor; agreed target language + scope-and-sequence; reviewer
  coverage for the language confirmed. On completion, related tasks update `requestor` and
  `verifiedNeed: true`.

`sequence-002` + `bookset-002` (multilingual, done right)
- The 2nd-language sequence + lexicon are **authored natively** (or aligned to that language's own
  published order) and **reviewed by a native-speaker linguistics/literacy reviewer** — **not** ported
  from English.
- Books are **authored natively** against that sequence; **no translated-and-retagged English text**.
  Every book passes the decodability gate **for that language** and carries linguistic + safeguarding
  sign-offs and complete license/provenance.

`delivery-001`
- Delivered set includes books + exports + provenance + attribution + all required sign-offs; CI
  green; **partner confirms actual use with real learners in writing** (recorded in the PR/receipt).
  This is the first true **Definition of Shipped** event.

**M2 Definition of Done:** partner/program secured (`verifiedNeed=true`); a 2nd language's native
sequence + lexicon linguistically reviewed and a native-authored set checker-passed + reviewed;
English set **integrated into `literacy-from-zero`**; **≥ 1 set delivered and confirmed used with
real learners**.

---

## Milestone M3 — Scale & sustain

Goal: scale languages/levels with sustained quality and tracked outcomes.

| ID | Title | Type | Size | Risk | Deliverable | Depends on | Reviewer |
|---|---|---|---|---|---|---|---|
| decodable-readers-scale-001 | Extend sets across more levels/languages in use | writing | large | medium | document | delivery-001 | Pedagogy + Linguistic + Safeguarding reviewers |
| decodable-readers-distribute-001 | Publish sets to open repos (StoryWeaver / Global Digital Library) with correct attribution | maintenance | small | low | document | delivery-001 | Steward / License reviewer |
| decodable-readers-outcomes-001 | Outcome tracking: usage + partner feedback + defect log | data | small | low | dataset | delivery-001 | Steward |
| decodable-readers-maint-001 | Sequence/lexicon maintenance cadence + re-validation on sequence edits + correction/withdrawal procedure | maintenance | small | low | document | sequence-001, lexicon-001 | Maintainer |
| decodable-readers-rotation-001 | Reviewer rotation + load balancing | maintenance | small | low | document | reviewers-001 | Maintainer |

**Acceptance criteria — key M3 tasks**

`outcomes-001`
- A maintained log capturing, per delivered set: usage status, partner-reported usefulness, any
  learner decoding signal the partner observes (clearly marked as the partner's observation, not an
  Elyos claim), and **post-delivery defects** (e.g., a slipped off-sequence word) with target **0**.
  Feeds PLAN.md success metrics.

`maint-001`
- Documented cadence to re-verify adapted-source licenses and refresh lexicons; **a sequence edit
  triggers re-running the checker on every dependent book** (sequences are versioned). Defines the
  **correction/withdrawal** procedure (fix → re-check → or withdraw + notify downstream repos).

**M3 Definition of Done:** ≥ 2 languages with multi-level sets in use; sets published to ≥ 1 open
repo with correct attribution; outcome tracking live; maintenance cadence + re-validation +
correction procedure in effect; reviewer rotation operating; named sustainability owner.

---

## Backlog / future

Sized but unscheduled:

- **(medium) Second English scope-and-sequence** — support an additional published order for programs
  that teach a different sequence (reuses the checker; new sequence data + re-checked books).
- **(large) Additional transparent + then deeper orthographies** — expand the multilingual catalog as
  reviewer coverage grows.
- **(medium) `read-aloud-audio` alignment hooks** — emit word-timing-ready structured text so
  narration can be aligned later (coordinate with that project; no audio produced here).
- **(small) Decodable-decodability "explain" mode** — per-word annotation output for teacher review
  (which GPC/step each word relies on).
- **(small) Teacher one-pager per set** — which step each book targets + the pre-taught tricky words
  (a reference card, not a lesson plan).
- **(large, funded — needs escrow) Surge authoring under funded lane** — metered authoring of
  partner-requested levels against a hard per-task budget; would require `fundedBudgetUsd` and is out
  of scope for v0.1.

---

## Example task JSON

Schema-valid Task JSON for the first M0 task. `verifiedNeed` is **false** (no partner secured);
`outputLicense` is **CC-BY-4.0** because the scope-and-sequence is openly-licensed content data.

```json
{
  "id": "decodable-readers-sequence-001",
  "title": "Publish an English scope-and-sequence as data",
  "project": "decodable-readers",
  "type": "data",
  "lane": "donated",
  "priority": "high",
  "domain": ["education", "literacy", "phonics", "early-reading"],
  "riskTier": "low",
  "urgent": false,
  "deliverable": "dataset",
  "tokenEstimate": "small",
  "status": "open",
  "context": "decodable-readers publishes phonics-aligned decodable storybooks whose level claims must be machine-checkable. The decodability checker needs the teaching order encoded as data: an ordered list of steps, each declaring the grapheme-phoneme correspondences (GPCs) introduced and the high-frequency 'tricky' words pre-taught at that step. The order must come from an openly-documented scope-and-sequence (no copying of a copyrighted commercial program), with its source and license recorded, so every later book can be verified cumulatively against it.",
  "objective": "Create sequences/en/<id>.yaml encoding an openly-documented English scope-and-sequence as ordered steps (GPCs introduced + tricky words pre-taught per step), with source/license provenance, so the decodability checker can verify book level claims deterministically.",
  "acceptanceCriteria": [
    "sequences/en/<id>.yaml lists ordered steps; each step declares the GPCs introduced and the tricky words pre-taught, interpretable cumulatively",
    "Records sourceName, sourceUrl, licenseName, licenseUrl, verifiedBy, verifiedDate; the order is from an openly-documented sequence with no copying of a copyrighted commercial program",
    "File validates against sequenceSchema and passes the CI structural check",
    "A short README documents the sequence schema, the chosen order, and how books reference a sequenceId + step",
    "Any ambiguity in source rights is resolved conservatively: if the order's license is unclear, an openly-documented alternative is used and the decision is recorded"
  ],
  "resources": [
    "C:/Users/jason/AppData/Local/Temp/claude/C--code-elyos/5eca0d44-6b8b-4c30-9696-37a524cb249a/scratchpad/plans/decodable-readers/PLAN.md",
    "C:/code/elyos/packages/schema/src/schemas.ts",
    "C:/code/elyos/docs/good-deed-definition.md",
    "Openly-documented English phonics scope-and-sequence reference (license to be recorded)"
  ],
  "output": "sequences/en/<id>.yaml plus a README documenting the sequence schema, source/license provenance, and the book-referencing convention (sequenceId + step)",
  "requestor": "TO BE SECURED",
  "verifiedNeed": false,
  "outputLicense": "CC-BY-4.0"
}
```
