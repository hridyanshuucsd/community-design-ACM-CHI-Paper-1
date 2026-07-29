# CHI 2027 Paper Structure
## Working Title: "PARCEL: Voice-First, Multi-User 3D Co-Design for Community-Authored Alternatives to Developer Proposals"

*Section-by-section architecture for the full paper. Each study component (formative study; each of the 3 conditions in the main study) includes an explicit, falsifiable statement of what a successful result looks like and what a null/negative result looks like, per the pre-submission rigor audit. "PARCEL" (Participatory Architecture for Real-time Community-Enabled Layout) is the working name for the Pascal-based tool; rename freely.*

**Pivot flag (2026-07-28): this document was written when the field site was San Diego.** The field site has since moved to Delhi NCR (Noida), India, with a domain shift from zoning/rezoning disputes to group-housing-society redevelopment disputes (see proposal.md). The methodological skeleton below (N sizes, between-subjects design, 3-condition actionability manipulation, pre-registration plan, baseline justification) still holds and does not need rework. What is now stale and needs a fresh pass before this is submission-ready: every mention of "San Diego," "zoning"/"rezoning," "ADU"/"HCD permit data," and "planning commission" below should read as Noida Authority building bylaws, FSI/setback norms, and redevelopment/RWA stakeholder processes instead; "two real or realistically-modeled San Diego infill parcels" should become two real or realistically-modeled Noida/Greater Noida group housing society redevelopment sites; and any recruitment-pipeline language tied to CHPD/Design for San Diego should be replaced with the formative-study recruitment plan described in proposal.md (personal local contacts, not an existing institutional pipeline).

---

## 0. Framing Decisions Made to Use This Structure (state these explicitly in the paper)

These are the concrete choices this structure commits to, in response to the rigor checklist's `[ASSUMPTION, confirm]` flags. State them plainly in §4 (Methods) rather than leaving them implicit:

- **Formative study:** N=20 to 24, semi-structured interviews + one co-design workshop, community members + 2 to 3 planning professionals, San Diego, needs-finding → numbered design goals (BoundarEase/Graphologue-style traceability).
- **Main study:** Between-subjects, N=36 individuals organized into **12 design teams of 3** (triads), **4 teams per condition**, one ~2.5-hour session per team on one of **two real or realistically-modeled San Diego infill parcels** (parcel assignment balanced 2-teams-per-parcel-per-condition to prevent site confound). Between-subjects, not within, because a single design session has strong path-dependence (you can't "forget" the design you just built to do it again in a different condition) and because session length (~2.5 hrs) makes repeated-measures burdensome.
- **The 3 conditions manipulate AI facilitation actionability** (none → awareness-centered → solution-centered), directly extending VizCrit's single-user actionability spectrum into a multi-user, legally-hard-constrained domain. Condition A (manual, rule-based binary compliance badge only, no AI facilitator) is the paper's **functional baseline/control**, explicitly justified below (§0.1) since a true "status-quo comment card" condition is not run as a 4th arm of the same tool.
- **Framing of confirmatory power:** the N=36 study is framed as **exploratory/formative-to-confirmatory hybrid**, not a fully powered confirmatory RCT, see §7.1 for the explicit non-circular justification and sensitivity analysis, following the precedent set by StoryEnsemble and Orca for novel systems.
- **Pre-registration:** the main study's primary hypotheses, primary vs. secondary DVs, and analysis plan are pre-registered on OSF before data collection (§7.2). The formative study is explicitly exploratory/unregistered.

### 0.1 Baseline justification (rigor gap #3/#4, explicit)

We do not run a 4th "comment-card-only" condition using an entirely different tool (e.g., a physical comment form or a static rendering viewer), for the same reason Orca and A11yShape give for omitting a baseline: the comparison would be confounded by tool-novelty and interface-modality effects unrelated to our actual manipulation (AI facilitation actionability), and, separately, running a real community group through a condition we already know from the civic-tech literature (BoundarEase, WeDesign, Sassmannshausen et al.) produces low agency and low legitimacy would not be a good use of a real community group's time within a single-session field study. Instead:

1. **Condition A (manual, binary-compliance-only) is a legitimate, non-trivial baseline**, it uses the *same* 3D tool, geometry engine, and voice/direct-manipulation input as B and C, isolating the AI-facilitation manipulation from tool/modality confounds (a cleaner counterfactual than a categorically different medium).
2. We separately collect a **recalled-baseline self-report item** from all 36 participants post-session ("Compared to public comment processes you've experienced before, a hearing, a comment card, a survey, how did this session compare on [fairness / being heard / influence]?"), giving a second-hand but real comparison against genuine prior civic-engagement experience, without requiring a live 4th arm.
3. We name the missing live status-quo-arm explicitly as a limitation (§10) and scope a follow-up field deployment (embedded in an actual live rezoning comment period, BoundarEase-style) as the necessary next study to make the "legitimate alternative to commenting" claim with full causal force.

---

## 1. Abstract (~250 words)

Digital participation tools have moved communities from being informed to being consulted, but rarely to being co-authors: across an 84-study systematic review, only 16.7% of urban-digital-twin participation projects list citizens as co-developers, and none support real-time multi-user authoring of a legally buildable alternative to a professional design. We present **PARCEL**, a voice-first, multi-user 3D architectural co-design tool built on the open-source Pascal editor, that lets community groups author a real, zoning-compliant building design on a real parcel, collaboratively, in real time, with legal constraints encoded as spatial affordances rather than text, producing a submittable design artifact rather than a comment. A formative study (N=22 San Diego community members and planning professionals) surfaces five recurring frictions in current engagement processes and yields five corresponding design goals realized in PARCEL's constraint model, AI facilitation layer, and voice interface. A between-subjects field study (N=36, 12 three-person design teams) compares three levels of AI facilitation actionability, none, awareness-centered, and solution-centered, on expert-rated design buildability, self-reported legitimacy and inclusion (adapted from validated civic-design instruments), and Arnstein-ladder-coded participation depth, with pre-registered hypotheses and an explicit check for AI-induced convergence across independent groups. We report [planned quantitative pattern], contribute a voice-command and zoning-compliance error taxonomy from an independent technical benchmark, and discuss what closing the "artifact gap", the asymmetry between a professional's buildable proposal and a community's comment, requires of both interface design and civic process. Materials, protocols, and the compliance-check benchmark are released as supplementary artifacts.

*(Note: keep the bracketed placeholder honest until real data exist, do not pre-write a specific result into the camera-ready abstract.)*

---

## 2. Introduction

**Structure (≈1.25 columns):**

1. **Hook + stakes.** A concrete vignette: a rezoning proposal, a comment period, a community that can react but not author. Ground in Arnstein's Ladder, most digital civic tools sit at rungs 3 to 5 (inform/consult), per the Luo et al. 2025 review's 16.7% co-developer statistic.
2. **The artifact gap (core theoretical contribution #3).** Define explicitly: a professional's design can be reviewed, costed, modified, compared; a comment can be acknowledged and set aside. This asymmetry, not lack of voice or lack of access, is the proposed mechanism keeping tools at low Arnstein rungs even when they are well-designed (WeDesign, CoDesignAI, BoundarEase all leave this gap explicitly open, cite the related-work synthesis's own gap statement).
3. **What existing tools do instead, briefly** (full treatment deferred to §3): image generation without editability (WeDesign); AI-facilitated conversation without a design output (CoDesignAI); browsing pre-built official alternatives without authoring a new one (BoundarEase). One sentence each; the detailed critique belongs in Related Work.
4. **The proposed solution.** PARCEL: constraints as spatial physics (not a rulebook), multi-user real-time authoring, voice-first interaction generated from the live constraint graph rather than a fixed command grammar, AI facilitation that scales from passive to proactive.
5. **Contributions, explicitly enumerated** (CHI reviewers reward this list):
   - **C1 (theory):** An extension of the design-feedback actionability spectrum (VizCrit) from single-user/heuristic to multi-user/legally-hard-constrained design, adding a "whose violation, shown to whom" axis.
   - **C2 (theory):** The first application of task-driven, malleable-UI generation (Cao/Jiang/Xia) to voice as a modality in a legally constrained physical domain, rather than visual affordances alone.
   - **C3 (system):** PARCEL itself, architecture, constraint-grounding pipeline, conflict-tolerant multi-user scene layer extending Spacetime's Parallel Objects, AI facilitation pipeline, and IFC-based proposal export, built on and contributed back to an actively maintained 18k-star open-source project.
   - **C4 (empirical):** A formative study yielding a traceable findings→design-goals table, and a between-subjects field study comparing three AI-facilitation actionability levels on expert-rated buildability, validated inclusion/legitimacy measures, and Arnstein-coded participation depth, the first study in this literature to pair a real/expert-rated outcome with self-report for a community-*authored* (not reviewed) design.
   - **C5 (methods):** A pre-registered analysis plan, an Arnstein-ladder coding scheme with reported inter-rater reliability, and an explicit AI-homogenization check across independent community groups, three rigor practices largely absent from this sub-field to date.
6. **Roadmap paragraph.**

---

## 3. Related Work

Organize exactly as the four clusters already synthesized (civic/participatory tools; feedback exchange & collective creativity; malleable/multi-user UI; voice & spatial CAD interfaces). **Main-body treatment should be compressed** relative to the full synthesis, roughly 120 to 180 words per paper for the ~8 to 10 most load-bearing citations, one sentence each for the rest, ending each cluster with an explicit gap statement that names exactly what PARCEL does differently. The full per-paper limitation/extension analysis already drafted (WeDesign, CoDesignAI, BoundarEase, Sassmannshausen et al., Luo et al.; Value-Centered Framing, VizCrit, Prototyping Dynamics, Self-Reflective Crowds, DesignWeaver, StoryEnsemble, Cueing the Crowd, Shepherding the Crowd; Spacetime, Cao/Jiang/Xia, Gradual Generation, CrossTalk, Orca, Belidor, Gazeify Then Voiceify, Making Abstraction Concrete; MozArt, Digital Modeling for Everyone, Khan & Tunçer, LLMR, Reinders et al., A11yShape, Put-That-There, WordsEye/Ulinski et al.) is **too long for the main body**, push the full paper-by-paper limitation/extension table to **Supplementary Materials §S1**, and keep only:

- 3.1 Civic & Participatory Design Tools (main body: WeDesign, BoundarEase, Luo et al. review statistic; one line each for CoDesignAI, Sassmannshausen et al.)
- 3.2 Feedback Exchange & Collective Creativity (main body: Value-Centered Framing, VizCrit, Prototyping Dynamics; one line each for Self-Reflective Crowds, DesignWeaver, StoryEnsemble, Cueing the Crowd, Shepherding the Crowd)
- 3.3 Malleable/Generative & Multi-User UI (main body: Spacetime, Cao/Jiang/Xia; one line each for the rest)
- 3.4 Voice & Spatial Interfaces for 3D/CAD (main body: Digital Modeling for Everyone, MozArt, LLMR; one line each for the rest)
- Close with an explicit **positioning table** (see below), this is the single cheapest, highest-signal addition a reviewer will look for.

**Table 1, Positioning against closest prior systems** (main body):

| System | Editable output? | Multi-user real-time? | Legally-constrained geometry? | Voice-first? | Real-decision outcome measured? |
|---|---|---|---|---|---|
| WeDesign | No (image) | Partial (shared session) | No | No | No |
| CoDesignAI | No (conversation) | Yes (multi-agent) | No | No | No |
| BoundarEase | No (browse pre-built options) | Partial | Yes (pre-built only) | No | No |
| Spacetime | Yes | Yes (never peer-tested) | No | No | No |
| **PARCEL (ours)** | **Yes** | **Yes (peer-tested)** | **Yes (live constraint graph)** | **Yes (malleable grammar)** | **Yes (planner-rated + legitimacy self-report)** |

---

## 4. Formative Study (N = 20 to 24)

### 4.1 Method
- Recruitment channels stated explicitly (Design for San Diego pipeline, CHPD community partners, snowball sampling from prior developer-outreach/design-review-board attendees), with demographic table (age, tenure/homeownership, prior civic engagement, primary language) reported honestly against the legitimacy claim (rigor gap #7).
- Semi-structured interviews (protocol in Supplementary §S2) + one half-day co-design workshop with a subset (n≈10 to 12), following the BoundarEase two-phase formative/evaluative structure and the Sassmannshausen et al. low-fidelity prototyping precedent (paper/clay/cardboard massing before any software).
- Thematic analysis: two coders, codebook developed iteratively, **inter-rater reliability reported** (Cohen's kappa target ≥0.7, BoundarEase/Self-Reflective-Crowds standard, not WeDesign's 20%-subsample floor).
- Explicitly exploratory / not pre-registered (state this contrast plainly, per rigor item #13).

### 4.2 Findings → Design Goals table (main body, cheap, high-value)

| Finding | Representative pattern | → Design Goal | → System Feature |
|---|---|---|---|
| F1: Professionals present renderings/jargon community can't independently verify | "They show us a picture and tell us it's compliant. I have no way to check that." | D1: Constraints must be spatial/geometric, not textual | Live constraint graph as editor physics (§5.1) |
| F2: Comment periods surface no tradeoffs between alternatives | "We're told to pick between two options someone else already decided on." | D2: Groups must be able to generate/compare multiple alternatives | Solution-centered AI massing suggestions (§5.3, Condition C) |
| F3: Community wants to know what's *allowed*, not just react to what's *proposed* | "Nobody explains why the building has to be that shape." | D3: Proactive, plain-language constraint explanation | Awareness-centered AI facilitation (§5.3, Condition B) |
| F4: Distrust of AI-generated content presented as authoritative | "If a computer made that up, why should the city listen to it?" | D4: AI content must be provenance-marked; compliance checks must be rule-based, not self-graded LLM output | Independent rule-based zoning checker (§5.2); watermarking of AI-suggested geometry |
| F5: Preference for spoken, plain-language interaction over reading dense code text, especially among older and non-English-fluent participants | "I don't want to read a zoning code. Just tell me what I can do." | D5: Voice-first interaction as primary (not secondary/accessibility) modality | Malleable voice grammar generated from constraint graph (§6) |

### 4.3 What a successful formative study looks like
- Thematic saturation reached by participant ~18 to 20 (report a saturation curve, per current qualitative-methods norms).
- Convergent, non-fragmented themes across participants with varied civic-engagement history (renters and homeowners; English- and Spanish-primary speakers), i.e., the frictions named above recur across subgroups rather than being idiosyncratic to one demographic slice, supporting a single tool design rather than requiring per-community customization.
- The workshop's low-fidelity (paper/clay) prototyping session surfaces spontaneous requests for exactly the system features independently named in interviews (e.g., participants ask "can I see what happens if I move this wall" before being shown any software), convergent validity between interview and workshop methods.
- At least one planning professional interviewee independently corroborates F1 F3 from the "supply side" (e.g., confirms that renderings are not designed to be independently verifiable), strengthening the claim that the gap is structural, not merely perceived.

### 4.4 What a null/negative formative result looks like (and what it would mean)
- **Fragmentation:** interviews fail to converge, different subgroups name unrelated, non-overlapping frictions (e.g., renters focus on trust/legitimacy, homeowners focus on property-value protection, non-English speakers focus purely on language access) with no thematic saturation by N=24. *Implication:* a single monolithic tool design cannot serve all community contexts equally; would require reframing the contribution around a configurable/modular design goal set rather than one fixed feature list, and explicitly narrowing the target population in later sections rather than claiming general community legitimacy.
- **Tool-agnostic distrust:** participants report distrust of *any* digital tool a professional or researcher hands them, independent of its features (a legitimacy-of-the-messenger problem, not a legitimacy-of-the-features problem). *Implication:* would require reframing PARCEL's own development as participatory (community co-design of the tool, not just participatory design *with* the tool) before the main study could proceed, a significant, honestly-reportable pivot rather than something to bury.
- **Low workshop transfer:** low-fidelity prototyping requests diverge sharply from interview themes (e.g., nobody asks for multi-alternative comparison despite F2 being a strong interview theme). *Implication:* would suggest F2 is a researcher-primed artifact of interview question wording rather than a genuine spontaneous need; report this discrepancy explicitly rather than cherry-picking the interview version.

---

## 5. System / Design

### 5.1 Constraint-grounding pipeline
- Parcel ingestion → zoning/setback/FAR/height/overlay-district constraint graph, implemented as a new Pascal "system" (per the existing dirty-node systems pattern) rather than a geometry-engine fork.
- Constraint graph as a **live, queryable, subscribed-to input** (Cao/Jiang/Xia's task-driven data model framing), affordances generated *before* editing, not validated *after*.

### 5.2 Independent, rule-based compliance checker (not LLM-self-graded)
- Explicit design decision, directly justified against LLMR's documented Inspector-leniency problem: the compliance checker is a deterministic rules engine validated against real municipal code determinations (ground-truthed against HCD permit records), *not* an LLM asked to grade its own edits.
- **Technical benchmark (decoupled from the human study, UIST-style systems contribution):** held-out set of N configurations (e.g., 300 synthetic + real parcel/design configurations spanning height, setback, FAR, overlay-district edge cases) scored against expert planner determination.
  - *Success:* rules-engine agreement with expert determination, Cohen's kappa ≥ 0.85 to 0.9, with the small residual disagreement concentrated in named edge cases (e.g., overlapping overlay districts, discretionary variances) reported as an explicit taxonomy, not smoothed over.
  - *Null:* kappa in the 0.5 to 0.7 range concentrated in *common* (not edge) cases, would indicate the constraint graph itself is under-specified for ordinary zoning determinations, a system-validity problem that would need fixing before any human study could proceed, and should be reported as a limitation on the entire compliance-feedback claim if it persists into the human study.

### 5.3 AI facilitation pipeline (the manipulation across the 3 main-study conditions)
Three ablatable modules, described as a pipeline (Planner → Compliance-Checker → Suggestion-Generator → Voice-NLU), enabling a **component ablation** (UIST rigor item C4) separate from the human conditions:
- **Module 1 (always on): rule-based compliance badge.** Pass/fail per §5.2.
- **Module 2 (Conditions B, C): awareness-centered explanation.** Plain-language explanation of *why* a move is blocked, citing the specific constraint; periodic value-surfacing convergence prompts adapted from Park/Hou/Sundu/Dow (CI 2025).
- **Module 3 (Condition C only): solution-centered suggestion generation.** Proactive generation of 2 to 3 concrete, compliant alternative massings/unit-mixes the group can preview, accept, remix, or reject, directly extending Prototyping Dynamics' "share multiple" mechanism from marketing-ad dyads to zoning-constrained building groups.

### 5.4 Multi-user real-time layer
- Extends Pascal's `useScene` store with a Spacetime-style Parallel-Objects conflict-tolerant mechanism: simultaneous edits are allowed and made visible (conflict markers), not blocked, then resolved through in-session discussion.
- Explicitly names and closes Spacetime's central limitation: all reported multi-user sessions are peer triads/groups of real, independently-recruited community members, **never a participant paired with the experimenter or a confederate** (rigor item #14). State this explicitly in Methods and contrast with Spacetime's methods section.

### 5.5 Proposal export
- Plan view, 3D elevation, compliance certificate, plain-language decision summary, editable Pascal scene file with IFC export, the "design artifact" that closes the artifact gap (Introduction, C3).

*(Full architecture diagrams, API/schema details, and the Pascal-fork diff go to Supplementary §S3; keep only a single system-architecture figure and the ablation module list in the main body.)*

---

## 6. Voice-First Interface

### 6.1 Design rationale
Directly extends the malleable/generative UI argument (Cao/Jiang/Xia; Gradual Generation) to voice: the set of *sayable, actionable* utterances at any moment is generated from the same live constraint graph and multi-user selection/gaze state driving the visual layer, not a fixed command grammar, avoiding MozArt's small fixed vocabulary and Khan & Tunçer's expert-biased elicited vocabulary.

### 6.2 Vocabulary elicitation methodology
- Re-run a Digital-Modeling-for-Everyone-style Wizard-of-Oz elicitation with **non-expert community members** (not CAD-literate professionals, unlike Khan & Tunçer), N≈15 to 18 drawn partly from the formative-study pool, thematically analyzed for selection strategies, axis-convention assumptions, and "implicit construction" phrasing.
- Deixis/reference-resolution handled via the same scene-graph + selection/gaze state Spacetime tracks for multi-user editing (not a separate module), addressing the Gazeify Then Voiceify error taxonomy (referencing / description / disambiguation-failure) directly for 3D object reference rather than gaze-only.
- Open-mic, multi-speaker disambiguation (MozArt's flagged unsolved problem): each speaker is tracked by a per-user audio channel/beamforming input bound to their avatar/cursor identity in the shared scene, so "move this wall" resolves against *that speaker's* current selection state, not a shared global one.

### 6.3 Technical evaluation (benchmark, decoupled from the human study, UIST rigor item C1/C2)
- Offline benchmark: N utterances (target 250 to 400) collected from the elicitation study, hand-labeled with intended edit; measure task-level execution accuracy of the voice-to-scene-edit pipeline.
- **Error taxonomy** (adapted from Gazeify Then Voiceify + WordsEye/Ulinski's module-level taxonomy): parsing failure / reference-resolution failure (ambiguous object) / constraint-conflict misexplanation / disambiguation-failure (system asks wrong clarifying question) / execution failure (correct interpretation, wrong geometry produced).
- *Success:* ≥80 to 85% first-try task-level success on unambiguous utterances; failures concentrated in reference-resolution (expected, matching the elicitation study's finding that novices use ≥7 distinct selection strategies), not in parsing or execution, meaning the hard problem is where prior work (Digital Modeling for Everyone) predicts it should be, not somewhere our pipeline introduces new failure.
- *Null:* success rate below ~60%, with failures spread across *all* taxonomy categories roughly evenly (no concentration), would indicate the malleable-grammar approach doesn't actually reduce the vocabulary-learning burden relative to a fixed command set, undermining Contribution C2; report this plainly and discuss whether a hybrid (malleable core + a small fixed "escape hatch" vocabulary) is the more honest design.
- Multi-speaker disambiguation sub-benchmark: success rate on utterances issued by 2+ simultaneous speakers referencing different objects; report separately, since this is exactly the condition MozArt names as unsolved and outside the single-speaker elicitation study's scope.

---

## 7. Evaluation Study (Main Study, N = 36)

### 7.1 Design overview and power framing
- Between-subjects, 12 triads (N=36), 4 triads/condition, 2 real/realistic parcels × 2 triads/parcel/condition.
- **Explicit exploratory framing, not confirmatory-only:** with 4 clusters/condition and individual-level Likert DVs nested within teams, we report a **sensitivity analysis** ("this design can detect between-condition effects of size *d* ≥ [X] at 80% power, given ICC estimate [from formative/pilot data]"), rather than claiming the study is powered to detect a pre-specified effect size. We designate a small number of **primary/confirmatory** DVs (§7.3) analyzed with pre-registered tests and multiple-comparison correction, and a larger set of **secondary/exploratory** DVs reported descriptively and flagged as such, avoiding VizCrit's own critiqued pattern of many uncorrected tests across many DVs at a comparable cell size.
- Multilevel/mixed-effects models (participant nested in team) for all individual-level DVs, explicitly to avoid the pseudoreplication problem flagged in Shepherding the Crowd's ANOVA.

### 7.2 Pre-registration
OSF pre-registration before data collection: primary hypotheses (H1 H3 below), primary vs. secondary DV designation, planned mixed-effects models + correction method (Holm-Bonferroni across the primary-DV family), stopping rule (fixed N=36, no optional stopping), and the full Arnstein-ladder coding procedure (§7.5). State explicitly that the formative study was not and did not need to be pre-registered, to make the contrast legible to reviewers.

### 7.3 Conditions, hypotheses, and measures

**Condition A, Manual (baseline).** Voice + direct-manipulation 3D authoring; compliance shown only as a binary pass/fail badge (Module 1, §5.3); no AI facilitator; no value-surfacing prompts; group self-organizes.

**Condition B, Awareness-centered AI facilitation.** Adds Module 2: plain-language constraint explanations + periodic value-surfacing convergence prompts. No proactive alternative-generation.

**Condition C, Solution-centered AI co-design.** Adds Module 3: proactive generation of 2 to 3 concrete compliant alternatives the group can preview/accept/remix/reject, on top of everything in B.

**Primary (confirmatory) DVs**, one per condition-contrast hypothesis:
- **H1 (A vs. B vs. C, buildability):** Expert-rated buildability/compliance-credibility of the final exported design, rated by 2 professional planners/architects blind to condition (rubric in Supplementary §S4), analyzed via mixed-effects ordinal regression with team as random effect.
- **H2 (A vs. B vs. C, legitimacy):** Single-item self-report, "I would trust this proposal to be presented to the planning commission as seriously as a developer's proposal" (7-pt Likert, new item, full wording and validation notes in Supplementary §S5, since no off-the-shelf scale exists for this construct, per rigor item #9).
- **H3 (A vs. B vs. C, inclusion/alignment/compromise):** The 6-item Value-Centered Framing (Park/Hou/Sundu/Dow, CI 2025) instrument, adapted from a discrete-idea civic probe to a continuous spatial design task, full item wording reused verbatim where possible (Supplementary §S5).

**Secondary (exploratory) DVs**, reported descriptively, not corrected:
- Arnstein-ladder-coded participation depth (§7.5).
- Constraint comprehension (pre/post multiple-choice quiz about the specific site's zoning).
- Confidence/self-efficacy (Self-Reflective-Crowds-style item battery) reported *alongside*, not substituting for, H1's objective quality measure, explicitly to test for the confidence/quality dissociation that paper found.
- SUS-adapted usability of the voice interface (wording adaptation process documented per Reinders et al.'s precedent).
- Design divergence/exploratory breadth: count of distinct massing/unit-mix alternatives seriously considered per team (a direct multi-user extension of Prototyping Dynamics' DV).
- Recalled-baseline comparison item (§0.1).
- **AI-homogenization check (Conditions B and C only):** embedding-similarity and verbatim-repetition analysis of AI-generated facilitation prompts/suggested alternatives across the 4 independent teams within each condition, directly modeled on Cueing the Crowd's 182/578-verbatim-repetition finding.
- **AI-suggestion acceptance/modification log (Condition C only):** proportion of proposed AI alternatives accepted unmodified vs. remixed vs. rejected, a check against passive "rubber-stamping" of AI-generated design content substituting for genuine community judgment.

### 7.4 Detailed per-condition success/null criteria

**Condition A (Manual/baseline), what success looks like:**
Teams can complete and export a design within the session (no floor effect, at least 9/12 teams across all conditions produce an exportable design, but A's completion may be slower/rougher). Expert-rated buildability in A is measurably lower than C (e.g., planners flag compliance issues on first export in roughly 50 to 70% of A's designs vs. a much lower rate in C). Qualitative log/session data show repeated "invisible wall" friction moments, participants attempting a move, being blocked with no explanation, and audibly expressing confusion ("I don't get why it won't let me do this"), establishing that binary-only feedback is a real, felt limitation, not a strawman. Comprehension-quiz gains in A are smaller than in B/C.

**Condition A, what a null/negative result looks like:**
(a) A produces designs statistically indistinguishable from B and C on buildability and legitimacy, would mean the AI-facilitation manipulation (the paper's central interaction contribution) adds nothing measurable, a genuine negative result to report honestly rather than explain away; (b) A teams fail so badly (e.g., <50% complete an exportable design at all) that floor effects make the A-vs-B/C comparison uninterpretable, a pilot-testing failure to catch *before* the main study, not something to discover after; (c) A teams show *higher* self-reported legitimacy or ownership than B/C (a plausible "I built this myself, unassisted" ownership effect), a genuinely interesting negative-for-the-AI-facilitation-thesis finding that must be reported, not suppressed, since it would meaningfully qualify the paper's claims about AI facilitation's value.

**Condition B (Awareness-centered), what success looks like:**
Constraint comprehension gains significantly exceed A (e.g., +20 to 35 percentage points on the pre/post quiz vs. A, medium-large effect), and self-reported inclusion/alignment (H3) is significantly higher than A (Cliff's delta ≈ 0.35 to 0.5), consistent with Value-Centered Framing's original finding replicating in this new continuous/spatial domain. Session logs show value-surfacing prompts are followed by observable group behavior change (e.g., an explicit values-statement precedes a subsequent design decision in a majority of surfaced instances). Critically, **buildability (H1) in B is not necessarily higher than A**, mirroring VizCrit's own dissociation between feeling more informed and objectively performing better, and this would be reported as a *supporting*, not disconfirming, finding, since it replicates a known and theoretically expected pattern.

**Condition B, what a null/negative result looks like:**
(a) No difference from A on comprehension or inclusion, would suggest binary compliance badges already convey sufficient awareness, and the "awareness-centered" tier of the actionability spectrum is not doing independent work in this domain (a real possibility worth naming, since VizCrit's own textbook-vs-awareness contrast was not its strongest result either); (b) participants report the value-surfacing prompts as naggy/over-constricting (DesignWeaver's flagged risk), e.g., open-ended comments like "it kept stopping us to ask what we valued when we just wanted to build" appearing in a substantial minority of B sessions, a concrete, reportable interaction-design failure mode, not a footnote.

**Condition C (Solution-centered), what success looks like:**
Buildability (H1) in C significantly exceeds both A and B (e.g., planners rate ≥70 to 90% of C's exported designs as compliant/buildable on first pass vs. ≤50% in A, a large effect), and legitimacy (H2) is highest in C (e.g., mean Likert 5.5 to 6/7 vs. 3.5 to 4.5/7 in A). Design-divergence/exploratory-breadth (secondary DV) is significantly higher in C than A/B, a direct multi-user, civic-domain replication of Prototyping Dynamics' "share multiple beats share one" finding. The AI-suggestion acceptance/modification log shows a *healthy mix* of accept/remix/reject (e.g., no single behavior exceeding ~60% of instances), evidence participants are exercising real judgment over AI-suggested content rather than rubber-stamping it, which is what "artifact gap closed via authentic community authorship" actually requires.

**Condition C, what a null/negative result looks like:**
(a) Buildability rises but comprehension does *not* (a reversed VizCrit-style dissociation: designs get better because the AI generates compliant geometry, not because the community understood the constraints better), a genuinely important negative finding for the "empowerment" framing, since it would suggest C closes the *artifact* gap while reopening a *knowledge* gap, and must be discussed as a real tension, not smoothed into a success narrative; (b) **AI-suggestion acceptance rate is near-ceiling (e.g., >90% accepted unmodified)**, a red flag that the AI is substituting for, not empowering, community judgment, directly undermining the "authentic community authorship" legitimacy claim, and must be reported prominently, not buried in a supplementary table; (c) **the homogenization check shows high cross-team similarity** in C's AI-suggested alternatives or facilitation language (e.g., a specific massing suggestion or explanatory phrase recurring near-verbatim across 3+ of the 4 independent C teams), a direct, damaging replication of Cueing the Crowd's 182/578 finding in a domain where it is far more consequential (independent *communities*, not brainstorming dyads, being nudged toward convergent designs), this must be reported as a first-order limitation on the paper's central legitimacy claim, with a concrete proposed mitigation (e.g., Wikipedia-grounded-regeneration-style diversity injection, per Cueing the Crowd's own partial fix) rather than treated as a footnote.

### 7.5 Arnstein-ladder coding scheme
- Defined *before* data collection (per rigor item #6): operationalize what counts as informing / consultation / placation / partnership / citizen control at the level of a single session's transcript + final-artifact disposition (e.g., "citizen control" requires evidence the group's own priorities, not an AI suggestion or facilitator prompt, determined a load-bearing design decision).
- ≥2 independent coders, calibrated on a 20%+ subsample (WeDesign's practice as a floor, not a target), Cohen's kappa reported for the full corpus (BoundarEase's 0.68 as the comparison benchmark to beat or match).

### 7.6 Mixed-methods triangulation (CHI-specific, item B1)
At least 3 of: self-report (H2, H3, secondary Likert battery) + behavioral/log data (acceptance/modification rates, homogenization metrics, value-prompt-to-decision linkage) + expert rating (H1) + qualitative interview (post-session semi-structured debrief, thematically coded). All four are planned here, exceeding the stated minimum.

*(Full statistical tables, the complete Likert item wordings, the full Arnstein codebook, and the full planner-rating rubric go to Supplementary §§S4 S6; the main body reports only the primary-DV results table and effect sizes.)*

---

## 8. Results

Structure as a **template to be populated**, not pre-written outcomes (this is a proposal/registered-report-style structure, not a report of data that doesn't yet exist):

- 8.1 Participant/team demographics and recruitment representativeness (honest reporting against the legitimacy claim, rigor item #7).
- 8.2 Technical benchmark results (voice pipeline accuracy + error taxonomy frequencies; compliance-checker kappa), reported independently of the human study, per UIST norms.
- 8.3 Formative study findings → design goals table (repeated/cross-referenced from §4.2).
- 8.4 Primary DV results (H1 H3), mixed-effects model outputs, effect sizes, Holm-Bonferroni-corrected p-values, presented condition-by-condition with a single summary figure (e.g., a forest plot of standardized effects across A→B→C).
- 8.5 Secondary/exploratory DV results, explicitly labeled as such, including the AI-homogenization and AI-acceptance-rate findings **regardless of direction** (rigor item B5, report null/negative results with the same prominence as positive ones).
- 8.6 Arnstein-ladder coding results with inter-rater reliability statistic.
- 8.7 Qualitative themes from post-session interviews, illustrative quotes per theme (both confirming and disconfirming quotes included).

---

## 9. Discussion

- Interpret H1 H3 jointly against the actionability-spectrum extension (Contribution C1): does the multi-user, legally-hard-constrained domain reproduce VizCrit's self-perception/objective-quality dissociation, extend it, or break it?
- Interpret the voice/malleable-grammar benchmark against Contribution C2: does constraint-generated voice affordance actually lower the expertise floor versus a fixed grammar, or does the Digital-Modeling-for-Everyone-style diversity of novice selection strategies dominate regardless of grammar design?
- Directly confront the AI-homogenization and AI-acceptance-rate findings here, whatever they show, this is where the paper's "authentic community authorship" legitimacy claim is either earned or qualified.
- Revisit the artifact-gap thesis (Contribution C3) in light of H1/H2: did producing a buildable, expert-rateable artifact actually change self-reported legitimacy relative to what the literature reports for comment-only or image-only processes (WeDesign, BoundarEase), even absent a live 4th baseline arm (cross-reference the recalled-baseline item, §0.1)?
- Explicit discussion of expectation-inflation/over-trust risk (rigor item #18): how PARCEL signals AI-suggestion confidence/uncertainty, and whether participants' post-session interviews show any conflation of "AI-passed compliance check" with "legally guaranteed."

---

## 10. Limitations

Each limitation names a specific mechanism and a specific proposed follow-up (per rigor item #12), not a boilerplate paragraph:

1. **No live status-quo comment-only baseline arm**, mitigated by the recalled-baseline item (§0.1), but the causal "vs. commenting" claim needs a follow-up field deployment embedded in a real, live comment period (BoundarEase-style) to fully establish.
2. **Single-session, ~2.5-hour exposure**, no data on sustained multi-session civic process use (per Orca's own flagged limitation); follow-up needs a longitudinal deployment across a multi-meeting rezoning process.
3. **Recruitment skew risk**, name the actual observed skew (e.g., toward already-engaged, higher-tenure residents, mirroring BoundarEase's own honestly-reported skew) and propose targeted outreach/incentive-structure changes for a follow-up.
4. **Two-parcel, single-city (San Diego) scope**, zoning-code generalization to other jurisdictions is unevaluated; name the specific constraint-graph-portability engineering work a multi-city follow-up would require.
5. **AI-homogenization risk**, whatever the magnitude found, name the specific diversity-injection mitigation (Wikipedia-grounded-regeneration-style, per Cueing the Crowd) as the concrete next experiment, not just "future work."
6. **Cluster count (4 teams/condition)** limits statistical power for team-level (as opposed to individual-level) inference, name the specific N of teams a fully powered confirmatory replication would need, computed from this study's own observed ICC.
7. **Expert-rater pool size** (2 planners/architects), name inter-rater reliability observed and the specific rater-pool expansion a follow-up would need if reliability is marginal.

---

## 11. Conclusion

Restate the artifact-gap thesis, the three theoretical contributions, and the concrete empirical pattern found (populated after data collection), end on the specific next study needed (the live-deployment, real-decision-outcome follow-up named in Limitations #1), positioning this paper as opening rather than closing the "citizen co-creation of a buildable alternative" gap the Luo et al. review identifies as almost entirely unaddressed in the field.

---

## 12. Supplementary Materials, Contents

- **S1.** Full per-paper related-work synthesis (all ~25 papers, limitation + extension analysis), the complete version of the compressed §3.
- **S2.** Formative study: full interview protocol, workshop activity materials, codebook, saturation-curve data, full demographic table.
- **S3.** Extended system architecture: Pascal fork/diff details, constraint-graph schema, MCP tool definitions used by the AI facilitation pipeline, multi-user sync protocol details, IFC export schema.
- **S4.** Full statistical tables: all mixed-effects model outputs (fixed + random effects, ICC estimates), full correction-adjusted p-value tables, sensitivity/power analysis calculations, planner-rating rubric (full item wording + anchors).
- **S5.** Full survey instrument wording: the new single-item legitimacy measure (H2) with validation/pretesting notes; the adapted Value-Centered-Framing 6-item instrument with adaptation notes; the SUS-adaptation wording-change log (per Reinders et al. precedent); the constraint-comprehension quiz; the confidence/self-efficacy battery.
- **S6.** Full Arnstein-ladder codebook (rung definitions operationalized for this domain), coder calibration procedure, full reliability statistics.
- **S7.** Voice-command grammar/vocabulary reference, full elicitation-study transcripts summary, full voice-pipeline error-taxonomy with per-category example utterances and frequency counts.
- **S8.** OSF pre-registration document (verbatim) for the main N=36 study.
- **S9.** Anonymized interaction logs (to the extent privacy/IRB terms allow) and/or a released interactive demo build or video figure of PARCEL (UIST rigor item C6).
- **S10.** Full limitations-to-follow-up-study mapping table (expanded version of §10).

---

## Cross-Reference: Priority Gap List Coverage

| Priority gap (from rigor audit) | Where addressed in this structure |
|---|---|
| 1. Power analysis / exploratory framing | §7.1 |
| 2. Pre-registration | §7.2, S8 |
| 3. Arnstein coding + IRR | §7.5, S6 |
| 4. Baseline/comparison justification | §0.1 |
| 5. Formative→design-goal traceability | §4.2 |
| 6. Real/expert outcome measure | §7.3 (H1), S4 |
| 7. AI-homogenization check | §7.3 (secondary DVs), §7.4 (Condition C), §9 |
| 8. Technical benchmark + error taxonomy | §5.2, §6.3, §8.2 |
| 9. Recruitment demographics honesty | §4.1, §7.1, §8.1, §10.3 |
| 10. Materials/appendix package | §12 (S1 S10) |
