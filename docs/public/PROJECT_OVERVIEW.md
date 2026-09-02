# プロジェクト概要

GUI-Shell は、Windows を第一対象とする desktop Runtime Operation Shell である。local Runtime と Agent 向け tool の操作面を提供しながら、権限、Approval、Audit、Recovery の境界を明示状態に保つ。

公開 repository は、次の review 可能な実装面を含む。

- Flutter によるデスクトップ操作画面
- Rust による権限ブローカーと補助機能
- Shell Core と Adapter package
- JSON Schema contract
- conformance と local validation tooling
- Windows の準備・証拠収集スクリプト
- 墨消し済み Windows proof asset

Rust Security Broker の production IPC を権限に敏感な本番境界とし、`no-python-runtime` と `no-ffi-authority` を release assertion として保持する。ただし、公開 source と local validation の存在だけでは Windows installed product proof にならず、対応する installed-path evidence は未解消の release blocker である。

公開 package は、元の local / private evidence 全体を複製しない。`public_assets/windows_proof_pack/` は公開レビュー用の写しであり、canonical release evidence ではない。

現在の release 境界:

- item: owner GO がない
  classification: release_blocker
  registry_id: owner_go
  reason: 公開資料は最終 owner Approval を代替しない。
  required_action: 実測証拠と厳格検証の完了後に限り owner GO を記録する。
  blocks_release: yes

OpenAI による推薦、認証、提携、採択、および完成製品 release は主張しない。
