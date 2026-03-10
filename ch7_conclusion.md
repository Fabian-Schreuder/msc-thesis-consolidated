# Chapter 7: Conclusion

<!-- MERGE NOTE: This chapter uses Phase 2's Conclusion as the primary fabric,
     expanded to encompass both phases of the thesis.
     Phase 2 has a strong conclusion with knowledge contribution framework.
     Changes:
     1. Summary expanded to encompass both phases
     2. Principal findings includes Phase 1 key findings
     3. Contribution to Knowledge adds Phase 1's evidence mapping contribution
     4. Limitations and Future Work draws from both phases
-->

<!-- [TONE HARMONIZED: Chapter-opening transition inserted — draws together both phases.] -->

This chapter draws together the findings of both research phases --- the evidence mapping and the artifact evaluation --- to assess the thesis's contribution to the field of AI-mediated dietary assessment for older adults.

## 7.1 Summary of the Research

<!-- SOURCE: Phase 2 Conclusion §6.1, expanded for two-phase scope -->

This thesis addressed the friction-fidelity paradox in automated dietary assessment: the inherent tension between minimising user burden to sustain longitudinal adherence and maximising nutritional data precision for clinical utility. The research was conducted in two complementary phases.

**Phase 1** employed a scoping review methodology (PRISMA-ScR) to systematically map the translational evidence on AI applications in geriatric nutrition. The review of 25 eligible studies revealed a nascent field characterised by a gap between technological potential and real-world application: 92% of systems remain at prototype or pilot stages, 84% rely on private datasets, and a systematic mismatch exists between system design and the needs of older adults. These findings established the evidence base and design requirements for the subsequent artifact development.

**Phase 2** followed the Design Science Research Methodology of Peffers et al. \citep{peffersDesignScienceResearch2007} to produce the Snap and Say artifact: a multimodal, agentic AI system for dietary assessment. The artifact's core contribution is a three-component routing architecture comprising (1) a Deterministic Complexity Scoring Engine that computes an auditable, non-linear complexity score $C = \sum w_d L_d^2 + P_{\text{sem}}$; (2) a Clinical Threshold Routing Module that compares this score against a configurable threshold $\tau$; and (3) a truncated adaptation of the USDA Automated Multiple-Pass Method, bounded by a safety budget of two clarification rounds.

## 7.2 Principal Findings

<!-- SOURCE: Phase 2 Conclusion §6.2, with Phase 1 findings integrated -->

**From the scoping review:** The evidence mapping revealed four distinct application categories (behaviour change, dietary assessment, screening, activity recognition) with a pronounced translational gap. The evidence gap map identified the absence of interventional studies in long-term care settings and the field's reliance on early-stage evaluations with small, non-representative samples. These findings provided both the motivation and the design constraints for the artifact.

**From the artifact evaluation:** The simulated benchmarking demonstrated that the agentic clarification cycles reduced caloric estimation MAE by 30.6% on Complex items relative to a single-shot baseline, yielding an overall MAE reduction of 22.8%. The suppression logic achieved a 92% True Negative Rate on Simple items, confirming that the routing architecture successfully withholds intervention where it would impose burden without commensurate clinical value. The optimal routing configuration ($C_{\text{thresh}}=15.0$, $\text{Conf}_{\text{thresh}}=0.85$) was established through systematic parameter sweep.

The human-in-the-loop evaluation confirmed that older adults ($N=19$, Age $\geq 65$) could operate the multimodal interface with low perceived complexity, temporal demand, and frustration. Qualitative feedback identified system latency (average 7.1 seconds) and occasional microphone failures as the primary sources of residual friction.

## 7.3 Contribution to Knowledge

<!-- SOURCE: Phase 2 Conclusion §6.3, expanded to include Phase 1 contribution -->

Situated within Gregor and Hevner's \citep{gregorNatureTheoryInformation2006} Knowledge Contribution Framework, this research constitutes an **Improvement**: a new solution to a known problem.

The thesis makes four specific contributions:

1. **A transportable algorithm for quantifying semantic ambiguity** of unstructured dietary inputs prior to generative inference, demonstrating that "probabilistic silence"---the capacity of an agent to recognise when it lacks sufficient context---can be enforced through deterministic pre-computation rather than post-generation prompting.

2. **The formalisation of the friction-fidelity paradox** as a design construct within dietary information systems, with empirical evidence that threshold-gated deferral can resolve it asymmetrically---intervening where clinical value justifies the cost, remaining silent where it does not.

3. **A translational contribution** demonstrating that established nutritional assessment methodologies (the USDA AMPM) can be partially automated through agentic state graph architectures without discarding their epistemic structure, extending their applicability to unsupervised, self-administered use by vulnerable populations.

4. **An evidence gap map of AI in geriatric nutrition** that systematically charts the translational landscape across application domains, study settings, and system maturity levels, providing a reusable methodological framework for identifying research priorities in emerging health informatics fields.

<!-- The fourth contribution is NEW — derived from Phase 1.
     [CORE NOVELTY: Evidence Gap Map methodology] -->

## 7.4 Limitations and Future Work

The findings are subject to constraints that delimit their generalisability. The human evaluation recruited a culturally homogeneous sample ($N=19$) in a controlled laboratory setting, precluding inferential statistical analysis. The simulated benchmarking represents upper-bound performance under ideal cooperative conditions. The architecture cannot distinguish food served from food consumed, and benchmarking was conducted against a single model version. The scoping review was restricted to English-language publications and excluded grey literature, meaning the design requirements derived from it may not fully capture the global state of the field.

The most consequential direction for future research is longitudinal field deployment to test whether the low-friction interaction model sustains adherence over weeks of daily use. Additional priorities include cohort-specific threshold validation, cultural localisation, post-meal image capture for intake estimation, clinician-facing interfaces with FHIR-compliant data export, and addressing the long-term care setting gap identified in the scoping review.

## 7.5 Closing Remarks

The prevailing assumption in dietary assessment technology has been that clinical precision and patient usability exist in zero-sum opposition. This thesis provides evidence to the contrary. By interposing a deterministic, auditable scoring layer between unstructured multimodal input and probabilistic generative reasoning, the Snap and Say artifact demonstrates that the depth of conversational inquiry can be calibrated to the clinical stakes of each individual meal entry---asking more when the data demand it, and asking nothing when they do not. The result is an architecture that respects both the clinical imperative for nutritional fidelity and the human imperative for frictionless interaction, offering a foundational step toward continuous dietary monitoring that serves the patients who need it most without overwhelming them in the process.

## 7.6 Data and Code Availability

In accordance with the principles of reproducible research and the open science mandate for Design Science Research in health informatics, the complete software artifact has been made publicly available.

The "Snap and Say" source code repository is archived at \url{https://github.com/Fabian-Schreuder/snapandsay}. This repository contains the full executable artifact, including:
1. The progressive web application (frontend).
2. The asynchronous API backend.
3. The LLM-orchestrated LangGraph agent layer, containing the exact system prompt iterations and the deterministic complexity scoring engine.
4. The Prompt Optimization benchmark scripts and historical execution logs used to derive the findings in Chapter 4 and Chapter 5.

Due to the constraints of the informed consent framework under which the study was conducted, the raw clinical interview transcripts and raw multimodal captures from the human-in-the-loop Phase 2 trials ($N=19$) are not publicly archived, but may be made available upon reasonable request to the corresponding institutions.

---

<!-- MERGE ANNOTATIONS SUMMARY FOR CH 7:
     - Phase 2 Conclusion preserved as primary fabric
     - Section 7.1 expanded: now summarises both phases (was Phase 2 only)
     - Section 7.2 expanded: Phase 1 key findings integrated before artifact findings
     - Section 7.3 expanded: Fourth contribution added (Phase 1's evidence gap map)
     - Section 7.4 expanded: Phase 1 methodological limitations included
     - Section 7.5 preserved verbatim — sacred fabric, strong closing
     - No Phase 2 content deleted
-->
