# Safety And Governance

Safety model:

- LLM output is not authority.
- UI state is not authority.
- adapter metadata is not authority.
- diagnostics are not authority.
- sensitive actions require capability, permission, approval, audit event, and recovery mapping.

Release governance:

- item: owner GO is absent
  classification: release_blocker
  reason: public evidence does not replace explicit owner approval
  required_action: record owner GO only after strict release review
  blocks_release: yes
