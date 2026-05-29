# STACK LAW — Python is the apex

> **Python sits at the very top of the stack. Nothing is ever placed above it.**
> Every other language exists only as a *subordinate unit beneath* Python. They
> may execute; they never orchestrate. Python is the conductor — the rest is
> instruments.

This is a foundational EpochCore architectural law, not a style preference. It
applies to every repo in the org. Companion doc: `STACK_PACKAGES.md` (the blessed
best-in-class dependencies by domain).

## The layers

| Layer | Language | Role | Where it runs |
|---|---|---|---|
| **Apex** | **Python** | Orchestration, control, intelligence, the contract author | Everywhere — own infra, control plane, agents |
| Muscle | **C++** (pybind11 / Cython / C-ext) | Hot-path compute Python calls down into (hashing, crypto, sim, backtest loops) | Own infra (Triton/vLLM box, containers, VMs). **Not** CF Workers/Pyodide. |
| Output | **CSS / HTML** | Presentation, emitted by Python as strings/templates | Wherever it's rendered |
| Edge leaf | **JavaScript** | Forced runtime at the edge only (Cloudflare Workers) | CF Workers, browser |

**The rule is "Python on top," NOT "Python only."** Other languages are welcome —
as long as Python remains the orchestrator and nothing calls *up* into it.

### Allowed
- Python imports a compiled C++ module and calls it for speed (numpy/scipy pattern).
- Python emits CSS/HTML as output.
- Python generates a contract (e.g. an OpenAPI spec); a JS edge worker implements it.

### Violations
- JS (or anything) orchestrating, coordinating, or calling down into Python.
- An agent / control brain written in JS.
- A C++ layer that drives Python rather than serving it.

## The apex-API pattern (Python repos)

Any hot-path compute is reached through a Python "apex API" that prefers a
compiled accelerator and falls back to a byte-identical pure-Python
implementation — so the system runs with or without the native build, and Python
is never *dependent* on the lower layer.

This repo ships the canonical scaffold when it contains Python:

- `tooling/epoch_native.py` — the Python apex API. Callers import from here. It
  prefers the compiled `epoch_native_cpp` (from `native/`) and falls back to a
  byte-identical pure-Python implementation when the extension isn't built.
- `native/epoch_native.cpp` (+ `setup.py`, `pyproject.toml`) — pybind11 C++17
  muscle. Build with `pip install ./native` (needs a C++17 compiler + pybind11).
- `tests/test_epoch_native.py` — known vectors + C++↔Python parity (the parity
  test auto-skips until the extension is built).

```bash
pip install pybind11
pip install ./native      # needs MSVC Build Tools / gcc / clang
python -c "from tooling.epoch_native import content_seal, BACKEND; print(BACKEND, content_seal(b'x'))"
```

On a box without a compiler everything still works on the pure-Python fallback —
that graceful degradation is the law in action: Python never depends on the C++.

## Edge / JS repos

A JavaScript worker exists only because Cloudflare Workers mandates JS at the
edge. It implements a Python-authored contract and is deployed/validated by
Python. The edge leaf sits *below* the Python control plane — consistent with
this law. JS repos carry this `STACK_LAW.md` + `STACK_PACKAGES.md` as the
standard; they do not get the Python `tooling/` scaffold (there is no apex to add).

## Canonical reference

The originating reference implementation lives in `studio-skills-worker`
(Python control plane: spec generation + deploy + smoke; JS only as the forced
edge leaf; the `tooling/`+`native/` apex-muscle pattern with passing tests).
