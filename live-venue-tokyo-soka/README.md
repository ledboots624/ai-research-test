# live-venue-tokyo-soka

## テーマ

**調査テーマ**: live-venue-tokyo-soka

**説明**: 東京都内および埼玉県草加市周辺における、ロック・ブルース・フォーク等の生演奏が可能な店舗（ライブバー、ライブハウス、オープンマイク等）のリサーチと出演戦略の策定

### 前提条件

> 生演奏可能な店舗リサーチの前提条件

- **area**: 東京都内, 埼玉県草加市周辺
- **genres**: ロック, ブルース, フォーク, 弾き語り
- **venue_type**: ライブバー、ライブハウス、オープンマイク、セッションバー等

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
| **実行日時** | 2026-03-14 20:49:56 |
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

# Research Report: Live Venues for Rock, Blues, and Folk in Tokyo & Soka

## Executive Summary（エグゼクティブサマリー）
**Claim**: 草加市周辺は「地域密着型の小規模コミュニティ」が中心であり、特定の曜日に開催されるイベント（オープンマイク等）への参加が鍵となります。一方、東京都内はジャンル（ロック・ブルース・フォーク）ごとに確立された「専門バー」が多数存在し、毎日何らかのセッションが行われています。
**Strategy**: 草加エリアでは近隣の八潮市を含めた広域での活動を視野に入れ、都内では自身のジャンルに特化した店舗へ「武者修行」的に参加する併用戦略が有効です。

---

## Findings: Soka City & Surroundings（草加市・周辺エリアの調査結果）

### 1. COOL BAR（クールバー）【八潮市】
**Claim**: 草加エリア近隣で最も確実に「ロック・ポップス・弾き語り」のオープンマイクに参加できる最重要スポットです。
**Evidence**:
- 毎週木曜日 19:00よりオープンマイクを開催。
- 参加費は1セット1,500円（3曲交代制）。
- ライブハウス級の機材があり、フルバンドから弾き語りまで対応可能。
**Confidence**: high
**Source**:
- [official_document/tier2] [COOL BAR 公式サイト](https://www.coolbar0924.org/) (2025-04-12) — primary_source: true
- [community_data/tier3] [オープンマイクナビ COOL BAR店舗情報](https://www.openmic-navi.com/shop/253) (2025-04-12) — primary_source: false

_...（全101行。詳細は[情報収集の全文](./01-information-gathering-gemini-3-pro-preview.md)を参照）_

### Step 2: 深層分析（claude-sonnet-4.6）

---

# Deep Analysis: 演奏の敷居・客層・機材・飛び入り参加性の比較評価

## Overview（分析概要）

本分析は、前ステップの情報収集結果に加え、各店舗の口コミデータ・イベントログ・公式情報を横断的に収集し、**演奏の敷居・常連客の雰囲気・機材充実度・飛び入り参加のしやすさ**の4軸で各店舗を評価する。特に「草加エリア vs 東京都内」の客層差と構造的な違いを深層分析する。

---

## Findings A: Soka & Surrounding Area — Deep Venue Assessment（草加・周辺エリアの深層評価）

### A-1. COOL BAR（クールバー）【八潮市】

#### 演奏の敷居（Barrier to Entry）

**Claim**: COOL BARは草加・八潮エリアで最も「参加へのハードルが低い」公演型スポットであり、初心者・経験者が混在する真の意味でのオープンマイクを実現している。

**Evidence**:
- 1セット3曲交代制（参加費1,500円）という明確なルールが、初心者が「何をすればいいか」わからないという不安を構造的に解消している。

_...（全380行。詳細は[深層分析の全文](./02-deep-analysis-claude-sonnet-4.6.md)を参照）_

### Step 3: 技術戦略（gpt-5.3-codex）

# Live-Venue Booking Technical Strategy（東京・草加周辺の出演獲得戦略）

## Findings（主要な発見）

### Claim（主張）
最短で出演実績を作るなら、**参加費型で参加手順が公開されている枠（COOL BAR / Space With / Bright Brown）**を先に回すのが最も成功確率が高い。

**Evidence（根拠）**
- COOL BARは木曜オープンマイク、`3曲交代・1,500円`の明示ルール。
- Space With「Rockin’ Tonight」は`演奏エントリー制・1,800円（ドリンク別）`を明示。
- Bright Brownは木曜セッション`1,000円＋ミニマムオーダー1,000円`を明示。

**Source（情報源）**
- [official_document/tier2] [COOL BAR 公式](https://www.coolbar0924.org/) (2026-03-14) — primary: true
- [official_document/tier2] [COOL BAR イベント一覧](https://www.coolbar0924.org/event-list) (2026-03-14) — primary: true
- [official_document/tier2] [Space With セッション](https://www.spacewith.co.jp/system/session.html) (2026-03-14) — primary: true
- [official_document/tier2] [Bright Brown SCHEDULE](https://brightbrownnakano.wixsite.com/brightbrown/schedule) (2026-03-14) — primary: true

**Confidence（確信度）**: high
### Claim（主張）

_...（全211行。詳細は[技術戦略の全文](./03-technical-strategy-gpt-5.3-codex.md)を参照）_

### Step 4: 批判的レビュー（claude-sonnet-4.6）

# Critic Review（批判的レビュー）

## Validation Summary（検証サマリー）

- **Claim without Evidence（根拠なしの主張）**: 0件（全 Claim に Evidence 記載あり）
- **Evidence without Source（出典なしの根拠）**: **2件**
  - Booking Flowchart の Evidence：「上記マトリクス掲載の公式各店ページ一式」— 列挙された個別URLへの明示リンクなし、参照範囲が曖昧
  - Recommendations の Evidence：「上記 Findings/Matrix/Flow の各ソース一式」— 同上、集約参照では検証不可
- **Conclusion without Confidence（確信度なしの結論）**: 0件
- **全体の Validation 結果**: **PARTIAL**

---

## 義務レベル分析（Requirement Level Analysis）

- **MUST（必須）**: 8件
- **SHOULD（推奨）**: 3件
- **MAY（任意）**: 2件
- **MUST_NOT（禁止）**: 1件
- **WANT**: 0件

_...（全173行。詳細は[批判的レビューの全文](./04-critic-review-claude-sonnet-4.6.md)を参照）_

### Step 5: 統合レビュー（claude-opus-4.6）

# 統合レビュー

## Executive Summary（総括）

東京都内および埼玉県草加市周辺におけるロック・ブルース・フォーク系の生演奏可能店舗を統合的にリサーチした結果、**Tier 1（即時参加可能）店舗6件、Tier 2（短期ブッキング可能）店舗5件、Tier 3（中期目標）店舗4件**の計15件を確認した。批判的レビューで指摘された「エリア偏在」「草加周辺の過少代表」「ブッキング型ライブハウスの欠落」を是正し、下北沢・高円寺・吉祥寺および越谷・八潮エリアの店舗を追加した。**COOL BAR の所在地は中野ではなく埼玉県八潮市**であり、草加市から至近（東武スカイツリーライン1駅）という重要な事実誤認を修正した。出演戦略は「低障壁枠での実績構築→近隣エリアでのブッキング挑戦→東京主要拠点への進出」の3段階を推奨するが、各段階の期間・回数は目安値であり、個人の状況に応じた柔軟な運用を前提とする。

## Research Question（調査の問い）

東京都内および埼玉県草加市周辺において、ロック・ブルース・フォーク・弾き語りの生演奏が可能な店舗（ライブバー、ライブハウス、オープンマイク、セッションバー等）はどこにあり、それぞれの出演条件・参加障壁はどの程度で、どのような順序・戦略で出演実績を構築すべきか。

## Findings（主要な発見）

### Finding 1: 草加市周辺の最有力店舗は COOL BAR（八潮市）である

- **Claim（主張）**: 前ステップで「COOL BAR 中野」と記載されていた店舗は**埼玉県八潮市**所在であり、草加駅から東武スカイツリーラインで1駅（約5分）の距離にある。毎週木曜日にオープンマイクを開催しており、1組3曲交代制・1セット1,500円で参加可能。草加在住者にとってアクセス・コスト両面で最も合理的な初動拠点である。
- **Evidence（根拠）**: COOL BAR 公式サイトに所在地（埼玉県八潮市上馬場7-5）、オープンマイク開催日（毎週木曜19:00〜）、料金（1セット1,500円）が明記。オープンマイクナビにも同様の情報が掲載。
- **Source（情報源）**: [SRC-001] [official_document/tier2] [COOL BAR 公式サイト](https://www.coolbar0924.org/) (2026-03-14) — primary_source: false / [SRC-002] [community_data/tier3] [オープンマイクナビ COOL BAR](https://www.openmic-navi.com/shop/253) (2026-03-14) — primary_source: false
- **Confidence（確信度）**: high（複数の独立ソースで裏付け）
- **Evidence Strength（根拠の強さ）**: strong
- **Requirement Level（義務レベル）**: N/A

_...（全336行。詳細は[統合レビューの全文](./05-integration-review-claude-opus-4.6.md)を参照）_

---

## Trust Summary（信頼性サマリー）

### Validation Result（検証結果）: **PARTIAL**

| 指標 | 値 |
|------|-----|
| 総 Claim（主張）数 | 55 |
| Evidence（根拠）なし | 1 |
| Source（情報源）なし | 12 |
| Confidence（確信度）なし | 0 |

### Confidence Distribution（確信度分布）

| Level（レベル） | 件数 |
|-------|------|
| High（高） | 28 |
| Medium（中） | 25 |
| Low（低） | 2 |

### Source Reliability（情報源の信頼性分布）

| Tier（階層） | 件数 | 説明 |
|------|------|------|
| Tier 1（公式） | 22 | 公式文書・法令・政府機関・標準化団体 |
| Tier 2（高信頼） | 12 | 企業公式・研究機関・学術論文 |
| Tier 3（参考） | 6 | 技術ブログ・専門メディア・業界記事 |
| Tier 4（コミュニティ） | 15 | フォーラム・コミュニティ・SNS |

一次情報（Primary Source）: 24件 / 二次情報: 31件

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
