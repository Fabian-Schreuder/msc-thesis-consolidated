# Harmonization Registry

<!-- HR step output (2026-03-10).
     This document records all harmonization decisions applied across the consolidated thesis.
     It serves as a reference for the student and for the final LaTeX conversion. -->

## 1. Spelling Standardization

**Decision:** British English throughout. The thesis originates from a Dutch university where British English is the standard. Phase 2 already uses British spelling consistently; Phase 1 had minor inconsistencies.

### Applied Changes

| American Form | British Form | Location Fixed | Status |
|--------------|-------------|----------------|--------|
| optimizing | optimising | Ch 1, §1.1 | FIXED |
| artefact (mixed) | artifact | Ch 5 (3 occurrences) | FIXED — standardised on "artifact" without diacritical variation |

### Verified Consistent (No Action Needed)

| Pattern | Standard Form | Notes |
|---------|--------------|-------|
| -ise/-ize | -ise (British) | "operationalise", "utilise", "minimise", "characterise" — consistent throughout |
| -our/-or | -our (British) | "behaviour", "colour" — consistent in Phase 2; Phase 1 does not use these words |
| -re/-er | -re (British) | "centre" — not used; "meter" appears only in citations |
| artifact/artefact | artifact | Standardised across all chapters. Note: Phase 2 LaTeX source uses "artefact" in some locations — the final LaTeX must apply this standardisation. |

<!-- [ONTOLOGY STANDARDIZED: "artefact" → "artifact" throughout consolidated drafts] -->
<!-- [ONTOLOGY STANDARDIZED: "optimizing" → "optimising" in Ch 1] -->

## 2. Terminology Ontology

**Decision:** Standardise demographic and technical terminology to ensure the thesis speaks with one voice about the same concepts.

### Demographic Terms

| Deprecated Term | Standardised Term | Permitted Exceptions | Rationale |
|----------------|-------------------|---------------------|-----------|
| "the elderly" | "older adults" | In direct quotations; in compound "Frail Elderly" (MeSH term) | Person-first language; "elderly" as a noun is considered outdated in gerontology |
| "elderly population" | "older adult population" or "aging population" | In compound terms: "geriatric nutrition," "geriatric-centric" | Same rationale |
| "senior(s)" | "older adults" | In direct quotations from participants | Consistency |
| "aging" | "aging" (NOT "ageing") | — | Both phases already use "aging"; maintaining consistency |

**Verification:** Grep confirmed no instances of "elderly" as standalone noun in the consolidated drafts. The term appears only in compound forms ("Frail Elderly" MeSH term in Ch 2) and in Phase 1 citations (which are not editable).

### Technical Terms

| Deprecated/Variant | Standardised Form | First Use | Notes |
|-------------------|-------------------|-----------|-------|
| directed cyclic graph / DCG | directed state graph | Ch 4, §4.1.3 | Aligned in Phase 2 rewrite; verified in consolidation |
| Critique step | Semantic Gatekeeper | Ch 4, §4.1.3 | Aligned in Phase 2 rewrite |
| confidence gate (standalone) | three-tier routing policy | Ch 4, §4.2.3 | The confidence gate is tier 3 of the routing policy, not the whole mechanism |
| WFR / Weighed Food Record | AMPM (as gold standard) | Ch 1, §1.1 | WFR mentioned once in Ch 1 as alternative; AMPM is the primary reference method |
| "the system" / "the artifact" / "Snap and Say" | Context-dependent | — | "the artifact" in DSR-framed sections (Ch 3, 5, 6); "Snap and Say" at first use per chapter and in titles; "the system" in technical descriptions (Ch 4) |

<!-- [ONTOLOGY STANDARDIZED: Terminology alignment verified across all chapters] -->

## 3. Tense Reconciliation

**Decision:** Phase-aware tense logic. The thesis describes completed work (past tense) and present-state systems (present tense). The reconciliation follows established academic convention.

| Chapter | Dominant Tense | Rationale |
|---------|---------------|-----------|
| Ch 1 (Introduction) | Present | States the problem, describes the research design |
| Ch 2 (Literature Review) | Past | Reports completed review activities ("was designed", "were included") |
| Ch 2.5 (Implications) | Present | Draws ongoing implications from completed findings |
| Ch 3 (Methodology) | Present (prescriptive) | "The artifact shall..." — specifies requirements |
| Ch 4 (Artifact) | Present | Describes the system as it exists ("is realised", "encodes") |
| Ch 5 (Evaluation) | Past | Reports completed evaluation ("was evaluated", "demonstrated") |
| Ch 6 (Discussion) | Mixed | Past for specific findings; present for interpretation and implications |
| Ch 7 (Conclusion) | Past (summary) + Present (contributions) | Summarises what was done; states what the thesis contributes |

### Specific Tense Fixes Applied

No tense changes were required. Both Phase 1 and Phase 2 source material already follow the correct conventions. The bridging sections (§1.5, §2.5, §3.1, §6.3) were written with appropriate tense from the outset.

<!-- [TENSE RECONCILED: No corrections needed — source material and bridging content
     already follow phase-aware tense conventions.] -->

## 4. Tonal Harmonization

**Decision:** The two phases have naturally different tones — Phase 1 (observational/critical) and Phase 2 (constructive/solution-oriented). These differences are *appropriate* and should be preserved, as they reflect the distinct epistemological functions of each phase. The harmonization task is to smooth the *transitions* between tones, not flatten the chapters.

### Tonal Map

| Section | Tone | Voice | Notes |
|---------|------|-------|-------|
| Ch 1 | Synthesising, authoritative | Third-person ("this thesis") with occasional "we" | Establishes the problem; bridges both phases |
| Ch 2, §2.1-2.4 | Observational, measured | Third-person; "we" for author actions only ("we focus") | Phase 1 review voice — appropriate for evidence synthesis |
| Ch 2, §2.5 | Analytical, bridging | Third-person; more assertive than §2.1-2.4 | Transitions from observation to prescription |
| Ch 3 | Prescriptive, precise | First-person inclusive ("we utilise", "this research proposes") | Phase 2 DSR voice — appropriate for design specification |
| Ch 4 | Technical, architectural | Third-person ("the system", "the agent") | Impersonal technical description — appropriate for artifact |
| Ch 5 | Empirical, reporting | Third-person; past tense | Evaluation reporting — neutral, data-driven |
| Ch 6 | Interpretive, honest | First-person for claims ("the central claim of this thesis"); impersonal for limitations | Discussion voice — appropriate for critical analysis |
| Ch 7 | Summative, measured | Third-person ("this thesis provides evidence") | Conclusion — returns to authoritative tone of Ch 1 |

### Tonal Transitions Applied

1. **Ch 1 → Ch 2:** Opening paragraph inserted. Shifts from synthesising voice (Ch 1) to evidence-mapping voice (Ch 2) by explicitly framing the review as "the first phase of this research."

2. **Ch 2 → Ch 3:** Closing sentence added to §2.5. Shifts from implications ("what the field needs") to prescription ("what the artifact must do") — smooth epistemic transition.

3. **Ch 3 → Ch 4:** Closing sentence added to §3.4; opening paragraph added to Ch 4. Shifts from requirements specification to implementation description within the DSR process model.

4. **Ch 4 → Ch 5:** No change needed. The transition from technical description to evaluation is natural and well-handled by Ch 5's opening sentence.

5. **Ch 5 → Ch 6:** No change needed. Ch 6 opens with an explicit reference to Ch 5's evidence.

6. **Ch 6 → Ch 7:** Opening paragraph added to Ch 7. Frames the conclusion as drawing together both phases.

<!-- [TONE HARMONIZED: Transitions between observational (Phase 1) and constructive (Phase 2)
     voices smoothed through chapter-boundary insertions. Internal chapter voices preserved.] -->

## 5. Pacing Harmonization

**Decision:** The pacing differences between chapters are *structural* and should be preserved. Phase 1 (Ch 2) has dense, data-heavy paragraphs with many citations — appropriate for a literature review. Phase 2 (Ch 4) has procedural, architectural paragraphs — appropriate for system description. The bridging sections (§2.5, §6.3) use an argumentative, flowing style that mediates between the two.

### Pacing Observations (No Changes Required)

| Chapter | Pacing Pattern | Average ¶ Length | Citation Density | Notes |
|---------|---------------|-----------------|------------------|-------|
| Ch 1 | Moderate — builds toward the paradox | Medium (4-6 sentences) | Moderate | Reads as a funnel: wide context → narrow problem |
| Ch 2 | Dense — synthesises 25 studies | Medium-long (5-8 sentences) | High (multiple citations per ¶) | Appropriate for review |
| Ch 2.5 | Flowing — argumentative bridges | Medium (4-5 sentences) | Moderate | Each subsection: finding → objective → section reference |
| Ch 3 | Moderate — prescriptive specification | Medium (4-6 sentences) | Moderate | Objectives section uses bold headers for scannability |
| Ch 4 | Technical — architectural description | Variable (3-8 sentences) | Low (mostly self-referential) | Enumerated lists for agent nodes; mathematical notation |
| Ch 5 | Data-driven — results reporting | Short-medium (3-5 sentences) | Low-moderate | Tables break up prose; qualitative quotes provide texture |
| Ch 6 | Interpretive — analytical paragraphs | Medium-long (5-7 sentences) | Moderate | §6.3 mirrors §2.5 structure (deliberate) |
| Ch 7 | Summative — broad strokes | Medium (4-6 sentences) | Low | Draws down from detail to overview |

No pacing changes applied — the natural rhythm differences between chapters serve their respective functions and would be flattened by homogenisation.

## 6. Pronoun Consistency

**Decision:** "We" refers to the author(s). Used sparingly and consistently.

| Usage | Convention | Examples |
|-------|-----------|----------|
| Author actions | "we" | "we utilise", "we evaluate", "we focus" |
| Thesis itself | "this thesis", "this research" | NOT "our thesis" |
| Artifact | "the artifact", "the system", "Snap and Say" | NOT "our system" |
| Review | "this review", "the scoping review" | "we" only for reviewer actions ("we focus specifically on") |

Verified consistent across all chapters. No changes needed.

## 7. Summary of Changes Applied

| Change Type | Count | Chapters Affected |
|-------------|-------|-------------------|
| Spelling fix (optimizing → optimising) | 1 | Ch 1 |
| Spelling standardisation (artefact → artifact) | 3 | Ch 5 |
| Transitional opening paragraph inserted | 3 | Ch 2, Ch 4, Ch 7 |
| Transitional closing sentence inserted | 2 | Ch 2 (§2.5), Ch 3 (§3.4) |
| Ontology/terminology changes | 0 | Already consistent |
| Tense corrections | 0 | Already correct |
| Tonal smoothing | 5 transitions | Ch 1-7 boundaries |

**Total substantive edits:** 9 (surgical, as intended)
