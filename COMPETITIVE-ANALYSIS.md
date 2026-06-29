# Competitive & Improvement Analysis — `ewing-biomarker-evidence-cards`

> Analyst review of PLAN.md / TASKS.md (v0.1.0, 2026-06-28), grounded in web research with cited
> sources. Scope: an open, source-cited library of **biomarker evidence cards** for Ewing Sarcoma —
> RESEARCH evidence, not clinical/treatment advice. Risk: MEDIUM (patient-facing layer HIGH).

---

## 1. Correctness & completeness review of PLAN.md

The plan is unusually strong on **governance, licensing, and provenance discipline** — genuinely
best-in-class for a donated good-deed. The compliance-first sequencing, `redistributionAllowed`
license-composition gate, no-source-no-claim test, runtime staleness fail-safe, dual-gated
patient layer, and honest `TO BE SECURED`/`verifiedNeed:false` posture are all correct and rare.
The gaps below are almost entirely on the **biomarker-science methodology** side, where the plan is
thinner than its compliance scaffolding.

### 1.1 The evidence-grading framework choice is mis-specified for *prognostic/diagnostic* markers (most important methodological gap)

The plan proposes grading every clinical association with **CIViC levels cross-walked to AMP/ASCO/CAP
tiers and ESMO ESCAT** (PLAN §Solution approach step 5; §Goals; Open question 1). All three of those
frameworks are **therapeutic-actionability / somatic-variant-significance** scales:

- **AMP/ASCO/CAP 2017** (Li et al., *J Mol Diagn* 2017, Tier I–IV) classifies **somatic variants by
  clinical/therapeutic significance** — https://www.jmdjournal.org/article/S1525-1578(16)30223-9/fulltext
- **ESMO ESCAT** (Mateo et al., *Ann Oncol* 2018, Tier I–X) ranks **alterations as drug targets** —
  it is explicitly about treatment-matching actionability —
  https://www.annalsofoncology.org/article/S0923-7534(19)34179-1/fulltext
- **CIViC** evidence levels (A validated → E inferential) are oriented to **variant–therapy/clinical
  relevance** — https://www.nature.com/articles/ng.3774

But the *core* of the Ewing card set is **diagnostic** (CD99, NKX2-2, EWSR1 FISH) and **prognostic**
(STAG2, 1q gain, fusion type, ctDNA) — and there is **no validated predictive/treatment-selection
biomarker in Ewing at all** (see §1.2). ESCAT in particular will be near-useless: almost every Ewing
marker is ESCAT **Tier X** ("no evidence of actionability") because EWSR1-FLI1 has no approved
targeted therapy. Grading a *prognostic* marker on an *actionability* scale is a category error.

**What's missing:** the plan does not cite the frameworks actually designed for prognostic/diagnostic
markers:
- **REMARK** (McShane et al. 2005, 20-item checklist) for prognostic-marker **study reporting
  quality** — https://pmc.ncbi.nlm.nih.gov/articles/PMC2361579/ (E&E: https://journals.plos.org/plosmedicine/article?id=10.1371/journal.pmed.1001216)
- **TMUGS / Simon–Hayes Levels of Evidence (LOE I–V)** for **tumor-marker utility** and the
  **prospective–retrospective design** that is the route to Level I —
  https://pmc.ncbi.nlm.nih.gov/articles/PMC2782246/
- **ACCE / EGAPP** analytic-validity → clinical-validity → clinical-utility decomposition —
  https://pmc.ncbi.nlm.nih.gov/articles/PMC2743609/

**Fix:** adopt a **two-axis grading model** — (a) an *evidence-strength* axis using Simon–Hayes LOE
(prognostic) and AMP/ASCO/CAP + ESCAT (predictive, where any exists), and (b) a *validity-type* axis
(analytic / clinical / clinical-utility per ACCE). Use REMARK as a **per-study appraisal checklist**,
not an evidence grade. This is the single most consequential change to the plan.

### 1.2 No explicit analytic-validity vs clinical-validity vs clinical-utility separation

The card data model (`clinicalAssociations[].associationType` = diagnostic/prognostic/predictive/
pharmacodynamic) captures *what kind* of association but not *which validity question* the cited
evidence answers. A marker can have excellent **analytic validity** (the assay measures it reliably)
and strong **clinical validity** (it correlates with outcome) yet **zero demonstrated clinical
utility** (acting on it improves no outcome). This is the most common error in biomarker appraisal
and exactly the trap a families-facing layer must avoid. The schema should add an explicit
`validityDimension` field (analytic | clinical-validity | clinical-utility) per assertion. Source:
ACCE/EGAPP — https://archive.cdc.gov/www_cdc_gov/genomics/gtesting/acce/index.htm

### 1.3 The "failed biomarker" history is not designed in — and Ewing's flagship example is missing

The plan lists "EWSR1-FLI1 fusion type" as a card (§Solution approach; M2 task `fusion-010`) but does
**not** flag that **fusion type is the textbook example of a refuted prognostic biomarker**. Early
retrospective single-institution series (de Alava et al., *JCO* 1998 — https://ascopubs.org/doi/10.1200/JCO.1998.16.4.1248;
Zoubek 1996) reported Type 1 fusion = better prognosis. This was **definitively overturned** in the
intensive-chemotherapy era by prospective cooperative-group data:

- **van Doorninck et al., *JCO* 2010 (COG):** title — *"Current Treatment Protocols Have Eliminated
  the Prognostic Advantage of Type 1 Fusions in Ewing Sarcoma"* —
  https://ascopubs.org/doi/10.1200/JCO.2009.24.5845
- **Le Deley et al., *JCO* 2010 (Euro-E.W.I.N.G. 99), n=565:** *"the prospective evaluation did not
  confirm a prognostic benefit for type 1 EWS-FLI1 fusions."* — https://pubmed.ncbi.nlm.nih.gov/20308673/

A card library that doesn't structurally encode **"this association was reported, then refuted by
larger/prospective data; do not use"** risks propagating a dead biomarker. **Fix:** add first-class
schema fields for **temporal evidence status** (`evidenceStatus`: emerging | established | contested
| **refuted/superseded**) and a `supersededBy[]` citation list. This single feature would
differentiate the product from every competitor (none of OncoKB/CIViC/CKB cleanly represent
"refuted-by-later-evidence" for prognostic markers).

### 1.4 Several illustrative biomarker claims need accuracy guardrails the plan doesn't yet impose

The "initial biomarker set" (PLAN §Solution approach) lists markers without their actual evidence
state, which a drafting agent could mis-summarize:
- **TP53 / CDKN2A alone are NOT reliable prognostic markers** (Lerman et al., COG, *Pediatr Blood
  Cancer* 2015 — https://pubmed.ncbi.nlm.nih.gov/25464386/); the adverse signal is for the
  **concurrent STAG2+TP53** subset (Tirode et al., *Cancer Discov* 2014 —
  https://pubmed.ncbi.nlm.nih.gov/25223734/). The plan lists them as flat "prognostic" markers.
- **Chromosome 8 gain** is the most frequent CNA (~50%) but has **no consistent prognostic value** (a
  passenger); the plan lists "chr 8 gain" alongside 1q gain as if comparable. **1q gain** is the
  genuinely validated one (COG, *JCO* 2025 — https://ascopubs.org/doi/10.1200/JCO-25-00157).
- **CD99** is sensitive but **not specific**; molecular confirmation (EWSR1 FISH/RT-PCR) is the gold
  standard, and **EWSR1 FISH is not partner-specific** (positive in DSRCT, clear cell sarcoma, myxoid
  liposarcoma, etc.). TASKS `ihc-011` does require sensitivity/specificity caveats — good — but PLAN
  should also require the **CIC-rearranged / BCOR-rearranged round-cell sarcoma** differential, since
  those are now distinct EWSR1-negative entities and a families-facing miss here is dangerous.

**Fix:** seed the M2/M3 card tasks with a **pre-written "evidence state" stub per marker** (refuted /
moderate / validated / emerging) so the domain reviewer corrects rather than originates.

### 1.5 Metrics gaps

- **"≥95% extraction faithfulness"** measures *fidelity to the cited source* but **not whether the
  source itself is strong evidence**. A card can be 100% faithful to a refuted 1998 paper. Add a
  metric: *% of prognostic/predictive assertions whose evidence level is corroborated by ≥1
  prospective or cooperative-group study (or explicitly flagged as not).*
- **No inter-rater / grading-consistency metric.** AMP/ASCO/CAP tiering is known to be applied
  inconsistently between labs; with a single domain reviewer there is no reproducibility check. Add a
  periodic **second-reviewer concordance** sample on evidence-level assignment.
- **No "contradiction coverage" metric** despite conflicting evidence being central to this disease
  (fusion type, TP53). Add: *% of markers with known conflicting evidence that carry a `contested`/
  `refuted` representation.*
- Adoption target "≥1 beneficiary" is honest but **binary**; consider a staged outcome (reviewer-
  validated card set → cited in a review/guideline → embedded in an advocacy resource).

### 1.6 Smaller correctness notes

- **Source register additions worth considering:** the **VICC meta-knowledgebase** (harmonizes
  OncoKB/CIViC/CKB/PMKB/etc.) is the obvious cross-KB consensus check and is GA4GH-standards-based
  (Wagner et al., *Nat Genet* 2020 — https://www.nature.com/articles/s41588-020-0603-8); **GA4GH VRS**
  for variant representation; **Mondo/NCIt** are already cited (good); consider **OncoTree** for tumor
  subtype coding. The plan's normalization stack (HGNC/SO/HGVS/Mondo) is correct but omits a
  **fusion-specific** representation standard.
- **PharmGKB is listed nowhere but is germline-PGx**, not somatic — correctly out of scope; if a
  reviewer suggests it, document why it's excluded (it does not cover EWSR1 somatic biomarkers).
- **OncoKB's "no AI/ML training" clause** (https://faq.oncokb.org/licensing) deserves an explicit line
  in the compliance spec: even *cite-only* use must not feed OncoKB text into the extraction LLM as
  training/fine-tuning data. The plan's cite-only stance covers redistribution but not the
  AI-training prohibition specifically.
- **CC-BY-SA contagion:** if PharmGKB or any **ShareAlike** source is ever ingested, its copyleft
  would force the whole card to SA — the license-composition gate currently models NC but should also
  model **SA** as a distinct contaminant.
- The plan asserts CIViC content is **CC0** and ClinVar is **public domain** — both verified correct.

---

## 2. Competitive landscape

No competitor is a **Ewing-specific, open, provenance-first, validity-tiered** biomarker reference.
The field splits into (a) somatic-variant *actionability* knowledgebases, (b) genomic archives, (c)
aggregators, and (d) the *methodology* frameworks. Each is summarized with strengths/gaps and cited.

### Somatic-variant clinical-interpretation knowledgebases

**OncoKB** (MSK) — https://www.oncokb.org/ · license https://faq.oncokb.org/licensing
- *Does:* curates oncogenicity + clinical actionability of somatic alterations with three Levels-of-
  Evidence systems (Therapeutic Tx 1–4/R1–R2; Diagnostic & Prognostic — **heme only**).
- *Strengths:* the **only FDA-partially-recognized** oncology variant DB (Oct 2021; FDA decision
  summary https://www.fda.gov/media/152847/download); MSK-curated; aligned to FDA/NCCN; has a dedicated
  **EWSR1-FLI1 in Ewing Sarcoma** entry (https://www.oncokb.org/gene/FLI1/EWSR1-FLI1%20Fusion/Ewing%20Sarcoma).
- *Gaps:* **restrictive license** (free academic, paid commercial/clinical), API-gated, **no bulk
  download**, and an **explicit "no AI/ML training" prohibition**; Dx/Px levels exist only for heme, so
  Ewing's prognostic markers fall outside its leveled system; Ewing actionability is near-zero
  (no approved targeted therapy → no Tx Level 1/2). (Re)usable Data Project graded its license 3.5/5
  "restrictive" — https://reusabledata.org/oncokb.html

**CIViC** (WashU) — https://civicdb.org/ · paper https://www.nature.com/articles/ng.3774
- *Does:* open, expert-crowdsourced evidence items (therapeutic/prognostic/diagnostic/predisposing)
  with Evidence Levels A–E plus a 1–5★ trust rating; SEPIO-aligned provenance model.
- *Strengths:* **fully open — content CC0, code MIT**, public API, the only source here that is
  explicitly **AI/ML-trainable and redistributable**; strong provenance. The natural reuse foundation.
- *Gaps:* **crowdsourced → uneven, long-tail/rare-entity coverage is sparse and can be stale**; smaller
  corpus than commercial KBs; no FDA recognition; not a sarcoma-focused resource.

**My Cancer Genome** (Vanderbilt → GenomOncology) — https://www.mycancergenome.org/
- *Does:* disease→biomarker→therapy→trial educational/clinical KB; pioneering (2011).
- *Strengths:* clinician/patient-friendly framing; published clinical-trial data model.
- *Gaps:* **strong signs of abandonment** — last relaunch 2019, vendor handoff, live domain serves a
  **broken TLS cert** (altnames point to `*.wellybox.net`); content should be treated as **stale**; no
  open license, not reusable. A clear gap a maintained resource can fill. Retrospective:
  https://pmc.ncbi.nlm.nih.gov/articles/PMC8807017/

**JAX-CKB / CKB CORE** (Jackson Lab → Genomenon, 2024) — https://ckb.genomenon.com/ · paper
https://www.nature.com/articles/s41698-018-0073-y
- *Does:* relational precision-oncology KB (genes/variants/effects/therapies/trials), strong on
  complex/compound variants.
- *Strengths:* rigorous somatic curation; trial matching.
- *Gaps:* **freemium** — free **CKB CORE covers only ~50 rotating genes**; full coverage (2,200+ genes)
  is behind paid **CKB BOOST**; EWSR1 availability depends on the rotation.

### Genomic archives / aggregators

**COSMIC** (Sanger) — https://cancer.sanger.ac.uk/cosmic · licensing https://www.cosmickb.org/licensing
- *Does:* largest expert-curated catalogue of somatic mutations + **fusions** + CNVs; covers EWSR1 and
  EWSR1-FLI1/ERG fusions.
- *Strengths:* breadth/depth; standard recurrence reference.
- *Gaps:* **non-commercial license; commercial/clinical use needs a paid license**; registration now
  required even for academic — the plan correctly marks it **cite-only**.

**ClinVar** (NCBI) — https://www.ncbi.nlm.nih.gov/clinvar/docs/review_status/
- *Does:* public archive of variant clinical-significance assertions with a 0–4★ review-status system.
- *Strengths:* **public domain** (most permissive terms of any resource here); transparent provenance.
- *Gaps:* **predominantly germline**; somatic/Ewing coverage thin; submission quality heterogeneous.

**cBioPortal** (MSK/DFCI) — https://www.cbioportal.org/ · paper https://pmc.ncbi.nlm.nih.gov/articles/PMC3956037/
- *Does:* open genomics visualizer; **aggregates external annotations** (OncoKB/CIViC/MCG/COSMIC) onto
  observed alterations; hosts TCGA-SARC and contributed sarcoma cohorts.
- *Strengths:* AGPL/open; excellent cohort frequency/co-occurrence/prevalence context.
- *Gaps:* an aggregator/viewer, **not an evidence curator**; actionability only as good as upstream
  KBs; TCGA-SARC is light on Ewing specifically.

**VICC meta-knowledgebase ("metakb")** — https://search.cancervariants.org/ · paper
https://www.nature.com/articles/s41588-020-0603-8
- *Does:* **harmonizes six somatic KBs** (OncoKB, CIViC, CKB, PMKB, MolecularMatch, CGI) into one
  searchable aggregate (12,856 harmonized interpretations); GA4GH driver project (VRS standards).
- *Strengths:* canonical **cross-KB consensus** front door; standards-based; open docs.
- *Gaps:* aggregates rather than originates → inherits source licenses, update lag, and each KB's
  rare-entity sparsity; not prognostic-marker-oriented.

**PharmGKB** (Stanford, now ClinPGx) — https://www.pharmgkb.org/ · license CC-BY-SA 4.0
- *Does:* germline **pharmacogenomics** (variant–drug PK/PD), Levels 1A–4, drives CPIC guidelines.
- *Relevance:* **out of scope** — germline drug-metabolism, not somatic tumor biomarkers; relevant only
  for chemo-dosing PGx. Note the **CC-BY-SA copyleft** if ever used.

### Methodology / reporting frameworks (these are *standards to adopt*, not competitors)

- **REMARK** — 20-item **reporting checklist** for prognostic tumor-marker studies (reporting quality,
  **NOT** an evidence grade). McShane et al. 2005 — https://pmc.ncbi.nlm.nih.gov/articles/PMC2361579/
- **ACCE / EGAPP** — analytic validity / clinical validity / clinical utility evaluation backbone —
  https://pmc.ncbi.nlm.nih.gov/articles/PMC2743609/
- **TMUGS / Simon–Hayes LOE** — tumor-marker **utility grading** + LOE I–V; the prospective–
  retrospective design as route to Level I — https://pmc.ncbi.nlm.nih.gov/articles/PMC2782246/
- **AMP/ASCO/CAP 2017** — somatic-variant clinical-significance **Tiers I–IV** —
  https://www.jmdjournal.org/article/S1525-1578(16)30223-9/fulltext
- **ESMO ESCAT** — target **actionability** Tiers I–X — https://www.annalsofoncology.org/article/S0923-7534(19)34179-1/fulltext
- **GRADE** — intervention-certainty system; needs adaptation for prognostic markers (Huguet 2013) —
  https://gradepro.org/handbook/

**Competitive bottom line:** every actionability KB is thin on Ewing **by biology** (no approved
targeted therapy), none cleanly represents **prognostic-marker evidence, refuted markers, or
validity-type**, the most reuse-friendly source (CIViC) is sparse on rare entities, and the one
patient-friendly resource (My Cancer Genome) is effectively abandoned. That is the opening.

---

## 3. Gaps we can fill

1. **A Ewing-specific, curated whole.** Every competitor is pan-cancer; Ewing is a long-tail
   afterthought. A complete, reviewer-signed Ewing biomarker set (diagnostic + prognostic +
   circulating) does not exist as a maintained open artifact.
2. **Prognostic/diagnostic evidence, graded properly.** OncoKB Px/Dx levels are heme-only; ESCAT is
   actionability-only. A library that grades *prognostic* and *diagnostic* markers with the right
   frameworks (Simon–Hayes LOE, REMARK appraisal, ACCE validity-type) fills a real methodological gap.
3. **First-class "refuted / contested" representation.** No competitor structurally encodes
   "fusion-type prognosis was overturned by Euro-EWING 99 / COG." This is uniquely valuable in a
   disease whose flagship marker is a cautionary tale.
4. **Provenance per assertion + license-clean openness.** CIViC is CC0 but sparse; OncoKB is curated
   but restrictive and non-trainable. A CC-BY/CC0 set with per-assertion provenance and a license-
   composition guarantee is reusable in ways OncoKB/COSMIC are not.
5. **The validity chain made explicit.** Separating analytic vs clinical validity vs utility — and
   stating plainly that **Ewing has no validated predictive biomarker** — is exactly the honest framing
   families/advocates need and competitors don't provide.
6. **A maintained, staleness-gated alternative** to the abandoned My Cancer Genome for this disease.
7. **Cross-KB consensus, curated for one disease.** Pull the VICC/CIViC/OncoKB Ewing slices into one
   reconciled, conflict-annotated view — something the aggregators leave to the user.

---

## 4. Differentiators to win

1. **Validity-tiered + temporally-honest grading.** Two-axis (evidence-strength × validity-type) plus
   an `evidenceStatus` (emerging/established/contested/refuted) — no competitor does this for
   prognostic markers. This is the **single strongest differentiator**.
2. **Provenance-per-assertion, license-clean, AI/ML-reusable.** Unlike OncoKB (restrictive, no-train)
   and COSMIC (paid commercial), the output is CC-BY/CC0 with a machine-checked `cardLicense`.
3. **Refuted-biomarker memory.** Structurally representing overturned claims (fusion type, TP53-alone)
   prevents the most common real-world harm: acting on dead evidence.
4. **Disease-deep, reviewer-signed, not pan-cancer-shallow.** Domain-reviewer sign-off per card +
   curation-QA faithfulness audit ≥95% gives a trust profile aggregators can't match.
5. **Runtime staleness fail-safe.** Cards past `validUntil` auto-flag/withhold — a live answer to the
   My Cancer Genome failure mode.
6. **Honest "no medical advice / no validated predictive biomarker" framing** as a feature, dual-gated
   for the family layer — trustworthy where hype resources are not.

---

## 5. Claude API leverage

Per Elyos rules, Claude is **assistive only**, behind a provider-neutral client, with every assertion
human-verified. Concrete high-value uses:

**Where Claude helps (with provenance + human gate):**
1. **Evidence extraction with span-level provenance.** From PMC-OA full text, draft candidate
   assertions as structured tuples (claim, associationType, direction, populationContext, **verbatim
   supporting quote + PMID/PMCID + location**). Forcing a verbatim quote per assertion makes
   hallucinated citations detectable and supports the no-source-no-claim test.
2. **Evidence-card structuring/normalization.** Map free-text findings to the JSON schema; normalize
   gene/variant/disease names to HGNC/SO/HGVS/Mondo candidate IDs (human-confirmed); de-duplicate.
3. **Contradiction & supersession detection.** Cluster assertions per marker and surface conflicts
   (e.g., de Alava 1998 "Type 1 favorable" vs Le Deley 2010 "not confirmed") as candidate
   `contested`/`refuted` flags with `supersededBy[]` — a flagging aid, reviewer decides.
4. **REMARK-style appraisal drafting.** Run the 20-item REMARK checklist against a prognostic study and
   draft a structured quality note (sample size, design, pre-specification, validation) for the
   reviewer — speeds appraisal without grading.
5. **Validity-dimension tagging (draft).** Suggest whether a cited result speaks to analytic /
   clinical-validity / clinical-utility, for reviewer confirmation.
6. **Plain-language drafting (M4, behind dual gate).** Draft family-facing renditions to a readability/
   tone standard — never published without oncologist + advocate sign-off.
7. **Staleness triage.** Periodically scan new PMC-OA literature for a marker and flag cards whose
   evidence may have moved (queues re-review; does not edit).

**Where Claude must NOT decide:**
- **No clinical recommendations, individual prognosis, diagnosis, or treatment selection** — ever, in
  either layer.
- **Evidence-level / actionability tier assignment** needs a **defined rubric + domain-reviewer
  confirmation**; the LLM may *propose* a level with rationale but must not set the shipped grade
  (AMP/ASCO/CAP tiering is inconsistent even among expert labs — a model alone is not authoritative).
- **No fabricated or inferred citations.** An assertion without a resolvable source + verbatim
  supporting span is dropped, not surfaced.
- **License and medical determinations are human-verified.** Whether a source permits reuse, whether
  content is "redistribution-restricted," and any clinical-safety judgment are compliance-reviewer /
  clinician calls — not the model's. Note OncoKB's **no-AI-training** clause: its text must not be used
  to train/fine-tune the extraction model even when cited.
- **Refuted/contested status is reviewer-ratified**, not auto-published from the model's clustering.

(Model-selection specifics belong to the Claude API skill; this plan only needs the neutral-client
boundary and the human-gate rules above.)

---

## 6. Ten concrete optimizations

1. **Adopt the right grading stack.** Use **Simon–Hayes LOE** for prognostic and **AMP/ASCO/CAP +
   ESCAT** only for the (rare) predictive context, and **REMARK** as a study-appraisal checklist — not
   CIViC/ESCAT for everything. Record the framework *per association type*. (Resolves Open question 1
   and §1.1.)
2. **Add `validityDimension`** (analytic | clinical-validity | clinical-utility) to every assertion in
   the schema (§1.2).
3. **Add `evidenceStatus` + `supersededBy[]`** to the schema so refuted/contested markers (fusion type,
   TP53-alone) are first-class (§1.3) — and seed the fusion card with the Euro-EWING 99 / COG
   refutation.
4. **Seed each M2/M3 card task with a pre-written evidence-state stub** (refuted/moderate/validated/
   emerging + key citations) so the domain reviewer corrects rather than originates; cuts review load
   and error risk.
5. **Split CD99/NKX2-2 card to require the differential-diagnosis caveat** including CIC-rearranged and
   BCOR-rearranged round-cell sarcomas and EWSR1-FISH non-partner-specificity (§1.4).
6. **Add a VICC-metakb cross-KB consensus check** to the pipeline: for each marker, reconcile the
   OncoKB/CIViC slices and record agreement/conflict as provenance.
7. **Add grading-consistency metrics:** periodic second-reviewer concordance on evidence levels; a
   "contradiction coverage" metric; and a "corroborated by prospective/cooperative-group study" metric
   (§1.5).
8. **Model SA (ShareAlike) and the OncoKB no-AI-training clause** explicitly in the compliance spec and
   license-composition gate, not just NC/redistribution (§1.6).
9. **Add a fusion-representation standard** (e.g., a structured EWSR1::FLI1 breakpoint/exon model) to
   normalization, since fusion *architecture* is core to Ewing and HGVS alone is awkward for fusions.
10. **Ship a tiny "how we grade" methods card** as the library's front page (frameworks, validity
    chain, refuted-marker policy, limitations) — turns the methodology rigor into visible trust and
    pre-empts misreading.

---

## 7. Parallel & perpendicular spin-offs

- **`ewsr1-fli1-knowledge-graph` (parallel).** Cards become evidence-bearing edges; normalized
  HGNC/SO/Mondo refs make linkage trivial (already in backlog `kg-022`). The KG gives structure; the
  cards give graded, cited evidence — complementary.
- **`ewing-drug-target-evidence` (perpendicular).** This project owns *prognostic/diagnostic*; that one
  owns *therapeutic target* evidence (where ESCAT/AMP-ASCO-CAP actually fit). Share the source pipeline,
  schema, and grading library; draw a clean boundary (Open question 6).
- **`ewing-immunotherapy-target-catalog` (perpendicular).** Surface-antigen targets (e.g., for cell
  therapy) — reuse the card schema and provenance engine; backlog `immuno-020` already flags this.
- **`biomarker-extraction` (parallel toolchain).** The provenance-first, verbatim-span extraction +
  contradiction-detection harness is reusable as a standalone Elyos capability across cancer projects.
- **Generalized rare-cancer biomarker-evidence engine (perpendicular, high-leverage).** The whole
  machine (license register → allowlist ingest → validity-tiered grading → refuted-status tracking →
  staleness fail-safe → review gates) is **disease-agnostic**. Ewing is the proving ground; the same
  engine serves other rare/pediatric cancers where pan-cancer KBs are thin. This is the biggest
  strategic spin-off.
- **MCP server (perpendicular).** Expose the validated card dataset as a read-only **MCP server**
  (query by gene/marker/association type, returns graded, cited, provenance-stamped cards) so other
  agents/tools consume it safely — open data, no PII, post-M5. Extends backlog `api-021`.
- **REMARK-appraisal-as-a-service (parallel).** The Claude-assisted REMARK checklist runner could be a
  general open tool for prognostic-marker study appraisal beyond Ewing.

---

## 8. Open questions for the maintainer

1. **Grading framework:** will you adopt the two-axis model (Simon–Hayes LOE + ACCE validity-type for
   prognostic/diagnostic; AMP-ASCO-CAP/ESCAT only for predictive; REMARK as appraisal)? This changes
   §Solution-approach step 5 and Open question 1 materially.
2. **Refuted-marker policy:** do you accept `evidenceStatus`/`supersededBy[]` as schema fields, and is
   the fusion-type card explicitly framed as a refuted prognostic marker?
3. **Predictive scope:** given Ewing has **no validated predictive biomarker**, should the library state
   that as a headline finding rather than implying candidate-predictive cards carry actionable weight?
4. **Differential-diagnosis depth:** how far into round-cell-sarcoma differentials (CIC/BCOR/DSRCT) do
   diagnostic cards go before it becomes a pathology atlas (scope boundary vs siblings)?
5. **VICC/metakb integration:** add it as a cross-KB consensus source (cite-only / standards-aligned),
   or keep the source list lean?
6. **Domain-reviewer profile:** is a molecular pathologist sufficient, or do prognostic-grading and the
   prospective–retrospective LOE judgments need a sarcoma *clinical trialist* too?
7. **Grading reproducibility:** with one domain reviewer, how will you sample second-reviewer
   concordance on evidence levels (given known AMP/ASCO/CAP inter-lab variability)?
8. **Family-layer go/no-go threshold** (already in Open questions) — confirm it stays gated; and confirm
   the OncoKB no-AI-training constraint is wired into the compliance spec.
