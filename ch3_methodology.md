# Chapter 3: Methodology

<!-- MERGE NOTE: This chapter is Phase 2's Methodology sections (§2.1 and §2.2), verbatim.
     Sacred fabric — supervisor-approved content. Only framing adjustments:
     1. DSR framework introduction is slightly expanded (now chapter-level, not section-level)
     2. NEW section 3.5 (Ethical Considerations) added — skeletal, student must complete
     3. AMPM description: this is the PRIMARY treatment location.
        [OVERLAP: AMPM methodology description] — AMPM appears in Phase 2 Introduction,
        Problem Identification, and Artifact §3.4. In the consolidated thesis:
        - First full introduction here in 3.2.1
        - Brief references in Ch 1 (framing) and Ch 4 (implementation)
     4. Usability citations (Wildenbos, Czaja, Berkowsky) — primary treatment is here in 3.2.2.
        [OVERLAP: Usability barriers for older adults] — deduplicated across Ch 1 and Ch 3.
-->

## 3.1 Design Science Research Framework

<!-- BRIDGING — expanded by BR step (2026-03-10).
     Methodological paradigm justification: why PRISMA-ScR and DSR coexist. -->

The preceding chapter established the translational landscape of AI in geriatric nutrition through a scoping review, identifying systemic gaps in deployment readiness, usability, and clinical routing. This chapter shifts from *mapping what exists* to *prescribing what should be built*, adopting the Design Science Research (DSR) methodology to systematically develop and evaluate the "Snap and Say" system.

The combination of a scoping review (PRISMA-ScR) with DSR requires methodological justification, as the two paradigms serve different epistemic functions. The scoping review is exploratory and descriptive: it maps the extent and nature of evidence without prescribing solutions \citep{arkseyScopingStudiesMethodological2005}. DSR, by contrast, is prescriptive and constructive: it produces evaluated artifacts that address identified problems \citep{hevnerDesignScienceInformation2004}. Their complementarity lies in the maturity of the field. In an established domain, a systematic review might identify which *existing* intervention is most effective, obviating the need for new artifact design. But in a nascent field --- where 92% of systems remain at prototype stage and no deployed system implements clinical routing for dietary clarification --- the appropriate response is not to synthesise effectiveness evidence (which scarcely exists) but to map the landscape and then design a principled response to the gaps it reveals. The scoping review provides the *problem identification and motivation* that the DSR process model of Peffers et al. \citep{peffersDesignScienceResearch2007} requires as its first phase, grounding the artifact's design objectives in empirical evidence rather than assumption.

We utilise the Peffers process model, which structures the research into six phases: problem identification and motivation, objectives of a solution, design and development, demonstration, evaluation, and communication. This approach ensures the artifact addresses the practical needs of the geriatric population while remaining grounded in existing theories of multimodal learning and cognitive architectures \citep{hevnerDesignScienceInformation2004}. The following two sections detail the first two phases --- problem identification and objective specification --- which collectively define the requirements that the subsequent design and development phase must satisfy.

## 3.2 Problem Identification and Motivation

### 3.2.1 The Burden of Dietary Assessment

Accurate dietary assessment is a fundamental pillar of nutritional epidemiology and clinical management for chronic metabolic conditions, such as diabetes and cardiovascular disease \citep{willettNutritionalEpidemiology2012,evertNutritionTherapyAdults2019}. Traditionally, methods like the 24-hour dietary recall and food frequency questionnaires have been employed to capture nutritional data. The AMPM represents the gold standard in reducing bias and improving the precision of energy intake reporting \citep{moshfeghUSDepartmentAgriculture2008}. However, these rigorous methods are inherently burdensome. They rely heavily on patient recall, subjective judgment of portion sizes, and significant administrative overhead, rendering continuous longitudinal tracking impractical for daily clinical care \citep{shimDietaryAssessmentMethods2014}.

<!-- [OVERLAP: AMPM methodology description]
     This is now the FIRST full treatment of AMPM in the consolidated thesis.
     Ch 1 mentions AMPM as "gold standard" without explanation.
     Ch 4 (Artifact §3.4) describes the software adaptation — references back here. -->

With the ubiquity of smartphones, mobile food journaling applications have emerged as a scalable alternative to traditional recall methods. Despite their proliferation, these digital tools suffer from notoriously high attrition rates. A primary driver of this abandonment is the high cognitive and manual burden required to explicitly log every food item, search extensive databases, and estimate portion sizes. Cordeiro et al. \citep{cordeiroBarriersNegativeNudges2015} identified that the constant demands of manual food tracking function as "negative nudges," inducing fatigue and subsequently leading to lapses in journaling. This adherence challenge is particularly pronounced among older adults, a demographic disproportionately affected by chronic conditions requiring dietary management, but who simultaneously face age-related cognitive, perceptual, and motivational barriers to complex mobile health (mHealth) interaction \citep{wildenbosAgingBarriersInfluencing2018,czajaImpactAgingAccess2007}. Consequently, there is an urgent need to transition from active, manual dietary logging to passive, low-friction modalities.

### 3.2.2 Conversational Interfaces and the Ambiguity Problem

To mitigate the friction of manual entry, Voice User Interfaces (VUIs) and large language model (LLM) conversational agents have been proposed as natural, accessible modalities for dietary logging \citep{tuConversationalDiagnosticArtificial2025,sumersCognitiveArchitecturesLanguage2023}. By allowing users to report meals in natural, unstructured language, conversational systems significantly lower the barrier to technological adoption for demographics typically resistant to complex applications \citep{berkowskyFactorsPredictingDecisions2017}. Furthermore, combining multimodal inputs, such as speech and image-based food volume estimation, presents a promising frontier in comprehensive dietary assessment \citep{zhangImagebasedMethodsDietary2024,loImageBasedFoodClassification2020}.

However, the shift toward unstructured voice input introduces a critical new challenge: semantic and nutritional ambiguity. A patient spontaneously reporting ingestion of a "hamburger" omits critical constituent parameters (e.g., bun composition, ad hoc condiment inclusion), yielding unacceptably wide confidence intervals for macronutrient extraction.

While advanced dialogue systems can selectively prompt users to clarify ambiguous questions \citep{kuhnCLAMSelectiveClarification2023,yaoReActSynergizingReasoning2022}, applying this capability naively to dietary logging replaces manual data-entry fatigue with *prompt fatigue*. Conversely, applying "probabilistic silence"---where the agent never asks for clarification and merely guesses the missing data---severely degrades the medical fidelity of the captured record. The clinical tolerance for ambiguity is not uniform; a missing condiment may be trivial for a generally healthy user, but the carbohydrate composition of a sauce is critical for a patient managing Type 1 Diabetes.

### 3.2.3 Motivation for the Artifact

The tension between minimising user friction (reducing conversational turns) and maximising clinical data fidelity (resolving nutritional ambiguities) forms the core problem addressed by this Design Science Research. Existing conversational agents lack deterministic mechanisms to dynamically weigh the clinical necessity of a clarifying question against the usability cost of interrupting the user \citep{xiRisePotentialLarge2025,myersPatternsHowUsers2018}.

Crucially, the technological viability of solving this paradox has only recently emerged. The advent of advanced reasoning capabilities in Large Language Models, particularly through techniques like Chain-of-Thought prompting \citep{weiChainofthoughtPromptingElicits2022} and interleaved reasoning-and-acting frameworks like ReAct \citep{yaoReActSynergizingReasoning2022}, allows systems to evaluate complex preconditions before executing dialogue turns.

Therefore, there is a motivated need for an intelligent routing architecture capable of executing *clinical threshold routing*. This research proposes the design and development of a Deterministic Complexity Scoring Engine. The intended artifact will programmatically evaluate the semantic complexity and nutritional ambiguity of an ingested dietary log and compare it against a personalised clinical threshold. Only when the complexity of a meal surpasses the patient-specific tolerance will the system route the user into a targeted, LLM-driven AMPM detail clarification cycle.

By operationalising this threshold-based routing, the artifact aims to resolve the core friction-fidelity paradox.

## 3.3 Objectives of a Solution

### 3.3.1 From Problem Space to Design Requirements

The preceding section established the **friction-fidelity paradox** as the core problem. This section articulates the design objectives that a solution artifact must satisfy to resolve this paradox. Following Peffers et al. \citep{peffersDesignScienceResearch2007}, the objectives are derived rationally from the problem specification and informed by the knowledge of what is feasible within the current technological landscape.

The overarching design goal is the construction of an intelligent routing architecture---a **Deterministic Complexity Scoring Engine** coupled with a **Clinical Threshold Routing Module**---that dynamically calibrates the depth of agent-driven dietary clarification to the clinical stakes of each individual meal entry.

### 3.3.2 Design Objectives

**Objective 1: Quantifiable Complexity Assessment.** The artifact shall compute a deterministic, auditable complexity score for each dietary log entry. The scoring mechanism must follow a transparent mathematical formula whose inputs and weights are inspectable by researchers and clinicians. The score shall integrate (a) LLM-derived ambiguity assessments across orthogonal dimensions of nutritional uncertainty with (b) domain-encoded risk metadata. The scoring function must produce a non-linear penalty curve. This design objective addresses the need for clinical accountability.

**Objective 2: Personalised Clinical Threshold Routing.** The artifact shall support configurable, per-session clinical thresholds that govern routing decisions. A single, fixed confidence gate is insufficient for heterogeneous patient populations. The routing module must accept a tuneable threshold parameter $\tau$ that clinicians or researchers can set per user profile.

**Objective 3: Mandatory Override for Taxonomically Ambiguous Foods.** Certain food categories exhibit irreducible taxonomic ambiguity that no confidence score can safely absorb. The artifact shall incorporate a mandatory clarification mechanism, triggered by domain-curated metadata in the food classification registry, that unconditionally routes such items into the clarification subgraph.

**Objective 4: Bounded Clarification to Preserve Usability.** The system shall enforce a safety budget limiting the maximum number of clarification rounds per log entry. Once this budget is exhausted, the system must finalise the log and flag the entry for manual clinical review rather than continuing to interrogate the user \citep{nahStudyTolerableWaiting2004,hartDevelopmentNASATLXTask1988}.

**Objective 5: Semantic Ambiguity Resolution Before Quantitative Clarification.** The artifact shall ensure that semantic ambiguity is resolved categorically before probabilistic nutritional clarification is attempted \citep{kuhnCLAMSelectiveClarification2023,yaoReActSynergizingReasoning2022}.

**Objective 6: Multimodal Input Fusion.** The artifact shall process concurrent image and voice inputs to maximise the information available for complexity assessment \citep{loImageBasedFoodClassification2020,zhangImagebasedMethodsDietary2024}. This objective is particularly relevant for older adult populations, who may prefer voice interaction over manual text entry but simultaneously benefit from visual evidence that compensates for recall bias \citep{wildenbosAgingBarriersInfluencing2018,moshfeghUSDepartmentAgriculture2008}.

### 3.3.3 Relationship to DSR Knowledge Base

These objectives collectively define a **Type 5 (Design and Action) theory** in Gregor's \citep{gregorNatureTheoryInformation2006} taxonomy. The objectives are grounded in Hevner et al.'s \citep{hevnerDesignScienceInformation2004} dual imperatives of **rigour** (Objectives 1, 3, and 5) and **relevance** (Objectives 2, 4, and 6).

## 3.4 Evaluation Strategy

<!-- NEW framing section — brief overview of the mixed-method evaluation approach.
     Details are in Chapter 5. This provides the methodological justification. -->

The artifact was evaluated using a mixed-methods approach structured by the FEDS framework \citep{venableFEDSFrameworkEvaluation2016}. The evaluation comprised two complementary strategies: (1) simulated benchmarking using an automated Oracle protocol against a stratified subset of the Nutrition5k dataset \citep{thamesNutrition5kAutomaticNutritional2021} to quantify baseline nutritional accuracy and routing efficiency, and (2) a human-in-the-loop "Digital Buffet" protocol involving older adult participants ($N=19$, Age $\geq 65$) to assess usability, perceived task load, and system latency. Chapter 5 presents the full evaluation methodology and results.

With the problem identified, the objectives specified, and the evaluation strategy outlined, the research proceeds to the third phase of the DSR process model: design and development. Chapter 4 describes the artifact that operationalises these six design objectives into a functioning system.

## 3.5 Ethical Considerations

<!-- NEW SECTION — addresses [GAP: Ethical considerations thin across both phases]
     This is skeletal — student must complete with:
     - IRB/ethics committee approval details
     - Informed consent procedure
     - Data protection measures (de-identified auth, managed ephemerality)
     - HIPAA/GDPR considerations
     [CITATION GAP: Privacy/regulatory compliance — no HIPAA, GDPR citations yet] -->

The research involved human participants (older adults aged 65 and over) and the processing of dietary data, necessitating careful attention to ethical governance.

**Informed consent.** All participants provided written informed consent prior to participation in the Digital Buffet evaluation. The consent form (Appendix G) described the study procedures, the voluntary nature of participation, and the right to withdraw at any time without consequence.

**De-identified authentication.** The system implements a de-identified anonymous sign-in pattern. Each participant receives a persistent UUID with no associated personally identifiable information, ensuring that dietary data cannot be traced to individual participants.

**Managed ephemerality.** While the deterministic analysis results are retained for research purposes, raw audio and image files are subject to managed ephemerality---retained only as long as necessary for clinical review, then purged to mitigate long-term re-identification risks.

**[TODO: Student to add IRB/ethics committee approval reference, GDPR/privacy regulation citations, and any institutional review details]**

---

<!-- MERGE ANNOTATIONS SUMMARY FOR CH 3:
     - Phase 2 Methodology (§2.1 + §2.2) preserved as sacred fabric
     - [OVERLAP: AMPM description] — first full treatment here in 3.2.1. Ch 1 and Ch 4
       reference back to this section. Prevents triple-definition.
     - [OVERLAP: Usability barriers] — primary treatment here in 3.2.1-3.2.2.
       Ch 1 retains framing-level citation only.
     - NEW section 3.4 (Evaluation Strategy) — brief methodological overview
     - NEW section 3.5 (Ethical Considerations) — skeletal, needs student completion
     - [CITATION GAP: HIPAA/GDPR] — flagged in 3.5 for student action
     - No content deleted from Phase 2 source
-->
