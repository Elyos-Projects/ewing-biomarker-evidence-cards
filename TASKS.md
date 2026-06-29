# Ewing-Biomarker-Evidence-Cards — TASKS.md

> Status: Draft · Version: 0.1.0 · Last updated: 2026-06-28 · Owner: TBD (maintainer) · Lane: donated

Backlog for **Ewing-Biomarker-Evidence-Cards** (slug: `ewing-biomarker-evidence-cards`), an open,
source-cited library of biomarker evidence cards for Ewing Sarcoma — a research-evidence reference
(with an optional, separately-gated plain-language layer for families that is education only, never
medical advice). See `PLAN.md` for full context.

The **data/licensing & compliance policy + source allowlist/license register is the first build
item** and a hard requirement. Binding cancer guardrails: **open / aggregate / de-identified data
only** (controlled-access — dbGaP, EGA, individual-level biobanks — and any identifiable patient data
are OUT OF SCOPE); **per-source license verification before reuse** (COSMIC/OncoKB non-commercial →
cite-only; CIViC CC0; DepMap CC-BY; ClinVar PD; PMC-OA per-article); **provenance on every
assertion**. This is a **MEDIUM** risk-tier project; the **patient/family-facing layer is HIGH** and
needs **oncologist + patient-advocate** sign-off. No partner/reviewer/steward secured, so
delivery-dependent tasks carry `requestor: TO BE SECURED` and `verifiedNeed: false`.

## How these tasks map to Elyos

Each task below becomes an Elyos **Task JSON** validated against `packages/schema/src/schemas.ts`.
Field mapping:

- **id** — stable slug `ewing-biomarker-evidence-cards-<area>-NNN`.
- **title** — the task title in the milestone table.
- **project** — `ewing-biomarker-evidence-cards`.
- **type** — one of `code | research | writing | data | design-spec | maintenance`.
- **lane** — `donated` (default; no funded tasks planned. Any `funded` task must add `fundedBudgetUsd`
  with a hard cap).
- **priority** — `high | medium | low`.
- **domain** — array, e.g. `["cancer","oncology","ewing-sarcoma","biomarkers","open-data","bioinformatics"]`.
- **riskTier** — `low | medium | high`. Compliance/schema/tooling is low–medium; cards asserting
  biomarker science are `medium`; **patient/family-facing cards are `high`** (oncologist + advocate
  sign-off).
- **urgent** — boolean (no urgent tasks at cold-start).
- **deliverable** — `pr | dataset | document | translation`.
- **tokenEstimate** — `small | medium | large` (maps to the Size column).
- **status** — `open | in-progress | review | delivered | done` (all start `open`).
- **context / objective / acceptanceCriteria[] / resources[] / output** — per task.
- **requestor** — partner/steward/reviewer; `TO BE SECURED` where unknown.
- **verifiedNeed** — `true` only once a specific partner/need is confirmed; otherwise `false`
  (currently `false` everywhere — no partner secured).
- **outputLicense** — code/tooling: **MIT**; card content/dataset: **CC-BY-4.0** (or **CC0** where all
  contributing sources are CC0; **CC-BY-NC-4.0** where an NC source contributes); docs: **CC-BY-4.0**.

Size legend: small ≈ tokenEstimate `small`, med ≈ `medium`, large ≈ `large`.
Reviewer key: **Domain reviewer** = molecular pathologist / sarcoma researcher / qualified
bioinformatician (MEDIUM-tier card accuracy + grading, **TO BE SECURED**). **Oncologist + Advocate** =
credentialed clinical oncologist **and** patient advocate (HIGH-tier patient layer dual sign-off,
**TO BE SECURED**). **Compliance reviewer** = licensing/allowlist auditor.

---

## Milestone M0 — Compliance, schema & cold-start

| ID | Title | Type | Size | Risk | Deliverable | Depends on | Reviewer |
|---|---|---|---|---|---|---|---|
| ewing-biomarker-evidence-cards-compliance-001 | Data/licensing & compliance policy spec (open/aggregate/de-identified only; cite-only restricted sources) | design-spec | medium | medium | document | — | Compliance reviewer + Maintainer |
| ewing-biomarker-evidence-cards-sources-002 | Source allowlist + license register (initial sources verified + dated; `redistributionAllowed` flags) | data | medium | medium | dataset | 001 | Compliance reviewer |
| ewing-biomarker-evidence-cards-schema-003 | Evidence-card JSON Schema + ajv validation in CI | code | medium | low | pr | 001 | Maintainer |
| ewing-biomarker-evidence-cards-grading-004 | Evidence-grading framework decision + mapping (CIViC / AMP-ASCO-CAP / ESCAT) | design-spec | small | medium | document | 003 | Domain reviewer + Maintainer |
| ewing-biomarker-evidence-cards-repo-005 | Monorepo + pnpm/TS/ESM + CI (build/test/lint + schema/allowlist/license/framing tests) | code | small | low | pr | — | Maintainer |

**Acceptance criteria — key tasks**

- **ewing-biomarker-evidence-cards-compliance-001** (compliance policy spec)
  - States the binding guardrails: **only open-access / aggregate / de-identified sources**;
    **controlled-access (dbGaP, EGA, individual-level biobanks) and any identifiable patient data are
    out of scope**; no re-identification or individual-level data.
  - Defines **per-source license verification before reuse** and the `redistributionAllowed`
    cite-only rule for restricted sources (COSMIC, OncoKB).
  - Defines the **no-source-no-claim** requirement and the per-assertion provenance fields
    (source, accession/PMID/PMCID/DOI, retrievalDate, license, redistributionAllowed).
  - Defines the **license-composition rule** (a card's effective `cardLicense` is the most restrictive
    of its contributing sources; restricted content never enters a card body).
  - Mandates the framing strings ("research reference — not clinical guidance"; patient layer:
    "research information, not medical advice") and the review gates (domain for research cards;
    oncologist + advocate for patient cards).
  - Reviewed + signed off by the compliance reviewer (recorded).

- **ewing-biomarker-evidence-cards-sources-002** (allowlist + license register)
  - Each source recorded with: name, type, **access tier (open/aggregate/de-identified only)**,
    license, `redistributionAllowed`, attribution requirement, verification date, verifier.
  - dbGaP/EGA/biobanks/individual-level sources are **absent by guardrail**; COSMIC/OncoKB marked
    `redistributionAllowed:false` (cite-only); CIViC `CC0`, DepMap `CC-BY-4.0`, ClinVar/dbSNP PD,
    PMC-OA per-article.
  - Ingestion tooling can **only** pull from allowlisted sources; an allowlist test fails the build if
    an off-allowlist source appears in provenance.

- **ewing-biomarker-evidence-cards-schema-003** (card schema)
  - JSON Schema covers identity (with HGNC/SO/HGVS/Mondo refs), biology, `clinicalAssociations[]`
    (claim, associationType, direction, populationContext, evidenceLevel+framework+version,
    citations[], provenance[], uncertaintyNote), governance (riskTier, reviewStatus, reviewers[],
    lastVerified, validUntil, disclaimer, cardLicense).
  - ajv validation runs in CI; an invalid card fails the build; `evidenceLevel` and ≥1 `citation` +
    `provenance` are **required** on every clinical association.

- **ewing-biomarker-evidence-cards-grading-004** (grading framework)
  - Chooses the framework(s) (proposal: CIViC levels cross-walked to AMP/ASCO/CAP + ESCAT) and records
    the cross-walk; every assertion must store the framework name + version.
  - Domain reviewer confirms the mapping is faithful to the published frameworks.

**M0 Definition of Done:** compliance policy spec (compliance-reviewed) + source allowlist/license
register merged; evidence-card JSON Schema + ajv validation green in CI; grading framework chosen +
documented; repo/CI skeleton with schema/allowlist/license/framing tests green; framing strings wired
into templates; **domain-reviewer recruitment started (dated)**.

---

## Milestone M1 — License-verified sourcing & provenance pipeline

| ID | Title | Type | Size | Risk | Deliverable | Depends on | Reviewer |
|---|---|---|---|---|---|---|---|
| ewing-biomarker-evidence-cards-ingest-006 | Ingestion connectors for allowlisted open sources (PMC-OA, CIViC, Open Targets, ClinVar, DepMap, open GEO/cBioPortal) | code | large | medium | pr | 002, 003 | Compliance reviewer + Maintainer |
| ewing-biomarker-evidence-cards-provenance-007 | Provenance + no-source-no-claim + allowlist + license-composition enforcement + staleness fail-safe | code | medium | medium | pr | 003, 006 | Maintainer + Compliance reviewer |
| ewing-biomarker-evidence-cards-normalize-008 | Normalization to HGNC/SO/HGVS/Mondo/NCIt + de-duplication | code | medium | low | pr | 003 | Maintainer |
| ewing-biomarker-evidence-cards-qa-009 | Curation-QA / extraction-faithfulness audit harness | code | small | medium | pr | 007 | Domain reviewer + Maintainer |

**Acceptance criteria — key tasks**

- **ewing-biomarker-evidence-cards-ingest-006** (ingestion connectors)
  - Pulls candidate evidence **only** from allowlisted sources; records full provenance per item;
    per-article PMC-OA license captured; COSMIC/OncoKB **not ingested** (cite-only references).
  - LLM-assisted extraction is **assistive**; no assertion is emitted without a resolvable citation;
    works on fixtures.

- **ewing-biomarker-evidence-cards-provenance-007** (enforcement + staleness)
  - **No-source-no-claim**, **allowlist**, and **license-composition** checks block non-compliant
    cards in CI; `cardLicense` computed as the most-restrictive contributing license.
  - Each assertion carries `lastVerified` + `validUntil`; at render time a past-`validUntil` assertion
    is **auto-flagged or withheld** until re-verified + re-signed-off (staleness test asserts this).

- **ewing-biomarker-evidence-cards-qa-009** (curation QA)
  - Samples shipped assertions and checks each faithfully represents its cited source (no
    overstatement); reports a faithfulness rate; target **≥ 95%**, with failures routed to re-review.

**M1 Definition of Done:** allowlisted-only ingestion with full provenance working on fixtures;
no-source-no-claim + allowlist + license-composition + staleness fail-safe implemented and tested;
normalization + de-duplication in place; curation-QA harness scaffolded.

---

## Milestone M2 — Core diagnostic / defining cards (domain-reviewed)

| ID | Title | Type | Size | Risk | Deliverable | Depends on | Reviewer |
|---|---|---|---|---|---|---|---|
| ewing-biomarker-evidence-cards-fusion-010 | EWSR1-FLI1 + EWSR1-ETS variant cards (EWSR1-ERG/ETV1/ETV4/FEV), cited + graded | writing | large | medium | document | 004, 007, 008 | Domain reviewer |
| ewing-biomarker-evidence-cards-ihc-011 | Diagnostic immunomarker cards (CD99/MIC2, NKX2-2), cited + graded | writing | medium | medium | document | 004, 007, 008 | Domain reviewer |

**Acceptance criteria — key tasks**

- **ewing-biomarker-evidence-cards-fusion-010** (fusion cards)
  - Every assertion cited + provenance-backed (no-source-no-claim) and assigned an evidence level
    (framework + version); diagnostic vs. prognostic associations distinguished with `populationContext`
    and an `uncertaintyNote`.
  - Genes/fusions normalized (HGNC/SO/HGVS); restricted-source facts cited, not copied; carries the
    "research reference — not clinical guidance" frame.
  - **Domain-reviewer sign-off recorded** (version-scoped) before ship.

- **ewing-biomarker-evidence-cards-ihc-011** (immunomarker cards)
  - CD99 and NKX2-2 cards state diagnostic use **with sensitivity/specificity caveats and
    non-specificity of CD99**, each assertion cited + graded; no diagnostic instruction to any
    individual; domain-reviewer sign-off recorded.

**M2 Definition of Done:** EWSR1-FLI1 + ETS-variant and CD99 + NKX2-2 cards drafted, cited,
evidence-graded, and **domain-reviewer-signed-off**; citation-coverage + license-composition checks
green; framing present. *(Gated on a secured domain reviewer.)*

---

## Milestone M3 — Prognostic / candidate-predictive / circulating cards + QA + site

| ID | Title | Type | Size | Risk | Deliverable | Depends on | Reviewer |
|---|---|---|---|---|---|---|---|
| ewing-biomarker-evidence-cards-prognostic-012 | Prognostic/biologic cards (STAG2, TP53, CDKN2A, characteristic copy-number changes), cited + graded | writing | large | medium | document | 010, 011 | Domain reviewer |
| ewing-biomarker-evidence-cards-circulating-013 | Circulating-biomarker card (EWSR1-FLI1 ctDNA / fusion-transcript), cited + graded | writing | medium | medium | document | 010 | Domain reviewer |
| ewing-biomarker-evidence-cards-dataset-014 | Release the machine-readable dataset (open license) + accessible static site (WCAG 2.2 AA) | code | medium | low | dataset | 012, 013 | Maintainer |
| ewing-biomarker-evidence-cards-audit-015 | Curation-QA audit pass on the research card set (faithfulness ≥ 95%) | maintenance | small | medium | document | 009, 012, 013 | Domain reviewer + Compliance reviewer |

**Acceptance criteria — key tasks**

- **ewing-biomarker-evidence-cards-prognostic-012** (prognostic cards)
  - STAG2/TP53/CDKN2A/copy-number cards report prognostic associations **with their evidence level,
    population context, and explicit uncertainty**; **no individual prognosis or treatment
    implication**; cited + provenance-backed; domain-reviewer sign-off recorded.

- **ewing-biomarker-evidence-cards-circulating-013** (ctDNA card)
  - Reports the evidence on EWSR1-FLI1 ctDNA/fusion-transcript detection (e.g., as a research/monitoring
    biomarker) with evidence level and uncertainty; **no claim of clinical utility for an individual**;
    cited; domain-reviewer sign-off recorded.

- **ewing-biomarker-evidence-cards-dataset-014** (dataset + site)
  - Dataset validates against the schema; each card's `cardLicense` is correct and exposed; restricted
    content absent from bodies; site meets **WCAG 2.2 AA**, no trackers, framing visible on every page.

- **ewing-biomarker-evidence-cards-audit-015** (QA audit)
  - Independent sampled audit confirms **≥ 95%** faithfulness; license + allowlist + provenance
    coverage = 100%; any failures fixed or cards withheld; results recorded.

**M3 Definition of Done:** prognostic/predictive/circulating cards drafted, cited, graded,
domain-signed-off; curation-QA audit ≥ 95%; open dataset + accessible site render the research cards
with correct licenses and framing.

---

## Milestone M4 — Patient/family plain-language layer (HIGH risk; dual-gated)

| ID | Title | Type | Size | Risk | Deliverable | Depends on | Reviewer |
|---|---|---|---|---|---|---|---|
| ewing-biomarker-evidence-cards-plainlang-016 | Plain-language family renditions of published cards (education-only, not medical advice) | writing | large | high | document | 014, 015 | Oncologist + Advocate |

**Acceptance criteria — ewing-biomarker-evidence-cards-plainlang-016**
- One plain-language rendition per **published, domain-reviewed** research card; each is sourced to its
  parent card's citations and carries the persistent **"this is research information, not medical
  advice — talk to your (or your child's) oncology team"** frame.
- Meets a **readability + tone standard**: no false hope (no cure/efficacy promises), no false despair,
  uncertainty shown plainly, no individualized risk/prognosis/treatment guidance.
- **Both an oncologist and a patient advocate sign off** every card (dual, version-scoped, recorded);
  COI disclosed/recused as needed.
- Accessibility (WCAG 2.2 AA) verified; cards withheld (not auto-published) if either sign-off is
  missing or stale.

**M4 Definition of Done:** plain-language family layer published **only** for cards with oncologist +
advocate dual sign-off, carrying the not-medical-advice frame and meeting the tone/readability +
accessibility standards. *(Gated on secured oncologist + advocate reviewers; if not secured, this
phase does not ship and the research-only library stands as the deed.)*

---

## Milestone M5 — Publication, adoption & handoff (the deed)

| ID | Title | Type | Size | Risk | Deliverable | Depends on | Reviewer |
|---|---|---|---|---|---|---|---|
| ewing-biomarker-evidence-cards-pilot-017 | Secure adopting partner/steward + independent verification of license/provenance/grading/review gates | research | medium | medium | document | 014, 015 | Steward + Compliance reviewer + Domain reviewer |

**Acceptance criteria — ewing-biomarker-evidence-cards-pilot-017**
- A real beneficiary (research group / advocacy nonprofit / curation community) is secured as an
  adopter; steward assigned; `verifiedNeed` flips to `true`.
- Independent verification that: provenance coverage = 100%, license-composition correct (no restricted
  content in bodies), evidence grading present + framework-named, all research cards domain-signed-off
  (and any shipped patient cards dual-signed-off).
- Driven by the **dated partner-acquisition plan**; if no adopter is secured by **~2027-03-31**, apply
  the **build-vs-mothball/pivot rule** (donate cards + schema to an open curation commons, or mothball
  to maintenance-only) — recorded in governance — rather than ship to no beneficiary.

**M5 Definition of Done:** project-level **Definition of Shipped** met — a real beneficiary adopts and
uses the cards, all licensing/provenance/grading/review gates independently verified, reuse recorded.
*(Gated on a secured partner — TO BE SECURED.)*

---

## Milestone M6 — Sustain & maintain (post-delivery)

| ID | Title | Type | Size | Risk | Deliverable | Depends on | Reviewer |
|---|---|---|---|---|---|---|---|
| ewing-biomarker-evidence-cards-ops-018 | Maintenance rotation + review cadence + reuse/outcomes dashboard + new-biomarker process | maintenance | medium | medium | document | 017 | Maintainer + Steward |

**Acceptance criteria — ewing-biomarker-evidence-cards-ops-018**
- Named maintenance rotation; review cadence driven by `lastVerified`/`validUntil` (re-verification +
  re-sign-off); license register re-verified on a cadence.
- Reuse/outcomes dashboard tracks adoptions, citations of the cards, and embeddings in advocacy
  resources (**not** page views).
- Documented, gated process for adding new biomarkers (cite → grade → domain review → [patient layer:
  oncologist + advocate]).

**M6 Definition of Done:** project sustainably maintained with a rotation, a runtime-enforced review
cadence, reuse-based outcome tracking, and a gated expansion process.

---

## Backlog / future

| ID | Title | Type | Size | Risk | Deliverable | Notes |
|---|---|---|---|---|---|---|
| ewing-biomarker-evidence-cards-i18n-019 | Multilingual patient-facing cards | writing | large | high | translation | Defer to / coordinate with `ewing-info-translations` (HIGH); per-language oncologist+advocate review |
| ewing-biomarker-evidence-cards-immuno-020 | Immunotherapy surface-antigen target cards | writing | medium | medium | document | Coordinate with `ewing-immunotherapy-target-catalog` to avoid duplication |
| ewing-biomarker-evidence-cards-api-021 | Read-only query API over the card dataset | code | medium | low | pr | Only after M5; static, no PII, open data |
| ewing-biomarker-evidence-cards-kg-022 | Link cards to `ewsr1-fli1-knowledge-graph` entities | code | medium | medium | pr | Interop via normalized HGNC/SO/Mondo refs |
| ewing-biomarker-evidence-cards-feedback-023 | Reviewer/advocate feedback + correction intake workflow | code | small | medium | pr | Corrections trigger re-review + re-sign-off |

---

## Example task JSON

Complete, schema-valid Task JSON for the first M0 task (`ewing-biomarker-evidence-cards-compliance-001`):

```json
{
  "id": "ewing-biomarker-evidence-cards-compliance-001",
  "title": "Data/licensing & compliance policy spec (open/aggregate/de-identified only; cite-only restricted sources)",
  "project": "ewing-biomarker-evidence-cards",
  "type": "design-spec",
  "lane": "donated",
  "priority": "high",
  "domain": ["cancer", "oncology", "ewing-sarcoma", "biomarkers", "open-data", "compliance"],
  "riskTier": "medium",
  "urgent": false,
  "deliverable": "document",
  "tokenEstimate": "medium",
  "status": "open",
  "context": "Ewing-Biomarker-Evidence-Cards builds an open, source-cited library of biomarker evidence cards for Ewing Sarcoma - a research-evidence reference, with an optional separately-gated plain-language layer for families that is education only, never medical advice. Because this is a cancer-data project that families and advocates may read, the binding cancer guardrails are the first build item, not an afterthought: ONLY open-access / aggregate / de-identified data may be used (controlled-access resources - dbGaP, EGA, individual-level biobanks - and ANY identifiable patient data are out of scope), and every source's license must be verified before reuse (COSMIC and OncoKB are non-commercial / redistribution-restricted and may be cited but never redistributed; CIViC is CC0; DepMap CC-BY-4.0; ClinVar public domain; PMC-OA per-article). Every assertion must carry a citation and provenance. No partner, reviewer, or steward is yet secured.",
  "objective": "Write the authoritative data/licensing & compliance policy specification that all later sourcing, schema, and content work must implement and be tested against: the open/aggregate/de-identified-only rule and the controlled-access/identifiable-data out-of-scope boundary; per-source license verification and the redistribution-allowed (cite-only) rule for restricted sources; the no-source-no-claim requirement and per-assertion provenance fields; the license-composition rule that sets each card's effective license; and the framing strings ('research reference - not clinical guidance' for research cards, 'research information, not medical advice' for the patient layer) plus the review gates (domain reviewer for research cards; oncologist + patient-advocate for patient cards).",
  "acceptanceCriteria": [
    "States the binding guardrails: only open-access / aggregate / de-identified sources; controlled-access (dbGaP, EGA, individual-level biobanks) and any identifiable patient data are out of scope; no re-identification or individual-level data",
    "Defines per-source license verification before reuse and the redistributionAllowed cite-only rule for restricted sources (e.g., COSMIC, OncoKB cited and linked, never redistributed)",
    "Defines the no-source-no-claim requirement and the per-assertion provenance fields (source, accession/PMID/PMCID/DOI, retrievalDate, license, redistributionAllowed)",
    "Defines the license-composition rule: a card's effective cardLicense is the most restrictive of its contributing sources, and restricted-source content never enters a card body",
    "Mandates the framing strings and review gates: 'research reference - not clinical guidance' (research) and 'research information, not medical advice' (patient layer); domain-reviewer sign-off for research cards; oncologist + patient-advocate dual sign-off for patient-facing cards",
    "Reviewed and signed off by the compliance/licensing reviewer (recorded)"
  ],
  "resources": [
    "planning/projects/ewing-biomarker-evidence-cards/PLAN.md",
    "planning/ROADMAP.md",
    "CLAUDE.md",
    "docs/good-deed-definition.md",
    "packages/schema/src/schemas.ts"
  ],
  "output": "A reviewed compliance policy-specification document defining the open/aggregate/de-identified-only data rule, the controlled-access/identifiable-data out-of-scope boundary, per-source license verification + cite-only rule, the no-source-no-claim provenance requirement, the license-composition rule, and the framing + review gates - the contract that ewing-biomarker-evidence-cards-sources-002 (allowlist/register), -schema-003 (card schema), and -provenance-007 (enforcement) implement and verify.",
  "requestor": "TO BE SECURED",
  "verifiedNeed": false,
  "outputLicense": "CC-BY-4.0"
}
```
