# Clean — Constitutional-Solutions Master Index

> **Purpose:** This repository is the organizational hub for all Constitutional-Solutions repositories.
> It documents the correct file hierarchy across every project, tracks what is complete vs. stub-only,
> and records all known bugs and missing dependencies.

---

## Quick Navigation

| Document | Purpose |
|----------|---------|
| [ECOSYSTEM.md](ECOSYSTEM.md) | Complete hierarchy of every repository and its files |
| [BUGS_AND_ISSUES.md](BUGS_AND_ISSUES.md) | All identified bugs, inconsistencies, and missing files |

---

## Ecosystem at a Glance

```
Constitutional-Solutions/
│
├── ── AEUC Stack (Adaptive Evolutionary Universal Consciousness)
│   ├── Phase 1 — Foundation
│   │   └── glyph-registry              ✅ COMPLETE
│   ├── Phase 2 — API + Semantic Memory
│   │   ├── aeuc-vector-db              ✅ COMPLETE
│   │   └── aeuc-api                    ✅ COMPLETE
│   ├── Phase 3 — Engine Libraries
│   │   ├── aeuc-harmonic-engine        ✅ COMPLETE
│   │   └── aeuc-geometry-engine        ❌ EMPTY REPO
│   ├── Phase 4 — Developer Experience
│   │   ├── aeuc-cli                    ❌ REPO NOT CREATED
│   │   └── aeuc-docs                   ❌ REPO NOT CREATED
│   ├── Provisioning
│   │   ├── aeuc-provisioner            ⚠️  PARTIAL (JS only)
│   │   ├── aeuc-provisioner-v2.4       ⚠️  README ONLY
│   │   └── aeuc-pipedream-provisioner  ⚠️  README ONLY
│   └── Sovereignty Teams
│       ├── aeuc-application  (Team 1)  ⚠️  README ONLY
│       ├── aeuc-substrate    (Team 2)  ⚠️  README ONLY
│       ├── [Team 3]                    ❌  MISSING
│       ├── aeuc-governance   (Team 4)  ⚠️  README ONLY
│       └── aeuc-hardware     (Team 5)  ⚠️  README ONLY
│
├── ── Spiral Primitives System
│   ├── spiral-base-lang                ⚠️  README ONLY (no Python code)
│   ├── relation-primitives             ⚠️  README ONLY
│   ├── unit-primitives                 ⚠️  README ONLY
│   ├── resonance-primitives            ⚠️  README ONLY
│   ├── sensation-primitives            ⚠️  README ONLY
│   ├── novelty-primitives              ⚠️  README ONLY
│   └── link-primitives                 ⚠️  README ONLY
│
├── ── Spiral OS
│   └── spiral-os                       ⚠️  UI PROTOTYPE (blocker bug: missing __init__.py)
│
├── ── Mathematical Research
│   ├── universal-math-without-integers ⚠️  STRUCTURE ONLY
│   └── -AXIOM-ENCODED-UNIVERSAL-CONSTANTS-AEUCs-  ⚠️  README ONLY
│
└── ── Archive / Other
    ├── Feb-21-26-The-Transmutation.    📁  LICENSE ONLY (hack incident record)
    ├── NF-Phase-Zero                   ⚠️  README ONLY
    ├── ALL-WORK                        ⚠️  README ONLY
    ├── community-security              ⚠️  README ONLY
    ├── navigation-communication        ⚠️  README ONLY
    ├── emotional-resilience-empathy    ⚠️  README ONLY
    ├── self-organization-governance    ⚠️  README ONLY
    └── Clean                           📌  THIS REPO
```

**Legend:** ✅ Complete  ⚠️ Partial/stub  ❌ Missing  📁 Archive  📌 This repo

---

## AEUC Dependency Graph

```
                     aeuc-cli  (Phase 4 — not yet created)
                         │ HTTP
              ┌──────────┴──────────┐
              │       aeuc-api      │  Phase 2
              │  (FastAPI REST)     │
              └──────────┬──────────┘
                         │ Python import (planned — currently embedded copy)
  ┌──────────────────────┴──────────────────────┐
  │             glyph-registry                  │  Phase 1
  │   types │ registry │ radix                  │
  └──────────────────────────────────────────────┘
        │                     │
        ▼                     ▼
aeuc-harmonic-engine    aeuc-geometry-engine   Phase 3
     (complete)            (empty repo ❌)
        │
        ▼
aeuc-vector-db                                 Phase 2.5
  (complete)
```

---

## Spiral Architecture Dependency Graph

```
spiral-base-lang  (core — README only, no Python)
├── relation-primitives  (README only)
├── unit-primitives      (README only)
├── resonance-primitives (README only)
├── sensation-primitives (README only)
├── novelty-primitives   (README only)
└── link-primitives      (README only)

spiral-os  (uses spiral primitives + AEUC stack)
├── core/      (README only — kernel placeholder)
├── ui/        (spiral_window.py — has BLOCKER bug)
├── agent/     (README only — Aletheia placeholder)
└── connectors/ (README only — API bridge placeholder)
```

---

## Top Priority Actions

1. **Fix `spiral-os`** — add `ui/__init__.py` (one-line fix, currently blocks all module-level execution)
2. **Initialize `aeuc-geometry-engine`** — empty repo needs `geometry_engine/` Python package
3. **Update `aeuc-api` README** — harmonic engine is now complete, remove "coming soon"
4. **Update `glyph-registry` ROADMAP** — Phase 3 harmonic engine is complete
5. **Reconcile context definitions** — `aeuc-api` and `glyph-registry` define different names for contexts 6–9
6. **Consolidate provisioner repos** — three repos overlap; decide canonical one

See [BUGS_AND_ISSUES.md](BUGS_AND_ISSUES.md) for the full list with details.
