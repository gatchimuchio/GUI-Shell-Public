# Long Pitch

GUI-Shell addresses a practical gap in agent tooling: local runtimes need usable operator surfaces, but those surfaces must not silently become authority boundaries.

The project implements a desktop shell where UI state, adapter metadata, memory, diagnostics, and LLM output are non-authority inputs. Shell Core contracts define the governed state. The Rust broker handles native helper and IPC paths. Validation tooling checks schemas, conformance, manifest integrity, release gates, and Windows evidence semantics.

The public repository includes sanitized Windows proof assets and application notes for review. It does not claim completed product release readiness.
