# Research: japan-semiconductor-cra

> 日本半導体装置関連ソフトウェア開発企業のEU CRA対策の打開策

---

## メタデータ

| 項目 | 値 |
|------|-----|
| **生成方法** | planner |
| **実行日時** | 2026-03-14 13:07:18 |
| **ツール** | AI Research Agent v2.2.0 |
| **手法** | Evidence-based Multi-Model Pipeline |

## パイプライン

| # | ID | Role | Model | 出力 |
|---|-----|------|-------|------|
| 1 | step1_cra_semi_landscape | information-gathering | gemini-3-pro-preview | [01-information-gathering-gemini-3-pro-preview.md](./01-information-gathering-gemini-3-pro-preview.md) (完了) |
| 2 | step2_gap_risk_analysis | deep-analysis | claude-sonnet-4.6 | [02-deep-analysis-claude-sonnet-4.6.md](./02-deep-analysis-claude-sonnet-4.6.md) (完了) |
| 3 | step3_technical_roadmap | technical-strategy | gpt-5.3-codex | [03-technical-strategy-gpt-5.3-codex.md](./03-technical-strategy-gpt-5.3-codex.md) (完了) |
| 4 | step4_feasibility_critique | critic-review | claude-sonnet-4.6 | [04-critic-review-claude-sonnet-4.6.md](./04-critic-review-claude-sonnet-4.6.md) (完了) |
| 5 | step5_final_synthesis | integration-review | claude-opus-4.6 | [05-integration-review-claude-opus-4.6.md](./05-integration-review-claude-opus-4.6.md) (完了) |

## 各ステップの要約

### 01-information-gathering-gemini-3-pro-preview.md

# Research Report: EU Cyber Resilience Act Impact on Japan Semiconductor Sector

## ## Executive Summary（概要）
EUサイバーレジリエンス法（CRA）は、デジタル要素を持つすべての製品にセキュリティ要件を義務付ける規制であり、日本の半導体製造装置メーカーにも甚大な影響を及ぼします。特に「重要製品」への該当可能性、SEMI E187標準とのギャップ（特に報告義務とライフサイクル管理）、およびレガシーOSを抱える長寿命製品のサポートが主要な課題として特定されました。

## ## Scope & Critical Products（適用範囲と重要製品定義）

- **Claim（主張）**: 半導体製造装置はCRAの適用対象であり、その制御システムは「重要製品（Important/Critical Products）」に分類される可能性が高い。
- **Evidence（根拠）**:
    - CRAは「デジタル要素を持つ製品（products with digital elements）」すべてに適用され、半導体製造装置も例外ではありません。
    - "Commission Implementing Regulation (EU) 2025/2392" および Annex I/II（旧Annex III/IV）において、産業用オートメーションおよび制御システム（IACS）、PLC、マイクロコントローラなどが「重要製品」としてリストされています。半導体製造装置の制御ユニットやネットワーク機能を持つコンポーネントはこれらに該当し、第三者認証を含む厳格な適合性評価が求められる可能性があります。
- **Source（情報源）**:
    - [official_document/tier1] [Regulation (EU) 2024/2847 (Cyber Resilience Act)](https://eur-lex.europa.eu/eli/reg/2024/2847/oj/eng) (2025-03-14) — primary: true
    - [official_document/tier1] [Commission Implementing Regulation (EU) 2025/2392](https://eur-lex.europa.eu/legal-content/EN/TXT/PDF/?uri=OJ:L_202502392) (2025-03-14) — primary: true
- **Confidence（確信度）**: high

## ## SEMI E187 Alignment & Gaps（SEMI E187との整合性とギャップ）

- **Claim（主張）**: SEMI E187への準拠だけではEU CRAの法的要件を完全には満たせない。特に「インシデント報告」「SBOM」「ライフサイクル管理」に明確なギャップが存在する。
- **Evidence（根拠）**:

_...（全46行。詳細は[ファイル本体](./01-information-gathering-gemini-3-pro-preview.md)を参照）_

### 02-deep-analysis-claude-sonnet-4.6.md

# Deep-Analysis: 日本半導体製造装置メーカーの開発プロセスとEU CRA要求事項のギャップ分析

## Overview（分析概要）

本分析は、日本の半導体製造装置メーカー（以下「装置メーカー」）が直面するEU Cyber Resilience Act（CRA）対応上の構造的矛盾を、開発・運用プロセスの実態とCRA法的要求事項の照合を通じて深層分析するものである。単なる技術的ギャップにとどまらず、**「装置品質保証の文化的前提」と「CRAの安全設計哲学」の根本的な対立**が存在することを示す。

---

## Gap 1: 変更管理とパッチ提供義務の時間軸矛盾（The Time-Axis Contradiction）

### 1.1 CRAが求める「迅速なパッチ提供」

- **Claim（主張）**: CRAは脆弱性確認後「速やかに（without undue delay）」セキュリティアップデートを提供することを義務付けており、業界慣行からこれは概ね数週間以内を意味すると解釈されている。
- **Evidence（根拠）**: CRA Annex I Part II（脆弱性ハンドリング要件）において、メーカーは「脆弱性を遅滞なく対処し、ユーザーにセキュリティアップデートを無償で提供」することが明記されている。さらにArticle 14に基づき、**積極的に悪用されている脆弱性については24時間以内にENISAへの速報、72時間以内に詳細通知、是正措置が利用可能になってから14日以内に最終報告**が義務化される（2026年9月11日施行）。
- **Source（情報源）**:
  - [official_document/tier1] [Regulation (EU) 2024/2847 Annex I Part II](https://eur-lex.europa.eu/eli/reg/2024/2847/oj/eng) (2026-03-14) — primary_source: true
  - [official_document/tier1] [Cyber Resilience Act Article 14 - Reporting Obligations](https://cyber-resilience-act.com/cra/chapter-2/article-14/) (2026-03-14) — primary_source: true
  - [official_document/tier1] [EU Digital Strategy - CRA Reporting Obligations](https://digital-strategy.ec.europa.eu/en/policies/cra-reporting) (2026-03-14) — primary_source: true
- **Confidence（確信度）**: high


_...（全222行。詳細は[ファイル本体](./02-deep-analysis-claude-sonnet-4.6.md)を参照）_

### 03-technical-strategy-gpt-5.3-codex.md

主要な一次情報を先に固めます。CRA条文、SBOM/VEX標準、OT向け更新・遠隔保守の公的ガイドを並行収集します。

取得できた根拠は十分です。次に、要件どおり「主張→根拠→情報源→確信度」で戦略案を組み立て、最後にソース欠落がないかを点検します。

# Technical Strategy for japan-semiconductor-cra（日本半導体装置ソフトウェア開発企業向けEU CRA技術戦略）

## Findings（主要な発見）

- **Claim（主張）**: CRAの報告期限（24h/72h/14日）と、半導体装置の現場適用リードタイム（数週間〜数か月）は、単一の変更管理フローでは衝突しやすい。  
- **Evidence（根拠）**: CRA Article 14は早期警戒・詳細通知・最終報告の期限を明示。前段分析では、ECO/IQOQ/PQ/顧客承認を含む長い適用サイクルが示された。  
- **Source（情報源）**:  
  - [official_document/tier1] [Regulation (EU) 2024/2847](https://eur-lex.europa.eu/eli/reg/2024/2847/oj/eng) (2026-03-14) — primary: true  
  - [official_document/tier1] [CRA reporting obligations (EU Digital Strategy)](https://digital-strategy.ec.europa.eu/en/policies/cra-reporting) (2026-03-14) — primary: true  
  - [unknown] 装置メーカーの現場変更リードタイム実務値（前段分析の業界推定）(unknown) (unknown) — primary: false  
- **Confidence（確信度）**: medium（法令要件はhigh、現場リードタイムの一般化は推測を含む）

- **Claim（主張）**: SBOMと脆弱性ハンドリングは「任意のベストプラクティス」ではなく、CRA適合の中核要件。  
- **Evidence（根拠）**: Annex I Part IIは脆弱性管理・コンポーネント把握を要求。ENISA/JRCの標準マッピングでもSBOM関連が主要ギャップとして扱われる。  
- **Source（情報源）**:  
  - [official_document/tier1] [Regulation (EU) 2024/2847](https://eur-lex.europa.eu/eli/reg/2024/2847/oj/eng) (2026-03-14) — primary: true  

_...（全169行。詳細は[ファイル本体](./03-technical-strategy-gpt-5.3-codex.md)を参照）_

### 04-critic-review-claude-sonnet-4.6.md

# Critic Review（批判的レビュー）

## Validation Summary（検証サマリー）

| チェック項目 | 件数 | 備考 |
|---|---|---|
| Claim without Evidence（根拠なし主張） | **0件** | すべての主張に何らかの根拠記述あり |
| Evidence without verifiable Source（検証可能な出典なし） | **5件** | [unknown]ソースが5箇所（後述） |
| Conclusion without Confidence（確信度なし結論） | **0件** | すべてhigh/medium/lowが付与されている |
| Confidence判定の基準逸脱 | **3件** | [unknown]ソース依存のmediumはlowが妥当 |

**全体のValidation結果: PARTIAL**

[unknown]ソースが5件存在するが、明示的に`[unknown]`と表記されているため技術的には規則に従っている。ただし、これらのソースが支える主張の確信度判定（medium）が過大評価であるという問題が残る。

---

## 論理矛盾・整合性の問題

### 矛盾①：バイナリ解析SBOMの有効性を過大評価

_...（全200行。詳細は[ファイル本体](./04-critic-review-claude-sonnet-4.6.md)を参照）_

### 05-integration-review-claude-opus-4.6.md

# 統合レビュー

## Executive Summary（総括）

日本半導体装置関連ソフトウェア開発企業のEU CRA対策は、**2026年9月11日の脆弱性報告義務開始まで残り約6か月**という切迫した状況にある。批判的レビューで検出された5つの重大矛盾（バイナリ解析SBOM精度の過大評価、24時間PSIRT体制コスト未計上、サプライヤSBOM能力の楽観視、エアギャップ環境の法解釈未確定、結論のConfidence過大判定）を統合修正し、**「報告義務先行→SBOM段階構築→適合性評価完了」の3フェーズロードマップ**を確定させた。最大のリスクは報告期限の不遵守（罰金最大€15M/全世界売上2.5%）であり、経営判断として**PSIRT体制構築を最優先投資対象**と位置付ける。製品ライフサイクル（15〜25年）とCRA支援期間の乖離、外為法とENISA報告の交差リスクは法務確認を並行実施する必要がある。

## Research Question（調査の問い）

日本の半導体装置関連ソフトウェア開発企業が、EU Cyber Resilience Act（Regulation (EU) 2024/2847）に対して、批判的レビューで指摘された戦略上の欠陥を修正した上で、**法定期限に間に合う現実的かつ段階的な対策ロードマップと、経営層が判断可能なリスク受容基準・優先順位**をどのように確定すべきか。

## Findings（主要な発見）

### Finding 1: 脆弱性報告義務は2026年9月開始——残り6か月で24/7 PSIRT体制が必須

- **Claim（主張）**: CRA Article 14に基づき、2026年9月11日から製造者は能動的に悪用された脆弱性を**認知後24時間以内にENISA Single Reporting Platform経由で早期警告**を提出しなければならない。72時間以内に詳細報告、修正後14日以内に最終報告が必要である。日本⇔EU間の時差（JST=CET+8h）を考慮すると、EU営業時間内の通報トリガーは日本の深夜〜早朝に発生するため、**24/7対応体制なしでは24時間SLAを物理的に達成できない**。
- **Evidence（根拠）**: CRA Article 14は「認知時点からの絶対時間」で24時間を計算する。ENISA Single Reporting Platformが報告窓口となり、製造者のEU域内の主要拠点所在国のCSIRTとENISAに同時通知される。非EU製造者はEU authorised representative（Article 17）または輸入者経由で報告する。
- **Source（情報源）**:
  - [SRC-001] [official_document/tier1] [Regulation (EU) 2024/2847](https://eur-lex.europa.eu/eli/reg/2024/2847/oj/eng) (2026-03-14) — primary: true
  - [SRC-002] [official_document/tier1] [CRA Reporting Obligations - EU Digital Strategy](https://digital-strategy.ec.europa.eu/en/policies/cra-reporting) (2026-03-14) — primary: true
  - [SRC-003] [official_document/tier1] [CRA Article 14 Full Text](https://www.european-cyber-resilience-act.com/Cyber_Resilience_Act_Article_14.html) (2026-03-14) — primary: true

_...（全340行。詳細は[ファイル本体](./05-integration-review-claude-opus-4.6.md)を参照）_

---

## Trust Summary（信頼性サマリー）

### Validation Result（検証結果）: **PARTIAL**

| 指標 | 値 |
|------|-----|
| 総 Claim（主張）数 | 74 |
| Evidence（根拠）なし | 0 |
| Source（情報源）なし | 10 |
| Confidence（確信度）なし | 0 |

### Confidence Distribution（確信度分布）

| Level（レベル） | 件数 |
|-------|------|
| High（高） | 38 |
| Medium（中） | 29 |
| Low（低） | 7 |

### Source Reliability（情報源の信頼性分布）

| Tier（階層） | 件数 | 説明 |
|------|------|------|
| Tier 1（公式） | 54 | 公式文書・法令・政府機関・標準化団体 |
| Tier 2（高信頼） | 7 | 企業公式・研究機関・学術論文 |
| Tier 3（参考） | 4 | 技術ブログ・専門メディア・業界記事 |
| Tier 4（コミュニティ） | 9 | フォーラム・コミュニティ・SNS |

一次情報（Primary Source）: 56件 / 二次情報: 18件

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
