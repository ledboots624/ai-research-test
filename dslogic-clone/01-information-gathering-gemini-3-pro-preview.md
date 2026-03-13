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

### "U2BASIC" on Amazon Japan
**Claim**: Amazon.co.jpで流通している「U2BASIC」という名称の製品は、仕様上 **DSLogic Plus相当（256Mbitバッファ搭載）** である可能性が高いです。
**Evidence**: Amazonの製品説明において「256Mbit SDRAM」「400Mサンプリング」という記述が確認できました。これは本来「Plus」モデルのスペックであり、Basicモデル（バッファなし）とは異なります。
**Source**:
- [merchant_data] DSLogic U2Basic Logic Analyzer With 16 Channel 400M Sampling (Amazon.com/Amazon.co.jp, access_date: 2026-03-13)
**Confidence**: medium (販売者による表記揺れの可能性があるため)

## Purchasing Channels (購入経路と価格)

### AliExpress (Recommended for Price)
**Claim**: 最安値で購入する場合、AliExpressが推奨されます。**DSLogic Plus** として販売されている製品を選定してください。
**Evidence**:
- **価格帯**: 約13,000円 〜 16,000円（送料無料が多い）。
- **検索キーワード**: "DSLogic Plus", "Logic Analyzer 16Ch 400MHz"。
- **注意点**: "Basic" を安易に選ばないこと。メタルケース（シールド）付きのモデルを選ぶとノイズ耐性が有利です。
**Source**:
- [merchant_data] AliExpress Search Results for "DSLogic Plus" (AliExpress, access_date: 2026-03-13)
**Confidence**: high

### Amazon.co.jp (Recommended for Speed/Support)
**Claim**: 国内在庫と返品保証を重視する場合、Amazon.co.jpの「U2BASIC」が選択肢となりますが、割高です。
**Evidence**:
- **価格帯**: 約32,000円 〜 38,000円。AliExpressの約2倍の価格設定です。
- **メリット**: 国内配送による短納期、Amazonの返品ポリシー適用。
**Source**:
- [merchant_data] Amazon.co.jp Search Results for "U2BASIC" (Amazon.co.jp, access_date: 2026-03-13)
**Confidence**: high

## Compatibility: sigrok & PulseView (互換性とソフトウェア)

### Sigrok Support Status
**Claim**: DSLogicシリーズ（Basic/Plus/Pro/U2Basic）は、オープンソースのロジックアナライザソフト **sigrok (PulseView)** でサポートされています。
**Evidence**: sigrokのハードウェア互換性リストに "DreamSourceLab DSLogic" シリーズが掲載されており、`fx2lafw` ドライバまたは専用ドライバで動作します。
**Source**:
- [official_document] Supported hardware - sigrok (sigrok.org, access_date: 2026-03-13)
- [community_data] GitHub sigrok-firmware-fx2lafw repository (GitHub, access_date: 2026-03-13)
**Confidence**: high

### Firmware Extraction Required for Advanced Features
**Claim**: DSLogic Plus相当の機能（FPGAビットストリームを必要とする高機能モード）をsigrokで使用するには、純正ソフトウェア（DSView）からファームウェアファイルを抽出・配置する作業が必要になる場合があります。
**Evidence**: sigrokのWikiには、DreamSourceLabの公式インストーラからファームウェアとFPGAビットストリームを抽出するためのスクリプト（`sigrok-fwextract-dreamsourcelab-dslogic`）や手順が記載されています。これを行わない場合、Basic相当の機能に制限される可能性があります。
**Source**:
- [official_document] DreamSourceLab DSLogic - sigrok Wiki (sigrok.org, access_date: 2026-03-13)
**Confidence**: high

### Windows Driver Setup (Zadig)
**Claim**: Windows環境でsigrokを使用する場合、USBドライバの差し替えが必要です。
**Evidence**: デフォルトのドライバまたは純正DSView用ドライバではなく、**Zadig** ツールを使用して `WinUSB` ドライバを割り当てることで、sigrokからデバイスを認識できるようになります。
**Source**:
- [community_data] Hobby Components' LOGIC ANALYZER not recognized by SIGROK (Arduino Forum, access_date: 2026-03-13)
**Confidence**: high

## Key Risks & Warnings (注意点)

### "Air" and "Basic" Models
**Claim**: "Air" という名称のモデルや、明示的にSDRAM搭載を謳っていない安価なモデルは避けるべきです。
**Evidence**: "U2Basic Air" などの名称は公式ラインナップに見当たらず、スペックダウンされた廉価版や、品質の劣るクローンの可能性があります。バッファメモリがないと、高サンプリングレート時に「サンプル落ち」が頻発し、ロジックアナライザとしての実用性が著しく低下します。
**Source**:
- [community_data] EEVblog forum discussions on DSLogic clones (EEVblog, access_date: 2026-03-13)
**Confidence**: medium

### DIY Upgrade Path (High Skill Required)
**Claim**: 安価なBasicモデルを購入し、自分でSDRAMチップをはんだ付けしてEEPROMを書き換えることでPlus相当にする「改造（Mod）」手法が存在しますが、推奨しません。
**Evidence**: 成功報告は多数ありますが、微細なはんだ付け技術が必要であり、失敗のリスクや、EEPROMのPID変更の手間を考慮すると、最初からPlus相当品を購入する方がコストパフォーマンスが良いと判断されます。
**Source**:
- [community_data] Upgrading DSLogic Basic to Plus without EEPROM modification (EEVblog, access_date: 2026-03-13)
**Confidence**: high

---
**Conclusion / Recommendation**
- **Best Buy**: AliExpressで **"DSLogic Plus"** (16Ch, 400MHz, 256Mbit RAM, Metal Case) を購入する（約1.5万円）。
- **Software**: 基本は純正ソフト **DSView** を使いつつ、必要に応じてファームウェアを抽出して **sigrok/PulseView** を併用する運用が最も安定的です。
