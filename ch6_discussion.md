# Chapter 6: Discussion

<!-- MERGE NOTE: This chapter uses Phase 2's Discussion as the primary fabric.
     Phase 2's Discussion is full academic prose (7 limitation subsections, 3 literature
     departures, 6 future directions) — sacred fabric.

     The consolidation task is NOT to rewrite the discussion but to:
     1. Preserve Phase 2's discussion content
     2. Add annotations for where Phase 1 citations need to be woven in (Closed Loop)
     3. Add NEW section 6.3 (Closing the Loop) — skeletal bridging content
     4. Expand 6.5 to include Phase 1's methodological limitations

     [GAP: Phase 2 Discussion does not bring back Phase 1's 25 studies — Closed Loop violation]
     [GAP: Gap-to-design bridges still implicit]
     These gaps are flagged inline; the BR and CL steps will draft the connective tissue.
-->

The evaluation presented in Chapter 5 provides converging evidence---from simulated benchmarking and human-in-the-loop trials---that the Snap and Say artifact meaningfully addresses the friction-fidelity paradox identified in Section 3.2. This section interprets those findings against the six design objectives, situates the contribution within the broader literature on conversational dietary assessment, identifies methodological and architectural limitations, and outlines directions for future research.

## 6.1 Interpretation of Findings

### 6.1.1 The Routing Architecture Addresses the Paradox

<!-- [CL: Claims calibration applied. Section title softened from "Resolves the Paradox" to
     "Addresses the Paradox" — the cross-sectional evaluation provides evidence of utility,
     not definitive resolution. Hedged language inserted throughout.] -->

The central claim of this thesis is that deterministic complexity scoring, coupled with clinical threshold routing, can dynamically balance interrogation depth against user burden. The evaluation provides initial evidence supporting this claim along two independent axes. First, the simulated benchmarking demonstrated that agentic clarification cycles reduced caloric estimation MAE by 30.6% on Complex items while introducing only a marginal 3.2% improvement on Simple items---the asymmetric behaviour the routing architecture was designed to produce. The system intervenes where intervention yields clinical value and remains silent where it does not. Second, the suppression logic achieved a 92% True Negative Rate on Simple items, confirming that the threshold-gated deferral mechanism (Section 4.4.1) successfully prevents unnecessary questioning. Taken together, these metrics indicate that the artifact does not merely reduce error in aggregate; it allocates its analytical effort in proportion to the epistemic uncertainty of each meal, which is the operational definition of managing the friction-fidelity paradox.

The human-in-the-loop evaluation reinforces this interpretation from the user's perspective. Older adult participants ($N=19$, Age $\geq 65$) reported low perceived complexity and temporal demand, and the agentic clarification interruption was generally received as helpful rather than intrusive. That participants did not perceive the system as cumbersome---despite the Complex meal triggering an active clarification cycle---suggests that the bounded interrogation strategy (Objective 4, $N_{max}=2$) and the conversational framing succeed in distributing cognitive load into tolerable increments.

### 6.1.2 Objective-Level Assessment

**Objective 1 (Quantifiable Complexity Assessment).** The deterministic scoring formula $C = \sum w_d L_d^2 + P_{\text{sem}}$ produced auditable, reproducible scores that governed routing decisions throughout both evaluation phases. The quadratic penalty curve ensured that genuinely ambiguous meals (level 2--3 ambiguity) were disproportionately flagged, while low-ambiguity entries passed through without intervention. The empirical threshold calibration (Section 5.1.1) further validated the formula's discriminative power: the optimal configuration ($C_{\text{thresh}}=15.0$, $\text{Conf}_{\text{thresh}}=0.85$) was identified through systematic parameter sweep rather than ad hoc tuning, grounding the scoring mechanism in empirical trade-off analysis.

**Objective 2 (Personalised Clinical Threshold Routing).** The configurable threshold $\tau$ was operationalised and tested at its default value ($\tau=15.0$). While the evaluation did not include cohort-specific threshold variation (e.g., $\tau=5.0$ for diabetic users), the architecture demonstrably supports per-session parameterisation. The clinical threshold calibration experiment confirmed that the routing boundary is not arbitrary but corresponds to a Pareto-optimal point in the friction-fidelity trade-off space.

**Objective 3 (Mandatory Override).** The Food Class Registry's mandatory clarification flags functioned as designed in the simulated benchmarking, unconditionally routing taxonomically ambiguous items (e.g., meat analogues) into the clarification subgraph regardless of score. This mechanism prevented the system from silently misclassifying nutritionally divergent food analogues---a failure mode with direct clinical consequences.

**Objective 4 (Bounded Clarification).** The safety budget ($N_{max}=2$) was enforced throughout both evaluation phases. No participant in the human trials was subjected to more than two clarification rounds, and items exceeding the budget were flagged for manual review rather than generating additional questions. The qualitative feedback contained no complaints about excessive questioning, providing indirect validation that the budget is appropriately calibrated for the target demographic.

**Objective 5 (Semantic Before Quantitative).** The Semantic Gatekeeper node (Section 4.1.3) enforced categorical resolution before quantitative clarification as a topological invariant of the state graph. In the simulated benchmarking, this ordering prevented wasted clarification cycles on items whose base identity remained unresolved---a structural guarantee that cannot be achieved through prompt engineering alone.

**Objective 6 (Multimodal Input Fusion).** The human-in-the-loop evaluation confirmed that older adults could successfully operate the multimodal interface (camera and voice), with survey responses indicating high learnability and confidence. However, the qualitative data revealed that when the acoustic modality failed (microphone issues), the interaction degraded substantially, exposing a dependency on reliable hardware that the current architecture does not fully mitigate.

## 6.2 Contextualisation Within the Literature

The Snap and Say artifact contributes to a growing body of work on AI-mediated dietary assessment, but departs from existing approaches in several consequential ways.

First, whereas prior image-based dietary assessment systems \citep{loImageBasedFoodClassification2020,zhangImagebasedMethodsDietary2024} operate as single-pass classifiers---accepting an image and returning a nutrient estimate without recourse to the user---Snap and Say introduces a conditional feedback loop. The system's ability to selectively request clarification when visual evidence is insufficient distinguishes it from purely passive estimation pipelines. The 30.6% MAE reduction on Complex items quantifies the information gain that targeted clarification provides over unimodal computer vision, a finding consistent with the selective clarification principle articulated by Kuhn et al. \citep{kuhnCLAMSelectiveClarification2023} but applied here to a clinical dietary domain rather than general-purpose dialogue.

Second, the deterministic complexity scoring engine addresses a gap identified in the broader literature on LLM-based clinical agents. Xi et al. \citep{xiRisePotentialLarge2025} and Sumers et al. \citep{sumersCognitiveArchitecturesLanguage2023} have documented the potential of large language models as cognitive architectures, but both note the absence of formal mechanisms for bounding generative autonomy in safety-critical domains. The $C = \sum w_d L_d^2 + P_{\text{sem}}$ formula operationalises such a mechanism: it imposes a mathematically transparent gate on the LLM's licence to interrogate the user, ensuring that the system's conversational behaviour is governed by auditable arithmetic rather than opaque token probabilities. This separation of deterministic control logic from probabilistic generation aligns with emerging best practices for deploying LLMs in healthcare settings, where explainability and reproducibility are non-negotiable requirements.

Third, the adaptation of the USDA AMPM \citep{moshfeghUSDepartmentAgriculture2008} into a software agent architecture represents a novel translational contribution. The traditional AMPM is a clinician-administered protocol requiring trained interviewers; by encoding its structured inquiry logic into a directed state graph with explicit budget constraints, this research demonstrates that established nutritional assessment methodologies can be partially automated without discarding their epistemic structure. The truncation to two clarification rounds is a deliberate departure from the five-step AMPM, motivated by the distinct cognitive constraints of unsupervised, self-administered use by older adults---a population for whom each additional conversational turn carries disproportionate burden \citep{wildenbosAgingBarriersInfluencing2018}.

<!-- [CL STEP — CLOSED LOOP CITATION AUDIT (2026-03-10)]
     The following paragraphs weave Phase 1's scoping review studies back into the
     Contextualisation section, closing the argumentative loop from evidence mapping
     to artifact evaluation. Each paragraph connects a specific Phase 1 finding to
     the artifact's design response and evaluation outcome. -->

Fourth, the artifact's multimodal approach extends beyond the image-only paradigm that dominated the reviewed systems. Papathanail et al. \citep{papathanailEvaluationNovelArtificial2021} achieved 11.64% mean relative error in energy estimation using RGB-D imaging with depth sensors, but acknowledged that the burden of obtaining ground-truth data through weighed food records was prohibitive for routine use. Snap and Say's agentic architecture addresses this limitation by replacing the static ground-truth requirement with a dynamic clarification protocol: when the system detects ambiguity, it solicits the information it lacks from the user rather than requiring pre-computed reference data. The evaluation's 30.6% MAE reduction on Complex items quantifies the information gain of this strategy, though direct metric comparison with Papathanail et al. is precluded by the different error measures used (MAE vs. MRE) and the distinct evaluation contexts (RGB-D laboratory imaging vs. unconstrained smartphone photography).

Fifth, the usability findings from the Digital Buffet evaluation speak directly to the barriers documented in the scoping review. Balsa et al. \citep{balsaUsabilityIntelligentVirtual2020} reported that older adults with diabetes found an intelligent virtual assistant difficult to use due to small interface elements and repetitive questioning patterns --- precisely the failure modes that Snap and Say's bounded clarification strategy ($N_{max}=2$) and Tap-to-Toggle interaction model were designed to prevent. The evaluation provides initial evidence that these design responses are effective: participants reported low perceived complexity and high learnability, and the agentic clarification interruption was received as helpful rather than intrusive (Section 5.3). The residual friction identified by participants --- latency and occasional microphone failures --- is architectural rather than interaction-design-related, suggesting that the usability barriers documented in prior work have been substantively addressed at the design level.

<!-- [CL ANNOTATION: Chopra et al. contextualises LLM application extension from coaching to assessment.
     Claims CALIBRATED: "extends" rather than "improves upon" — different application domains prevent direct comparison.] -->

Sixth, Chopra et al. \citep{chopraNutritionistAIAgent2024} demonstrated the feasibility of LLM-based conversational agents for dietary guidance in a randomised controlled trial, reporting statistically significant improvements in dietary quality among users of an AI coaching agent. Snap and Say extends the conversational AI approach from *coaching* (advising users what to eat) to *assessment* (determining what users have already eaten), adding the deterministic routing layer absent from their design. The conceptual link is significant: both systems leverage conversational AI to reduce friction in dietary interaction, but they operate at different points in the dietary cycle. Chopra et al.'s positive clinical outcomes at scale ($N=308$) suggest that the conversational modality itself is viable for sustained dietary engagement --- a promising indicator for Snap and Say's longitudinal deployment aspirations, though the distinct application contexts preclude direct efficacy comparison.

Seventh, the review identified that computer vision systems for dietary assessment faced fundamental limitations in food segmentation. Pfisterer et al. \citep{pfistererEnhancingFoodIntake2022} demonstrated that CV models struggled to segment low-density or stacked foods in long-term care settings, exposing an inherent information ceiling of image-only approaches. The artifact's agentic architecture was designed precisely to address this ceiling: the conditional feedback loop triggers targeted clarification for visually ambiguous items --- the "hidden ingredients" that pure CV cannot detect. The evaluation's asymmetric improvement profile (30.6% MAE reduction on Complex items vs. 3.2% on Simple) provides empirical evidence that the agentic clarification protocol recovers information that image-only approaches leave on the table.

Finally, two threads from the scoping review situate the artifact's scope and aspirations. Di Martino et al. \citep{dimartinoMalnutritionRiskAssessment2021} developed one of only two operationally tested systems in the review --- a malnutrition detection model deployed within long-term care. Snap and Say targets community-dwelling older adults, and the LTC gap identified in the evidence map (Section 2.3) remains unaddressed (Section 6.3.5). Seok et al. \citep{seokContinuousGlucoseMonitoring2022} provided the only evidence of clinical impact sustained over time, demonstrating that continuous monitoring with AI feedback could alter health outcomes. Snap and Say's longitudinal deployment --- the most consequential future direction identified in Section 6.5 --- aspires to generate similar evidence, but such validation lies beyond the scope of this thesis's cross-sectional evaluation.

## 6.3 Closing the Loop: From Evidence Gaps to Artifact Validation

<!-- BRIDGING SECTION — expanded by BR step (2026-03-10), enriched by CL step (2026-03-10).
     Traces the argumentative thread from evidence gaps (Ch 2) through design responses (Ch 3-4)
     to evaluation outcomes (Ch 5). Phase 1 studies have been woven into §6.2 above;
     this section traces the gap → design → evidence threads at the structural level. -->

The two-phase structure of this thesis embodies a deliberate epistemological commitment: that the gap between evidence and artifact should be bridged explicitly, not assumed. Section 2.5 mapped the review's findings to the artifact's design objectives; this section traces those threads forward through the evaluation outcomes, assessing the degree to which the artifact addresses the gaps it was designed to close.

### 6.3.1 The Translational Gap: From Prototype Stagnation to Deployment Readiness

The review found that 92% of systems ($n=23$) remained at prototype or pilot stages, with only two systems --- an operationally tested malnutrition detection system in long-term care \citep{dimartinoMalnutritionRiskAssessment2021} and an evaluation of a commercially available LLM for dietary guidance \citep{wangAreNutritionalLabeling2024} --- advancing beyond preliminary development. This stagnation was attributed to a confluence of factors: small sample sizes, limited real-world evaluation, and isolated architectures disconnected from clinical workflows.

Snap and Say was designed with deployment consciousness: a cloud-native architecture (progressive web application, managed relational database, platform-hosted API server), configurable clinical parameters ($\tau$) adjustable without code changes, and a streaming event model for real-time feedback. The evaluation demonstrated that the system functions end-to-end with real users ($N=19$) in a controlled setting. However, an honest assessment reveals that the artifact has not yet escaped the prototype trap it was designed to transcend: the evaluation was cross-sectional, the sample was culturally homogeneous, and the Digital Buffet protocol --- with its monitor-displayed images --- does not constitute ecological deployment. The thesis thus advances the *readiness* for deployment without yet achieving it, a distinction that the field's maturity trajectory demands be stated plainly.

<!-- [BRIDGE ANNOTATION: This paragraph acknowledges the tension identified in DF-002.
     The thesis designed for deployment but is still a prototype. Honest. ] -->

### 6.3.2 The Usability Mismatch: From Age-Related Barriers to Validated Acceptance

The review documented systematic usability failures across the reviewed systems. Balsa et al. \citep{balsaUsabilityIntelligentVirtual2020} reported that older adults with diabetes found an intelligent virtual assistant difficult to use due to small interface elements and repetitive questioning patterns. More broadly, the review found that few systems embedded user-centric or co-design methodologies (Section 2.4), resulting in technologies that were functional but inaccessible to their intended demographic.

The artifact addressed this gap through three specific design responses: (1) the Tap-to-Toggle voice capture model, which eliminates the need for sustained fine motor control (Section 4.6); (2) the bounded clarification strategy ($N_{max}=2$), which prevents the "repetitive questions" pattern directly cited as a usability failure in the reviewed literature; and (3) redundant haptic and visual feedback channels to accommodate sensory decline. The Digital Buffet evaluation provides initial evidence that these responses are effective: older adult participants reported low perceived complexity and high learnability, and the agentic clarification interruption was received as helpful rather than intrusive (Section 5.3). The qualitative feedback confirmed that the *system* was not perceived as the barrier --- residual friction arose from latency and acoustic hardware failures rather than interaction design.

<!-- [BRIDGE ANNOTATION: Gap → Design → Evidence thread closed for usability.
     Residual friction (latency, microphone) is architectural, not design-philosophical.
     This distinction matters for examiners assessing whether the contribution is valid. ] -->

### 6.3.3 From Single-Shot Limitation to Agentic Clarification

The dietary assessment systems in the review ($n=6$) uniformly employed single-pass inference: a photograph enters the system, a nutrient estimate exits, and the user has no opportunity to correct or supplement the analysis. Papathanail et al. \citep{papathanailEvaluationNovelArtificial2021} achieved 11.64% mean relative error in energy estimation using RGB-D imaging, but acknowledged that the burden of obtaining ground-truth data through weighed food records was prohibitive for routine use. Pfisterer et al. \citep{pfistererEnhancingFoodIntake2022} demonstrated that CV models struggled to segment low-density or stacked foods, highlighting the inherent information ceiling of image-only approaches.

The artifact's agentic architecture directly addresses this ceiling. The conditional feedback loop --- triggered only when the complexity score $C$ exceeds the clinical threshold $\tau$ --- resolves precisely the "hidden ingredient" ambiguities that single-shot systems cannot capture. The evaluation quantifies the information gain: the 30.6% MAE reduction on Complex items represents the clinical value of targeted clarification, while the 92% True Negative Rate on Simple items demonstrates that this value is achieved without imposing burden on unambiguous entries. The asymmetric improvement profile (substantial on Complex, marginal on Simple) is the empirical signature of a routing architecture that allocates effort in proportion to epistemic uncertainty.

### 6.3.4 From Data Opacity to Transparent Evaluation

The review found that 84% of studies relied on private datasets, impeding reproducibility and cross-system comparison. The artifact was evaluated against the publicly available Nutrition5k dataset \citep{thamesNutrition5kAutomaticNutritional2021}, ensuring that the reported metrics are independently verifiable. However, this decision introduces its own limitations: the Nutrition5k corpus encodes Western dietary patterns and controlled photography conditions, and the threshold calibration was performed on the same dataset used for evaluation (a circularity acknowledged in Section 6.4.7). The system's data layer (a managed relational database) stores user-generated dietary records privately, meaning the artifact partially replicates the data opacity pattern it set out to address. Future work should explore privacy-preserving data sharing strategies that balance clinical data governance with the open science imperatives identified in the review.

<!-- [TRANSLATED: "Supabase PostgreSQL" → "managed relational database" to maintain
     architectural-concept framing consistent with Ch 4 translations.]
     [PRIVACY GAP: The tension between data transparency and privacy protection
     could benefit from specific regulatory citations (GDPR, HIPAA) that frame
     why private storage is necessary and what governance framework applies.] -->

<!-- [BRIDGE ANNOTATION: DF-007 and DF-019 addressed. The thesis uses public data for
     evaluation but stores user data privately. Acknowledged honestly. ] -->

### 6.3.5 The Long-Term Care Gap: An Honest Limitation

The evidence gap map (Section 2.3) identified the absence of behaviour change and recommendation interventions in long-term care settings as a notable void. Snap and Say was designed for community-dwelling older adults --- a deliberate scoping decision driven by the DSR framework's emphasis on a well-defined problem context (Section 3.2). The LTC gap identified in Phase 1 remains unaddressed by this thesis. Extending the artifact to institutional settings would require adaptation for caregiver-mediated use, integration with facility meal management systems, and validation with populations exhibiting higher prevalences of cognitive impairment. This remains a clear future direction that the thesis's evidence base has identified but its artifact has not yet reached.

<!-- [BRIDGE ANNOTATION: DF-008 and DF-020 addressed. The LTC gap is cleanly acknowledged.
     The thread from evidence gap to honest limitation is traceable. ] -->

## 6.4 Limitations

Despite the encouraging results, several limitations constrain the generalisability of the findings.

### 6.4.1 Sample Size and Demographic Scope

The human-in-the-loop evaluation recruited $N=19$ participants, all aged 65 or older and residing in the Netherlands. While sufficient for formative usability assessment within a DSR framework \citep{venableFEDSFrameworkEvaluation2016}, this sample size precludes inferential statistical analysis and limits the external validity of the usability findings. The cultural homogeneity of the sample further restricts generalisability: dietary vocabularies, food preparation conventions, and meal structures vary substantially across populations, and the system's performance on culturally diverse dietary inputs remains untested.

### 6.4.2 Oracle Simulation Versus Real-World Interaction

The simulated benchmarking employed an Oracle that supplied ground-truth metadata in response to the agent's clarification questions. This design isolates the system's analytical reasoning from human behavioural variability but simultaneously represents a best-case scenario. In practice, users may provide incomplete, contradictory, or irrelevant responses to clarification prompts, and the system's ability to recover gracefully from such inputs was not evaluated. The 22.8% overall MAE reduction and 78% True Positive Rate on Complex items should therefore be interpreted as upper-bound performance estimates under ideal cooperative conditions.

### 6.4.3 Cultural and Lexical Ambiguity

The current Food Class Registry and the LLM's training data encode predominantly Western dietary taxonomies. The semantic referent of a food term is culturally contingent: "regular coffee" implies milk and sugar in parts of the United States but denotes black coffee elsewhere; "biscuit" maps to radically different macronutrient profiles in British versus American English. The deterministic scoring engine treats food names as stable referents once resolved by the Semantic Gatekeeper, but this assumption is fragile in multilingual or multicultural deployment contexts. Extending the Food Class Registry with locale-specific aliases and cultural modifiers is a necessary prerequisite for broader deployment.

### 6.4.4 Intake Versus Serving

A fundamental limitation of the current architecture is its inability to distinguish between food served and food consumed. The system may accurately identify and quantify every component of a complex meal, yet the user may consume only a fraction of the plate. This "plate waste" problem is a recognised challenge in dietary assessment \citep{moshfeghUSDepartmentAgriculture2008} that lies outside the scope of the current artifact's sensing modalities. Addressing this limitation would require either post-meal visual confirmation (e.g., a photograph of the remaining plate) or integration with wearable sensing technologies capable of estimating actual intake.

### 6.4.5 Latency Constraints

The average 7.1-second round-trip for the full agentic cycle, while within the tolerance range for a research prototype \citep{nahStudyTolerableWaiting2004}, was explicitly noted by participants as a source of friction. The multi-stage pipeline---comprising image retrieval, audio transcription, LLM inference, complexity scoring, and potential clarification generation---introduces cumulative latency that compounds with each clarification round. For production deployment, optimisation of the execution layer, parallelisation of independent pipeline stages, and migration to lower-latency model endpoints would be required to meet the expectations surfaced in qualitative feedback.

### 6.4.6 Provider Coupling and Model Volatility

<!-- [TRANSLATED: "Gemini 3.0 Flash Preview" → identified as a specific foundational model version.
     Rationale: The specific model is a vehicle; the observation about provider coupling is the contribution.
     The brand name is preserved here because identifying the specific model version is essential
     for reproducibility — this is a LIMITATION section, not a design claim.] -->

The artifact supports two LLM providers---Google Gemini Flash (the default) and OpenAI GPT-4o (an explicit alternative)---selectable per session via configuration or runtime request parameters. While this dual-provider architecture avoids hard-coded dependence on a single vendor, the system does not implement automatic failover: if the configured provider becomes unavailable during a session, the pipeline fails rather than transparently switching to the alternative. Moreover, the benchmarking and threshold calibration reported in Chapter 5 were conducted against a single foundational model version (Gemini 3.0 Flash Preview, with prompt variant v4 optimised for that model). The reproducibility of the reported metrics under model updates, provider-side behavioural changes, or when operating under the GPT-4o alternative remains unverified. The architectural separation between deterministic scoring and probabilistic generation partially mitigates this risk---the scoring formula $C = \sum w_d L_d^2 + P_{\text{sem}}$ is model-agnostic---but the quality of the LLM-derived ambiguity levels that feed into that formula is inherently model-dependent, and prompt optimisations tuned for one provider may not transfer to another without recalibration.

### 6.4.7 Threats to Validity

**Internal validity.** The simulated benchmarking controlled for human variability through the Oracle protocol, but the stratification of the Nutrition5k subset into Simple and Complex categories relied on researcher judgement. Alternative stratification criteria could yield different performance profiles. Additionally, the threshold calibration was performed on the same dataset used for evaluation, introducing a risk of overfitting the routing parameters to the specific characteristics of the Nutrition5k corpus.

**External validity.** The Digital Buffet protocol presented high-resolution images on a monitor rather than photographs of the participants' own meals. This controlled environment eliminates variability in image quality, lighting, and food presentation that would be present in naturalistic use. The usability findings may therefore overestimate real-world performance, where suboptimal image capture is a likely occurrence.

**Construct validity.** The adapted SUS/NASA-TLX instrument was not independently validated for the specific context of conversational dietary assessment. While the constituent scales are well-established, their combination and the specific item wordings may not fully capture the nuances of the interaction, particularly the distinction between friction imposed by the system and friction inherent to the dietary logging task itself.

### 6.4.8 Methodological Limitations of the Scoping Review

<!-- NEW — Phase 1's limitations integrated into the consolidated discussion.
     [MERGE: Phase 1 Discussion, final paragraph — methodological limitations] -->

The scoping review (Chapter 2) is subject to its own methodological limitations that propagate into the artifact's design rationale. The restriction to English-language publications may have excluded relevant systems from non-Anglophone regions, particularly Asian countries where gerontechnology is advancing rapidly. The exclusion of grey literature likely omits the most recent commercial prototypes. Primary screening was conducted by a single reviewer, which may have introduced selection bias. No formal critical appraisal of study quality was performed, consistent with scoping review methodology. These constraints mean that the design requirements derived from the review (Section 2.5) may not fully capture the global state of AI in geriatric nutrition.

## 6.5 Future Directions

The limitations identified above suggest several avenues for future research, organised by proximity to the current artifact.

**Longitudinal deployment.** The most pressing next step is a longitudinal field study in which participants use the system for daily dietary logging over a period of weeks. Such a study would test the critical hypothesis that the low-friction interaction model translates into sustained adherence---the ultimate clinical metric that the current cross-sectional evaluation cannot address.

**Cohort-specific threshold validation.** The configurable threshold $\tau$ was evaluated only at its default value. Future work should validate cohort-specific configurations (e.g., $\tau=5.0$ for diabetic users, $\tau=8.0$ for chronic kidney disease) through clinical trials with patient populations where the cost of estimation error is quantifiable in terms of treatment outcomes.

**Intake estimation.** Addressing the plate waste limitation requires extending the input pipeline to support post-meal image capture. A before-and-after photograph pair, analysed by a volume estimation model, could provide a coarse intake fraction that the system applies as a scalar correction to the initial serving estimate.

**Cultural localisation.** Expanding the Food Class Registry with locale-specific food taxonomies, aliases, and preparation conventions would enable deployment across culturally diverse populations. This expansion should be informed by dietary assessment standards from non-Western nutritional epidemiology.

**Clinician-facing interfaces.** The current artifact is patient-facing only. A clinician dashboard enabling dietitians to review flagged entries, adjust per-patient thresholds, and monitor longitudinal trends would complete the clinical feedback loop envisioned in the design objectives. Integration with electronic health record systems via FHIR-compliant data export would further embed the artifact within existing clinical workflows.

**Model robustness and diversification.** Evaluating the artifact across multiple LLM providers and model versions would establish the robustness of the routing architecture to changes in the underlying generative model. A model-switching strategy---where the deterministic scoring layer selects the most appropriate model for a given task (e.g., a specialised food recognition model for visual analysis, a general-purpose LLM for clarification dialogue)---could improve both accuracy and resilience.

---

<!-- MERGE ANNOTATIONS SUMMARY FOR CH 6:
     - Phase 2 Discussion preserved as sacred fabric (sections 6.1, 6.2, 6.4, 6.5)
     - EXPANDED §6.2 (CL step): 6 Phase 1 studies woven in (Papathanail, Balsa, Chopra,
       Pfisterer, Di Martino, Seok) — Closed Loop citation gap CLOSED
     - §6.3 (Closing the Loop) — expanded by BR step, enriched by CL step
     - §6.4.8 — Phase 1's methodological limitations integrated (MG step)
     - Claims calibration applied to §6.1 (CL step): hedged language, section title softened
     - Translation applied to §6.3.1, §6.3.4, §6.4.6 (TR step)
     - Discussion Fuel items DF-001 through DF-028 inform the CL step drafting
     - All Phase 1 citation threads now closed or explicitly acknowledged as future work
-->
