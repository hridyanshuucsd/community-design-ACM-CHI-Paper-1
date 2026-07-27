RESEARCH PROPOSAL

# From Consultation to Co-Authorship
### A Collaborative 3D Design Tool for Community Spatial Agency in Urban Planning

**Hridyanshu**
UC San Diego, Mathematics–Computer Science B.S.
**Proposed for: ACM CHI 2028**
**Proposed mentors/collaborators: Mai Thi Nguyen · Steven Dow**

*Revision note: this version replaces the earlier draft's Haijun Xia / Nadir Weibel framing with Steven Dow, and retargets the submission cycle from CHI 2027 (Sept 2026 deadline) to CHI 2028. The original deadline is roughly six weeks from today, which is not compatible with this proposal's own methodological commitment that the formative study must precede system design (Risk 1). CHI 2028 gives a real year: formative study and IRB this fall/winter, system build through spring, main study in late spring/summer, submission next September.*

---

## Why I Am Writing This

I have spent the last several months building CLARO, a 3D spatial intelligence platform for San Diego that combines photorealistic Google Maps tiles with dynamic, user-defined UI and AI interaction. In the process, I kept running into the same problem: the people most affected by decisions about built space — renters, long-time neighborhood residents, communities that have been displaced or are fighting displacement — have almost no real tools to participate in those decisions.

They can show up to the town hall. They can fill out a comment card. They can speak for two minutes into a microphone. Then the professionals go back and do whatever they were going to do anyway.

This proposal is about fixing that. Specifically, I want to build a novel multi-user, AI-assisted 3D design tool that lets a community group design built space together — not comment on someone else's design, but build their own, with all the zoning rules built in automatically, producing a real architectural proposal they can put on the table as a legitimate alternative to what a developer is offering.

I am writing to you, Mai, because this is the computational form of the problem your research is about. I am writing to you, Steven, because Design for San Diego already proved that San Diegans will show up to co-design solutions to their own housing crisis when the process is built right — and because your lab's work on feedback exchange and value-centered convergence in civic design is the closest existing empirical foundation for what I want this tool to do. Between the two of you, this project has both the community legitimacy and the methodological rigor it needs to be more than a demo.

I am also self-funding the engineering and the entire R&D. I have my own funds and donors for everything I do. I want to be upfront about that. I am not asking for lab resources or research budgets. I am asking for your guidance, your expertise, and your access to the communities and city processes that would make this real.

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

Imagine a community group of ten residents sitting together around a screen. Their city is considering a rezoning that would allow a twelve-story development on a vacant lot in their neighborhood. The developer's architect has produced a design. The community has been invited to comment.

Instead of comment cards, they open our tool. It shows them the lot in 3D, at real scale, with the existing buildings around it. It shows them the building envelope — the maximum volume the zoning allows. A wall cannot be dragged outside the envelope. If they try to build taller than the height limit, the wall resists. The rules are not in a document they have to read. They are in the space itself.

Now they start designing. One person suggests putting parking underground instead of on the ground floor. Another moves the entrance to face the park. They argue about whether the ground floor should be commercial or community space. They can see their disagreements spatially, in 3D, in real time — and when the tool surfaces *why* a move is or isn't allowed, it does so the way Steven's VizCrit work frames computational design feedback: not just a hard stop, but feedback with a spectrum of actionability, from "here's the rule" to "here's what would work instead."

At the end of the session, they export a floor plan, a 3D render, a compliance summary showing the design meets all zoning requirements, and a full editable file an architect could pick up and continue from. They have produced a legitimate design proposal that can sit across the table from the developer's proposal as an equal alternative.

### The three technical layers

**Layer 1 — The constraint model.** Given a real parcel address, the system builds a spatial constraint graph: lot boundary, existing structures, zoning setbacks, the allowable building envelope (height limit, floor-area ratio, lot coverage), required clearances, allowable uses, and special overlay districts (historic, coastal, fire). These become the physics of the editor: the constraints are not text, they are encoded in what the tool will and will not let you do. The constraint graph is a live, queryable structure the interface subscribes to as a first-class input, not a validation pass run after the fact — affordances are generated from rules *before* you design, not checked *after*.

I already have a head start here: CLARO and my ADU permit analysis work (matching HCD permit records against real property geometry) give me a dataset of real parcel data and real permit outcomes to ground-truth the constraint model against — what actually gets approved is not always what the zoning code technically says.

**Layer 2 — Constraint feedback as a first-class interaction, not a validation layer.** This is where Steven's research is directly load-bearing. VizCrit (CHI'26) shows that computational feedback in a design tool is not a single thing — it spans an actionability spectrum from static, textbook-style flags to awareness-centered annotations to solution-centered suggestions that show the fix. VizCrit built this for 2D visual design heuristics (contrast, alignment, hierarchy) for a single designer. I am extending that framework into a domain it has never been applied to: 3D architectural constraints that are legally hard (not heuristic), spatial rather than visual, and shared in real time by a *group* rather than a single author.

Concretely: if a wall exceeds the height envelope, the system does not just refuse the move — it can show the envelope boundary (awareness), or suggest a compliant alternative massing (solution-centered), the same way VizCrit's annotations scale from "there's an issue" to "here's the fix." If the zoning allows an upper-floor addition, that control becomes available; if the site is commercial-only, residential controls simply don't exist. The interface surfaces only the decisions actually available for that specific site.

The multi-user layer is where Value-Centered Framing becomes directly relevant rather than just a related-work citation. Multiple community members edit the shared 3D model simultaneously. Disagreement is spatially visible — if two people move the same wall in opposite directions, a conflict marker appears. Dow et al.'s finding that surfacing *why* someone wants something (their underlying value — proximity to the park, noise concerns, accessibility) produces more inclusive convergence than raw preference aggregation is a direct, testable design hypothesis here: does value-surfacing before spatial negotiation produce better convergence on a *continuous, constrained* design artifact the same way it did on a discrete list of park amenities? That is an open empirical question their CI'25 paper does not answer, and one this paper is positioned to answer first.

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

### Theoretical contribution 2: The artifact gap in civic participation

The participatory planning literature documents extensively that digital tools have not moved communities up Arnstein's ladder despite decades of effort. Existing explanations: lack of access, lack of trust, tokenistic process design. I am proposing a different, previously unarticulated explanation: the missing element is not voice or access. It is the design artifact.

A developer's architect brings a design to a planning commission. The community brings a comment. These are not equivalent inputs. A design can be reviewed, costed, modified, and compared against alternatives. A comment can be acknowledged and set aside. I call this the **artifact gap**, and introduce **artifact-empowered participation**: a mode of community engagement whose output is a design artifact with formal properties, rather than an opinion or a verbal summary. The paper tests whether closing this gap shifts communities up Arnstein's ladder — and, building on Value-Centered Framing, whether *how* the group converges on that artifact (value-surfaced vs. popularity-driven) changes both the artifact's quality and the group's sense that the process was fair.

---

## The Study

Two sequential phases. The formative study is a prerequisite to system design, not an afterthought — the frictions in existing engagement processes must define what I am solving before I build anything. Every design decision in the system should be traceable to a specific finding from the formative study. This is the same methodological commitment BoundarEase made, and a significant part of why that paper is credible.

### Phase 1: Formative study (N = 20–24 community members)

Semi-structured interviews and direct observation with community members who have participated in San Diego urban design engagement — developer outreach meetings, design review board processes, planning commission hearings, or community plan update sessions in the past three years. Questions: Where does community input get lost, ignored, or overridden? What does "winning" look like to a community member in a design process? What prevents communities from producing their own proposals rather than reacting to someone else's? How do community members currently understand the spatial and regulatory constraints on what is buildable in their neighborhood?

This is where Mai's networks and the Center for Housing Policy and Design are indispensable — without access to communities inside real planning decisions, this produces anecdotes rather than grounded findings. It is also where **Design for San Diego is a direct asset rather than a hypothetical one**: D4SD has already run public workshops specifically on San Diego housing insecurity, bringing together residents, students, and professionals around exactly the kind of built-environment civic problem this paper addresses. That is an existing, IRB-familiar community pipeline this project can build on rather than construct from zero — it substantially de-risks Phase 1 timing.

### Phase 2: Main study (N = 36, three conditions)

Three groups of community members from the same neighborhood, working on the same real site with the same design brief. Same stakes, different tools.

- **Condition A — Current practice.** Site/constraint presentation followed by a comment session. What actually happens today.
- **Condition B — Image generation (the WeDesign approach).** AI generates images of design proposals from verbal descriptions. People see options; they cannot edit them.
- **Condition C — Our constraint-reactive 3D co-design system.** Full multi-user editing, real-time constraint feedback, proposal export.

**What we measure**, deliberately reusing validated instruments from Dow's own prior work rather than inventing new ones from scratch:

- **Proposal quality and implementability** — outputs rated by professional planners blind to condition. Buildable? Meets code? Reflects real community priorities?
- **Inclusion, value-alignment, and willingness to compromise** — the exact constructs validated in Value-Centered Framing (CI'25), applied here to a continuous spatial artifact instead of a discrete idea list, to test whether the finding generalizes.
- **Position on Arnstein's ladder** — coded from session transcripts and outputs. How much genuine agency did participants exercise? Did they produce something that could enter the planning process as a real alternative?
- **Collective vs. individual reasoning** — did participants consider impacts on neighbors, not just themselves? (BoundarEase showed this was a key benefit of well-designed civic tools.)
- **Constraint comprehension** — do participants understand why certain design choices are or aren't available on their specific site? Do they leave with more knowledge of their own zoning situation?
- **Agency and ownership** — do participants feel their input will matter, and that the process was fair?

---

## Why Each Collaborator Is Essential

### Mai Thi Nguyen

This paper's legitimacy depends on working with communities inside real planning decisions, not simulated ones. Without Mai's networks, her Center for Housing Policy and Design, her relationships with San Diego city planning, and her decades of community engagement work in the planning literature, this is a demo with a fake study. With her, it is a study about real communities facing real decisions, with findings that speak to policy.

Mai's work on racial equity and spatial justice — on who gets to shape the built environment and who gets excluded from those decisions — is the intellectual foundation of what we are trying to prove. She does not need to know anything about Pascal or multi-user 3D editors. She needs to help find the right communities, design a study that is genuinely ethical and rigorous, and interpret findings in a way that connects to planning scholarship and policy. Her city contacts are also essential to Risk 4 below: we need to know early what format a community proposal needs to be taken seriously in a real San Diego planning process.

### Steven Dow

Steven's involvement is not a UI-polish role — it is where the paper's second scientific claim actually comes from, and it is where the community pipeline for Phase 1 already partly exists.

**Intellectually:** ProtoLab's research on feedback exchange, collective creativity, and civic decision-making is the direct foundation for Layer 2 and the multi-user negotiation study. VizCrit's actionability spectrum is the framework Layer 2 extends into a new, harder-constrained, multi-user domain. Value-Centered Framing is the closest empirical precedent for the multi-user convergence question this paper asks, and gives us validated measurement instruments instead of ad hoc ones. His lab's broader research theme — using social technology to organize people around complex civic problems — is exactly the claim this paper is making about built space specifically.

**Practically:** Steven co-founded Design for San Diego, which has already run public civic-design workshops on San Diego housing insecurity with real residents. That is existing community trust and process infrastructure this project can plug into for Phase 1 recruitment, rather than something we have to build from a cold start — a direct answer to Risk 1 (the formative study must be real, not thin).

**On non-expert usability:** Rather than treating "can non-experts use a spatial editor" as a separate accessibility problem needing a different lab's expertise, it is the same question ProtoLab already studies — how non-expert crowds, students, and community members engage productively with creative/design tasks they have no formal training in. That is a better methodological fit for Risk 3 than treating it as a bolt-on XR/accessibility concern.

**On study infrastructure:** ProtoLab's stated method already spans "human-centered design, data science, qualitative methods, and system prototyping" — the mixed-methods rigor a 36-participant, three-condition study needs.

A note on scope: the earlier draft of this proposal built in an XR/AR extension (VR walkthroughs, AR on-site viewing) as a natural follow-on paper. That is still a reasonable second project, but it depends on a different kind of infrastructure (headset hardware, on-site fieldwork logistics) than what Mai and Steven's current collaboration covers, so I am treating it as explicitly out of scope for this paper rather than implying a third collaborator who isn't actually attached to this proposal yet.

---

## Acknowledged Risks

I would rather surface these now than discover them during peer review or after building the wrong thing.

**Risk 1 — The formative study must come first.** The biggest threat is a technology demo with a thin user study. To avoid this, the formative study must precede system development and be substantial enough that specific design decisions are traceable to specific findings. Design for San Diego's existing community relationships materially reduce this risk versus starting from zero.

**Risk 2 — Scope must be bounded.** If I try to solve zoning compliance for all cities simultaneously, I build nothing deployable. The right scope is San Diego specifically: one zoning code, real parcels, real pending decisions, real communities Mai and D4SD already have relationships with. Future work generalizes to other jurisdictions.

**Risk 3 — The tool may reproduce the expertise gap in a new form.** Removing the legal-permissibility expertise gap could introduce a new expertise gap around operating a 3D spatial editor. The evaluation must explicitly measure whether participants across literacy/technical-comfort levels can produce meaningful designs, not just report average-case usability — this is where ProtoLab's experience scaffolding non-expert creative participation (crowds, students, community members) is the operative expertise, not a separate accessibility specialization.

**Risk 4 — The proposal export may not have real planning standing.** Whether San Diego planning authorities will actually accept a Pascal-exported document (or its IFC export) as a formal public comment or counter-proposal requires early coordination with Mai's city contacts to establish what format and content is needed. This is a design constraint on the export layer, not something to figure out after the system is built.

---

## What You Get from Me

The primary deliverable is a full ACM CHI 2028 paper, from first draft to camera-ready. I will write it, run the HCI-side literature review, and manage submission logistics and reviewer correspondence. Mai and Steven shape the intellectual direction and provide the domain grounding and community/methodological legitimacy. The writing and process management are mine to own.

The second deliverable is the design tool itself, built to a standard I'd describe as the best community spatial design tool published at CHI in 2028 — not a prototype that only runs under controlled conditions, but a system rigorous enough that CHPD (and D4SD) can hand it to a real community group in a real planning session and have it work. The constraint model is grounded in actual San Diego zoning data, not mocked. Multi-user editing is stable under real session conditions. The proposal export produces a document a planning commissioner would take seriously.

Both are on me to deliver. I am not asking for engineering help, writing support, or production work — I am asking for expertise and mentorship.

---

## What Success Looks Like

**Short term:** the first rigorous study showing that giving communities real, usable 3D design tools — not comment cards, not images, but submittable proposals — changes the quality and equity of their participation in planning processes.

**Medium term:** the tool becomes something CHPD, D4SD, and their city partners can actually use in real community engagement sessions with real pending decisions in San Diego, and potentially beyond. Alongside CLARO, this becomes foundational civic-tech infrastructure, not a one-off study artifact.

**Long term:** one piece of a larger argument about the relationship between technology, design, and spatial equity. The communities most affected by planning decisions — low-income communities, communities of color, immigrant communities — consistently have the least power over those decisions. Some of that is political. Some of it is that the tools of design are gated behind professional expertise. This work attacks the second part.

---

## What I Am Asking For

An hour with each of you, separately or together, to pressure-test this. Where is the study design weak? Where is the theoretical framing overclaimed? Which communities and city contacts (and, Steven, which parts of D4SD's existing pipeline) are the right starting point? The open question I most want to think through is the exact boundary between what the two theoretical claims require to prove theoretically versus empirically, and how to design a study that speaks simultaneously to the CHI interfaces community, the civic/collective-intelligence community your own recent work sits in, and the urban planning community whose theory we're building on.

Before that meeting I can prepare a detailed technical architecture document, a draft Phase 1 protocol, and a clearer articulation of the constraint-feedback argument with full references to the VizCrit and Value-Centered Framing lines of work.

If this direction seems promising, I will propose starting the formative study this fall — community outreach and IRB preparation over the next couple of months — targeting the ACM CHI 2028 submission cycle.

Thank you for reading this. I've tried to be honest about what is speculative and what is concrete. The paper proposal is my best current thinking on where the contribution lives. What comes next is the conversation.

---

**HRIDYANSHU**
Currently involved with Mai Thi Nguyen in CHPD, among other work.

- First product, UmeedVR, acquired at 17 during high school by CSRI — a VR tool helping people with autism or neurodivergence practice conversations as part of therapy. Congratulated by Jensen Huang (Founder/CEO, NVIDIA) on the acquisition; one of the earliest people in India to sell an AI product; first person outside Google Research India to use BERT in a commercial not-for-profit setting.
- Presented at Johns Hopkins DREAMS Symposium (Spring 2023); finalist for India's team at Intel International Science and Engineering Fair; public vote winner, Moonshot Pirates 2022.
- Prior solo author, ACM CHI Student Research Competition (global top-12 finalist, presented in Barcelona); ACM SIGGRAPH poster accepted (LA Convention Center) — both grounded in new approaches across computer vision, audio decomposition, and UI/UX for film sound design, recognized by researchers at Adobe and Disney Research.
- 2x Google SWE Fellow.
- Specializes in 3D modeling and rendering; letter from a NASA engineer on 3D design/engineering work from the NASA Lunar Loo Jr. Challenge (Artemis missions).
- Designed a patent-pending futuristic skintight spacesuit, presented at NASA Johnson Space Center under the NASA Conrad Challenge.
- Worked with Randi Zuckerberg and her team at HUG, pioneering new digital-art techniques exhibited at the Tony Awards 2024, Oculus at World Trade Center NY, and the World of Women Annual Gala in Paris.

More at https://www.linkedin.com/in/hridyanshu/
