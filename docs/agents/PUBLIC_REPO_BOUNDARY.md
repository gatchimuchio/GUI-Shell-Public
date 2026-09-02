# 公開 Repository 境界

本書は、GUI-Shell-Public に含めてよい対象と、公開してはならない対象を定める。

## 公開してよい対象

- ソースコード
- ローカル検証用ツール
- Schema と conformance test
- 公開用の仕様、設計、操作文書
- 墨消し済み Windows proof asset
- OpenAI / Codex 応募・外部レビュー資料
- private environment data を含まない example
- 日本語正本から作成し、非正本であることを明示した外部言語射影

これらは、アーキテクチャ、権限境界、検証範囲、Windows-first scope、Agent が読む contract を公開レビューできるようにする。

## 公開してはならない対象

- 未加工の非公開証拠
- ローカル端末の経路、利用者名、ホスト名
- 秘密情報、認証情報、トークン
- owner 専用 log と非公開判断記録
- 墨消し前 transcript
- private repository 専用 note
- workstation 固有情報を含む environment dump
- `release_evidence/` の raw content

公開価値と private data が混在する場合は、由来を記録した墨消し済み copy を作成し、墨消し範囲と証拠上の限界を明示する。

## 証拠境界

`public_assets/windows_proof_pack/` は公開レビュー用の非正本 copy である。

Canonical release evidence は release tooling が生成・検証する。公開 asset を手作業で PASS に見せたり、raw evidence として再入力したり、release blocker を閉じるために使ったりしてはならない。

`release_evidence/` は公開 source package に含めない。

Owner GO は公開 Agent が編集する状態ではない。owner GO がないことは報告できるが、記録してはならない。

## Release 境界

公開 repository が述べられること:

- Windows-first desktop evidence に由来する墨消し済み review copy がある。
- 列挙した local validation が、実際に実行した範囲で通過した。
- strict release は canonical evidence と owner GO により引き続き gate される。

公開 repository が述べてはならないこと:

- 完成製品の release readiness
- OpenAI の推薦、認証、提携、採択
- 別途検証していない Mobile または macOS v1.0 support
- external / signed evidence がない状態での administrator / root 改ざん耐性

## 言語境界

規定、仕様、設計、監査、運用、release claim の意味正本は日本語とする。

英語その他の表層は、国際公開、応募、外部規格との接続に必要な範囲へ限定する。その表層は日本語正本を置き換えず、権限、release readiness、evidence class を独自に変更しない。

## 公開前検査

次を確認する。

- `release_ready=true` の主張がない。
- owner GO が記録されていない。
- OpenAI endorsement の主張がない。
- Windows user-home path、username、hostname がない。
- API key、token、secret-like value がない。
- raw `release_evidence/` がない。
- local build output と cache がない。
- 公開 proof copy が canonical evidence と記述されていない。
- 外部言語射影が日本語正本へ接続されている。
