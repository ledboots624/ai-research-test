# logic-analyzer-japan

## テーマ

**調査テーマ**: logic-analyzer-japan

**説明**: 日本国内から3万円以下で購入可能なMCU開発向けロジックアナライザーの調査と選定

### 前提条件

> ロジックアナライザー購入リサーチの前提条件

- **budget**: 3万円以下
- **priority**: コストパフォーマンス重視
- **use_case**: MCU開発のデバッグ・プロトコル解析
- **protocols**: SPI, UART, I2C
- **target_mcu**: STM32, ESP32, Arduino
- **host_pc**: Mac Mini M4（Apple Silicon）
- **purchase**: 日本国内から購入可能であること（Amazon.co.jp、秋月電子、マルツ等）

---

## 主要ドキュメント

| ドキュメント | 説明 |
|------------|------|
| [最終レポート（統合レビュー）](./05-integration-review-claude-opus-4.6.md) | 全調査結果を統合した最終結論 |
| [批判的レビュー](./04-critic-review-claude-sonnet-4.6.md) | 論理矛盾・エビデンス不足・バイアスの検証 |
| [情報源一覧](./sources.json) | 全ソースの集約リスト（Source ID・URL・信頼性評価） |

## メタデータ

| 項目 | 値 |
|------|-----|
| **生成方法** | planner |
| **実行日時** | 2026-03-14 21:08:30 |
| **ツール** | AI Research Agent v2.2.0 |
| **手法** | Evidence-based Multi-Model Pipeline |

## パイプライン

| # | 役割 | モデル | 出力 |
|---|------|-------|------|
| 1 | 情報収集 | gemini-3-pro-preview | [情報収集の出力を見る](./01-information-gathering-gemini-3-pro-preview.md) (完了) |
| 2 | 深層分析 | claude-sonnet-4.6 | [深層分析の出力を見る](./02-deep-analysis-claude-sonnet-4.6.md) (完了) |
| 3 | 技術戦略 | gpt-5.3-codex | [技術戦略の出力を見る](./03-technical-strategy-gpt-5.3-codex.md) (完了) |
| 4 | 批判的レビュー | claude-sonnet-4.6 | [批判的レビューの出力を見る](./04-critic-review-claude-sonnet-4.6.md) (完了) |
| 5 | 統合レビュー | claude-opus-4.6 | [統合レビューの出力を見る](./05-integration-review-claude-opus-4.6.md) (完了) |

## 各ステップの要約

### Step 1: 情報収集（gemini-3-pro-preview）

# Executive Summary (結論)

Apple Silicon Mac (M4) 環境における3万円以下のMCU開発用ロジックアナライザーとして、以下の3つの選択肢が推奨されます。

1.  **Best Performance:** **Kingst LA2016** (約2.7万円) - 高速・多機能、Mac対応可(Rosetta 2)。
2.  **Best Cost-Performance:** **Kingst LA1010** (約1.3万円) - 必要十分な性能、Mac対応可(Rosetta 2)。
3.  **Entry Level:** **汎用 24MHz 8ch** (約1,500円) - 圧倒的安価、Macネイティブ対応(Sigrok)。

**注意:** 秋月電子等で流通する **Zeroplus LAP-Cシリーズ** はWindows専用であり、Mac環境では非推奨です。

---

## 1. High-Performance Option: Kingst LA2016

予算上限に近いですが、最も性能が高く、将来的に複雑な信号解析にも対応可能なモデルです。

*   **Claim**: 16ch/200MHzの高性能モデルがAmazon.co.jpで約2.7万円で入手可能。Mac環境(Mシリーズ)ではRosetta 2経由で公式ソフトが動作する。
*   **Evidence**:
    *   Amazon.co.jpでの実勢価格は税込27,294円。
    *   公式ソフトウェア「KingstVIS」はmacOS対応だが、Intel版(x86_64)のみ提供。Apple Silicon上ではRosetta 2変換で動作する。

_...（全97行。詳細は[情報収集の全文](./01-information-gathering-gemini-3-pro-preview.md)を参照）_

### Step 2: 深層分析（claude-sonnet-4.6）

# Deep Analysis: ロジックアナライザー スペック詳細比較とMac Apple Silicon互換性

## Overview（分析概要）

本分析は、前ステップの情報収集結果を基盤として、各製品のスペック詳細（最大サンプリングレート・メモリ深度・入力電圧範囲）の深掘りと、Mac（Apple Silicon Mシリーズ）上での純正ソフトウェアおよびsigrok/PulseView動作状況を検証する。特に、マーケティング数値と実効性能の乖離、ソフトウェア互換性の落とし穴を重点的に掘り下げる。

---

## Section 1: Detailed Spec Comparison（スペック詳細比較）

### 1-1. Kingst LA2016

**Claim（主張）**: LA2016はハードウェアメモリを搭載した真の高性能モデルであり、スペックシートの数値は実運用において信頼できる。

**Evidence（根拠）**:
- **最大サンプリングレート**: 200MHz（全16ch同時）
- **等価帯域幅**: 40MHz（20MHzの信号まで確実に解析可能）
- **ハードウェアメモリ**: **1Gbit（128MB）搭載**、50M samples/ch（ハードウェアバッファ）
- **最大圧縮深度**: 10G samples/ch（RLE圧縮、ソフトウェア依存）
- **入力電圧範囲**: -50V ～ +50V（保護回路付き）

_...（全346行。詳細は[深層分析の全文](./02-deep-analysis-claude-sonnet-4.6.md)を参照）_

### Step 3: 技術戦略（gpt-5.3-codex）

技術評価の根拠を固めるため、サンプリング理論・I2C規格・主要デバイス仕様の一次情報を先に確認します。

# Technical-Strategy Assessment for logic-analyzer-japan（技術戦略評価）

## Findings（主要な発見）

**Claim（主張）**: 4候補（LA2016 / LA1010 / DSLogic Plus / FX2系）は、`高速SPI`への適格性で明確に2群へ分かれる。  
**Evidence（根拠）**: LA2016は16ch同時200MHz、DSLogic Plusは4ch時400MHz（Buffer）だが、LA1010は4ch想定で50MHz、FX2系は24MHz級。  
**Source（情報源）**: S9, S10, S11, S12, S13  
**Confidence（確信度）**: high
**Claim（主張）**: MCU開発で「高速通信含む」を満たすには、`Nyquist最小`と`実務オーバーサンプリング`を分離して判定すべき。  
**Evidence（根拠）**: Shannonの標本化定理は最小条件 `fs >= 2fmax`、一方デジタルデコード実務では4x以上が推奨される。  
**Requirement Level（要件レベル）**:  
- **MUST**: `fs >= 2fclock`（理論下限）  
- **SHOULD**: `fs >= 4fclock`（実務推奨）  
- **WANT**: `fs >= 10fclock`（余裕ある解析）  
**Standard Reference Mapping（規格参照）**: Shannon theorem（direct）, Saleae実装ガイド（partial）  
**Source（情報源）**: S1, S2  
**Confidence（確信度）**: high（MUST）, medium（SHOULD/WANT）
**Claim（主張）**: I2C/UARTは多くの候補で十分だが、I2C Fm+/Hsでは`プローブ容量`が支配要因になりやすい。  

_...（全230行。詳細は[技術戦略の全文](./03-technical-strategy-gpt-5.3-codex.md)を参照）_

### Step 4: 批判的レビュー（claude-sonnet-4.6）

# Critic Review（批判的レビュー）

## Validation Summary（検証サマリー）

| 検証項目 | 件数 | 備考 |
|---|---|---|
| Claim without Evidence（根拠なし主張） | 0件 | すべての主要 Claim に Evidence が付与されている |
| Evidence without Source（出典なし根拠） | 2件 | 後述：Δtr計算式の自己導出部分、Saleae FAQ 4x推奨の具体箇所 |
| Conclusion without Confidence（確信度なし結論） | 0件 | Device Verdicts と Conclusion はすべて Confidence 付与済み |
| Confidence 判定の過大評価 | 3件 | 後述：DSLogic Plus Buffer 400MHz/high、LA1010 16MHz@16ch/high、FX2系の電気特性/medium |
| 全体 Validation 結果 | **PARTIAL** | 構造的品質は高いが、ソフトウェアリスク・macOS互換性・グランドループ問題が完全に欠落 |

---

## 義務レベル分析（Requirement Level Analysis）

- **MUST（必須）**: 4件 — Shannon 2x下限、I2C規格タイミング、MCU SPI上限対応、I2C Fm+/Hs容量制約
- **SHOULD（推奨）**: 2件 — 4xオーバーサンプリング（実務推奨）、10xオーバーサンプリング（高精度志向）
- **MAY（任意）**: 0件
- **MUST_NOT（禁止）**: 0件

_...（全240行。詳細は[批判的レビューの全文](./04-critic-review-claude-sonnet-4.6.md)を参照）_

### Step 5: 統合レビュー（claude-opus-4.6）

Now let me research current pricing, availability, and macOS compatibility for the logic analyzer candidates.

Now I have all the research data needed. Let me compile the comprehensive integration review.

# 統合レビュー

## Executive Summary（総括）

3万円以下で日本国内から購入可能な MCU 開発向けロジックアナライザーを評価した結果、**Kingst LA2016（16ch/200MHz、約22,000〜27,000円）** が総合的にもっともコストパフォーマンスの高い第一推奨となる。ただし Mac Mini M4 ユーザーにとって、すべての候補が macOS ネイティブ対応ではなく、sigrok/PulseView（Rosetta 2 経由または Homebrew ソースビルド）が事実上の標準ソフトウェアとなる点は購入前に認識すべき重大な前提条件である。予算に余裕がある場合は USB アイソレーター（約1,600〜3,300円）の追加を強く推奨する。DSLogic Plus は高スペックだが macOS での SPI デコードバグ等の既知問題があり、現時点では条件付き推奨に留まる。

## Research Question（調査の問い）

「Mac Mini M4 を使用する MCU 開発者が、日本国内から3万円以下で購入可能なロジックアナライザーのうち、SPI/UART/I2C プロトコル解析においてもっともコストパフォーマンスが高く信頼性のある選択肢はどれか」

## Findings（主要な発見）

### Finding 1: Kingst LA2016 は価格対性能比で最優秀

- **Claim（主張）**: Kingst LA2016（16ch/200MHz/128Mbit メモリ）は、Amazon.co.jp で約22,000〜27,000円で購入可能であり、MCU 開発で標準的な SPI（〜50MHz）、I2C（Standard〜Fast-mode）、UART の解析に十分なサンプリングレートを持つ。3万円以下の予算枠で購入できる候補の中で、チャンネル数・サンプリングレート・メモリ深度のバランスがもっとも優れている。
- **Evidence（根拠）**: 200MHz サンプリングレートは、50MHz SPI に対して 4x オーバーサンプリング（SHOULD レベルの推奨）を達成可能。16ch により SPI+I2C の同時キャプチャが可能。128Mbit バッファにより長時間キャプチャに対応。Amazon.co.jp での複数出品者による価格帯は22,000〜29,000円。

_...（全415行。詳細は[統合レビューの全文](./05-integration-review-claude-opus-4.6.md)を参照）_

---

## Trust Summary（信頼性サマリー）

### Validation Result（検証結果）: **PARTIAL**

| 指標 | 値 |
|------|-----|
| 総 Claim（主張）数 | 63 |
| Evidence（根拠）なし | 1 |
| Source（情報源）なし | 4 |
| Confidence（確信度）なし | 0 |

### Confidence Distribution（確信度分布）

| Level（レベル） | 件数 |
|-------|------|
| High（高） | 51 |
| Medium（中） | 12 |
| Low（低） | 0 |

### Source Reliability（情報源の信頼性分布）

| Tier（階層） | 件数 | 説明 |
|------|------|------|
| Tier 1（公式） | 25 | 公式文書・法令・政府機関・標準化団体 |
| Tier 2（高信頼） | 19 | 企業公式・研究機関・学術論文 |
| Tier 3（参考） | 4 | 技術ブログ・専門メディア・業界記事 |
| Tier 4（コミュニティ） | 15 | フォーラム・コミュニティ・SNS |

一次情報（Primary Source）: 38件 / 二次情報: 25件

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

_Generated by [AI Research Agent](https://github.com/ledboots624/ai-research-agent) v2.2.0_
