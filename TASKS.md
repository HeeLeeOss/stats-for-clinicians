# TASKS — stats-for-clinicians

> Status: Draft · Version: 0.1.0 · Last updated: 2026-06-28 · Owner: TBD (maintainer) · Lane: donated
>
> **Binding guardrails (apply to every task):** clinician-facing **methods education only** — no
> medical advice, no treatment recommendations, no patient-facing content (that escalates to
> `riskTier: high` with oncologist + advocate sign-off). Every worked example uses **open-access /
> aggregate / de-identified / synthetic data only**; controlled-access and identifiable patient data
> are out of scope; verify each example dataset's license. **Provenance on every assertion** (each
> claim cited to an authoritative methods source; each number reproducible from a committed script).
> A stated statistical falsehood is a release-failing defect; **credentialed biostatistician sign-off
> is non-skippable** before any explainer ships.

## How these tasks map to Hee-Lee Oss

Each task below becomes a Hee-Lee Oss **Task JSON** validated against
`packages/schema/src/schemas.ts`. Field mapping:

- `id` — stable slug from the tables (e.g. `stats-for-clinicians-template-002`).
- `title` — the table's Title.
- `project` — `stats-for-clinicians`.
- `type` — one of `code | research | writing | data | design-spec | maintenance` (per table).
- `lane` — `donated` for all current tasks (no funded escrow); a funded task adds `fundedBudgetUsd`
  (see backlog `funded-021`).
- `priority` — `high | medium | low`.
- `domain` — array, e.g. `["cancer-research","biostatistics","medical-education","open-education"]`.
- `riskTier` — `low | medium | high`. Authoring/accuracy/license tasks are `medium`; any
  patient-facing/treatment-advice derivative would be `high` (out of scope here).
- `urgent` — boolean; `false` for all current tasks.
- `deliverable` — `pr | dataset | document | translation`. Code (toolkit/harness) → `pr`; explainers,
  template, gates, checklists → `document`; generated/redistributed example data → `dataset`.
- `tokenEstimate` — `small | medium | large` (Size column).
- `status` — `open | in-progress | review | delivered | done`; all start `open`.
- `context`, `objective`, `acceptanceCriteria[]`, `resources[]`, `output` — per task.
- `requestor` — **TO BE SECURED** until an adopting beneficiary is confirmed.
- `verifiedNeed` — **`false`** until a named training program / education committee / journal-club
  network / OER repository / advocacy org confirms it will adopt the output (the general need is real;
  per-output adoption need is unproven).
- `outputLicense` — `CC-BY-4.0` for explainers/content/datasets we author; `MIT` for toolkit/harness
  code; redistributed open datasets keep their upstream license (recorded in the manifest).

---

## Milestone M0 — Foundation & cold-start

| ID | Title | Type | Size | Risk | Deliverable | Depends on | Reviewer |
| --- | --- | --- | --- | --- | --- | --- | --- |
| stats-for-clinicians-reviewer-001 | Name/secure the biostatistician + licensing/provenance reviewers (blocking gate roles) | research | small | low | document | — | Maintainer |
| stats-for-clinicians-template-002 | Explainer authoring template + curriculum map + "not clinical advice" frame | design-spec | medium | medium | document | — | Technical, Stats, Clinical |
| stats-for-clinicians-datagate-003 | Licensing & data gate + per-dataset data manifest (open/synthetic only) | design-spec | small | medium | document | — | Licensing+Provenance |
| stats-for-clinicians-hr-004 | Explainer: hazard ratio + proportional-hazards assumption (highest-misread) | writing | medium | medium | document | template-002, datagate-003, repro-005 | Stats, Clinical |
| stats-for-clinicians-repro-005 | Reproducibility harness (regenerate worked-example figures/numbers in CI) | code | medium | low | pr | template-002 | Technical |
| stats-for-clinicians-km-006 | Explainer: censoring & Kaplan–Meier curves + at-risk tables | writing | medium | medium | document | template-002, datagate-003, repro-005 | Stats, Clinical |
| stats-for-clinicians-syntheticdata-007 | Synthetic survival-data generator + ≥1 license-verified open teaching dataset (manifest entries) | data | small | low | dataset | datagate-003 | Licensing+Provenance, Technical |
| stats-for-clinicians-outreach-008 | Adopting-beneficiary outreach + partner shortlist (training programs, OER, advocacy) | research | small | low | document | — | Steward |
| stats-for-clinicians-comprehension-009 | Pre/post comprehension-check instrument + pilot on a reader group | research | small | medium | document | hr-004, km-006 | Stats, Clinical |

**Acceptance criteria — key tasks**

- **template-002 (authoring template + curriculum map)**
  - [ ] Template defines every explainer field: `learningObjectives[]`, `plainDefinition`,
        `preciseDefinition`, `workedExample {datasetRef, scriptRef, figures[], reproducible}`,
        `commonTrap[] {misreading, why_wrong, correct_reading}`, `howToReadInAPaper[]`,
        `assumptionsAndLimits[]`, `claims[] {statement, sourceRef, sourceType}`, `provenance`,
        `framing`, `accessibility`, `review {statsReviewer, statsSignoff, clinicalReviewer, status}`,
        `specVersions`.
  - [ ] A required, structural **"educational, not clinical advice"** `framing` field on every
        explainer (not a footnote); treatment-advice/patient-facing content explicitly excluded.
  - [ ] Curriculum map enumerates the concept set (core survival → trial-reading → advanced/bias),
        prioritized by how often each is misread, with HR + KM first.
  - [ ] Every conceptual claim requires a `sourceRef` (authoritative methods source) and every
        quantitative claim requires a `scriptRef` — uncited/unreproducible content is invalid.
  - [ ] `pnpm build && pnpm test && pnpm lint` pass for committed template tooling; DCO signed-off.

- **datagate-003 (licensing & data gate + manifest)**
  - [ ] Enumerates the in-bounds data classes (synthetic; license-verified open teaching datasets;
        open aggregate summaries; CC-BY/CC0 curve-derived synthetic pseudo-data) and the hard
        exclusions (individual patient data, dbGaP/EGA/individual-level biobanks, controlled-access,
        identifiable records).
  - [ ] Objective PASS criterion: dataset usable only if its license is recorded (id + evidence) and
        permits our teaching use + redistribution where applicable; missing/unparseable = EXCLUDE.
  - [ ] Requires a committed **data manifest** entry per example dataset (source, version, license,
        de-identification status, redistribution permission).
  - [ ] Curve-derived pseudo-data must be synthetic-by-construction from CC-BY/CC0 figures only,
        labelled synthetic, never claimed as real patients, never re-identifying.
  - [ ] Forbids reproducing copyrighted figures verbatim (recreate from open/synthetic data instead).

- **hr-004 (hazard ratio explainer)**
  - [ ] Plain + precise definition of a hazard ratio; explicit `commonTrap` (HR is **not** a simple
        risk ratio, not a constant "X% lower risk over the whole period"; it is an instantaneous,
        time-averaged relative hazard) with why-wrong and correct-reading.
  - [ ] States the **proportional-hazards assumption** and what to check (and what RMST offers when
        it fails), in `assumptionsAndLimits`.
  - [ ] Worked example built on an open/synthetic dataset (manifest entry present); every number/
        figure regenerated by a committed script and diff-matched in CI (100% reproducible).
  - [ ] Every conceptual claim cited to an authoritative methods source; "how to read it in a paper"
        checklist included; alt-text + colorblind-safe figures.
  - [ ] **Biostatistician sign-off recorded** and clinical-framing review passed; structural "not
        clinical advice" frame present; zero stated statistical falsehoods.

- **repro-005 (reproducibility harness)**
  - [ ] CI regenerates every worked-example figure/number from its committed script on its cited
        open/synthetic dataset and **diff-matches** the published values; any mismatch fails the build.
  - [ ] A statistic cannot appear in an explainer unless the harness can produce it (no hand-typed
        numbers); figures are checksummed and recorded in `provenance`.
  - [ ] Uses only committed synthetic/open datasets; MIT; CI green.

**M0 Definition of Done:** biostatistician + licensing reviewers **named** (blocking, before the pilot
is reviewed); authoring template + curriculum map + "not clinical advice" frame published; licensing/
data gate + data manifest authored and applied; reproducibility harness green in CI on the pilot's
open/synthetic datasets; **≥ 2 pilot explainers (HR, KM) biostatistician-signed-off** and clinical-
framing-reviewed with 100% reproducible figures and full provenance; comprehension check piloted on
≥ 1 reader group; ≥ 1 beneficiary-outreach thread opened; pilot adopted via an informal/self-serve
channel (evidence recorded) — or published with the adoption blocker surfaced.

---

## Milestone M1 — Core survival-analysis set hardened

| ID | Title | Type | Size | Risk | Deliverable | Depends on | Reviewer |
| --- | --- | --- | --- | --- | --- | --- | --- |
| stats-for-clinicians-median-010 | Explainer: median survival, survival-at-t & at-risk numbers | writing | small | medium | document | template-002, repro-005 | Stats, Clinical |
| stats-for-clinicians-logrank-cox-011 | Explainers: log-rank test + Cox regression (assumptions & what it adjusts) | writing | medium | medium | document | hr-004, km-006 | Stats, Clinical |
| stats-for-clinicians-ci-pvalue-012 | Explainer: confidence intervals vs p-values (and what each does/doesn't say) | writing | medium | medium | document | template-002, repro-005 | Stats, Clinical |
| stats-for-clinicians-partner-013 | Secure first confirmed adopting beneficiary | research | small | low | document | outreach-008 | Steward |
| stats-for-clinicians-absrel-nnt-014 | Explainer: absolute vs relative risk + NNT/NNH | writing | medium | medium | document | template-002, repro-005 | Stats, Clinical |
| stats-for-clinicians-provgate-015 | Provenance gate in CI (claims cited + datasets licensed) | code | small | low | pr | template-002, datagate-003 | Technical, Licensing+Provenance |

**Acceptance criteria — key tasks**

- **logrank-cox-011 (log-rank + Cox)**
  - [ ] Plain + precise treatment of the log-rank test (what it tests, that it follows from
        proportional hazards) and Cox regression (what "adjusting for covariates" means and assumes).
  - [ ] Explicit `commonTrap` for each (e.g. log-rank low power under crossing curves; "adjusted HR"
        misread as causal); assumptions/limits stated; worked examples 100% reproducible.
  - [ ] Every claim cited; biostatistician sign-off recorded; clinical-framing passed; checklist + a11y.

- **ci-pvalue-012 (CI vs p-value)**
  - [ ] Corrects the two classic traps — non-significant ≠ "no effect"; significant ≠ "important/large"
        — with why-wrong and correct-reading; explains what a CI conveys that a p-value does not.
  - [ ] Worked example reproducible in CI; every claim cited; biostatistician sign-off; "not clinical
        advice" frame; checklist included.

- **partner-013 (first confirmed adopting beneficiary)**
  - [ ] A named training program / education committee / journal-club network / OER repo / advocacy
        org confirms it will adopt/teach from the explainers.
  - [ ] Adoption mechanism documented (curriculum embed / journal-club kit / repository acceptance).
  - [ ] Tasks for that beneficiary updated to `verifiedNeed: true` with `requestor` set.

- **provgate-015 (provenance gate)**
  - [ ] CI verifies every conceptual claim in each explainer has a resolvable `sourceRef` and every
        example dataset is recorded in the manifest with a permitted license.
  - [ ] Build fails on an uncited claim or an unlicensed/unrecorded dataset; MIT; CI green.

**M1 Definition of Done:** core set authored and **biostatistician-signed-off** (median/at-risk,
log-rank, Cox, CI-vs-p, absolute-vs-relative/NNT, plus M0's HR + KM); reproducibility gate = 100% and
provenance gate green in CI; readability + accessibility targets met across the set; comprehension lift
measured on ≥ 3 core misconceptions; ≥ 1 confirmed adopting beneficiary **or** a documented self-serve
adoption by a verifiable third party.

---

## Milestone M2 — Advanced/bias set, interactive toolkit & release v1.0

| ID | Title | Type | Size | Risk | Deliverable | Depends on | Reviewer |
| --- | --- | --- | --- | --- | --- | --- | --- |
| stats-for-clinicians-surrogate-rmst-016a | Explainers: surrogate endpoints (PFS/DFS/ORR/pCR) vs OS + RMST | writing | medium | medium | document | logrank-cox-011 | Stats, Clinical |
| stats-for-clinicians-bias-016b | Explainers: immortal-time bias, lead-time bias, competing risks | writing | medium | medium | document | km-006, logrank-cox-011 | Stats, Clinical |
| stats-for-clinicians-trialdesign-016c | Explainers: subgroup/multiplicity, non-inferiority, forest plots & heterogeneity | writing | medium | medium | document | ci-pvalue-012, absrel-nnt-014 | Stats, Clinical |
| stats-for-clinicians-checklist-016 | "How to read it in a paper" checklist deck (journal-club/tumor-board ready) | writing | small | medium | document | hr-004, km-006, ci-pvalue-012 | Stats, Clinical |
| stats-for-clinicians-a11y-017 | Accessibility + readability pass (alt-text, colorblind-safe curves, reading level) | design-spec | small | low | document | hr-004, km-006 | Plain-language/Accessibility |
| stats-for-clinicians-toolkit-018 | Interactive toolkit (KM generator + HR-under-non-PH simulator) on open/synthetic data | code | large | medium | pr | repro-005, hr-004, km-006 | Technical, Stats |
| stats-for-clinicians-release-019 | Release v1.0 (versioned + DOI, CC-BY content + MIT toolkit), core + advanced set | data | medium | medium | dataset | surrogate-rmst-016a, bias-016b, trialdesign-016c, toolkit-018, partner-013 | Stats, Licensing+Provenance |

**Acceptance criteria — key tasks**

- **surrogate-rmst-016a (surrogate endpoints + RMST)**
  - [ ] Explains why a surrogate endpoint (PFS/DFS/ORR/pCR) is **not** automatically overall survival,
        and the conditions for a surrogate to be trustworthy; `commonTrap` made explicit.
  - [ ] Introduces RMST as an alternative summary when proportional hazards fails (ties back to
        hr-004); worked example 100% reproducible; every claim cited; biostatistician sign-off.
  - [ ] Stays methods-education (no claim that any specific drug's surrogate benefit implies survival
        benefit); "not clinical advice" frame; checklist + a11y.

- **toolkit-018 (interactive toolkit)**
  - [ ] KM-curve generator and an "HR under non-proportional hazards" simulator run entirely on
        open/synthetic data; no patient data; no clinical calculation on a user's own patient data.
  - [ ] Deterministic, tested, reproducible figures; alt-text/colorblind-safe; MIT; CI green; no
        embedded credentials.
  - [ ] Each widget carries the "educational, not clinical advice" frame and links its source claims.

- **release-019 (release v1.0)**
  - [ ] Covers the core + advanced set; **every explainer biostatistician-signed-off**, 100%
        reproducible, fully cited, accessibility-passed, with the "not clinical advice" frame.
  - [ ] Only open/synthetic example data with verified licenses (manifest complete); no copyrighted
        figure reproduced; no individual-patient data.
  - [ ] Versioned + DOI'd; CC-BY-4.0 content + MIT toolkit; checklist deck included; release notes
        record `specVersions` (template/datasets/toolkit/guideline editions).

**M2 Definition of Done:** advanced/bias set authored and signed off; interactive toolkit released
(MIT, open/synthetic data, CI-tested); checklist deck published; release v1.0 published (versioned +
DOI, CC-BY + MIT); ≥ 6 explainers **adopted/in active use** by a named beneficiary (evidence artifacts
recorded).

---

## Milestone M3 — Reuse, translation-readiness, corrections & sustainability

| ID | Title | Type | Size | Risk | Deliverable | Depends on | Reviewer |
| --- | --- | --- | --- | --- | --- | --- | --- |
| stats-for-clinicians-errata-019m | Corrections/erratum process (versioned, attributed) | design-spec | small | low | document | release-019 | Maintainer, Stats |
| stats-for-clinicians-i18nready-020 | Translation-readiness: text/figure separation + localization export | code | small | low | pr | release-019 | Technical |
| stats-for-clinicians-refresh-020m | Refresh process: updated guideline editions + dataset/toolkit drift | maintenance | small | low | document | template-002, toolkit-018 | Maintainer |
| stats-for-clinicians-reuse-021m | Track + verify external adoption/reuse events | research | small | low | document | release-019, partner-013 | Steward |

**Acceptance criteria — key tasks**

- **reuse-021m (adoption/reuse tracking)**
  - [ ] ≥ 3 verifiable external adoption/reuse events recorded (a syllabus/journal-club kit embedding
        an explainer, an OER repository acceptance, a documented training using it) — externally
        verifiable, no self-reported use.
  - [ ] Each event links to the consumed release version and the acceptance evidence artifact
        (`outcomes/<release-id>.json`).

- **errata-019m (corrections/erratum process)**
  - [ ] Public mechanism for anyone to flag a statistical error or a misleading framing.
  - [ ] Corrections are versioned and attributed; a confirmed falsehood triggers an erratum **and** a
        review of related explainers; superseded versions are marked, not silently overwritten.

- **i18nready-020 (translation-readiness)**
  - [ ] Explainer text is separated from figures/code so a localizer can translate strings without
        touching computation; a localization-ready export is produced (synergy: `oncology-glossary-
        multilingual`).
  - [ ] Figures regenerate from the same scripts after string substitution; MIT; CI green.

**M3 Definition of Done:** corrections/erratum process live; ≥ 3 verifiable adoption/reuse events;
translation-readiness export shipped; refresh process documented (guideline editions + dataset/toolkit
drift) with a named steward; cumulative adoption recorded against the success metrics.

---

## Backlog / future

| ID | Title | Type | Size | Risk | Deliverable | Notes |
| --- | --- | --- | --- | --- | --- | --- |
| stats-for-clinicians-funded-021 | Funded-lane batch authoring/computation run (budget-capped) | code | large | medium | pr | **funded** lane → requires `fundedBudgetUsd`; runs via `packages/runner` with a hard cap; keys never logged |
| stats-for-clinicians-patientfacing-022 | (GATED, out of scope here) patient-facing plain-language versions | writing | large | high | document | Separate `high`-risk deed: oncologist + patient-advocate sign-off + "not medical advice" required before any work |
| stats-for-clinicians-missingdata-023 | Explainer: missing data, attrition & intention-to-treat basics | writing | medium | medium | document | Extends the core set; biostatistician-reviewed |
| stats-for-clinicians-bayes-024 | Explainer: frequentist vs Bayesian reading of a result (intro) | writing | medium | medium | document | Advanced; scope-guard against becoming a stats course |
| stats-for-clinicians-slidedeck-025 | Slide/teaching-deck export for educators (from the explainer source) | code | medium | low | pr | Educator-facing; CC-BY; depends on adoption demand |

---

## Example task JSON

```json
{
  "id": "stats-for-clinicians-template-002",
  "title": "Explainer authoring template + curriculum map + 'not clinical advice' frame",
  "project": "stats-for-clinicians",
  "type": "design-spec",
  "lane": "donated",
  "priority": "high",
  "domain": ["cancer-research", "biostatistics", "medical-education", "open-education"],
  "riskTier": "medium",
  "urgent": false,
  "deliverable": "document",
  "tokenEstimate": "medium",
  "status": "open",
  "context": "Clinicians routinely misread oncology trial statistics (hazard ratios as simple risk ratios, non-significant p-values as 'no effect', surrogate endpoints as overall survival). Before authoring any explainer, the project needs one shared authoring template and a prioritized curriculum map that every explainer is a projection of. The template must make accuracy and provenance structural: every conceptual claim requires an authoritative-source citation, every worked-example number requires a committed reproducibility script, every explainer carries an explicit 'common trap' section and a standing 'educational, not clinical advice' frame, and a credentialed biostatistician sign-off is required before release. Methods education only — no treatment advice, no patient-facing content, no individual patient data; worked examples use open-access/aggregate/de-identified/synthetic data with each dataset's license verified.",
  "objective": "Define and publish the canonical explainer authoring template and the prioritized curriculum map (HR and Kaplan-Meier first) that every authoring, review, and release task will use.",
  "acceptanceCriteria": [
    "Template defines all explainer fields: learningObjectives[], plainDefinition, preciseDefinition, workedExample {datasetRef, scriptRef, figures[], reproducible}, commonTrap[] {misreading, why_wrong, correct_reading}, howToReadInAPaper[], assumptionsAndLimits[], claims[] {statement, sourceRef, sourceType}, provenance {sources[], dataManifestRefs[], generatedFiguresChecksum}, framing, accessibility {altTextComplete, readabilityScore, colorblindSafeFigures}, review {statsReviewer, statsSignoff, clinicalReviewer, plainLanguageReviewer, status}, specVersions.",
    "A required, structural 'educational, not clinical advice' framing field is mandatory on every explainer; patient-facing content and treatment/dosing advice are explicitly excluded and flagged as a separate riskTier:high deed.",
    "Curriculum map enumerates the concept set (core survival analysis -> trial-reading -> advanced/bias), prioritized by how often each concept is misread, with hazard ratio and Kaplan-Meier sequenced first.",
    "Template requires a sourceRef (authoritative methods source: peer-reviewed | textbook | guideline) for every conceptual claim and a scriptRef for every quantitative claim; uncited or unreproducible content is invalid by construction.",
    "Each explainer must carry an explicit commonTrap section and a howToReadInAPaper checklist; biostatistician statsSignoff is required before status can become 'published'.",
    "Template tooling is committed with golden valid/invalid fixtures exercised in CI; pnpm build && pnpm test && pnpm lint pass; commit DCO signed-off."
  ],
  "resources": [
    "C:\\code\\hee-lee-oss\\planning\\projects\\stats-for-clinicians\\PLAN.md",
    "C:\\code\\hee-lee-oss\\packages\\schema\\src\\schemas.ts",
    "Reporting guidelines: CONSORT, SAMPL, STROBE, ICH E9; EQUATOR network",
    "Recognized survival-analysis methods literature and textbooks (cited per-claim, never copied)",
    "Open/synthetic example data: simulated survival datasets; license-verified open teaching survival datasets; open-access aggregate survival summaries"
  ],
  "output": "A published explainer authoring template plus a prioritized curriculum map and the standing 'not clinical advice' framing, committed to the project repo and ready for reuse by every authoring, review, and release task.",
  "requestor": "TO BE SECURED",
  "verifiedNeed": false,
  "outputLicense": "CC-BY-4.0"
}
```

---

## Backlog rollup

- **26 milestone tasks** (M0: 9 · M1: 6 · M2: 7 · M3: 4) + **5 backlog tasks** = **31 tasks**.
- Deliverables (all 31): `document` (explainers/specs/checklists) 23 · `pr` (code/toolkit/harness) 6 ·
  `dataset` (generated/redistributed example data + release) 2.
- Risk (all 31): `low` 12 · `medium` 18 · `high` 1 (`patientfacing-022`, gated/out of scope here).
- Lanes: all `donated` except `funded-021` (carries `fundedBudgetUsd`).
- Blocking gates before any explainer ships: biostatistician + licensing reviewers named
  (reviewer-001), licensing/data gate (datagate-003), reproducibility gate (repro-005), provenance
  gate (provgate-015), and a non-skippable biostatistician sign-off with a zero-falsehood rule.
  `verifiedNeed` stays `false` portfolio-wide until an adopting beneficiary is confirmed (partner-013).

---

## Generated task index

> Generated by Hee-Lee Oss task-decomposition agent on 2026-06-29. All 31 tasks validated against
> `packages/schema/src/schemas.ts` (draft-07, additionalProperties:false). Seed file
> `tasks/stats-for-clinicians-template-002.json` was pre-existing; 30 new task files were generated.
> Fan-out: none — all tasks are explicitly enumerated in the milestone tables above.

| File | Milestone | Type | Priority | Deliverable | Risk |
| --- | --- | --- | --- | --- | --- |
| tasks/stats-for-clinicians-reviewer-001.json | M0 | research | high | document | low |
| tasks/stats-for-clinicians-template-002.json | M0 (seed) | design-spec | high | document | medium |
| tasks/stats-for-clinicians-datagate-003.json | M0 | design-spec | high | document | medium |
| tasks/stats-for-clinicians-hr-004.json | M0 | writing | high | document | medium |
| tasks/stats-for-clinicians-repro-005.json | M0 | code | high | pr | low |
| tasks/stats-for-clinicians-km-006.json | M0 | writing | high | document | medium |
| tasks/stats-for-clinicians-syntheticdata-007.json | M0 | data | high | dataset | low |
| tasks/stats-for-clinicians-outreach-008.json | M0 | research | medium | document | low |
| tasks/stats-for-clinicians-comprehension-009.json | M0 | research | medium | document | medium |
| tasks/stats-for-clinicians-median-010.json | M1 | writing | medium | document | medium |
| tasks/stats-for-clinicians-logrank-cox-011.json | M1 | writing | medium | document | medium |
| tasks/stats-for-clinicians-ci-pvalue-012.json | M1 | writing | medium | document | medium |
| tasks/stats-for-clinicians-partner-013.json | M1 | research | high | document | low |
| tasks/stats-for-clinicians-absrel-nnt-014.json | M1 | writing | medium | document | medium |
| tasks/stats-for-clinicians-provgate-015.json | M1 | code | medium | pr | low |
| tasks/stats-for-clinicians-surrogate-rmst-016a.json | M2 | writing | medium | document | medium |
| tasks/stats-for-clinicians-bias-016b.json | M2 | writing | medium | document | medium |
| tasks/stats-for-clinicians-trialdesign-016c.json | M2 | writing | medium | document | medium |
| tasks/stats-for-clinicians-checklist-016.json | M2 | writing | medium | document | medium |
| tasks/stats-for-clinicians-a11y-017.json | M2 | design-spec | low | document | low |
| tasks/stats-for-clinicians-toolkit-018.json | M2 | code | medium | pr | medium |
| tasks/stats-for-clinicians-release-019.json | M2 | data | high | dataset | medium |
| tasks/stats-for-clinicians-errata-019m.json | M3 | design-spec | medium | document | low |
| tasks/stats-for-clinicians-i18nready-020.json | M3 | code | low | pr | low |
| tasks/stats-for-clinicians-refresh-020m.json | M3 | maintenance | low | document | low |
| tasks/stats-for-clinicians-reuse-021m.json | M3 | research | medium | document | low |
| tasks/stats-for-clinicians-funded-021.json | Backlog | code | low | pr | medium |
| tasks/stats-for-clinicians-patientfacing-022.json | Backlog | writing | low | document | high |
| tasks/stats-for-clinicians-missingdata-023.json | Backlog | writing | low | document | medium |
| tasks/stats-for-clinicians-bayes-024.json | Backlog | writing | low | document | medium |
| tasks/stats-for-clinicians-slidedeck-025.json | Backlog | code | low | pr | low |
