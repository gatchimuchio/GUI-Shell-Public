# Rust Security Broker 移行計画

> 状態: 公開用 migration 概要
> 射程: GUI-Shell-Public の review package

本書は release-facing な参照を解決する公開概要である。非公開実装note、raw evidence、canonical release evidence、`release_blockers.registry.json`を置き換えない。

## 目的

GUI-Shell は、権限に敏感な実働 Runtime の責任を、独立した Rust Security Broker のプロセスへ移す。Flutter は交換可能な UI 層であり、権限を所有しない。Python は開発用ツール、Schema 検証、適合検査、移行時の同等性検査、証拠検証に残せるが、導入済み完成製品の実働権限経路には依存させない。

## 境界

- Rust Security Broker は authority-sensitive IPC、Approval eligibility、Audit、Recovery、command-envelope gate、fail-closed response を担う。
- Flutter は rendering、navigation、operator input、local UI state だけを担う。
- Shell Core contract と Schema は contract gate を保つ。
- 権限に敏感な Flutter と Rust の接続には独立プロセス IPC を優先し、FFI は権限、署名、承認 token、外部 command 送信、Audit 確定の境界外に限る。
- Adapter metadata、UI state、LLM output、diagnostics、memory、cache、previous state は権限を付与しない。

## 現在の公開状態

- item: broker path をreviewできる
  classification: required_for_v1
  reason: broker IPC contract、Rust helper code、parity assertion、release runtime assertionを公開している。
  required_action: broker assertion、negative case、conformanceを通過状態に保つ。
  blocks_release: no

- item: 実働コマンドの送信
  classification: release_blocker
  reason: Capability、Permission、Approval、Audit、Recovery、installed-path evidenceのgateが完了するまで、real external command dispatchはSUSPENDする。
  required_action: 明示的な統治作業と実測証拠なしにdispatchを有効化しない。
  blocks_release: yes

- item: 導入済み製品の証拠
  classification: release_blocker
  reason: 完成製品releaseには、`release_blockers.registry.json`が定めるWindows installed-path evidenceとowner GOが必要である。
  required_action: native Windowsで統治されたevidenceを収集し、strict local validationを通過する。
  blocks_release: yes

## 非主張

本概要はauthority cutover、command dispatch readiness、installed productのno-Python-runtime proof、完成製品release readinessを主張しない。public proof copyはこれらを証明しない。
