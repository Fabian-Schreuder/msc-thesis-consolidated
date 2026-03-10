# Chapter 5: Evaluation

<!-- MERGE NOTE: This chapter is Phase 2's Evaluation section (§4), verbatim.
     Sacred fabric — supervisor-approved. No substantive changes.
     Minimal framing adjustments for chapter-level presentation. -->

In alignment with the Design Science Research Methodology \citep{peffersDesignScienceResearch2007}---which mandates rigorous evaluation to ascertain whether the designed artifact adequately addresses the specified problem---the Snap and Say application was subjected to empirical testing. The evaluation was structured using the FEDS framework \citep{venableFEDSFrameworkEvaluation2016} to assess the artifact against the core tension identified during the problem identification phase: the friction-fidelity paradox. The goal was to demonstrate utility, quality, and efficacy \citep{hevnerDesignScienceInformation2004} while validating the proposed agent-driven routing mechanisms.

The system was evaluated through a mixed-methods approach: early simulated benchmarking to quantify baseline nutritional accuracy and autonomous logic efficiency (Objective 1), followed by a human-in-the-loop "Digital Buffet" protocol involving older adult participants ($N=19$, Age $\geq 65$) to assess usability and system latency (Objectives 4 and 6).

<!-- Figure: Demonstration of the Snap and Say interface -->

## 5.1 Simulated Benchmarking: Accuracy and Suppression Efficiency

Prior to human trials, the artifact's core routing logic (Objective 2) and nutritional estimation accuracy (Objective 1) were evaluated using an automated "Oracle" simulation against a stratified $N=500$ subset of the Nutrition5k dataset \citep{thamesNutrition5kAutomaticNutritional2021}. The dataset was bifurcated into "Simple" items (visually distinct, $\leq 3$ ingredients) and "Complex" items (high visual ambiguity or hidden caloric density). The Oracle mechanically supplied ground-truth metadata whenever the agent's Clarification Node generated a question, isolating the system's analytical reasoning capacity from human behavioural variability.

**Estimation Accuracy.** The Agentic Loop was compared against a Single-Shot baseline relying exclusively on a foundational Vision-Language Model (VLM) for zero-shot inference. For Simple items, the Agentic approach yielded a marginal 3.2% reduction in MAE for caloric estimation (74.5 kcal vs. 72.1 kcal). However, for Complex items, the agentic clarification cycles drove a substantial 30.6% error reduction (185.0 kcal vs. 128.4 kcal), resulting in an overall MAE improvement of 22.8% across the test bed.

| Category | Baseline (Single-Shot) | Snap and Say (Agentic) | Error Reduction |
|---|---|---|---|
| Simple Items | 74.5 kcal | 72.1 kcal | 3.2% |
| Complex Items | 185.0 kcal | 128.4 kcal | 30.6% |
| **Overall MAE** | **129.8 kcal** | **100.3 kcal** | **22.8%** |

**Suppression Logic Efficiency.** The dynamic complexity scoring architecture successfully bounded unnecessary querying. The Clarification Node achieved a 92% True Negative Rate on Simple items (the system autonomously bypassed intervention for obvious foods) while correctly triggering intervention on 78% of Complex cases.

## 5.2 Calibration of Routing Thresholds

Initial benchmarking revealed a performance gap wherein the default configuration yielded a sub-optimal True Negative Rate on Simple items (approximately 54%). An empirical threshold tuning experiment was conducted to map the operational space between interaction friction and nutritional fidelity. The parameter sweep evaluated two independent gating mechanisms concurrently:

1. **Deterministic Clinical Threshold ($C_{\text{thresh}}$):** Ranging from 5.0 to 20.0.
2. **LLM Confidence Threshold ($\text{Conf}_{\text{thresh}}$):** Ranging from 0.70 to 0.90.

The optimal threshold combination ($C_{\text{thresh}} = 15.0$, $\text{Conf}_{\text{thresh}} = 0.85$) was selected to maximise the True Negative Rate on Simple items while preserving the True Positive Rate required for the 30.6% MAE reduction on Complex items. This tuning methodology grounds the artifact's routing logic in empirical trade-off analysis.

## 5.3 User Experience: Digital Buffet Protocol ($N=19$)

Participants were tasked with logging two distinct meal types---one "Simple" (roasted chicken) and one "Complex" (mixed green salad)---designed to trigger the single-shot and agentic clarification routing paths respectively. Participant responses were collected utilising an adapted survey instrument combining elements of the System Usability Scale (SUS) and NASA Task Load Index \citep{hartDevelopmentNASATLXTask1988}.

Based on the quantitative survey feedback (Q1--Q13 on a 5-point Likert scale):

1. **System Usability and Learnability (Objective 6 Validated).** The overwhelming majority of participants experienced low barriers to entry. Responses indicated strong agreement that the system was easy to learn and that users felt confident utilising the multimodal interface \citep{wildenbosAgingBarriersInfluencing2018}.

2. **Perception of Temporal Demand and Complexity.** Scores clustered heavily toward the lower end of the scale (disagreement), indicating a smooth perceived experience despite the complex asynchronous processing.

3. **Acceptance of the Clarification Interruption (Objective 4 Validated).** The agentic intervention was generally well-received. Participants largely did not find the system cumbersome or unnecessarily complex.

## 5.4 Qualitative Insights and Edge Cases

Beyond the numeric metrics, qualitative open-text responses generated critical insights:

- **Latency as Residual Friction.** Several participants explicitly noted speed as an area for improvement. One participant stated, "De app mag sneller werken" (The app could work faster). The average 7.1-second round-trip is acceptable for a research prototype \citep{nahStudyTolerableWaiting2004} but requires further optimisation for deployment.

- **Acoustic and System Confidence Challenges.** A subset of users experienced issues with the microphone or speech recognition (e.g., "Microfoon werkte niet"). When the primary modality failed, the fallback mechanisms became the hardest part of the interaction.

- **Desire for System Competence.** Qualitative feedback requesting features such as "Let op verschil tussen cal. en kcal." (Note the difference between cal. and kcal.) demonstrates that users quickly escalate their expectations of the system's nutritional expertise once the basic friction of logging is removed.

In synthesis, the evaluation confirms the utility of the Snap and Say architecture. The decoupling of unstructured capture from deterministic complexity analysis actively manages the friction-fidelity paradox.

---

<!-- MERGE ANNOTATIONS SUMMARY FOR CH 5:
     - Phase 2 Evaluation preserved verbatim as sacred fabric
     - Section numbering adjusted for chapter context
     - MAE comparison table converted to Markdown format
     - No content deleted or added
     - [GAP: Threshold calibration circularity] — acknowledged in Phase 2 Discussion (Ch 6)
     - [GAP: Digital Buffet ecological validity] — acknowledged in Phase 2 Discussion (Ch 6)
-->
