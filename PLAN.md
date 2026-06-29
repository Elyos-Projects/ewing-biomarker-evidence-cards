# Ewing-Biomarker-Evidence-Cards — PLAN.md

> Status: Draft · Version: 0.2.0 · Last updated: 2026-06-29 · Owner: TBD (maintainer) · Lane: donated

An open, source-cited library of **biomarker evidence cards** for Ewing Sarcoma — structured,
provenance-backed summaries of what the published, openly-available research says about each
biomarker (the EWSR1-FLI1 fusion and its variants, diagnostic immunomarkers, prognostic and
candidate-predictive alterations, and circulating biomarkers). The cards are built **for
researchers, data scientists, and informed patient advocates** as a research-evidence reference,
with an optional, separately-gated plain-language layer for **families** that is education only,
never medical advice. Every assertion carries a citation and provenance; the library uses **only
open-access, aggregate, or de-identified data**; and the patient-facing layer ships only after
oncologist **and** patient-advocate review.

This is a project about a cancer that mostly strikes children, teenagers, and young adults, and the
people who will read these cards may be parents in the worst week of their lives. The plan treats
that as a design constraint, not a footnote: it optimizes for **honest, sourced, uncertainty-aware**
information that neither over-promises a cure nor closes a door, and it refuses anything that looks
like individualized medical advice.

---

## Executive summary

Ewing Sarcoma is a rare, aggressive bone-and-soft-tissue cancer with a peak incidence in
adolescents and young adults. Its defining molecular feature — a fusion of *EWSR1* with an ETS-family
transcription factor, most often *FLI1* — has made it one of the most molecularly studied rare
cancers, yet the published evidence is scattered across thousands of papers and a dozen databases
with incompatible terminology, uneven quality, and very different reuse licenses. Researchers
re-derive the same summaries repeatedly; advocates and families struggle to find a trustworthy,
plain account of what a given biomarker actually means and how strong the evidence behind it is.

This project builds a **curated, openly-licensed set of evidence cards**, one per biomarker, each
stating: what the biomarker is, its role in Ewing biology, its diagnostic / prognostic / candidate-
predictive associations, a **two-axis evidence grade** (an *evidence-strength* level using the
framework appropriate to the association type, **plus** an explicit *validity dimension* — analytic
validity vs. clinical validity vs. clinical utility), an explicit **evidence status** (emerging /
established / contested / **refuted-superseded**) so a marker that was reported then overturned is
visible rather than silently propagated, and a **complete citation + provenance trail** for every
assertion. The cards are machine-readable (a validated JSON schema) and human-readable (a static,
accessible site), released under open licenses so the commons can build on them.

This combination — **validity-tiered, refuted-aware, provenance-bearing open biomarker cards for a
rare cancer** — is the project's differentiator: no existing knowledgebase (see *Competitive landscape
& differentiation*) grades *prognostic/diagnostic* Ewing markers with the right frameworks, cleanly
represents *refuted* markers, or ships a license-clean, AI/ML-reusable Ewing-specific set.

The single most important design fact is the **data-and-licensing discipline**, and it is the first
build item, not a compliance afterthought. Two hard, binding rules govern everything:

1. **Open / aggregate / de-identified data only.** Controlled-access resources (dbGaP, EGA,
   individual-level biobanks) and **any** identifiable patient data are **out of scope** — they need
   authorized access and IRB oversight this project does not have and does not seek. The library is
   built from open-access literature and aggregate/summary-level open databases only.
2. **Per-source license verification before any reuse.** Reuse terms differ sharply by source
   (e.g., **COSMIC and OncoKB are non-commercial / redistribution-restricted**; **TCGA/TARGET open
   tiers, GEO, ClinVar, CIViC, Open Targets, DepMap** are open or CC0 but with different attribution
   duties). No source's content is reused until its license is verified and recorded, and
   restrictively-licensed sources are **cited, not redistributed**.

This is a **MEDIUM** risk-tier project overall (research-facing biomarker science needs domain
accuracy and careful evidence framing). The **patient/family-facing plain-language layer is HIGH
risk** and is gated behind credentialed **oncologist + patient-advocate** sign-off and a persistent
"**this is research information, not medical advice**" frame. Research cards carry a "**research
reference, not clinical guidance**" frame and are reviewed for accuracy by a qualified domain
reviewer (molecular pathologist / sarcoma researcher).

Honesty note: **no partner organization, requestor, or steward is yet secured.** Every
delivery-dependent task is marked `TO BE SECURED` with `verifiedNeed: false`. The project is not
"shipped" on merge; it is shipped only when the cards are adopted and used by a real beneficiary
(e.g., a sarcoma research group, a pediatric-oncology advocacy organization, or an informed
patient-advocate community) with the licensing, provenance, and review gates independently verified.

**Partner-acquisition plan (dated) and build-vs-mothball decision rule.** Outreach is time-boxed
against the build rather than left an open-ended `TBD`: by **2026-08-31**, ≥ 3 candidate partners
(a sarcoma research consortium, a pediatric/sarcoma advocacy nonprofit, or an academic Ewing lab)
are in active conversation and a **domain reviewer (molecular pathologist / sarcoma researcher)** is
secured for the research cards (M2 gate); by **2026-10-31**, a **credentialed oncologist and a
patient-advocate reviewer** are secured for the patient-facing layer (M4 gate); by **2026-12-31**, a
**steward / adopting partner** is secured (M5 gate). **Decision rule:** if no domain reviewer is
secured by the M2 entry date, M2 evidence-card authoring does not start and the project holds at M1
(pipeline + schema). If no oncologist + advocate reviewers are secured, the patient-facing layer
(M4) **does not ship** — the research-only library still stands as a deed. If no adopting
partner/steward is secured by **2027-03-31**, the project does not "ship to no one": it either (a)
**pivots** the last-mile beneficiary (donate the research card set + schema to an open cancer-data
commons such as a CIViC-style curation community as a reusable reference) or (b) **mothballs** to
maintenance-only, with the decision recorded in governance. The research card library remains a
valuable reference artifact in either case.

---

## Problem & beneficiaries

**Who is helped (directly):**
- **Researchers and computational biologists** working on Ewing Sarcoma — especially small labs,
  trainees, and groups in under-resourced settings — who need a fast, trustworthy, source-cited map
  of the biomarker evidence without re-reading the whole literature.
- **Informed patient advocates and advocacy organizations** who translate the science for families
  and need a vetted, neutral, well-sourced foundation.

**Who is helped (ultimately):**
- **Patients and families facing Ewing Sarcoma** — through the optional, expert-reviewed
  plain-language layer and through better-informed advocates and researchers. The benefit to
  families is real but **indirect and mediated by experts**; this project never positions itself as
  a source of individual medical guidance.

**The need.** Ewing Sarcoma is rare (roughly 1 case per million per year), which has two
consequences: the evidence base, while molecularly rich, is thin per-question and easy to
misread; and the *attention* base is thin — there are few well-maintained, openly-licensed,
plain-language, source-cited references compared with common cancers. A parent who reads "STAG2
mutation" or "EWSR1-FLI1 fusion type" in a report, or an advocate trying to summarize "what does the
evidence say about ctDNA monitoring in Ewing," faces fragmented primary literature, paywalls,
inconsistent gene/variant naming, database licenses they cannot evaluate, and a real risk of
encountering hype, outdated claims, or predatory misinformation. Authoritative information exists
but is **scattered, jargon-dense, unevenly licensed, and not graded by evidence strength**.

**Verified need / partner:** **TO BE SECURED.** No specific research group, advocacy organization,
or patient-advocate community has yet agreed to adopt or co-develop the cards, and no credentialed
oncologist or patient-advocate reviewer is yet engaged. Target partner profiles to pursue: a sarcoma
or pediatric-oncology **research consortium**; a **sarcoma/childhood-cancer advocacy nonprofit** with
a science-communication mission; an **academic Ewing laboratory** willing to provide domain review;
an open cancer-knowledge curation community (e.g., a CIViC-style effort) as a downstream home. Until
a partner is secured, the project builds the schema, the license-verified sourcing pipeline, and an
initial set of **research-only** cards for review, and marks all delivery/adoption and all
patient-facing work `TO BE SECURED`. Outreach is **dated, not open-ended** (see the partner-
acquisition plan in the executive summary), and a **build-vs-mothball/pivot decision rule** governs
what happens if those dates slip — rather than shipping a patient-facing cancer resource to no
verified beneficiary and with no expert sign-off.

---

## Goals and non-goals

**Goals**

- Produce an **open, source-cited evidence-card library** for Ewing Sarcoma biomarkers, each card
  carrying an explicit evidence level and a complete provenance trail for every assertion.
- Enforce, as a hard build requirement, the **data guardrails**: open/aggregate/de-identified
  sources only; per-source license verification; no controlled-access or identifiable patient data.
- Make every assertion **traceable** to a primary or aggregate open source (PMID/PMCID, database +
  accession, retrieval date, license) — "no source, no claim."
- Grade evidence with the **published framework that fits the association type**, on **two axes**: an
  *evidence-strength* axis — **Simon–Hayes / TMUGS Levels of Evidence (I–V)** for prognostic markers,
  **AMP/ASCO/CAP (Tier I–IV)** and **ESMO ESCAT (Tier I–X)** for the *predictive/actionability* context
  where any exists, with **REMARK** used as a per-study reporting-quality appraisal checklist (not an
  evidence grade) — and a *validity-type* axis (**analytic validity / clinical validity / clinical
  utility**, per ACCE/EGAPP). Strength and validity-type are transparent and not the curator's opinion.
  (CIViC evidence levels are used where its open content already grades a variant, cross-walked.)
- Record an explicit **evidence status** (`emerging | established | contested | refuted-superseded`)
  with a `supersededBy[]` citation list, so markers overturned by later/prospective data (e.g.,
  EWSR1-FLI1 **fusion type** as a prognostic marker) are first-class and never silently propagated.
- Ship a machine-readable dataset (validated JSON) **and** an accessible human-readable site, both
  openly licensed, so researchers and advocates can reuse them.
- Provide an optional, **expert-reviewed (oncologist + patient-advocate) plain-language layer** for
  families that is honest about uncertainty, framed as education, and explicitly **not medical
  advice**.
- Keep the library **alive**: a review cadence and a runtime staleness fail-safe so cards do not
  silently rot as the science moves.

**Non-goals**

- **Not medical advice, diagnosis, prognosis, or treatment recommendation** for any individual — and
  never a substitute for a patient's oncology team.
- **Not** a system that uses controlled-access (dbGaP, EGA, biobank) or **any** identifiable
  patient-level data; **not** a re-identification or individual-data project of any kind.
- **Not** a redistribution of restrictively-licensed databases (e.g., bulk COSMIC/OncoKB content) —
  those are **cited and linked**, never copied into the open commons.
- **Not** a clinical decision-support tool, a trial-matching service, or a drug-efficacy claim
  generator; cards summarize *evidence and its strength*, not "what works."
- **Not** a comprehensive textbook of Ewing biology — depth and verified accuracy on a curated,
  high-value biomarker set first; breadth later, always behind review.
- **Not** a hype or fundraising vehicle; no efficacy promises, no "breakthrough" framing, no
  for-profit primary benefit.

---

## Success metrics (outcomes)

Outcome-centric and beneficiary-first. Card counts and site traffic are **outputs, not success**;
the measures that matter are accuracy, provenance integrity, license safety, and real reuse by
beneficiaries.

| Metric | Baseline | Target | How measured |
|---|---|---|---|
| Assertions with a complete citation + provenance trail | n/a | **100%** of shipped assertions (no claim without a source) | Citation-coverage test in CI + reviewer checklist |
| Sources with verified, recorded reuse license before use | n/a | **100%**; **0** assertions sourced from an unverified or non-compliant source | License-register audit + ingestion gate test |
| Controlled-access / identifiable-data leakage | n/a | **0** instances (hard gate) | Source-allowlist enforcement + provenance audit |
| Evidence level assigned to every clinical-association assertion | n/a | **100%**, using a published framework with the framework named | Schema validation (required field) |
| Domain-reviewer sign-off before a research card ships | none | **100%** of research cards | Reviewers ledger / governance log |
| Oncologist **and** advocate sign-off before any patient-facing card ships | none | **100%** of patient-facing cards (both required) | Reviewers ledger (dual sign-off) |
| Extraction accuracy vs. source (curation QA) | n/a | **≥ 95%** of audited assertions faithfully represent the cited source (no overstatement) | Independent reviewer audit on a random sample each release |
| Prognostic/predictive assertions corroborated by a prospective / cooperative-group study | n/a | **100%** either corroborated **or explicitly flagged as not** (faithfulness to a *weak* source is not strength) | Schema field + reviewer audit |
| Contradiction coverage (markers with known conflicting evidence carrying a `contested`/`refuted-superseded` representation) | n/a | **100%** of known-conflicted markers (e.g., fusion type, TP53) | Schema audit + reviewer checklist |
| Grading reproducibility (second-reviewer concordance on evidence-level/validity-type) | n/a | Periodic **second-reviewer concordance sample**; divergences reconciled and recorded | Sampled dual-grading audit (guards known AMP/ASCO/CAP inter-rater variability) |
| Stale-card containment | n/a | **0** cards served as current past their `validUntil` without an auto-flag/withhold + re-review | Staleness test against `lastVerified`/`validUntil`; runtime audit |
| "Not medical advice" / "research reference" framing present | n/a | **100%** of cards and pages (research + patient layers) | Template lint + review checklist |
| Adoption by a real beneficiary (the deed) | 0 | **≥ 1** research group / advocacy org / advocate community adopts and reuses the cards, tracked as a **staged outcome** (reviewer-validated card set → cited in a review/guideline → embedded in an advocacy resource) | Steward-confirmed adoption record |
| Documented reuse / downstream benefit | 0 | **≥ 3** concrete instances (cited in a review, embedded in an advocacy resource, reused in a dataset) | Steward-tracked reuse log |

The **defining success outcome** (Definition of Shipped): a real beneficiary adopts and uses the
cards — and, where the patient-facing layer ships, families receive honest, sourced, expert-reviewed
information — with licensing, provenance, evidence-grading, and review gates all independently
verified.

---

## Scope

**In scope**

- An **evidence-card schema** (machine-readable JSON Schema) covering biomarker identity, biology,
  clinical associations, **two-axis evidence grade** (evidence-strength level + `validityDimension`),
  **`evidenceStatus` + `supersededBy[]`**, citations, per-source provenance + license, review status,
  risk tier, validity dates, and disclaimers.
- A **license-verified sourcing pipeline**: ingest only from an allowlist of open/aggregate sources;
  record provenance and license per source; enforce "no source, no claim."
- **Research evidence cards** for a curated, high-value Ewing biomarker set (see *Solution approach*),
  each cited, graded, and domain-reviewed.
- An **evidence-grading mapping** onto the framework that fits each association type (Simon–Hayes LOE
  for prognostic; AMP/ASCO/CAP + ESCAT for predictive/actionability; REMARK as a study-appraisal
  checklist; ACCE validity-type axis; CIViC levels where applicable), with the framework name + version
  recorded **per association type** on each assertion.
- A **machine-readable dataset** (CC-BY-4.0 / CC0 as licensing allows) and an **accessible static
  site** (WCAG 2.2 AA) presenting the cards.
- An optional, **separately-gated patient/family plain-language layer** — education only, "not
  medical advice," oncologist + advocate reviewed.
- A **runtime staleness fail-safe** + a documented **review cadence**, and a **curation QA / audit**
  process for extraction accuracy.

**Out of scope (explicitly will NOT do)**

- **Controlled-access data** (dbGaP, EGA), **individual-level biobank data**, and **any identifiable
  patient data** — needs authorized access + IRB; not pursued.
- **Re-identification, individual patient data linkage, or any individual-level analysis.**
- **Bulk redistribution** of restrictively-licensed databases (COSMIC, OncoKB, or any source whose
  license forbids redistribution) — cited and linked only.
- **Medical advice, individual diagnosis/prognosis/treatment recommendations**, clinical
  decision-support, dosing, or "this therapy will work" claims.
- **Trial matching / eligibility determination** (a sibling project, `ewing-trial-finder`, owns
  that, and it is HIGH risk).
- **Primary efficacy/outcome claims** beyond faithfully reporting, with evidence level and
  uncertainty, what a cited open source states.
- **For-profit primary benefit**, fundraising copy, or sponsored/biased content of any kind.

When a request or contribution would cross a line (e.g., "add an individual patient's variant
report," "summarize this dbGaP cohort," "tell families which drug to ask for"), the project refuses
that part, records why, and where possible points to the correct lawful/ethical channel (the
patient's oncology team; an authorized-access process; a sibling project).

---

## Solution approach & architecture

This is primarily a **content + data curation** project with supporting tooling. The pipeline,
schema, and review gates are the architecture.

### Pipeline (source → card)

1. **Source allowlist + license register (built first).** A machine-readable register of approved
   sources, each with: name, type, access tier (**open/aggregate/de-identified only**), license,
   redistribution-allowed flag, attribution requirement, and a verification record. The ingestion
   tooling **cannot pull from a source not on the allowlist**, and `redistribution-allowed: false`
   sources may be cited but never copied into card bodies or the released dataset.
2. **Ingestion / retrieval.** Pull candidate evidence from approved sources: the **PMC Open Access
   subset** (per-article CC license, recorded per article), **CIViC** (CC0), **Open Targets**
   (open), **ClinVar / dbSNP** (US-gov public domain), **DepMap** (CC-BY-4.0), aggregate/open tiers
   of **GEO / cBioPortal / TARGET-open / St. Jude PeCan aggregate views**, and HGNC/Sequence
   Ontology/Gene Ontology for normalization. COSMIC and OncoKB are **reference-and-cite only**
   (non-commercial / redistribution-restricted) — and OncoKB's text must **not** be fed to the
   extraction model even when cited (its license forbids AI/ML training; see *Data, licensing &
   compliance*). The **VICC meta-knowledgebase** (GA4GH, standards-based) is used as a **cross-KB
   consensus check** — for each marker, reconcile the OncoKB/CIViC/CKB slices and record
   agreement/conflict as provenance, rather than treating any single KB as ground truth.
3. **Extraction (LLM-assisted, human-verified).** Draft candidate assertions (claim, association
   type, direction, population/context, source) from open-access text and structured records.
   Extraction is **assistive**: a domain reviewer verifies every assertion against its source before
   it can ship. The LLM never invents citations; an assertion with no resolvable source is dropped.
4. **Normalization.** Map genes to **HGNC** symbols + IDs, variants to **HGVS / Sequence Ontology**
   terms, diseases to an ontology (e.g., **Mondo / NCIt**; optionally **OncoTree** for tumor-subtype
   coding), and variant representation to **GA4GH VRS** where applicable, so cards are interoperable and
   de-duplicated. Because fusion *architecture* is core to Ewing and HGVS is awkward for fusions, add a
   **structured fusion-representation model** (e.g., `EWSR1::FLI1` 5′/3′ partners + breakpoint/exon
   structure) alongside the HGVS/SO terms.
5. **Evidence grading (two-axis, framework-by-association-type).** Grade each clinical-association
   assertion on **two axes**, with the framework name + version stored on the assertion and grading
   reviewer-confirmed:
   - *Evidence-strength axis* — use the framework that **fits the association type**: **Simon–Hayes /
     TMUGS Levels of Evidence (I–V)** for **prognostic** markers (the prospective–retrospective design
     is the route to Level I); **AMP/ASCO/CAP** somatic tiers and **ESMO ESCAT** actionability **only**
     for the **predictive/therapeutic** context where any exists; **CIViC** levels (A validated → E
     inferential) where its open content already grades the variant, cross-walked. **Do not grade a
     prognostic or diagnostic marker on an actionability scale** — ESCAT in particular is near-useless
     for Ewing (almost every marker is ESCAT Tier X because EWSR1-FLI1 has no approved targeted
     therapy), and grading prognosis on an actionability tier is a category error.
   - *Validity-type axis* — tag the **`validityDimension`** the cited evidence actually answers:
     **analytic validity** (the assay measures the marker reliably) vs. **clinical validity** (it
     correlates with outcome) vs. **clinical utility** (acting on it improves an outcome), per
     **ACCE/EGAPP**. A marker can have strong analytic and clinical validity yet **zero demonstrated
     clinical utility** — the most common biomarker-appraisal error and exactly the trap the
     family-facing layer must avoid.
   - *Study appraisal* — **REMARK** (20-item checklist) is run **per cited prognostic study** as a
     reporting-quality appraisal note, **not** as an evidence grade.
   - *Evidence status* — set **`evidenceStatus`** (`emerging | established | contested |
     refuted-superseded`) with a **`supersededBy[]`** citation list, so a marker reported then
     overturned (the flagship example: EWSR1-FLI1 **fusion type**, overturned by van Doorninck 2010
     (COG) and Le Deley 2010 (Euro-E.W.I.N.G. 99)) is explicit and routed to a "do not use as
     prognostic" representation. Status is **reviewer-ratified**, never auto-published.
6. **Card assembly + validation.** Compose the card; validate against the JSON Schema (ajv) in CI;
   run the citation-coverage, license, allowlist, framing, and staleness tests.
7. **Review.** Domain reviewer (research cards) and, for the patient layer, oncologist + advocate
   sign-off — recorded, version-scoped, in a reviewers ledger.
8. **Publish.** Emit the dataset (open license) and render the accessible static site. Patient-facing
   cards render only after dual sign-off.

### Data model (the evidence card, abbreviated)

A card (machine-readable) carries roughly:

- **identity** — `biomarkerId`, `name`, `aliases[]`, normalized refs (HGNC, SO, HGVS, Mondo/NCIt,
  GA4GH VRS where applicable; structured fusion model for fusions),
  `biomarkerType` (`fusion | snv | indel | cnv | expression | protein-ihc | circulating | other`).
- **biology** — a sourced summary of role in Ewing biology.
- **clinicalAssociations[]** — each: `claim`, `associationType` (`diagnostic | prognostic |
  predictive | pharmacodynamic`), `direction` (e.g., favorable/adverse/associated), `populationContext`
  (e.g., localized vs. metastatic; pediatric/AYA; **and rare-cancer cohort caveats — small-n,
  single-institution vs. cooperative-group**), `evidenceLevel` (+ framework + version, framework chosen
  by association type), **`validityDimension`** (`analytic | clinical-validity | clinical-utility`),
  **`evidenceStatus`** (`emerging | established | contested | refuted-superseded`), **`supersededBy[]`**
  (citations that overturned/replaced the claim), **`maturity`** (`preclinical | translational |
  clinically-validated` — keeps preclinical/cell-line evidence from being read as validated),
  `citations[]` (PMID/PMCID/DOI), `provenance[]` (source, accession, retrievalDate, license,
  redistributionAllowed), `uncertaintyNote`.
- **governance** — `riskTier`, `reviewStatus`, `reviewers[]` (version-scoped), `lastVerified`,
  `validUntil`, `disclaimer` (research-reference and, for patient cards, not-medical-advice),
  `cardLicense`.

### Tooling & stack

TypeScript, ESM, pnpm workspaces (per Elyos conventions). JSON Schema + **ajv** validation. A static
site generator for the human-readable layer (accessible, no tracking). Anthropic Claude as the
**assistive** extraction/summarization model behind a thin, provider-neutral client (per the Claude
API skill for model selection; the agent-neutral core carries no vendor-specific logic — Elyos
core/adapter rule). CI runs build/test/lint plus the schema, citation-coverage, allowlist, license,
framing, and staleness checks.

### Claude (assistive) — span-grounded extraction, normalization & contradiction surfacing

Claude is used **assistively only**, behind the neutral client, with **every assertion human-verified**
and no clinical, grading, licensing, or refuted-status determination left to the model:

- **Span-grounded extraction with mandatory citations.** From PMC-OA full text, draft candidate
  assertions as structured tuples (`claim`, `associationType`, `direction`, `populationContext`, **plus
  a verbatim supporting quote + PMID/PMCID + location**). Requiring a verbatim span per assertion makes
  hallucinated citations detectable and directly backs the **no-source-no-claim** test — an assertion
  with no resolvable source + supporting span is **dropped, not surfaced**. Extraction runs **only under
  the per-article license check** (and never ingests OncoKB text as model input).
- **Schema structuring & normalization (draft).** Map free-text findings into the card schema; propose
  HGNC/SO/HGVS/Mondo/VRS candidate IDs and the structured fusion model; de-duplicate — all
  human-confirmed.
- **Contradiction & supersession surfacing (never resolution).** Cluster assertions per marker and
  **surface** conflicts (e.g., de Alava 1998 "Type 1 favorable" vs. Le Deley 2010 "not confirmed") as
  *candidate* `contested` / `refuted-superseded` flags with `supersededBy[]` — the model **flags; the
  reviewer decides**. Status is never auto-published.
- **REMARK appraisal & validity-dimension drafting (draft).** Draft a structured REMARK quality note
  and a *suggested* `validityDimension` for reviewer confirmation — speeds appraisal, never sets grades.
- **Plain-language drafting (M4, behind the dual gate).** Draft family renditions to the
  readability/tone standard; never published without oncologist + advocate sign-off.
- **Staleness triage.** Periodically scan new PMC-OA literature and **queue** cards for re-review; the
  model does not edit cards.

**Human gates Claude must never cross** (these are guardrails, not preferences): **no clinical
recommendations, individual prognosis, diagnosis, or treatment selection** in either layer; **no
final evidence grade or actionability tier** (the model may *propose* a level with rationale, but the
shipped grade is the domain reviewer's — AMP/ASCO/CAP tiering is inconsistent even between expert
labs); **no fabricated/inferred citations**; **refuted/contested status, license determinations, and
any clinical-safety judgment are human-ratified**. Model-selection specifics live in the Claude API
skill; the plan needs only the neutral-client boundary and these human-gate rules.

### Initial biomarker set (curated, high-value — illustrative, reviewer-confirmed)

Diagnostic/defining: **EWSR1-FLI1** fusion and **EWSR1-ETS variants** (EWSR1-ERG, -ETV1, -ETV4/E1AF,
-FEV); **CD99 (MIC2)** and **NKX2-2** immunomarkers. Prognostic/biologic: **STAG2**, **TP53**,
**CDKN2A** alterations; characteristic **copy-number** changes (e.g., 1q gain, chr 8 gain, 16q loss).
Circulating/monitoring: **EWSR1-FLI1 ctDNA / fusion-transcript** detection. Candidate-predictive /
pathway context is reported **only as evidence with its level and uncertainty**, never as a treatment
recommendation. The final set is confirmed with the domain reviewer.

To stop a drafting agent mis-summarizing a marker, each card task is **seeded with a pre-written
"evidence-state stub"** (status + key citations) that the domain reviewer **corrects rather than
originates**. The binding accuracy guardrails for this set:

- **EWSR1-FLI1 fusion *type* is a refuted prognostic marker.** Early retrospective series (de Alava
  1998; Zoubek 1996) reported Type 1 = better prognosis; this was **overturned in the
  intensive-chemotherapy era** by prospective cooperative-group data (van Doorninck 2010, COG;
  Le Deley 2010, Euro-E.W.I.N.G. 99, n=565). The card carries `evidenceStatus: refuted-superseded`
  with `supersededBy[]` and a "do not use as prognostic" representation. (The fusion as a **diagnostic**
  marker remains valid — the refutation is specific to the *type-as-prognosis* claim.)
- **TP53 and CDKN2A *alone* are NOT reliable prognostic markers** (Lerman 2015, COG). The adverse
  signal is for the **concurrent STAG2 + TP53** subset (Tirode 2014); cards must not present TP53/CDKN2A
  as flat "prognostic" markers.
- **Copy-number: 1q gain is the validated one** (COG, JCO 2025); **chromosome 8 gain is the most
  frequent CNA (~50%) but has no consistent prognostic value** (a passenger) — they are not comparable
  and must not be listed as if equivalent.
- **CD99 is sensitive but NOT specific**; molecular confirmation (EWSR1 FISH / RT-PCR) is the gold
  standard, and **EWSR1 FISH is not partner-specific** (positive in DSRCT, clear cell sarcoma, myxoid
  liposarcoma, etc.). The diagnostic cards must carry the **round-cell-sarcoma differential** —
  especially **CIC-rearranged** and **BCOR-rearranged** sarcomas (now distinct EWSR1-negative entities)
  — since a family-facing miss here is dangerous.
- **Maturity/preclinical distinction:** cell-line/DepMap and preclinical findings are tagged
  `maturity: preclinical` and never rendered as clinically validated.

### Key decisions

- **Compliance is the first build item** (the source allowlist + license register), and is a release
  gate — not documentation.
- **No source, no claim**, enforced by a test; restrictively-licensed sources are cited, never
  copied.
- **Evidence is graded on two axes** (evidence-strength **by association type** — Simon–Hayes LOE for
  prognostic, AMP/ASCO/CAP + ESCAT for predictive, REMARK as appraisal — plus a `validityDimension`
  analytic/clinical/utility axis), recorded per assertion — transparency over curator opinion, and no
  category errors (no prognostic marker graded on an actionability scale).
- **Refuted markers are first-class.** `evidenceStatus` + `supersededBy[]` make an overturned marker
  (fusion type) explicit rather than silently propagated — the project's strongest differentiator.
- **Patient-facing layer is a separate, dual-gated track**; the research library stands on its own
  even if the patient layer never ships.
- **Agent-neutral core**; Claude is assistive only and sits behind the neutral client; humans verify
  every assertion.
- **Operationalized sibling boundaries (not duplicative).** This project owns **prognostic / diagnostic
  / circulating** Ewing-marker evidence; **`ewing-drug-target-evidence`** owns **therapeutic-target**
  evidence (where ESCAT / AMP-ASCO-CAP actually fit). They **share** the source pipeline, the
  evidence-card schema/grading library (a common **`evidence-card-core`**), and the provenance engine,
  but draw a clean association-type boundary. **`biomarker-extraction`** is the shared, provenance-first
  verbatim-span extraction + contradiction-detection toolchain that **feeds curated candidate assertions
  in** to this project (and to siblings) — extraction is upstream tooling; curation/grading/review live
  here. See *Adjacent opportunities*.

---

## Competitive landscape & differentiation

No existing resource is a **Ewing-specific, open, provenance-first, validity-tiered, refuted-aware**
biomarker reference. The field splits into somatic-variant *actionability* knowledgebases, genomic
archives/aggregators, and *methodology* frameworks (the last are **standards to adopt, not
competitors**). The structural reason the opening exists: every actionability KB is thin on Ewing **by
biology** — EWSR1-FLI1 has **no approved targeted therapy**, so actionability tiers are near-empty —
and none cleanly represents **prognostic-marker evidence, refuted markers, or validity-type**.

| Resource | What it is | License / reuse | Gap for Ewing |
|---|---|---|---|
| **OncoKB** (MSK) | Somatic actionability KB; only FDA-partially-recognized oncology variant DB; has an EWSR1-FLI1/Ewing entry | **Restrictive** (free academic, paid commercial/clinical), API-gated, **no bulk download**, **explicit no-AI/ML-training** | Dx/Px levels are **heme-only**; Ewing actionability ~zero (no Tx Level 1/2); not reusable/trainable |
| **CIViC** (WashU) | Open, expert-crowdsourced evidence items (Levels A–E + trust stars); SEPIO provenance | **Fully open — content CC0, code MIT**, public API, AI/ML-trainable | Crowdsourced → **sparse/stale on rare entities**; not sarcoma-focused; smaller corpus |
| **My Cancer Genome** | Disease→biomarker→therapy educational KB; clinician/patient-friendly | No open license | **Effectively abandoned** (stale content, broken TLS) — a maintained, staleness-gated resource fills this gap |
| **JAX-CKB / CKB CORE** | Relational precision-oncology KB; strong on complex variants | **Freemium** — free CORE covers only ~50 rotating genes; full set paywalled | EWSR1 availability depends on rotation |
| **COSMIC** (Sanger) | Largest somatic-mutation/fusion/CNV catalogue; covers EWSR1 fusions | **Non-commercial; registration required; paid commercial** | **Cite-only** (already marked); recurrence reference, not graded evidence |
| **ClinVar** (NCBI) | Variant clinical-significance archive; 0–4★ review status | **Public domain** (most permissive) | **Predominantly germline**; somatic/Ewing coverage thin |
| **cBioPortal** (MSK/DFCI) | Open genomics visualizer; aggregates external annotations | AGPL/open | An **aggregator/viewer, not an evidence curator**; TCGA-SARC light on Ewing |
| **VICC metakb** | Harmonizes 6 somatic KBs (GA4GH/VRS) into one searchable aggregate | Standards-based; inherits source terms | Aggregates not originates; inherits sparsity; **not prognostic-oriented** — we use it as a cross-KB consensus check |
| **PharmGKB** (ClinPGx) | Germline pharmacogenomics; Levels 1A–4; drives CPIC | **CC-BY-SA** (copyleft) | **Out of scope** (germline PGx, not somatic tumor biomarkers); note SA contagion if ever used |

**Methodology frameworks adopted (not competitors):** **REMARK** (prognostic-marker reporting
checklist), **ACCE/EGAPP** (analytic/clinical-validity/utility backbone), **TMUGS / Simon–Hayes LOE**
(tumor-marker utility grading), **AMP/ASCO/CAP 2017** (somatic-variant significance tiers), **ESMO
ESCAT** (target actionability), **GRADE** (intervention certainty; adapted for prognosis).

**Differentiators to win:**

1. **Validity-tiered + temporally-honest grading.** Two-axis (evidence-strength × validity-type) plus
   `evidenceStatus` (emerging/established/contested/refuted-superseded) — **no competitor does this for
   prognostic markers.** The single strongest differentiator.
2. **Refuted-biomarker memory.** Structurally representing overturned claims (fusion type as prognosis,
   TP53-alone) prevents the most common real-world harm: acting on dead evidence.
3. **Provenance-per-assertion, license-clean, AI/ML-reusable.** Unlike OncoKB (restrictive, no-train)
   and COSMIC (paid commercial), the output is **CC-BY/CC0 with a machine-checked `cardLicense`**.
4. **Disease-deep, reviewer-signed, not pan-cancer-shallow.** Domain-reviewer sign-off per card +
   curation-QA faithfulness audit ≥95% gives a trust profile aggregators can't match.
5. **Runtime staleness fail-safe.** Cards past `validUntil` auto-flag/withhold — a live answer to the
   My Cancer Genome failure mode.
6. **Honest "no validated predictive biomarker in Ewing / no medical advice" framing as a feature**,
   dual-gated for the family layer — trustworthy where hype resources are not.

The validity chain is also a **headline finding, not a footnote**: the library states plainly that
**Ewing has no validated *predictive* (treatment-selection) biomarker**, so candidate-predictive
context never implies actionable weight.

---

## Data, licensing & compliance

**THIS IS THE CRITICAL SECTION. The cancer guardrails below are binding and lead the design.**

### Binding data guardrails

1. **Open / aggregate / de-identified data only.** The project uses **only** open-access literature
   and aggregate/summary-level open databases. **Controlled-access resources — dbGaP, EGA,
   individual-level biobanks — and ANY identifiable patient data are OUT OF SCOPE.** They require
   authorized access and IRB oversight that this donated, open project does not have and will not
   seek. There is no individual-level data, no re-identification, and no patient-record ingestion of
   any kind.
2. **Per-source license verification before any reuse.** Every source is verified and recorded in
   the **license register** before its content is used, with a `redistributionAllowed` flag.
   Restrictively-licensed sources are **cited and linked, never redistributed.** The register also
   tracks two constraints beyond redistribution: (a) an **`aiTrainingAllowed` flag** — **OncoKB's
   license explicitly forbids feeding its content into AI/ML training**, so OncoKB text must **never**
   be used as input to the extraction model even when cited; and (b) a **`shareAlike` flag** — a
   **CC-BY-SA / copyleft** source (e.g., PharmGKB, were it ever in scope) would force its entire card to
   ShareAlike, so SA is modeled as a **distinct contaminant** in the license-composition gate, alongside
   NC. (PharmGKB is germline pharmacogenomics, **out of scope** for somatic Ewing biomarkers; if a
   reviewer proposes it, the exclusion rationale is recorded.)

### Source register (initial; each verified + dated before use)

| Source | Type | Access | License / terms | Reuse stance |
|---|---|---|---|---|
| **PMC Open Access subset** | Literature | Open | Per-article (CC-BY, CC-BY-NC, CC0) — recorded per article | Quote/summarize per the article's CC terms; NC articles → cite, limited reuse |
| **CIViC** | Variant–evidence | Open | **CC0** | Reuse freely (attribution as courtesy) |
| **Open Targets** | Target–disease evidence | Open | Open (mostly CC0; component-specific) | Reuse per component terms |
| **ClinVar / dbSNP** | Variants | Open | US-gov **public domain** | Reuse freely |
| **DepMap** | Cell-line dependencies | Open/aggregate | **CC-BY-4.0** | Reuse with attribution |
| **GEO / cBioPortal (open studies) / TARGET open tier / St. Jude PeCan (aggregate views)** | Expression / aggregate genomics | Open/aggregate | Per-study; **open/aggregate only** | Aggregate-level only; verify per study; no controlled tiers |
| **HGNC / Sequence Ontology / Gene Ontology / Mondo / NCIt / OncoTree** | Ontologies | Open | Open (CC-BY/CC0-class) | Normalization; reuse with attribution |
| **VICC meta-knowledgebase (metakb)** | Cross-KB harmonizer (GA4GH/VRS) | Open/aggregate | Standards-based; inherits each source KB's terms | **Cross-KB consensus check only**; record agreement/conflict as provenance; respect each upstream KB's reuse terms |
| **COSMIC** | Mutations | Aggregate | **Non-commercial; redistribution-restricted** | **Cite + link only — never redistributed** |
| **OncoKB** | Precision-oncology annotations | Aggregate | **Non-commercial; registration/redistribution-restricted; explicit no-AI/ML-training clause** | **Cite + link only — never redistributed; text must never be fed to the extraction model** |

> dbGaP, EGA, and individual-level biobanks are intentionally **absent** from this register — they
> are out of scope by guardrail, not by omission.

### Provenance model

Every **assertion** is backed by one or more `provenance` records: source name, accession/identifier
(PMID/PMCID/DOI or DB accession), retrieval date, license, and `redistributionAllowed`. A card may
not surface an assertion without at least one resolvable citation **and** a verified source; a
**citation-coverage test** and an **allowlist/license test** enforce this in CI. Faithfulness to the
source (no overstatement beyond what the source says) is checked by a **curation QA audit** each
release (sampled, independent reviewer).

### Staleness is fail-safe, not best-effort

Each assertion carries `lastVerified` and a `validUntil` derived from a per-association-type
**review interval** (e.g., fast-moving predictive/therapeutic associations re-verified on a shorter
cadence than stable diagnostic biology). At **render time**, an assertion past its `validUntil` is
**not served as current**: the card **auto-flags** it ("this summary is past its review window —
verify against current literature") or **withholds** it for the patient-facing layer, and routes it
to re-review. It returns to normal service only after re-verification and re-sign-off, which writes a
fresh `lastVerified`/`validUntil`. The common failure mode — the science moved and a card silently
went stale — becomes a visible, gated event.

### Output licensing

- **Code/tooling:** **MIT** (Elyos default).
- **Card content / dataset:** **CC-BY-4.0** for original curated text and structure; assertions
  derived from **CC0** sources may be released **CC0**; assertions touching **CC-BY-NC** source text
  inherit the **NC** constraint and are marked accordingly; assertions touching a **CC-BY-SA /
  ShareAlike** source would force the card to **SA** — the composition gate models **SA as a distinct
  contaminant** and blocks silent relaxation to CC-BY. **No card body contains redistribution-
  restricted (COSMIC/OncoKB) content** — those are references only, and **OncoKB text is additionally
  barred from AI/ML training input.** A per-card `cardLicense` field records the effective (most
  restrictive) license; a license-composition check prevents shipping a card whose body violates any
  contributing source's terms.
- **Docs:** CC-BY-4.0.

### Privacy / PII stance (conservative)

There is **no patient PII in this project by construction** — it consumes only aggregate/open data
and stores only biomarker-level assertions and citations. No individual-level records, no
re-identification, no linkage. If any contributed source is later found to contain individual-level
or identifiable data, it is **rejected** and any derived content is removed. No secrets, tokens, or
keys in logs, receipts, or committed files (Elyos rule).

### Attribution

Cards cite primary literature and aggregate sources; redistribution preserves attribution per CC-BY.
Domain, oncologist, and advocate reviewers are credited (with consent, version-scoped) in a
reviewers ledger.

---

## Quality, review & risk gates

**Risk tier: MEDIUM overall; HIGH for the patient/family-facing layer.** Per
`docs/good-deed-definition.md`, medium needs domain-accurate review by a relevantly-skilled reviewer;
health content that reaches patients/families is HIGH and **requires credentialed expert sign-off
before merge**. Pure tooling/schema/site-infra tasks are low–medium; any card asserting biomarker
science is medium; any patient-facing card is HIGH.

**Required reviews before a deed is "done":**

- **Maintainer** review on all PRs (engineering quality, agent-neutral core, schema validity, tests/
  CI green, no secrets).
- **Domain reviewer sign-off** (molecular pathologist / sarcoma researcher / qualified
  bioinformatician) on **every research card** — accuracy, correct evidence grading, faithful
  representation of sources. **No domain review, no research card ships.**
- **Credentialed oncologist + patient-advocate dual sign-off** on **every patient-facing card** — for
  accuracy, tone, absence of false hope/despair, and the not-medical-advice frame. **Both required;
  no dual sign-off, no patient card ships.**
- **Compliance/licensing review** — the source allowlist + license register and the license-
  composition check pass; restrictively-licensed content is cited, not copied; no controlled-access
  or identifiable data.
- **Curation QA audit** — sampled extraction-faithfulness check each release.

**Every card** carries a persistent frame: research cards say "**research reference — not clinical
guidance**"; patient cards say "**this is research information, not medical advice; talk to your (or
your child's) oncology team**." Evidence levels and uncertainty are shown, never hidden.

**Definition of Shipped (project):** the cards are adopted and used by a real beneficiary (research
group / advocacy org / advocate community), with licensing, provenance, evidence-grading, and review
gates independently verified; where the patient-facing layer ships, it carries oncologist + advocate
sign-off and the not-medical-advice frame throughout.

---

## Roadmap & milestones

Phased: compliance + schema first; license-verified pipeline next; research cards (domain-reviewed)
before any patient-facing content; the patient layer is its own dual-gated phase; adoption last and
gated on a secured partner. Dependencies flow M0 → M1 → M2 → M3 → (M4) → M5 → M6.

- **M0 — Compliance, schema & cold-start.**
  *Goal:* the data/licensing guardrails and the card schema exist before any card.
  *Exit:* the **data/licensing & compliance policy spec** merged; the **source allowlist + license
  register** (initial sources verified + dated, incl. VICC-metakb consensus source; `aiTrainingAllowed`
  + `shareAlike` flags) merged; the **evidence-card JSON Schema** + ajv validation in CI, including
  **`validityDimension`**, **`evidenceStatus` + `supersededBy[]`**, and **`maturity`** fields; the
  **two-axis evidence-grading model** chosen and documented (Simon–Hayes LOE for prognostic; AMP/ASCO/CAP
  + ESCAT for predictive; REMARK appraisal; ACCE validity-type) plus a **"how we grade" methods card**
  drafted as the library's front page; repo + pnpm/TS/ESM + CI (build/test/lint +
  schema/allowlist/license/framing tests) green; "research reference, not clinical guidance / not
  medical advice" framing wired into templates. **Domain-reviewer recruitment started** (dated plan).

- **M1 — License-verified sourcing & provenance pipeline.**
  *Goal:* ingest only from approved sources with full provenance and enforced grading.
  *Exit:* ingestion connectors for the approved open sources working on fixtures (with **span-grounded,
  verbatim-quote** extraction so citations are checkable); provenance recorded per assertion;
  **no-source-no-claim** + **allowlist** + **license-composition (NC + SA + redistribution + OncoKB
  no-AI-training)** + **staleness** fail-safe implemented and tested; normalization (HGNC/SO/HGVS/Mondo,
  **GA4GH VRS, and the structured fusion-representation model**) in place; **VICC-metakb cross-KB
  consensus check** wired in; **contradiction-surfacing** that proposes `contested`/`refuted` candidates
  (reviewer-ratified); **curation-QA harness** scaffolded.

- **M2 — Core diagnostic/defining cards (domain-reviewed).**
  *Goal:* the foundational cards, cited + graded + reviewed.
  *Exit:* EWSR1-FLI1 + EWSR1-ETS variant cards, CD99 and NKX2-2 cards drafted from **pre-written
  evidence-state stubs** the reviewer corrects, cited, evidence-graded, and **domain-reviewer-signed-off**;
  the **fusion-type card explicitly carries `evidenceStatus: refuted-superseded`** (van Doorninck 2010 /
  Le Deley 2010) for the prognostic claim while keeping the diagnostic use valid; the **CD99/NKX2-2 cards
  carry the round-cell-sarcoma differential** (CIC-/BCOR-rearranged, EWSR1-FISH non-partner-specificity);
  citation-coverage + license checks green; framing present. *(Gated on a secured domain reviewer.)*

- **M3 — Prognostic / candidate-predictive / circulating cards + QA.**
  *Goal:* extend the research library and prove curation accuracy.
  *Exit:* STAG2 / TP53 / CDKN2A / copy-number cards and the EWSR1-FLI1 ctDNA card drafted, cited,
  graded, domain-signed-off — with the **accuracy guardrails enforced** (TP53/CDKN2A **not** flat
  prognostic markers; adverse signal is the **STAG2+TP53** subset; **1q gain validated** vs. **chr 8
  gain a passenger**); **contradiction-coverage = 100%** for known-conflicted markers; **curation-QA
  audit ≥ 95%** faithfulness plus a **prospective/cooperative-group corroboration** check and a
  **second-reviewer concordance** sample on grading; dataset + accessible site (WCAG 2.2 AA) render the
  research cards.

- **M4 — Patient/family plain-language layer (HIGH risk; dual-gated).**
  *Goal:* honest, sourced, education-only versions for families.
  *Exit:* plain-language renditions of the published research cards, each **oncologist + advocate
  signed-off**, carrying the not-medical-advice frame, written to a readability + tone standard (no
  false hope/despair), accessibility verified. *(Gated on secured oncologist + advocate reviewers; if
  not secured, this phase does not ship and the research-only library stands.)*

- **M5 — Publication, adoption & handoff (the deed).**
  *Goal:* a real beneficiary adopts and uses the cards.
  *Exit (Definition of Shipped):* secured partner/steward adopts the cards; independent verification
  of licensing/provenance/grading/review gates; reuse recorded. *(Gated on a secured partner — TO BE
  SECURED.)*

- **M6 — Sustain & maintain (post-delivery).**
  *Goal:* keep the library accurate and alive.
  *Exit:* maintenance rotation + review cadence (re-verification + re-sign-off driven by
  `lastVerified`/`validUntil`); outcomes/reuse dashboard (not traffic); documented process for adding
  new biomarkers behind the same gates.

Critical-path human dependencies: the **domain reviewer** (gates M2–M3), the **oncologist + advocate
reviewers** (gate M4), and the **adopting partner/steward** (gates M5). Per the dated
partner-acquisition plan and the build-vs-mothball/pivot decision rule, the project holds or pivots
rather than shipping a patient-facing cancer resource without review or to no beneficiary.

---

## Work breakdown

The itemized, schema-mapped backlog lives in **`TASKS.md`**: ~18 tasks across milestones M0–M6 plus
a future backlog, each mapped to the Elyos Task JSON schema, with per-task acceptance criteria for
the most important items, milestone Definitions of Done, and a complete example Task JSON for the
first M0 task (the **data/licensing & compliance policy spec**, reflecting its status as the hard
first build item). The sequencing puts compliance + schema first, license-verified sourcing second,
domain-reviewed research cards third, and the dual-gated patient-facing layer only after the research
library is solid.

---

## Governance, roles & stakeholders

- **Maintainer (Owner): TBD.** Owns the schema, the agent-neutral tooling core, CI, the license
  register, and merge quality.
- **Reviewers / rotation:** at least one engineering reviewer plus a designated **compliance/licensing
  reviewer** who audits the allowlist, license register, and license-composition checks independently.
- **Domain reviewer (MEDIUM tier): TO BE SECURED** — a molecular pathologist, sarcoma researcher, or
  qualified bioinformatician; signs off every research card for accuracy and correct evidence grading.
- **Credentialed expert reviewers (HIGH tier, patient layer): TO BE SECURED** — a **clinical
  oncologist** (ideally pediatric/sarcoma) **and** a **patient advocate** (ideally with lived Ewing
  experience or an advocacy-org science role); **both** sign off every patient-facing card.
  - **Independence / COI:** reviewers disclose material conflicts (industry funding, ownership in a
    referenced assay/drug) and **recuse** from any card touching a product they are tied to.
  - **Version-scoped sign-off:** sign-off attaches to a specific card version + citation set and does
    **not** carry forward to edits (dovetails with `lastVerified`/`validUntil`); re-edits need
    re-sign-off.
  - **Name-use limits:** a reviewer's name/credentials may appear in the reviewers ledger only for the
    versions they approved, with consent, and may not imply endorsement of the whole product.
  - **Disagreement fallback:** the relevant expert holds a **veto on whether a card is safe/accurate
    to ship**; a maintainer cannot override a "do not ship" on substance. Disagreements are logged and
    escalated to Elyos governance / a second reviewer; for contested patient-facing content the
    project prefers a **second expert** before shipping.
- **Steward (last-mile owner): TO BE SECURED** — owns the partner relationship, adoption, and
  outcome/reuse tracking that constitute shipping.
- **Partner / requestor: TO BE SECURED** — a sarcoma/pediatric-oncology research consortium, an
  advocacy nonprofit, an academic Ewing lab, or an open cancer-knowledge curation community.
- **Community / board:** licensing edge-cases (e.g., NC-source composition) and the patient-layer
  go/no-go go through Elyos governance.

---

## Dependencies & integrations

- **External services:** Anthropic Claude API (assistive extraction/summarization only, behind the
  neutral client); a static-site host (no tracking); CI.
- **Datasets / sources:** the approved open/aggregate sources in the license register (PMC-OA, CIViC,
  Open Targets, ClinVar/dbSNP, DepMap, open GEO/cBioPortal/TARGET-open/PeCan-aggregate, ontologies),
  each verified + dated; COSMIC/OncoKB as cite-only references.
- **Ontologies/standards:** HGNC, Sequence Ontology, HGVS, Mondo/NCIt, OncoTree, GA4GH VRS, a
  structured fusion-representation model; evidence/appraisal frameworks (**Simon–Hayes/TMUGS LOE**,
  **REMARK**, **ACCE/EGAPP**, AMP/ASCO/CAP, ESMO ESCAT, CIViC); VICC metakb for cross-KB consensus.
- **Upstream/sibling Elyos projects:** `ewing-open-data-catalog` (dataset datasheets), `ewing-
  literature-corpus` (PMC-OA Ewing corpus), `ewsr1-fli1-knowledge-graph`, `ewing-drug-target-evidence`,
  `ewing-immunotherapy-target-catalog` — share sources and normalization; `ewing-trial-finder` /
  `ewing-family-guide` own the HIGH-risk patient-facing surfaces this project deliberately stays out
  of beyond the gated card layer.
- **Elyos pieces:** `packages/schema` (Task JSON), `CLAUDE.md` (work rules + refusal guardrails),
  `docs/good-deed-definition.md` (risk tiers), Elyos governance for licensing/edge-case decisions.
- **Human/decision dependencies (critical path):** secured **domain reviewer** (blocks M2–M3),
  secured **oncologist + advocate reviewers** (block M4), secured **partner/steward** (blocks M5).

---

## Adjacent opportunities

The machinery built here is broader than one card set. These are **opportunities, not commitments** —
sequenced behind the core deed and the same gates:

- **`biomarker-extraction` (upstream toolchain, feeds in).** The provenance-first, **verbatim-span**
  extraction + contradiction-detection harness is reusable as a standalone Elyos capability; it
  produces curated *candidate* assertions that this project (and siblings) grade and review. Keeping it
  a shared upstream tool — not re-implemented per project — is the anti-duplication boundary.
- **`ewsr1-fli1-knowledge-graph` (parallel).** Cards become **evidence-bearing edges**; the normalized
  HGNC/SO/Mondo/VRS refs make linkage trivial (backlog `kg-022`). The KG gives structure; the cards
  give graded, cited, refuted-aware evidence.
- **`ewing-drug-target-evidence` / `drug-target-evidence-cards` (perpendicular).** Therapeutic-target
  evidence (where ESCAT/AMP-ASCO-CAP fit) shares a common **`evidence-card-core`** (schema + grading
  library + provenance engine) with a clean prognostic/diagnostic-vs-therapeutic boundary.
- **`ewing-immunotherapy-target-catalog` (perpendicular).** Surface-antigen / cell-therapy targets
  reuse the card schema and provenance engine (backlog `immuno-020`).
- **Generalized rare-cancer biomarker-evidence engine (high-leverage, perpendicular).** The whole
  machine — license register → allowlist ingest → validity-tiered grading → refuted-status tracking →
  staleness fail-safe → review gates — is **disease-agnostic**. Ewing is the proving ground; the same
  engine serves other rare/pediatric cancers where pan-cancer KBs are thin.
- **Read-only MCP server (perpendicular, post-M5).** Expose the validated card dataset as an MCP server
  (query by gene/marker/association type → graded, cited, provenance-stamped cards) so other agents
  consume it safely — open data, no PII (extends backlog `api-021`).
- **REMARK-appraisal-as-a-service (parallel).** The Claude-assisted REMARK checklist runner could be a
  general open tool for prognostic-marker study appraisal beyond Ewing.

---

## Risks & mitigations

| Risk | Likelihood | Impact | Mitigation | Owner |
|---|---|---|---|---|
| Controlled-access or identifiable patient data enters the project | Low | Critical | Source allowlist enforced in tooling (cannot pull off-allowlist); dbGaP/EGA/biobank intentionally excluded; provenance audit; reject + purge any individual-level source | Compliance reviewer / Maintainer |
| Restrictively-licensed data (COSMIC/OncoKB) redistributed in violation | Medium | High | `redistributionAllowed:false` in register; license-composition check fails the build on copied restricted content; cite-and-link-only policy | Compliance reviewer |
| Inaccurate biomarker claim or wrong evidence grade | Medium | High | Two-axis grading by association type recorded per assertion; domain-reviewer sign-off before ship; curation-QA faithfulness audit (≥95%); no-source-no-claim | Domain reviewer |
| Category error — prognostic/diagnostic marker graded on an actionability scale (ESCAT/AMP-ASCO-CAP) | Medium | High | Framework chosen **by association type** (Simon–Hayes LOE for prognostic; AMP-ASCO-CAP/ESCAT only for predictive); REMARK as appraisal; schema stores framework per association | Domain reviewer |
| Refuted/contested marker propagated as current (e.g., fusion-type prognosis, TP53-alone) | Medium | High | First-class `evidenceStatus` + `supersededBy[]`; contradiction-coverage metric = 100%; pre-written evidence-state stubs; reviewer-ratified status | Domain reviewer |
| Faithful summary of a *weak* source (100% faithful to a refuted 1998 paper) | Medium | High | Prospective/cooperative-group corroboration metric; evidence-strength axis distinct from faithfulness; second-reviewer concordance sample | Domain reviewer |
| OncoKB text used as AI/ML training input in violation of license | Low | High | `aiTrainingAllowed:false` in register; OncoKB never fed to the extraction model (cite-only); compliance audit | Compliance reviewer |
| ShareAlike (copyleft) source silently relaxes a card's license | Low | Medium | `shareAlike` flag modeled as a distinct contaminant in the license-composition gate; PharmGKB/SA sources out of scope | Compliance reviewer |
| Patient-facing content read as medical advice / gives false hope or despair | Medium | Critical | HIGH tier; oncologist **and** advocate dual sign-off; persistent not-medical-advice frame; tone standard; layer ships only when secured (else withheld) | Oncologist + Advocate |
| Card silently goes stale as the science moves | High | High | `lastVerified`/`validUntil` runtime fail-safe (auto-flag/withhold past window); review cadence; re-sign-off required to restore | Maintainer / Domain reviewer |
| LLM extraction hallucinates a citation or overstates a source | Medium | High | Extraction is assistive only; every assertion human-verified against source; no-source-no-claim drops unresolvable citations; QA audit | Domain reviewer |
| NC-source content contaminates a CC-BY/CC0 card | Medium | Medium | Per-source license tracked on each provenance record; license-composition check sets `cardLicense` and blocks incompatible composition | Compliance reviewer |
| No domain reviewer secured → research cards blocked | Medium | High | Dated recruitment (by 2026-08-31) via sarcoma labs/advocacy networks; hold at M1 without one (hard gate) | Maintainer |
| No oncologist + advocate secured → patient layer can't ship | Medium | High | Patient layer is optional/gated; research-only library still a valid deed; recruit via advocacy orgs/clinics | Steward / Maintainer |
| No adopting partner → cannot reach Definition of Shipped | High | High | Honest `TO BE SECURED`/`verifiedNeed:false`; dated partner plan + build-vs-mothball/pivot rule at ~2027-03-31 (donate to a curation commons or mothball, not ship to no one) | Steward / Maintainer |
| Misinformation/hype creep (overstated "breakthroughs") | Medium | High | Evidence levels + uncertainty mandatory; no efficacy/treatment claims; tone + framing lint; review | Domain reviewer |
| Source/database terms change after ingestion | Medium | Medium | License register carries verification date + re-check cadence; staleness applies to licenses too; re-verify on cadence | Compliance reviewer |

---

## Security & privacy

**Threat surface.** This is a public, open-data content project, so the surface is narrower than a
multi-tenant app, but real risks remain: (a) **ingesting prohibited data** (controlled-access or
identifiable patient data) — the top guardrail; (b) **license violation** (redistributing restricted
content); (c) **misinformation injection** via a malicious or low-quality source/contribution; (d)
**LLM hallucination** of citations or claims; (e) **secrets leakage** in tooling.

**Controls.**
- **Source allowlist as a hard gate** — ingestion cannot pull from an unapproved source; dbGaP/EGA/
  biobanks and any individual-level data are excluded by construction; provenance audited.
- **License-composition + redistribution checks** in CI — a card body containing
  `redistributionAllowed:false` content fails the build; `cardLicense` is computed from contributing
  sources.
- **No-source-no-claim + curation-QA audit** — every assertion human-verified against its source;
  hallucinated/unresolvable citations dropped; sampled faithfulness audit each release.
- **No PII by construction** — only aggregate/open data and biomarker-level assertions are stored; no
  individual records, no linkage, no re-identification.
- **Contribution review** — external contributions go through maintainer + compliance + domain review
  before merge; a source not in the register is rejected until verified.
- **Secrets hygiene** — API keys via environment/secret store only; no secrets/tokens in logs,
  receipts, or committed files (Elyos rule); dependency + secret scanning in CI.

**Abuse/misuse prevention.** The project refuses requests to add individual patient data, to mine
controlled-access cohorts, to make individualized medical recommendations, or to produce hype/efficacy
claims — and records the refusal. The patient-facing layer's not-medical-advice frame and dual
expert gate are the primary safety controls against the most consequential misuse (a family acting on
a card as if it were advice).

---

## Sustainability & maintenance

After delivery, a named **maintenance rotation** owns the schema, tooling, license register, and
site; the **steward** owns the partner relationship and outcome/reuse tracking. Cards carry a
**review cadence** enforced at **runtime, not just on a calendar**: `lastVerified`/`validUntil` drive
the fail-safe staleness behavior so a card past its review window is auto-flagged or withheld until a
reviewer re-verifies it and the relevant expert re-signs-off. The **license register is itself
re-verified on a cadence** (source terms change). Outcomes are tracked as **reuse and beneficiary
impact** (adoptions, citations of the cards, embeddings in advocacy resources) — **not** page views.
Adding new biomarkers follows the same documented, gated process (cite → grade → domain review →
[patient layer: dual review]). The curation-QA audit is maintained as a living check and expanded as
new failure modes appear.

---

## Open questions

- **Two-axis grading model — confirm.** This plan adopts **Simon–Hayes/TMUGS LOE** for prognostic +
  **AMP/ASCO/CAP + ESCAT** for predictive (where any exists) + **REMARK** as a study-appraisal checklist
  + an **ACCE validity-type axis** (CIViC levels cross-walked where applicable). This **supersedes** the
  v0.1 "CIViC/AMP-ASCO-CAP/ESCAT for everything" default (which was a category error for prognostic
  markers). Needs domain-reviewer + Elyos governance confirmation. (Decided in M0.)
- **Refuted-marker policy — confirm.** Accept `evidenceStatus` (`emerging | established | contested |
  refuted-superseded`) + `supersededBy[]` as schema fields, and frame the **fusion-type card explicitly
  as a refuted prognostic marker** (diagnostic use retained). Proposed: yes.
- **Predictive-scope headline.** Given Ewing has **no validated predictive (treatment-selection)
  biomarker**, state that as a headline finding rather than implying candidate-predictive cards carry
  actionable weight. Proposed: yes (front-page methods card).
- **Differential-diagnosis depth** — how far into round-cell-sarcoma differentials (CIC-/BCOR-rearranged,
  DSRCT) the diagnostic cards go before it becomes a pathology atlas (scope boundary vs. siblings).
- **VICC/metakb integration** — confirm adding it as a cross-KB consensus source (cite-only /
  standards-aligned) vs. keeping the source list lean. Proposed: add as consensus check.
- **Domain-reviewer profile** — is a molecular pathologist sufficient, or do the prognostic LOE /
  prospective–retrospective judgments also need a sarcoma **clinical trialist**?
- **Grading reproducibility** — with one domain reviewer, how to sample **second-reviewer concordance**
  on evidence levels (known AMP/ASCO/CAP inter-lab variability)?
- **OncoKB no-AI-training wiring** — confirm the `aiTrainingAllowed:false` constraint is enforced in
  the compliance spec and the extraction pipeline (OncoKB text never used as model input).
- **CC-BY-NC and CC-BY-SA source composition** — how aggressively to use CC-BY-NC PMC-OA articles
  (they make affected cards NC); SA/copyleft sources are excluded by the composition gate. Proposal:
  prefer CC-BY/CC0 for card bodies; NC sources for citation and clearly-marked NC cards only. Needs
  governance.
- **Patient-facing layer go/no-go** — ship only if both an oncologist and an advocate reviewer are
  secured; otherwise the research-only library is the deliverable. Confirm the threshold.
- **Which partner/steward first** — research consortium vs. advocacy nonprofit vs. curation community;
  affects whether the patient layer is in scope for v1.
- **Reviewer compensation/credit** — volunteer vs. a future funded lane for review hours (hard budget
  cap) without compromising independence.
- **Biomarker set boundaries** — where the line sits between this project (prognostic/diagnostic/
  circulating) and siblings (`ewing-drug-target-evidence` = therapeutic targets;
  `ewing-immunotherapy-target-catalog`) via the shared `evidence-card-core`, to avoid duplication.
- **Multilingual patient layer** — defer to `ewing-info-translations` (HIGH), or produce source
  English only here?

---

## References

- Proposal: `governance/proposals/ewing-biomarker-evidence-cards.md` *(TO BE WRITTEN — not yet
  created)*
- Portfolio entry: `planning/ROADMAP.md` (Track 8a — Ewing's Sarcoma focus)
- Elyos work rules & refusal guardrails: `CLAUDE.md`
- Good-deed definition & risk tiers: `docs/good-deed-definition.md`
- Task JSON schema: `packages/schema/src/schemas.ts`
- House-style sibling plans: `planning/projects/public-official-guide/{PLAN,TASKS}.md`,
  `planning/projects/revolutionary-patriots-kg/{PLAN,TASKS}.md`
- Evidence / appraisal frameworks: CIViC evidence levels; AMP/ASCO/CAP somatic variant tiers (Li et
  al. 2017); ESMO ESCAT scale of clinical actionability (Mateo et al. 2018); REMARK prognostic-marker
  reporting checklist (McShane et al. 2005); ACCE/EGAPP analytic/clinical-validity/utility framework;
  TMUGS / Simon–Hayes Levels of Evidence (tumor-marker utility); GRADE
- Ewing evidence (fusion-type refutation): de Alava et al., *JCO* 1998; van Doorninck et al., *JCO*
  2010 (COG); Le Deley et al., *JCO* 2010 (Euro-E.W.I.N.G. 99); Tirode et al., *Cancer Discov* 2014
  (STAG2+TP53); Lerman et al., *Pediatr Blood Cancer* 2015 (TP53/CDKN2A); COG *JCO* 2025 (1q gain)
- Cross-KB / standards: VICC meta-knowledgebase (Wagner et al., *Nat Genet* 2020; GA4GH); GA4GH VRS;
  OncoTree
- Sources & licenses: PMC Open Access subset; CIViC (CC0); Open Targets; ClinVar/dbSNP (US-gov PD);
  DepMap (CC-BY-4.0); GEO / cBioPortal / TARGET-open / St. Jude PeCan (open/aggregate tiers); COSMIC
  and OncoKB (non-commercial / redistribution-restricted — cite-only; **OncoKB additionally no-AI/ML-
  training**); PharmGKB (CC-BY-SA — out of scope, germline PGx)
- Competitive analysis: `COMPETITIVE-ANALYSIS.md` (this repo) — landscape, differentiators, and the
  correctness fixes merged into this v0.2

---

## Appendix A — Improvements applied

The following 25 specific improvements were identified during drafting and **applied** to this plan
and to `TASKS.md` (not merely noted). Each is concrete and reflected in the text above.

1. **Compliance-first sequencing.** Made the data/licensing & compliance policy spec + source
   allowlist/license register the literal first build items (M0), mirroring the exemplar's
   "guardrails-first" pattern — applied in *Roadmap M0*, *Work breakdown*, and TASKS M0.
2. **Binding cancer guardrails lead the data section.** *Data, licensing & compliance* opens with the
   two binding rules (open/aggregate/de-identified only; per-source license verification) before any
   source list.
3. **Explicit out-of-scope for controlled-access + identifiable data.** dbGaP/EGA/biobanks and
   identifiable patient data named as out-of-scope in *Scope*, *Data section*, and as a Critical risk.
4. **Source register with per-source `redistributionAllowed`.** Added a license table distinguishing
   reuse-OK (CIViC/CC0, DepMap/CC-BY, ClinVar/PD) from cite-only (COSMIC/OncoKB) — *Data section*.
5. **License-composition check + per-card `cardLicense`.** Added a CI check that computes the
   effective card license and blocks NC/restricted contamination — *Data section*, *Risks*, TASKS.
6. **No-source-no-claim enforced by a test**, not a guideline — *Success metrics*, *Data section*,
   M1 task.
7. **Published evidence-grading framework, recorded per assertion.** Adopted CIViC/AMP-ASCO-CAP/ESCAT
   instead of an ad-hoc curator scale — *Solution approach*, *Open questions* (M0 decision).
8. **Runtime staleness fail-safe** (`lastVerified`/`validUntil` auto-flag/withhold) ported from the
   exemplar — *Data section*, *Sustainability*, *Risks*, M1 task.
9. **Patient-facing layer split into a separate, dual-gated phase (M4)** so the research library
   stands alone — *Roadmap*, *Quality gates*, *Scope*.
10. **Dual expert sign-off (oncologist + advocate) for any patient card**, both required — *Quality
    gates*, *Governance*, *Success metrics*.
11. **Domain-reviewer gate for every research card** distinct from the patient gate — *Quality gates*,
    TASKS reviewers.
12. **Curation-QA faithfulness audit (≥95%, sampled)** to catch extraction overstatement — *Success
    metrics*, M3 task.
13. **Dated partner-acquisition plan** (reviewer by 2026-08-31; oncologist+advocate by 2026-10-31;
    steward by 2026-12-31) instead of open-ended TBD — *Executive summary*, *Problem*.
14. **Build-vs-mothball/pivot decision rule** at ~2027-03-31 (donate to a curation commons or
    mothball) so a cancer resource never ships to no beneficiary — *Executive summary*, *Risks*.
15. **Honest `verifiedNeed:false` / TO BE SECURED** across all delivery tasks and the example JSON —
    *Problem*, TASKS.
16. **Outcome-based success metrics** (adoption, documented reuse, faithfulness, provenance coverage)
    instead of card-count/traffic vanity metrics — *Success metrics*, *Sustainability*.
17. **Normalization to standards** (HGNC/SO/HGVS/Mondo/NCIt) for interoperability + de-duplication —
    *Solution approach*, M1 task.
18. **Curated high-value biomarker set named** (EWSR1-FLI1/ETS variants, CD99, NKX2-2, STAG2, TP53,
    CDKN2A, copy-number, ctDNA) with reviewer-confirmed final set — *Solution approach*, M2–M3 tasks.
19. **Tone/safety standard for the patient layer** (no false hope, no false despair, uncertainty
    shown) — *Quality gates*, *Risks*, M4 task.
20. **Persistent framing strings** ("research reference, not clinical guidance" / "research
    information, not medical advice") lint-checked — *Quality gates*, *Success metrics*, M0 task.
21. **Reviewer independence/COI, version-scoped sign-off, name-use limits, disagreement veto** ported
    and adapted to oncologist/advocate context — *Governance*.
22. **Sibling-project boundaries** drawn to avoid duplication and to keep HIGH-risk trial/eligibility
    work out of scope — *Scope*, *Dependencies*, *Open questions*.
23. **Accessibility (WCAG 2.2 AA)** for the human-readable site, given a family/advocate audience —
    *Scope*, *Roadmap M3/M4*.
24. **Machine-readable + human-readable dual deliverable** (validated JSON dataset + accessible site)
    so both researchers and advocates are served — *Scope*, *Solution approach*.
25. **Schema-valid example Task JSON** for the first M0 task with honest fields and a real
    `outputLicense` — TASKS *Example task JSON*.

---

## Review sign-off

A completeness + correctness review was performed against the PLAN_SPEC 17-section structure, the
Elyos rules (`CLAUDE.md`), the good-deed definition, and the binding cancer guardrails.

**Checks performed and result:**
- **All 17 required H2 sections present and in order** — verified (Executive summary → References),
  plus two v0.2 additive sections (*Competitive landscape & differentiation*, *Adjacent
  opportunities*) merged from the competitive analysis without displacing the required structure.
- **Cancer guardrails lead the data section and are binding** — verified (open/aggregate/de-identified
  only; per-source license verification; controlled-access + identifiable data out of scope).
- **License treatment correct** — COSMIC/OncoKB marked non-commercial/cite-only; CIViC CC0; DepMap
  CC-BY; ClinVar PD; PMC-OA per-article; license-composition check added. Fixed during review: added
  an explicit `cardLicense` composition gate so NC sources cannot silently relax to CC-BY.
- **No medical advice / patient layer HIGH + dual-reviewed** — verified across Scope, Quality gates,
  Governance, Roadmap M4, Risks.
- **Provenance on every assertion** — verified (no-source-no-claim test, provenance model, curation-QA
  audit).
- **Schema mapping** — TASKS fields map to `packages/schema/src/schemas.ts`; example Task JSON
  validated field-by-field against the schema's required list and enums (type/lane/priority/riskTier/
  deliverable/tokenEstimate/status), `additionalProperties:false` respected, `verifiedNeed:false`.
  Fixed during review: confirmed no funded tasks (so no `fundedBudgetUsd` required); set example
  `riskTier` to `medium` (research compliance card) consistent with the project tier.
- **Honesty** — no partner/reviewer invented; all delivery tasks `TO BE SECURED` / `verifiedNeed:false`;
  dated partner plan + pivot/mothball rule present.
- **Outcome-based metrics** — verified (no vanity metrics in the success table).

**Residual items requiring a human decision** (carried in *Open questions*): final evidence-grading
framework choice; CC-BY-NC composition policy; patient-layer go/no-go threshold; first partner type;
reviewer compensation model.

**Sign-off:** Plan is internally consistent, schema-aligned, guardrail-compliant, and ready for
maintainer + Elyos-governance review. It is **not** approved to ship any patient-facing content —
that remains gated on secured oncologist + advocate reviewers per the plan.

---

## Changelog — v0.2 (analysis merged)

v0.2 merges `COMPETITIVE-ANALYSIS.md` into the plan. All changes are **additive/surgical**; no cancer
guardrail was weakened and no fact was invented. Highlights:

**Correctness / schema / methodology fixes applied:**
- **Fixed the grading category error.** Replaced the v0.1 "CIViC/AMP-ASCO-CAP/ESCAT for everything"
  default with a **two-axis model**: evidence-strength **by association type** (Simon–Hayes/TMUGS LOE
  for prognostic; AMP/ASCO/CAP + ESCAT only for predictive; REMARK as a study-appraisal checklist) ×
  a **`validityDimension`** axis (analytic / clinical-validity / clinical-utility, per ACCE/EGAPP).
  No prognostic/diagnostic marker is graded on an actionability scale (most Ewing markers are ESCAT
  Tier X). — *Goals, Solution approach §5, Open questions, Risks.*
- **Made refuted markers first-class.** Added **`evidenceStatus`** (`emerging | established | contested
  | refuted-superseded`) + **`supersededBy[]`** to the schema; the **fusion-type prognostic card is
  explicitly refuted** (van Doorninck 2010 / Le Deley 2010) while its diagnostic use is retained. —
  *Data model, Solution approach, Roadmap M2, Risks.*
- **Added a `maturity` (preclinical/translational/clinically-validated) field** and **rare-cancer
  cohort caveats** (small-n / single-institution vs. cooperative-group) to every association.
- **Per-marker accuracy guardrails:** TP53/CDKN2A **not** flat prognostic (adverse signal is the
  STAG2+TP53 subset); **1q gain validated vs. chr 8 gain a passenger**; CD99 sensitive-not-specific +
  **EWSR1-FISH non-partner-specificity** + **CIC-/BCOR-rearranged differential**; pre-written
  evidence-state stubs seed M2/M3 cards so the reviewer corrects rather than originates.
- **Licensing/provenance hardening:** added **VICC metakb** (cross-KB consensus), **GA4GH VRS /
  OncoTree / structured fusion model** to normalization; modeled **OncoKB's no-AI/ML-training clause**
  (`aiTrainingAllowed:false` — never fed to the extraction model) and **CC-BY-SA / ShareAlike** as a
  distinct license contaminant alongside NC; documented PharmGKB as out-of-scope germline PGx.
- **Metrics gaps closed:** added prospective/cooperative-group **corroboration**, **contradiction
  coverage = 100%**, **second-reviewer concordance**, and **staged adoption** outcomes.

**Strategy integrated:**
- New **## Competitive landscape & differentiation** (OncoKB non-commercial/no-train, CIViC CC0,
  abandoned My Cancer Genome, JAX-CKB freemium, COSMIC/ClinVar/cBioPortal/VICC, PharmGKB) — differentiator
  = **validity-tiered, refuted-aware, provenance-bearing open biomarker cards for a rare cancer**, with
  "no validated predictive biomarker in Ewing" as a headline.
- **Claude API leverage folded into architecture** — span-grounded extraction with mandatory verbatim
  quotes under per-article license checks, normalization into the schema, **contradiction surfacing
  (never resolution)**, REMARK/validity drafting — keeping no-clinical-recommendations, no-final-grades,
  and curator/oncologist review as human gates.
- New **## Adjacent opportunities** — `biomarker-extraction` (feeds candidate assertions in),
  `ewsr1-fli1-knowledge-graph` (evidence edges), shared **`evidence-card-core`** with
  `ewing-drug-target-evidence` / `drug-target-evidence-cards`, the generalized rare-cancer engine, and
  a read-only **MCP server**; optimizations folded into the Roadmap; Open questions merged.

**Preserved:** all 17 required sections, the binding cancer guardrails (open data only; per-source
license verify; provenance per assertion; research-not-advice; rare-cancer caution), the dual-gated
patient layer, the honest `TO BE SECURED` / `verifiedNeed:false` posture, and the dated
partner-acquisition + mothball/pivot rule. Version bumped **0.1.0 → 0.2.0**.
