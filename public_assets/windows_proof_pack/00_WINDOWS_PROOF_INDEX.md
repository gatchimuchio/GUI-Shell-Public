# Windows 公開 Proof Pack

本 directory は、Windows installed-path evidence から作成した墨消し済み公開レビュー資料を収録する。

ここにある copy は canonical release evidence ではなく、この公開 repository の完成製品 release blocker を解消しない。保存 log は取得時点の抜粋であり、後日の conformance 件数や文言に合わせて書き換えない。

収録物:

- `hashes/artifact_hashes.txt`: release candidate artifact と evidence の hash
- `evidence_copies/EVIDENCE_HASHES.txt`: raw source と公開 copy の hash
- `evidence_copies/*.redacted.json`: 墨消し済み JSON evidence copy
- `logs/validation.log`: 保存済み validation evidence 抜粋
- `logs/build_validation.log`: build、Rust、Flutter、artifact hash の保存 log 抜粋
- `SCREENSHOT_INDEX.md`: screenshot の収録可否

収録しないもの:

- raw `release_evidence/`
- local user path、hostname、environment dump 全体
- 非公開会話記録
- owner 専用の判断記録

OpenAI による推薦、認証、提携、採択、および完成製品 release を主張しない。

- item: owner GO がない
  classification: release_blocker
  reason: Windows evidence と自動検査の成功は明示的な owner Approval を代替しない。
  required_action: strict evidence review 後に限り owner GO を記録する。
  blocks_release: yes
