# Mobile 状態

所有者が明示的に範囲を変更しない限り、Mobile は v1.0 完成製品 release に含めない。

Mobile は `post_v1_scope` である。本 GUI-Shell-Public package は `apps/mobile_flutter/` を収録せず、Mobile implementation、build、実端末挙動を証明しない。

所有者が Mobile を release 範囲へ明示的に追加しない限り、v1.0 release gate、local validation の製品主張、広告する support 表層から除外する。

## 公開 package における状態

- item: 公開パッケージ内の Mobile アプリソース
  classification: post_v1_scope
  reason: 公開 package は Windows-first の desktop レビュー範囲であり、Mobile のソースを収録しない。
  required_action: Mobile scopeを明示採用する場合は、source、contract、権限境界、実端末evidence、packagingを別作業として追加・検証する。
  blocks_release: no

## v1後の範囲

- item: 実端末 pairing
  classification: post_v1_scope
  reason: Mobile 完全 release は v1.0 desktop 範囲外である。
  required_action: mobile release 段階で完了する。
  blocks_release: no

- item: push 通知
  classification: post_v1_scope
  reason: Mobile 完全 release は v1.0 desktop 範囲外である。
  required_action: mobile release 段階で完了する。
  blocks_release: no

- item: mobile 用 Shell Core IPC
  classification: post_v1_scope
  reason: Mobile 完全 release は v1.0 desktop 範囲外である。
  required_action: mobile release 段階で完了する。
  blocks_release: no

- item: 暗号学的な端末結合
  classification: post_v1_scope
  reason: Mobile 完全 release は v1.0 desktop 範囲外である。
  required_action: mobile release 段階で完了する。
  blocks_release: no

- item: mobile release 用 packaging
  classification: post_v1_scope
  reason: Mobile 完全 release は v1.0 desktop 範囲外である。
  required_action: mobile release 段階で完了する。
  blocks_release: no
