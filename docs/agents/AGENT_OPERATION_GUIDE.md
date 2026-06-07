# Agent Operation Guide

This guide explains how LLM-based development agents should approach public GUI-Shell tasks.

## Basic Workflow

1. Inspect the repository state and relevant files before editing.
2. Identify the task scope and affected boundary.
3. Avoid authority, release, evidence, owner GO, and credential mutation unless explicitly instructed.
4. Make small, reviewable diffs.
5. Run the relevant validation commands.
6. Report evidence, limits, and release-gate status accurately.

## Task Profiles

### Documentation Edit

Allowed files:

- `README.md`
- `QUICKSTART.md`
- `docs/public/`
- `docs/application/`
- `docs/agents/`

Restricted files:

- `release_blockers.registry.json`
- `tooling/release_gate_check.py`
- canonical release evidence paths

Required tests:

- `python tooling/release_gate_check.py`
- `python tooling/manifest.py --check`
- `python tooling/validate_all.py --python-only` when the edit affects release, validation, or evidence language

Failure handling:

- Keep public claims conservative.
- Classify release-sensitive limits as `release_blocker`, `post_v1_scope`, or `known_limitation`.
- Do not repair release language by weakening a gate.

### UI Edit

Allowed files:

- non-authority UI surfaces under `apps/desktop_flutter/`
- UI tests under `apps/desktop_flutter/test/`

Restricted files:

- Shell Core authority logic
- Rust broker authority paths
- command dispatch or approval-finalization logic

Required tests:

- `flutter analyze`
- `flutter test`
- `dart format --output=none --set-exit-if-changed apps/desktop_flutter`
- Python validation if UI text affects release/evidence claims

Failure handling:

- Treat UI display success as UI evidence only.
- Do not claim runtime authority or release evidence from UI tests alone.

### Validation Or Tooling Edit

Allowed files:

- `tooling/`
- validation docs under `docs/public/` or `docs/agents/`

Restricted files:

- `tooling/release_gate_check.py`
- `tooling/windows_release_evidence.py`
- `tooling/evidence_bundle.py`
- `tooling/release_runtime_assertions.py`
- `MANIFEST.sha256.json`

Required tests:

- `python tooling/schema_check/check_schemas.py`
- `python tooling/conformance_tests/run_conformance_skeleton.py`
- `python tooling/manifest.py --check`
- `python tooling/release_gate_check.py`
- `python tooling/validate_all.py --python-only`

Failure handling:

- Fix the validator or source contract only when the failure shows a real public package problem.
- Do not loosen a validator to make a release gate pass.

### Broker Or Security Edit

Allowed files:

- `native/rust_helper/`
- `docs/security/`
- `docs/architecture/`
- matching tests and schema files

Restricted files:

- authority cutover
- command dispatch enablement
- credential handling
- audit finalization
- release evidence promotion

Required tests:

- `cargo fmt --check`
- `cargo test`
- `python tooling/conformance_tests/run_conformance_skeleton.py`
- `python tooling/release_runtime_assertions.py --check`
- `python tooling/validate_all.py --python-only`

Failure handling:

- Prefer fail-closed behavior.
- Report evidence class accurately: CONFIG, INTERNAL_STATE, LIVE_RUNTIME, EXTERNAL_EVIDENCE, or FIXTURE.

### Public Asset Edit

Allowed files:

- `public_assets/`
- redacted proof summaries
- screenshot indexes
- hash indexes

Restricted files:

- raw private evidence
- local transcripts
- private owner logs
- machine-specific environment dumps

Required tests:

- public/private boundary scan
- `python tooling/manifest.py --check`
- `python tooling/release_gate_check.py` if the asset index contains release language

Failure handling:

- Redact local paths, usernames, hostnames, and secret-like strings.
- Treat public proof assets as review material, not canonical release evidence.

### Release Evidence Edit

Allowed files:

- release evidence documentation
- Windows collector scripts only with explicit rationale

Restricted files:

- `release_blockers.registry.json`
- `tooling/windows_release_evidence.py`
- `tooling/evidence_bundle.py`
- `release_ready` flow
- owner GO flow

Required tests:

- `python tooling/windows_release_evidence.py`
- `python tooling/evidence_bundle.py --check`
- `python tooling/release_runtime_assertions.py --check`
- `python tooling/release_gate_check.py`
- `python tooling/validate_all.py --python-only`

Failure handling:

- Do not fabricate missing evidence.
- Do not convert CONFIG or FIXTURE evidence into installed-path proof.
- Report strict release status honestly.
