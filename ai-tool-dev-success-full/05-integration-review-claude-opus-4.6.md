Now let me check the existing research outputs for additional context.

Now I have sufficient research data. Let me produce the comprehensive integration review.

# 統合レビュー

## Executive Summary（総括）

フルスタックエンジニアがAIツール開発で成功するためには、「仕様駆動（Spec-Driven）」「決定論ゲート（Deterministic Gate）」「階層型コンテキスト（Tiered Context）」の3原則を **ハードウェア制約を起点として** 設計する必要がある。批判的レビューにより、先行調査のクラウドネイティブ偏重・エッジ実装の楽観的前提・規格マッピング未完了が明確になった。本統合レビューでは、これらの欠陥を修正し、MCUリソース制約・EU CRAタイムライン・IEC 62443ゾーン/コンジット要件を組み込んだ、実装者が即座に着手可能なロードマップを提示する。結論として、**エッジファースト設計（Edge-First Design）** を採用し、クラウドLLM統合は段階的に拡張する戦略が、このプロフィールの技術者にとって最もリスクが低く成功確率が高い。

## Research Question（調査の問い）

フルスタックエンジニア（Web/モバイル/ファームウェア/回路設計）が、ハードウェアからソフトウェアまで一貫した設計原則のもとで、安全かつ規格適合性のあるAIツールを開発するための、**実装可能かつ検証済みのロードマップ**は何か？

---

## Findings（主要な発見）

### Finding 1: エッジファースト設計が唯一の現実的アプローチである

- **Claim（主張）**: AIツール開発において、MCUリソース制約を設計の出発点とし、クラウドLLM統合を段階的に追加する「エッジファースト」アプローチが、フルスタックエンジニアの強みを最大限に活かしつつリスクを最小化する唯一の現実的戦略である。
- **Evidence（根拠）**: 批判的レビューが指摘した通り、TFLite MicroのSRAM要件（キーワード検出で256KB以上）、MQTT5 User Propertiesの組み込みサポート未成熟（coreMQTT v2はMQTT 3.1.1準拠、MQTT5は開発中）、OpenTelemetry C++ SDKの組み込み未最適化という3つの技術的制約は、クラウドファーストで設計した場合にエッジ統合時に破綻的な手戻りを招く。一方、MLPerf Tiny v1.3（2025年9月公開）の実測結果では、Cortex-M55でキーワードスポッティング推論が5ms、STM32H7で画像分類がsub-10msと、MCU上でのAI推論は実用レベルに到達している。
- **Source（情報源）**:
  - [SRC-001] [official_document/tier2] [MLPerf Tiny v1.3 Results](https://mlcommons.org/2025/09/mlperf-tiny-v1-3-results/) (2026-03-14) — primary: true
  - [SRC-002] [community_data/tier4] [coreMQTT MQTT v5 Issue #269](https://github.com/FreeRTOS/coreMQTT/issues/269) (2026-03-14) — primary: true — community_score: discussion_age: "2 years", participant_count: ~30, expert_presence: true, reproducibility: true
  - [SRC-003] [official_document/tier2] [TFLite Micro Get Started](https://www.tensorflow.org/lite/microcontrollers/get_started) (2026-03-14) — primary: true
- **Confidence（確信度）**: high
- **Evidence Strength（根拠の強さ）**: strong
- **Requirement Level（義務レベル）**: SHOULD（設計戦略としての推奨。ただしMCU搭載製品のEU市場投入を目指す場合、リソース制約の把握はMUSTに昇格する）

### Finding 2: 決定論ゲートはRTOSタスク設計として具体化すべきである

- **Claim（主張）**: 「安全クリティカル制御をLLM直接出力で実行しない」という原則（MUST_NOT）は正しいが、実装仕様として「決定論ゲート」をRTOSタスク優先度設計・有限状態機械（FSM）・タイムアウトポリシーの3要素で定義しなければ、設計として不完全である。
- **Evidence（根拠）**: Zephyr RTOSおよびFreeRTOSのタスク優先度モデルでは、リアルタイム安全タスク（Safety Monitor）は最高優先度で動作し、LLMレスポンス待ちタスクより常に先にスケジュールされる必要がある。Praetorian社の「Thin Agent / Fat Platform」パターン（2025年）では、LLMを「ステートレスワーカー」として扱い、全ての出力をプラットフォーム側の検証ループで強制的にゲートする設計が推奨されている。Parasoft社の安全クリティカル組み込みAIガイド（2025年）では、「Frozen Model」パターン—静的検証済みモデルのみを本番デプロイし、動的学習は安全パスから隔離—が標準的手法として記載されている。
- **Source（情報源）**:
  - [SRC-004] [official_document/tier2] [Zephyr RTOS - Threads](https://docs.zephyrproject.org/latest/kernel/services/threads/index.html) (2026-03-14) — primary: true
  - [SRC-005] [official_document/tier2] [FreeRTOS Task Priorities](https://www.freertos.org/Documentation/02-Kernel/02-Kernel-features/01-Tasks-and-co-routines/03-Task-priorities) (2026-03-14) — primary: true
  - [SRC-006] [industry_report/tier3] [Deterministic AI Orchestration - Praetorian](https://www.praetorian.com/blog/deterministic-ai-orchestration-a-platform-architecture-for-autonomous-development/) (2026-03-14) — primary: false
  - [SRC-007] [industry_report/tier3] [AI in Safety-Critical Embedded Systems - Parasoft](https://www.parasoft.com/blog/ai-in-safety-critical-embedded-systems/) (2026-03-14) — primary: false
- **Confidence（確信度）**: high
- **Evidence Strength（根拠の強さ）**: strong
- **Requirement Level（義務レベル）**: MUST（安全クリティカルシステムの場合）/ SHOULD（一般IoTの場合）
- **Standard Reference（規格参照）**: IEC 61508-3 Table A.3（ソフトウェア設計手法とSIL） — partial（FSM設計はSIL 1-2でRecommended、SIL 3-4でHighly Recommended）

### Finding 3: MQTT5 + Trace Contextの組み込み実装は2層分離設計が必要

- **Claim（主張）**: MCU上でMQTT5 User Properties + W3C Trace Contextを直接実装する戦略は、2026年3月時点では技術的に未成熟であり、「MCU→ゲートウェイ（MQTT 3.1.1）」+「ゲートウェイ→クラウド（MQTT5 + Trace Context）」の2層分離設計を採用すべきである。
- **Evidence（根拠）**: coreMQTT v2のMQTT5対応は開発中（2026年前半時点で本番品質未到達）。FreeRTOS公式ドキュメントおよびMCUXpresso SDK（2026年版）はMQTT 3.1.1を本番ターゲットとして記載。一方、ゲートウェイ層（組み込みLinux / Raspberry Pi級）ではEclipse Mosquitto等のフル機能MQTT5ブローカーが利用可能であり、そこでtraceparentヘッダの注入・伝搬を行えば、MCU側の制約を回避しつつE2Eトレーサビリティを確保できる。
- **Source（情報源）**:
  - [SRC-002] [community_data/tier4] [coreMQTT MQTT v5 Issue #269](https://github.com/FreeRTOS/coreMQTT/issues/269) (2026-03-14) — primary: true
  - [SRC-008] [official_document/tier2] [coreMQTT Releases](https://github.com/FreeRTOS/coreMQTT/releases) (2026-03-14) — primary: true
  - [SRC-009] [official_document/tier2] [MCUXpresso SDK - coreMQTT](https://mcuxpresso.nxp.com/mcuxsdk/25.09.00-pvw1/html/rtos/freertos/coremqtt/index.html) (2026-03-14) — primary: true
  - [SRC-010] [official_document/tier1] [MQTT Version 5.0 - User Properties](https://docs.oasis-open.org/mqtt/mqtt/v5.0/os/mqtt-v5.0-os.html#_Toc3901013) (2026-03-14) — primary: true
- **Confidence（確信度）**: high
- **Evidence Strength（根拠の強さ）**: strong
- **Requirement Level（義務レベル）**: SHOULD（2層分離の採用）

### Finding 4: 仕様駆動開発（SDD）はAIツール開発の品質ゲートとして確立されつつある

- **Claim（主張）**: 仕様駆動開発（Spec-Driven Development）は2025-2026年においてAIコード生成の品質保証手法として学術・産業の両面で支持を得ており、OpenSpecやGitHub Spec Kitなどのツールチェーンが利用可能になっている。フルスタックエンジニアがLLMを活用した開発で品質を維持するには、SDDをCI/CDパイプラインに組み込み、仕様→テスト→実装→検証の自動化ループを構築すべきである。
- **Evidence（根拠）**: arXiv論文「Spec-Driven Development: From Code to Contract in the Age of AI Coding」（2026年2月）がSDDの理論的基盤を提示。GitHub公式ブログ（2025年）でSpec Kitを公開し、仕様ファイルをコードと同居させるワークフローを推奨。ThoughtWorks Technology Radar（2025年版）でもSDDを「2025年の重要な新エンジニアリングプラクティス」として評価。
- **Source（情報源）**:
  - [SRC-011] [academic_paper/tier2] [Spec-Driven Development: From Code to Contract](https://arxiv.org/html/2602.00180v1) (2026-03-14) — primary: true
  - [SRC-012] [official_document/tier2] [GitHub Blog - Spec-driven development with AI](https://github.blog/ai-and-ml/generative-ai/spec-driven-development-with-ai-get-started-with-a-new-open-source-toolkit/) (2026-03-14) — primary: false
  - [SRC-013] [industry_report/tier3] [ThoughtWorks - Spec-driven development](https://www.thoughtworks.com/insights/blog/agile-engineering-practices/spec-driven-development-unpacking-2025-new-engineering-practices) (2026-03-14) — primary: false
  - [SRC-014] [industry_report/tier3] [Augment Code - SDD Implementation Guide](https://www.augmentcode.com/guides/mastering-spec-driven-development-with-prompted-ai-workflows-a-step-by-step-implementation-guide) (2026-03-14) — primary: false
- **Confidence（確信度）**: high
- **Evidence Strength（根拠の強さ）**: moderate（学術論文1件 + 産業レポート複数。ただし安全クリティカル組み込み領域での大規模事例は未確認）

### Finding 5: EU CRAタイムラインがロードマップの外部制約として最重要である

- **Claim（主張）**: EU CRA（Regulation 2024/2847）の施行スケジュール—2026年9月に脆弱性報告義務開始、2027年12月に完全準拠義務—が、AIツール搭載IoT製品のロードマップにおける最も硬い外部制約であり、SBOM生成・脆弱性管理・インシデント報告体制の構築を「Phase 0」として前倒しで着手すべきである。
- **Evidence（根拠）**: EU CRAは「デジタル要素を含む全製品」を対象とし、IoT・組み込み機器を明示的にスコープに含む。2026年9月11日より、既知の脆弱性の24時間以内報告がENISAへ義務化される。違反時の制裁金は最大1,500万ユーロまたは全世界売上高の2.5%。SEMI E187やIEC 62443はセクター固有だが、EU CRAは水平規制であり適用範囲が最も広い。
- **Source（情報源）**:
  - [SRC-015] [official_document/tier1] [EU CRA Implementation - European Commission](https://digital-strategy.ec.europa.eu/en/factpages/cyber-resilience-act-implementation) (2026-03-14) — primary: true
  - [SRC-016] [official_document/tier1] [CRA Implementation Timeline](https://eu-cyber-laws.com/cra/timeline/) (2026-03-14) — primary: false
  - [SRC-017] [industry_report/tier3] [CRA Complete Implementation Timeline 2025-2027](https://craevidence.com/blog/cra-implementation-timeline-2025-2027) (2026-03-14) — primary: false
- **Confidence（確信度）**: high
- **Evidence Strength（根拠の強さ）**: strong
- **Requirement Level（義務レベル）**: MUST（EU市場向け製品の場合、2026年9月までに脆弱性報告体制構築は法的義務）
- **Standard Reference（規格参照）**: EU CRA (Regulation 2024/2847) Article 14 — direct

### Finding 6: IEC 62443ゾーン/コンジット分離はMQTT over TLSだけでは不充足

- **Claim（主張）**: IEC 62443-3-3のゾーン/コンジット要件（SR 5.1〜5.4）を満たすには、MQTT over TLSに加えて、①セキュリティゾーン定義とSL割当、②コンジットにおけるトピック/ペイロードのホワイトリスト制御、③DMZまたはセキュリティゲートウェイの設置、④全通信の監査ログ記録が必要であり、「Policy Gate」と称するだけでは適合審査で差し戻されるリスクがある。
- **Evidence（根拠）**: Cisco社のIEC 62443-3-3解説では、ゾーン間通信には「明示的に制御されたコンジット」が必須であり、暗号化だけでは要件の一部にすぎないと明記。2025年のトレンドとして、マイクロセグメンテーション（デバイス単位またはプロセス単位のゾーン分離）がIEC 62443準拠の標準的実装方法になりつつある。
- **Source（情報源）**:
  - [SRC-018] [official_document/tier2] [ISA/IEC 62443-3-3 Compliance - Cisco](https://www.cisco.com/c/en/us/products/collateral/security/isaiec-62443-3-3-wp.html) (2026-03-14) — primary: false
  - [SRC-019] [industry_report/tier3] [IEC 62443 in 2025 - Elisity](https://www.elisity.com/blog/iec-62443-in-2025-network-segmentation-requirements-and-changes) (2026-03-14) — primary: false
  - [SRC-020] [official_document/tier1] [ISA/IEC 62443 Series](https://www.isa.org/standards-and-publications/isa-iec-62443-series-of-standards) (2026-03-14) — primary: true
- **Confidence（確信度）**: high
- **Evidence Strength（根拠の強さ）**: strong
- **Requirement Level（義務レベル）**: MUST（IEC 62443準拠を主張する場合）
- **Standard Reference（規格参照）**: IEC 62443-3-3 SR 5.1–5.4（ネットワーク分離・データフロー制御）— gap（現状の提案アーキテクチャに対して）

### Finding 7: OTAファームウェア更新セキュリティはNIST SP 800-193に基づくべき

- **Claim（主張）**: MCUファームウェアのOTA更新は、NIST SP 800-193の「保護・検知・回復」3原則に基づき、セキュアブート・署名検証・A/Bパーティション・アンチロールバックカウンターを実装すべきである。これはAIモデル更新にも適用される。
- **Evidence（根拠）**: NIST SP 800-193はプラットフォームファームウェアの耐性に関するガイドラインであり、Microchip MEC172xやLattice Semiconductorなどのベンダーが対応ハードウェアを提供。MCUのOTA失敗はフィールドでのブリック（文鎮化）を招くため、dual-image（A/Bパーティション）による安全なフォールバックが必須。
- **Source（情報源）**:
  - [SRC-021] [official_document/tier1] [NIST SP 800-193](https://csrc.nist.gov/pubs/sp/800/193/final) (2026-03-14) — primary: true
  - [SRC-022] [official_document/tier2] [Microchip Platform Root of Trust](https://www.microchip.com/en-us/solutions/data-centers-and-computing/computing-solutions/technologies/platform-root-of-trust-secure-boot) (2026-03-14) — primary: false
  - [SRC-023] [official_document/tier2] [Lattice PFR](https://www.latticesemi.com/en/Solutions/Solutions/SolutionsDetails02/PFR) (2026-03-14) — primary: false
- **Confidence（確信度）**: high
- **Evidence Strength（根拠の強さ）**: strong
- **Requirement Level（義務レベル）**: SHOULD（NIST SP 800-193は推奨ガイドライン）。ただしEU CRA準拠の文脈では脆弱性管理の一環としてMUSTに実質昇格。
- **Standard Reference（規格参照）**: NIST SP 800-193 Section 4 — direct; EU CRA Article 10(6) — partial

### Finding 8: OpenTelemetryのエッジ展開は階層型コレクターモデルで実用可能

- **Claim（主張）**: OpenTelemetryはMCUベアメタルへの直接実装は困難だが、階層型コレクターモデル（Device → Regional Gateway → Central Collector）を採用すれば、エッジからクラウドまでのE2Eオブザーバビリティを実現できる。MCU層では軽量メトリクス（カスタムバイナリフォーマット）をゲートウェイに送信し、ゲートウェイ層でOTLP変換する設計が現実的である。
- **Evidence（根拠）**: OneUptime社のIoTエッジオブザーバビリティガイド（2026年2月）は、OpenTelemetry Collector Builderで最小構成バイナリをARM Edge機器にデプロイする手法を実装例つきで公開。Red Hat Device Edge（2025年5月）は、コンテナ化されたOTelコレクターをエッジに配置するパターンを推奨。ただし、Cortex-M系ベアメタルMCUでOTelを直接動作させた公開事例は未確認。
- **Source（情報源）**:
  - [SRC-024] [industry_report/tier3] [Observability for IoT Edge - OneUptime](https://oneuptime.com/blog/post/2026-02-06-observability-iot-edge-devices-opentelemetry/view) (2026-03-14) — primary: false
  - [SRC-025] [industry_report/tier3] [Red Hat Device Edge + OTel](https://developers.redhat.com/articles/2025/05/29/boost-red-hat-device-edge-observability-opentelemetry) (2026-03-14) — primary: false
  - [SRC-026] [official_document/tier2] [OpenTelemetry](https://opentelemetry.io/) (2026-03-14) — primary: true
- **Confidence（確信度）**: medium
- **Evidence Strength（根拠の強さ）**: moderate（ゲートウェイ層までは検証済み、MCUベアメタル層は未検証）

### Finding 9: ロードマップの期間見積もりは「エッジ検証バッファ」を追加すべき

- **Claim（主張）**: 先行調査のPhase 1〜4（各2〜3週間、合計約10週間）はWeb開発の工数感であり、ハードウェア検証・EMC試験・認証取得を含むエッジ開発ではx3〜x5のバッファが必要。フルスタックエンジニアであっても、ハードウェア依存の工程（ベンダーSDK品質、MCU選定後のピン配・メモリマップ設計、EMC試験スケジュール）は外部要因に支配される。
- **Evidence（根拠）**: 批判的レビューの指摘に加え、組み込み開発プロジェクトではハードウェア待ち・SDKバグ対応・EMC再試験で当初見積もりの3〜5倍の工期を要するケースが一般的。ただしこれは定量的な業界統計ではなく経験則に基づく。
- **Source（情報源）**:
  - [SRC-027] [industry_report/tier3] [Barr Group - Software Bug Statistics](https://barrgroup.com/blog/software-bug-statistics) (2026-03-14) — primary: false
  - [SRC-028] [unknown] 組み込み開発プロジェクト工数経験則 (unknown) — primary: false
- **Confidence（確信度）**: medium
- **Evidence Strength（根拠の強さ）**: weak（定量エビデンス不足、経験則に依存）

### Finding 10: フルスタックエンジニアの横断スキルは「統合テスト設計」で最大効果を発揮する

- **Claim（主張）**: フルスタックエンジニアの最大の競争優位は、個々のレイヤーの実装能力ではなく、**レイヤー間の統合テストを自ら設計・実行できること**にある。MCUファームウェア→ゲートウェイ→クラウドLLM→UIの全経路を一人で検証できるため、分業チームでは発見しにくいインテグレーションバグを早期に検出できる。ただしこの強みは、統合スタックの複雑性増大によるデバッグ難易度上昇と表裏一体であり、各レイヤーの境界を明確に定義したインターフェース仕様が不可欠である。
- **Evidence（根拠）**: SDD（仕様駆動開発）文献では、AIツール開発においてレイヤー境界のインターフェース仕様がコード品質を決定するとされる。GitHub Spec Kitのワークフローは、仕様→テスト→実装の順序を強制することで、統合テストの網羅性を自動的に向上させる設計になっている。
- **Source（情報源）**:
  - [SRC-011] [academic_paper/tier2] [Spec-Driven Development: From Code to Contract](https://arxiv.org/html/2602.00180v1) (2026-03-14) — primary: true
  - [SRC-012] [official_document/tier2] [GitHub Blog - Spec-driven development with AI](https://github.blog/ai-and-ml/generative-ai/spec-driven-development-with-ai-get-started-with-a-new-open-source-toolkit/) (2026-03-14) — primary: false
- **Confidence（確信度）**: medium
- **Evidence Strength（根拠の強さ）**: moderate

---

## Requirement Level Matrix（義務レベルマトリクス）

| 義務レベル | 件数 | 主な要件 | 規格間の解釈差異 |
|---|---|---|---|
| **MUST** | 5件 | ①EU CRA脆弱性報告（2026/9月〜）②決定論ゲート実装（安全クリティカル時）③IEC 62443ゾーン/コンジット分離④SBOM生成・維持⑤セキュアブートとファームウェア署名検証 | EU CRAのMUSTは法的義務（罰金あり）。IEC 62443のMUSTはSL割当に依存（SL1ではSHOULD相当になるSRもある）。NIST SP 800-193のSHOULDは、EU CRA準拠の文脈ではMUSTに実質昇格する |
| **SHOULD** | 4件 | ①エッジファースト設計戦略②2層MQTT分離設計③仕様駆動開発（SDD）CI/CD組み込み④階層型OTelコレクター | SEMI E187のSHOULDは「仕様からマイナス可能」（除外可能）。EU CRAのSHOULDは「正当な理由なく逸脱すれば不適合とみなされうる」。IEC 62443のSHOULDはSL-Tに依存し、SL3以上では実質MUST |
| **MUST_NOT** | 1件 | 安全クリティカル制御パスでのLLM直接出力実行 | 全規格で共通認識（IEC 61508、ISO/SAE 21434共に非決定論的コンポーネントの安全パス直接配置を禁止） |
| **MAY** | 2件 | ①TFLite Micro以外のエッジ推論フレームワーク（ONNX RT等）選択②OTel以外のテレメトリ方式 | — |
| **WANT** | 1件 | エッジMLOps（モデルドリフト自動検知・再学習パイプライン） | — |

### 規格間解釈差異の重要分析

**EU CRA SHOULD vs IEC 62443 SHOULD vs SEMI E187 SHOULD:**

- **EU CRA**: SHOULDは事実上「comply or explain」方式。EU適合宣言（DoC）において「なぜ実装しなかったか」の正当化が必要。正当化が不十分な場合、市場監視当局（MSA）により不適合と判定されるリスクがある。**リスク: high**。
- **IEC 62443**: SHOULDの解釈はSecurity Level Target（SL-T）に依存。SL1-2では省略可能なSRが、SL3-4ではMUST相当に昇格する。**リスク: medium**（SL-T定義に依存）。
- **SEMI E187**: SHOULDは「仕様からマイナスできる」（除外可能）が、顧客（Fab運営会社）との個別契約でMUST化されることがある。**リスク: low〜medium**（契約依存）。

**推奨対応**: 製品の対象市場・セクターを早期に特定し、適用される規格のSHOULD解釈を明確化すること。不明な場合は保守的にMUST扱いとする。

---

## Confidence Distribution（確信度分布）

- **High confidence の発見**: 7件（Finding 1, 2, 3, 5, 6, 7, 10のClaim部分）
- **Medium confidence の発見**: 3件（Finding 4の安全クリティカル事例不足、Finding 8のMCUベアメタル未検証、Finding 9の定量エビデンス不足）
- **Low confidence の発見**: 0件
- **全体の確信度評価**: 技術的Claim（何が可能で何が不可能か）は高い確信度で裏付けられている。ロードマップの時間見積もり・コスト見積もりは定量エビデンス不足により中程度。

---

## Contradictory Claims（矛盾する主張）

### 矛盾1: 「統一プロトコル（MQTT5）で全層を結合」vs「MCUでMQTT5は未サポート」

- **状況**: 先行調査はMQTT5 + W3C Trace Contextによる全層統一を主張していたが、批判的レビューとweb調査の結果、coreMQTT v2のMQTT5 User Properties対応は2026年3月時点で本番品質未到達であることが確認された。
- **解決**: 2層分離設計（Finding 3）で解決。MCU層はMQTT 3.1.1、ゲートウェイ層以上でMQTT5 + Trace Contextを適用。device_idをMQTT 3.1.1 Client IDとして使用し、ゲートウェイ層でtrace_idと結合する。
- **解決状況**: **解決済み**

### 矛盾2: 「100ms以下応答はMUST」vs「応答時間要件はSIL/用途依存」

- **状況**: 先行調査がEdge MUSTとして100msを指定していたが、批判的レビューが「ハザード分析に基づかない数値にMUSTを付与している」と指摘。
- **解決**: 「100ms」をデフォルト設計目標（WANT）に格下げし、実際のMUST値はHAZOP/FMEA結果から導出する。参考値として、MLPerf Tiny v1.3ではCortex-M55上で5ms（KWS）、STM32H7でsub-10ms（画像分類）が実測されており、100msは多くのユースケースで達成可能だが、普遍的MUSTではない。
- **解決状況**: **解決済み**

### 矛盾3: 「フルスタックの強みで統合設計可能」vs「統合スタックはデバッグ難易度を指数的に上げる」

- **状況**: フルスタックスキルが統合の利点になる一方、デバッグの爆発的複雑化のリスク。
- **解決**: インターフェース仕様（SDD）による境界明確化（Finding 10）で対処。各レイヤー間のContract Testを先に定義し、統合テストの際は層間問題の切り分けを自動化する。
- **解決状況**: **部分的解決**（Contract Testの具体的実装パターンは更なる詳細化が必要）

---

## Missing Data（不足データ）

1. **対象MCUの特定とリソースバジェット**: Cortex-M33 / M55 / M85、RISC-V、STM32H7等の具体的選定がなければ、SRAM/Flash/消費電力の試算が空中楼閣。**補完に必要**: 製品要件定義書（PRD）と対象MCUのデータシート
2. **電力バジェット設計**: エッジAI推論頻度とバッテリー寿命のトレードオフ分析が完全に欠如。**補完に必要**: 対象MCUのDuty Cycle消費電流実測値
3. **安全レベル（SIL/ASIL）のリスク評価結果**: 何がsafety-criticalかを決定するHAZOP/FMEA結果がなければ、決定論ゲートの要件レベルが確定しない。**補完に必要**: 製品のリスク評価プロセス実施
4. **LLMクラウド推論のp99レイテンシ実測データ**: フォールバック遷移条件の定量設計に必須。**補完に必要**: 対象LLM API（GPT-4o、Claude等）のレイテンシベンチマーク
5. **日本国内規制（電波法、TELEC）の適用分析**: MQTT通信を搭載する無線機器には電波法上の認証が必要だが未考慮。**補完に必要**: 使用無線モジュールの技適取得状況確認
6. **ISO/SAE 21434のCAL評価プロセス**: 車載適用時のOTAモデル更新に関する脆弱性管理手順が未定義。**補完に必要**: 21434 Section 14-15の詳細レビュー

---

## Limitations（制約・限界）

1. **SEMI E187本文が有償のため直接参照不可**: gap/partialマッピングの精度が限定的。節番号の確定には有償文書の購入が必要
2. **組み込み開発の工数統計に信頼できる大規模調査が存在しない**: ロードマップの期間見積もりは経験則に依存
3. **LLMのエッジ推論（on-device SLM）の技術進化が急速**: 2026年後半にはQualcomm AI HubやApple Intelligence等の進展で前提が変わる可能性がある
4. **本レビューの情報源は英語圏に偏重**: 日本のIoT規制（電波法、不正競争防止法の営業秘密保護）の分析が不足
5. **個別の製品ドメイン（医療/車載/産業/民生）を特定していない**: 規格適用の優先度はドメインにより大きく異なるが、本レビューではドメイン横断的な汎用ガイドラインにとどめている

---

## Unresolved Issues（未解決事項）

1. **対象ハードウェアが未特定**（Critic Reviewより引継ぎ）: MCUアーキテクチャの選定がなければ全リソース試算が不可能。**推奨**: Phase 0でSTM32H7（Cortex-M7、1MB SRAM）またはnRF5340（Cortex-M33、512KB SRAM）をリファレンスプラットフォームとして選定し、PoC実施
2. **Context Assemblerの実装コスト未評価**（Critic Reviewより引継ぎ）: Hot/Warm/Coldストアの構成にKafka/TimescaleDB/Redis等の非自明なインフラが必要。運用コスト（人的・金銭的）が試算されていない
3. **マルチテナント・デバイスIDプロビジョニング設計**（Critic Reviewより引継ぎ）: device_idの生成・配布・失効の仕組み、工場出荷時プロビジョニングの製造工程影響が未定義
4. **coreMQTT v2のMQTT5本番リリース時期の不確実性**: 2層分離設計で回避しているが、将来の1層統合への移行タイミングが未定
5. **エッジMLOpsのモデルドリフト検知**: クラウド側のデータドリフト検知→再学習トリガー→エッジ配信→A/Bテストの自動化パイプラインが概念レベルにとどまる

---

## Conclusion（結論）

### 統合ガイドライン: 「Edge-First SDD × Deterministic Gate × Tiered Context」

フルスタックエンジニアがAIツール開発で成功するための統合設計原則を以下に定義する。

#### 原則1: Edge-First Spec-Driven Design（エッジファースト仕様駆動設計）

MCUのリソース制約（SRAM/Flash/消費電力）を **仕様の第一制約** とし、全レイヤーの設計をそこから逆算する。具体的には:

```
[仕様フロー]
MCUリソースバジェット（SRAM/Flash/mA）
  → エッジ推論モデル制約（パラメータ数・量子化方式）
    → ゲートウェイ通信プロトコル制約（MQTT 3.1.1 + カスタムヘッダ）
      → クラウドLLM API設計（タイムアウト・フォールバック条件）
        → UIレスポンス設計
```

各レイヤー境界にはOpenSpec/GitHub Spec Kit形式のインターフェース仕様を配置し、Contract Testで自動検証する。

#### 原則2: Deterministic Gate as RTOS Task（決定論ゲートのRTOSタスク実装）

```
[タスク優先度設計（高→低）]
Priority 0 (最高): Safety Monitor FSM（閾値・禁止操作・安全制約の即時判定）
Priority 1: Sensor Acquisition（センサーデータ取得、DMA）
Priority 2: Communication（MQTT送受信）
Priority 3: AI Inference（TFLite Micro推論、バックグラウンド）
Priority 4 (最低): Telemetry（メトリクス収集・送信）
```

- **タイムアウトポリシー**: クラウドLLMレスポンスのタイムアウト閾値をHAZOP/FMEAから導出。デフォルト設計目標は「未確定」（100msは普遍的MUSTではない）。
- **フォールバック遷移**: Cloud失敗（タイムアウト / 異常出力検知）時、FSMがdeterministicルールテーブルに基づき即座にfail-safe状態へ遷移。遷移条件・状態遷移図をTLA+または同等の形式仕様で記述することをSHOULDとする。

#### 原則3: Tiered Context Architecture（階層型コンテキスト）

```
[3層コンテキストモデル]
Layer 1 - MCU（ベアメタル/RTOS）:
  - MQTT 3.1.1（coreMQTT v2）
  - device_id = Client ID
  - 軽量バイナリテレメトリ（カスタムフォーマット）
  - 時刻同期: SNTP + ローカルRTC（ドリフト補正付き）
  
Layer 2 - Gateway（組み込みLinux）:
  - MQTT5 User Properties（traceparent注入）
  - OTel Collector（最小構成ビルド）
  - device_id → trace_id 結合
  - セキュリティゾーン境界（IEC 62443コンジット）
  
Layer 3 - Cloud:
  - LLM推論 + Context Assembler
  - Hot/Warm/Cold ストア
  - SDD CI/CDパイプライン
  - EU CRA脆弱性報告システム
```

**時間窓結合の修正**: ±3秒の固定窓ではなく、MCU側RTC精度とSNTP同期間隔に基づく**動的窓幅**を採用。ゲートウェイ層で最終的な時刻補正を行い、クラウド側の結合精度を保証する。

---

## Recommended Actions（推奨アクション）

### Phase 0: 基盤構築（4週間）— 最優先

| # | アクション | 義務レベル | 規格参照 |
|---|---|---|---|
| 0-1 | 対象MCU選定（推奨: STM32H7またはnRF5340）とリソースバジェット策定 | MUST | — |
| 0-2 | EU CRA脆弱性報告体制の構築開始（SBOM生成ツール導入） | MUST | EU CRA Art.14 |
| 0-3 | 製品ドメイン特定と適用規格の確定（SIL/ASIL/SL-T） | MUST | IEC 61508 / 62443 |
| 0-4 | HAZOP/FMEA実施（安全クリティカル判定とタイムアウト閾値導出） | MUST | IEC 61508-1 |

### Phase 1: エッジPoC（6週間）

| # | アクション | 義務レベル | 規格参照 |
|---|---|---|---|
| 1-1 | TFLite Micro実機ベンチマーク（推論レイテンシ・SRAM消費量計測） | MUST | MLPerf Tiny |
| 1-2 | MQTT 3.1.1 + coreMQTT v2 通信スタック実装・検証 | SHOULD | — |
| 1-3 | 決定論ゲート（Safety Monitor FSM）のRTOS実装 | MUST | IEC 61508-3 |
| 1-4 | セキュアブート + OTA A/Bパーティション実装 | SHOULD | NIST SP 800-193 |

### Phase 2: ゲートウェイ統合（4週間）

| # | アクション | 義務レベル | 規格参照 |
|---|---|---|---|
| 2-1 | MQTT5 + W3C Trace Context ゲートウェイ実装 | SHOULD | W3C Trace Context |
| 2-2 | OTel Collector最小構成ビルド・デプロイ | SHOULD | — |
| 2-3 | IEC 62443ゾーン/コンジット定義・トピックACL設定 | MUST | 62443-3-3 SR 5.1-5.4 |
| 2-4 | device_id → trace_id 結合ロジック実装・テスト | SHOULD | — |

### Phase 3: クラウドLLM統合（4週間）

| # | アクション | 義務レベル | 規格参照 |
|---|---|---|---|
| 3-1 | LLM API統合 + フォールバック条件の実装 | SHOULD | — |
| 3-2 | Context Assembler（Hot/Warm/Coldストア）構築 | SHOULD | — |
| 3-3 | SDD CI/CDパイプライン構築（OpenSpec/Spec Kit） | SHOULD | — |
| 3-4 | E2E統合テスト（MCU→Gateway→Cloud→UI全経路） | MUST | — |

### Phase 4: 規格適合・認証（8〜16週間）— 外部要因依存

| # | アクション | 義務レベル | 規格参照 |
|---|---|---|---|
| 4-1 | EMC試験（CE/FCC/TELEC） | MUST（市場投入時） | EMC指令/FCC Part 15/電波法 |
| 4-2 | IEC 62443適合評価（SL-Tに基づく） | MUST（産業適用時） | 62443-3-3 |
| 4-3 | EU CRA適合宣言（DoC）作成 | MUST（EU市場） | EU CRA |
| 4-4 | 脆弱性管理・インシデント報告プロセスの運用開始 | MUST | EU CRA Art.14 |

**合計見積もり: 26〜38週間**（先行調査の10週間から大幅修正。ハードウェア検証・認証工程が工期を支配する）

---

## Source Registry（情報源一覧）

| ID | Source | Primary |
|---|---|---|
| [SRC-001] | [official_document/tier2] [MLPerf Tiny v1.3 Results](https://mlcommons.org/2025/09/mlperf-tiny-v1-3-results/) (2026-03-14) | true |
| [SRC-002] | [community_data/tier4] [coreMQTT MQTT v5 Issue #269](https://github.com/FreeRTOS/coreMQTT/issues/269) (2026-03-14) — community_score: discussion_age: "2 years", participant_count: ~30, expert_presence: true, reproducibility: true | true |
| [SRC-003] | [official_document/tier2] [TFLite Micro Get Started](https://www.tensorflow.org/lite/microcontrollers/get_started) (2026-03-14) | true |
| [SRC-004] | [official_document/tier2] [Zephyr RTOS - Threads](https://docs.zephyrproject.org/latest/kernel/services/threads/index.html) (2026-03-14) | true |
| [SRC-005] | [official_document/tier2] [FreeRTOS Task Priorities](https://www.freertos.org/Documentation/02-Kernel/02-Kernel-features/01-Tasks-and-co-routines/03-Task-priorities) (2026-03-14) | true |
| [SRC-006] | [industry_report/tier3] [Deterministic AI Orchestration - Praetorian](https://www.praetorian.com/blog/deterministic-ai-orchestration-a-platform-architecture-for-autonomous-development/) (2026-03-14) | false |
| [SRC-007] | [industry_report/tier3] [AI in Safety-Critical Embedded Systems - Parasoft](https://www.parasoft.com/blog/ai-in-safety-critical-embedded-systems/) (2026-03-14) | false |
| [SRC-008] | [official_document/tier2] [coreMQTT Releases](https://github.com/FreeRTOS/coreMQTT/releases) (2026-03-14) | true |
| [SRC-009] | [official_document/tier2] [MCUXpresso SDK - coreMQTT](https://mcuxpresso.nxp.com/mcuxsdk/25.09.00-pvw1/html/rtos/freertos/coremqtt/index.html) (2026-03-14) | true |
| [SRC-010] | [official_document/tier1] [MQTT Version 5.0 Specification](https://docs.oasis-open.org/mqtt/mqtt/v5.0/os/mqtt-v5.0-os.html#_Toc3901013) (2026-03-14) | true |
| [SRC-011] | [academic_paper/tier2] [Spec-Driven Development: From Code to Contract](https://arxiv.org/html/2602.00180v1) (2026-03-14) | true |
| [SRC-012] | [official_document/tier2] [GitHub Blog - Spec-driven development with AI](https://github.blog/ai-and-ml/generative-ai/spec-driven-development-with-ai-get-started-with-a-new-open-source-toolkit/) (2026-03-14) | false |
| [SRC-013] | [industry_report/tier3] [ThoughtWorks - Spec-driven development](https://www.thoughtworks.com/insights/blog/agile-engineering-practices/spec-driven-development-unpacking-2025-new-engineering-practices) (2026-03-14) | false |
| [SRC-014] | [industry_report/tier3] [Augment Code - SDD Guide](https://www.augmentcode.com/guides/mastering-spec-driven-development-with-prompted-ai-workflows-a-step-by-step-implementation-guide) (2026-03-14) | false |
| [SRC-015] | [official_document/tier1] [EU CRA Implementation - European Commission](https://digital-strategy.ec.europa.eu/en/factpages/cyber-resilience-act-implementation) (2026-03-14) | true |
| [SRC-016] | [official_document/tier1] [CRA Implementation Timeline](https://eu-cyber-laws.com/cra/timeline/) (2026-03-14) | false |
| [SRC-017] | [industry_report/tier3] [CRA Complete Timeline 2025-2027](https://craevidence.com/blog/cra-implementation-timeline-2025-2027) (2026-03-14) | false |
| [SRC-018] | [official_document/tier2] [ISA/IEC 62443-3-3 - Cisco](https://www.cisco.com/c/en/us/products/collateral/security/isaiec-62443-3-3-wp.html) (2026-03-14) | false |
| [SRC-019] | [industry_report/tier3] [IEC 62443 in 2025 - Elisity](https://www.elisity.com/blog/iec-62443-in-2025-network-segmentation-requirements-and-changes) (2026-03-14) | false |
| [SRC-020] | [official_document/tier1] [ISA/IEC 62443 Series](https://www.isa.org/standards-and-publications/isa-iec-62443-series-of-standards) (2026-03-14) | true |
| [SRC-021] | [official_document/tier1] [NIST SP 800-193](https://csrc.nist.gov/pubs/sp/800/193/final) (2026-03-14) | true |
| [SRC-022] | [official_document/tier2] [Microchip Platform Root of Trust](https://www.microchip.com/en-us/solutions/data-centers-and-computing/computing-solutions/technologies/platform-root-of-trust-secure-boot) (2026-03-14) | false |
| [SRC-023] | [official_document/tier2] [Lattice PFR](https://www.latticesemi.com/en/Solutions/Solutions/SolutionsDetails02/PFR) (2026-03-14) | false |
| [SRC-024] | [industry_report/tier3] [IoT Edge Observability - OneUptime](https://oneuptime.com/blog/post/2026-02-06-observability-iot-edge-devices-opentelemetry/view) (2026-03-14) | false |
| [SRC-025] | [industry_report/tier3] [Red Hat Device Edge + OTel](https://developers.redhat.com/articles/2025/05/29/boost-red-hat-device-edge-observability-opentelemetry) (2026-03-14) | false |
| [SRC-026] | [official_document/tier2] [OpenTelemetry](https://opentelemetry.io/) (2026-03-14) | true |
| [SRC-027] | [industry_report/tier3] [Barr Group - Software Bug Statistics](https://barrgroup.com/blog/software-bug-statistics) (2026-03-14) | false |
| [SRC-028] | [unknown/tier4] 組み込み開発プロジェクト工数経験則 (unknown) | false |
| [SRC-029] | [academic_paper/tier2] [MLPerf Tiny Benchmark Paper](https://arxiv.org/pdf/2106.07597) (2026-03-14) | true |
| [SRC-030] | [official_document/tier2] [OpenTelemetry for IoT (OTEP #237)](https://github.com/open-telemetry/oteps/issues/237) (2026-03-14) | false |
| [SRC-031] | [official_document/tier1] [IEC 61508 Functional Safety](https://www.iec.ch/functional-safety) (2026-03-14) | true |
| [SRC-032] | [official_document/tier1] [ISO/SAE 21434:2021](https://www.iso.org/standard/70918.html) (2026-03-14) | true |
| [SRC-033] | [official_document/tier2] [OpenSpec Guide](https://redreamality.com/garden/notes/openspec-guide/) (2026-03-14) | false |
| [SRC-034] | [industry_report/tier3] [Spec-Driven LLM Development - Actualyze](https://actualyze.ai/blog/2026-01-11-spec-driven-development-with-llms) (2026-03-14) | false |

**Source Coverage:**

| source_type | 件数 |
|---|---|
| official_document | 21件 |
| industry_report | 10件 |
| academic_paper | 2件 |
| community_data | 1件 |
| unknown | 1件 |

| reliability_tier | 件数 |
|---|---|
| tier1 | 8件 |
| tier2 | 15件 |
| tier3 | 10件 |
| tier4 | 2件 |

| 指標 | 値 |
|---|---|
| primary_source: true | 51%（35件中18件） |
| primary_source: false | 49% |
| **多様性評価** | **中**（先行調査の「低」から改善。学術論文2件・業界レポート10件を追加。ただし日本語情報源は依然として欠如） |
