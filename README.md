# GUI-Shell

GUI-Shell is a Windows-first desktop Runtime Operation Shell for operating local runtimes, agents, tools, and helper services through explicit contracts instead of hidden authority.

It combines a Flutter desktop UI, a Rust broker/helper, Python validation tooling, JSON Schema contracts, and conformance tests. The project is structured so reviewers can inspect how authority, approval, audit, recovery, and evidence gates are represented.

No OpenAI endorsement is claimed.

## What GUI-Shell Is

GUI-Shell is a generic Runtime Operation Shell control plane. It is designed to sit between an operator and one or more runtimes, exposing status, approvals, diagnostics, and recovery surfaces without making the UI itself the authority boundary.

The current public package focuses on the desktop implementation and the validation substrate:

- Flutter desktop UI in `apps/desktop_flutter`
- Rust broker/helper in `native/rust_helper`
- Shell Core and adapter packages in `packages`
- JSON Schema contracts in `specs`
- validation, manifest, release-gate, and evidence tooling in `tooling`
- Windows installer/evidence collectors in `installer/windows`

## Why It Exists

Agent and local-runtime tools often blur UI state, runtime state, diagnostics, and execution authority. GUI-Shell makes those boundaries explicit:

- the UI renders and collects operator input
- Shell Core owns policy-shaped state and contract checks
- adapters normalize runtime data without granting authority
- the Rust broker is the native boundary for authority-sensitive helper paths
- release evidence is separated from product-release claims

## Windows-First Desktop Scope

The public repository is scoped to the Windows-first desktop path for v1.0 review. Linux remains a development and verification slice. Mobile and macOS are not v1.0 claims.

Mobile and macOS status:

- item: mobile implementation is outside the public v1.0 package
  classification: post_v1_scope
  reason: the public package is focused on Windows-first desktop review
  required_action: validate and publish mobile separately if it becomes part of a later public scope
  blocks_release: no
- item: macOS host validation is not part of this Windows-first evidence package
  classification: known_limitation
  reason: macOS evidence requires a macOS validation host
  required_action: validate on macOS before claiming macOS support
  blocks_release: no

## Architecture Overview

The core boundary is:

```text
Flutter UI -> Shell Core contracts -> Rust broker/helper -> runtime or native operation
```

Important directories:

- `apps/desktop_flutter`: desktop operator UI
- `packages/shell_core`: runtime-neutral policy, approval, audit, recovery, and state helpers
- `packages/blue_tanuki_adapter`: reference adapter boundary example
- `native/rust_helper`: broker IPC, audit anchor, diagnostics, and native helper logic
- `specs`: JSON Schema contracts
- `tooling/conformance_tests`: conformance checks for authority and evidence behavior
- `installer/windows`: Windows staging and evidence collectors

## Safety / Authority Boundary

GUI-Shell treats LLM output, UI state, adapter metadata, memory, logs, previous state, and diagnostics as non-authority sources. Sensitive actions must map to capability, permission, approval state, audit event, and recovery action.

The public code preserves these constraints:

- Flutter does not own authority decisions
- adapter metadata cannot grant permission
- full payload display requires `content_visibility=full`
- approval edits are field-scoped and revalidated
- broker command dispatch remains fail-closed unless explicitly eligible
- audit anchor evidence is separated from external tamper-evidence claims

## Validation and Evidence

Core validation commands:

```bash
python3 tooling/schema_check/check_schemas.py
python3 tooling/conformance_tests/run_conformance_skeleton.py
python3 tooling/manifest.py --check
python3 tooling/release_gate_check.py
python3 tooling/validate_all.py --python-only
```

Native and UI validation when toolchains are available:

```bash
cd native/rust_helper && cargo test
cd apps/desktop_flutter && flutter analyze
cd apps/desktop_flutter && flutter test
dart format --output=none --set-exit-if-changed apps/desktop_flutter
```

## Windows Proof Assets

Public Windows evidence summaries live under:

```text
public_assets/windows_proof_pack/
```

The proof pack contains sanitized indexes, hashes, selected validation logs, and redacted evidence copies. It does not contain the private repository's raw `release_evidence/` directory as canonical release evidence.

## What Is Not Claimed

- No OpenAI endorsement is claimed.
- No completed product release is claimed.
- Strict release remains gated by owner GO.
- Mobile and macOS are not v1.0 product claims.
- Public proof assets are review material, not a replacement for the private release gate.

Current release blockers for completed product release:

- item: explicit owner GO is absent
  classification: release_blocker
  registry_id: owner_go
  reason: owner GO must be recorded separately from CI and Windows evidence
  required_action: record explicit owner GO only after strict evidence review
  blocks_release: yes

## Current Status

Windows-first desktop evidence has been collected and summarized for review. The public package is intended for code review, architecture review, safety-boundary review, and OpenAI/Codex application context.

`release_ready` is not asserted by this repository.

## How To Run

Install Flutter 3.22.x or newer, Rust, and Python 3.12 or newer. Then:

```bash
cd apps/desktop_flutter
flutter pub get
flutter run -d windows
```

On Linux development hosts:

```bash
cd apps/desktop_flutter
flutter pub get
flutter run -d linux
```

## How To Validate

For a Python-only review pass:

```bash
python3 tooling/validate_all.py --python-only
```

For Rust broker validation:

```bash
cd native/rust_helper
cargo test
```

For Flutter desktop validation:

```bash
cd apps/desktop_flutter
flutter analyze
flutter test
```

## Roadmap

The current public package centers on Windows-first desktop review. See `ROADMAP.md`, `CLAIM.md`, and `RELEASE_CHECKLIST.md` for release-gate classification and known non-claims.

## OpenAI/Codex Relevance

GUI-Shell is relevant to Codex and agent tooling because it presents an LLM-readable responsibility substrate: schemas, conformance checks, release gates, approval boundaries, audit mapping, and recovery paths are intended to be inspectable by implementation agents without treating the LLM as an authority source.
