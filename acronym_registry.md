# Acronym and Definition Registry

<!-- MERGE NOTE: First-use-only definitions. In the consolidated thesis, each term
     is defined at its FIRST occurrence only. Subsequent uses are the acronym alone.
     This registry tracks where each term is first defined. -->

## Acronyms

| Acronym | Full Form | First Use Location |
|---------|-----------|-------------------|
| AI | Artificial Intelligence | Ch 1, §1.1 |
| AMPM | Automated Multiple-Pass Method | Ch 1, §1.1 (mentioned); Ch 3, §3.2.1 (defined) |
| API | Application Programming Interface | Ch 4, §4.1.2 |
| AUC | Area Under Curve | Ch 2, §2.3 |
| CNN | Convolutional Neural Network | Ch 2, §2.3 |
| CV | Computer Vision | Ch 2, §2.1 |
| DSR | Design Science Research | Ch 3, §3.1 |
| EHR | Electronic Health Record | Ch 1, §1.1 |
| FEDS | Framework for Evaluation in Design Science | Ch 3, §3.4 |
| GDPR | General Data Protection Regulation | Ch 3, §3.5 |
| HCI | Human-Computer Interaction | Ch 4, §4.3.3 |
| HIPAA | Health Insurance Portability and Accountability Act | Ch 3, §3.5 |
| IMU | Inertial Measurement Unit | Ch 2, §2.3 |
| IRB | Institutional Review Board | Ch 3, §3.5 |
| JWT | JSON Web Token | Ch 4, §4.1.2 |
| LLM | Large Language Model | Ch 1, §1.3 |
| LSTM | Long Short-Term Memory | Ch 2, §2.3 |
| LTC | Long-Term Care | Ch 2, §2.2 |
| MAE | Mean Absolute Error | Ch 1, §1.4 |
| MeSH | Medical Subject Headings | Ch 2, §2.2 |
| mHealth | Mobile Health | Ch 3, §3.2.1 |
| MNA-SF | Mini Nutritional Assessment - Short Form | Ch 2, §2.3 |
| NASA-TLX | NASA Task Load Index | Ch 5, §5.3 |
| NLP | Natural Language Processing | Ch 2, §2.3 |
| OSF | Open Science Framework | Ch 2, §2.2 |
| PCC | Population-Concept-Context | Ch 2, §2.2 |
| PRISMA-ScR | PRISMA extension for Scoping Reviews | Ch 2, §2.1 |
| PWA | Progressive Web Application | Ch 4, §4.1.1 |
| RCT | Randomised Controlled Trial | Ch 2, §2.3 |
| RNN | Recurrent Neural Network | Ch 2, §2.3 |
| RQ | Research Question | Ch 1, §1.4 |
| SSE | Server-Sent Events | Ch 4, §4.1.1 |
| SUS | System Usability Scale | Ch 5, §5.3 |
| TCN | Temporal Convolutional Network | Ch 2, §2.3 |
| TNR | True Negative Rate | Ch 5, §5.1 |
| TPR | True Positive Rate | Ch 5, §5.1 |
| UUID | Universally Unique Identifier | Ch 4, §4.1.1 |
| VLM | Vision-Language Model | Ch 5, §5.1 |
| VUI | Voice User Interface | Ch 3, §3.2.2 |
| WCAG | Web Content Accessibility Guidelines | Ch 3, §3.5 |
| WHO | World Health Organization | Ch 2, §2.2 |

## Key Terms (First-Use Definitions)

| Term | Definition Location | Notes |
|------|-------------------|-------|
| Friction-fidelity paradox | Ch 1, §1.2 | Central construct. Defined once, referenced throughout. |
| Directed state graph | Ch 4, §4.1.3 | NOT "directed cyclic graph." Aligned terminology. |
| Semantic Gatekeeper | Ch 4, §4.1.3 | NOT "Critique step." Aligned terminology. |
| Deterministic complexity scoring | Ch 4, §4.2 | Full formula in §4.2.2. |
| Clinical threshold routing | Ch 4, §4.2.3 | Three-tier policy. |
| Threshold-gated deferral | Ch 4, §4.4.1 | Architectural pattern name. |
| Safety budget | Ch 4, §4.4.3 | $N_{max}=2$. |
| Probabilistic silence | Ch 3, §3.2.2 | Agent never requesting clarification. |
| Dominant factor | Ch 4, §4.2.2 | Highest weighted-square ambiguity dimension. |
| Food Class Registry | Ch 4, §4.2.1 | YAML-based knowledge base. Full spec in Appendix H. |
| Managed ephemerality | Ch 4, §4.3.1 | Raw media retention policy. |
| Oracle protocol | Ch 5, §5.1 | Automated ground-truth supply for benchmarking. |
| Digital Buffet | Ch 5, §5.3 | Human-in-the-loop evaluation protocol. |

## Citation Key Unification Notes

<!-- [MERGE: Citation keys across phases]
     Phase 1 and Phase 2 use separate .bib files. When consolidating to a single
     references.bib, the following potential conflicts should be checked: -->

- Phase 1 .bib: `references.bib` (1.6MB — includes many unused entries)
- Phase 2 .bib: `references.bib` (65KB — focused on used citations)
- **Action:** Merge Phase 2 .bib into Phase 1 .bib. Check for:
  - Duplicate citation keys for the same source (e.g., same paper cited in both phases)
  - Conflicting keys (different papers with similar auto-generated keys)
  - ~~Phase 2's `[CITATION NEEDED]` placeholder~~ — RESOLVED: consolidated thesis uses Chapter 2 cross-reference

<!-- Known shared citations (appear in both phases):
     - moshfeghUSDepartmentAgriculture2008 (AMPM)
     - wildenbosAgingBarriersInfluencing2018
     - czajaImpactAgingAccess2007
     - berkowskyFactorsPredictingDecisions2017
     - nielsenUsabilityEngineering1994
     - shimDietaryAssessmentMethods2014
     - loImageBasedFoodClassification2020
     - minSurveyFoodComputing2019
     - zhangImagebasedMethodsDietary2024
     These should be verified for consistent keys when merging .bib files. -->
