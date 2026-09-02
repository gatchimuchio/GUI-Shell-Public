# GUI Shell クイックスタート

GUI Shell は v1.0 の完成製品 release に向けた作業中である。クイックスタートは contract、conformance、owner-use 経路を確認するためのものであり、release readiness を主張しない。

## 前提条件

- POSIX 類似 shell
- `python` または `python3` として利用可能な Python
- 任意: `native/rust_helper` 用の Rust
- 任意: `apps/desktop_flutter` 用の Flutter

## 契約検証

推奨 command:

```bash
python tooling/schema_check/check_schemas.py
python tooling/conformance_tests/run_conformance_skeleton.py
```

`python` が `PATH` にない場合の fallback:

```bash
python3 tooling/schema_check/check_schemas.py
python3 tooling/conformance_tests/run_conformance_skeleton.py
```

成功時は両 command が終了code `0`を返す。schema数、example数、negative fixture数、conformance check数は実行出力を現在証拠とし、文書に固定した過去値で置き換えない。

## 段階Bの所有者起動

局所の所有者操作 snapshot を生成し、厳格 release 検証を実行せずに desktop shell を起動する。

```bash
bash scripts/launch_owner_desktop.sh
```

native Windows では次を実行する。

```powershell
powershell -ExecutionPolicy Bypass -File scripts\launch_owner_desktop.ps1
```

この経路は段階Bの所有者局所操作用である。`release_evidence/windows_installed_smoke.json` を作成せず、release readiness を主張せず、実測済み Windows インストール先 release 証拠を満たさない。

## 任意の Rust helper 検査

```bash
cd native/rust_helper
cargo test
```

Rust helper には、broker envelope 検証と拒否 audit 用の現行 Rust Security Broker 骨格がある。権限移行と IPC 統合の証拠が成立するまで、実際の外部 command dispatch は無効である。

## 任意の desktop Flutter 検査

```bash
cd apps/desktop_flutter
flutter analyze
```

Flutter は交換可能な UI 層である。UI widget は操作者入力の収集と状態の描画をできるが、権限、permission、approval、audit、recovery の意味を定義してはならない。

Mobile は `post_v1_scope` であり、この公開 package に Mobile app 実装を含めない。所有者が明示的に範囲を変更しない限り、v1.0 release gate、local validation の製品主張、広告する support 表層から除外する。

## 次の実装順序

1. `docs/specs/gui-shell-spec-v1.md` と関連 contract 文書を読む。
2. 必要な contract 変更を `specs/` の JSON Schema へ反映する。
3. conformance test と必須 failure case を追加または更新する。
4. 境界付きの実装を行う。
5. 主張文書を実際の検証証拠と整合させる。
