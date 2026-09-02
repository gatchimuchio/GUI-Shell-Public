# GUI Shell LLM 責任基盤 完成ロードマップ

> 状態: 公開用 roadmap 概要
> 射程: GUI-Shell-Public の review package

本書は、公開可能な責任境界と作業系列だけを記録する。非公開 planning note、raw evidence、canonical release evidence を複製せず、それらの代替にもならない。

GUI-Shell は汎用 Runtime Operation Shell であり、LLM が読む application responsibility substrate でもある。LLM 実装・統合 Agent は contract、Schema、conformance、release gate 文書を実装面として読めるが、権限源にはならない。

## 現在の公開位置

- item: 公開レビュー時点の写し
  classification: known_limitation
  reason: 本 repository は code、architecture、安全境界、応募資料の review package であり、完成製品 release ではない。
  required_action: public review の成立と完成製品 release readiness を分離する。
  blocks_release: no

- item: 完成製品 release
  classification: release_blocker
  reason: `release_blockers.registry.json` の blocker、strict validation、明示的な owner GO が未完了である。
  required_action: 統治された実測証拠と validation によって active blocker を閉じる。
  blocks_release: yes

- item: 公開用の証拠資料
  classification: known_limitation
  reason: 墨消し済み review copy であり、canonical release evidence ではない。
  required_action: 公開 copy を blocker 解消や owner GO の根拠として使用しない。
  blocks_release: no

## 作業系列

- 系列C: 公開 claim hygiene、日本語正本、文書整合、manifest、非主張境界
- 系列R: Rust Security Broker の本番収束、Windows installed-path evidence、strict release、owner GO
- 系列L: contract と conformance による、範囲を限定した LLM-readable extension の実証

系列Cは公開 review snapshotを成立させられるが、完成製品 releaseを成立させない。系列Rとowner GOが製品releaseを制御する。系列Lは限定された責任基盤claimだけを支え、installed-product proofを置き換えない。

## 現在検証

```bash
python3 tooling/schema_check/check_schemas.py
python3 tooling/conformance_tests/run_conformance_skeleton.py
python3 tooling/manifest.py --check
python3 tooling/release_gate_check.py
python3 tooling/evidence_bundle.py --check
python3 tooling/release_runtime_assertions.py --check
python3 tooling/validate_all.py --python-only
```

件数とPASS/FAILは各runの出力を現在証拠とする。GitHub Actions / CI workflowを品質判定基準面にせず、local validationとWindows実機evidenceの責任を分離する。

## 非主張

- 完成製品 release を主張しない。
- OpenAI endorsement、認証、提携、採択を主張しない。
- public proof asset を canonical release evidence としない。
- LLM-readable substrate の限定実証を、公開標準採用や広範なecosystem互換の証明にしない。
- Public package に含まれない Mobile app、raw evidence、内部計画の成立を主張しない。
