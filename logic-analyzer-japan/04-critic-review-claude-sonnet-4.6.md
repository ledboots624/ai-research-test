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
- **WANT（希望）**: 1件 — 10xサンプリング（WANT として区分されているが実質 SHOULD と区別不明確）

**義務レベル解釈リスクの検証:**

前ステップは「本件の SHOULD は法令準拠用語ではなく測定品質ガイドであり、EU CRA 等の規制 SHOULD とは重みが異なる」と末尾に補注しているが、この断り書きが Summary Table のセル内に含まれておらず、表を単独で読んだ場合に規格的 SHOULD として誤読されるリスクがある。

**具体的問題点:**
- Saleae FAQ（S2）は `tier2 / primary_source: true` とマークされているが、これは一民間企業の FAQ であり、法令・標準化団体の文書ではない。IEC や IEEE の測定ガイドラインが引用されていれば SHOULD/WANT の重みを客観的に裏付けられるが、現状は単一民間企業ガイドへの依存。
- 「SHOULD: fs >= 4fclock」の Standard Reference Mapping が「partial（Saleae FAQ）」とされているが、独立した学術的または標準化団体由来の 4x 推奨根拠が示されていない。

**Source（情報源）:**
[community/tier4] [sigrok-devel LA2016 bug fixes](https://sourceforge.net/p/sigrok/mailman/sigrok-devel/thread/7f43da14-26bc-e012-de4d-0c56903affe7%40gmx.net/) (access_date: 2026-03-14) — primary: false

---

## 規格間ギャップ（Standard Mapping Gaps）

| 規格 | マッピング | 問題箇所 | リスク | 対応策 |
|---|---|---|---|---|
| Shannon 1949 ↔ Saleae 4x FAQ | gap | 「2x = MUST, 4x = SHOULD」とするが 4x の独立根拠が単一民間 FAQ のみ | SHOULD の重みが過大評価される可能性 | IEEE や JEDEC 測定ガイドラインで裏付けを補完 |
| UM10204 Fm+ timing ↔ LA2016/DSLogic Plus 実測 | gap | 規格上の Δtr 計算は理論式由来であり、実際の入力容量 12–13pF は仕様値であって個体差・ケーブル長を含まない | 実環境では計算値より悪化する可能性 | 実測値（third-party レビュー）での検証が必要 |
| STM32H7 SPI 100MHz超 ↔ 測定器適格性 | partial | STM32H743 の SPI 上限を一般化しているが、製品バリアント・クロック設定依存 | confidenceをmediumとしているが表上は高速SPI不適格の決定的根拠として使われている | 具体的なバリアント・設定を特定した上で引用 |

---

## 論理矛盾・整合性の問題

### 問題1: DSLogic Plus Buffer 400MHz への Confidence 過大評価
**該当箇所:** Sampling Fit Analysis テーブル、Device Verdicts（DSLogic Plus）  
**問題:** Buffer 400MHz に `high` confidence が付与されているが、複数の第三者レビューによると、付属フライワイヤーを用いた実環境では 150–200MHz 超の信頼性が大幅に低下する。ターミネーション抵抗なしのプローブは 100MHz 超で信号劣化を引き起こすことが実測で確認されている。400MHz は理論上の値であり `medium` が適切。  
**重大度:** **high** — 購入意思決定に直結する誤誘導

**Claim（主張）:** DSLogic Plus の 400MHz Buffer 達成は付属プローブ環境では理論値に留まり、実用上の有効帯域は約 150–250MHz に限られる  
**Evidence（根拠）:** フライワイヤー先端にターミネーション抵抗がなく、高速信号で反射・容量負荷が発生。EEVblog/Hackaday 第三者レビューが「typical electronics bench setup では max spec での安定取得は困難」と指摘  
**Source（情報源）:**
- [community/tier4] [EEVblog DSLogic Logic Analyzer forum thread](https://www.eevblog.com/forum/testgear/dslogic-logic-analyzer-from-dreamsourcelab-(16-ch-256-mbits-400-mhz)/) (access_date: 2026-03-14) — primary: false
- [industry_report/tier3] [DSLogic Plus Teardown And Review – Hackaday](https://hackaday.com/2017/08/24/dslogic-plus-teardown-and-review/) (access_date: 2026-03-14) — primary: false  
**Confidence（確信度）:** medium

---

### 問題2: LA1010 の多チャンネル時サンプリングレート記述の不整合
**該当箇所:** Sampling Fit Analysis テーブル（「4ch想定で50MHz、16chで16MHz」）  
**問題:** Kingst 公式仕様（S10）ではLA1010の最大サンプリングレートは 100MHz と記載されており、「4ch想定で50MHz」の根拠が明示されていない。チャンネル数依存の帯域縮退は実装依存であり、sigrok ドライバーの対応状況と合わせて検証が必要。sigrok コミュニティでは LA1010 のドライバーサポートが `experimental` レベルであることが報告されており、Sampling Fit テーブルの数値の信頼性に影響する。  
**重大度:** **medium**

**Claim（主張）:** LA1010 の sigrok ドライバーサポートは 2023 年時点で実験的段階であり、Sampling Fit テーブルに示された数値は未検証の可能性がある  
**Source（情報源）:**
- [community/tier4] [sigrok-devel kingst la1010 thread](https://sourceforge.net/p/sigrok/mailman/sigrok-devel/thread/20230131184537.GC4396%40book.gsilab.sittig.org/) (access_date: 2026-03-14) — primary: false  
**Confidence（確信度）:** medium

---

### 問題3: Δtr 計算式の自己導出と独立検証不在
**該当箇所:** Electrical Characteristics セクション（「4.7kΩプルアップ時、12pFで約48ns追加」）  
**問題:** UM10204 の式を変形した自己導出計算であり、独立した測定データやシミュレーション結果による検証が示されていない。`high` confidence が付与されているが、式の変形過程の妥当性・前提条件（プルアップ抵抗4.7kΩ固定、既存 Cbus 無視）が未検証。  
**重大度:** **low**（定性的方向性は正しいと判断できる）

---

## エビデンス不足

### 不足1: macOS / Apple Silicon 互換性リスク（最重大ギャップ）
**主張内容:** 前ステップは技術適格性を論じているが、Mac Mini M4 での実際の動作可否についてのエビデンスが一切なし  
**不足しているエビデンス:**
- LA2016 公式ソフトは x86_64 バイナリのみ。Apple Silicon での動作には Rosetta 2 経由、または sigrok/PulseView が必要
- sigrok 経由使用時、KingstVIS から**毎接続ごとに**プロプライエタリファームウェアを手動抽出・配置する作業が必要
- PulseView はホットアンプラグ時にクラッシュする既知のバグがある
- **ハードウェア制限: エッジトリガーは1チャンネル分のみ対応**。GUI 上で複数設定すると未定義動作を引き起こす

**Source（情報源）:**
- [community/tier4] [Kingst LA2016 – OpenTraceLab](https://opentracelab.github.io/website/hardware/logic-analyzer/kingst-la2016/) (access_date: 2026-03-14) — primary: false
- [community/tier4] [AnotherWayIn/kingst-sigrok-tool – GitHub](https://github.com/AnotherWayIn/kingst-sigrok-tool) (access_date: 2026-03-14) — primary: false
- [official_document/tier2] [Kingst LA2016 – sigrok wiki](https://sigrok.org/wiki/Kingst_LA2016) (access_date: 2026-03-14) — primary: false  
**Confidence（確信度）:** high

---

### 不足2: DSLogic Plus の macOS Sonoma / Apple Silicon 既知バグ
**主張内容:** DSLogic Plus が高速 SPI の最有力候補として推奨されているが、macOS 固有の重大バグが言及なし  
**不足しているエビデンス:**
- macOS Sonoma 14.6.1（Mac Mini M2 Pro）において DSView v1.3.2 で **SPI プロトコルデコードが 1 クロック分ずれる**不具合が報告済み
- "device is busy, switch failed" / "failed to start session" エラーが Apple Silicon Mac で複数バージョンにわたって報告されている
- DSView は**クローズドソース FPGA ビットストリーム**を使用しており、「オープンソースハードウェア」という表記は実態と乖離している。将来の macOS 更新への対応が保証されない

**Source（情報源）:**
- [community/tier4] [Protocol decode problem in DSView v1.3.2 – GitHub Issue #832](https://github.com/DreamSourceLab/DSView/issues/832) (access_date: 2026-03-14) — primary: false  
  - community_score: discussion_age: "active 2024–2025", participant_count: "~10", expert_presence: false, reproducibility: true
- [community/tier4] [DSLogicPlus / Mac Os: "the device is busy" – GitHub Issue #676](https://github.com/DreamSourceLab/DSView/issues/676) (access_date: 2026-03-14) — primary: false
- [industry_report/tier3] [DSLogic Plus Teardown And Review – Hackaday](https://hackaday.com/2017/08/24/dslogic-plus-teardown-and-review/) (access_date: 2026-03-14) — primary: false  
**Confidence（確信度）:** high

---

### 不足3: USB グランドループ・アイソレーション問題
**主張内容:** 電気特性セクションでプローブ負荷（容量・抵抗）を論じているが、USB 接続に起因するグランドループ問題が未言及  
**不足しているエビデンス:**
- USB ロジックアナライザーは USB グランドを PC シャーシグランドと共有する。STM32/ESP32 ボードが別電源（ACアダプター・独立電源）から給電されている場合、グランド電位差によりノイズ混入・誤トリガー・最悪ハードウェア損傷が発生しうる
- USB アイソレーター未使用時に DUT やアナライザーを破損させた実例がコミュニティで複数報告されている
- Mac Mini M4 は電源 AC ラインを介してシャーシグランドと接続されるため、リスクが高い構成になりやすい

**Source（情報源）:**
- [official_document/tier2] [Electrical Isolation Suggestions – Saleae Support](https://support.saleae.com/getting-help/troubleshooting/technical-faq/suggestions-for-electrical-isolation) (access_date: 2026-03-14) — primary: false
- [community/tier4] [Blew up my logic analyzer and device – SparkFun Community](https://community.sparkfun.com/t/blew-up-my-logic-analyzer-and-device-ground-issues/36399) (access_date: 2026-03-14) — primary: false
  - community_score: discussion_age: "5+ years", participant_count: "~20", expert_presence: false, reproducibility: false  
**Confidence（確信度）:** high（リスクの実在性は high、発生確率は medium）

---

### 不足4: 日本国内調達可能性の未検証
**主張内容:** 前ステップは技術性能を評価しているが、Amazon.co.jp・秋月電子・マルツ等での実際の取り扱い・在庫・価格が一切検証されていない  
**不足しているエビデンス:** 各候補の国内正規流通経路、偽物・非正規品混入リスク  
**Confidence（確信度）:** medium（問題の実在性は明確だが具体的影響度は不明）

---

### 不足5: FX2 系クローンの品質管理・偽物リスク
**主張内容:** FX2 系が「I2C/UART や低速 SPI の学習用途には有効」と評価されているが、品質のばらつきリスクが過小評価  
**不足しているエビデンス:** FX2 系は AliExpress・Amazon を経由した無保証クローンが大多数。同一外観でも内部チップが異なり sigrok 互換性が保証されない個体が存在する。一部では GND ピンが信号ピンとして機能するなどの組み立てミスが報告されている  
**Source（情報源）:**
- [community/tier4] [Best Cheap knockoff Logic Analyzer – EEVblog Forum](https://www.eevblog.com/forum/testgear/best-cheap-knockoff-logic-analyzer/) (access_date: 2026-03-14) — primary: false  
**Confidence（確信度）:** medium

---

## 潜在的バイアス

### バイアス1: 技術スペック中心主義バイアス
**種類:** 確証バイアス（スペックシート優先）  
**該当箇所:** Sampling Fit Analysis、Device Verdicts 全体  
**問題:** ハードウェアスペックの比較に終始し、実使用上のソフトウェア品質・ドライバー安定性・セットアップ複雑性（macOS でのファームウェア抽出手順等）が評価軸に含まれていない。MCU デバッグの現場では「接続すれば動く」という信頼性も重要な評価軸である  
**影響度:** **high** — DSLogic Plus の推奨の妥当性に直接影響

### バイアス2: 選択肢の固定化バイアス
**種類:** フレーミングバイアス  
**該当箇所:** 4製品（LA2016/LA1010/DSLogic Plus/FX2系）への分析範囲の限定  
**問題:** 選定根拠が明示されていない。3万円以下で日本国内入手可能な他候補（例: Cypress FX3 ベース製品、Saleae Logic 8 廉価版、OLS 等）が評価対象外となっている。候補選定プロセス自体が不透明  
**影響度:** medium

### バイアス3: オープンソース好意バイアス
**種類:** アドボカシーバイアス  
**該当箇所:** DSLogic Plus の評価（「オープンソース対応」の言及）  
**問題:** DSLogic Plus は DSView ソフトウェアは GPLv3 だが **FPGA ビットストリームはクローズドソース**。「オープンソースハードウェア」という評価は不正確であり、長期サポートリスクを過小評価させる可能性がある  
**影響度:** medium

---

## 情報ギャップ

調査されていない重要な観点を以下に列挙する:

| # | 未調査領域 | 重要度 | 影響する意思決定 |
|---|---|---|---|
| 1 | **macOS Sequoia（15.x）での各製品動作確認** | **critical** | LA2016・DSLogic Plus 両方の推奨の妥当性 |
| 2 | **日本国内販売価格・流通経路の現況** | **critical** | 予算3万円以内の達成可能性 |
| 3 | **USB アイソレーター追加コストの試算** | high | 実際の総コスト（DUT 保護に推奨されるが、2,000〜5,000円の追加費用） |
| 4 | **KingstVIS ソフトウェアの macOS 対応状況** | high | LA2016 使用における sigrok 必須度の確認 |
| 5 | **DSLogic Plus の現行 DSView バージョンと Apple Silicon ネイティブ対応状況** | high | 推奨の妥当性 |
| 6 | **長期サポートリスク（Kingst・DreamSourceLab の企業持続性）** | medium | 5年スパンでの使用可能性 |
| 7 | **La1010 の sigrok ドライバー完成度（2025年現在）** | medium | LA1010 推奨の妥当性 |
| 8 | **各デバイスの ESD 耐量の実測評価** | medium | 現場デバッグでの事故耐性 |
| 9 | **3.3V/1.8V 混在環境での閾値設定精度** | medium | 近年の MCU（STM32L4系等1.8V IO）への適用性 |
| 10 | **Windows 環境での動作比較（Mac との対比）** | low | Mac 限定問題か汎用問題かの切り分け |

---

## Source Coverage Summary（情報源カバレッジ）

| source_type | 件数 | 備考 |
|---|---|---|
| official_document | 8件（S1〜S9, S10, S11相当） | 主要な技術根拠をカバー |
| academic_paper | 1件（S1: Shannon 1949） | |
| community_data | 2件（S8, S12） | community_score 付与済み |
| industry_report | 1件（S13） | |
| news_article | 0件 | |

| reliability_tier | 件数 |
|---|---|
| tier1（公式標準・規格） | 1件（S3: NXP UM10204） |
| tier2（企業公式・研究機関） | 9件 |
| tier3（技術ブログ・専門メディア） | 1件（S13） |
| tier4（フォーラム・コミュニティ） | 2件（S8, S12） |

**primary_source 比率:** 9/13 = **69%**（true が9件）

**多様性評価:**  
- ハードウェアスペックの一次情報カバレッジは高品質  
- **ソフトウェア品質・実運用レビュー・macOS 実績のソースがゼロ** — 実際の購入リスク評価に必要なコミュニティレベル情報（tier3/tier4）が著しく不足  
- 日本語ソース（秋月電子、マルツ等の国内流通情報）が皆無  
- 独立した第三者ベンチマーク・実測レビュー（tier3）がほぼゼロ

---

## Unresolved Issues（未解決事項）

1. **LA2016 on macOS Sequoia / M4 の動作保証有無** — Rosetta 2 経由動作の安定性は M4 Mac Mini で未確認。sigrok の libusb 実装が Apple Silicon の USB スタック変更に追従できているか不明  
2. **DSLogic Plus の DSView v1.3.2 における SPI デコードバグの修正状況** — GitHub Issue #832 はアクティブだが解決済みか不明  
3. **LA1010 sigrok ドライバーの実用完成度** — 2023 年時点で experimental とされており、Sampling Fit テーブルに示した 4ch/50MHz の実測値が実際に達成されるかは未検証  
4. **DSLogic Plus の FPGA ビットストリームの macOS 更新追随スケジュール** — DreamSourceLab の開発体制が公開されておらず、次期 macOS での動作保証が不明  
5. **FX2 系クローンの個体品質の最低保証ライン** — バッチごとの品質差が大きく、sigrok 互換性を維持した正規品の見分け方が不明確  

---

## 改善提案

優先度順に列挙:

**P1（購入前に必須）:**
1. **macOS Sequoia + Apple Silicon 動作実績を専門コミュニティで確認する** — sigrok メーリングリスト、DreamSourceLab GitHub Issues、Reddit r/embedded での実績報告を LA2016・DSLogic Plus それぞれ個別に調査。評価結果を Sampling Fit 判定の前提として組み込む
2. **DSLogic Plus の DSView SPI デコードバグ（Issue #832）の解決状況を確認してから最終推奨に含める** — Confidence を `medium` に格下げするか、「macOS Sonoma 以降での SPI 用途には要確認」と明記する必要がある

**P2（リスク評価の精度向上）:**
3. **USB グランドループリスクを全候補の制限事項として明記し、USB アイソレーター追加を推奨事項に加える** — Saleae の公式ガイドを tier2 ソースとして引用可能。総コスト（本体 + アイソレーター）で3万円枠内に収まるか検証
4. **DSLogic Plus の「オープンソース」表記を訂正する** — FPGA ビットストリームはクローズドソースであることを明記し、長期サポートリスクを Device Verdicts に追加
5. **LA1010 の Confidence を `high` から `medium` に格下げし**、sigrok ドライバーが実験的段階であることを Sampling Fit テーブルの注記として追加

**P3（網羅性の補完）:**
6. **日本国内流通情報を1ステップとして追加する** — Amazon.co.jp での現在の取り扱い品番・出品者・価格帯、秋月電子・マルツでの取扱状況を確認し、偽物混入リスクを評価
7. **独立した第三者ベンチマーク（tier3以上）を各候補に最低1件追加する** — 特に「DSLogic Plus Buffer 400MHz の実効帯域」と「LA1010 多チャンネル時の実効サンプリングレート」については公式スペックへの過度な依存を是正する
8. **1.8V IO 対応（STM32L4 等）の閾値設定可否を各製品で確認する** — 前ステップの電気特性評価は 3.3V/5V 系を暗黙の前提としており、低電圧 IO への適用可能性が未評価
