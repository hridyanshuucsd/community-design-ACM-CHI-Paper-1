RESEARCH PROPOSAL

# From Consultation to Co-Authorship
### A Collaborative 3D Design Tool for Community Spatial Agency in Urban Planning

**Hridyanshu**
UC San Diego, Mathematics–Computer Science B.S.
**Proposed for: ACM CHI 2027 (submission deadline January 2027)**
**Proposed mentors/collaborators: Mai Thi Nguyen · Steven Dow · Haijun Xia**
**Field site: Delhi–NCR (Noida), India**

*Revision note: the formative and main study now run in Delhi–NCR, specifically Noida, rather than San Diego — this is where I personally already have community and local contacts, which makes real (not simulated) recruitment feasible on the timeline this paper needs. This is a genuine pivot, not a cosmetic one: Mai's and Steven's roles shift from "provide access to a real community and city process" to theoretical and methodological advisors, since their existing institutional access (CHPD, Design for San Diego) is San Diego-specific and doesn't transfer. I'm stating that reframing plainly below and flagging it as open to discussion — if either of them would rather this stay San Diego-based, or wants to be involved differently, that's a real conversation to have before anything is locked in. System infrastructure (data storage, backend) can still run through a UC San Diego–hosted Google Workspace/cloud environment if that matters for IRB and data-governance purposes, independent of where the field site is.*

---

## Why I Am Writing This

I have spent the last several months building CLARO, a 3D spatial intelligence platform for San Diego that combines photorealistic Google Maps tiles with dynamic, user-defined UI and AI interaction. In the process, I kept running into the same problem: the people most affected by decisions about built space — renters, long-time neighborhood residents, communities that have been displaced or are fighting displacement — have almost no real tools to participate in those decisions.

They can show up to the town hall. They can fill out a comment card. They can speak for two minutes into a microphone. Then the professionals go back and do whatever they were going to do anyway.

This proposal is about fixing that. Specifically, I want to build a novel multi-user, AI-assisted, **voice-first** 3D design tool that lets a community group design built space together — not comment on someone else's design, but build their own, with all the zoning rules built in automatically, producing a real architectural proposal they can put on the table as a legitimate alternative to what a developer is offering. Voice-first is not a convenience feature here. A resident who has never touched CAD software should be able to say "put the entrance on the park side" and see it happen, the same way they'd say it to an architect standing next to them. The 3D view stays as the shared visual ground truth; voice is how a layperson actually acts on it.

I am writing to you, Mai, because this is the computational form of the problem your research is about — your work on spatial justice and who gets excluded from decisions about the built environment is the theoretical spine of this paper, wherever it is fielded. I am writing to you, Steven, because your lab's work on feedback exchange and value-centered convergence in civic design is the closest existing empirical foundation for what I want this tool to do. And I am writing to you, Haijun, because a voice-first interface for a domain this constrained cannot be a fixed command grammar — it has to be generated from the same live constraint graph your malleable-UI work argues an interface should come from, and your Spacetime work on multi-user collaborative 3D editing is exactly the multi-user architecture this needs. Between the three of you, this project has the theoretical grounding, the feedback/convergence methodology, and the interface theory it needs to be more than a demo.

The community and city access itself — the part that makes the study real rather than simulated — is something I already have: Delhi–NCR, specifically Noida, is where I have personal community relationships and local contacts, built independent of this project. I want to be upfront that this means Mai's and Steven's roles here are advisory rather than access-providing, which is a real change from how I'd have framed this if the field site stayed in San Diego, and I say more about that below.

I am also self-funding the engineering and the entire R&D. I have my own funds and donors for everything I do. I want to be upfront about that. I am not asking for lab resources or research budgets. I am asking for your guidance and your expertise.

---

## The Problem

### Participation in urban planning is mostly theater

There is a concept in planning theory called Arnstein's Ladder of Citizen Participation, from a landmark 1969 paper still quoted in every urban planning classroom. It describes eight rungs of participation, from the bottom (manipulation, where you are told what is happening and expected to be grateful) to the top (citizen control, where communities have real decision-making power). Most digital participation tools in 2026 sit at rungs 3 through 5. They help communities be informed. They help communities be consulted. They do not give communities the power to shape outcomes. Literature has a word for this: tokenism.

The reason is structural. Designing built space requires expertise that most communities do not have: knowledge of what is physically buildable, what local zoning permits, how to represent a proposal in a form a planning commission will take seriously. The moment you hand a community group a design tool, the gap in expertise becomes a gap in power. A professional planner can say something is not buildable and end the conversation, because the community has no way to verify whether that claim is even true.

> **The core insight.** If the constraints are built into the tool itself — not in a rulebook that requires a lawyer to read, but in the spatial physics of the editor — then the expertise gap closes. Communities can produce designs that are already constrained to what is possible. The constraint is no longer a weapon in the hands of professionals. It becomes a shared material fact visible to everyone in the room.

### What the existing tools get wrong

I looked carefully at every digital participation tool published at CHI, IEEE VR, CSCW, and CI over the last several years, plus the planning literature. Here is what each does and where it falls short:

- **WeDesign (2025)** uses AI to generate images during community consultations about public space. A study in Montreal showed it helped people express ideas, but also revealed failures that matter directly: the system could not accurately visualize the needs of marginalized groups (one participant typed "wheelchair-friendly benches" and the system generated random wheelchairs everywhere). At its core it generates images, not editable designs. You cannot submit an image to a planning commission. You cannot modify it. You cannot verify it follows the rules.
- **CoDesignAI (2026)** uses multiple AI agents to facilitate urban design discussions integrated with spatial mapping. It helps people talk. The AI agents hold expert knowledge residents do not, which reproduces the very expertise asymmetry it appears to address. The output is a conversation summary, not a design proposal.
- **BoundarEase (CSCW 2025)** is the most methodologically rigorous civic-tech paper in this space, and I intend to model my study design closely on it. It is a platform for community engagement around school attendance boundary policies, built with a real school district serving nearly 150,000 students. It showed the tool helped people think collectively rather than individually — but people were choosing between options someone else created. It sits at rung four: consultation. My system aims for rung six: partnership.
- **Value-Centered Framing for Participatory Civic Design (Park, Hou, Sundu, Dow — CI 2025)** is the closest existing precedent, and it comes directly out of Steven's own lab. In a within-subjects study (N=24) using a civic decision-making probe for a local recreational park, surfacing participants' personal values before convergence — rather than converging on raw idea popularity — significantly increased participants' sense of inclusion, alignment with community values, and willingness to compromise, without increasing perceived effort. This is strong evidence that *how* a group converges on a shared civic design matters as much as whether they get to design at all. But the domain is discrete: a list of park amenities to vote on. It has not been tested in a domain with continuous, legally-constrained spatial design, where the "idea" is a 3D building and convergence means resolving where a wall goes, not which item wins a vote.
- **Participatory AR tools** let people view 3D models in augmented reality. People can see. They cannot edit freely. Viewing is informing. Editing is a prerequisite for co-authorship.

None of these produce what I am calling a **design artifact**: something geometrically correct, legally compliant, and formally submittable to a planning process as an alternative proposal. That is the gap.

---

## What We Are Building

### The System

Imagine a group of ten residents of a Noida group housing society sitting together around a screen. Their society's redevelopment — because the original builder stalled, went insolvent, or a court/authority order requires it, the kind of dispute that has left thousands of Noida and Greater Noida homebuyers in limbo for years — is now being decided by a court-appointed body, the developer's successor, or the Noida Authority itself. The residents have been invited to a stakeholder meeting.

Instead of a comment sheet, they open our tool. It shows them their plot in 3D, at real scale, with the existing structure and surrounding buildings. It shows them the building envelope — the maximum volume the applicable Noida Authority building bylaws and Floor Area Ratio (FAR/FSI) allow, including setbacks and ground coverage limits. A wall cannot be dragged outside the envelope. If they try to build taller than the permitted height, the wall resists. The rules are not in a bylaw document they have to read — they are in the space itself, the same physics that turned the Supertech Emerald Court twin towers into a Supreme Court–ordered demolition when a real building violated exactly these norms.

Now they start designing. One person suggests reconfiguring parking to free up ground-floor common space. Another wants units reoriented to face the internal courtyard rather than the road. They argue about how many towers versus how much open/amenity space the redevelopment should have. They can see their disagreements spatially, in 3D, in real time — and when the tool surfaces *why* a move is or isn't allowed, it does so the way Steven's VizCrit work frames computational design feedback: not just a hard stop, but feedback with a spectrum of actionability, from "here's the rule" to "here's what would work instead" — spoken in Hindi, English, or the code-switched mix people in Delhi–NCR actually use.

At the end of the session, they export a floor plan, a 3D render, a compliance summary showing the design meets applicable building bylaws, and a full editable file an architect or the redevelopment's project consultant could pick up and continue from. They have produced a legitimate design proposal that can sit across the table from whatever the builder, the resolution professional, or the authority is offering as an equal alternative.

### The three technical layers

**Layer 1 — The constraint model.** Given a real plot/society address, the system builds a spatial constraint graph: plot boundary, existing structures, setbacks, the allowable building envelope (height limit, Floor Area Ratio/FSI, ground coverage), required clearances, allowable uses, and applicable overlay conditions. These become the physics of the editor: the constraints are not text, they are encoded in what the tool will and will not let you do. The constraint graph is a live, queryable structure the interface subscribes to as a first-class input, not a validation pass run after the fact — affordances are generated from rules *before* you design, not checked *after*.

For Delhi–NCR/Noida this means encoding Noida Authority building bylaws, applicable UP-RERA project norms, and (where relevant) Uttar Pradesh Apartment Act provisions for existing group housing societies, rather than the California zoning/ADU-permit data my earlier CLARO work was grounded in. That earlier work (matching HCD permit records against real property geometry for San Diego) is still the methodology I'm reusing — ground-truthing the constraint model against what actually gets approved, not just what the bylaw technically says — but the underlying dataset for this paper has to be built fresh for Noida during the formative study, using my local contacts to identify what documentation (sanctioned building plans, RERA filings, authority correspondence) is actually available to work from.

**Layer 2 — Constraint feedback as a first-class interaction, not a validation layer.** This is where Steven's research is directly load-bearing. VizCrit (CHI'26) shows that computational feedback in a design tool is not a single thing — it spans an actionability spectrum from static, textbook-style flags to awareness-centered annotations to solution-centered suggestions that show the fix. VizCrit built this for 2D visual design heuristics (contrast, alignment, hierarchy) for a single designer. I am extending that framework into a domain it has never been applied to: 3D architectural constraints that are legally hard (not heuristic), spatial rather than visual, and shared in real time by a *group* rather than a single author.

Concretely: if a wall exceeds the height envelope, the system does not just refuse the move — it can show the envelope boundary (awareness), or suggest a compliant alternative massing (solution-centered), the same way VizCrit's annotations scale from "there's an issue" to "here's the fix." If the zoning allows an upper-floor addition, that control becomes available; if the site is commercial-only, residential controls simply don't exist. The interface surfaces only the decisions actually available for that specific site.

The multi-user layer is where Value-Centered Framing becomes directly relevant rather than just a related-work citation. Multiple community members edit the shared 3D model simultaneously. Disagreement is spatially visible — if two people move the same wall in opposite directions, a conflict marker appears. Dow et al.'s finding that surfacing *why* someone wants something (their underlying value — proximity to the park, noise concerns, accessibility) produces more inclusive convergence than raw preference aggregation is a direct, testable design hypothesis here: does value-surfacing before spatial negotiation produce better convergence on a *continuous, constrained* design artifact the same way it did on a discrete list of park amenities? That is an open empirical question their CI'25 paper does not answer, and one this paper is positioned to answer first.

**Layer 2.5 — Voice as the primary input modality.** The 3D scene is the shared ground truth; voice is how most participants will actually act on it. This is where Haijun's malleable/generative UI framework does its second job in this paper: the same argument that a visual interface's affordances should be generated from the live constraint graph, not a fixed menu, applies directly to voice. A fixed voice command grammar ("move wall," "add door") would just relocate the expertise gap into "knowing the magic words." Instead, the set of voice actions available at any moment should be a function of the same constraint graph driving Layer 1 and the same feedback spectrum driving Layer 2 — if the site is commercial-only, "make the ground floor an apartment" isn't a command the system silently ignores or misinterprets, it's a request the system can immediately explain is unavailable and why, in the same actionability spectrum VizCrit describes (here spoken rather than annotated). Ambiguous or underspecified voice input ("move the wall over there") resolves against the same scene graph and the current selection/gaze state Spacetime already tracks for multi-user editing, rather than requiring precise CAD vocabulary. This makes voice-first not a bolt-on accessibility mode but a third rendering surface for the same constraint-reactive interface layer — text/visual, spatial/geometric, and spoken, all generated from one underlying model.

**Layer 3 — Proposal export.** When the group is satisfied, the system exports a submittable proposal package: a plan view at standard architectural scale, a 3D rendered elevation, a constraint compliance certificate, a plain-language summary of design decisions and why they are permissible, and the underlying editable Pascal scene file (with an IFC — Industry Foundation Classes — export path, so it is directly usable by any architect's or contractor's existing BIM software, not a proprietary dead end). This is not an image. It is a design with the same formal properties as a professionally-drawn proposal.

---

## The Technical Foundation

I am building this on top of Pascal (github.com/pascalorg/editor), an open-source 3D architectural editor. As of this writing it has **18,000+ GitHub stars and 2,480 forks**, is MIT-licensed, published as versioned npm packages (`@pascal-app/core`, `@pascal-app/viewer`, `@pascal-app/editor`, `@pascal-app/nodes`), and is under active development (commits as recently as this week) with a Discord community around it. This is real, load-bearing infrastructure, not an abandoned research prototype.

Architecturally, Pascal is a Turborepo monorepo:

- **`@pascal-app/core`** — node schemas, scene state (a Zustand store, `useScene`, persisted to IndexedDB with Zundo-based undo/redo), registry contracts, spatial queries, an event bus.
- **`@pascal-app/viewer`** — 3D rendering via React Three Fiber and WebGPU, camera/controls, post-processing.
- **`@pascal-app/editor`** — editing tools, panels, selection, direct-manipulation UI.
- **`@pascal-app/nodes`** — built-in node definitions, renderers, geometry, and *systems* (the pattern by which node types like walls get their actual geometry computed).
- **`@pascal-app/ifc-converter`** — a standalone, DOM-free package converting IFC (industry-standard BIM format) files into Pascal scene graphs. This is significant: it means Pascal already has a bridge into the format the professional architecture/planning world actually uses, which directly de-risks Layer 3's "real planning standing" requirement.
- **`@pascal-app/mcp`** — a headless Model Context Protocol server that exposes the same scene mutations the editor UI uses (create walls, place items, cut openings, undo) as MCP tools an AI agent can call directly. This is the exact hook needed for the "AI-assisted" half of this proposal's framing — an AI facilitator or design-feedback agent can sit on the *same* scene graph as the human editors without a separate integration layer.

The scene model itself is a flat node dictionary (`Site → Building → Level → {Wall, Slab, Ceiling, Roof, Zone, Item}`), not a nested tree, with a "dirty-node" system so systems only recompute geometry for nodes that actually changed. This matters for the three things I need to add:

1. **A parcel-grounding system** that pulls real zoning constraint data and encodes it as a constraint graph the wall/level systems can query against — a new "system" in Pascal's existing systems pattern, not a fork of the geometry engine.
2. **A constraint-feedback layer** (Layer 2 above), implemented as annotation/overlay renderers subscribing to the same registry pattern Pascal already uses for node rendering.
3. **Multi-user real-time sync**, layered onto the existing `useScene` store and its dirty-node/undo machinery rather than replacing it.

Pascal gives me hard architectural geometry, IFC interoperability, and an AI-agent integration surface for free. That means my engineering time goes to the parts that are scientifically interesting rather than rebuilding a 3D editor from scratch.

---

## What Is Genuinely New Here

I want to be specific about what is a scientific contribution and what is engineering, because CHI rewards both but they are different things.

### Theoretical contribution 1: Constraint feedback as a distinct point on (and extension of) the design-feedback actionability spectrum

VizCrit establishes that computational feedback in a design tool spans a spectrum from static/textbook to awareness-centered to solution-centered, and that where a tool sits on that spectrum changes how designers act on feedback. That work is single-user, single-author, and the constraints are heuristic (visual design principles admit judgment calls). Architectural/zoning constraints are categorically different: they are legally hard, not heuristic, and they are being negotiated by a *group* with different priorities, not applied by one designer to their own work.

This paper argues and tests a distinct claim: that in a hard-constrained, multi-user physical design domain, the actionability spectrum needs a fourth axis beyond individual feedback — *whose* constraint violation is being shown, and to *whom*, changes the social dynamics of a group design session in ways that don't arise when the "user" is a single designer getting feedback on their own mockup. Making constraints visible and reactive should teach the whole group about the actual shape of the decision space, rather than any one person hitting an invisible wall alone. The paper formalizes this extension and tests whether groups experience constraint-reactive feedback differently than they would with a static rulebook or an unconstrained free-form editor.

### Theoretical contribution 2: Voice-first malleable interfaces for constrained physical domains

Haijun's task-driven data model framework argues the right interface for a task is generated from the task's underlying data model, not a fixed menu. That framework has so far been applied to visual/spatial interfaces (Spacetime) and progressive disclosure of visual affordances (Gradual Generation). It has not been applied to voice. Voice interfaces in commercial and research CAD tools today are overwhelmingly command-grammar systems: a fixed, learnable vocabulary that itself becomes a new expertise gap for a first-time user. This paper argues that constraint-reactive malleability, applied to voice, means the space of *sayable, actionable* things changes with the constraint graph and the multi-user editing state in real time — and tests whether this materially lowers the floor for non-expert, non-technical community members to actually author changes to a shared 3D model, versus a fixed-vocabulary voice command set or no voice modality at all.

### Theoretical contribution 3: The artifact gap in civic participation

The participatory planning literature documents extensively that digital tools have not moved communities up Arnstein's ladder despite decades of effort. Existing explanations: lack of access, lack of trust, tokenistic process design. I am proposing a different, previously unarticulated explanation: the missing element is not voice or access. It is the design artifact.

A developer's architect brings a design to a planning commission. The community brings a comment. These are not equivalent inputs. A design can be reviewed, costed, modified, and compared against alternatives. A comment can be acknowledged and set aside. I call this the **artifact gap**, and introduce **artifact-empowered participation**: a mode of community engagement whose output is a design artifact with formal properties, rather than an opinion or a verbal summary. The paper tests whether closing this gap shifts communities up Arnstein's ladder — and, building on Value-Centered Framing, whether *how* the group converges on that artifact (value-surfaced vs. popularity-driven) changes both the artifact's quality and the group's sense that the process was fair.

---

## The Study

Two sequential phases. The formative study is a prerequisite to system design, not an afterthought — the frictions in existing engagement processes must define what I am solving before I build anything. Every design decision in the system should be traceable to a specific finding from the formative study. This is the same methodological commitment BoundarEase made, and a significant part of why that paper is credible.

### Phase 1: Formative study (N = 20–24 community members)

Semi-structured interviews and direct observation with residents of Noida/Greater Noida group housing societies (and, where relevant, unauthorized-colony residents facing regularization decisions) who have gone through builder-redevelopment disputes, RWA (Resident Welfare Association) negotiations with developers or resolution professionals, or Noida Authority stakeholder processes in the past several years. Questions: Where does resident input get lost, ignored, or overridden? What does "winning" look like to a homebuyer or resident in one of these disputes? What prevents residents from producing their own redevelopment proposal rather than reacting to the builder's or authority's? How do residents currently understand the FSI, setback, and building-bylaw constraints on what can actually be built on their site?

Recruitment here runs through my own existing community relationships and local contacts in Delhi–NCR/Noida, not through Mai's or Steven's institutional networks — their San Diego-specific access (CHPD, Design for San Diego) doesn't transfer to this field site. That is a real, honestly-stated difference from how this paper would have been de-risked if it stayed in San Diego: I don't yet have an equivalent of an existing IRB-familiar civic-workshop pipeline like D4SD in Noida, and building that credibly is part of Phase 1 itself, not something to assume away.

### Phase 2: Main study (N = 36, three conditions)

Three groups of community members from the same neighborhood, working on the same real site with the same design brief. Same stakes, different tools.

- **Condition A — Current practice.** Site/constraint presentation followed by a comment session. What actually happens today.
- **Condition B — Image generation (the WeDesign approach).** AI generates images of design proposals from verbal descriptions. People see options; they cannot edit them.
- **Condition C — Our constraint-reactive, voice-first 3D co-design system.** Full multi-user editing via voice and direct manipulation, real-time constraint feedback, proposal export.

**What we measure**, deliberately reusing validated instruments from Dow's own prior work rather than inventing new ones from scratch:

- **Proposal quality and implementability** — outputs rated by professional planners blind to condition. Buildable? Meets code? Reflects real community priorities?
- **Inclusion, value-alignment, and willingness to compromise** — the exact constructs validated in Value-Centered Framing (CI'25), applied here to a continuous spatial artifact instead of a discrete idea list, to test whether the finding generalizes.
- **Position on Arnstein's ladder** — coded from session transcripts and outputs. How much genuine agency did participants exercise? Did they produce something that could enter the planning process as a real alternative?
- **Collective vs. individual reasoning** — did participants consider impacts on neighbors, not just themselves? (BoundarEase showed this was a key benefit of well-designed civic tools.)
- **Constraint comprehension** — do participants understand why certain design choices are or aren't available on their specific site? Do they leave with more knowledge of their own zoning situation?
- **Agency and ownership** — do participants feel their input will matter, and that the process was fair?
- **Voice-first accessibility across literacy and technical-comfort levels** — do participants who would never touch a CAD tool directly author meaningful changes via voice? Does the malleable (constraint-generated) voice interface outperform a fixed command grammar for first-time users specifically?

---

## Why Each Collaborator Is Essential

### A note on this section, up front

With the field site now in Delhi–NCR (Noida) rather than San Diego, Mai's and Steven's roles below are reframed from "provides access to the real community/process" to "shapes the theory and methodology." I think that's still a real and valuable role, not a downgrade to a courtesy credit, but I want to say plainly that this is a proposal, not a settled decision — if either of them would rather stay involved the original way (e.g., a parallel San Diego study, or simply not being on this specific paper), that's worth discussing directly rather than me deciding it in a document.

### Mai Thi Nguyen

Mai's work on racial equity and spatial justice — on who gets to shape the built environment and who gets excluded from those decisions — is the intellectual foundation of what this paper is trying to prove, and that foundation doesn't depend on the field site being San Diego. What changes is the practical role: rather than providing city/community access (which was specific to her San Diego relationships), her contribution becomes shaping the study design so it is genuinely ethical and rigorous for a cross-cultural, non-US field site, and helping interpret findings in a way that connects to the broader planning-equity literature rather than only San Diego/CHPD's specific policy context. Risk 4 below (does a Pascal-exported proposal have real standing with the relevant authority) no longer runs through her existing city contacts — it now depends on relationships I build myself in Noida during Phase 1, which is an honest gap relative to the original framing.

### Steven Dow

Steven's involvement is not a UI-polish role — it is where the paper's second scientific claim actually comes from.

**Intellectually:** ProtoLab's research on feedback exchange, collective creativity, and civic decision-making is the direct foundation for Layer 2 and the multi-user negotiation study. VizCrit's actionability spectrum is the framework Layer 2 extends into a new, harder-constrained, multi-user domain. Value-Centered Framing is the closest empirical precedent for the multi-user convergence question this paper asks, and gives us validated measurement instruments instead of ad hoc ones. His lab's broader research theme — using social technology to organize people around complex civic problems — is exactly the claim this paper is making about built space specifically, and none of that is San Diego-specific.

**What changes:** Design for San Diego was previously framed as a direct recruitment/trust pipeline for Phase 1. With the field site in Noida, that specific pipeline doesn't apply, and I'm not claiming it does. Steven's role becomes advising on how to structure a formative study and a value-centered convergence protocol well, drawing on ProtoLab's methodology, rather than opening doors to a specific community.

**On non-expert usability:** This part doesn't change with geography — ProtoLab's research on how non-expert crowds, students, and community members engage productively with creative/design tasks they have no formal training in is the same operative expertise for Risk 3, wherever participants are recruited from.

**On study infrastructure:** ProtoLab's stated method spans "human-centered design, data science, qualitative methods, and system prototyping" — the mixed-methods rigor a 36-participant, three-condition study needs, regardless of field site.

### Haijun Xia

Haijun's role is the interface architecture — and it is a scientific contribution, not a UI-polish pass. Two of his existing research lines are load-bearing here, not just cited:

**His task-driven, malleable/generative UI framework** is the direct theoretical foundation for both Layer 2 (constraint feedback) and Layer 2.5 (voice). The claim that the right interface is generated from the task's data model rather than a fixed menu has been demonstrated for visual interfaces; this paper is the first application of that argument to voice, and to a domain — legally constrained physical space — where the "menu" isn't just inconvenient when wrong, it's a wall you can't move through.

**His Spacetime research on multi-user collaborative 3D editing** is the direct foundation for the multi-user layer: how simultaneous edits, conflicting intents, and shared awareness are represented when several people manipulate the same 3D model at once. Community design sessions are inherently multi-user and often disagree in real time — this is precisely the interaction problem Spacetime was built to study, just with legal constraints and voice input layered on top.

Practically, Haijun already collaborates with Steven's lab (they are co-authors on VizCrit, CHI'26), which means this is not an artificial three-way pairing — two of the three collaborators already have a working research relationship this project can build on directly.

Delhi–NCR adds a genuinely new wrinkle to Haijun's malleable-voice argument rather than just relocating it: participants will plausibly speak Hindi, English, or a code-switched mix of both in the same session, sometimes mid-sentence. A malleable interface generated from a live constraint graph has to resolve intent across that mix, not just across vocabulary within one language — which is a harder and more novel version of the claim than a monolingual English deployment would have been, and worth stating as a feature of the pivot rather than a complication to hide.

A note on scope: an XR/AR extension (VR walkthroughs, AR on-site viewing) remains a reasonable follow-on paper, given Haijun's and the broader group's interest in spatial computing. It depends on a different kind of infrastructure (headset hardware, on-site fieldwork logistics) than this paper's voice-first, browser-based system, so I am treating it as explicitly out of scope here rather than folding it in prematurely.

---

## Acknowledged Risks

I would rather surface these now than discover them during peer review or after building the wrong thing.

**Risk 1 — The formative study must come first.** The biggest threat is a technology demo with a thin user study. To avoid this, the formative study must precede system development and be substantial enough that specific design decisions are traceable to specific findings. Unlike the San Diego framing of this proposal, I don't yet have an institutional pipeline (like Design for San Diego) in Noida — I have personal community relationships, which is real access but not yet a proven, IRB-familiar workshop process. Part of Phase 1 is building that credibly, not assuming it.

**Risk 2 — Scope must be bounded.** If I try to solve building-bylaw compliance for all of India simultaneously, I build nothing deployable. The right scope is Delhi–NCR, specifically Noida: one authority's building bylaws, real group housing societies, real redevelopment disputes, communities I already have relationships with. Future work generalizes to other jurisdictions (including, potentially, a parallel San Diego study building on CLARO).

**Risk 3 — The tool may reproduce the expertise gap in a new form.** Removing the legal-permissibility expertise gap could introduce a new expertise gap around operating a 3D spatial editor. Voice-first interaction, generated from the constraint graph rather than a fixed command grammar (Haijun's contribution), is the primary mechanism for addressing this, and ProtoLab's experience scaffolding non-expert creative participation is the operative expertise for evaluating whether it actually works. In Noida this risk has a language dimension too: the evaluation must measure whether participants across literacy, technical-comfort, *and* language-mixing (Hindi/English/code-switched) can produce meaningful designs by voice, not just report average-case usability.

**Risk 4 — The proposal export may not have real planning standing.** Whether the Noida Authority (or a court-appointed resolution process, where relevant) would actually accept a Pascal-exported document as a formal stakeholder submission or counter-proposal requires early coordination with whatever local contacts I can establish to determine what format and content is needed. This is a design constraint on the export layer, and — honestly — a bigger open question here than it was under the original San Diego framing, since it no longer runs through an existing collaborator's city relationships.

---

## What You Get from Me

The primary deliverable is a full ACM CHI 2027 paper, from first draft to camera-ready. I will write it, run the HCI-side literature review, and manage submission logistics and reviewer correspondence. Mai and Steven shape the intellectual direction and provide the domain grounding and community/methodological legitimacy. The writing and process management are mine to own.

The second deliverable is the design tool itself, built to a standard I'd describe as the best community spatial design tool published at CHI in 2027 — not a prototype that only runs under controlled conditions, but a system rigorous enough that a real Noida group housing society could use it in a real redevelopment stakeholder process and have it work. The constraint model is grounded in actual Noida Authority building-bylaw data, not mocked. Multi-user editing is stable under real session conditions, in Hindi, English, or a mix of both. The proposal export produces a document the Authority (or a resolution process) would take seriously.

Both are on me to deliver. I am not asking for engineering help, writing support, or production work — I am asking for expertise and mentorship.

---

## What Success Looks Like

**Short term:** the first rigorous study showing that giving communities real, usable 3D design tools — not comment cards, not images, but submittable proposals — changes the quality and equity of their participation in planning processes.

**Medium term:** the tool becomes something Noida/Greater Noida group housing societies, RWAs, and eventually redevelopment authorities or resolution professionals can actually use in real stakeholder sessions with real pending decisions — and, potentially, something that extends back to San Diego through CLARO as a parallel deployment rather than a competing one. This becomes foundational civic-tech infrastructure, not a one-off study artifact.

**Long term:** one piece of a larger argument about the relationship between technology, design, and spatial equity — one that is, if anything, a sharper test case in a Global South, rapid-urbanization context like Delhi–NCR than in a US zoning context, since the redevelopment and displacement stakes for ordinary homebuyers and residents there are often more acute and less protected. The communities most affected by decisions about their own buildings consistently have the least power over those decisions. Some of that is political. Some of it is that the tools of design are gated behind professional expertise. This work attacks the second part.

---

## What I Am Asking For

An hour with each of you, separately or together, to pressure-test this — including the geography and role change itself. Where is the study design weak? Where is the theoretical framing overclaimed? Does it make more sense for this to be a Noida study with you both as advisors, as drafted here, or would a different split of roles (or a parallel San Diego study) serve the science and everyone's interests better? Beyond that: the open question I most want to think through is the exact boundary between what the two theoretical claims require to prove theoretically versus empirically, and how to design a study that speaks simultaneously to the CHI interfaces community, the civic/collective-intelligence community your own recent work sits in, and the urban planning community whose theory we're building on.

Before that meeting I can prepare a detailed technical architecture document, a draft Phase 1 protocol, and a clearer articulation of the constraint-feedback argument with full references to the VizCrit and Value-Centered Framing lines of work.

If this direction seems promising, I will propose starting the formative study this fall — community outreach and IRB preparation over the next couple of months — targeting the ACM CHI 2027 January submission deadline.

Thank you for reading this. I've tried to be honest about what is speculative and what is concrete. The paper proposal is my best current thinking on where the contribution lives. What comes next is the conversation.

---

**HRIDYANSHU**
Currently involved with Mai Thi Nguyen in CHPD, among other work. Maintains active personal and professional relationships in Delhi–NCR, India, which is the basis for this proposal's field site.

- First product, UmeedVR, acquired at 17 during high school by CSRI — a VR tool helping people with autism or neurodivergence practice conversations as part of therapy. Congratulated by Jensen Huang (Founder/CEO, NVIDIA) on the acquisition; one of the earliest people in India to sell an AI product; first person outside Google Research India to use BERT in a commercial not-for-profit setting.
- Presented at Johns Hopkins DREAMS Symposium (Spring 2023); finalist for India's team at Intel International Science and Engineering Fair; public vote winner, Moonshot Pirates 2022.
- Prior solo author, ACM CHI Student Research Competition (global top-12 finalist, presented in Barcelona); ACM SIGGRAPH poster accepted (LA Convention Center) — both grounded in new approaches across computer vision, audio decomposition, and UI/UX for film sound design, recognized by researchers at Adobe and Disney Research.
- 2x Google SWE Fellow.
- Specializes in 3D modeling and rendering; letter from a NASA engineer on 3D design/engineering work from the NASA Lunar Loo Jr. Challenge (Artemis missions).
- Designed a patent-pending futuristic skintight spacesuit, presented at NASA Johnson Space Center under the NASA Conrad Challenge.
- Worked with Randi Zuckerberg and her team at HUG, pioneering new digital-art techniques exhibited at the Tony Awards 2024, Oculus at World Trade Center NY, and the World of Women Annual Gala in Paris.

More at https://www.linkedin.com/in/hridyanshu/
