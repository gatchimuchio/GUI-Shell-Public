# 変異検証

> 状態: 公開可能な conformance 概要
> 射程: 公開 review snapshot の説明

本書は、変異検証が何を確かめるかを公開範囲で記録する。壊れた変異 code を repository へ残す指示ではない。

## 対象面

- item: production Authority Strip の変異検出
  classification: required_for_v1
  status: `passed`
  evidence: conformance は production の権限除去挙動を読み、入力の authority key または authority metadata が残る変異を失敗として検出する。
  blocks_release: no

- item: production Approval 編集 guard の変異検出
  classification: required_for_v1
  status: `passed`
  evidence: conformance は production の `ApprovalQueue` を読み、authority、sealed、hidden、sacred、protected field を編集可能にする変異を失敗として検出する。
  blocks_release: no

- item: authority key 定義の重複
  classification: required_for_v1
  status: `resolved_for_current_public_scope`
  evidence: production code は authority key の扱いを一か所に集約し、Authority Strip の挙動を conformance で検査する。
  blocks_release: no

## 現在検証

```bash
python3 tooling/conformance_tests/run_conformance_skeleton.py
```

現在の件数と成否は実行出力を証拠とする。保存された過去件数や本書の記述を現在の PASS へ読み替えない。

変異検証は conformance 品質の補助証拠であり、installed-product behavior、canonical Windows evidence、完成製品 release readiness を証明しない。
