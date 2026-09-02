# アーキテクチャ概要

GUI-Shell は、表示、契約評価、native helper の挙動、Runtime 接続を分離する。

```text
操作者
  -> Flutter desktop UI
  -> Shell Core の契約と方針に従う状態
  -> Rust broker / helper
  -> Runtime Adapter または native operation
```

境界は次のとおりである。

- Flutter は描画、画面遷移、操作者入力だけを所有する。
- Shell Core は Runtime に依存しない権限、Approval、Audit、Recovery、内容可視性の意味を所有する。
- Adapter は Runtime data を正規化するが、権限を付与しない。
- Rust helper は native broker、IPC、診断、Audit anchor 支援を担う。
- JSON Schema と conformance test は公開契約の gate を定める。

BLUE-TANUKI は参照 Adapter の接続対象に限る。Shell Core は Runtime 中立を保つ。

権限に敏感な経路では Rust Security Broker の独立した production IPC を本番境界とする。公開 package でも、`no-python-runtime` と `no-ffi-authority` を release assertion として保持する。

ただし、これらの source、contract、local test が存在するだけでは installed product の成立を証明しない。Windows installed-path の Broker evidence、no-Python runtime evidence、FFI authority bypass がないことの release evidence は引き続き `release_blocker` である。

アーキテクチャ文書や公開証拠は、release readiness や owner GO を生成しない。
