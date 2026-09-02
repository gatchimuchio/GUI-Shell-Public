# 問題と解決

## 問題

Agent tool では、UIの利便性、diagnostics、Runtime metadata、実行権限が混在し、責任と失敗時挙動を監査しにくくなりやすい。

## 解決

GUI-Shell は責任を次のように分離する。

- Flutter は操作者向け表示と入力を担う。
- Shell Core は契約に従う権限状態を担う。
- Adapter は Runtime data を正規化するが Permission を付与しない。
- Rust broker は native helper と IPC の安全境界を担う。
- local validation は release blocker と証拠範囲を明示する。

## 外部公開用英語射影（非正本）

```text
Agent tools often blur UI convenience, diagnostics, runtime metadata, and execution authority. GUI-Shell separates them across Flutter, Shell Core contracts, adapters, a Rust broker, and explicit validation gates.
```
