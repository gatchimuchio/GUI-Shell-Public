# Rust Security Broker Migration Plan

Status: public-safe migration summary
Scope: public review snapshot documentation only

This file is a public-safe summary so release-facing references resolve. It does not replace private implementation notes, canonical release evidence, or governed release blockers.

## Objective

GUI-Shell is moving authority-sensitive runtime responsibilities toward an independent Rust Security Broker process. Flutter remains the replaceable UI layer and must not own authority. Python may remain for tooling, schema validation, conformance, migration parity, and evidence validation, but it must not become the installed active authority runtime dependency for completed product release.

## Boundaries

- Rust Security Broker owns authority-sensitive IPC, approval eligibility, audit, recovery, command-envelope gating, and fail-closed broker responses.
- Flutter owns rendering, navigation, operator input, and local UI state only.
- Shell Core contracts and schemas remain the contract gate.
- Adapter metadata, UI state, LLM output, diagnostics, memory, cache, and previous state do not grant authority.

## Current Public State

- item: broker path visible
  classification: required_for_v1
  reason: broker IPC contracts, Rust helper code, parity assertions, and release runtime assertions are present for review.
  required_action: keep broker assertions and conformance checks passing.
  blocks_release: no

- item: active command dispatch
  classification: release_blocker
  reason: real external command dispatch remains suspended until capability, permission, approval, audit, recovery, and installed-path evidence gates are complete.
  required_action: do not enable dispatch without explicit governed release work.
  blocks_release: yes

- item: installed product proof
  classification: release_blocker
  reason: completed product release requires installed-path Windows evidence and explicit owner GO as defined in `release_blockers.registry.json`.
  required_action: collect governed Windows installed-path evidence and pass strict validation before product release claims.
  blocks_release: yes

## Non-Claims

This summary does not claim authority cutover completion, command dispatch readiness, installed-product no-Python proof, or completed product release readiness.
