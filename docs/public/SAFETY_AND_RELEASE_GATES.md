# 安全境界とリリース関門

GUI-Shell は、UI state、LLM output、Adapter metadata、previous state、local cache、diagnostics、tool output を権限源として扱わない。

すべての sensitive action は次へ対応付ける。

- Capability
- Permission
- Approval state
- AuditEvent
- failure 時の RecoveryAction

現在の release 状態:

- item: owner GO がない
  classification: release_blocker
  registry_id: owner_go
  reason: owner Approval は公開証拠とは別の release input である。
  required_action: 完成製品 release の主張より前に明示的な owner GO を記録する。
  blocks_release: yes
- item: Mobile は公開 Windows-first package の範囲外
  classification: post_v1_scope
  reason: 現在の review target は Windows desktop である。
  required_action: Mobile を主張する前に別途実装・検証する。
  blocks_release: no
- item: macOS host evidence がない
  classification: known_limitation
  reason: macOS support は macOS host で未検証である。
  required_action: macOS support を主張する前に macOS host で検証する。
  blocks_release: no

Public review snapshot の GitHub Release は、完成製品 release ではない。完成製品の release readiness は `release_blockers.registry.json` と明示的な owner GO が制御する。

公開 Windows proof asset は実測証拠から作成した墨消し済み review copy であり、canonical release evidence へ昇格せず、release blocker を解消しない。

Rust Security Broker の production IPC は authority-sensitive な本番境界である。`no-python-runtime` と `no-ffi-authority` は release assertion として検査するが、local source / test の成功を installed-path product evidence へ昇格しない。Broker production IPC、installed no-Python runtime、FFI authority bypass 不在の実測証拠が不足する間は release blocker を維持する。
