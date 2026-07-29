# CHI / UIST Rigor Checklist, Self-Audit for the Pascal-Based Community Co-Design Paper

**How to use this:** work section by section before submission. Each item states the criterion, which venue(s) it matters most for, and a verdict against your stated design, a **formative study (N=20 to 24)** feeding into a **3-condition main study (N=36)**. Where I don't know a design detail (e.g., between- vs. within-subjects, what the 3 conditions actually manipulate), I've flagged it as an **[ASSUMPTION, confirm]** rather than guessing silently.

**Stated design as I understand it** (confirm/correct before using this checklist further):
- Formative: N=20 to 24 community members/stakeholders, likely interviews + workshop(s), needs-finding → design requirements (BoundarEase-style two-phase, or WeDesign/Value-Centered-Framing-scale recruitment).
- Main study: 3 conditions, N=36, this is the *exact* N and condition-count of VizCrit (CHI 2026, ~12/cell, between-subjects). If you are consciously mirroring that design (e.g., three levels of AI facilitation/feedback actionability, or three levels of value-framing à la Park/Hou/Sundu/Dow CI'25), say so explicitly in your methods section, reviewers who know these papers will recognize the number and will read it as either a deliberate homage/precedent or an under-powered coincidence.

---

## PART A, Combined Checklist (matters for both CHI and UIST)

### 1. Sample size justification / power analysis
**Why it matters:** Both venues now expect either a power analysis or an explicit, non-circular justification for N (Dhakal et al.'s 168K is justified by subgroup-breakdown claims; VizCrit's N=36/~12-per-cell is *not* power-justified and multiple reviewers in our own critique set flagged it as likely underpowered for its 6 DVs).
**Our status: MISSING / WEAK.** No power analysis is mentioned. N=36 across 3 conditions (~12/cell) is exactly VizCrit's cell size, which our own lit-review critique explicitly calls underpowered for multi-DV comparisons. If our main study has more than 2 to 3 outcome measures (likely, given zoning-compliance + creativity + inclusion + rapport + confidence would all be relevant), 12/cell is thin.
**Action:** Add a short power analysis (even a post-hoc sensitivity analysis, "our N=36 can detect effects of size d≥X at 80% power") or explicitly frame the N=36 study as formative/exploratory rather than confirmatory (UIST reviewers accept this, StoryEnsemble and Orca both explicitly framed small-N single-condition studies as "exploratory" citing precedent). Pick one framing and be consistent; don't claim confirmatory power you don't have.

### 2. Formative-to-design traceability
**Why it matters:** CHI/UIST reviewers reward a visible chain from formative finding → numbered design requirement → system feature → evaluated claim (BoundarEase's affinity-diagrammed formative→design pipeline; Graphologue's C1 C4 findings → D1 D4 design goals; DesignWeaver's expert-interview themes → dimensional scaffold).
**Our status: LIKELY PARTIAL.** You have a formative N=20 to 24 study, but nothing in what you've shared states numbered findings/design-goals that map onto specific tool features (voice command set, AI facilitation style, zoning-feedback surfacing).
**Action:** Write an explicit "Formative Findings → Design Goals" table (F1→D1, etc.) mirroring Graphologue/DesignWeaver. This is cheap to add and is one of the highest-leverage rigor signals in this literature.

### 3. Baseline / comparison condition
**Why it matters:** CHI wants a clear counterfactual (status-quo commenting tool, or single-developer-proposal condition) to support "legitimate alternative" framing. UIST accepts *omitting* a baseline only with an explicit, argued justification (Orca: "conventional tabbed browsing already serves as participants' de facto baseline"; A11yShape: "existing tools are effectively unusable... making such comparisons uninformative").
**Our status: NEEDS CLARIFICATION.** [ASSUMPTION, confirm] If your 3 conditions are non-AI-facilitation vs. two AI-facilitation styles (or similar), you likely lack a true "status quo civic engagement" (comment-only / developer-proposal-only) baseline arm. Given the paper's whole thesis is "co-design vs. commenting," this is a significant gap if absent.
**Action:** Either (a) add a 4th "commenting-only" condition, or (b) explicitly justify its omission the way Orca/A11yShape do, citing that the comparison would be uninformative/impossible to run ethically in a single-session study.

### 4. Statistical test matched to data type, small-n, ordinal data
**Why it matters:** Value-Centered Framing and VizCrit both correctly used Friedman/Kruskal-Wallis + Bonferroni-corrected Wilcoxon/Mann-Whitney for Likert data rather than defaulting to ANOVA; HapticBots used Wilcoxon signed-rank + repeated-measures ANOVA appropriately matched to design.
**Our status: TBD, not yet specified.**
**Action:** Pre-decide (and pre-register, see #13) whether the 3-condition design is between- or within-subjects, then commit to non-parametric omnibus + corrected pairwise tests for any Likert-scale DV. State the correction method (Bonferroni/Holm) explicitly.

### 5. Multiple-comparisons correction across DVs
**Why it matters:** Several papers in our own lit review (Value-Centered Framing, Cueing the Crowd) ran many separate tests across several DVs/covariates without family-wise correction, flagged as a weakness in each critique.
**Our status: TBD.**
**Action:** If you report 5+ Likert DVs (inclusion, alignment, confidence, compliance-feedback usefulness, willingness-to-compromise, etc.) across 3 conditions, either correct across the full DV family or pre-register a small number of primary/confirmatory outcomes vs. secondary/exploratory ones, and label them as such in the paper.

### 6. Inter-rater reliability for qualitative/behavioral coding, **explicitly flagged, including Arnstein's-ladder coding**
**Why it matters:** This is one of the most consistently criticized gaps across the whole lit set: WeDesign computed kappa on only 20% of its data; CrossTalk's formative coding reported no IRR at all; A11yShape and StoryEnsemble both skipped IRR; Cueing the Crowd asserted "strong intercoder agreement" with no number reported. BoundarEase (kappa=0.68) and Self-Reflective Crowds (kappa>0.7, ICC 0.8 to 0.9) are the positive exemplars.
**Our status: MISSING, this is a named gap in your prompt, and rightly so.** If any part of the paper codes participation depth against **Arnstein's Ladder of Citizen Participation** (as the Urban Digital Twins review and the Sassmannshausen AR paper both do, and as would be natural for positioning your tool's "co-design vs. commenting" claim), you currently have no stated coding scheme, number of coders, or reliability statistic for that coding.
**Action (concrete):**
- Define the Arnstein-ladder coding scheme *before* data collection: what counts as "informing," "consultation," "placation," "partnership," "citizen control" in the context of a single interaction/utterance/session.
- Use ≥2 independent coders, calibrate on a subsample (e.g., 20%, per WeDesign precedent, but state this is a floor, not a target), report Cohen's kappa or ICC for the full corpus if feasible (BoundarEase/Self-Reflective Crowds level, not WeDesign level).
- Do the same for any other qualitative coding (thematic analysis of interviews, error taxonomies for voice-command failures, etc.), report coder count, calibration procedure, and a reliability statistic every time, or explicitly state (as CrossTalk/Graphologue do) that IRR was not collected and why.

### 7. Recruitment representativeness vs. the claims being made
**Why it matters:** This is the single most repeated critique across the entire civic-design cluster: WeDesign, BoundarEase, Value-Centered Framing, and the AR/Oslo paper all recruit already-engaged, often higher-SES, self-selected participants while making claims about "inclusion," "equity," or "community legitimacy."
**Our status: TBD, but high risk given the paper's central legitimacy claim.**
**Action:** Report exact recruitment channel(s), demographic breakdown (age, SES/income proxy, tenure/homeownership, prior civic engagement), and explicitly discuss how this compares to the population your "alternative to a developer's proposal" claim needs to represent. If recruitment leans toward already-engaged parents/homeowners (as in BoundarEase), say so plainly rather than let a reviewer discover it.

### 8. Real-stakes / ecological validity of the design task
**Why it matters:** BoundarEase and WeDesign both benefit from being embedded in real, live civic processes; Value-Centered Framing and VizCrit are explicitly critiqued for using static, non-consequential design probes with no real outcome.
**Our status: TBD.**
**Action:** State plainly whether the study's zoning/parcel is a real, currently-contested site (strongest) or a synthetic scenario (weaker, but acceptable if disclosed). If synthetic, note this as a limitation the way Value-Centered Framing does, and note what a "real deployment" follow-up would need (per BoundarEase's phase-2 caveat about GPT-3.5 synthetic personas standing in for real community voice, don't let your AI-generated feedback/personas quietly substitute for real stakeholder input without flagging it).

### 9. Measurement validity, validated instruments vs. ad hoc scales
**Why it matters:** REP-derived value taxonomy (Value-Centered Framing), CSI (VizCrit, StoryEnsemble), SUS (A11yShape, Reinders et al., Orca), SVI + IOS (Prototyping Dynamics) are all validated instruments reused/adapted rather than invented from scratch. Several papers (Cueing the Crowd's 4-item self-efficacy battery, Sensecape/Orca's unnamed custom Likert items) are dinged for ad hoc, unvalidated items.
**Our status: TBD.**
**Action:** For every subjective DV (inclusion, legitimacy, sense of ownership, confidence, creativity), check whether a validated scale exists (CSI for creativity support; SVI/IOS for rapport; SUS/NASA-TLX for usability/workload) and use/adapt it with citation, rather than writing new items. If you must write new items (e.g., "sense of legitimacy vs. a developer's proposal" has no off-the-shelf scale), say so explicitly and report full item wording in an appendix, do what Value-Centered Framing did (Appendix A.1/A.2), not what Orca/Sensecape did (unnamed items, counts only).

### 10. Self-report vs. objective/expert-rated outcome divergence
**Why it matters:** VizCrit and DesignWeaver both found self-perceived quality/creativity/satisfaction diverging from expert-rated or behavioral outcomes, a now-expected sophistication in this literature.
**Our status: LIKELY MISSING if the study only has self-report DVs.**
**Action:** Include at least one independent/expert-rated outcome alongside self-report, e.g., a planner/architect (blind to condition) rating each group's final design for zoning compliance and design quality, paired with participants' self-reported confidence/satisfaction. This directly pre-empts the "does it just make people feel more heard, or actually produce a better/compliant design" critique that a CHI reviewer familiar with VizCrit would raise.

### 11. Reproducibility, data/code/materials release
**Why it matters:** Dhakal et al. released the full dataset; Kay et al. released analysis scripts; Value-Centered Framing published full survey instrument + value taxonomy in an appendix. CHI/UIST reviewers increasingly expect at least materials (protocols, item wording, codebooks), even when raw data can't be released for privacy/IRB reasons.
**Our status: TBD.**
**Action:** Plan a supplementary package now: full interview protocol, full survey/Likert item wording, Arnstein-ladder codebook, voice-command vocabulary/grammar, and (if feasible) anonymized interaction logs. Decide what's held back for participant privacy and say so explicitly.

### 12. Limitations reporting, specific, not boilerplate
**Why it matters:** The strongest papers in this set (WeDesign, BoundarEase, Sensecape, EM-Sense) name a *specific mechanism* and a *specific proposed fix or follow-up*, not a generic "future work" paragraph.
**Our status: to be written, but easy to get right** given how much precedent exists.
**Action:** For each known weakness (sample recruitment skew, single-site/single-zoning-scenario, short session length, no longitudinal/adoption data, AI-facilitation homogenization risk across independent groups), name the mechanism and a concrete follow-up study, mirroring Sensecape's P11-quote-grounded limitation or EM-Sense's physics-grounded failure-mode-by-failure-mode limitations list.

### 13. Pre-registration
**Why it matters:** None of the papers in your own lit review pre-registered (this is a known soft spot for this entire sub-field), so pre-registering would be a genuine differentiator, especially for the confirmatory 3-condition N=36 study.
**Our status: MISSING, explicitly flagged in your prompt.**
**Action:** Pre-register (OSF or AsPredicted) the main N=36 study: primary hypotheses, primary vs. secondary DVs, planned statistical tests + correction method, stopping rule/sample-size justification, and the Arnstein-ladder coding procedure. Keep the formative N=20 to 24 study explicitly exploratory/unregistered, that's normal and doesn't need registration, but say so to make the contrast clear to reviewers.

### 14. Multi-user/collaborative validity, no confederate partners
**Why it matters:** Spacetime's biggest, most citable weakness (per your own lit critique) is that every "collaborative" session paired a participant with a researcher-confederate, never two naive users, undermining its core multi-user claim. Your paper's whole premise is genuine multi-user community co-design, so this is a trap to explicitly avoid.
**Our status: NEEDS CONFIRMATION.** [ASSUMPTION, confirm] If your N=36/3-condition sessions group real community members together (not participant+experimenter), state this explicitly and contrast it with Spacetime's limitation in your related-work/methods section, this is a "free" novelty claim if true.
**Action:** If any condition pairs a participant with a confederate/experimenter rather than a peer, disclose it plainly and discuss the same construct-validity concern Spacetime's critique raises.

### 15. Order/counterbalancing effects
**Why it matters:** Value-Centered Framing (Friedman+RM-ANOVA check, dropped after null result but under-reported) and Sensecape (full factorial counterbalance) show two different rigor levels for handling order/topic confounds.
**Our status: TBD** depending on within- vs. between-subjects design.
**Action:** If within-subjects, counterbalance condition order and report the actual test statistics for the order-effect check (not just "no significant effect, thus omitted," which Value-Centered Framing was dinged for). If between-subjects, counterbalance/match the zoning scenario or site across conditions instead.

### 16. Real-decision / legitimacy outcome measure
**Why it matters:** This is your paper's central novel claim ("legitimate alternative," not just feedback), and it is explicitly the gap every related paper in your cluster (WeDesign, CoDesignAI, BoundarEase, the Oslo/AR paper) leaves open, none of them measure whether community-generated design input was actually adopted or treated as decision-equivalent to a professional proposal.
**Our status: HIGH-VALUE GAP, likely your single biggest opportunity.**
**Action:** Even a self-report proxy (e.g., "I would trust this proposal to be presented to the planning commission as seriously as a developer's proposal," compared pre/post) would be a genuine contribution beyond prior work. If you can get any planning-official or professional-architect judgment of the community-generated proposal's credibility/buildability, that closes the single gap every related paper explicitly leaves for future work.

### 17. AI-facilitation homogenization / bias check
**Why it matters:** Cueing the Crowd found that 182/578 AI-generated conversational cues repeated verbatim across independent groups, a real risk that AI facilitation nudges many different community groups toward convergent, "less authentically community-specific" outputs, directly undermining a legitimacy claim.
**Our status: LIKELY NOT PLANNED, worth adding.**
**Action:** If you run multiple independent groups through AI-facilitated conditions, check embedding-similarity/verbatim-repetition of AI-generated design suggestions or facilitation prompts across groups. This is cheap to add post hoc from logs and directly pre-empts a specific, citable critique.

### 18. Expectation-inflation / over-trust in AI-generated content
**Why it matters:** WeDesign (watermarking, "conversation starters not approvals") and BoundarEase (synthetic GPT-3.5 personas as a validity-limited stand-in for real community voice) both flag this. DesignWeaver shows scaffolding can raise perceived quality without raising satisfaction, because expectations outrun capability.
**Our status: worth a proactive design + reporting note.**
**Action:** State explicitly how the tool signals confidence/uncertainty in AI zoning-compliance checks and AI-suggested designs (don't let participants believe an AI "pass" is a legal guarantee), and discuss this explicitly in your ethics/limitations section.

---

## PART B, CHI-Specific Emphases

| # | Criterion | Status |
|---|---|---|
| B1 | **Mixed-methods triangulation** (self-report + behavioral/log + expert rating + qualitative interview) expected as standard, per VizCrit/BoundarEase/DesignWeaver | TBD, plan to include at least 3 of these 4 for the main study |
| B2 | **Values/inclusion framing grounded in a validated construct** (REP scale, Arnstein's Ladder, IOS/SVI) rather than invented constructs | Partially planned (Arnstein flagged), extend to inclusion/rapport measures too |
| B3 | **Diverse, non-convenience stakeholder recruitment** explicitly reported and honestly limited | TBD, high priority given legitimacy claims |
| B4 | **Structured deliberation/convergence mechanism** (value-framing before voting, not raw popularity) as the mechanism under test, per Park/Hou/Sundu/Dow | Confirm this is actually what your 3 conditions manipulate, or state what is |
| B5 | **Honest null/negative-result reporting** (e.g., no significant learning gain, no mental-effort cost) rather than only confirmatory framing | Plan to report all DVs regardless of significance |
| B6 | **IRB/ethics-adjacent discussion** of AI-generated content standing in for real community voice | Add explicitly, citing BoundarEase precedent |

## PART C, UIST-Specific Emphases

| # | Criterion | Status |
|---|---|---|
| C1 | **Clear systems/technical contribution separable from the human evaluation** (e.g., voice→scene-edit pipeline accuracy reported independently, à la LLMR's benchmark track, Graphologue's entity/relationship F-scores, A11yShape's OpenSCAD-generation accuracy) | MISSING if not planned, add a technical accuracy/benchmark evaluation of the voice-command-to-scene-edit and zoning-compliance-check pipelines, decoupled from the human study |
| C2 | **Error taxonomy for the technical pipeline** (per Graphologue's entity/relationship error types, VizCrit's heuristic-accuracy validation, Gazeify's referencing-error taxonomy) | MISSING, add a named taxonomy of voice-command misinterpretation / zoning-check failure modes with frequency counts |
| C3 | **Justification if no baseline/control condition is used**, explicitly argued (Orca, A11yShape precedent) rather than silently omitted | Needed if condition 4 (status-quo commenting) is not included, see A3 |
| C4 | **Ablation of system components** if claiming multiple mechanisms contribute (Capacitivo's electrode-geometry ablation, LLMR's pipeline-module ablation) | Consider for the AI facilitation/feedback pipeline if it has multiple stages (planner, compliance-checker, voice-NLU) |
| C5 | **Small-N expert/formative walkthrough is an acceptable substitute** for a full study if explicitly scoped as such (Spacetime's N=6, Belidor's "Olsen 2007" justification for no human study on a language/grammar contribution) | Useful cover for the formative N=20 to 24 study if some of it is closer to expert/system walkthrough than full deployment, but don't use this to justify skipping rigor on the *main* N=36 study |
| C6 | **Released interactive artifact/demo** for reviewer/reader inspection (Belidor's public Analogy Viewer) | Consider releasing a demo build or video figure of the Pascal-based tool |

---

## PART D, Priority Gap List (fix before submission, ranked)

1. **Power analysis or explicit exploratory framing for N=36/3-condition study**, right now this is naked and will draw the same critique your own lit review levels at VizCrit.
2. **Pre-registration of the confirmatory main study**, zero comparable papers in your space did this; it's a cheap, high-signal differentiator.
3. **Arnstein's-ladder (and any other) coding scheme + inter-rater reliability plan**, decided *before* data collection, not reconstructed after, this is explicitly named as missing in the prompt and is a top-3 recurring critique across the entire lit set.
4. **Baseline/comparison condition decision**, either add a true status-quo (commenting-only) arm or write an explicit justification for omitting one.
5. **Formative-finding → design-goal traceability table**, cheap, high-value, currently absent.
6. **A real or realistic outcome/legitimacy measure** (planner/expert judgment of buildability-credibility), this is the single biggest opportunity to out-rigor every paper in your own related-work set, all of which explicitly leave this open.
7. **AI-homogenization check across independent groups**, if multiple groups get AI facilitation, cheap to compute post hoc from logs, directly citable precedent (Cueing the Crowd) to pre-empt.
8. **Technical/benchmark evaluation of the voice→scene-edit and compliance-check pipeline**, decoupled from the human study, with a named error taxonomy, needed for UIST-style systems credibility regardless of which venue you target.
9. **Recruitment demographics reported honestly against the legitimacy claim**, with explicit discussion of representativeness limits.
10. **Materials/appendix package planned now**: full item wording, codebooks, voice-command grammar, protocols, decide what's releasable and draft it alongside the study, not after.