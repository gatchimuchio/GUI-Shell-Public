# Public Repository Boundary

This document defines what belongs in the public GUI-Shell repository and what must stay outside it.

## Intentionally Public

The public repository may contain:

- source code
- validation tools
- schema and conformance tests
- public documentation
- redacted Windows proof assets
- OpenAI/Codex application support materials
- examples that do not reveal private environment data

These files support public review of architecture, authority boundaries, validation coverage, Windows-first scope, and agent-readable development contracts.

## Must Remain Private

The public repository must not contain:

- raw private evidence
- local machine paths
- secrets or credentials
- private owner logs
- unredacted transcripts
- owner GO private decision trail
- private repository-only notes
- full environment dumps that expose usernames, hostnames, or workstation-specific state

If a file contains public value but also private data, publish a redacted copy and document the redaction.

## Evidence Boundary

Public proof assets are promotional and reference materials for reviewers.

Canonical release evidence must be produced and validated by release tooling. Public assets must not be fabricated, hand-edited into a pass, or promoted into release evidence.

`release_evidence/` is not part of the public source package. Redacted summaries may live under `public_assets/windows_proof_pack/`.

Owner GO is not editable by public agents. Agents may report that owner GO is absent, but must not record it.

## Release Boundary

The public repository may state:

- Windows-first desktop evidence has been collected and summarized.
- Normal development validation passes when the listed commands pass.
- Strict release remains gated by owner GO and release evidence policy.

The public repository must not state:

- completed product release readiness
- OpenAI endorsement, certification, partnership, or acceptance
- mobile or macOS v1.0 support without separate validation
- administrator/root tamper resistance without external or signed evidence

## Boundary Scan Checklist

Before publishing public changes, scan for:

- `release_ready` true claims
- owner GO recorded
- OpenAI endorsement wording
- Windows user-home paths
- local usernames
- API key markers
- secret-like values
- token-like values
- raw `release_evidence/`
- local build outputs and caches
