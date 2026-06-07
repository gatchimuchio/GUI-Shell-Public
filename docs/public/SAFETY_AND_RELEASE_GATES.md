# Safety And Release Gates

GUI-Shell does not treat UI state, LLM output, adapter metadata, previous state, local cache, diagnostics, or external tool output as authority.

Sensitive actions must map to:

- capability
- permission
- approval state
- audit event
- recovery action

Release gate status:

- item: owner GO is absent
  classification: release_blocker
  registry_id: owner_go
  reason: owner approval is a separate release input
  required_action: record explicit owner GO before any completed product release claim
  blocks_release: yes
- item: mobile scope is outside this public Windows-first package
  classification: post_v1_scope
  reason: desktop Windows evidence is the current review target
  required_action: validate mobile separately before claiming mobile support
  blocks_release: no
- item: macOS host evidence is outside this Windows-first package
  classification: known_limitation
  reason: macOS validation requires a macOS host
  required_action: validate on macOS before claiming macOS support
  blocks_release: no

No OpenAI endorsement is claimed.

Language-policy runtime blockers are tracked through the Rust Security Broker and production IPC path. The public package keeps no-python-runtime and no-ffi-authority assertions visible in validation material.
