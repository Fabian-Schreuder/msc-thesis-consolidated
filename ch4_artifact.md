# Chapter 4: Artifact Design --- Snap and Say

<!-- MERGE NOTE: This chapter is Phase 2's Artifact sections (§3.1--§3.6), verbatim.
     This is the technical heart of the thesis. Sacred fabric.
     [TECHNICAL PRESERVATION] flags apply throughout — the technology IS the contribution.

     Changes:
     1. Section numbering adjusted for chapter context
     2. AMPM references point back to Ch 3 (first definition) rather than re-explaining
     3. [OVERLAP: AMPM methodology description] — Phase 2 Artifact §3.4 describes the SOFTWARE
        ADAPTATION of AMPM. The five-step protocol explanation is truncated here; full introduction
        is in Ch 3.2.1. Only the mapping of steps to software nodes is preserved here.
     4. All [TECHNICAL PRESERVATION] content preserved exactly as written
-->

<!-- [TONE HARMONIZED: Chapter-opening transition inserted — connects design objectives
     (Ch 3) to implementation (Ch 4) within the DSR design cycle.] -->

This chapter presents the instantiation of the design objectives formalised in Chapter 3. Each subsystem of the Snap and Say architecture addresses one or more of the six objectives, and each architectural decision can be traced back to a specific gap identified in the scoping review (Section 2.5). The chapter is organised by subsystem to reflect the artifact's modular decomposition.

## 4.1 System Architecture Overview

The Snap and Say artifact is realised as a client-server web application composed of four principal subsystems---three independently deployed units and one logically separated layer: a progressive web application (PWA) front-end, a Python API server, an LLM-orchestrated agent layer co-hosted within the API server, and a managed relational database. Each subsystem addresses a distinct concern identified in the design objectives (Section 3.3), yet cross-cutting requirements---streaming feedback, de-identified authentication, and clinical configurability---shape the communication patterns that bind them.

This section provides a top-level architectural description of the instantiated artifact, situating the subsystems within Hevner et al.'s \citep{hevnerDesignScienceInformation2004} three-cycle view of Design Science Research: the *relevance cycle* (user-facing interaction), the *design cycle* (the engineered artifact), and the *rigour cycle* (methodological grounding). The architecture is designed to resolve the friction-fidelity paradox identified in Section 3.2: every subsystem boundary reflects a deliberate trade-off between minimising user burden and maximising clinical data quality.

<!-- Figure: Reference architecture of Snap and Say -->

### 4.1.1 Front-End: Progressive Web Application

<!-- [TRANSLATED: Next.js → reactive component-based PWA framework; Vercel → cloud-native edge deployment platform.
     Rationale: The specific framework and hosting platform are vehicles for delivering the PWA.
     The contribution is the multimodal input fusion and streaming feedback architecture, not the framework choice.] -->

The user-facing layer is a progressive web application (PWA) built on a reactive component-based framework (Next.js) and deployed to a cloud-native edge platform (Vercel), enabling cross-device accessibility without app-store installation barriers --- a deployment characteristic directly motivated by the translational gap identified in Section 2.5.1. Its primary responsibility is to mediate the multimodal input fusion specified in Objective 6: users capture a meal photograph via the device camera and, optionally, provide a concurrent voice description through the browser's native audio capture API. Both media streams are transmitted to the backend as binary objects, decoupling the front-end from any nutritional reasoning. During analysis, the interface subscribes to a Server-Sent Events (SSE) channel that relays structured agent events, enabling the user to observe the agent's reasoning process in real time rather than confronting a silent loading state. This streaming architecture addresses the established two-second tolerance threshold for perceived system responsiveness \citep{nahStudyTolerableWaiting2004}.

<!-- [TRANSLATED: Supabase Auth → managed authentication service implementing de-identified sign-in.
     Rationale: The specific authentication provider is a vehicle. The contribution is the
     de-identified anonymous sign-in pattern itself.]
     [PRIVACY GAP: No HIPAA/GDPR citations accompany the privacy claims here.
     The de-identified authentication pattern should reference regulatory frameworks
     that necessitate this design choice. Suggested: GDPR Article 25 (data protection by design),
     or equivalent health data regulation.] -->

Authentication follows a *de-identified anonymous sign-in* pattern managed by a cloud-hosted authentication service (Supabase Auth). Each participant receives a persistent UUID with no associated personally identifiable information, satisfying the privacy requirements of clinical research while maintaining session continuity for longitudinal dietary tracking.

### 4.1.2 Back-End: API Server

<!-- [TRANSLATED: FastAPI → asynchronous Python API framework with native streaming support;
     Railway → managed cloud platform supporting persistent server processes.
     Rationale: The specific framework and hosting platform are vehicles. The contribution is
     the streaming event architecture and the stateful agent graph it serves.] -->

The backend is an asynchronous Python API server (FastAPI) deployed on a managed cloud platform (Railway) selected for its support of persistent processes---a requirement imposed by the stateful nature of the agent graph. The API layer exposes versioned REST endpoints for dietary log management, user authentication verification, and data export, while a dedicated SSE endpoint streams agent events to the front-end.

### 4.1.3 Agent Layer: LangGraph State Graph

<!-- [TECHNICAL PRESERVATION: Directed state graph architecture] -->
<!-- [CORE NOVELTY: Agentic AI architecture for dietary assessment] -->

The core intellectual contribution of the artifact resides in the agent layer, implemented as a directed state graph using the LangGraph framework \citep{chaseLangChain2022}. The graph encodes the three-phase ordering mandated by Objective 5---semantic resolution before quantitative clarification---as a topological invariant of its node structure. The numbered nodes correspond to five stages:

1. **Analysis.** The entry node consumes the multimodal input and invokes a large language model to produce structured nutritional estimates, including per-item confidence scores and integer ambiguity levels along three orthogonal dimensions (hidden ingredients, invisible preparation, and portion ambiguity).

2. **Semantic Gatekeeper.** A dedicated node evaluates whether any reported food item is taxonomically unbounded---an "umbrella term" whose referent is nutritionally unconstrained. When such terms are detected, the agent interrupts the flow and requests categorical clarification from the user before any quantitative assessment is attempted. This node operationalises Objective 3 by consulting a curated Food Class Registry.

3. **Conditional Routing.** A decision edge evaluates three criteria in strict priority order: (a) mandatory override flags from the registry, (b) the deterministic complexity score $C$ computed against the session-specific clinical threshold $\tau$ (Objectives 1 and 2), and (c) a standard confidence gate.

4. **AMPM Subgraph.** When routing directs the flow toward clarification, an embedded subgraph modelled after the USDA Automated Multiple-Pass Method (described in Section 3.2.1) conducts a targeted detail cycle. Rather than interrogating every dimension equally, the cycle prioritises the *dominant factor*. A safety budget of two clarification rounds enforces Objective 4.

5. **Finalisation.** The terminal node persists the validated nutritional data to the database and closes the SSE stream.

### 4.1.4 Data Layer: Managed Relational Database

<!-- [TRANSLATED: Supabase PostgreSQL → managed, encrypted relational database.
     Rationale: The specific database provider is a vehicle. The contribution is the schema
     design separating clinical metrics from session metadata.]
     [PRIVACY GAP: The database stores dietary data that constitutes health information.
     No reference to data-at-rest encryption, access control policies, or regulatory
     compliance (GDPR, HIPAA) is provided. Suggested: cite data protection regulation
     and describe the managed platform's compliance certifications.] -->

All dietary records are persisted in a managed, encrypted relational database (Supabase-hosted PostgreSQL). The database schema separates computed nutritional metrics from session metadata, enabling researchers to query evaluation metrics without accessing raw user interactions.

### 4.1.5 Architectural Rationale

The decomposition into four subsystems reflects a deliberate mapping between design objectives and system boundaries. The front-end isolates input acquisition (Objective 6) from clinical reasoning; the API server enforces schema integrity; the agent layer encapsulates the entire friction-fidelity control policy (Objectives 1--5); and the data layer preserves the evidentiary record required for summative evaluation.

## 4.2 Dynamic Complexity Scoring and Agent Routing

<!-- SOURCE: Phase 2 Artifact §3.2. Verbatim.
     [TECHNICAL PRESERVATION: Deterministic Complexity Scoring Engine]
     [TECHNICAL PRESERVATION: Three-tier routing policy] -->

### 4.2.1 Inputs to the Complexity Calculator

The scoring pipeline operates on two categories of input. First, the LLM produces *ambiguity levels* to systematically trigger selective clarification \citep{kuhnCLAMSelectiveClarification2023}. These levels quantify uncertainty along three orthogonal dimensions, each rated on an integer scale from 0 to 3: (i) **Hidden Ingredients** ($L_i$), (ii) **Invisible Preparation** ($L_p$), and (iii) **Portion Ambiguity** ($L_v$).

Second, the system consults a *Food Class Registry*---a curated, YAML-based knowledge base that maps food names and aliases to predefined risk profiles. Each risk profile encodes dimension-specific weights ($w_i$, $w_p$, $w_v$), a semantic penalty ($P_{\text{sem}}$), and a mandatory clarification flag.

### 4.2.2 The Deterministic Scoring Formula

$$C = \sum_{d \in \{i,\, p,\, v\}} w_d \cdot L_d^{2} + P_{\text{sem}}$$

The quadratic term $L^2$ ensures that moderate ambiguity (level 2) contributes four times more than low ambiguity (level 1), producing a non-linear penalty curve that disproportionately flags meals whose nutritional content is genuinely uncertain. The system additionally identifies the *dominant factor*---the dimension contributing the largest weighted-square term---which steers clarification question generation.

### 4.2.3 Routing Logic Within the State Graph

The computed score $C$ drives a three-tier conditional routing policy:

1. **Mandatory override.** If the matched food class carries a `mandatory_clarification` flag, the system unconditionally routes to the clarification subgraph.

2. **Clinical threshold override.** The score $C$ is compared against a configurable per-session threshold $\tau$ (default $\tau = 15.0$). If $C > \tau$, the system enters the clarification subgraph. Critically, $\tau$ can be lowered for clinically sensitive cohorts \citep{evertNutritionTherapyAdults2019}.

3. **Standard confidence check.** Meals whose average per-item confidence exceeds 0.85 are routed directly to database insertion; those below enter the clarification subgraph.

A safety budget of at most two clarification rounds prevents unbounded questioning.

## 4.3 Multimodal Input Pipeline

<!-- SOURCE: Phase 2 Artifact §3.3. Verbatim. -->

The dietary assessment process is initiated through a multimodal input pipeline designed to satisfy Objective 6 (Multimodal Input Fusion). By fusing visual and acoustic data, the architecture compensates for the ambiguity inherent in isolated modalities.

<!-- Figure: Sequence diagram of multimodal input pipeline -->

### 4.3.1 Input Capture and Transmission

Users capture a photograph and/or record a natural-language description. The PWA does not perform local processing. Both media are uploaded to a secure Supabase Storage bucket. Raw media blobs are subject to managed ephemerality.

### 4.3.2 Audio Transcription

<!-- [TRANSLATED: OpenAI Whisper → automatic speech recognition (ASR) service.
     Rationale: The specific ASR provider is a vehicle. The contribution is the multimodal
     fusion pipeline that integrates speech transcription with visual analysis.
     The language configuration capability is the clinically relevant feature.] -->

The audio is processed by an automatic speech recognition service (OpenAI Whisper) to generate a text transcript. The service incorporates a language parameter (defaulting to Dutch, reflecting the target population), enabling locale-specific transcription that accommodates the linguistic context of the target population.

### 4.3.3 LLM Fusion and Execution

The LLM service retrieves the image blob and encodes it as a base64 data URI. This encoded image, alongside the Whisper-generated transcript and a strictly typed system prompt, is packaged into a unified message structure. The prompt instructs the reasoning engine to prioritise explicit vocal assertions when resolving direct contradictions with the visual evidence.

Execution is handled via a streaming protocol. Partial tokens are broadcast over the SSE connection to the front-end, satisfying the two-second threshold requirement \citep{nahStudyTolerableWaiting2004}. Streaming the agent's reasoning trace serves a critical psychological function in HCI: exposing the reasoning process fosters user trust and mitigates algorithmic aversion.

## 4.4 Dynamic Routing and the AMPM Subgraph

<!-- SOURCE: Phase 2 Artifact §3.4. Verbatim.
     [CORE NOVELTY: AMPM-to-software-agent translation]
     [OVERLAP: AMPM methodology description] — The five-step AMPM is introduced in Ch 3.2.1.
     Here we describe only the SOFTWARE ADAPTATION mapping. The full protocol
     explanation is NOT repeated; instead, we reference Section 3.2.1. -->

The core innovation lies in the ability to dynamically modulate the depth of dietary inquiry based on the clinical sensitivity of the meal entry. This section describes the conditional routing mechanism and the truncated adaptation of the USDA AMPM (introduced in Section 3.2.1).

### 4.4.1 Clinical Threshold Routing

Following the initial multimodal analysis and semantic resolution (Objective 5), the execution flow reaches the conditional router. This routing node evaluates the deterministic complexity score against the configurable threshold $\tau$.

The router implements an architectural pattern this thesis terms *threshold-gated deferral*. If $C \leq \tau$ and no mandatory overrides are triggered, the agent accepts the probabilistic risk that the initial estimates are clinically sufficient. If $C > \tau$, the flow is routed into the AMPM subgraph.

### 4.4.2 The AMPM Adaptation State Graph

<!-- [OVERLAP: AMPM five-step description] — Truncated here. Full protocol in Ch 3.2.1.
     Only the step-to-node mapping is preserved as it constitutes the novel translation. -->

The AMPM subgraph consists of two primary nodes: the **Detail Cycle** and the **Final Probe**. This architecture represents a deliberate translation and truncation of the traditional five-step clinician-led AMPM, mapping its structured inquiry approach onto an asynchronous, conversational software agent. The mapping accounts for all five traditional steps:

- Steps 1 (Quick List) and 3 (Time and Occasion) are satisfied passively by the multimodal input pipeline (Section 4.3) and the PWA's temporal metadata, respectively.
- Step 2 (Forgotten Foods) and Step 4 (Detail Cycle) are instantiated explicitly as LangGraph nodes.
- Step 5 (Final Probe) is conditionally merged into the forgotten foods pass.

1. **The Detail Cycle Node.** An iterative clarification engine that parses the complexity score to identify the food item contributing the highest variance. The LLM generates a single, highly targeted clarification question aimed at resolving the dominant ambiguity.

2. **The Final Probe Node.** If the Detail Cycle proves inconclusive, the flow transitions to a generic trailing inquiry mirroring the "Forgotten Foods" pass.

### 4.4.3 The Safety Budget as a Design Proposition

The subgraph enforces a strict safety budget ($N_{max}=2$). Within the DSR framework, $N_{max}$ is not merely a hardcoded parameter but an explicitly testable design proposition hypothesising the optimal equilibrium point between patient burden and data yield.

If $N_{max}$ is exhausted and $C$ remains above $\tau$, the router triggers a fail-safe mechanism, flagging the record for manual clinical review.

## 4.5 Prompt Engineering and Model Configuration

<!-- SOURCE: Phase 2 Artifact §3.5. Verbatim.
     [TRANSLATED: Pydantic → typed schema validation library enforcing structural determinism;
     GPT-4o → multimodal large language model.
     Rationale: The specific validation library and LLM are vehicles. The contribution is the
     prompt engineering strategy (persona stability, graceful degradation) and the architectural
     decision to enforce structural determinism through typed schemas rather than post-hoc parsing.] -->

To ensure appropriate interaction with a geriatric user base, the system relies on rigorous prompt engineering to enforce persona stability. A meta-prompt conditions the model to adopt a supportive, non-clinical tone. The agent operates under "Graceful Degradation" instructions to prevent refusal responses.

The system enforces strict syntactic correctness using typed schema validation (Pydantic), requiring a JSON structure comprising a title, an item list (name, quantity, confidence), and a synthesis comment. The system maintains the default temperature ($T=1.0$) for the multimodal large language model (GPT-4o at time of evaluation). The typed schema validation handles structural determinism, allowing the higher temperature to generate naturalistic variations. To mitigate hallucination, the context window is scoped strictly to the current meal session.

## 4.6 Geriatric-Centric Interface Design

<!-- SOURCE: Phase 2 Artifact §3.6. Verbatim.
     [ACCESSIBILITY GAP: The interface design describes features that align with WCAG 2.1
     Level AA accessibility guidelines (high contrast, haptic feedback, simplified interaction models),
     but does not cite WCAG 2.1 or any geriatric-specific HCI accessibility standard.
     Suggested citation: W3C Web Content Accessibility Guidelines (WCAG) 2.1, Level AA.
     This would ground the design decisions in established accessibility standards
     rather than presenting them as ad hoc choices.] -->

The frontend implementation addresses physical and sensory limitations identified in the problem analysis. The voice input mechanism utilises a "Tap-to-Toggle" interaction model rather than "Push-to-Talk," specifically accommodating users with reduced fine motor skills. The `VoiceButton` component provides redundant feedback channels to support users with sensory decline, pairing a high-contrast dynamic waveform visualisation with distinct haptic vibration patterns (20 ms versus 50 ms) to confirm state transitions \citep{venableFEDSFrameworkEvaluation2016}.

---

<!-- MERGE ANNOTATIONS SUMMARY FOR CH 4:
     - All Phase 2 Artifact content (§3.1-§3.6) preserved as sacred fabric
     - [TECHNICAL PRESERVATION] flags maintained throughout
     - [OVERLAP: AMPM description] — Five-step AMPM NOT re-explained here.
       Section 4.4 references Ch 3.2.1 for the protocol; preserves only the
       step-to-node mapping (which is the novel contribution).
     - Section numbering adjusted for chapter context
     - No content deleted
     - No content added (BR step may add bridging annotations later)
-->
