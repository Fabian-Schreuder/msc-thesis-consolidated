# Chapter 1: Introduction

<!-- MERGE NOTE: This chapter consolidates the introductions from both Phase 1 and Phase 2.
     Phase 2's friction-fidelity paradox framing becomes the central narrative construct.
     Phase 1's broader landscape sets the stage; Phase 2 narrows to the specific problem.

     [OVERLAP: Repeated background on aging populations and nutritional challenges]
     Decision: Phase 1's broad framing (aging + chronic conditions + dietary assessment)
     is subsumed into 1.1. Phase 2's sharper framing (diabetes/sarcopenia + AMPM + friction-fidelity)
     becomes the problem narrowing in 1.2.

     [OVERLAP: AI in nutrition landscape description]
     Decision: Phase 1's comprehensive field mapping is referenced via cross-reference to Ch 2,
     not re-described. Phase 2's single-paragraph callback is preserved.

     [OVERLAP: Translational gap framing]
     Decision: Single narrative arc — gap identified (Ch 2) → gap addressed (Ch 4-5).
     Phase 1's "92% prototype" finding is cited once, not repeated.

     [OVERLAP: Usability barriers for older adults]
     Decision: Primary treatment moves to Problem Identification (Ch 3). Only framing-level
     references remain here. Wildenbos, Czaja, Berkowsky citations deduplicated.
-->

## 1.1 Background and Problem Context

The global population is aging, presenting challenges to healthcare systems worldwide \citep{worldhealthorganizationWHOClinicalConsortium2025}. Among the key health determinants for older adults is nutritional status. Malnutrition and sarcopenia are associated with increased morbidity, frailty, impaired cognitive function, and healthcare costs \citep{dentMalnutritionOlderAdults2023}. However, optimising nutrition in this demographic is complex; the nutritional needs of older adults are heterogeneous, driven by an interplay of physiological changes, multimorbidity, polypharmacy, and psychosocial factors \citep{kaurNutritionalInterventionsElderly2019}. Traditional "one-size-fits-all" dietary guidelines often fail to address this variability, necessitating a shift toward personalised, timely, and continuous monitoring strategies that are difficult to scale using manual clinical methods alone \citep{ordovasPersonalizedNutritionHealthy2020}.

Managing chronic conditions such as diabetes and sarcopenia in older adults requires accurate longitudinal dietary assessment \citep{shimDietaryAssessmentMethods2014,evertNutritionTherapyAdults2019}. The United States Department of Agriculture's Automated Multiple-Pass Method (AMPM) represents the clinical gold standard for reducing bias and improving the precision of energy intake reporting \citep{moshfeghUSDepartmentAgriculture2008}. However, such rigorous methods are inherently burdensome: they rely heavily on patient recall, subjective judgement of portion sizes, and significant administrative overhead, rendering continuous longitudinal tracking impractical for daily clinical care \citep{shimDietaryAssessmentMethods2014,willettNutritionalEpidemiology2012}. While alternative instruments such as the Weighed Food Record offer high accuracy, the cognitive and physical burden they impose creates significant friction, invariably leading to poor compliance outside of strictly controlled clinical trials \citep{burkeSelfMonitoringWeightLoss2011,cordeiroBarriersNegativeNudges2015,thompsonDietaryAssessmentMethodology2017}.

In this context, artificial intelligence (AI) has emerged as a tool for enabling precision nutrition \citep{alowaisRevolutionizingHealthcareRole2023}. AI systems offer the capability to integrate and analyse multimodal data streams---ranging from electronic health records and real-time wearable sensor data to dietary logs and food imagery \citep{tsolakidisArtificialIntelligenceMachine2024}. This computational power allows for the identification of non-linear patterns in nutritional decline and the delivery of personalised interventions \citep{kirkMachineLearningNutrition2022}. Yet despite this technological potential, a translational gap remains: there is limited evidence distinguishing purely algorithmic experiments from systems that have been operationally tested or deployed specifically within geriatric settings. Chapter 2 presents a scoping review that systematically maps this translational landscape.

<!-- [OVERLAP: RESOLVED] The detailed AI landscape description from Phase 1's introduction
     is NOT repeated here. Instead, a single forward reference to Chapter 2 replaces it.
     Phase 1's citations (theodorearmandApplicationsArtificialIntelligence2024,
     grootswagersArtificialIntelligenceNutrition2025) are preserved in Chapter 2. -->

## 1.2 The Friction-Fidelity Paradox

<!-- SOURCE: Phase 2 Introduction, paragraphs 2-3. This is Phase 2's core framing,
     now elevated to the thesis's central construct. [CORE NOVELTY: Friction-fidelity paradox
     as a design construct] -->

Existing digital health interventions present a binary trade-off that is ill-suited for the geriatric demographic. Manual logging offers high fidelity but imposes high friction through complex navigation and text entry, processes that conflict with age-related declines in motor skills and visual acuity \citep{wildenbosAgingBarriersInfluencing2018,czajaImpactAgingAccess2007,berkowskyFactorsPredictingDecisions2017}. Conversely, the prevailing alternative---passive computer vision---introduces a critical limitation regarding medical fidelity. While these systems significantly reduce friction by analysing food photos, they rely on single-shot inference that cannot account for "hidden variables" intrinsic to complex dietary intake \citep{minSurveyFoodComputing2019,zhangImagebasedMethodsDietary2024}. A static image cannot verify determining attributes---such as whether a pureed soup was thickened with heavy cream or vegetable stock, or quantify the specific volume of cooking oil absorbed during preparation. Without the ability to query these missing contexts, passive systems resort to probabilistic assumptions that degrade accuracy, preventing clinical utility \citep{loImageBasedFoodClassification2020}. This tension between minimising user friction and maximising clinical data fidelity constitutes the core *friction-fidelity paradox* addressed by this research.

## 1.3 Research Gap and Motivation

<!-- SOURCE: Phase 2 Introduction paragraph 3 + Phase 1 Introduction paragraph 3.
     The scoping review findings motivate the artifact design.
     [CITATION GAP: RESOLVED — consolidated thesis uses "Chapter 2" cross-reference directly.] -->

This technological limitation is substantiated by our scoping review of AI in geriatric nutrition (Chapter 2). Analysis of 25 eligible studies revealed that 92% of existing applications remain in prototype or pilot phases (median sample size $n=28$), often lacking operational validity. While the literature addresses visual recognition accuracy \citep{minSurveyFoodComputing2019,loImageBasedFoodClassification2020}, there is a demonstrated lack of deployed agentic AI workflows capable of self-auditing low confidence.

To bridge the gap between high-friction manual logging and low-fidelity passive vision, technological design must mimic the investigative rigour of clinical practice. Emerging "Agentic AI" architectures --- a term that, in the absence of a field-wide consensus definition, we operationalise here through converging characterisations in the recent literature \citep{xiRisePotentialLarge2025,qiuLLMbasedAgenticSystems2024,sumersCognitiveArchitecturesLanguage2023} as LLM-driven systems capable of autonomous reasoning, tool use, and iterative self-correction within a bounded task --- offer a digital equivalent to the structured, recurring interview approach of the AMPM. Rather than acting as a static classifier, an agentic system employs active reasoning to detect uncertainty and initiate a feedback loop---much like a dietitian probing a patient to "remember the milk" in their coffee \citep{tuConversationalDiagnosticArtificial2025,kuhnCLAMSelectiveClarification2023}. By shifting from static perception to dynamic reasoning, the system can apply suppression logic to minor ambiguities while actively intervening via conversational repair only when medically necessary, balancing data fidelity with user fatigue.

## 1.4 Research Questions

<!-- SOURCE: Phase 1 RQs become "Evidence Mapping" questions under the scoping review (Ch 2).
     Phase 2 RQs become "Artifact Design & Evaluation" questions.
     In the consolidated thesis, only Phase 2's high-level RQs appear in the introduction.
     Phase 1's 7 RQs are presented in Chapter 2 as the review's guiding questions. -->

This thesis proposes the design and evaluation of "Snap and Say," a multimodal agentic AI system that operationalises threshold-based routing to resolve the friction-fidelity paradox. The research is structured in two phases: (1) a scoping review that maps the translational landscape of AI in geriatric nutrition, and (2) a Design Science Research study that designs, develops, and evaluates an artifact informed by the review's findings.

The artifact's core contribution is a three-component architecture comprising (1) a *Deterministic Complexity Scoring Engine* that computes an auditable, non-linear complexity score from LLM-derived ambiguity levels and domain-curated food class metadata; (2) a *Clinical Threshold Routing Module* that compares this score against a configurable, per-session clinical threshold to determine whether clarification is warranted; and (3) a truncated adaptation of the USDA AMPM, bounded by a safety budget of two clarification rounds, that conducts targeted inquiry only when the clinical stakes justify the interaction cost.

We evaluate the efficacy of this system through three high-level research questions:

- **RQ1: Assessment Fidelity.** To what extent can an agentic architecture replicate the accuracy of clinical standards?
  - *Sub-question 1.1:* How does a multi-turn agentic photo-dialogue workflow affect per-meal mean absolute error (MAE) in estimated energy and macronutrients relative to a single-shot photo-only estimator?

- **RQ2: Agentic Efficiency.** How does the system mediate the trade-off between interaction friction and data precision?
  - *Sub-question 2.1:* How does confidence-gated clarification alter interaction friction and estimation accuracy relative to an always-clarify policy?

- **RQ3: Technical Feasibility.** Does the system performance meet the specific usability constraints of the geriatric population?
  - *Sub-question 3.1:* Do the turn-level and end-to-end latencies of the agentic self-correction workflow meet predefined acceptability thresholds for older adults \citep{nielsenUsabilityEngineering1994,nahStudyTolerableWaiting2004}?

## 1.5 Research Approach Overview

<!-- BRIDGING CONTENT — expanded by BR step (2026-03-10).
     Explains the two-phase structure and traces the argumentative thread. -->

The research follows a two-phase design that traces a continuous thread from evidence to architecture to evaluation.

**Phase 1** employs a scoping review methodology (PRISMA-ScR) to map the extent, range, and nature of evidence on deployed or operationally-tested AI systems in nutrition for aging populations. This evidence mapping serves a dual function: it establishes the state of the field as a contribution in its own right (Chapter 2), and it derives the design requirements that the subsequent artifact must satisfy. Specifically, the review's principal findings --- the 92% prototype stagnation rate, the usability mismatch with geriatric populations, the limitations of single-shot image analysis, and the absence of clinical routing mechanisms --- each map to a concrete design objective in Phase 2 (see Section 2.5 for the explicit gap-to-requirement mapping).

**Phase 2** adopts the Design Science Research Methodology \citep{peffersDesignScienceResearch2007} to systematically develop and evaluate the Snap and Say system. The DSR framework was selected for its explicit emphasis on producing evaluated artifacts that address identified problems --- a natural complement to the evidence-mapping function of the scoping review. Together, the two methodologies instantiate what this thesis terms the *Closed Loop Model*: Phase 1 establishes the baseline (what the field has achieved and where it falls short), Phase 2 defines metrics and designs a response (the artifact's six design objectives and their evaluation criteria), and the Discussion (Chapter 6) closes the loop by tracing the artifact's outcomes back to the original evidence gaps.

This two-phase structure is itself a methodological contribution: it demonstrates a replicable pattern for health informatics research in which systematic evidence mapping directly informs artifact design, ensuring that the gap between "what we know" and "what we build" is bridged explicitly rather than assumed.

<!-- [BRIDGE: Methodological paradigm justification]
     The combination of PRISMA-ScR (exploratory, landscape-mapping) with DSR
     (prescriptive, artifact-building) is justified by the maturity of the field:
     a field where 92% of systems are prototypes needs both a map of what exists
     and a principled method for building what's next. -->

## 1.6 Thesis Structure

The remainder of this thesis is organised as follows. Chapter 2 presents the scoping review of AI in geriatric nutrition, serving as the literature foundation. Chapter 3 articulates the Design Science Research methodology, including problem identification, design objectives, and evaluation strategy. Chapter 4 details the design and development of the Snap and Say artifact. Chapter 5 presents the demonstration and evaluation results. Chapter 6 discusses the implications, limitations, and future directions. Chapter 7 concludes the thesis.

---

<!-- MERGE ANNOTATIONS SUMMARY FOR CH 1:
     - [OVERLAP: Aging background] — Phase 1 broad framing condensed into 1.1 opening paragraph;
       Phase 2 sharper framing (AMPM, friction) follows naturally.
     - [OVERLAP: AI landscape] — Removed from introduction; forward-referenced to Ch 2.
     - [OVERLAP: Translational gap] — Single mention of 92% finding; points to Ch 2 for detail.
     - [OVERLAP: Usability barriers] — Single citation cluster in 1.2; full treatment deferred to Ch 3.
     - [CITATION GAP: Self-citation] — RESOLVED. Chapter cross-reference used (consolidated thesis).
     - Phase 1's 7 scoping review RQs moved to Chapter 2.
     - Phase 2's RQs preserved verbatim.
     - Section 1.5 is NEW bridging content (skeletal — BR step will expand).
-->
