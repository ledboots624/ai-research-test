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

> **矛盾指摘**: 前ステップの調査では「sigrokサポートあり（Confidence: high）」と結論づけているが、**2023年以降の新ロットに関しては sigrok 非対応の可能性が高い**。前ステップの調査は旧ハードウェアの情報を現在も有効なものとして扱っており、致命的な情報ギャップがある。

---

### ⚠️ Gap 2: Amazon.co.jp「U2BASIC」の価格乖離——前ステップ報告との矛盾

**Claim（前ステップ）**: Amazon.co.jpでは約32,000〜38,000円。

**Claim（今回確認）**: Amazon.co.jpの実際のリスティングでは **¥19,000〜¥20,000** 前後の価格帯が確認された。

**Evidence**:
- Amazon.co.jp ASIN B0DZMJWXFP（U2BASIC）および ASIN B09VK5WM8L の両リスティングで¥19,000〜¥20,000前後のプライシングが確認されている。
- 前ステップが提示した「32,000〜38,000円」は、**より上位グレード製品（Pro系・U3Pro等）の価格と混同**している可能性が高い。

**Source**:
- [merchant_data] Amazon.co.jp ASIN B0DZMJWXFP (https://www.amazon.co.jp/-/en/Oscilloscope-U2BASIC-Analyzer-Channel-Sampling/dp/B0DZMJWXFP, 2026-03-13)
- [merchant_data] Amazon.co.jp ASIN B09VK5WM8L (https://www.amazon.co.jp/-/en/Logic-Analyzer-U2BASIC-Channel-Sampling/dp/B09VK5WM8L, 2026-03-13)

**Confidence**: **medium**（価格は変動するため、閲覧時点の情報として解釈すること）

> **修正推奨**: AliExpress との価格差は前ステップが主張するほど大きくない可能性がある（約1.5倍差 → 実態は1.2〜1.3倍差程度か）。Amazon.co.jp の U2BASIC は想定より費用対効果が高い選択肢である可能性がある。

---

## Findings（主要な発見）

### Finding 1: DSView vs. sigrok ——「どちらを主軸に使うか」は決定的に重要な分岐点

**Claim**: sigrok/PulseView での利用を主目的とする場合と、DSView での利用を前提とする場合では、**購入すべき製品ロットが根本的に異なる**。両者を「併用できる」と前ステップが示唆しているのは過度に楽観的な評価である。

**Evidence**:
| 用途 | 必要なハードウェア | リスク |
|---|---|---|
| DSView のみ利用 | 最新 Pango 版を含む全ロット対応 | なし |
| sigrok/PulseView 利用 | **旧 Xilinx Spartan-6 版必須**（VID:PID `2a0e:0020`）| 購入時にロットが選べない |
| sigrok＋DSView 併用 | 旧 Xilinx 版のみ + ファームウェア抽出作業必要 | 手間あり、新ロット購入リスク |

**Source**:
- [official_document] DreamSourceLab DSLogic Plus - sigrok (https://sigrok.org/wiki/DreamSourceLab_DSLogic_Plus, 2026-03-13)
- [community_data] EEVblog DSlogic Plus Pango Version forum thread (https://www.eevblog.com/forum/testgear/dslogic-plus-pango-version-(2023)/, 2026-03-13)

**Confidence**: **high**

---

### Finding 2: DreamSourceLab と sigrok コミュニティの歴史的緊張関係

**Claim**: DreamSourceLab は当初 GPL 違反の疑い（DSView が PulseView のリスキン）を受けており、コミュニティからの圧力を受けた後に DSView を OSS 公開した経緯がある。この背景が、sigrok 開発者側の DSLogic サポートに対する消極姿勢に影響している。

**Evidence**:
- DSView は PulseView（GPLv3）をベースとしていながら、当初クローズドソースとして配布。GPL 違反を指摘されたのちに GitHub にてソース公開。
- sigrok 開発者側は DSLogic の新ハードウェア（Pango 版、U3Pro16 等）への対応に積極的ではなく、コミュニティ頼みの状況。
- U3Pro16 の sigrok 対応は API 変更の問題で停滞中（2023年時点）。

**Source**:
- [community_data] DSLogic U3Pro16 Review and Teardown - tomverbeure.github.io (https://tomverbeure.github.io/2025/04/12/DSLogic-U3Pro16-Teardown.html, 2026-03-13)
- [official_document] GitHub DreamSourceLab/DSView repository (https://github.com/DreamSourceLab/DSView, 2026-03-13)
- [community_data] sigrok-devel mailing list: Support for DSLogic U3Pro16 (https://sourceforge.net/p/sigrok/mailman/sigrok-devel/thread/8fdf1c0f-f9ed-11ed-099f-5a6417beb7dc%40dresco.co.uk/, 2026-03-13)

**Confidence**: **high**

> **洞察**: sigrok の DSLogic 対応は**構造的に縮小傾向**にある。新製品・新ロットへの対応速度は遅く、今後も改善は期待しにくい。sigrok を主目的とするなら、DSLogic シリーズへの依存度を下げた選定（例: Cypress FX2LA ベース製品への移行）を検討すべきである。

---

### Finding 3: ファームウェア抽出手順は「追加作業」ではなく「必須作業」

**Claim**: 旧 Xilinx 版 DSLogic Plus を sigrok で使用する場合でも、ファームウェア抽出作業（`sigrok-fwextract-dreamsourcelab-dslogic` スクリプトの実行）は**任意ではなく必須**である。

**Evidence**:
- DSLogic シリーズの FPGA ビットストリームおよび FX2 ファームウェアは sigrok パッケージに同梱されておらず、DreamSourceLab の DSView リポジトリから取得・配置する必要がある。
- 具体的ファイルマッピング:
  - `DSLogic50.bin` → `dreamsourcelab-dslogic-fpga-5v.fw`
  - `DSLogic33.bin` → `dreamsourcelab-dslogic-fpga-3v3.fw`
  - `DSLogic.fw` → `dreamsourcelab-dslogic-fx2.fw`
- これらのファイルがない状態では、PulseView を起動してもデバイスが正常にロードされない。

**Source**:
- [official_document] DreamSourceLab DSLogic - sigrok Wiki (https://sigrok.org/wiki/DreamSourceLab_DSLogic, 2026-03-13)
- [official_document] sigrok-util firmware extraction script (https://github.com/sigrokproject/sigrok-util/blob/master/firmware/dreamsourcelab-dslogic/sigrok-fwextract-dreamsourcelab-dslogic, 2026-03-13)

**Confidence**: **high**

---

### Finding 4: AliExpress クローンの品質リスクは「軽視できない水準」

**Claim**: AliExpress で販売される DSLogic クローンは、ハードウェア品質・ファームウェア整合性・長期信頼性において**genuine 品と比較して実質的なリスクがある**。「最安値で十分」という前ステップの推奨は、リスクを過小評価している。

**Evidence**:
- シールド・ケーブル・コネクタ品質が劣るため、高速取得（100MHz+）時の S/N 比が低下するリスクがある。
- クローン品は VID:PID をコピーしている場合とそうでない場合があり、ファームウェアの適用可否が不定。
- 一部クローンが数ヶ月で動作不良になったというコミュニティ報告がある。
- USB 電源設計の品質不足による PC 側リスクも排除できない（未確認だが構造上のリスクとして実在）。

**Source**:
- [community_data] Best <$50 USB Logic Analyzer compatible with sigrok - EEVblog (https://www.eevblog.com/forum/testgear/best-lt$50-or-$lt100-usb-logic-analyzer-compatible-with-sigrok/, 2026-03-13)
- [community_data] DSLogic U3Pro16 Review and Teardown (https://tomverbeure.github.io/2025/04/12/DSLogic-U3Pro16-Teardown.html, 2026-03-13)

**Confidence**: **medium**（クローン固有の詳細テスト結果は未確認。一般的な品質傾向からの推論を含む）

---

## Confidence Assessment（前ステップ調査の信頼度再評価）

| 前ステップのClaimの内容 | 元の Confidence | 再評価後 | 理由 |
|---|---|---|---|
| sigrok サポートあり（Confidence: high） | high | **⬇ medium〜low** | Pango 版（2023+）では非対応。ロット判別不可の問題を無視 |
| Plus相当品をAliExpressで購入推奨（Confidence: high） | high | **⬇ medium** | Pango 版リスクおよびクローン品質リスクが未加味 |
| Amazon.co.jp ¥32,000〜38,000 | high | **矛盾（要修正）** | 実態は¥19,000〜¥20,000帯の可能性が高い |
| ファームウェア抽出は「必要になる場合がある」 | high | **修正要** | 実際は常に必須の手順。任意ではない |
| "Air" 等の廉価版を避けるべき | medium | **⬆ high** | 合理的なリスク評価。確認できる情報と整合 |

---

## Recommendations（推奨と戦略的結論）

### ケース別推奨戦略

**ケース A：sigrok/PulseView を必ず使いたい**
- **推奨**: 旧 Xilinx 版（VID:PID `2a0e:0020`）を入手できる場合のみ DSLogic Plus を選択。
- **現実的な問題**: 現在 AliExpress/Amazon で販売されているユニットが旧版か Pango 版かを購入前に判別する確実な方法がない。
- **代替案（推奨）**: FX2-based ロジアナ（例: Cypress FX2 ベースの 16ch 製品）または Saleae 互換品への移行を検討すること。sigrok サポートが構造的に安定している。
- **Confidence**: medium（旧版の在庫状況は未確認のため）

**ケース B：DSView のみ利用（Windows/Mac/Linux）**
- **推奨**: Pango 版を含む現行ロットで問題なし。
- Amazon.co.jp U2BASIC（¥19,000〜¥20,000）は genuine 品として、AliExpress のクローン品（¥13,000〜¥16,000）と比較して追加の安心感を提供する。
- **Confidence**: high

**ケース C：コスト最優先、hobbyist 利用（I2C/SPI/UART 程度）**
- **推奨**: AliExpress での DSLogic Plus 相当品（metal case 付き）を選択するが、sigrok 非対応となっても受け入れる前提で。純正 DSView の品質は十分実用的。
- **Confidence**: high

---

### 最終スコアカード（本調査の統合判断）

| 評価軸 | スコア | 備考 |
|---|---|---|
| AliExpress クローンの価格競争力 | ★★★★☆ | genuine の約半額〜7割 |
| sigrok 互換性（現行ロット） | ★★☆☆☆ | Pango 版リスクで大幅減点 |
| DSView による安定動作 | ★★★★☆ | 旧版・Pango版問わず対応 |
| Amazon.co.jp U2BASIC のコスパ | ★★★☆☆ | genuine品で¥19,000〜は妥当 |
| 日本からの入手容易性 | ★★★★☆ | Amazon/AliExpress共に問題なし |
