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
- **Source（情報源）**:
  - [official_document/tier2] [Kingst LA2016 仕様 — sigrok wiki](https://sigrok.org/wiki/Kingst_LA2016) (2026-03-14) — primary: true
  - [official_document/tier2] [Amazon.co.jp Kingst LA2016 商品ページ](https://www.amazon.co.jp/Kingst-%E6%9C%80%E5%A4%A7%E3%82%B5%E3%83%B3%E3%83%97%E3%83%AB%E3%83%AC%E3%83%BC%E3%83%88-%E8%8B%B1%E8%AA%9E%E3%82%BD%E3%83%95%E3%83%88%E3%82%A6%E3%82%A7%E3%82%A2%E7%94%A8-%E3%83%87%E3%83%BC%E3%82%BF%E3%82%AD%E3%83%A3%E3%83%97%E3%83%81%E3%83%A3-%E3%83%A1%E3%83%A2%E3%83%AA%E3%82%A2%E3%83%8A%E3%83%A9%E3%82%A4%E3%82%B6/dp/B0DQ4GSXP3) (2026-03-14) — primary: false
- **Confidence（確信度）**: high
- **Evidence Strength（根拠の強さ）**: strong — 公式仕様と複数の流通価格データで裏付け
- **Requirement Level（義務レベル）**: SHOULD — 4x オーバーサンプリングは実務推奨レベル

---

### Finding 2: macOS Apple Silicon 環境では sigrok/PulseView が事実上の統一プラットフォーム

- **Claim（主張）**: Mac Mini M4 で利用可能な候補製品すべて（LA2016、LA1010、DSLogic Plus、FX2 系）において、macOS での利用は sigrok/PulseView が中心となる。公式 DMG は x86_64 のみだが Rosetta 2 で動作し、`takesako/homebrew-sigrok` タップを用いた ARM64 ネイティブビルドも技術的に可能。Kingst 製品は初回セットアップ時に KingstVIS からファームウェアを抽出して配置する作業（1回限り）が必要。
- **Evidence（根拠）**: sigrok 公式サイトの macOS ページおよびダウンロードページでは x86_64 DMG のみ提供。Homebrew の libsigrok には ARM64 ボトルが存在。非公式タップ（takesako/homebrew-sigrok）で PulseView の ARM64 ネイティブビルドが可能。ファームウェア抽出には AnotherWayIn/kingst-sigrok-tool が利用可能。
- **Source（情報源）**:
  - [official_document/tier2] [sigrok Mac OS X ページ](https://sigrok.org/wiki/Mac_OS_X) (2026-03-14) — primary: true
  - [community_data/tier4] [takesako/homebrew-sigrok — GitHub](https://github.com/takesako/homebrew-sigrok) (2026-03-14) — primary: false — community_score: discussion_age: "2+ years", participant_count: "~5", expert_presence: true, reproducibility: true
  - [community_data/tier4] [AnotherWayIn/kingst-sigrok-tool — GitHub](https://github.com/AnotherWayIn/kingst-sigrok-tool) (2026-03-14) — primary: false
  - [official_document/tier2] [libsigrok Homebrew Formulae](https://formulae.brew.sh/formula/libsigrok) (2026-03-14) — primary: true
- **Confidence（確信度）**: high
- **Evidence Strength（根拠の強さ）**: strong — 公式ビルドシステムと Homebrew レジストリで確認済み

---

### Finding 3: DSLogic Plus は macOS 環境での SPI デコードに既知バグがあり条件付き推奨

- **Claim（主張）**: DSLogic Plus（16ch/400MHz バッファ）は約13,000〜17,500円（海外通販・輸入代理店）で購入可能だが、macOS Sonoma 上の DSView v1.3.2 で SPI プロトコルデコードが1クロックずれるバグ（GitHub Issue #832）が報告されている。v1.3.3 で修正されたとの報告があるものの、macOS 向けの公式最新バイナリは v1.3.2 のまま。加えて Apple Silicon Mac での "device is busy" エラー（Issue #676）が複数バージョンにわたり報告されており、Mac 環境での安定性に懸念が残る。
- **Evidence（根拠）**: GitHub Issue #832 に再現手順と複数ユーザーの確認あり。CSDN 記事が v1.3.3 での修正を報告するも macOS 向けバイナリの提供未確認。Issue #676 は 2024〜2025 年にわたりアクティブ。FPGA ビットストリームはクローズドソースであり、「オープンソースハードウェア」表記は不正確。
- **Source（情報源）**:
  - [community_data/tier4] [Protocol decode problem — DSView Issue #832](https://github.com/DreamSourceLab/DSView/issues/832) (2026-03-14) — primary: false — community_score: discussion_age: "active 2024–2025", participant_count: "~10", expert_presence: false, reproducibility: true
  - [community_data/tier4] [Device is busy — DSView Issue #676](https://github.com/DreamSourceLab/DSView/issues/676) (2026-03-14) — primary: false
  - [industry_report/tier3] [DSView SPI デコードバグ分析 — CSDN](https://blog.csdn.net/gitblog_07113/article/details/148888506) (2026-03-14) — primary: false
  - [official_document/tier2] [DSView ダウンロードページ](https://www.dreamsourcelab.com/download/) (2026-03-14) — primary: true
- **Confidence（確信度）**: high
- **Evidence Strength（根拠の強さ）**: strong — 複数の独立報告と再現可能なバグ

---

### Finding 4: Kingst LA1010 は中価格帯の妥協的選択肢だが sigrok ドライバーが未成熟

- **Claim（主張）**: Kingst LA1010（16ch/最大100MHz）は Amazon.co.jp で約13,500〜14,000円で購入可能であり、予算をさらに抑えたい場合の次善策となる。ただし sigrok ドライバーサポートは 2023 年時点で experimental レベルであり、多チャンネル使用時のサンプリングレート縮退（16ch 時 16MHz）の実測検証が不足している。
- **Evidence（根拠）**: Amazon.co.jp 価格データ。Kingst 公式仕様では最大 100MHz。sigrok-devel メーリングリストでの LA1010 ドライバー議論が experimental 段階であることを示唆。チャンネル数依存の帯域縮退は実装依存であり、sigrok 経由での実測値が公式スペックと一致する保証がない。
- **Source（情報源）**:
  - [official_document/tier2] [Amazon.co.jp LA1010 商品ページ](https://www.amazon.co.jp/LA1010-%E3%82%B5%E3%83%B3%E3%83%97%E3%83%AB%E3%83%AC%E3%83%BC%E3%83%88-16%E3%83%81%E3%83%A3%E3%83%B3%E3%83%8D%E3%83%AB-10B%E3%82%B5%E3%83%B3%E3%83%97%E3%83%AB-%E3%83%87%E3%83%90%E3%83%83%E3%82%B0%E3%83%84%E3%83%BC%E3%83%AB/dp/B078Y8BKKK) (2026-03-14) — primary: false
  - [community_data/tier4] [sigrok-devel LA1010 thread](https://sourceforge.net/p/sigrok/mailman/sigrok-devel/thread/20230131184537.GC4396%40book.gsilab.sittig.org/) (2026-03-14) — primary: false — community_score: discussion_age: "3+ years", participant_count: "~5", expert_presence: true, reproducibility: false
- **Confidence（確信度）**: medium
- **Evidence Strength（根拠の強さ）**: moderate — sigrok ドライバーの実用完成度が未検証

---

### Finding 5: FX2 系クローンは学習用サブ機として有効だが品質リスクあり

- **Claim（主張）**: CY7C68013A（FX2LP）ベースの 8ch/24MHz クローンは Amazon.co.jp で830〜2,000円と極めて安価であり、UART や低速 I2C/SPI（〜数 MHz）の基本的なデバッグに使える。sigrok での対応は成熟しており macOS での動作実績も豊富。ただし無保証クローン品であるため個体品質のばらつきが大きく、メイン機としての信頼性には欠ける。
- **Evidence（根拠）**: Amazon.co.jp で複数ブランド（Senzooe, Reland Sun, Jolooyo, Comimark）から出品。sigrok wiki での FX2 サポートは成熟（Tier 1 サポートデバイス）。EEVblog フォーラムで品質ばらつきの報告あり。
- **Source（情報源）**:
  - [official_document/tier2] [Amazon.co.jp CY7C68013A 商品一覧](https://www.amazon.co.jp/EZ-USB-FX2LP-CY7C68013A-%E3%83%AD%E3%82%B8%E3%83%83%E3%82%AF-%E3%82%A2%E3%83%8A%E3%83%A9%E3%82%A4%E3%82%B6%E3%83%BC/dp/B0CDGRQQ9W) (2026-03-14) — primary: false
  - [community_data/tier4] [Best Cheap knockoff Logic Analyzer — EEVblog Forum](https://www.eevblog.com/forum/testgear/best-cheap-knockoff-logic-analyzer/) (2026-03-14) — primary: false — community_score: discussion_age: "5+ years", participant_count: "~50", expert_presence: true, reproducibility: true
- **Confidence（確信度）**: high
- **Evidence Strength（根拠の強さ）**: strong — 長年のコミュニティ実績

---

### Finding 6: ZEROPLUS LAP-C は macOS 非対応のため Mac Mini M4 ユーザーには不適格

- **Claim（主張）**: 秋月電子で取り扱いのある ZEROPLUS LAP-C 16064（16ch/100MHz、約22,800円）は Windows 専用であり、macOS（Intel/Apple Silicon 問わず）のネイティブ対応はない。sigrok にも対応デバイスとして含まれていない。Mac Mini M4 ユーザーが選択すべきではない。
- **Evidence（根拠）**: ZEROPLUS 公式サイトのソフトウェアダウンロードページは Windows 7/8.1/10/11 のみ。sigrok のサポートデバイスリストに LAP-C シリーズは含まれていない。
- **Source（情報源）**:
  - [official_document/tier2] [ZEROPLUS 公式ダウンロード](https://www.zeroplus.com.tw/logic-analyzer_en/technical_support_search.php?model=LAP-C%2816064%29&class1=1) (2026-03-14) — primary: true
  - [official_document/tier2] [秋月電子 LAP-C 16064 商品ページ](https://akizukidenshi.com/catalog/g/g104426/) (2026-03-14) — primary: false
- **Confidence（確信度）**: high
- **Evidence Strength（根拠の強さ）**: strong — 公式情報源で確定

---

### Finding 7: Saleae Logic 8 は予算超過のため候補外

- **Claim（主張）**: Saleae Logic 8 は macOS ネイティブ対応のゴールドスタンダードだが、Amazon.co.jp で約99,934円、秋月電子でも8万円以上であり、3万円予算の3倍超となる。
- **Evidence（根拠）**: Amazon.co.jp、秋月電子、Mouser Japan での販売価格。
- **Source（情報源）**:
  - [official_document/tier2] [Amazon.co.jp Saleae Logic 8](https://www.amazon.co.jp/Saleae-Logic-8%EF%BC%88%E3%83%96%E3%83%A9%E3%83%83%E3%82%AF%EF%BC%89-8%E3%83%81%E3%83%A3%E3%83%B3%E3%83%8D%E3%83%AB%E3%83%AD%E3%82%B8%E3%83%83%E3%82%AF%E3%82%A2%E3%83%8A%E3%83%A9%E3%82%A4%E3%82%B6/dp/B0749G85W2) (2026-03-14) — primary: false
  - [official_document/tier2] [秋月電子 Logic8-Black](https://akizukidenshi.com/catalog/g/g108975/) (2026-03-14) — primary: false
- **Confidence（確信度）**: high
- **Evidence Strength（根拠の強さ）**: strong

---

### Finding 8: USB グランドループ対策は全候補に共通する安全上の SHOULD 要件

- **Claim（主張）**: USB ロジックアナライザーはすべて PC の USB グランドと DUT のグランドを共有するため、異なる電源系統から給電された MCU ボードとの接続時にグランドループが発生するリスクがある。USB アイソレーター（Amazon.co.jp で約1,600〜3,300円）の追加は強く推奨される。LA2016 + USB アイソレーターの組み合わせでも約23,600〜25,300円と3万円以内に収まる。
- **Evidence（根拠）**: Saleae 公式サポートドキュメントが電気的アイソレーションを推奨。SparkFun コミュニティでグランドループによるロジアナ/DUT 破損事例の報告あり。Amazon.co.jp で DSD TECH SH-G01A（ADUM3160/1,500V 絶縁）が約3,300円で購入可能。
- **Source（情報源）**:
  - [official_document/tier2] [Saleae Electrical Isolation Suggestions](https://support.saleae.com/getting-help/troubleshooting/technical-faq/suggestions-for-electrical-isolation) (2026-03-14) — primary: true
  - [community_data/tier4] [Blew up my logic analyzer — SparkFun Community](https://community.sparkfun.com/t/blew-up-my-logic-analyzer-and-device-ground-issues/36399) (2026-03-14) — primary: false — community_score: discussion_age: "5+ years", participant_count: "~20", expert_presence: false, reproducibility: false
  - [official_document/tier2] [DSD TECH SH-G01A — Amazon.co.jp](https://www.amazon.co.jp/DSD-TECH-SH-G01A-%E3%82%A2%E3%82%A4%E3%82%BD%E3%83%AC%E3%83%BC%E3%82%BF%E3%83%BC-ADUM3160%E3%83%81%E3%83%83%E3%83%97%E4%BB%98%E3%81%8D/dp/B093LKN3YH) (2026-03-14) — primary: false
- **Confidence（確信度）**: high
- **Evidence Strength（根拠の強さ）**: strong
- **Requirement Level（義務レベル）**: SHOULD — ハードウェア保護の観点から強く推奨。MUST_NOT ではないが、別電源系統での接続時にはリスクが高い

---

### Finding 9: 日本国内調達においてマーケットプレイス経由の品質リスクが存在する

- **Claim（主張）**: Kingst LA2016・LA1010・DSLogic Plus はいずれも Amazon.co.jp のマーケットプレイス出品者を経由した購入が主流であり、秋月電子やマルツなどの国内正規代理店での取り扱いはない（秋月は ZEROPLUS と Saleae のみ）。マーケットプレイスでは OEM 品や互換品が混在しており、購入時には出品者の信頼性確認が必要。
- **Evidence（根拠）**: Amazon.co.jp での検索結果に「InnoMaker」ブランド名義の LA2016 互換品（21,999円）が存在。秋月電子のロジアナカテゴリには Kingst・DSLogic 製品なし。
- **Source（情報源）**:
  - [official_document/tier2] [秋月電子 ロジックアナライザーカテゴリ](https://akizukidenshi.com/catalog/r/rlogiana/) (2026-03-14) — primary: true
  - [official_document/tier2] [Amazon.co.jp InnoMaker LA2016](https://www.amazon.co.jp/-/en/InnoMaker-Logic-Analyzer-Support-Windows/dp/B07WVPMDHY) (2026-03-14) — primary: false
- **Confidence（確信度）**: high
- **Evidence Strength（根拠の強さ）**: strong

---

## 推奨ランキング（総合評価）

### 🥇 第1位: Kingst LA2016（最推奨）

| 項目 | 内容 |
|---|---|
| **型番** | Kingst LA2016 |
| **スペック** | 16ch / 200MHz / 128Mbit バッファ |
| **価格帯** | ¥22,000〜¥29,000（Amazon.co.jp） |
| **macOS 対応** | sigrok/PulseView（Rosetta 2 or Homebrew ARM64 ビルド） |
| **プロトコルデコード** | SPI, I2C, UART, CAN, JTAG 他（sigrok 経由） |
| **総合スコア** | ★★★★☆（4.0/5.0） |
| **推奨用途** | MCU 開発のメイン解析ツール |
| **購入先** | [Amazon.co.jp](https://www.amazon.co.jp/Kingst-%E6%9C%80%E5%A4%A7%E3%82%B5%E3%83%B3%E3%83%97%E3%83%AB%E3%83%AC%E3%83%BC%E3%83%88-%E8%8B%B1%E8%AA%9E%E3%82%BD%E3%83%95%E3%83%88%E3%82%A6%E3%82%A7%E3%82%A2%E7%94%A8-%E3%83%87%E3%83%BC%E3%82%BF%E3%82%AD%E3%83%A3%E3%83%97%E3%83%81%E3%83%A3-%E3%83%A1%E3%83%A2%E3%83%AA%E3%82%A2%E3%83%8A%E3%83%A9%E3%82%A4%E3%82%B6/dp/B0DQ4GSXP3)、[楽天市場](https://item.rakuten.co.jp/zmart-shop/2330/)、[zmart.jp](https://zmart.jp/products/detail.php?product_id=2205) |

**長所:**
- 16ch/200MHz という豊富なチャンネル数と十分なサンプリングレートにより、STM32/ESP32 の SPI（〜50MHz）・I2C・UART を同時にキャプチャ可能
- 128Mbit バッファによる長時間連続キャプチャ
- sigrok での成熟したドライバーサポート（LA2016 は sigrok のメインラインに統合済み）
- 3万円以内の予算で USB アイソレーターとの同時購入が可能

**短所・リスク:**
- KingstVIS は macOS 非対応（x86_64 のみ）。sigrok 利用が必須
- 初回セットアップ時にファームウェア抽出手順が必要（1回限り、スクリプトで自動化可能）
- エッジトリガーは1チャンネル分のみ対応（sigrok wiki で確認済み）
- Amazon マーケットプレイスでの購入時は出品者の信頼性確認が必要

---

### 🥈 第2位: Kingst LA1010（コスト重視の妥協策）

| 項目 | 内容 |
|---|---|
| **型番** | Kingst LA1010 |
| **スペック** | 16ch / 最大100MHz / 10GB サンプリング深度 |
| **価格帯** | ¥13,500〜¥14,000（Amazon.co.jp） |
| **macOS 対応** | sigrok/PulseView（実験的） |
| **総合スコア** | ★★★☆☆（3.0/5.0） |
| **推奨用途** | UART/I2C 中心のデバッグ、低速〜中速 SPI |
| **購入先** | [Amazon.co.jp](https://www.amazon.co.jp/LA1010-%E3%82%B5%E3%83%B3%E3%83%97%E3%83%AB%E3%83%AC%E3%83%BC%E3%83%88-16%E3%83%81%E3%83%A3%E3%83%B3%E3%83%8D%E3%83%AB-10B%E3%82%B5%E3%83%B3%E3%83%97%E3%83%AB-%E3%83%87%E3%83%90%E3%83%83%E3%82%B0%E3%83%84%E3%83%BC%E3%83%AB/dp/B078Y8BKKK)、[楽天市場](https://item.rakuten.co.jp/zmart-shop/1596/) |

**長所:**
- 約14,000円と LA2016 の半額程度
- 16ch 対応
- USB アイソレーターを追加しても約16,000〜17,000円で収まる

**短所・リスク:**
- sigrok ドライバーが experimental 段階（Confidence: medium）
- 16ch 使用時のサンプリングレートが 16MHz に制限される可能性あり
- 高速 SPI（10MHz 超）でのオーバーサンプリングマージンが不足する可能性

---

### 🥉 第3位: DSLogic Plus 16ch/400MHz（条件付き推奨）

| 項目 | 内容 |
|---|---|
| **型番** | DSLogic Plus |
| **スペック** | 16ch / 最大400MHz（バッファモード）/ 256Mbit |
| **価格帯** | ¥13,000〜¥17,500（AliExpress/輸入代理店） |
| **macOS 対応** | DSView（Rosetta 2） + sigrok/PulseView |
| **総合スコア** | ★★★☆☆（2.5/5.0） |
| **推奨用途** | 高速 SPI 解析が必要で Windows 併用可能な場合 |
| **購入先** | [AliExpress](https://ja.aliexpress.com/item/1005004996880356.html)、[DreamSourceLab 公式](https://www.dreamsourcelab.com/shop/logic-analyzer/dslogic-plus/) |

**⚠️ 購入時の重要警告:**
- Amazon.co.jp で約2,229円の「DSLogic Plus」は **24MHz/8ch の簡易版**であり、16ch/400MHz の本来の DSLogic Plus ではない。型番・スペックの慎重な確認が必須
- 日本国内の正規代理店は確認できず、AliExpress または個人輸入が主な入手経路

**長所:**
- 400MHz バッファモードにより理論上もっとも高速な信号をキャプチャ可能
- アルミ筐体で物理的な品質が高い
- USB Type-C 接続

**短所・リスク（重大）:**
- macOS Sonoma での SPI デコードバグ（Issue #832）が v1.3.2 で未修正
- Apple Silicon Mac で "device is busy" エラー（Issue #676）が複数報告
- FPGA ビットストリームがクローズドソースであり、長期 macOS 互換性の保証なし
- 400MHz の実効帯域は付属プローブ環境では 150〜250MHz 程度に低下する可能性
- 日本国内での入手性が低く、AliExpress 経由の配送リスクあり

---

### 第4位: FX2 CY7C68013A クローン（学習用サブ機）

| 項目 | 内容 |
|---|---|
| **型番** | CY7C68013A-56 EZ-USB FX2LP クローン各種 |
| **スペック** | 8ch / 24MHz |
| **価格帯** | ¥830〜¥2,000（Amazon.co.jp） |
| **macOS 対応** | sigrok/PulseView（成熟サポート） |
| **総合スコア** | ★★☆☆☆（2.0/5.0）※サブ機として ★★★★☆ |
| **推奨用途** | UART デバッグ、I2C 基本解析、学習用 |
| **購入先** | [Amazon.co.jp（830円〜）](https://www.amazon.co.jp/EZ-USB-FX2LP-CY7C68013A-%E3%83%AD%E3%82%B8%E3%83%83%E3%82%AF-%E3%82%A2%E3%83%8A%E3%83%A9%E3%82%A4%E3%82%B6%E3%83%BC/dp/B0CDGRQQ9W) |

**長所:** 圧倒的低価格、sigrok 最成熟サポート、壊れても即座に買い替え可能

**短所・リスク:** 8ch/24MHz では SPI 10MHz 超の解析は困難、個体品質のばらつきが大きい

---

### ❌ 候補外: ZEROPLUS LAP-C 16064 — macOS 非対応（Windows 専用）、sigrok 非対応
### ❌ 候補外: Saleae Logic 8 — 約99,934円、予算の3倍超

---

## 推奨構成（Mac Mini M4 ユーザー向け）

### 構成A: ベストバランス（推奨）— 合計約25,300〜30,300円

| アイテム | 価格 | 購入先 |
|---|---|---|
| Kingst LA2016 | ¥22,000〜¥27,000 | Amazon.co.jp / zmart.jp |
| DSD TECH SH-G01A USB アイソレーター | ¥3,300 | [Amazon.co.jp](https://www.amazon.co.jp/DSD-TECH-SH-G01A-%E3%82%A2%E3%82%A4%E3%82%BD%E3%83%AC%E3%83%BC%E3%82%BF%E3%83%BC-ADUM3160%E3%83%81%E3%83%83%E3%83%97%E4%BB%98%E3%81%8D/dp/B093LKN3YH) |
| **合計** | **¥25,300〜¥30,300** | |

### 構成B: 最小予算 — 合計約16,100〜16,600円

| アイテム | 価格 | 購入先 |
|---|---|---|
| Kingst LA1010 | ¥13,500〜¥14,000 | Amazon.co.jp |
| DAOKAI USB アイソレーター（ADUM4160） | ¥1,600 | [Amazon.co.jp](https://www.amazon.co.jp/DAOKAI-1500V%E3%82%A2%E3%82%A4%E3%82%BD%E3%83%AC%E3%83%BC%E3%82%BFUSB-%E3%83%9C%E3%83%BC%E3%83%89ADUM4160-%E3%83%97%E3%83%A9%E3%82%B0%E3%82%A2%E3%83%B3%E3%83%89%E3%83%97%E3%83%AC%E3%82%A4%E5%AF%BE%E5%BF%9C-USB%E3%82%B1%E3%83%BC%E3%83%96%E3%83%AB%E4%BB%98%E3%81%8D/dp/B0B82HMLJY) |
| FX2 クローン（サブ機） | ¥1,000 | Amazon.co.jp |
| **合計** | **¥16,100〜¥16,600** | |

### 構成C: 高スペック志向（⚠️ macOS リスクあり）— 合計約16,300〜20,800円

| アイテム | 価格 | 購入先 |
|---|---|---|
| DSLogic Plus（16ch/400MHz） | ¥13,000〜¥17,500 | AliExpress / 輸入代理店 |
| USB アイソレーター | ¥3,300 | Amazon.co.jp |
| **合計** | **¥16,300〜¥20,800** | |
| **⚠️ 注意** | macOS SPI バグ・Apple Silicon 安定性問題あり | |

---

## Mac Mini M4 セットアップ手順（Kingst LA2016 向け）

```bash
# 1. sigrok/PulseView の ARM64 ネイティブインストール（推奨）
brew tap takesako/sigrok
brew install takesako/sigrok/pulseview

# 2. ファームウェア抽出（初回のみ）
git clone https://github.com/AnotherWayIn/kingst-sigrok-tool
# README の手順に従い KingstVIS からファームウェアを抽出

# 3. デバイス接続・テスト
# LA2016 を USB 接続 → PulseView 起動 → デバイス選択 → テストキャプチャ
```

---

## Requirement Level Matrix（義務レベルマトリクス）

| 義務レベル | 件数 | 主な要件 | 規格間の解釈差異 |
|---|---|---|---|
| **MUST** | 3件 | Shannon 2x 下限（fs ≥ 2f_signal）、I2C 規格タイミング準拠（UM10204）、USB デバイスクラス準拠 | Shannon の定理は数学的法則であり解釈の余地なし |
| **SHOULD** | 3件 | 4x オーバーサンプリング、USB アイソレーター使用、macOS 動作確認後の購入 | Saleae FAQ 由来の 4x 推奨は民間企業ガイド。IEC/IEEE の測定標準とは異なるが実務的妥当性は高い |
| **MAY** | 2件 | 10x オーバーサンプリング、ARM64 ネイティブビルド | 10x は理想値であり実務で必須となるケースは稀 |
| **WANT** | 1件 | macOS ネイティブ対応ソフトウェア（ベンダー公式） | 現実的には sigrok 経由が前提 |
| **MUST_NOT** | 1件 | 異なる電源系統の DUT をアイソレーションなしで USB 直結接続 | Saleae 公式ガイドでは「推奨」だが電気的安全性の観点では実質 MUST_NOT |

**解釈差異に関する注記:** 本調査の SHOULD/MUST は法令・規格（EU CRA, SEMI E187 等）の義務レベル用語とは文脈が異なる。本調査における SHOULD は「測定品質を担保するための実務的推奨」であり、法的拘束力はない。

---

## Confidence Distribution（確信度分布）

- **High confidence の発見:** 7件（Finding 1, 2, 3, 5, 6, 7, 8）
- **Medium confidence の発見:** 1件（Finding 4: LA1010 sigrok ドライバーの実用完成度）
- **Low confidence の発見:** 0件
- **全体の確信度評価:** 高い。主要な推奨（LA2016 第一推奨）は複数の独立した公式ソースで裏付けられている。ただし macOS M4 での実機テストは未実施。

---

## Contradictory Claims（矛盾する主張）

| 矛盾 | 前ステップ主張 | Critic 指摘 | 本レビューの解決 |
|---|---|---|---|
| DSLogic Plus 400MHz スペック | Buffer 400MHz で高速 SPI に最適（high） | 実効 150〜250MHz に低下 | Confidence を medium に格下げ、実効帯域制限を明記 |
| DSLogic Plus「オープンソース」 | オープンソース対応 | FPGA ビットストリームはクローズド | 「DSView は GPLv3、FPGA はクローズド」と正確に記述 |
| LA1010 Sampling Fit テーブル | 4ch/50MHz（high） | sigrok ドライバー experimental | LA1010 全体の Confidence を medium に格下げ |

---

## Missing Data（不足データ）

| # | 不足データ | 影響度 | 現状の対処 |
|---|---|---|---|
| 1 | macOS Sequoia 15.x + M4 チップでの実機動作確認 | critical | M1/M2 での報告から推定（medium） |
| 2 | LA2016 の 1.8V IO 対応閾値精度 | medium | STM32L4 系での使用可否が未検証 |
| 3 | DSView v1.3.3 macOS 向けバイナリ提供状況 | high | 公式ダウンロードでは v1.3.2 のみ |
| 4 | Kingst・DreamSourceLab の企業持続性 | medium | 5年スパンでの存続保証なし |
| 5 | USB アイソレーター（ADUM3160/12Mbps）のロジアナ動作影響 | medium | 実測検証未実施 |

---

## Limitations（制約・限界）

1. **実機検証未実施** — Mac Mini M4 での実機動作テストを行っていない
2. **価格変動** — Amazon.co.jp マーケットプレイスの価格は 2026年3月14日時点のスナップショット
3. **ソフトウェアバージョン依存** — sigrok/DSView のバージョンアップにより評価が変化する可能性
4. **個体差リスク** — 特に FX2 クローンおよびマーケットプレイス経由品の品質ばらつき
5. **情報源の偏り** — macOS + ロジアナのコミュニティはニッチであり tier3/tier4 依存度が高い

---

## Unresolved Issues（未解決事項）

1. **LA2016 on macOS Sequoia M4** — sigrok の libusb が Apple USB スタック変更に追従できているか未確認
2. **DSView v1.3.3 macOS バイナリ** — SPI バグ修正の macOS ユーザーへの到達時期不明
3. **LA1010 sigrok ドライバー** — 2023 年の experimental から改善されたかの最新情報不足
4. **Kingst ファームウェア抽出ツール** — 個人開発プロジェクトの継続メンテナンスリスク
5. **USB アイソレーター速度制限** — ADUM3160（12Mbps）が LA2016 の USB 通信に影響するかの実測未実施

---

## Conclusion（結論）

**Kingst LA2016 を第一推奨とする。** 16ch/200MHz/128Mbit のスペックは STM32/ESP32/Arduino の SPI・I2C・UART 解析に十分であり、約22,000〜27,000円で3万円以内。USB アイソレーター（約3,300円）を合わせても約25,300〜30,300円で安全なデバッグ環境を構築可能。

Mac Mini M4 では sigrok/PulseView を Homebrew ARM64 ビルド（`brew tap takesako/sigrok`）で使用するのが最善。

DSLogic Plus は macOS での既知バグと FPGA クローズドソースのリスクにより Mac ユーザーへの第一推奨からは除外。予算を抑えたい場合は **LA1010 + FX2 クローンのダブル構成**（約16,000円）が実用的。

**未確定:** macOS Sequoia + M4 での実機動作確認は未実施（Confidence: medium）。購入前に sigrok コミュニティでの最新動作報告確認を推奨する。

---

## Recommended Actions（推奨アクション）

| 優先度 | アクション | 理由 |
|---|---|---|
| 🔴 **P0** | sigrok コミュニティで「macOS Sequoia + M4 + LA2016」の動作報告を確認 | 購入前の最重要確認事項 |
| 🔴 **P1** | Kingst LA2016 を Amazon.co.jp で購入（出品者信頼性を確認） | 第一推奨 |
| 🔴 **P1** | DSD TECH SH-G01A USB アイソレーターを同時購入 | ハードウェア保護 |
| 🟡 **P2** | sigrok/PulseView を Homebrew ARM64 でビルド | ネイティブ ARM64 が最適 |
| 🟡 **P2** | FX2 クローンをサブ機として1個購入（約1,000円） | 常時接続 UART モニター用 |
| 🟢 **P3** | DSView Issue #832/#676 のステータスを定期ウォッチ | 将来の DSLogic Plus 検討用 |

---

## Source Registry（情報源一覧）

| ID | source_type/tier | タイトル | access_date | primary |
|---|---|---|---|---|
| [SRC-001] | official_document/tier2 | [Kingst LA2016 — sigrok wiki](https://sigrok.org/wiki/Kingst_LA2016) | 2026-03-14 | true |
| [SRC-002] | official_document/tier2 | [Amazon.co.jp Kingst LA2016](https://www.amazon.co.jp/Kingst-%E6%9C%80%E5%A4%A7%E3%82%B5%E3%83%B3%E3%83%97%E3%83%AB%E3%83%AC%E3%83%BC%E3%83%88-%E8%8B%B1%E8%AA%9E%E3%82%BD%E3%83%95%E3%83%88%E3%82%A6%E3%82%A7%E3%82%A2%E7%94%A8-%E3%83%87%E3%83%BC%E3%82%BF%E3%82%AD%E3%83%A3%E3%83%97%E3%83%81%E3%83%A3-%E3%83%A1%E3%83%A2%E3%83%AA%E3%82%A2%E3%83%8A%E3%83%A9%E3%82%A4%E3%82%B6/dp/B0DQ4GSXP3) | 2026-03-14 | false |
| [SRC-003] | official_document/tier2 | [sigrok Mac OS X ビルドガイド](https://sigrok.org/wiki/Mac_OS_X) | 2026-03-14 | true |
| [SRC-004] | community_data/tier4 | [takesako/homebrew-sigrok](https://github.com/takesako/homebrew-sigrok) | 2026-03-14 | false |
| [SRC-005] | community_data/tier4 | [kingst-sigrok-tool](https://github.com/AnotherWayIn/kingst-sigrok-tool) | 2026-03-14 | false |
| [SRC-006] | official_document/tier2 | [libsigrok Homebrew](https://formulae.brew.sh/formula/libsigrok) | 2026-03-14 | true |
| [SRC-007] | community_data/tier4 | [DSView Issue #832](https://github.com/DreamSourceLab/DSView/issues/832) | 2026-03-14 | false |
| [SRC-008] | community_data/tier4 | [DSView Issue #676](https://github.com/DreamSourceLab/DSView/issues/676) | 2026-03-14 | false |
| [SRC-009] | industry_report/tier3 | [DSView SPI bug — CSDN](https://blog.csdn.net/gitblog_07113/article/details/148888506) | 2026-03-14 | false |
| [SRC-010] | official_document/tier2 | [DSView ダウンロード](https://www.dreamsourcelab.com/download/) | 2026-03-14 | true |
| [SRC-011] | official_document/tier2 | [Amazon.co.jp LA1010](https://www.amazon.co.jp/LA1010-%E3%82%B5%E3%83%B3%E3%83%97%E3%83%AB%E3%83%AC%E3%83%BC%E3%83%88-16%E3%83%81%E3%83%A3%E3%83%B3%E3%83%8D%E3%83%AB-10B%E3%82%B5%E3%83%B3%E3%83%97%E3%83%AB-%E3%83%87%E3%83%90%E3%83%83%E3%82%B0%E3%83%84%E3%83%BC%E3%83%AB/dp/B078Y8BKKK) | 2026-03-14 | false |
| [SRC-012] | community_data/tier4 | [sigrok-devel LA1010](https://sourceforge.net/p/sigrok/mailman/sigrok-devel/thread/20230131184537.GC4396%40book.gsilab.sittig.org/) | 2026-03-14 | false |
| [SRC-013] | official_document/tier2 | [Amazon.co.jp CY7C68013A](https://www.amazon.co.jp/EZ-USB-FX2LP-CY7C68013A-%E3%83%AD%E3%82%B8%E3%83%83%E3%82%AF-%E3%82%A2%E3%83%8A%E3%83%A9%E3%82%A4%E3%82%B6%E3%83%BC/dp/B0CDGRQQ9W) | 2026-03-14 | false |
| [SRC-014] | community_data/tier4 | [EEVblog knockoff LA](https://www.eevblog.com/forum/testgear/best-cheap-knockoff-logic-analyzer/) | 2026-03-14 | false |
| [SRC-015] | official_document/tier2 | [ZEROPLUS 公式](https://www.zeroplus.com.tw/logic-analyzer_en/technical_support_search.php?model=LAP-C%2816064%29&class1=1) | 2026-03-14 | true |
| [SRC-016] | official_document/tier2 | [秋月電子 ロジアナカテゴリ](https://akizukidenshi.com/catalog/r/rlogiana/) | 2026-03-14 | true |
| [SRC-017] | official_document/tier2 | [秋月電子 Saleae Logic 8](https://akizukidenshi.com/catalog/g/g108975/) | 2026-03-14 | false |
| [SRC-018] | official_document/tier2 | [Amazon.co.jp Saleae Logic 8](https://www.amazon.co.jp/Saleae-Logic-8%EF%BC%88%E3%83%96%E3%83%A9%E3%83%83%E3%82%AF%EF%BC%89-8%E3%83%81%E3%83%A3%E3%83%B3%E3%83%8D%E3%83%AB%E3%83%AD%E3%82%B8%E3%83%83%E3%82%AF%E3%82%A2%E3%83%8A%E3%83%A9%E3%82%A4%E3%82%B6/dp/B0749G85W2) | 2026-03-14 | false |
| [SRC-019] | official_document/tier2 | [Saleae Isolation Guide](https://support.saleae.com/getting-help/troubleshooting/technical-faq/suggestions-for-electrical-isolation) | 2026-03-14 | true |
| [SRC-020] | community_data/tier4 | [SparkFun ground loop](https://community.sparkfun.com/t/blew-up-my-logic-analyzer-and-device-ground-issues/36399) | 2026-03-14 | false |
| [SRC-021] | official_document/tier2 | [DSD TECH SH-G01A](https://www.amazon.co.jp/DSD-TECH-SH-G01A-%E3%82%A2%E3%82%A4%E3%82%BD%E3%83%AC%E3%83%BC%E3%82%BF%E3%83%BC-ADUM3160%E3%83%81%E3%83%83%E3%83%97%E4%BB%98%E3%81%8D/dp/B093LKN3YH) | 2026-03-14 | false |
| [SRC-022] | official_document/tier2 | [InnoMaker LA2016](https://www.amazon.co.jp/-/en/InnoMaker-Logic-Analyzer-Support-Windows/dp/B07WVPMDHY) | 2026-03-14 | false |
| [SRC-023] | official_document/tier2 | [DSLogic Plus 公式ストア](https://www.dreamsourcelab.com/shop/logic-analyzer/dslogic-plus/) | 2026-03-14 | true |
| [SRC-024] | industry_report/tier3 | [DSLogic レビュー — ちかてつ.com](https://tikatetu.com/electronics/dslogic-review/) | 2026-03-14 | false |
| [SRC-025] | industry_report/tier3 | [DSLogic Plus — Lang-ship](https://lang-ship.com/blog/work/dslogic-plus/) | 2026-03-14 | false |
| [SRC-026] | community_data/tier4 | [EEVblog DSLogic thread](https://www.eevblog.com/forum/testgear/dslogic-logic-analyzer-from-dreamsourcelab-(16-ch-256-mbits-400-mhz)/) | 2026-03-14 | false |
| [SRC-027] | industry_report/tier3 | [DSLogic Teardown — Hackaday](https://hackaday.com/2017/08/24/dslogic-plus-teardown-and-review/) | 2026-03-14 | false |
| [SRC-028] | community_data/tier4 | [PulseView on M1 Mac](https://nishtahir.com/running-pulseview-on-an-m1-mac/) | 2026-03-14 | false |
| [SRC-029] | official_document/tier2 | [sigrok Downloads](https://sigrok.org/wiki/Downloads) | 2026-03-14 | true |
| [SRC-030] | official_document/tier2 | [DAOKAI USB アイソレーター](https://www.amazon.co.jp/DAOKAI-1500V%E3%82%A2%E3%82%A4%E3%82%BD%E3%83%AC%E3%83%BC%E3%82%BFUSB-%E3%83%9C%E3%83%BC%E3%83%89ADUM4160-%E3%83%97%E3%83%A9%E3%82%B0%E3%82%A2%E3%83%B3%E3%83%89%E3%83%97%E3%83%AC%E3%82%A4%E5%AF%BE%E5%BF%9C-USB%E3%82%B1%E3%83%BC%E3%83%96%E3%83%AB%E4%BB%98%E3%81%8D/dp/B0B82HMLJY) | 2026-03-14 | false |

**Community Score 付記（tier4 ソース）:**
- [SRC-004] takesako/homebrew-sigrok: discussion_age: "2+ years", participant_count: ~5, expert_presence: true, reproducibility: true
- [SRC-007] DSView Issue #832: discussion_age: "active 2024–2025", participant_count: ~10, expert_presence: false, reproducibility: true
- [SRC-012] sigrok-devel LA1010: discussion_age: "3+ years", participant_count: ~5, expert_presence: true, reproducibility: false
- [SRC-014] EEVblog knockoff LA: discussion_age: "5+ years", participant_count: ~50, expert_presence: true, reproducibility: true
- [SRC-020] SparkFun ground loop: discussion_age: "5+ years", participant_count: ~20, expert_presence: false, reproducibility: false
