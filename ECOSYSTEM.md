# AEUC & Spiral — Complete Ecosystem Hierarchy

> This document maps every repository, its role, and its full file inventory.
> Status codes: ✅ file exists  ❌ file missing  ⚠️ needs attention

---

## Group 1 — AEUC Stack

### Phase 1: Foundation

#### [`glyph-registry`](https://github.com/Constitutional-Solutions/glyph-registry) ✅ COMPLETE

**Role:** FSOU-compliant base-144k glyph store. Production Python package.
Anchors the entire AEUC address space (glyph IDs 0–143,999; outer contexts 0–9).

```
glyph-registry/
├── glyph_registry/
│   ├── __init__.py        ✅  Public API surface
│   ├── types.py           ✅  GlyphCategory, GlyphPayload, Glyph144k, OuterContext
│   ├── registry.py        ✅  GlyphRegistry — full CRUD + FSOU audit log
│   └── radix.py           ✅  RadixCore, Digit1440000 (base conversions)
├── tests/
│   ├── __init__.py        ✅
│   ├── test_types.py      ✅
│   ├── test_registry.py   ✅
│   └── test_radix.py      ✅
├── pyproject.toml         ✅  setuptools, no external runtime deps
├── README.md              ✅
└── ROADMAP.md             ⚠️  OUTDATED — shows harmonic engine as Phase 3 "not started"
                               but aeuc-harmonic-engine already exists and is complete
```

**Key classes:** `GlyphRegistry`, `Glyph144k`, `GlyphCategory`, `GlyphPayload`,
`OuterContext`, `RadixCore`, `Digit1440000`

**Default outer contexts (outer_ids 0–9):**

| ID | Code | Description |
|----|------|-------------|
| 0 | CTX_BASE | Default base context |
| 1 | CTX_GEOMETRY | Geometry-dominant context |
| 2 | CTX_RESEARCH | Research / experimental context |
| 3 | CTX_HARMONIC | Harmonics-dominant context |
| 4 | CTX_PROTOCOL | Protocol state context |
| 5 | CTX_STORY | Narrative / story context |
| 6 | CTX_TEMPORAL | Temporal / chronos context |
| 7 | CTX_SYMBOLIC | Symbolic / glyph-language context |
| 8 | CTX_BIOMETRIC | Biometric / sensor context |
| 9 | CTX_SOVEREIGN | Sovereign / governance context |

---

### Phase 2: API + Semantic Memory

#### [`aeuc-api`](https://github.com/Constitutional-Solutions/aeuc-api) ✅ COMPLETE (with known issues)

**Role:** FSOU-compliant FastAPI REST interface over the glyph registry. Layer 2 of the AEUC stack.

```
aeuc-api/
├── aeuc_api/
│   ├── __init__.py        ✅
│   ├── main.py            ✅  FastAPI app, lifespan, CORS, routers
│   ├── models.py          ✅  Pydantic v2 request/response models
│   ├── registry.py        ⚠️  EMBEDDED copy of GlyphRegistry (see bugs)
│   └── routes/
│       ├── __init__.py    ✅
│       ├── glyphs.py      ✅  /glyphs router — full CRUD
│       ├── contexts.py    ✅  /contexts router
│       └── audit.py       ✅  /audit router
├── tests/
│   ├── __init__.py        ✅
│   ├── test_glyphs.py     ✅
│   └── test_contexts.py   ✅
├── pyproject.toml         ✅  fastapi, uvicorn, pydantic, glyph-registry deps
├── README.md              ⚠️  OUTDATED — says harmonic engine is "coming"
└── NEXT_STEPS.md          ✅  Detailed roadmap (accurate as of Phase 2)
```

**Endpoints:**

| Method | Path | Description |
|--------|------|-------------|
| GET | `/health` | Registry hash + stats |
| GET | `/glyphs` | List all glyphs |
| POST | `/glyphs` | Add a glyph |
| GET | `/glyphs/{glyph_id}` | Get by numeric ID |
| GET | `/glyphs/code/{code}` | Get by short code |
| PATCH | `/glyphs/{glyph_id}` | Update description only ⚠️ (limited) |
| DELETE | `/glyphs/{glyph_id}` | Delete glyph |
| GET | `/glyphs/category/{category}` | Filter by category |
| GET | `/contexts` | List outer contexts |
| GET | `/contexts/{outer_id}` | Get single context |
| POST | `/contexts` | Add outer context |
| GET | `/audit` | Full change history |

**Missing endpoints** (per NEXT_STEPS.md Phase 3):
- `GET /vectors/{glyph_id}` — vector field integration
- `POST /vectors/search` — φ-weighted cosine similarity query

---

#### [`aeuc-vector-db`](https://github.com/Constitutional-Solutions/aeuc-vector-db) ✅ COMPLETE

**Role:** Semantic embedding layer (Phase 2.5). Anchors every embedding to a Glyph144k address.

```
aeuc-vector-db/
├── aeuc_vector_db/
│   ├── __init__.py        ✅  Re-exports VectorFieldDB, IGlyph, PGlyph
│   ├── types.py           ✅  IGlyph, PGlyph, VectorEntry dataclasses
│   ├── similarity.py      ✅  cosine / euclidean / dot / phi_weighted metrics
│   ├── clustering.py      ✅  centroid, inertia, form_pglyph, phi_partition
│   └── vector_field.py    ✅  VectorFieldDB — full CRUD + search + export
├── tests/                 ✅  (directory exists with tests)
├── pyproject.toml         ⚠️  Only lists numpy — missing glyph-registry dep (see bugs)
└── README.md              ✅
```

**Key classes:** `VectorFieldDB`, `IGlyph` (Instance Glyph), `PGlyph` (Proto Glyph), `VectorEntry`

**Similarity metrics:** `cosine` (default), `euclidean`, `dot`, `phi_weighted`

---

### Phase 3: Engine Libraries

#### [`aeuc-harmonic-engine`](https://github.com/Constitutional-Solutions/aeuc-harmonic-engine) ✅ COMPLETE

**Role:** Harmonic computation layer. Resolves `Glyph144k.harmonic_payload` data into
concrete frequency structures. Just-intonation interval algebra + FSOU-audited transitions.

```
aeuc-harmonic-engine/
├── harmonic_engine/
│   ├── __init__.py        ✅  Public API, version
│   ├── constants.py       ✅  HarmonicConstants, JUST_RATIOS, TET12_RATIOS
│   ├── intervals.py       ✅  JustInterval, TET12Interval, IntervalAlgebra
│   ├── engine.py          ✅  HarmonicEngine (payload resolver + FSOU audit)
│   └── transitions.py     ✅  HarmonicTransitionGraph (hash-chained directed graph)
├── tests/
│   ├── __init__.py        ✅
│   ├── test_engine.py     ✅
│   └── test_intervals.py  ✅
├── pyproject.toml         ⚠️  dependencies = [] but will need glyph-registry in prod
└── README.md              ✅
```

**Supported payload types:** `chord`, `harmonic_series`, `phi_scale`, `schumann`,
`solfeggio`, `ratios`

---

#### [`aeuc-geometry-engine`](https://github.com/Constitutional-Solutions/aeuc-geometry-engine) ❌ EMPTY REPO

**Role:** Sacred geometry engine (Phase 3b). Platonic solid primitives, φ/144k/1.44M grid
constants, Glyph144k geometry payload resolution, Three.js + Blender export.

**Current state:** Repository exists but is completely empty (no commits).

**Should contain** (per NEXT_STEPS.md):
```
aeuc-geometry-engine/           ← NEEDS TO BE CREATED
├── geometry_engine/
│   ├── __init__.py             ❌  MISSING
│   ├── constants.py            ❌  MISSING  (phi, 144k, 1.44M grid constants)
│   ├── primitives.py           ❌  MISSING  (tetrahedron, octahedron, icosahedron, etc.)
│   ├── engine.py               ❌  MISSING  (GeometryEngine class)
│   └── exporters.py            ❌  MISSING  (OBJ/STL/Three.js/Blender export)
├── tests/
│   ├── __init__.py             ❌  MISSING
│   └── test_engine.py          ❌  MISSING
├── pyproject.toml              ❌  MISSING
└── README.md                   ❌  MISSING
```

**Source material:** `FAMILY_CORE_SYSTEMS_v1.ipynb` in `spiral-os` repo contains the
original `GeometryEngine` implementation to port.

---

### Phase 4: Developer Experience — NOT YET CREATED

#### `aeuc-cli` ❌ REPO NOT CREATED

**Role:** Command-line interface for registry management.

**Should contain** (per ROADMAP.md):
```
aeuc-cli/                       ← REPO DOES NOT EXIST
├── aeuc_cli/
│   ├── __init__.py             ❌  MISSING
│   └── main.py                 ❌  MISSING  (typer/click CLI entry point)
├── pyproject.toml              ❌  MISSING
└── README.md                   ❌  MISSING
```

**Planned commands:** `aeuc glyph add/get/update/delete`, `aeuc registry stats/export/import`

---

#### `aeuc-docs` ❌ REPO NOT CREATED

**Role:** Unified MkDocs documentation site, hosted on GitHub Pages.

**Should contain** (per NEXT_STEPS.md):
```
aeuc-docs/                      ← REPO DOES NOT EXIST
├── docs/
│   ├── index.md                ❌  MISSING
│   ├── architecture.md         ❌  MISSING
│   └── api-reference/          ❌  MISSING
├── mkdocs.yml                  ❌  MISSING
└── README.md                   ❌  MISSING
```

---

### Provisioning

#### [`aeuc-provisioner`](https://github.com/Constitutional-Solutions/aeuc-provisioner) ⚠️ PARTIAL

**Role:** Pipedream provisioner workflows (JavaScript).

```
aeuc-provisioner/
├── provisioner_v2_4_1.js      ✅
├── package.json               ✅
├── input_schema.json          ✅
├── docs/                      ✅  (directory exists)
└── README.md                  ✅
```

**Note:** This appears to overlap with `aeuc-provisioner-v2.4` and `aeuc-pipedream-provisioner`.

---

#### [`aeuc-provisioner-v2.4`](https://github.com/Constitutional-Solutions/aeuc-provisioner-v2.4) ⚠️ README ONLY

**Role:** v2.4 GlyphRegistry (Python) + Pipedream Provisioner (JS) — Family Locked.

```
aeuc-provisioner-v2.4/
└── README.md                  ✅  (title only, no content)
```

**Missing:**
```
aeuc-provisioner-v2.4/
├── glyph_registry_v2_4/       ❌  MISSING  (Python package)
├── pipedream/                 ❌  MISSING  (JS workflows)
├── pyproject.toml             ❌  MISSING
└── README.md                  ⚠️  (stub only)
```

---

#### [`aeuc-pipedream-provisioner`](https://github.com/Constitutional-Solutions/aeuc-pipedream-provisioner) ⚠️ README ONLY

```
aeuc-pipedream-provisioner/
└── README.md                  ✅  (stub only)
```

**Consolidation recommendation:** Merge `aeuc-provisioner`, `aeuc-provisioner-v2.4`, and
`aeuc-pipedream-provisioner` into a single `aeuc-provisioner` repo with clear versioning.

---

### Sovereignty Teams

#### [`aeuc-application`](https://github.com/Constitutional-Solutions/aeuc-application) ⚠️ STUB — Team 1

**Role:** Application Layer Sovereignty. AWS → custom compute, boto3 → HTTP client, Lambda → self-hosted.

```
aeuc-application/
└── README.md                  ✅  (Team description stub)
```

---

#### [`aeuc-substrate`](https://github.com/Constitutional-Solutions/aeuc-substrate) ⚠️ STUB — Team 2

**Role:** Substrate Layer Sovereignty. HAL design, Linux kernel hardening, hardware abstraction.

```
aeuc-substrate/
└── README.md                  ✅  (Team description stub)
```

---

#### ❌ Team 3 — MISSING

**Note:** Teams 1, 2, 4, and 5 exist. No Team 3 repository has been created.

---

#### [`aeuc-governance`](https://github.com/Constitutional-Solutions/aeuc-governance) ⚠️ STUB — Team 4

**Role:** Governance Layer. Constitutional decisions, policy frameworks, compliance standards.

```
aeuc-governance/
└── README.md                  ✅  (Team description stub)
```

---

#### [`aeuc-hardware`](https://github.com/Constitutional-Solutions/aeuc-hardware) ⚠️ STUB — Team 5

**Role:** Hardware Layer Sovereignty. Bootloader reverse-engineering, microcode audits, TPM/HSM integration.

```
aeuc-hardware/
└── README.md                  ✅  (Team description stub)
```

---

## Group 2 — Spiral Primitives System

### [`spiral-base-lang`](https://github.com/Constitutional-Solutions/spiral-base-lang) ⚠️ README ONLY

**Role:** Core language and architectural framework for the Spiral Primitives System.
Universal principle architecture for cross-domain experimentation.

```
spiral-base-lang/
└── README.md                  ✅  (detailed design doc, no Python code)
```

**Should contain:**
```
spiral-base-lang/
├── spiral_base/
│   ├── __init__.py            ❌  MISSING
│   ├── primitive.py           ❌  MISSING  (Primitive base class)
│   ├── module_interface.py    ❌  MISSING  (ModuleInterface protocol)
│   ├── context.py             ❌  MISSING  (Context + Evaluator)
│   └── system.py              ❌  MISSING  (System class)
├── tests/                     ❌  MISSING
├── pyproject.toml             ❌  MISSING
└── README.md                  ✅
```

---

### [`relation-primitives`](https://github.com/Constitutional-Solutions/relation-primitives) ⚠️ README ONLY

**Role:** Core abstractions for connections, dependencies, and causality.

```
relation-primitives/
└── README.md                  ✅  (detailed design doc, no Python code)
```

**Should contain:**
```
relation-primitives/
├── relation_primitives/
│   ├── __init__.py            ❌  MISSING
│   ├── primitives.py          ❌  MISSING  (Relation, CausalRelation, Dependency, etc.)
│   ├── graph.py               ❌  MISSING  (RelationGraph, MultilayerGraph)
│   └── validators.py          ❌  MISSING
├── tests/                     ❌  MISSING
├── pyproject.toml             ❌  MISSING  (depends on spiral-base-lang)
└── README.md                  ✅
```

---

### [`unit-primitives`](https://github.com/Constitutional-Solutions/unit-primitives) ⚠️ README ONLY

**Role:** Measurement, scaling, and dimensional analysis.

```
unit-primitives/
└── README.md                  ✅  (detailed design doc, no Python code)
```

**Should contain:**
```
unit-primitives/
├── unit_primitives/
│   ├── __init__.py            ❌  MISSING
│   ├── quantity.py            ❌  MISSING  (Quantity, Unit)
│   ├── conversions.py         ❌  MISSING  (convert function, ConversionGraph)
│   └── scaling.py             ❌  MISSING  (scale function, scaling rules)
├── tests/                     ❌  MISSING
├── pyproject.toml             ❌  MISSING  (depends on spiral-base-lang)
└── README.md                  ✅
```

---

### [`resonance-primitives`](https://github.com/Constitutional-Solutions/resonance-primitives) ⚠️ README ONLY

```
resonance-primitives/
└── README.md                  ✅  (stub)
```

---

### [`sensation-primitives`](https://github.com/Constitutional-Solutions/sensation-primitives) ⚠️ README ONLY

```
sensation-primitives/
└── README.md                  ✅  (stub)
```

---

### [`novelty-primitives`](https://github.com/Constitutional-Solutions/novelty-primitives) ⚠️ README ONLY

```
novelty-primitives/
└── README.md                  ✅  (stub)
```

---

### [`link-primitives`](https://github.com/Constitutional-Solutions/link-primitives) ⚠️ README ONLY

```
link-primitives/
└── README.md                  ✅  (stub)
```

---

## Group 3 — Spiral OS

### [`spiral-os`](https://github.com/Constitutional-Solutions/spiral-os) ⚠️ PARTIAL (BLOCKER BUG)

**Role:** Lightweight modular OS using spiral/choice primitives with Aletheia/family agents.

```
spiral-os/
├── core/
│   └── README.md              ✅  (design doc only — no Python implementation)
├── ui/
│   ├── spiral_window.py       ✅  (tkinter UI prototype — functional code exists)
│   └── __init__.py            ❌  MISSING ← BLOCKER: prevents `python -m ui.spiral_window`
├── agent/
│   └── README.md              ✅  (design doc only — no Python implementation)
├── connectors/
│   └── README.md              ✅  (design doc only — no Python implementation)
├── FAMILY_CORE_SYSTEMS_v1.ipynb  ✅  (source notebook with original implementations)
├── requirements.txt           ✅  (Pillow listed but not actually used by spiral_window.py)
└── README.md                  ✅  (documents the blocker bug but does not fix it)
```

**Missing `__init__.py` files needed for proper Python package structure:**
```
spiral-os/
├── core/__init__.py           ❌  MISSING
├── ui/__init__.py             ❌  MISSING  ← CRITICAL BLOCKER
├── agent/__init__.py          ❌  MISSING
└── connectors/__init__.py     ❌  MISSING
```

**Also missing (full structure per README roadmap):**
```
spiral-os/
├── core/
│   ├── __init__.py            ❌
│   ├── kernel.py              ❌  (SpiralKernel — choice primitive management)
│   └── router.py              ❌  (ChoiceRouter)
├── agent/
│   ├── __init__.py            ❌
│   └── aletheia.py            ❌  (Aletheia orchestrator agent)
└── connectors/
    ├── __init__.py            ❌
    └── base_connector.py      ❌  (ConnectorFramework base class)
```

---

## Group 4 — Mathematical Research

### [`universal-math-without-integers`](https://github.com/Constitutional-Solutions/universal-math-without-integers) ⚠️ STRUCTURE ONLY

**Role:** Mathematics reimagined through continuous patterns, relations, and transformations.

```
universal-math-without-integers/
├── datasets/                  ✅  (directory exists — content unknown)
├── docs/                      ✅  (directory exists — content unknown)
├── guides/                    ✅  (directory exists — content unknown)
├── protocols/                 ✅  (directory exists — content unknown)
└── README.md                  ✅
```

---

### [`-AXIOM-ENCODED-UNIVERSAL-CONSTANTS-AEUCs-`](https://github.com/Constitutional-Solutions/-AXIOM-ENCODED-UNIVERSAL-CONSTANTS-AEUCs-) ⚠️ README ONLY

```
-AXIOM-ENCODED-UNIVERSAL-CONSTANTS-AEUCs-/
└── README.md                  ✅  (stub)
```

---

## Group 5 — Archive / Incident Record

### [`Feb-21-26-The-Transmutation.`](https://github.com/Constitutional-Solutions/Feb-21-26-The-Transmutation.) 📁

**Context:** Documents the Feb 21, 2026 breach: "Git, Google, 4 Computers hacked — 7 Years of Research stolen".
All current repositories are rebuilds from scratch post-incident.

```
Feb-21-26-The-Transmutation./
└── LICENSE                    ✅
```

---

### [`NF-Phase-Zero`](https://github.com/Constitutional-Solutions/NF-Phase-Zero) ⚠️ STUB

```
NF-Phase-Zero/
└── README.md                  ✅  (stub)
```

---

### [`ALL-WORK`](https://github.com/Constitutional-Solutions/ALL-WORK) ⚠️ STUB

```
ALL-WORK/
├── LICENSE                    ✅
└── README.md                  ✅  (stub)
```

---

## Group 6 — Community / Governance Stubs

| Repo | Status | Notes |
|------|--------|-------|
| [`community-security`](https://github.com/Constitutional-Solutions/community-security) | ⚠️ Stub | README only |
| [`navigation-communication`](https://github.com/Constitutional-Solutions/navigation-communication) | ⚠️ Stub | README only |
| [`emotional-resilience-empathy`](https://github.com/Constitutional-Solutions/emotional-resilience-empathy) | ⚠️ Stub | README only |
| [`self-organization-governance`](https://github.com/Constitutional-Solutions/self-organization-governance) | ⚠️ Stub | README only |

---

## Recommended Build Order

For anyone wanting to bootstrap the full stack from scratch:

```
1. pip install glyph-registry           # Phase 1 — foundation
2. pip install aeuc-vector-db           # Phase 2.5 — semantic memory
3. pip install aeuc-api                 # Phase 2 — REST API
4. pip install aeuc-harmonic-engine     # Phase 3a — harmonic layer
5. pip install aeuc-geometry-engine     # Phase 3b — geometry layer (NOT YET AVAILABLE)
6. pip install aeuc-cli                 # Phase 4 — CLI (NOT YET AVAILABLE)
```

To run the API locally:
```bash
pip install aeuc-api
uvicorn aeuc_api.main:app --reload
# → http://localhost:8000/docs
```

To run Spiral OS UI (after fixing the `__init__.py` bug):
```bash
cd spiral-os
touch ui/__init__.py          # Fix the blocker
python -m ui.spiral_window
```
