# Open Source Landscape: Integrating Pascal Editor for Multi-User Zoning-Compliant Co-Design

This document recommends specific, real, currently-maintained open-source projects to integrate alongside the Pascal editor (github.com/pascalorg/editor, React Three Fiber + WebGPU, MIT) across the four subsystems the CHI proposal needs: (1) real-time multi-user sync, (2) zoning/parcel data grounding, (3) voice/speech pipeline, (4) IFC/BIM compliance export. Each entry gives concrete integration notes against Pascal's `@pascal-app/core` Zustand store and systems pattern, and flags licensing conflicts against Pascal's MIT baseline.

---

## 1. Real-Time Multi-User Sync

Pascal's scene graph (nodes, transforms, materials, hierarchy) already lives in a Zustand store. The cleanest sync strategy is a CRDT whose nested map/array/tree types can be observed and piped into `setState`, rather than rearchitecting Pascal around a bespoke network protocol.

### Recommended primary: Yjs
- **URL:** https://github.com/yjs/yjs
- **License:** MIT — fully compatible with Pascal.
- **Integration note:** Model Pascal's scene graph as nested `Y.Map`/`Y.Array` structures (one `Y.Map` per node keyed by node ID, with a `Y.Map` for transform/material props and a `Y.Array` for children order). Use `ydoc.on('update', ...)` / `yMap.observeDeep(...)` to translate remote CRDT deltas into calls to the existing Zustand `setState`/actions in `@pascal-app/core`, so R3F components re-render exactly as they do for local edits — no separate "remote" code path. The community `zustand-middleware-yjs` package demonstrates this exact binding pattern and can be forked/adapted rather than used as-is (not officially maintained, but functional and small enough to vendor). Yjs's built-in **awareness** protocol gives multi-cursor / multi-avatar presence (which participant is editing which wall) for free — directly useful for a legitimacy-oriented UI that shows who proposed what.
- **Transport companions:**
  - `y-websocket` (https://github.com/yjs/y-websocket, MIT) — simple relay server with auth/cookie support; good default for a facilitated workshop session with one shared room.
  - `y-webrtc` (https://github.com/yjs/y-webrtc, MIT) — serverless peer-to-peer sync for small community sessions without needing a hosted backend.

### Alternative: Automerge
- **URL:** https://github.com/automerge/automerge
- **License:** MIT.
- **Integration note:** JSON-like CRDT with git-like branching/merge and full history — attractive if the paper wants a "design alternatives" feature (two community factions branch the same building, then compare/merge massing options as a first-class interaction, echoing the Value-Centered Framing and Prototyping-Dynamics findings on sharing multiple parallel designs). Would need custom glue code into Zustand (no existing middleware equivalent to Yjs's), and historically heavier memory/perf on large docs (improved in Automerge 3.x, but still worth benchmarking against a full building model before committing).

### Worth prototyping: Loro
- **URL:** https://github.com/loro-dev/loro
- **License:** MIT (dual MIT/Apache-2.0 in places) — compatible.
- **Integration note:** Native **Movable-Tree CRDT** type is the best conceptual match for a scene graph where nodes get reparented (moving a window between walls, regrouping rooms) — Yjs/Automerge only approximate this with Maps/Arrays. Smaller ecosystem and shorter track record = more integration risk; recommend a spike/prototype rather than committing at proposal stage.

### Consider with caution: Liveblocks
- **URL:** https://github.com/liveblocks/liveblocks
- **License split — flag this explicitly:** client SDKs are Apache-2.0, but the self-hostable **sync engine is AGPL-3.0-or-later**. AGPL is a strong copyleft license: if Pascal's server-side sync component links against or is built on Liveblocks' AGPL sync engine and is offered as a network service (which a multi-user co-design tool inherently is), AGPL's network-use clause can require making the combined server-side source available under AGPL too. This is a genuine licensing conflict with the goal of an MIT-licensed academic tool that others can freely relicense/deploy commercially. If used at all, use only the Apache-2.0 client hooks against a self-built or Yjs-based backend, not the AGPL sync engine — or avoid entirely for a citable, freely-redistributable research artifact.

### Alternative architecture: Colyseus
- **URL:** https://github.com/colyseus/colyseus
- **License:** MIT.
- **Integration note:** Authoritative-server model (Schema/StateView diffing) rather than peer CRDT merge. Better fit if the team prefers a single source of truth for enforcing zoning-compliance rules centrally (reject/rewrite mutations server-side before broadcast) rather than post-hoc CRDT merge + conflict UI. Trade-off: less offline-first than Yjs; would replace rather than augment the CRDT approach, so pick one path, not both, for the initial system.

**Recommendation:** Yjs + y-websocket for the paper's core system, with an explicit acknowledgment in the design-rationale section that a Loro-based tree-CRDT migration is future work once reparenting-heavy interactions are stress-tested.

---

## 2. Zoning / Parcel Data Grounding

### SanGIS/SANDAG Regional GIS Data Warehouse
- **URL:** https://rdw.sandag.org/ (public portal: https://sdgis-sandag.opendata.arcgis.com/)
- **License:** Public/open data with usage disclaimer (Data Access Memorandum Agreement needed only for higher-frequency parcel-ownership updates).
- **Integration note:** This is the authoritative source of real San Diego parcel geometry and zoning boundaries. Build a one-time (or periodically refreshed) ETL step that pulls parcel + zoning shapefiles, reprojects to the scene's local coordinate system, and materializes each parcel as a `ParcelConstraint` object registered in a new Pascal "systems" module (alongside existing systems like transform/material systems) — e.g., a `zoningSystem` that reads the active parcel ID from the Zustand store and exposes derived envelope constraints (setbacks, height limit, FAR) as selectors other systems (geometry validation, AI agent tool calls) can query synchronously. Using the official regional source lends the compliance-check results real legitimacy for the Center for Housing Policy and Design collaboration.

### City of San Diego Open Data Portal — GIS Zoning dataset
- **URL:** https://data.sandiego.gov/datasets/gis-zoning/
- **License:** Open government data (City of San Diego open data terms).
- **Integration note:** Machine-readable base-zone layer (CSV/SHP/GeoJSON/JSON) updated as rezone actions occur; join to parcel geometry from SanGIS to derive per-lot allowed-use/density/height envelopes. Citable as an official municipal source rather than a scrape — matters for the paper's legitimacy argument.

### Zoning and Parcel Information Portal (ZAPP)
- **URL:** https://www.sandiego.gov/development-services/zoning
- **License:** Public city service (ArcGIS-hosted).
- **Integration note:** Not for integration per se — use as a validation/cross-check reference and as a UX precedent in the paper: it demonstrates how the city already exposes zoning-by-parcel lookup to residents, which the proposed tool is explicitly surpassing (lookup → active co-design).

### National Zoning Atlas
- **URL:** https://www.zoningatlas.org/
- **License:** Open/public data, CC-licensed outputs; nonprofit (Cornell-led) research initiative.
- **Integration note:** Provides a standardized schema for translating dense municipal zoning-code text into structured numeric constraints (setbacks, height, FAR, use tables). Adopt this schema (rather than inventing an ad hoc one) as the constraint-rule format consumed by the `zoningSystem`, giving the paper a credible academic precedent for the rule representation.

### Zoneomics Zoning API (supplementary/fallback)
- **URL:** https://www.zoneomics.com/product/api
- **License:** Commercial/proprietary (API key, tiered pricing; free tier for tile service).
- **Integration note:** Fallback programmatic source for structured zoning attributes where SanGIS/city layers are incomplete, and its natural-language zoning-Q&A assistant ("Bassett") is a relevant related-system precedent for the voice interface design. **Flag:** commercial dependency — do not make this a hard requirement for reproducing the open-source research artifact; keep it optional/pluggable behind the same `zoningSystem` interface as the free SanGIS data.

### California Statewide Parcel Boundaries (ICE / UCLA Geoportal)
- **URL:** https://apps.gis.ucla.edu/geodata/dataset/california-statewide-parcel-boundaries
- **License:** Open/public academic GIS data.
- **Integration note:** Fallback/cross-validation parcel geometry and a path to generalize the constraint model beyond San Diego for future work — worth a sentence in the paper's generalizability discussion.

---

## 3. Voice / Speech Pipeline

### STT core: whisper.cpp
- **URL:** https://github.com/ggml-org/whisper.cpp
- **License:** MIT.
- **Integration note:** Dependency-free C/C++ Whisper port with Metal/CUDA/Vulkan/CPU backends and `--stream` mode (~0.5–2s latency with tiny/base models). Runs fully on-device — important for workshop venues with unreliable connectivity and for community privacy concerns (residents' voices should not have to leave the room). Run as a local process/service; a thin WebSocket bridge feeds partial transcripts to a new `voiceIntentSystem` that sits alongside Pascal's existing systems and dispatches resolved intents as the same store actions used by direct manipulation, so voice and mouse/touch edits go through one code path.

### Server-side alternative: faster-whisper
- **URL:** https://github.com/SYSTRAN/faster-whisper
- **License:** MIT.
- **Integration note:** CTranslate2 reimplementation, ~4x faster, lower memory. Use if the tool runs on a shared workshop laptop/GPU rather than in-browser; wrap as a small STT microservice.

### Streaming/incremental transcripts: whisper_streaming (ufal)
- **URL:** https://github.com/ufal/whisper_streaming
- **License:** MIT.
- **Integration note:** Implements local-agreement policies for genuinely incremental (not just final) transcription — lets the UI show provisional highlighting of the referenced 3D object *before* the utterance finishes, directly supporting voice-first disambiguation of spatial references (cf. Gazeify-Then-Voiceify's error taxonomy for referencing failures, a useful template for classifying our own voice-target ambiguity errors).

### Offline/low-resource fallback: Vosk
- **URL:** https://github.com/alphacep/vosk-api
- **License:** Apache-2.0.
- **Integration note:** Fully offline, as small as 50MB, streaming API, 20+ language models — appropriate for low-bandwidth/no-GPU community venues (libraries, church halls). Compatible license, no conflict.

### Voice-agent orchestration: Pipecat
- **URL:** https://github.com/pipecat-ai/pipecat
- **License:** BSD-2-Clause.
- **Integration note:** Ready-made pipeline skeleton (VAD, turn-taking, barge-in/interruption handling) orchestrating STT → LLM → TTS. Its function/tool-calling hooks are the natural attachment point for MCP-style tools that mutate the Pascal scene graph (see MCP below) — wire each tool call to dispatch a Zustand action, mirroring how CRDT remote updates are applied, so all mutation paths (local UI, remote CRDT, voice/LLM) converge on the same store API.

### Multi-participant alternative: LiveKit Agents
- **URL:** https://github.com/livekit/agents
- **License:** Apache-2.0.
- **Integration note:** WebRTC-native multi-participant room model — better fit than single-user voice frameworks for several community members speaking/gesturing concurrently around the same shared scene. Exposes function calls the LLM can invoke as MCP-style tools; would sit alongside (or replace) Pipecat depending on how many simultaneous speaking channels the study design needs.

### Modular local-only reference architecture: Rhasspy3
- **URL:** https://github.com/rhasspy/rhasspy3
- **License:** MIT.
- **Integration note:** Independent offline services (wake word, STT, intent, TTS) coordinated via the lightweight Wyoming protocol — a privacy-preserving reference architecture worth citing in the paper's ethics/data-handling discussion even if not fully adopted, since it demonstrates a fully local voice-assistant stack is achievable without cloud dependency.

### NLU baseline (maintenance-mode): Rasa Open Source
- **URL:** https://github.com/RasaHQ/rasa
- **License:** Apache-2.0.
- **Integration note:** Entity extraction/slot-filling directly applicable to resolving ambiguous spatial references ("move that wall back two feet") into structured intents. RasaHQ has shifted primary R&D elsewhere, so cite as a proven-pattern baseline rather than adopt as the production NLU layer.

### Tool-calling standard: Model Context Protocol (MCP)
- **URL:** https://github.com/modelcontextprotocol
- **License:** MIT.
- **Integration note:** This is the concrete standard underlying "the voice agent's LLM calls tools that mutate the scene graph." Expose Pascal's scene-graph operations (add wall, resize room, check setback, query zoning envelope) as a typed MCP server. The voice/LLM pipeline (Pipecat or LiveKit Agents) calls these tools after resolving an utterance; each tool implementation is a thin wrapper around existing `@pascal-app/core` Zustand actions — giving a standards-based integration rather than a bespoke voice-to-geometry mapping (directly differentiating from ad hoc approaches critiqued in the CoDesignAI and WeDesign literature reviews).

### Lightest-weight browser-only prototype path: Web Speech API + annyang
- **URL:** https://developer.mozilla.org/en-US/docs/Web/API/Web_Speech_API (polyfill/wrapper: e.g. `annyang`, MIT)
- **License:** Browser-native (varies by vendor); wrapper libraries typically MIT.
- **Integration note:** Since Pascal is already an R3F web app, this is the lowest-friction way to get voice input running in-browser with zero server round-trip — useful for an early demo/pilot version before investing in the full whisper/Pipecat backend described above.

---

## 4. IFC / BIM Compliance Export

### Core IFC engine: IfcOpenShell (+ Bonsai)
- **URL:** https://github.com/IfcOpenShell/IfcOpenShell
- **License:** **LGPL / GPL for the Bonsai add-on — flag this.** LGPL is weak copyleft: dynamic linking / calling IfcOpenShell as a separate process or library via its Python/CLI interface (rather than statically linking its code into Pascal's own MIT-licensed source) is the standard, safe integration pattern and does not obligate Pascal's own code to relicense. Treat IfcOpenShell as an external service/CLI dependency (e.g., invoked via `IfcConvert` or a Python microservice), not compiled into the core client bundle, to keep Pascal's MIT core clean. The Bonsai Blender add-on is GPL and should be treated purely as an optional external authoring tool for planners/reviewers, never embedded in the web client.
- **Integration note:** Use as the upstream complement to Pascal's own `@pascal-app/ifc-converter` — rather than reimplementing full IFC4/IFC4x3 schema handling, shell out to IfcOpenShell (server-side microservice) for spec-complete export/import, producing a standards-compliant BIM file architects, planning staff, and permit reviewers can open in Revit/ArchiCAD/Solibri.

### In-browser IFC: web-ifc / engine_web-ifc (That Open Company)
- **URL:** https://github.com/ThatOpen/engine_web-ifc
- **License:** MIT — no conflict.
- **Integration note:** WASM-compiled, native-speed in-browser IFC read/write. Since Pascal is R3F-based, this is a drop-in client-side layer: add an `ifcExportSystem` that serializes the current Zustand scene-graph state into web-ifc's in-memory model and emits an IFC file directly in the browser, no server round trip, keeping the live voice-first collaborative session fast while still producing a genuine IFC deliverable at any checkpoint.

### Compliance rule format: buildingSMART IDS
- **URL:** https://github.com/buildingSMART/IDS
- **License:** Open standard, openly maintained — no conflict.
- **Integration note:** Author each jurisdiction's zoning/code checklist once as an `.ids` rule file (setbacks present, unit counts tagged, egress widths specified) rather than ad hoc validation code. This is the standards-based analog to the National Zoning Atlas schema above, applied at the BIM/IFC layer instead of the parcel/GIS layer.

### IDS validation engine: IfcOpenShell IfcTester
- **URL:** https://github.com/IfcOpenShell/IfcOpenShell (IfcTester module)
- **License:** **LGPL — same flag as above; run as external process, not linked in-tree.**
- **Integration note:** Runs `.ids` rule files against exported IFC models and reports pass/fail per requirement. Pair directly with the IDS files above: the proposal-export pipeline (server-side, invoking IfcOpenShell/IfcTester as an external tool) produces a pass/fail zoning-compliance report attached to the community's exported proposal — this is the concrete mechanism for the paper's "legitimate alternative to a developer's proposal" claim, since it's the same class of automated check a planning department could recognize.

### UI reference: Model Checker (opensource-construction)
- **URL:** https://github.com/opensource-construction/model-checker
- **License:** Open source (verify specific LICENSE file before use; not explicitly confirmed as MIT/permissive).
- **Integration note:** Reference design (not necessarily reused code) for surfacing IDS/compliance results to non-expert users in an approachable web UI — useful as a design pattern for the plain-language compliance report shown alongside the community's 3D model, echoing the VizCrit finding that solution-centered (not just awareness-centered) feedback improves non-expert outcomes.

### Precedent for automated permit-check geometry logic: GeoBIM Building Permit Tool (TU Delft)
- **URL:** https://github.com/tudelft3d/GeoBIM-building-permit-tool
- **License:** Open source (TU Delft 3D geoinformation group; verify specific terms in repo).
- **Integration note:** Concrete, citable precedent for automated IFC-driven zoning/planning-code checks (max height, storey-footprint overlap, overhangs) already demonstrated in the field (Rotterdam GeoBIM pilot) — strengthens the paper's claim that geometry-based zoning compliance is a proven, not speculative, capability. Can inform the specific geometric-check algorithms implemented in Pascal's own `zoningSystem`/export pipeline rather than being integrated wholesale.

### .NET alternative: xbim Toolkit
- **URL:** https://github.com/xBimTeam/XbimEssentials
- **License:** **CDDL — flag this.** CDDL is a weak-copyleft, file-level license (similar spirit to LGPL but Mozilla-lineage); safe to use as a separate service/process (e.g., a .NET back-office review portal) but should not be statically combined into the MIT-licensed core client. Relevant only if a planning-department-facing back office component ends up being .NET-based; offers BCF (BIM Collaboration Format) issue-tracking for structured planner feedback on submitted proposals.

---

## Summary Table

| Subsystem | Primary pick | License | Conflict flag |
|---|---|---|---|
| Multi-user sync | Yjs + y-websocket | MIT | None |
| Multi-user sync (tree-native, prototype) | Loro | MIT/Apache-2.0 | None |
| Multi-user sync (avoid/limit) | Liveblocks sync engine | AGPL-3.0 (server) | **Yes — AGPL network-use clause** |
| Zoning/parcel data | SanGIS/SANDAG + City of San Diego GIS Zoning | Public/open gov data | None |
| Zoning rule schema | National Zoning Atlas | CC-licensed outputs | None |
| Voice STT | whisper.cpp / faster-whisper / Vosk | MIT / MIT / Apache-2.0 | None |
| Voice orchestration | Pipecat / LiveKit Agents | BSD-2-Clause / Apache-2.0 | None |
| Voice tool-calling standard | Model Context Protocol | MIT | None |
| IFC export (browser) | web-ifc | MIT | None |
| IFC export (server, spec-complete) | IfcOpenShell / IfcTester | **LGPL/GPL** | **Yes — use as external process only, do not statically link into MIT core** |
| Compliance rules | buildingSMART IDS | Open standard | None |
| .NET back-office (optional) | xbim Toolkit | **CDDL** | **Yes — same external-process caveat as LGPL** |

**Overall licensing posture:** Pascal's MIT core stays clean as long as (a) the AGPL Liveblocks sync engine is avoided or replaced with the MIT-licensed Yjs stack, and (b) IfcOpenShell/IfcTester (LGPL/GPL) and xbim (CDDL) are integrated exclusively as out-of-process services/CLI tools invoked over IPC/HTTP rather than compiled or statically linked into the client or core package — the standard, well-established pattern for combining permissively-licensed applications with copyleft-licensed libraries.