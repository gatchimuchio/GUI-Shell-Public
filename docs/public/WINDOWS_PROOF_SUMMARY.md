# Windows 証拠の公開概要

Windows-first desktop の実測結果から、公開可能な範囲だけを墨消し済み proof pack として収録している。

公開場所:

```text
public_assets/windows_proof_pack/
```

収録対象:

- 証拠索引
- validation log の保存抜粋
- build validation log の保存抜粋
- 成果物と証拠のハッシュ
- 公開可能な墨消し済み JSON copy

収録しない対象:

- raw `release_evidence/`
- local user path と hostname
- environment dump 全体
- 非公開会話記録
- owner 専用 log と判断記録

この proof pack は公開レビュー用の非正本 copy である。内容を canonical release evidence として再入力したり、owner GO や `release_ready=true` の根拠にしたりしてはならない。

- item: proof pack は owner GO ではない
  classification: release_blocker
  registry_id: owner_go
  reason: strict release は公開 copy と別に明示的な owner Approval を必要とする。
  required_action: 公開 proof asset と完成製品 release claim を分離する。
  blocks_release: yes
