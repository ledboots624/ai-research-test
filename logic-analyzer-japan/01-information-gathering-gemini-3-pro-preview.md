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
*   **Specs**:
    *   Channels: 16
    *   Max Sample Rate: 200MHz (State), 500MHz (Timing - partial)
    *   Protocols: SPI, UART, I2C, CAN, etc.
*   **Source**:
    *   [community/tier4] [Amazon.co.jp: Kingst LA2016 販売ページ](https://www.amazon.co.jp/dp/B0DQ4GSXP3) (2025-03-14) — primary: false
    *   [official/tier2] [KingstVIS Download Page](http://www.qdkingst.com/en/download) (2025-03-14) — primary: true
*   **Confidence**: high

## 2. Balanced Option: Kingst LA1010

MCU開発（SPI/I2C/UART）には十分なスペックを持ち、予算の半分以下で購入可能です。

*   **Claim**: 16ch/100MHzのモデルが約1.3万円で購入可能。LA2016と同様のソフトウェア環境で動作する。
*   **Evidence**:
    *   Amazon.co.jpでのKingstブランド正規品価格は約13,553円。
    *   廉価なOEM/類似品（MiiElAOD等）は7,500円程度で存在するが、サポート品質に不確実性がある。
*   **Specs**:
    *   Channels: 16
    *   Max Sample Rate: 100MHz
*   **Source**:
    *   [community/tier4] [Amazon.co.jp: Kingst LA1010 販売ページ](https://www.amazon.co.jp/dp/B078Y8BKKK) (2025-03-14) — primary: false
*   **Confidence**: high

## 3. Entry Option: Generic 24MHz 8ch (Saleae Clone)

「とりあえず波形が見たい」という用途に最適です。オープンソースソフトウェアとの相性が良く、Mac M4での動作が最も軽量です。

*   **Claim**: Amazonやスイッチサイエンスで1,000円〜2,000円程度で入手可能。オープンソースの「PulseView (Sigrok)」を使用することで、Apple Silicon Mac上でネイティブ動作が可能。
*   **Evidence**:
    *   Amazonでは「HiLetgo」「KeeYees」「AzDelivery」等のブランドで販売（約1,000円〜1,600円）。
    *   スイッチサイエンスでは「USBロジックアナライザ」として約5,870円で販売（信頼性重視）。
    *   ソフトウェア「PulseView」はApple Silicon (ARM64) 向けNightlyビルドが提供されている。
*   **Specs**:
    *   Channels: 8
    *   Max Sample Rate: 24MHz (実効速度はUSB帯域依存でこれより低い場合あり)
    *   Note: 高速なSPI通信（数MHz以上）ではサンプリング不足になる可能性がある。
*   **Source**:
    *   [community/tier4] [Amazon.co.jp: KeeYees 24MHz 8ch](https://www.amazon.co.jp/dp/B07RKLKQ4B) (2025-03-14) — primary: false
    *   [community/tier3] [Sigrok PulseView Downloads](https://sigrok.org/wiki/Downloads) (2025-03-14) — primary: true
*   **Confidence**: high

## 4. Availability of DSLogic Series

*   **Claim**: DSLogic Plusは高性能だが、日本国内Amazonでの正規品在庫が不安定かつ価格が上昇傾向にある。
*   **Evidence**:
    *   Amazon.co.jpでは「在庫切れ」やマーケットプレイスによる高額出品（約2.8万円〜）が散見される。
    *   公式ソフトウェア「DSView」はmacOS対応（Intel版）だが、MシリーズMacでもRosetta 2で動作実績がある。
*   **Source**:
    *   [community/tier4] [Amazon.co.jp: DSLogic Plus Search](https://www.amazon.co.jp/s?k=DSLogic+Plus) (2025-03-14) — primary: false
*   **Confidence**: medium

## 5. Non-Recommended: Zeroplus LAP-C Series

*   **Claim**: 秋月電子等で入手しやすいが、Mac環境での利用には適さない。
*   **Evidence**:
    *   Zeroplus公式サイトおよびマニュアルにおいて、対応OSはWindowsのみと明記されている。
    *   macOS用の公式ドライバ・ソフトウェアが存在しないため、Parallels等の仮想環境が必要となり、セットアップが煩雑である。
*   **Source**:
    *   [official/tier2] [Zeroplus LAP-C Technical Support](https://www.zeroplus.com.tw/logic-analyzer_en/technical_support_search.php?model=LAP-C%2816064%29&class1=1) (2025-03-14) — primary: true
*   **Confidence**: high

---

## Comparison Table

| Model | Est. Price | Channels | Max Rate | Mac Software | Availability |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Kingst LA2016** | ¥27,300 | 16 | 200MHz | KingstVIS (Rosetta 2) | Amazon (Good) |
| **Kingst LA1010** | ¥13,600 | 16 | 100MHz | KingstVIS (Rosetta 2) | Amazon (Good) |
| **Generic 24MHz** | ¥1,500 | 8 | 24MHz | PulseView (Native ARM) | Amazon (Excellent) |
| **DSLogic Plus** | ¥28,600+ | 16 | 400MHz | DSView (Rosetta 2) | Amazon (Poor/Volatile) |
| **Zeroplus LAP-C**| ¥22,800 | 16 | 100MHz | **Windows Only** | Akizuki (Good) |

### Recommendation for Mac Mini M4 User
1.  **コストパフォーマンス最優先なら**: Amazonで **Generic 24MHz 8ch** (KeeYees等) を購入し、`PulseView` (Nightly ARM build) を使用する。Arduinoや低速なUART/I2Cデバッグには十分です。
2.  **本格的な開発・信頼性重視なら**: **Kingst LA1010** または **LA2016**。Rosetta 2経由ですがメーカー製ソフトが動作し、チャネル数・帯域幅に余裕があるため、SPI通信の解析でも安心です。
