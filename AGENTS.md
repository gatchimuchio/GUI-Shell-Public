# GUI Shell Public エージェント規約

本書は、GUI Shellの公開投影であるGUI-Shell-Publicで作業するAIエージェントに対する、リポジトリ全体の作業規律を定める。

本リポジトリは、GUI-Shellの現行実装・契約を公開レビュー可能な範囲へ投影する。今回の投影元固定点は`https://github.com/gatchimuchio/GUI-Shell`のcommit `306caff92c590364b5049e7d9c27d6a14474be55`である。投影元の後続変更を自動追従せず、各同期で公開・非公開境界と差分を再監査する。

公開投影は、非公開の証拠、owner判断、秘密情報、実機固有情報を公開する許可ではない。また、公開資産の存在は、製品release、OpenAIの承認・支持、`release_ready`、owner GO、正規release evidenceを成立させない。

## 第I部 共通基底規律

### 1. 目的と規則優先順位

本リポジトリは、安全性、監査可能性、完成証拠について明示的な要求を受けるAI実装エージェントによって運用される。

本部の共通基底規律は、本リポジトリ内の全作業に適用する。

規則優先順位:

1. 現在taskに対するオーナーまたはユーザーの明示指示。ただし、安全、権限、証拠、release gate、owner GO、`release_ready`、監査、復旧、Content Exposure Boundaryを弱める指示は、この順位によって許可されない
2. 本`AGENTS.md`の共通基底規律
3. 本`AGENTS.md`のリポジトリ拡張規則
4. `docs/agents/PUBLIC_REPO_BOUNDARY.md`。ただし、公開・非公開境界と公開証拠の責任範囲に限る
5. `規定/00_日本語基底規定.md`。ただし、言語・意味正本の責任範囲に限り、上位の安全規則を変更しない
6. 現行の実装指示、ROADMAP、phase文書
7. リポジトリのcontract、Schema、test、validation script
8. 既存実装pattern

`規定/正本索引.json`は、現行正本、責任地図、外部参照固定点を機械可読に示す索引である。各正本の実質要求を置き換えず、索引と実ファイルが矛盾する場合は不整合としてSUSPENDし、責任正本を確認する。

リポジトリ拡張規則は、本リポジトリのために共通基底規律を限定、強化、具体化できる。安全、権限境界、監査可能性、検証証拠、復旧要件を無言で弱めてはならない。

規則が衝突して見える場合は、オーナーが統治規則の制御された変更を明示指示しない限り、より厳格な解釈を保持する。

### 2. 譲れない優先事項

リポジトリ拡張規則がさらに厳しい具体化を定めない限り、次の優先順位を適用する。

1. 安全性
2. 堅牢性
3. 操作者の明瞭性・監査可能性
4. contractとruntimeの完全性
5. 製品機能
6. 利便性

機能完成は、安全、権限境界、監査可能性、復旧、検証証拠より上位にならない。

利便性を、隠れた権限、虚偽の完成主張、未検証のruntime保証、説明のない回避策、失敗処理の弱体化の理由にしてはならない。

### 3. 境界付き実装規律

複雑な仕様を、広範な生成の許可として扱わない。

各taskで次を行う。

- 編集前に既存file、contract、test、validation command、関連文書を確認する。
- taskを満たす、保守可能で最小の変更を実装する。
- リポジトリ固有の境界を保持する。
- 機会的refactorを行わない。
- 推測的機能を追加しない。
- 明示要求なしに、Permission、権限、runtime到達範囲、依存、toolchain、環境前提を広げない。
- 完了報告前に、taskが導入した残骸、古いTODO、放棄した部分経路、一時的な実装残余を除去する。

大量の生成構造は、完全性の証拠ではない。

#### 3.1 P-Series基準面凍結

GitHub Actions / CI workflowは、GUI-Shellの品質判定基準面から廃止済みである。`.github/workflows`配下にworkflow YAMLを置かない。品質判定の基準面は、owner / Codexが明示的に実行するlocal validation、smoke、release verification、Windows実機evidenceとする。基準面の変更は、追加を含め、active ROADMAPまたはphase instructionを経由する。検査の削除・弱体化にはowner承認を必須とする。

### 4. 完成証拠規則

完成の主張は証拠ではない。

作業blockを完了と報告する前に、次を特定する。

- 実装した挙動
- その挙動を実行するproduction、runtime、contract、validationの経路
- 実際に実行した正確なvalidation command
- 正確な結果
- 実行しなかったvalidation
- 残存するstub、mock、placeholder、TODO、未接続contract、環境制約、既知の制約

文書、Schemaの存在、mock成功、fixture成功、単体test成功だけを、production経路または製品挙動が完成した証拠として報告してはならない。

security-critical、authority-critical、audit-critical、recovery-critical、release-criticalな変更では、実際の統治経路が実行されたことを何の証拠が示すかを記載する。

リポジトリ状態を変更する実装taskでは、オーナーがlocal-only、audit-only、review-only、no-commit、no-pushに明示限定しない限り、完成にはリポジトリ状態の閉包も必要である。

リポジトリ状態を変更するtaskは、次を満たすまで完了ではない。

- file編集前に、リポジトリの2世代backup規約で現在push済みのリポジトリ状態を保存する。
- 意図した変更を実装し、taskの残骸を残さない。
- 必須validationを実行し、正確な結果を把握する。
- 意図した変更をすべて正確なmessageでcommitする。
- 指定remote branchへcommitをpushする。
- push先remote HEADが報告commitと一致することを確認する。
- リポジトリbackup規約に従って、復旧可能な2世代backupを確認する。
- push後にworking treeとbranchの整合を確認する。

リポジトリ状態を変更した作業の完了報告には、次を含める。

- 作業branch
- commit hash（コミット識別値）
- push結果
- remote HEAD確認
- backup世代のrefとhash
- rollback point（復旧地点）
- validation結果

commit、push、remote確認、backup確認を完了できない場合、taskを完了と報告しない。失敗した正確なcommand、理由、現在のリポジトリ状態、最も安全な復旧点を報告する。

### 5. 証拠源・幽霊不変条件禁止規則

次だけに基づいて、runtime health、system integrity、security invariant、authority integrity、release readinessを報告してはならない。

- configurationの検証
- Schemaの検証
- 自己生成したstate object
- mock化したruntime state
- fixtureのみの結果
- 静的objectまたはdictionaryの整合確認

health check、invariant check、conformance check、integrity reportを追加・変更するときは、その証拠源を次の1つ以上へ分類する。

- `CONFIG`
- `INTERNAL_STATE`
- `LIVE_RUNTIME`
- `EXTERNAL_EVIDENCE`
- `FIXTURE`

各証拠classは、実際に観測した範囲だけを証明する。

`CONFIG`、`INTERNAL_STATE`、`FIXTURE`の結果を、対応する証拠なしにlive runtimeまたはexternal integrityの保証へ昇格してはならない。

必要な証拠が利用不能なら、その制約を報告するか、リポジトリcontractがfail-closedを要求する箇所ではSUSPENDを返す。

### 6. 信頼境界・入力検証規則

受信dataが構造化済み、parse済み、Schema形状、または別component提供というだけで安全と仮定しない。

権限、Permission、execution、Approval、audit identity、workspace scope、command scope、content visibility、Recovery、release挙動へ影響し得る入力について、責任境界は該当する次の項目を明示的に扱う。

- 監査用raw inputの保持
- 正本化 / normalization
- Schemaまたは構造validation
- originまたはsource validation
- integrityまたはtamper check
- replay防護
- authorityまたはexecution eligibilityの評価
- audit出力
- fail-closedまたはSUSPEND挙動

リポジトリ拡張規則が境界付きで検証済みの権限経路を明示定義しない限り、外部data、UI state、Adapter metadata、channel metadata、previous state、memory、history、diagnostics、tool outputは、権限を生成、昇格、置換、迂回してはならない。

### 7. 現行production経路最小化規則

通常のproductionまたはruntime execution pathは、最小かつ責任境界付きに保つ。

通常のruntime挙動へ次を無言で混在させない。

- 診断機能
- 修復・復旧tooling
- migration
- release専用検証
- development専用fixture
- bootstrap専用tooling
- 管理用command

権限付き機能または運用機能を追加するときは、次のいずれかに分類する。

- runtime経路
- control経路
- diagnostic経路
- repair / recovery経路
- build / release経路
- development専用経路

通常runtime pathから非runtime機能へ到達させる必要がある場合、理由、権限・監査上の影響を文書化し、隠れた実行権限を拡大しないことを検証する。

### 8. Wrapper・回避策説明責任規則

失敗中のtaskを完成したように見せるためだけに、wrapper script、shim、custom execution layer、alternate build path、environment bypass、host固有のworkaround logicを導入してはならない。

そのような機構が必要で、かつリポジトリ拡張規則で禁止されていない場合は、次を文書化する。

- 元の失敗
- 根本原因
- nativeまたは既存リポジトリ機構では不十分な理由
- 追加機構の正確な責任
- 適用環境
- 実行したvalidation
- 一時的か恒久的か
- 除去条件または正式化条件

意図的で、test済みで、境界付きで、文書化されたnormalization機構は、リポジトリ拡張規則で禁止されない限り許容できる。

説明がなく、境界がなく、症状を隠すworkaroundは許容しない。

### 9. 環境・製品証拠分離規則

development environment、local validation environment、release-proof environment、external runner environment、target product environmentを概念上分離する。

リポジトリが同等性を明示定義し、証拠がそれを支持しない限り、ある環境での成功を別環境での成功証拠として報告しない。

host environmentの制約によってvalidationが阻害または歪曲される場合は、次を行う。

- 環境制約を特定する。
- 製品regressionと区別する。
- host failureを隠すためだけに製品architectureを変更しない。
- target environmentで未検証の範囲を報告する。

local developmentの利便性を、恒久的な製品architectureまたはrelease evidenceへ無言昇格してはならない。

### 10. Contract・runtime接続規則

Schema、interface、protocol object、Adapter Contract、audit contract、invariant contract、fixture、success profileは、存在するだけでは完成していない。

実挙動へ影響するcontractを追加・変更するときは、次を特定する。

- それを消費するproduction、runtime、validator、または統治されたexecution path
- それを実行するvalidationまたはconformance path
- reject、block、audit、SUSPENDすべきnegative caseまたはfailure case
- 意図的に未接続または延期する部分

意図した統治経路で実行されないcontractについて、挙動の完成を主張しない。

### 11. 監査結果・報告規則

完了した全作業報告で、次を区別する。

- 観測した実装事実
- 実際に実行したvalidation
- 未検証の主張
- 環境制約付きcheck
- 残存risk
- 意図的な延期範囲
- リポジトリ固有のrelease blocker

リポジトリがrelease gate分類を使用する場合、その分類を保持して適用する。

safety-critical、authority-critical、execution-criticalな要件を検証できない場合、成功を推論しない。SUSPEND、blocker、またはリポジトリ固有の同等状態を報告する。

#### 11.1 公開投影・証拠境界

本リポジトリへ含められるのは、公開レビュー可能性を確認した次の範囲に限る。

- ソースコード、検証tool、Schema、適合test
- 公開用の仕様、設計、利用者文書、エージェント運用文書
- 秘密情報と実機固有情報を除去し、由来と非正規性を明示した墨消し済み公開資産
- OpenAI / Codex応募・説明用資料。ただし支持、認定、採用、提携を主張しない

次を本リポジトリへ含めてはならない。

- rawまたは未墨消しのrelease evidence、log、transcript、screenshot、environment dump
- token、credential、secret、private key、実利用可能なsession情報
- 実利用者名、host名、端末名、home path、OneDrive pathその他の機械固有情報
- owner-private decision log、owner GOの非公開判断経路、非公開repository専用note
- `release_evidence/`または投影元の`openai_submission_assets/windows_proof_pack/`の内容

投影元から同期するときは、commitを固定し、共通実装、公開版固有資産、投影元だけの資産を区分する。ファイル名または追跡状態だけで公開可と推論しない。公開価値と非公開情報が混在する場合は原文をcopyせず、墨消しした別資産を作り、墨消し範囲と非正規性を記録する。

`public_assets/windows_proof_pack/`の墨消し済み資産は、公開review用の履歴保存物である。正規release evidence、現行実行結果、owner GO、release blocker解消、`release_ready`の根拠へ昇格させてはならない。過去logを現在の検査件数に合わせて書き換えず、合格を見せる目的で編集・再生成しない。

本リポジトリでは、`release_blockers.registry.json`の`release_ready`を`false`から変更してはならず、owner GOを記録してはならない。これらを変更する要求を受けても、公開repositoryからは実行せず、境界違反としてSUSPENDする。公開review用GitHub Releaseまたはtagも、完成製品releaseではない。

release blockerを通過したように見せるためにregistry、validator、evidenceを手編集してはならない。証拠を捏造・合成せず、CONFIG、INTERNAL_STATE、FIXTURE、mock、development evidenceを正規release evidenceへ昇格しない。公開宣伝資産をrelease evidenceとして使用しない。

公開変更前後に、少なくとも実利用者名、host名、home path、secret形式、raw evidence path、`release_ready=true`、owner GO記録、OpenAI支持表現を走査する。走査の無検出は公開安全性の完全証明ではないため、変更内容の意味reviewも行う。

公開版固有の通常編集範囲は、`README.md`、`docs/public/`、`docs/application/`、`docs/agents/`、`examples/`、権限判断を含まないUI表現、test、validation追加、墨消し済み資産の日本語索引である。

次は公開版でも制限変更範囲とし、必要性、影響する境界、実行した検証、release挙動の変化有無を報告する。

- `tooling/release_gate_check.py`
- `tooling/windows_release_evidence.py`
- `tooling/evidence_bundle.py`
- `tooling/release_runtime_assertions.py`
- `release_blockers.registry.json`
- `installer/windows/`
- `native/rust_helper/`
- `packages/shell_core/`
- `MANIFEST.sha256.json`

owner GO flow、`release_ready` flow、release evidence昇格、authority cutover、command dispatch有効化、credential処理、private evidence処理は、公開版から変更してはならない。墨消し済み保存証拠として`規定/日本語基底例外.json`にhash固定したfileは、明示された保存証拠更新と公開境界再監査なしに編集しない。

## 第II部 リポジトリ拡張規則

### 12. リポジトリの同一性と射程

本リポジトリを通常のapp scaffoldまたは単純な公開mirrorとして扱わない。

GUI-Shell-Publicは、GUI Shellの公開review用投影である。投影後の公開表現、公開・非公開境界、墨消し済み資産、公開用文書については本リポジトリ自身が責任を持つ。投影元GUI-Shellの実装を参照しても、非公開資産を無審査で継承しない。

GUI Shellは、汎用のRuntime Operation Shell制御planeである。

Flutter、Adapter、reference runtime、local cache、memory、installer、native helper、product UIは、下流または境界付きの実装面であり、権限を所有しない。

GUI Shellは、汎用のGUI Shell / Runtime Operation Shellを実装する。

BLUE-TANUKI専用GUIではない。

BLUE-TANUKIは最初のreference runtimeであり、Adapter Boundaryを介して接続しなければならない。

GUI Shellは、LLMが読むapplication responsibility substrateでもある。contract、安全境界、Adapter model、Approval model、Audit model、Recovery model、拡張規則は、LLM開発・統合エージェントが第一級の実装・統合面として読み、使用することを意図する。

LLMはGUI Shell contractの第一級の実装・統合consumerだが、権限源には決してならない。

#### LLM開発・統合エージェント規則

本リポジトリで作業するAIまたはLLM実装エージェントは、次を守る。

- 新機能、module、Adapter、tool、service、runtime integrationの必須接続面としてGUI Shell contractを扱う。
- 各変更がruntime path、control path、diagnostic path、repair / recovery path、build / release path、development-only pathのどれに属するかを特定する。
- 宣言済みcontractとconformance boundaryの外へ、Adapter、tool、module、external connection、privileged behaviorを追加しない。
- LLM output、memory、external metadata、generated configuration、GUI state、Adapter metadata、tool response、local cache、previous state、diagnosticsを通じて権限を与えない。
- 自身のsensitive actionを決して自己承認しない。
- 統合を容易にするために、Approval、Audit、Recovery、Authority Strip、Content Exposure、broker boundaryの挙動を弱めない。
- 拡張の完成を主張する前に、消費contract、必須conformance test、必須failure case、統治されたruntime pathを特定する。
- 提案統合に新contractが必要な場合、shortcutを無言で即興せず、その必要性を報告する。
- human ownerのApproval、Recovery判断、release claim、最終責任を明示状態に保つ。

### 13. 構造制約

- UI frameworkはFlutter
- Native helperはRust
- Contract: JSON Schema
- 実装言語・安全境界方針: `docs/LANGUAGE_POLICY.md`
- 日本語基底・意味正本: `規定/00_日本語基底規定.md`
- Reference runtime: BLUE-TANUKI。Adapter経由に限る
- Shell Coreはframework非依存を保つ
- Adapter Contractはruntime中立を保つ
- Flutterは交換可能なUI Layerを保つ
- Flutter / DartはUI Product Layerであり、Authority Boundaryになってはならない
- Rustは、権限に敏感なhelper、broker、IPC、Audit、signature、runtime command envelope作業のnative safety boundaryとする
- TypeScript / NodeをGUI-Shell core runtimeにしてはならない。external SDK、Adapter sample、protocol client sample、bridge exampleの範囲に限定する
- PythonをGUI-Shell runtime dependencyにしてはならない。dev-only tooling、Schema generation、migration helper、local validation、release evidence validation、一時validation scriptの範囲に限定する
- Authority-sensitiveなFlutter-Rust接続は、独立process IPCを優先する。FFI / direct bridgeは、authority、signature、approval token、external command dispatch、audit finalizationの境界外でのみ許可する
- オーナーが明示要求しない限り、GUI Shellの利便性のためにBLUE-TANUKI実装を変更してはならない

### 14. 境界の意味

#### Shell Core

Shell Coreは次を所有する。

- runtimeのregistry
- permissionのledger
- approvalのqueue
- auditのstore
- recoveryのcatalog
- updateのpolicy
- content exposureの強制
- adapter conformanceの強制

Shell Coreは次を行ってはならない。

- Flutterをimportする
- BLUE-TANUKI固有logicを含む
- Adapter metadataを信頼する
- memory、cache、previous stateだけを権限として使用する
- Permissionを無言で広げる

#### UI層（Layer）

Flutterは次を所有できる。

- rendering
- operatorの入力
- navigation
- localのUI state
- theme
- localization
- accessibility

Flutterは次を所有してはならない。

- authorityの判定
- Permissionの意味
- Approvalの意味
- Auditの意味
- Recoveryの分類
- content visibilityの規則
- runtime trustの規則

GUI表示成功は、runtime contract完成または権限安全性の証拠ではない。

#### Adapter層（Layer）

Adapterは次を行える。

- runtime stateを正規化する
- runtime healthを露出する
- runtime diagnosticsを露出する
- runtime eventをGUI Shell Schemaへ変換する

Adapterは次を行ってはならない。

- metadataを通じてPermissionを付与する
- runtimeが付与していないauthority contextを生成する
- 許可されたvisibilityを超えてraw payloadを表示する
- sealed、hidden、sacred、authority fieldを編集する
- Approval stateを迂回する
- Audit生成を迂回する

Adapterが露出するhealthまたはdiagnosticsは、それが表す証拠範囲を記載しなければならない。

#### Rust helper層

Rust helperは、境界付きのnative diagnosticsとoperationを実行できる。

Rust helperは次を行ってはならない。

- 隠れた権限経路になる
- filesystem、process、network、credential、IPC、updateへのaccessを無言で導入する
- Capability、Permission、Approval、Audit、Recoveryの対応なしにsensitive actionを実行する
- 構造化されていないsensitive dataを返す

### 15. リポジトリ固有の禁止pattern

次を行ってはならない。

- UI widgetへauthority decisionを置く
- Adapter metadataにPermissionを付与させる
- Adapter metadataにauthority contextを生成させる
- memory、local cache、previous stateだけに権限を付与させる
- `content_visibility=full`でないのに全文contentを表示する
- Approval payloadのauthority、sealed、hidden、sacred fieldを編集する
- 隠れたnetwork、filesystem、process、credential、IPC、update accessを導入する
- runtime Permissionを無言で広げる
- BLUE-TANUKI固有logicをShell Coreへ追加する
- core contractをFlutter固有code内へ置く
- validation evidenceなしにrelease readinessを主張する
- first-run成功を製品完成として扱う
- Product UI完成をcontract完成として扱う
- ROADMAP外の推測的機能を作る
- taskに必要でない広範なrefactorを行う

### 16. 必須監査対応

すべてのsensitive actionは、次へ対応づけなければならない。

- Capability
- Permission
- Approval state
- AuditEvent
- failure時のRecoveryAction

sensitive actionには次を含む。

- filesystemへのaccess
- processのexecution / control
- networkへのaccess
- credentialへのaccess
- IPC
- updateのverification
- runtime Adapterのaction
- Approval payloadのedit
- Auditのexport / inspection
- Recoveryのexecution
- installer stateのchange
- deviceのpairing

### 17. 内容露出規則

許可するcontent visibility値:

```text
none
hash_only
summary
redacted
full
```

規則:

- `none`: raw contentを表示しない
- `hash_only`: payload hashだけを表示する
- `summary`: 承認済みsummaryだけを表示する
- `redacted`: redacted projectionだけを表示する
- `full`: full contentを表示できる

全文payload表示を許可するのは`full`だけである。

### 18. Approval編集規則

Approval編集はfield scopeを限定しなければならない。

次の編集を許可しない。

- authorityのfield
- sealedのfield
- hiddenのfield
- sacred domainのfield
- runtimeのidentity
- permissionのidentity
- auditのidentity
- payload hashの直接編集

許可された編集の後は、次を行う。

- payloadを再hashする
- payloadを再validationする
- 必要に応じてApprovalをvalidation requiredとしてmarkする
- AuditEventをemitする

### 19. コミット前の必須検証

最低限、次を実行する。

```bash
python tooling/schema_check/check_schemas.py
python tooling/conformance_tests/run_conformance_skeleton.py
```

`python`が利用不能なら、次を実行する。

```bash
python3 tooling/schema_check/check_schemas.py
python3 tooling/conformance_tests/run_conformance_skeleton.py
```

Rustが導入済みでRust helperを変更した場合は、次を実行する。

```bash
cd native/rust_helper && cargo test
```

Flutterが導入済みでFlutter appを変更した場合は、次を実行する。

```bash
cd apps/desktop_flutter && flutter analyze
cd apps/mobile_flutter && flutter analyze
```

validationを実行できない場合は、理由を報告する。

実際に成功していないvalidationを、成功したと主張してはならない。

### 20. Git運用方針

本リポジトリは、direct-main owner workflowを使用する。

完了した各作業blockはcommitし、pushしなければならない。オーナーがcommitしない、またはpushしないと明示指示しない限り、完了したリポジトリ変更をlocal working treeだけに残さない。

標準workflow:

1. `main`で作業する
2. オーナーが明示要求しない限り、feature branchまたはpull requestを作らない
3. リポジトリ状態を変更するtaskでfile変更を開始する前に、`origin`をfetch / pruneし、`main`がcleanかつ`origin/main`と整合していることを確認する。不整合なら、編集前に解消するかblockerを報告する
4. localの2世代backup pairをrotateし、現在push済みの変更前状態を保存する
   - `codex/backup-main`が存在する場合、`codex/backup-main-prev`を`codex/backup-main`へforce-updateする
   - `codex/backup-main`を現在push済みの`main`へforce-updateする
5. 2つのbackup世代をremote branchではなく、PR-neutralなremote tagとしてpushする
   - `git push -f origin codex/backup-main-prev:refs/tags/codex/backup-main-prev codex/backup-main:refs/tags/codex/backup-main`
6. 境界付き実装と必須validationを行う
7. 完了した作業blockを`main`へ直接commitする
8. commit直後に`main`をpushする
9. `git ls-remote origin refs/heads/main`がlocal `HEAD`と一致することを確認する
10. remote backup tagの存在を確認してhashを記録する
    - `refs/tags/codex/backup-main`
    - `refs/tags/codex/backup-main-prev`
11. remoteに`codex/backup-main`または`codex/backup-main-prev` branchが存在する場合、`main`がcleanかつ整合した後にremote backup branchを削除する。GitHub上のbackup branchはpull request候補を生成するため、保持してはならない
12. `git status --short --branch`がcleanかつ`origin/main`と整合することを確認する
13. backup、commit、push、remote HEAD確認、remote backup確認のいずれかが失敗した場合、失敗した正確なcommandと理由を報告する

バックアップブランチ:

```text
codex/backup-main
codex/backup-main-prev
```

リモートバックアップref:

```text
refs/tags/codex/backup-main
refs/tags/codex/backup-main-prev
```

Backup branchはlocal recovery refに限る。GitHubがpull request候補として表示しないよう、remote backup世代はtagとしてpushする。backup refからpull requestをopen、request、mergeしてはならない。

オーナーがその緊急handoffを明示要求した場合に限り、backup branchをremote branchとしてpushする。この例外でbackup branchをGitHubへpushした場合、GitHubがpull request候補として表示し得ることを報告し、オーナーが不要とした時点でcleanupする。

追加のbackup世代を作らない。

次をstageしてはならない。

- secret
- localのruntime state
- Flutterのbuild output
- Rustのtarget output
- installerのartifact
- localのcache
- 明示要求されていないgenerated log

### 21. 完了報告とリリース関門分類

完了したすべての変更報告に、次を含める。

1. 概要
2. 変更file
3. risk分類
4. validation結果
5. release gate分類
6. 分類済みの残存risk
7. 作業branch
8. commit hash、または`not committed`
9. push結果、または`not pushed`
10. remote HEAD確認
11. backup世代refとhash
12. rollback point（復旧地点）
13. 公開・非公開境界scanの結果
14. evidence fileを作成、編集、copyしたか
15. `release_ready`とowner GOを変更したか

validation結果は、どのcommandが成功、失敗、未実行かを明記しなければならない。

本リポジトリで`release`は、完成した製品releaseを意味する。

final report、release report、validation report、remaining risk sectionでは、未完了項目をすべて次のいずれかに分類する。

- `release_blocker`
- `post_v1_scope`
- `known_limitation`

分類のない`remaining risks`、`still needed`、`not run`、`not implemented`、`not verified`、`TODO`、`skeleton only`、`future work`項目を許可しない。

`release_blocker`が1つでもあればreleaseを主張できない。

`post_v1_scope`項目は、v1.0 scope外である理由を明記しなければならない。

`known_limitation`は、release前に`README.md`、`CLAIM.md`、`RELEASE_CHECKLIST.md`のいずれかへ文書化しなければならない。

残存riskの形式:

```text
- item:
  classification: release_blocker | post_v1_scope | known_limitation
  reason:
  required_action:
  blocks_release: yes | no
```

### 22. 日本語基底・文書言語

GUI-Shell-Publicの基底言語、規定言語、内部意味正本、設計言語、監査言語、運用報告言語は日本語とする。詳細な対象、局所例外、意味監査、版差境界は`規定/00_日本語基底規定.md`を正本とし、現行正本、投影元固定点、責任地図は`規定/正本索引.json`で確認する。

```text
日本語で対象化・差異化・関係化
→ 日本語で定義・設計・監査
→ 日本語正本成立
→ 実務上やむを得ない外部接続だけ他言語へ局所射影
```

他言語は、正確性、互換性、実行可能性、検索再現性のため必要な接続面に限り、局所例外として使用できる。

局所例外には次を含む。

- conventionalなcode commentのうち、外部規約または既存project慣行が固定する表現
- Schemaのidentifier
- protocolのterm
- command nameとoption
- packageのmetadata
- programming languageの予約語・標準構文
- 外部API・library・frameworkの固定識別子
- URL、path、environment variable、commit hash、branch、tag、版識別子
- 外部toolが固定文言を要求する短いagent instruction text

例外表記の一般語義を内部意味へ逆流させず、GUI-Shell側の責任、境界、採否、失敗時挙動は日本語で定義する。

次の確立用語・固有名は原形を保持する。

- GUI Shell
- Runtime Operation Shell
- Shell Core
- Adapter Contract
- Authority Strip Conformance
- Content Exposure Boundary
- FrameworkRiskProfile
- Approval
- AuditEvent
- RecoveryAction
- BLUE-TANUKI
- Rust helper

既存文書に英語正本または日英並列正本が残ることを、本規定への適合とみなさない。移行は対象、責任、互換性、test、履歴を固定した境界付き変更として行い、機械翻訳や一括置換を完成証拠にしない。

### 23. 製品姿勢

GUI Shellは、操作を快適にしてよい。

GUI Shellは、権限を隠してはならない。

UIは操作面であり、system authorityではない。

Schemaとconformanceはcontract gateである。

快適性のために、Approval、Audit、visibility、Recovery要件を弱めない。

local owner operationを、堅牢性低下の理由にしない。

Product UI完成をcontract完成として扱わない。

## 第III部 現行作業・phase指針

### 24. 正本

第I部の優先順位modelの範囲内で、リポジトリ固有の現行指針を次の順に使用する。

1. `docs/specs/gui-shell-spec-v1.md`
2. `docs/agents/PUBLIC_REPO_BOUNDARY.md`
3. `docs/specs/adapter-conformance.md`、`docs/specs/content-exposure-policy.md`、`docs/specs/approval-visibility-boundary.md`、および関連する`docs/specs/` contract文書
4. `specs/*.schema.json`
5. `tooling/conformance_tests/`
6. phaseおよび技術選択の文脈を示す`ROADMAP.md`と`docs/standards/gui-shell-extended-standard.md`
7. 既存実装pattern

日本語の意味正本については`規定/00_日本語基底規定.md`、正本の所在と責任については`規定/正本索引.json`を併せて確認する。これらは、上記実装contractの安全要件を弱めない。

公開・非公開境界については`docs/agents/PUBLIC_REPO_BOUNDARY.md`を併せて確認する。同文書はraw evidenceの公開、owner GOの記録、`release_ready`の変更を許可しない。

衝突がある場合は、Shell Core authority boundary、Schema integrity、conformance coverage、operator safetyを保持する、より厳格な規則を選ぶ。

### 25. 必須作業順序

オーナーが明示的に別の指示をしない限り、次の順で作業する。

1. `docs/specs/gui-shell-spec-v1.md`を読む
2. 関連する`docs/specs/` contract文書を読む
3. `docs/standards/gui-shell-extended-standard.md`を読む
4. `specs/`配下の関連Schemaを読む
5. Shell Core / UI / Adapter / Rust helperの境界を保持する
6. contract変更時は実装前にSchemaを追加または更新する
7. Product UIより前にconformance testを追加または更新する
8. 最小かつ境界付きのcodeを実装する
9. validationを実行する
10. 正確な結果を報告する

後続phaseの安全性、検査可能性、validation可能性を低下させる方法でlocal taskを最適化してはならない。

### 26. 現行指示・ROADMAP参照

現行ROADMAPの正本は`ROADMAP.md`である。

正式実装仕様は`docs/specs/gui-shell-spec-v1.md`である。

拡張標準は`docs/standards/gui-shell-extended-standard.md`である。

日本語基底と意味正本は`規定/00_日本語基底規定.md`である。

現行正本、責任地図、外部参照固定点は`規定/正本索引.json`である。

実装言語と安全境界の方針は`docs/LANGUAGE_POLICY.md`である。

`specs/`配下のSchemaは、runtime、Adapter、Capability、Permission、Approval、Audit、Recovery、diagnostic、update、content exposure、framework risk、runtime manifest、Adapter manifest、agent runtimeの各surfaceに対するcontract gateを定義する。

### 27. 段階固有規則

`ROADMAP.md`の現行phaseとrelease boundaryに従う。

リポジトリ文書内のrelease gate blockerが解消され、strict validationが成功し、明示的なowner GOがあるまで、完成製品releaseを主張してはならない。

phase固有実装は、次を保持する。

- Schema-firstのcontract change
- conformance-firstのcoverage
- Shell CoreのFlutterからの独立
- BLUE-TANUKI reference runtimeのAdapter Boundary背後への隔離
- 境界付きRust helper authority
- Windows-first release evidenceとLinux development evidenceの分離
- macOS host validationが成立するまでのmacOS known limitation処理
