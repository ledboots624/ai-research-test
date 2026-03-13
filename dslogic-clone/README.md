# Research: dslogic-clone

> DSLogic（ロジックアナライザ）のクローン製品調査。格安かつ品質の良い製品を特定し、日本から購入可能な入手経路（AliExpress、Amazon.co.jp等）を調査する。対象スペック、互換性、sigrok/PulseView対応状況も含む

---

## メタデータ

| 項目 | 値 |
|------|-----|
| **生成方法** | default-standard |
| **実行日時** | 2026-03-14 02:58:38 |
| **ツール** | AI Research Agent v2.1.0 |
| **手法** | Evidence-based Multi-Model Pipeline |

## パイプライン

| # | ID | Role | Model | 出力 |
|---|-----|------|-------|------|
| 1 | research | information-gathering | gemini-3-pro-preview | [01-information-gathering-gemini-3-pro-preview.md](./01-information-gathering-gemini-3-pro-preview.md) (完了) |
| 2 | analysis | deep-analysis | claude-sonnet-4.6 | [02-deep-analysis-claude-sonnet-4.6.md](./02-deep-analysis-claude-sonnet-4.6.md) (完了) |
| 3 | review | integration-review | claude-opus-4.6 | [03-integration-review-claude-opus-4.6.md](./03-integration-review-claude-opus-4.6.md) (完了) |

## 各ステップの要約

### 01-information-gathering-gemini-3-pro-preview.md

# Executive Summary (概要)

DSLogicシリーズのクローン製品（互換機）について調査を行いました。結論として、**「DSLogic Plus」相当のハードウェアスペック（特に256Mbit SDRAM搭載）を持つ製品**が、コストパフォーマンスとsigrok互換性の観点で推奨されます。

購入経路としては、**AliExpressが最安（約1.3〜1.6万円）**ですが、納期とサポートを重視する場合はAmazon.co.jp（約3.2万円〜）も選択肢に入ります。Amazonでは「U2BASIC」という名称で販売されている製品が、スペック上はPlus相当である可能性が高いことが判明しました。

以下に詳細な調査結果を報告します。

## Product Selection & Specs (製品選定とスペック)

### Recommended Model: DSLogic Plus Clone
**Claim**: クローン製品を購入する場合、**DSLogic Plus** 相当のモデル（またはAmazonでの **U2BASIC**）を強く推奨します。Basic版との決定的な違いはオンボードメモリの有無です。
**Evidence**:
- **DSLogic Basic**: ストリーミング転送のみ。USB帯域に依存するため、高サンプリングレートでの長時間キャプチャが困難。
- **DSLogic Plus (U2BASIC)**: 256Mbit (32MB) SDRAMバッファを搭載。これにより、最大400MSa/sでのバースト転送や、100MHz帯域幅での安定したキャプチャが可能になります。
**Source**:
- [community_data] DSLogic Basic vs Plus hardware difference (EEVblog, access_date: 2026-03-13)
- [merchant_data] Amazon U2BASIC Product Description (Amazon.co.jp, access_date: 2026-03-13)
**Confidence**: high


_...（全92行。詳細は[ファイル本体](./01-information-gathering-gemini-3-pro-preview.md)を参照）_

### 02-deep-analysis-claude-sonnet-4.6.md

# Deep Analysis: DSLogic クローン製品調査（深層分析）

## Critical Gaps & Contradictions（重大なギャップと矛盾点）

### ⚠️ Gap 1: 「Pango版」問題——前ステップ調査で完全に見落とされた最重要リスク

**Claim**: 2023年以降に製造・出荷されている DSLogic Plus には、内部 FPGA が Xilinx Spartan-6 から **PangoMicro PGL12G（中国製 FPGA）に変更された「Pango バリアント」**が存在し、sigrok/PulseView と**完全に非互換**である。

**Evidence**:
- USB VID:PID が `2a0e:0020`（旧 Xilinx 版）から `2a0e:0030`（Pango 版）に変更されており、libsigrok 側でデバイスが認識されないか、正常動作しない。
- AliExpress や Amazon で現在販売されているユニットが旧版・新版のどちらかを判別する確実な方法は存在せず、製品写真や説明文からは判断不可能。
- sigrok コミュニティでは Pango バリアントの逆解析・対応作業が議論されているが、2026年3月時点でマージされた対応は存在しない。

**Source**:
- [official_document] DreamSourceLab DSLogic Plus - sigrok Wiki (https://sigrok.org/wiki/DreamSourceLab_DSLogic_Plus, 2026-03-13)
- [community_data] DSlogic Plus Pango Version (2023) - EEVblog (https://www.eevblog.com/forum/testgear/dslogic-plus-pango-version-(2023)/, 2026-03-13)
- [community_data] Cheap solution for a fast SPI analyzer - EEVblog page 4 (https://www.eevblog.com/forum/microcontrollers/cheap-solution-for-a-fast-spi-analyzersniffer/75/, 2026-03-13)

**Confidence**: **high**


_...（全165行。詳細は[ファイル本体](./02-deep-analysis-claude-sonnet-4.6.md)を参照）_

### 03-integration-review-claude-opus-4.6.md

Now let me write the integrated review to the research output file.

Now I have enough context. Let me produce the final integrated review:

# 統合レビュー

## Executive Summary（総括）

DSLogicクローン製品の調査において、**2023年以降のPango FPGA版への移行**が最も重大な変数である。sigrok/PulseView利用を前提とする場合、現行流通品はロット判別が不可能でありリスクが高い。DSView専用利用であれば現行品で問題ない。価格面ではAliExpressが¥13,000〜16,000、Amazon.co.jpのU2BASICが¥30,000前後（販売者により幅あり）であり、**sigrok互換性を重視しないならばDSLogic Plus相当品のDSView運用が最良**、**sigrokを重視するならばKingst LA2016/LA5016への切り替えを推奨する**。

## Research Question（調査の問い）

DSLogic（ロジックアナライザ）のクローン製品について、格安かつ品質の良い製品を特定し、日本から購入可能な入手経路（AliExpress、Amazon.co.jp等）を調査する。対象スペック、互換性、sigrok/PulseView対応状況を含めた総合評価を行う。

## Findings（主要な発見）

### Finding 1: Pango FPGA版移行——sigrok互換性に対する最大リスク

- **Claim（主張）**: 2023年以降に製造されたDSLogic Plus/U2Basicは、内部FPGAがXilinx Spartan-6からPangoMicro PGL12Gに変更されている。この変更により、sigrok公式のlibsigrokでは2026年3月時点でも正式サポートされていない。ただし、コミュニティ提供のファームウェアファイル（cantclosevi/sigrok-firmware）を手動配置することで、**部分的な動作は可能**になりつつある。
- **Evidence（根拠）**:

_...（全228行。詳細は[ファイル本体](./03-integration-review-claude-opus-4.6.md)を参照）_

---

## Trust Summary（信頼性サマリー）

### Validation Result（検証結果）: **PARTIAL**

| 指標 | 値 |
|------|-----|
| 総 Claim（主張）数 | 34 |
| Evidence（根拠）なし | 0 |
| Source（情報源）なし | 3 |
| Confidence（確信度）なし | 0 |

### Confidence Distribution（確信度分布）

| Level（レベル） | 件数 |
|-------|------|
| High（高） | 25 |
| Medium（中） | 8 |
| Low（低） | 1 |

### Confidence 判定基準（確信度の基準）

| Level（レベル） | 基準 |
|-------|------|
| **High（高）** | 一次情報または複数独立ソースで裏付けあり |
| **Medium（中）** | 信頼できるソースあり、ただし補強不足 |
| **Low（低）** | 推測を含む、または情報不足 |

### Validation Rules（検証ルール）

1. すべての Claim（主張）に Evidence（根拠）が付いていること
2. すべての Evidence（根拠）に Source（情報源）が付いていること
3. すべての結論に Confidence（確信度）が付いていること
4. Critic Review（批判的レビュー）が未解決事項をリスト化していること

### 注意事項

- AI生成のため、重要な意思決定には人間による検証を推奨
- Evidence JSON (`.evidence.json`) で機械的な監査が可能
- Validation 結果 (`.validation.json`) でレポートの品質を定量評価

---

_Generated by [AI Research Agent](https://github.com/ledboots624/ai-research-agent) v2.1.0_
