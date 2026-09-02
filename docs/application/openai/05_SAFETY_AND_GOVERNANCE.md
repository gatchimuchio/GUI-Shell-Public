# 安全と統治

安全境界:

- LLM output は権限ではない。
- UI state は権限ではない。
- Adapter metadata は権限ではない。
- diagnostics は権限ではない。
- sensitive action には Capability、Permission、Approval、AuditEvent、RecoveryAction の対応が必要である。

Release 統治:

- item: owner GO がない
  classification: release_blocker
  reason: 公開証拠は明示的な owner Approval を代替しない。
  required_action: strict release review 後に限り owner GO を記録する。
  blocks_release: yes

## 外部公開用英語射影（非正本）

```text
LLM output, UI state, adapter metadata, and diagnostics never grant authority. Sensitive actions require capability, permission, approval, audit, and recovery mapping; public evidence does not replace owner GO.
```
