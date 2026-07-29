# Open Source Landscape: Integrating Pascal Editor for Multiuser, Building Bylaw Compliant Codesign

This document recommends specific, real, currently maintained open source projects to integrate alongside the Pascal editor (github.com/pascalorg/editor, React Three Fiber plus WebGPU, MIT) across the four subsystems the CHI proposal needs: (1) real time multiuser sync, (2) local building rule and parcel data grounding, (3) voice and speech pipeline, (4) IFC/BIM compliance export. Each entry gives concrete integration notes against Pascal's `@pascal-app/core` Zustand store and systems pattern, and flags licensing conflicts against Pascal's MIT baseline.

Section 2 below is written for the current field site, Delhi NCR (Noida), India. Sections 1, 3, and 4 (sync, voice/speech, IFC/BIM) are not geography specific and hold regardless of field site, with the voice/speech section additionally noting a Hindi/English code switching capability check.

---

## 1. Real Time Multiuser Sync

Pascal's scene graph (nodes, transforms, materials, hierarchy) already lives in a Zustand store. The cleanest sync strategy is a CRDT whose nested map, array, and tree types can be observed and piped into `setState`, rather than rearchitecting Pascal around a bespoke network protocol.

### Recommended primary: Yjs
- **URL:** https://github.com/yjs/yjs
- **License:** MIT, fully compatible with Pascal.
- **Integration note:** Model Pascal's scene graph as nested `Y.Map`/`Y.Array` structures (one `Y.Map` per node keyed by node ID, with a `Y.Map` for transform/material props and a `Y.Array` for children order). Use `ydoc.on('update', ...)` and `yMap.observeDeep(...)` to translate remote CRDT deltas into calls to the existing Zustand `setState`/actions in `@pascal-app/core`, so R3F components rerender exactly as they do for local edits, with no separate remote code path. The community `zustand-middleware-yjs` package demonstrates this exact binding pattern and can be forked or adapted rather than used as is (not officially maintained, but functional and small enough to vendor). Yjs's built in awareness protocol gives multi cursor and multi avatar presence (which participant is editing which wall) for free, directly useful for a legitimacy oriented UI that shows who proposed what.
- **Transport companions:**
  - `y-websocket` (https://github.com/yjs/y-websocket, MIT), a simple relay server with auth/cookie support, a good default for a facilitated workshop session with one shared room.
  - `y-webrtc` (https://github.com/yjs/y-webrtc, MIT), serverless peer to peer sync for small community sessions without needing a hosted backend.

### Alternative: Automerge
- **URL:** https://github.com/automerge/automerge
- **License:** MIT.
- **Integration note:** JSON-like CRDT with git-like branching, merging, and full history. Attractive if the paper wants a design alternatives feature (two factions branch the same building, then compare or merge massing options as a first class interaction, echoing the Value-Centered Framing findings on sharing multiple parallel designs). Would need custom glue code into Zustand (no existing middleware equivalent to Yjs's), and historically heavier memory and performance cost on large documents (improved in Automerge 3.x, but still worth benchmarking against a full building model before committing).

### Worth prototyping: Loro
- **URL:** https://github.com/loro-dev/loro
- **License:** MIT (dual MIT/Apache 2.0 in places), compatible.
- **Integration note:** Its native movable tree CRDT type is the best conceptual match for a scene graph where nodes get reparented (moving a window between walls, regrouping rooms). Yjs and Automerge only approximate this with maps and arrays. Smaller ecosystem and shorter track record mean more integration risk, so a spike or prototype rather than committing at proposal stage is the right call.

### Consider with caution: Liveblocks
- **URL:** https://github.com/liveblocks/liveblocks
- **License split, flag this explicitly:** client SDKs are Apache 2.0, but the self hostable sync engine is AGPL 3.0 or later. AGPL is a strong copyleft license. If Pascal's server side sync component links against or is built on Liveblocks' AGPL sync engine and is offered as a network service (which a multiuser codesign tool inherently is), AGPL's network use clause can require making the combined server side source available under AGPL too. This is a genuine licensing conflict with the goal of an MIT licensed academic tool that others can freely relicense or deploy commercially. If used at all, use only the Apache 2.0 client hooks against a self built or Yjs based backend, not the AGPL sync engine, or avoid entirely for a citable, freely redistributable research artifact.

### Alternative architecture: Colyseus
- **URL:** https://github.com/colyseus/colyseus
- **License:** MIT.
- **Integration note:** An authoritative server model (schema/state view diffing) rather than peer CRDT merge. Better fit if the team prefers a single source of truth for enforcing building bylaw compliance centrally (reject or rewrite mutations server side before broadcast) rather than post hoc CRDT merge and conflict UI. Trade off: less offline first than Yjs, and would replace rather than augment the CRDT approach, so pick one path, not both, for the initial system.

**Recommendation:** Yjs plus y-websocket for the paper's core system, with an explicit acknowledgment in the design rationale section that a Loro based tree CRDT migration is future work once reparenting heavy interactions are stress tested.

---

## 2. Local Building Rule and Parcel Data Grounding (Delhi NCR / Noida)

Unlike a US city with a single well maintained open GIS API (San Diego's SanGIS/SANDAG portal, for example), Delhi NCR and Noida do not have one equivalent authoritative, machine readable source. Being upfront about this is itself a useful methodological note for the paper: the constraint model here will realistically be a hybrid of a few partial, sometimes PDF only sources, joined with documentation gathered directly from residents and local contacts during the formative study, rather than a single clean ETL pipeline. That is a genuine difference in feasibility from the original San Diego framing, and worth stating plainly rather than glossing over.

### Bhuvan (ISRO National Remote Sensing Centre geoportal)
- **URL:** https://bhuvan.nrsc.gov.in/
- **License:** Government of India open geoportal, terms vary by layer, generally free for research and non commercial use.
- **Integration note:** Satellite imagery and land use/land cover layers useful for validating plot boundaries and surrounding context (what is actually built around a given society today) when no cleaner parcel source is available. Treat as a base map and cross check layer, not a source of legal building envelope data.

### Bhu-Naksha (cadastral/parcel map portal, Uttar Pradesh instance)
- **URL:** https://upbhunaksha.gov.in/ (the National Informatics Centre also hosts state specific instances for other states at bhunaksha.nic.in)
- **License:** Government of Uttar Pradesh public land record service.
- **Integration note:** The closest analog to a parcel geometry source for the region, showing individual plot boundaries (khasra) from land records. Coverage and digitization quality vary significantly by area and are worth verifying directly for the specific Noida sectors used in the study before relying on it as ground truth, rather than assuming API level reliability the way SanGIS provides for San Diego.

### UP Bhulekh (Uttar Pradesh land records portal)
- **URL:** https://upbhulekh.gov.in/
- **License:** Government of Uttar Pradesh public land record service.
- **Integration note:** Ownership and land record data (khasra/khatauni) that can help confirm plot identity and status, complementary to Bhu-Naksha's geometry rather than a substitute for the actual Noida Authority building bylaws that govern what can be built.

### UP RERA (Uttar Pradesh Real Estate Regulatory Authority) project portal
- **URL:** https://up-rera.in/
- **License:** Government of Uttar Pradesh regulatory portal, public filings.
- **Integration note:** For group housing societies caught in builder redevelopment disputes specifically, this is a directly relevant source: registered project filings, promoter details, and project status are public here, and are likely to matter more to the formative study's recruitment and case selection than a general zoning API would.

### Open Government Data (OGD) Platform India
- **URL:** https://data.gov.in/
- **License:** Government of India open data platform, most datasets under a permissive Indian government open data license.
- **Integration note:** Worth checking per formative study site for any published municipal or state planning datasets, though coverage for Noida specifically may be thin. Cite as the standard place a reviewer familiar with Indian open data would expect this to be checked, even where a specific useful dataset is not found.

### OpenStreetMap building footprints
- **URL:** https://www.openstreetmap.org/ (India coverage, community mapped)
- **License:** Open Data Commons Open Database License (ODbL).
- **Integration note:** A practical fallback for building footprint geometry where official sources are incomplete, since OSM's building coverage in the National Capital Region is comparatively good due to active local mapping communities. Useful as a starting geometry layer for surrounding context buildings even if the subject plot itself needs to be surveyed or hand modeled from a real sanctioned plan.

### Open Buildings style AI derived footprint datasets
- **URL:** e.g. Google's Open Buildings dataset, and Microsoft's Building Footprints, both with India coverage
- **License:** CC-BY style open licenses (verify per dataset before use).
- **Integration note:** A useful cross check for building footprint completeness against OSM and Bhu-Naksha, though these are AI derived from satellite imagery and not authoritative for legal plot boundaries or building bylaw compliance.

### Noida Authority Master Plan and building bylaws
- **URL:** published as PDF documents by the Noida Authority (New Okhla Industrial Development Authority) rather than a structured open API.
- **License:** Public government document.
- **Integration note:** This is the actual source of the legal constraint numbers the tool needs (FAR/FSI, setbacks, ground coverage, height limits), but it means the `zoningSystem` equivalent here has to be seeded from manually encoded bylaw values rather than an automatic feed, at least initially. Encoding this once as a structured rule set, the same spirit as the National Zoning Atlas project does for US municipal codes, is worthwhile future work in its own right, and something worth flagging to reviewers as a contribution rather than a shortcut taken.

---

## 3. Voice and Speech Pipeline

### STT core: whisper.cpp
- **URL:** https://github.com/ggml-org/whisper.cpp
- **License:** MIT.
- **Integration note:** A dependency free C/C++ Whisper port with Metal, CUDA, Vulkan, and CPU backends, and a streaming mode with roughly half a second to two second latency using tiny or base models. Runs fully on device, which matters both for workshop venues with unreliable connectivity and for community privacy concerns (residents' voices should not have to leave the room). Run as a local process or service, with a thin WebSocket bridge feeding partial transcripts to a new `voiceIntentSystem` that sits alongside Pascal's existing systems and dispatches resolved intents as the same store actions used by direct manipulation, so voice and mouse/touch edits go through one code path. Whisper's multilingual training also makes it a reasonable starting point for the Hindi/English code switching case this field site needs, though real accuracy on code switched speech should be validated directly rather than assumed.

### Server side alternative: faster-whisper
- **URL:** https://github.com/SYSTRAN/faster-whisper
- **License:** MIT.
- **Integration note:** A CTranslate2 reimplementation, roughly four times faster with lower memory use. Use if the tool runs on a shared workshop laptop or GPU rather than in browser, wrapped as a small STT microservice.

### Streaming and incremental transcripts: whisper_streaming (ufal)
- **URL:** https://github.com/ufal/whisper_streaming
- **License:** MIT.
- **Integration note:** Implements local agreement policies for genuinely incremental, not just final, transcription, letting the UI show provisional highlighting of the referenced 3D object before the utterance finishes, directly supporting disambiguation of spatial references while someone is still speaking.

### Offline and low resource fallback: Vosk
- **URL:** https://github.com/alphacep/vosk-api
- **License:** Apache 2.0.
- **Integration note:** Fully offline, as small as 50 megabytes, with a streaming API and models in twenty plus languages including Hindi, appropriate for low bandwidth or low compute community venues. Compatible license, no conflict.

### Voice agent orchestration: Pipecat
- **URL:** https://github.com/pipecat-ai/pipecat
- **License:** BSD 2-Clause.
- **Integration note:** A ready made pipeline skeleton (voice activity detection, turn taking, interruption handling) orchestrating speech to text, then an LLM, then text to speech. Its function and tool calling hooks are the natural attachment point for MCP style tools that mutate the Pascal scene graph, wiring each tool call to dispatch a Zustand action the same way CRDT remote updates are applied, so all mutation paths (local UI, remote CRDT, voice/LLM) converge on the same store API.

### Multi participant alternative: LiveKit Agents
- **URL:** https://github.com/livekit/agents
- **License:** Apache 2.0.
- **Integration note:** A WebRTC native multi participant room model, a better fit than single user voice frameworks for several community members speaking or gesturing concurrently around the same shared scene. Exposes function calls the LLM can invoke as MCP style tools, and would sit alongside or replace Pipecat depending on how many simultaneous speaking channels the study design needs.

### Modular local only reference architecture: Rhasspy3
- **URL:** https://github.com/rhasspy/rhasspy3
- **License:** MIT.
- **Integration note:** Independent offline services (wake word, STT, intent, TTS) coordinated via the lightweight Wyoming protocol, a privacy preserving reference architecture worth citing in the paper's ethics and data handling discussion even if not fully adopted, since it demonstrates that a fully local voice assistant stack is achievable without cloud dependency.

### NLU baseline (maintenance mode): Rasa Open Source
- **URL:** https://github.com/RasaHQ/rasa
- **License:** Apache 2.0.
- **Integration note:** Entity extraction and slot filling directly applicable to resolving ambiguous spatial references ("move that wall back two feet") into structured intents. RasaHQ has shifted primary development elsewhere, so cite as a proven pattern baseline rather than adopt as the production NLU layer.

### Tool calling standard: Model Context Protocol (MCP)
- **URL:** https://github.com/modelcontextprotocol
- **License:** MIT.
- **Integration note:** The concrete standard underlying the voice agent's LLM calling tools that mutate the scene graph. Expose Pascal's scene graph operations (add wall, resize room, check setback, query envelope) as a typed MCP server. The voice/LLM pipeline calls these tools after resolving an utterance, and each tool implementation is a thin wrapper around existing `@pascal-app/core` Zustand actions, giving a standards based integration rather than a bespoke voice to geometry mapping. Pascal already ships its own `@pascal-app/mcp` package doing exactly this for the core editor, which this voice layer builds directly on.

### Lightest weight browser only prototype path: Web Speech API plus annyang
- **URL:** https://developer.mozilla.org/en-US/docs/Web/API/Web_Speech_API (polyfill/wrapper: `annyang`, MIT)
- **License:** Browser native (varies by vendor), wrapper libraries typically MIT.
- **Integration note:** Since Pascal is already an R3F web app, this is the lowest friction way to get voice input running in browser with zero server round trip, useful for an early demo or pilot version before investing in the full Whisper/Pipecat backend described above.

---

## 4. IFC and BIM Compliance Export

### Core IFC engine: IfcOpenShell (plus Bonsai)
- **URL:** https://github.com/IfcOpenShell/IfcOpenShell
- **License:** LGPL/GPL for the Bonsai add on, flag this. LGPL is weak copyleft. Dynamic linking, or calling IfcOpenShell as a separate process or library via its Python/CLI interface rather than statically linking its code into Pascal's own MIT licensed source, is the standard, safe integration pattern and does not obligate Pascal's own code to relicense. Treat IfcOpenShell as an external service or CLI dependency (invoked via `IfcConvert` or a Python microservice), not compiled into the core client bundle, to keep Pascal's MIT core clean. The Bonsai Blender add on is GPL and should be treated purely as an optional external authoring tool for planners or reviewers, never embedded in the web client.
- **Integration note:** Use as the upstream complement to Pascal's own `@pascal-app/ifc-converter`, rather than reimplementing full IFC4/IFC4x3 schema handling, shelling out to IfcOpenShell (server side microservice) for spec complete export and import, producing a standards compliant BIM file architects, planning staff, and permit reviewers can open in Revit, ArchiCAD, or Solibri.

### In browser IFC: web-ifc / engine_web-ifc (That Open Company)
- **URL:** https://github.com/ThatOpen/engine_web-ifc
- **License:** MIT, no conflict.
- **Integration note:** WASM compiled, native speed in browser IFC read and write. Since Pascal is R3F based, this is a drop in client side layer: add an `ifcExportSystem` that serializes the current Zustand scene graph state into web-ifc's in memory model and emits an IFC file directly in the browser, no server round trip, keeping the live voice first collaborative session fast while still producing a genuine IFC deliverable at any checkpoint.

### Compliance rule format: buildingSMART IDS
- **URL:** https://github.com/buildingSMART/IDS
- **License:** Open standard, openly maintained, no conflict.
- **Integration note:** Author each jurisdiction's building rule checklist once as an `.ids` rule file (setbacks present, unit counts tagged, egress widths specified) rather than ad hoc validation code. This is the standards based analog to a structured Noida building bylaw rule set, applied at the BIM/IFC layer instead of the parcel/GIS layer.

### IDS validation engine: IfcOpenShell IfcTester
- **URL:** https://github.com/IfcOpenShell/IfcOpenShell (IfcTester module)
- **License:** LGPL, same flag as above, run as an external process, not linked in tree.
- **Integration note:** Runs `.ids` rule files against exported IFC models and reports pass or fail per requirement. Pair directly with the IDS files above: the proposal export pipeline (server side, invoking IfcOpenShell/IfcTester as an external tool) produces a pass or fail compliance report attached to the community's exported proposal, the concrete mechanism for the paper's "legitimate alternative to a developer's proposal" claim, since it is the same class of automated check an authority or resolution process could recognize.

### UI reference: Model Checker (opensource-construction)
- **URL:** https://github.com/opensource-construction/model-checker
- **License:** Open source (verify the specific license file before use, not explicitly confirmed as MIT or otherwise permissive).
- **Integration note:** A reference design, not necessarily reused code, for surfacing IDS/compliance results to non expert users in an approachable web UI, useful as a design pattern for the plain language compliance report shown alongside the community's 3D model, echoing the VizCrit finding that solution centered, not just awareness centered, feedback improves non expert outcomes.

### Precedent for automated permit check geometry logic: GeoBIM Building Permit Tool (TU Delft)
- **URL:** https://github.com/tudelft3d/GeoBIM-building-permit-tool
- **License:** Open source (TU Delft 3D geoinformation group, verify specific terms in the repository).
- **Integration note:** A concrete, citable precedent for automated IFC driven planning code checks (max height, storey footprint overlap, overhangs) already demonstrated in the field (a Rotterdam GeoBIM pilot), strengthening the paper's claim that geometry based compliance checking is a proven, not speculative, capability. Can inform the specific geometric check algorithms implemented in Pascal's own constraint and export pipeline rather than being integrated wholesale.

### .NET alternative: xbim Toolkit
- **URL:** https://github.com/xBimTeam/XbimEssentials
- **License:** CDDL, flag this. CDDL is a weak copyleft, file level license (similar spirit to LGPL but Mozilla lineage), safe to use as a separate service or process (for example a .NET back office review portal) but should not be statically combined into the MIT licensed core client. Relevant only if a planning department facing back office component ends up being .NET based, and offers BCF (BIM Collaboration Format) issue tracking for structured planner feedback on submitted proposals.

---

## Summary Table

| Subsystem | Primary pick | License | Conflict flag |
|---|---|---|---|
| Multiuser sync | Yjs plus y-websocket | MIT | None |
| Multiuser sync (tree native, prototype) | Loro | MIT/Apache 2.0 | None |
| Multiuser sync (avoid or limit) | Liveblocks sync engine | AGPL 3.0 (server) | Yes, AGPL network use clause |
| Local parcel/rule data | Bhuvan, Bhu-Naksha, UP Bhulekh, UP RERA, OSM, Noida Authority Master Plan (PDF) | Mostly public government or open data, one PDF only source | None, but coverage/reliability caveats noted above |
| Building rule schema pattern | buildingSMART IDS, National Zoning Atlas as a US precedent for the encoding approach | Open standard | None |
| Voice STT | whisper.cpp / faster-whisper / Vosk | MIT / MIT / Apache 2.0 | None |
| Voice orchestration | Pipecat / LiveKit Agents | BSD 2-Clause / Apache 2.0 | None |
| Voice tool calling standard | Model Context Protocol | MIT | None |
| IFC export (browser) | web-ifc | MIT | None |
| IFC export (server, spec complete) | IfcOpenShell / IfcTester | LGPL/GPL | Yes, use as external process only, do not statically link into MIT core |
| Compliance rules | buildingSMART IDS | Open standard | None |
| .NET back office (optional) | xbim Toolkit | CDDL | Yes, same external process caveat as LGPL |

**Overall licensing posture:** Pascal's MIT core stays clean as long as (a) the AGPL Liveblocks sync engine is avoided or replaced with the MIT licensed Yjs stack, and (b) IfcOpenShell/IfcTester (LGPL/GPL) and xbim (CDDL) are integrated exclusively as out of process services or CLI tools invoked over IPC/HTTP rather than compiled or statically linked into the client or core package, the standard, well established pattern for combining permissively licensed applications with copyleft licensed libraries.
