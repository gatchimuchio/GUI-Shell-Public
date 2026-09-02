# Agent 作業ガイド

本書は、LLM を用いる実装 Agent が公開 GUI-Shell repository を変更するときの作業型を示す。規則の正本は `AGENTS.md`、日本語基底の正本は `規定/00_日本語基底規定.md`、公開境界は `docs/agents/PUBLIC_REPO_BOUNDARY.md` である。

## 基本作業流

1. 編集前に repository state、適用規則、関連契約、試験、公開境界を確認する。
2. task が runtime / control / diagnostic / repair-recovery / build-release / development-only のどの経路に属するかを特定する。
3. 権限、release、evidence、owner GO、credential を明示指示なしに変更しない。
4. 必要十分で review 可能な差分に限定する。
5. 目的に直接対応する local validation を実行する。
6. 観測事実、未検証範囲、release gate、公開境界を正確に報告する。

GitHub Actions / CI workflow は品質判定基準面ではない。local validation、smoke、release verification、Windows 実機 evidence を、それぞれの証拠範囲を区別して使用する。

## 文書変更

通常の対象:

- `README.md`
- `QUICKSTART.md`
- `docs/public/`
- `docs/application/`
- `docs/agents/`

注意対象:

- `release_blockers.registry.json`
- `tooling/release_gate_check.py`
- 正本リリース証拠の保存経路

必須事項:

- 日本語を意味正本とする。
- 国際公開に必要な英語は局所射影と明記し、日本語正本と独立した並列正本にしない。
- release に関係する未完了項目を `release_blocker`、`post_v1_scope`、`known_limitation` に分類する。
- public proof copy を canonical evidence と表現しない。

関連検証:

```bash
python3 tooling/manifest.py --check
python3 tooling/release_gate_check.py
python3 tooling/validate_all.py --python-only
```

## UI変更

通常の対象:

- `apps/desktop_flutter/` の非権限 UI surface
- `apps/desktop_flutter/test/`

変更してはならない意味:

- Shell Core の authority decision
- Rust broker の authority path
- コマンド送信の適格性
- Approval finalization

関連検証:

```bash
cd apps/desktop_flutter
flutter analyze
flutter test
dart format --output=none --set-exit-if-changed .
```

UI の描画成功は UI evidence に限る。Runtime authority や release evidence へ昇格しない。

## 検証・tooling変更

通常の対象:

- `tooling/`
- 検証説明文書

特に慎重に扱う対象:

- `tooling/release_gate_check.py`
- `tooling/windows_release_evidence.py`
- `tooling/evidence_bundle.py`
- `tooling/release_runtime_assertions.py`
- `MANIFEST.sha256.json`

関連検証:

```bash
python3 tooling/schema_check/check_schemas.py
python3 tooling/conformance_tests/run_conformance_skeleton.py
python3 tooling/manifest.py --check
python3 tooling/release_gate_check.py
python3 tooling/validate_all.py --python-only
```

検査を通すために validator や negative case を弱めない。

## Broker・security変更

対象:

- `native/rust_helper/`
- `docs/security/`
- `docs/architecture/`
- 対応する Schema と test

明示指示なしに変更しない対象:

- 権限境界の切替
- 実外部コマンドの送信
- 認証情報の取扱い
- Audit の確定
- リリース証拠への昇格

関連検証:

```bash
cd native/rust_helper
cargo fmt --check
cargo test
cd ../..
python3 tooling/conformance_tests/run_conformance_skeleton.py
python3 tooling/release_runtime_assertions.py --check
python3 tooling/validate_all.py --python-only
```

失敗は fail-closed に扱い、証拠を CONFIG / INTERNAL_STATE / LIVE_RUNTIME / EXTERNAL_EVIDENCE / FIXTURE の範囲に分類する。

## 公開 asset変更

対象:

- `public_assets/`
- 墨消し済み proof summary
- 画面画像とハッシュの索引

含めてはならないもの:

- 未加工の非公開証拠
- ローカル会話記録
- owner 専用 log
- machine 固有の environment dump
- 利用者名、ホスト名、非公開経路、秘密情報

既存の保存 log、hash、墨消し済み evidence copy は、後日の実装状態に合わせて書き換えない。必要な更新は由来を保持した新しい公開 copy として行う。

## Release evidence変更

release evidence の collector や説明を変更する場合でも、次を行ってはならない。

- `release_ready=true` を作る。
- owner GO を記録する。
- CONFIG / FIXTURE / public proof copy を installed-path evidence へ昇格する。
- blocker を validator の根拠なく消す。

必要な証拠を取得できない場合は、未検証または SUSPEND として報告する。

## 完了報告

少なくとも次を区別する。

1. 変更 file
2. 実行 command
3. 検証結果
4. リリース関門の状態
5. 残存 blocker
6. `release_ready` の変更有無
7. owner GO の変更有無
8. public/private boundary の問題
9. evidence file の作成・編集・copy の有無

実行していない検証を成功と報告せず、公開 package の成立を完成製品 release と同一視しない。
