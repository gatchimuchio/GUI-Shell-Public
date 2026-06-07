# Project Overview

GUI-Shell is a Windows-first desktop Runtime Operation Shell. It provides an inspectable control surface for local runtimes and agent-oriented tools while preserving explicit authority, approval, audit, and recovery boundaries.

The public repository contains the reviewable implementation package:

- Flutter desktop UI
- Rust broker/helper
- Shell Core and adapter packages
- JSON Schema contracts
- conformance and validation tooling
- Windows staging and evidence collection scripts
- sanitized Windows proof assets

No OpenAI endorsement is claimed. No completed product release is claimed.

Release status:

- item: owner GO is absent
  classification: release_blocker
  registry_id: owner_go
  reason: public review material does not replace final owner approval
  required_action: record owner GO only after strict evidence review
  blocks_release: yes

Rust Security Broker production IPC, no-python-runtime, and no-ffi-authority checks remain visible release gate concepts in the public package.
