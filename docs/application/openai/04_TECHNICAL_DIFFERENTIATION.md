# 技術的差異

GUI-Shell の差異は、契約優先と証拠範囲の明示にある。

- Runtime、Adapter、Approval、Audit、Recovery、broker、Agent surface を表す JSON Schema contract
- Authority Strip と Content Exposure Boundary の conformance test
- replay を拒否し、fail-closed に処理する broker IPC
- 機械可読な release blocker registry
- source manifest の整合性検証
- 墨消し済み Windows installed-path evidence の公開概要

GUI-Shell は terminal wrapper でも、BLUE-TANUKI 専用 UI でもない。

## 外部公開用英語射影（非正本）

```text
GUI-Shell combines contract-first schemas, authority/content conformance, fail-closed broker IPC, explicit release blockers, manifest integrity, and sanitized Windows proof summaries. It is neither a terminal wrapper nor a BLUE-TANUKI-specific UI.
```
