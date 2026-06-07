# Architecture Summary

GUI-Shell separates display, contract evaluation, native helper behavior, and runtime integration.

```text
Operator
  -> Flutter desktop UI
  -> Shell Core contracts and policy-shaped state
  -> Rust broker/helper
  -> runtime adapter or native operation
```

Boundary summary:

- Flutter owns rendering, navigation, and operator input.
- Shell Core owns runtime-neutral authority, approval, audit, recovery, and content visibility logic.
- Adapters normalize runtime data but do not grant authority.
- Rust helper owns native broker, IPC, diagnostics, and audit-anchor support.
- JSON Schema and conformance tests define the public contract gate.

The public package keeps BLUE-TANUKI as a reference adapter surface only. Shell Core remains runtime-neutral.

Rust Security Broker production IPC is the intended native boundary for authority-sensitive paths. Public validation keeps no-python-runtime and no-ffi-authority assertions explicit, including the rule that FFI is not an authority path.

Release evidence and owner approval remain separate release_blocker inputs; architecture documentation does not convert evidence into release readiness.
