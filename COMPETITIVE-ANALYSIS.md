# Competitive + Improvement Analysis — `stats-for-clinicians`

> Scope: plain-language explainers of survival analysis / hazard ratios / oncology statistics for
> clinicians and trainees. Medium risk; statistician/expert review is the dominant accuracy gate.
> Guardrails: accurate, expert-reviewed, sourced; open-licensed; explicitly not a substitute for
> formal biostatistics training and not clinical advice.
> Analysis date: 2026-06-29. Plan reviewed: PLAN.md v0.1.0 (Draft, 2026-06-28).

---

## 1. Correctness & completeness review of PLAN.md

The plan is unusually strong and self-aware: it already names the right dominant risk (statistical
accuracy, not infra/privacy), front-loads a non-skippable biostatistician sign-off, mandates
reproducible-figures-only, and sequences the two most-misread concepts (HR + Kaplan–Meier) first.
The 17-section structure, the authoring template, the claims ledger, and the data manifest are all
appropriate. What follows is a critical pass focused on *statistical substance*, because that is
where an authoritative-sounding-but-wrong explainer would do the most harm.

**Statistical-accuracy strengths (correctly anticipated in the plan).**
- The HR concept list correctly distinguishes hazard ratio from risk ratio and from a constant
  "X% lower risk," and explicitly pairs HR with the proportional-hazards (PH) assumption. This is
  exactly the error documented in the oncology-trial literature, where intertwined early KM curves
  signal PH violation and make the reported HR "not clinically interpretable"
  (PMC11479825; JNCI Cancer Spectrum pkac007).
- Surrogate endpoints (PFS/DFS/ORR/pCR vs OS), immortal-time bias, lead-time bias, competing risks,
  subgroup/multiplicity, and non-inferiority framing are all in scope. That is a complete-enough
  map of the high-yield misreadings; few competitors cover this *whole* set in one place.
- RMST is included as a first-class topic, which matches the current methodological direction
  (RMST is increasingly recommended precisely *because* HR fails under non-PH).

**Gaps / risks the plan should tighten (correctness findings):**

1. **The PH-assumption explainer needs a "what to do when it's violated" arm, not just "name the
   assumption."** The plan lists "assumptions and limits" per explainer, but the highest-value
   teaching point is the *consequence*: under non-PH the single HR is a time-averaged summary that
   can hide crossing curves, delayed effects (immunotherapy), or early harm. The explainer must
   teach the diagnostic cues (crossing/intertwined KM curves, log-cumulative-hazard plots,
   Schoenfeld residuals at a conceptual level) and the alternatives (RMST, milestone survival,
   weighted log-rank). Recommend making "HR + PH" and "RMST/alternatives when PH fails" an
   explicitly cross-linked pair rather than two independent entries.

2. **"Median survival" is a known trap that deserves its own explicit common-trap treatment.** The
   plan lists "median survival & at-risk tables" but should call out specific misreadings: median
   is unstable when curves are flat near 50% survival; a *median ratio* is not the hazard ratio
   (a documented point of clinician confusion — LITFL CCC; PMC478551); and median survival ignores
   the tail (long-term responders) that often drives the clinical decision.

3. **Censoring needs the *informative-censoring* caveat, not just the mechanics.** KM/log-rank
   assume censoring is non-informative (independent of prognosis). Teaching censoring only as
   "incomplete follow-up" without the independence assumption is the kind of oversimplification the
   plan's own Risk #"oversimplification introduces a new misconception" warns against. Make the
   non-informative-censoring assumption a structural `assumptionsAndLimits` item for the KM
   explainer.

4. **p-value / CI explainer should adopt an authoritative misinterpretation taxonomy rather than
   write one from scratch.** Greenland et al. 2016 ("Statistical tests, P values, confidence
   intervals, and power: a guide to misinterpretations," PMC4877414) enumerates 25 specific
   misreadings and is the canonical, open, citable source. Anchoring the explainer's `commonTrap`
   list to this taxonomy both strengthens provenance and reduces the chance the explainer itself
   states a subtle falsehood (e.g., "the 95% CI has a 95% probability of containing the true
   value"). Recommend citing it explicitly in the claims ledger for the CI/p-value explainer.

5. **"Number at risk" reading and KM tail instability** should be an explicit checklist item in
   "how to read it in a paper": when the at-risk count is small, the curve's tail is noise, and
   confidence bands widen. The plan implies this but does not make it a structural checkpoint.

6. **Surrogate-endpoint explainer must separate *trial-level* from *patient-level* surrogacy.** A
   common error is to treat a within-patient PFS→OS correlation as validation of surrogacy; proper
   surrogate validation is trial-level (correlation of treatment *effects* across trials). The
   literature in this space (e.g., neoadjuvant breast pCR/iDFS surrogacy debates, JCO-24-01360) is
   exactly about this distinction. Make it the explainer's headline trap.

7. **Comprehension instrument is under-specified and is a real validity risk.** The plan commits to
   a pre/post misconception quiz but flags (correctly, in Open Questions) that the instrument and
   cohort size are undecided. Recommend adopting/adapting a *published, validated* statistical-
   literacy item bank rather than authoring novel items, and reporting effect sizes with CIs (and
   eating its own dog food on small-N interpretation). An unvalidated home-grown quiz could itself
   mis-measure "comprehension lift," undermining the outcome metric.

8. **The reproducibility gate is excellent but needs a numerical-tolerance and seed policy.** "Diff
   against published values" must define floating-point tolerance and pin RNG seeds for synthetic
   data; otherwise CI will either flap or silently accept drift. Minor but load-bearing for the
   "no hand-typed numbers" guarantee.

9. **Reviewer-credential bar should be operationalized.** "Credentialed biostatistician/
   epidemiologist" is the right gate, but the plan should specify *what counts* (e.g., terminal
   degree in (bio)statistics/epidemiology + survival-analysis publication record) and require a
   named, signed `statsReviewer` identity in the sign-off block for auditability, since the whole
   trust model rests on this one role. Single-reviewer risk is partly mitigated by the "rotate ≥2"
   rule; consider requiring two independent sign-offs specifically for the foundational HR/KM/CI
   explainers that everything else builds on.

10. **Accessibility for survival curves is more than colorblind-safe palettes.** KM plots are
    information-dense; alt-text must convey the *clinically relevant* features (curve separation,
    crossing, censoring ticks, at-risk numbers), not just "a survival curve." Recommend a structured
    alt-text spec for figures.

**Completeness:** all 17 PLAN_SPEC sections are present and the governance/honesty posture
(`verifiedNeed:false`, requestor TO BE SECURED, "delivered = adopted") is exemplary. The single
largest *project* risk remains non-statistical: no confirmed adopting beneficiary. The self-serve
CC-BY fallback is a reasonable de-risk, but see §6 for stronger cold-start moves.

---

## 2. Competitive landscape

The space is crowded with *good but fragmented* resources. None combine all four of: oncology-
specific + plain-language + openly-licensed/reusable + explicitly expert-reviewed-and-reproducible.

**BMJ Statistics Notes (Altman & Bland)** — the gold-standard short-format series; survival/KM,
log-rank, and HR notes are canonical and heavily cited.
- Strengths: authoritative, peer-reviewed, concise, trusted by clinicians for 30+ years.
- Weaknesses: not openly licensed (BMJ paywall/copyright), not oncology-specific, no reproducible
  code/figures, no interactive elements, and dated typography/format; no "how to read it in a
  paper" checklist artifact.
- https://www.medcalc.org/en/manual/statistical-literature-BMJ-notes.php ·
  https://www-users.york.ac.uk/~mb55/pubs/pbstnote.htm ·
  https://pmc.ncbi.nlm.nih.gov/articles/PMC403858/ (the logrank test)

**"Statistics at Square One" (Swinscow/Campbell, BMJ Books)** — bestselling intro medical-stats
textbook, partially free online.
- Strengths: friendly, comprehensive fundamentals, explicitly flags common errors.
- Weaknesses: general (not oncology), book-length (not bite-size), licensing restricted, no survival
  depth, no reproducible figures, no interactivity.
- https://pmc.ncbi.nlm.nih.gov/articles/PMC1742433/ ·
  https://publish.uwo.ca/~gzou2/Stats_at_Square1.pdf

**JTO "Biostatistics Primer: What a Clinician Ought to Know" series** — the *closest direct
competitor*: oncology-specific primers (Hazard Ratios; Subgroup Analyses; Prognostic vs Predictive
Factors) written explicitly for clinicians.
- Strengths: exactly the audience and framing; oncology context; peer-reviewed; even had a published
  *erratum* (proof the accuracy bar matters — and that errors slip through without a repro gate).
- Weaknesses: paywalled/journal-copyright (not reusable OER), no code/reproducible figures, no
  interactive simulators, incomplete coverage, no machine-readable claims/provenance, static.
- https://www.jto.org/article/S1556-0864(15)32030-X/fulltext (HR) ·
  https://www.jto.org/article/S1556-0864(15)32152-3/fulltext (subgroups) ·
  https://www.jto.org/article/S1556-0864(15)31084-4/fulltext (HR erratum)

**Nature Methods "Points of Significance" / "Statistics for Biologists" collection** — high-quality,
heuristic, simulation-driven stats explainers incl. survival/censoring and regression of
time-to-event data.
- Strengths: superb pedagogy (simulated examples, graphical, mathematically careful), respected.
- Weaknesses: aimed at *biologists/researchers*, not clinicians reading trials; not oncology-
  decision-focused; Nature copyright (not CC-BY OER); no checklist artifacts; no interactivity.
- https://www.nature.com/collections/qghhqm/pointsofsignificance ·
  https://www.nature.com/articles/s41592-022-01563-7 (survival/censoring)

**Students 4 Best Evidence (Cochrane)** — free, student-friendly EBM blog with HR and KM tutorials.
- Strengths: free, plain, beginner-oriented, large reach, Cochrane-affiliated.
- Weaknesses: blog-quality and uneven; *not formally expert-reviewed per post*; some posts contain
  oversimplifications; not oncology-specific; no reproducible figures/provenance ledger; not a
  versioned, citable, curated library.
- https://s4be.cochrane.org/blog/2016/04/05/tutorial-hazard-ratios/ ·
  https://s4be.cochrane.org/blog/tag/hazard-ratio/

**Centre for Evidence-Based Medicine (CEBM, Oxford)** — practical resources (e.g., estimating HR
from a KM curve and at-risk numbers).
- Strengths: practical, trusted, Oxford brand, meta-analysis/data-extraction focus.
- Weaknesses: task-specific and fragmented (not a structured clinician curriculum), not oncology-
  framed, licensing unclear, no reproducible toolkit, no comprehension assessment.
- https://www.cebm.ox.ac.uk/resources/data-extraction-tips-meta-analysis/estimating-hazard-ratio-kaplan-curve-numbers-at-risk

**ASCO University / ASCO eLearning + ASCO–AACR Methods Workshop** — oncology society courses
(biostatistics for trials, endpoints, subgroup analysis) and an intensive methods workshop.
- Strengths: oncology-authoritative, society-backed, structured, CME, large oncology audience.
- Weaknesses: membership/registration-gated and *not openly licensed* (not OER you can embed/remix),
  course-format (not quick reference), not reproducible/auditable, not free.
- https://connection.asco.org/do/new-updated-asco-elearning-courses-put-oncology-education-your-fingertips ·
  https://www.asco.org/career-development/trainee-fellow-resources

**UpToDate (point-of-care reference)** — clinicians' default lookup, includes some
statistics/EBM background sections.
- Strengths: ubiquitous at point of care, clinically trusted, maintained.
- Weaknesses: expensive subscription (the opposite of open), not a methods curriculum, shallow on
  survival-statistics mechanics, no reproducible figures, no remixable license.

**Adjacent primary literature (not competitors, but the authoritative source base):** PH-violation
incidence in phase-3 oncology trials (PMC11479825; JNCI pkac007); Greenland et al. p-value/CI
misinterpretation guide (PMC4877414); survival-analysis primers for clinician-scientists
(PMC8743691); StatPearls Survival Analysis (NCBI Bookshelf NBK560604). These should be *cited
inputs*, not things we duplicate.

**Net:** every incumbent is missing at least two of {open/CC-BY-remixable, oncology-trial-decision-
focused, reproducible-figures, interactive, structured "trap + paper-reading checklist" artifacts,
machine-readable provenance}. That intersection is the open lane.

---

## 3. Gaps we can fill

1. **An openly-licensed (CC-BY) oncology-stats explainer library** — the authoritative incumbents
   (BMJ, JTO, Nature, ASCO, UpToDate) are all copyright-restricted; educators cannot legally embed/
   remix them into curricula. OER reusability is the single biggest unmet need.
2. **Reproducible, regenerable figures** — *no* competitor ships the script that produced the curve.
   "See why HR misleads under non-PH" via a runnable simulator beats "be told it does."
3. **The complete bias/trap set in one curated place** — incumbents cover HR *or* surrogates *or*
   subgroups; none assemble censoring→KM→HR/PH→Cox→CI/p→absolute-vs-relative/NNT→surrogates→RMST→
   competing-risks→immortal/lead-time→subgroups→non-inferiority→forest-plots as one versioned set.
4. **Structural "common trap" + "how to read it in a paper" checklist artifacts** — a journal-club/
   tumor-board-ready deck is a format no incumbent provides.
5. **Machine-readable provenance + per-claim citation ledger** — auditable accuracy, and reusable by
   downstream tools (RAG, MCP, sibling projects). Unique.
6. **Measured comprehension lift** — competitors publish; almost none *measure whether the reader's
   specific misconception was corrected*. An evidence-of-impact posture is differentiating.
7. **Interactive simulators** (KM generator; HR-under-non-PH; immortal-time visualizer) under MIT —
   nothing comparable is open.
8. **Accessibility + translation-readiness** (structured KM alt-text, colorblind-safe curves, text/
   figure separation) — essentially absent across incumbents.

---

## 4. Differentiators to win

1. **Correct-first, provably**: non-skippable credentialed-biostatistician sign-off + a zero-
   falsehood release-failing rule + reproducible-figures-only CI. The JTO HR *erratum* is the proof
   that even peer-reviewed primers ship errors; our gate is stronger and *visible*.
2. **Open + remixable (CC-BY content / MIT toolkit)** where every authoritative competitor is
   paywalled — educators can legally drop modules into curricula.
3. **Show, don't assert**: every figure is regenerable from committed code on open/synthetic data,
   plus interactive simulators that make non-PH/immortal-time *visible*.
4. **"Teach the trap" + paper-reading checklists** as structural artifacts, not prose asides — built
   for the journal club / tumor board where the misreadings actually happen.
5. **Auditable provenance**: a per-claim citation ledger + data manifest, so a skeptical statistician
   can verify the library, and downstream tools can consume it.
6. **Outcome-measured**: comprehension-lift evidence and "delivered = adopted," not "explainers
   written" — credibility no blog or textbook claims.

---

## 5. Claude API leverage (and hard limits)

**Where Claude adds the most value (drafting/transform, human-and-code-verified):**
1. **Draft explainer prose to the fixed template** from human-supplied authoritative sources —
   plain definition, precise definition, the `commonTrap` triple (misreading → why wrong → correct
   reading), and the "how to read it in a paper" checklist — for biostatistician editing. Drafting,
   not deciding.
2. **Generate practice interpretation exercises** — e.g., "here is a KM figure + HR + CI; which of
   these four interpretations is wrong and why," graded answer keys, and pre/post misconception-quiz
   item *candidates* (then validated by a statistician against a published item bank).
3. **Propose worked-example scaffolding and figure specs** — suggest which synthetic/open dataset
   and which visualization best exposes a given trap (e.g., crossing-curves dataset to show non-PH),
   and draft the *narrative* around figures the deterministic script produces.
4. Secondary: draft structured KM **alt-text**, readability-leveling passes, source-summary triage
   for the claims ledger (every citation still human-verified), and consistency/cross-link checks
   across explainers.

**Where Claude must NOT decide (hard guardrails — match the plan):**
- **Statistical truth.** No definition, interpretation, "trap," assumption, or correction ships on
  the model's authority. A credentialed biostatistician sign-off is the blocking accuracy gate; a
  single stated falsehood fails the release.
- **Numbers/figures.** Every statistic and figure is emitted by the committed reproducibility script
  (deterministic code), never by the model. No model-typed number ever appears in prose.
- **Citations/provenance.** No fabricated, guessed, or "plausible" references, datasets, effect
  sizes, or examples. Every claim traces to a recorded, resolvable authoritative source; every
  number to a script on a license-verified open/synthetic dataset.
- **Clinical drift.** The model must not introduce treatment/prognosis/"which drug is better"
  framing; any such drift is out of scope and escalates to `riskTier: high`.
- **Data boundary.** The model never invents "realistic" patient data or reconstructs identifiable
  pseudo-IPD; synthetic-by-construction only, from open figures, labelled as such.

(Per Hee-Lee Oss rules: model is behind a thin, vendor-neutral provider interface; donated lane = human
runs their own agent; any funded drafting runs via `packages/runner` under a hard budget cap; keys
never enter logs/receipts.)

---

## 6. Ten concrete optimizations

1. **Pair HR/PH with an explicit "when PH fails" explainer** (RMST, milestone survival, weighted
   log-rank) and cross-link them; teach the diagnostic cues (crossing/intertwined KM curves), per
   PMC11479825 / JNCI pkac007. This is the highest-yield content fix.
2. **Anchor the CI/p-value `commonTrap` list to Greenland et al. 2016 (PMC4877414)** — adopt its
   25-misinterpretation taxonomy as the citable backbone; reduces the risk of the explainer itself
   stating a CI/p falsehood.
3. **Make "median survival ≠ median-ratio ≠ HR" an explicit trap** with a worked example showing
   median instability near flat curves and tail/long-responder loss.
4. **Add the non-informative-censoring assumption as a structural `assumptionsAndLimits` item** on
   the KM/censoring explainer, with a synthetic example of informative censoring biasing the curve.
5. **Split surrogate surrogacy into trial-level vs patient-level** as the headline trap (cite the
   neoadjuvant pCR/iDFS surrogacy literature, JCO-24-01360).
6. **Adopt/adapt a validated comprehension instrument** instead of authoring novel quiz items; pre-
   register the pre/post design, report effect sizes with CIs, and state a minimum cohort.
7. **Lead the cold-start with the journal-club/tumor-board checklist deck** (not the prose library):
   it is the lowest-friction adoptable artifact and the best outreach hook to a journal-club lead —
   front-load it from M0 rather than M2.
8. **Require two independent biostatistician sign-offs for the foundational HR/KM/CI explainers**
   (everything else builds on them); single sign-off is fine for downstream topics.
9. **Specify the reproducibility gate's numerical tolerance + pinned RNG seeds** and a structured
   alt-text spec for KM figures, so CI neither flaps nor silently drifts and figures are accessible.
10. **Ship a per-explainer "evidence-of-impact card"** (pre/post delta, reviewer identities, source
    list, dataset+license, figure checksums) — turns the provenance ledger into a public trust
    artifact and a reuse/MCP-ready record.

---

## 7. Parallel & perpendicular spin-offs

**Parallel (same engine, adjacent audience/output):**
- **`oncology-data-literacy`** — shares the explainer engine and curriculum map; this project's
  clinician explainers become the "deep" tier behind a broader data-literacy track (overlapping
  audience explicitly noted in the plan).
- **`patient-advocate-research-primer`** — the *high*-risk patient-facing derivative: same concepts,
  re-leveled with oncologist + patient-advocate sign-off and "not medical advice." The text/figure
  separation built here is the enabler; this is the natural sibling for the escalated lane.
- **`ewing-outcomes-harmonization`** — a downstream *consumer*: harmonized rare-cancer outcomes need
  exactly these survival/HR/competing-risks reading skills; the checklist deck plugs straight in,
  and Ewing-track family guides reuse clinician-facing companion material.
- **`oncology-glossary-multilingual`** — consumes the translation-ready text/figure separation to
  localize the set without re-authoring.

**Perpendicular (new capability the engine unlocks):**
- **A reusable clinical-stats explainer engine** — the template + claims-ledger + data-manifest +
  reproducible-figure harness + review-gate workflow is domain-general. Re-pointed at other high-
  stakes literatures (cardiology trial reading, diagnostic-test stats/likelihood ratios, public-
  health screening), it becomes Hee-Lee Oss's standard "correct-first, reproducible, expert-gated explainer
  factory."
- **`systematic-review-assist`** — the HR-from-KM-curve extraction skills (CEBM-style) and the
  forest-plot/heterogeneity explainers feed directly into a systematic-review/meta-analysis assist
  tool; the reproducible toolkit's curve-digitization-to-synthetic-data utilities are shared infra.
- **An MCP server** exposing the machine-readable explainer library + claims ledger + checklists as
  tools (e.g., `explain_hazard_ratio`, `check_PH_assumption_cues`, `read_this_paper_checklist`,
  `interpretation_quiz`). Any agent (or a clinician's assistant) could query *expert-reviewed,
  sourced* stats explanations instead of hallucinating them — a high-leverage, guardrail-preserving
  distribution channel (server returns only signed-off, cited content; never generates new stats).

---

## 8. Open questions

1. **First confirmed adopting beneficiary** — which training program / education committee / journal-
   club network / OER repo (MedEdPORTAL-style) / advocacy org? (Plan's own #1 blocker.) Does leading
   with the checklist deck (Opt. 7) accelerate this?
2. **Comprehension instrument** — adopt which *validated* item bank, what minimum cohort, and what
   counts as a defensible "lift"? (Avoid a home-grown quiz that mis-measures.)
3. **Pseudo-IPD from published curves** — permit synthetic-by-construction reconstruction from CC-BY
   KM figures, or restrict strictly to synthetic + named open datasets to remove all reconstruction
   ambiguity? (Note KM-GPT-style auto-reconstruction tooling exists; the *ethical/licensing* line,
   not the technical one, is the question.)
4. **Depth ceiling** — how far into RMST / competing risks / non-inferiority before it stops being
   "plain-language" and becomes a stats course? Define a "precision vs accessibility" stopping rule.
5. **Reviewer supply** — can we name and retain ≥2 credentialed biostatistician reviewers (the whole
   trust model depends on this single role)? What is the fallback if the named reviewer lapses?
6. **Interactive widgets in v1.0 or deferred** — do static reproducible figures suffice for first
   adoption, given the maintenance cost of interactive components?
7. **Readability target** — what reading level fits clinicians/trainees without sacrificing the
   precise definition that guards against new misconceptions?
8. **MCP/engine generalization timing** — build the reusable engine + MCP server now (more upfront
   cost, bigger downstream payoff) or after the oncology set proves adoption?

---

### Key sources
- BMJ Statistics Notes (Altman/Bland): https://www-users.york.ac.uk/~mb55/pubs/pbstnote.htm ·
  https://www.medcalc.org/en/manual/statistical-literature-BMJ-notes.php ·
  https://pmc.ncbi.nlm.nih.gov/articles/PMC403858/
- Statistics at Square One: https://pmc.ncbi.nlm.nih.gov/articles/PMC1742433/
- JTO "Biostatistics Primer" series (HR + erratum, subgroups):
  https://www.jto.org/article/S1556-0864(15)32030-X/fulltext ·
  https://www.jto.org/article/S1556-0864(15)31084-4/fulltext ·
  https://www.jto.org/article/S1556-0864(15)32152-3/fulltext
- Nature Methods Points of Significance (survival/censoring):
  https://www.nature.com/collections/qghhqm/pointsofsignificance ·
  https://www.nature.com/articles/s41592-022-01563-7
- Students 4 Best Evidence: https://s4be.cochrane.org/blog/2016/04/05/tutorial-hazard-ratios/
- CEBM Oxford: https://www.cebm.ox.ac.uk/resources/data-extraction-tips-meta-analysis/estimating-hazard-ratio-kaplan-curve-numbers-at-risk
- ASCO eLearning / trainee resources:
  https://connection.asco.org/do/new-updated-asco-elearning-courses-put-oncology-education-your-fingertips ·
  https://www.asco.org/career-development/trainee-fellow-resources
- PH violations in phase-3 oncology trials: https://pmc.ncbi.nlm.nih.gov/articles/PMC11479825/ ·
  https://academic.oup.com/jncics/article/6/1/pkac007/6522126
- Greenland et al. 2016, p-value/CI misinterpretation guide: https://pmc.ncbi.nlm.nih.gov/articles/PMC4877414/
- HR vs median ratio confusion: https://litfl.com/hazard-ratio-median-ratio-and-kaplan-meier-curves/ ·
  https://pmc.ncbi.nlm.nih.gov/articles/PMC478551/
- Surrogate endpoints (trial- vs patient-level): https://ascopubs.org/doi/10.1200/JCO-24-01360
- Survival-analysis primer for clinician-scientists: https://pmc.ncbi.nlm.nih.gov/articles/PMC8743691/ ·
  StatPearls: https://www.ncbi.nlm.nih.gov/books/NBK560604/
