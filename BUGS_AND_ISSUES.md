# Bugs and Issues — Constitutional-Solutions Repositories

> Auto-generated analysis of all repos as of 2026-02-22.
> Severity: 🔴 Critical (blocking)  🟡 Major (incorrect behaviour)  🔵 Minor (inconsistency / docs)

---

## 🔴 CRITICAL — Blocking Bugs

### BUG-001 — `spiral-os`: Missing `ui/__init__.py`

**Repo:** [`spiral-os`](https://github.com/Constitutional-Solutions/spiral-os)
**File:** `ui/__init__.py` — does not exist

Running `python -m ui.spiral_window` fails with `ModuleNotFoundError` because Python cannot
treat `ui/` as a package without `__init__.py`.

**Fix:**
```bash
# In the spiral-os repo root
touch ui/__init__.py
```

All other subdirectories also lack `__init__.py`:
- `core/__init__.py` ❌
- `agent/__init__.py` ❌
- `connectors/__init__.py` ❌

**Workaround** (runs but does not fix the import path):
```bash
python ui/spiral_window.py
```

---

### BUG-002 — `aeuc-geometry-engine`: Repository is completely empty

**Repo:** [`aeuc-geometry-engine`](https://github.com/Constitutional-Solutions/aeuc-geometry-engine)
**State:** Empty — no commits, no files (HTTP 409 on default branch reference)

The repository was created and appears in the AEUC stack diagrams, but contains no code.
The original implementation exists in `spiral-os/FAMILY_CORE_SYSTEMS_v1.ipynb`
(the `GeometryEngine` class) and the full spec is in `aeuc-api/NEXT_STEPS.md` Phase 3.

**Fix:** Port the `GeometryEngine` from `FAMILY_CORE_SYSTEMS_v1.ipynb` into a proper
pip-installable package following the same structure as `aeuc-harmonic-engine`:
```
aeuc-geometry-engine/
├── geometry_engine/
│   ├── __init__.py
│   ├── constants.py      (phi, 144k, 1.44M grid constants, Platonic solid data)
│   ├── primitives.py     (tetrahedron, octahedron, cube, icosahedron, dodecahedron)
│   ├── engine.py         (GeometryEngine — payload resolver + FSOU audit)
│   └── exporters.py      (OBJ / STL / Three.js JSON / Blender export)
├── tests/
│   ├── __init__.py
│   └── test_engine.py
├── pyproject.toml        (name = "aeuc-geometry-engine", deps = [])
└── README.md
```

---

## 🟡 MAJOR — Incorrect / Divergent Behaviour

### BUG-003 — `aeuc-api`: Divergent outer context definitions vs. `glyph-registry`

**Repos:** [`aeuc-api`](https://github.com/Constitutional-Solutions/aeuc-api) vs.
[`glyph-registry`](https://github.com/Constitutional-Solutions/glyph-registry)
**Files:** `aeuc_api/registry.py` vs. `glyph_registry/registry.py`

The two repos define **different names for outer context IDs 6–9**. When `aeuc-api`
eventually imports from `glyph-registry` (as planned), context lookups will break.

| outer_id | `glyph-registry` (canonical) | `aeuc-api` (divergent) |
|----------|-------------------------------|------------------------|
| 6 | `CTX_TEMPORAL` | `CTX_EVOLUTION` |
| 7 | `CTX_SYMBOLIC` | `CTX_SYNTHESIS` |
| 8 | `CTX_BIOMETRIC` | `CTX_ARCHIVE` |
| 9 | `CTX_SOVEREIGN` | `CTX_SOVEREIGN` ✅ |

**Fix in `aeuc-api/aeuc_api/registry.py`:** Update `_initialize_default_contexts` to match
`glyph-registry`:
```python
# Replace the bottom four entries:
(6, "CTX_TEMPORAL",  "Temporal / chronos context"),
(7, "CTX_SYMBOLIC",  "Symbolic / glyph-language context"),
(8, "CTX_BIOMETRIC", "Biometric / sensor context"),
(9, "CTX_SOVEREIGN", "Sovereign / governance context"),
```

---

### BUG-004 — `aeuc-api`: `update_glyph` only allows updating `description`

**Repo:** [`aeuc-api`](https://github.com/Constitutional-Solutions/aeuc-api)
**File:** `aeuc_api/registry.py`, method `update_glyph`

The `aeuc-api` embedded registry's `update_glyph` signature is:
```python
def update_glyph(self, glyph_id: int, description: str) -> Glyph144k:
```

The `glyph-registry` version allows updating any mutable field:
```python
def update_glyph(self, glyph_id: int, **updates: Any) -> Glyph144k:
    # Mutable: code, description, category, geometry_payload,
    #          harmonic_payload, protocol_payload, version
```

The `PATCH /glyphs/{glyph_id}` endpoint is therefore more limited than what `glyph-registry`
supports. The API cannot currently update `category`, payloads, etc.

**Fix:** When replacing `aeuc_api/registry.py` with a proper import from `glyph-registry`,
also update the `PATCH` route and Pydantic models to expose all mutable fields.

---

### BUG-005 — `aeuc-api/registry.py`: `_audit` method hashes before AND after in same call (double hash risk)

**Repo:** [`aeuc-api`](https://github.com/Constitutional-Solutions/aeuc-api)
**File:** `aeuc_api/registry.py`

In the embedded `GlyphRegistry`, the `_audit` helper:
1. Captures `old = self.current_hash` (pre-change hash — still the previous hash)
2. Calls `self._update_hash()` (recomputes based on already-mutated data)
3. Appends log entry

This ordering **looks** correct, but in `add_glyph` the data has already been mutated
before `_audit` is called. So `old` captures the hash from the previous state, then
`_update_hash` recomputes over the new data. This actually works, but it's fragile —
any future developer who calls `_audit` before mutating data will get a wrong `hash_before`.

The `glyph-registry` version is cleaner: it always saves `old_hash = self.current_hash`
in the CRUD method before mutation, then calls `_update_hash()` and then `_audit(action, old_hash, ...)`.

**Fix:** When replacing `aeuc_api/registry.py` with the real import, this issue disappears.
Until then, document the call order clearly in `aeuc_api/registry.py`.

---

## 🔵 MINOR — Documentation / Consistency Issues

### BUG-006 — `aeuc-api/README.md`: Harmonic engine incorrectly marked "coming"

**Repo:** [`aeuc-api`](https://github.com/Constitutional-Solutions/aeuc-api)
**File:** `README.md`

The stack table shows:
```
| 3 — Harmonic engine | aeuc-harmonic-engine *(coming)* |
```

But `aeuc-harmonic-engine` is complete (v1.0.0 with full test suite).

**Fix:** Update the table:
```markdown
| 3 — Harmonic engine | [`aeuc-harmonic-engine`](https://github.com/Constitutional-Solutions/aeuc-harmonic-engine) |
```

---

### BUG-007 — `glyph-registry/ROADMAP.md`: Phase 3 marked as "not started"

**Repo:** [`glyph-registry`](https://github.com/Constitutional-Solutions/glyph-registry)
**File:** `ROADMAP.md`

Phase 3 in the ROADMAP shows harmonic engine items as unchecked `[ ]`, implying they
have not been implemented. But `aeuc-harmonic-engine` is complete.

**Fix:** Mark the harmonic engine items as complete `[x]` and update the geometry engine
to reflect its "in progress" status.

---

### BUG-008 — `aeuc-harmonic-engine/pyproject.toml`: Empty dependency list

**Repo:** [`aeuc-harmonic-engine`](https://github.com/Constitutional-Solutions/aeuc-harmonic-engine)
**File:** `pyproject.toml`

```toml
dependencies = []
```

The `HarmonicEngine` currently works standalone (no external imports from `glyph-registry`).
However, its primary purpose is to resolve `Glyph144k.harmonic_payload` dictionaries.
When integrated into production pipelines that pass actual `Glyph144k` objects, a dependency
on `glyph-registry` will be needed.

**Fix:** No immediate action required since the engine works standalone. Add `glyph-registry`
when creating the integration layer.

---

### BUG-009 — `aeuc-vector-db/pyproject.toml`: Missing `glyph-registry` dependency

**Repo:** [`aeuc-vector-db`](https://github.com/Constitutional-Solutions/aeuc-vector-db)
**File:** `pyproject.toml`

`VectorFieldDB` accepts `glyph_id` and `outer_context_id` as integers (anchored to the
`glyph-registry` address space) but does not validate against a live registry instance.
The dependency is not listed:
```toml
dependencies = [
    "numpy>=1.24",
]
```

**Fix:** When the vector DB is wired up to validate `glyph_id` ranges against a live
`GlyphRegistry`, add `glyph-registry>=1.0.0` to the dependencies.

---

### BUG-010 — `spiral-os/requirements.txt`: Lists `Pillow` but it is unused

**Repo:** [`spiral-os`](https://github.com/Constitutional-Solutions/spiral-os)
**File:** `requirements.txt`, `ui/spiral_window.py`

`requirements.txt` lists `Pillow>=10.0.0` but `spiral_window.py` does not import or use it.
The spiral_window UI uses only Python standard library (`tkinter`, `json`, `math`, `time`,
`dataclasses`).

**Fix:** Remove `Pillow>=10.0.0` from `requirements.txt` or add a comment marking it as
a future/optional dependency:
```
# Pillow>=10.0.0  # Reserved for future image/icon support
```

---

### BUG-011 — `aeuc-api`: Missing `aeuc-api` package (no `__init__.py` content)

**Repo:** [`aeuc-api`](https://github.com/Constitutional-Solutions/aeuc-api)
**File:** `aeuc_api/__init__.py`

The `__init__.py` is present but its content is minimal (149 bytes). It should export the
`app` object and version for `from aeuc_api import app` style imports.

**Fix:** Add to `aeuc_api/__init__.py`:
```python
from .main import app
from importlib.metadata import version, PackageNotFoundError

try:
    __version__ = version("aeuc-api")
except PackageNotFoundError:
    __version__ = "0.0.0"

__all__ = ["app", "__version__"]
```

---

## 🗂️ STRUCTURAL — Missing Implementations

These are not bugs but planned features that have no code yet.

| ID | Repo | Missing | Priority |
|----|------|---------|----------|
| S-001 | `aeuc-geometry-engine` | Entire Python package | HIGH (phase 3b) |
| S-002 | `aeuc-cli` | Entire repo does not exist | MEDIUM (phase 4) |
| S-003 | `aeuc-docs` | Entire repo does not exist | MEDIUM (phase 4) |
| S-004 | `spiral-base-lang` | Python package (`spiral_base/`) | HIGH (all primitives depend on it) |
| S-005 | `relation-primitives` | Python package (`relation_primitives/`) | MEDIUM |
| S-006 | `unit-primitives` | Python package (`unit_primitives/`) | MEDIUM |
| S-007 | `resonance-primitives` | Python package (`resonance_primitives/`) | LOW |
| S-008 | `sensation-primitives` | Python package (`sensation_primitives/`) | LOW |
| S-009 | `novelty-primitives` | Python package (`novelty_primitives/`) | LOW |
| S-010 | `link-primitives` | Python package (`link_primitives/`) | LOW |
| S-011 | `spiral-os/core/` | Kernel + ChoiceRouter Python code | MEDIUM |
| S-012 | `spiral-os/agent/` | Aletheia orchestrator Python code | MEDIUM |
| S-013 | `spiral-os/connectors/` | Connector framework Python code | LOW |
| S-014 | Team 3 repo | Entire repo missing (Teams 1,2,4,5 exist) | LOW |

---

## 🔀 CONSOLIDATION NEEDED

### CONSOLIDATION-001 — Three provisioner repos

The following three repos appear to overlap significantly:
- [`aeuc-provisioner`](https://github.com/Constitutional-Solutions/aeuc-provisioner) — has JS code
- [`aeuc-provisioner-v2.4`](https://github.com/Constitutional-Solutions/aeuc-provisioner-v2.4) — README only
- [`aeuc-pipedream-provisioner`](https://github.com/Constitutional-Solutions/aeuc-pipedream-provisioner) — README only

**Recommendation:** Decide on one canonical repo. If `aeuc-provisioner` is canonical,
archive or delete the others. If `aeuc-provisioner-v2.4` is the "latest", port the
JS code from `aeuc-provisioner` into it.

---

## Summary

| Severity | Count | Status |
|----------|-------|--------|
| 🔴 Critical | 2 | BUG-001 (missing `__init__.py`), BUG-002 (empty geometry engine repo) |
| 🟡 Major | 3 | BUG-003 (context mismatch), BUG-004 (limited update), BUG-005 (audit fragility) |
| 🔵 Minor | 6 | BUG-006 through BUG-011 (docs, dependencies, style) |
| 🗂️ Structural | 14 | S-001 through S-014 (planned features not yet implemented) |
| 🔀 Consolidation | 1 | Three overlapping provisioner repos |
