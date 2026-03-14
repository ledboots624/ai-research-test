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
- **入力インピーダンス**: 220kΩ, 12pF
- **スレッショルド電圧**: -4V ～ +4V（0.01V刻み、調整可能）
- **最小検出パルス幅**: 12.5ns
- **PWM出力**: 2ch（0.1〜20MHz、+3.3V出力）
- **接続**: USB 2.0（USB 3.0互換）

**Source（情報源）**:
- [official/tier2] [Kingst LA2016 Product Page - qdkingst.com](http://www.qdkingst.com/en/products/LA2016) (2026-03-14) — primary: true
- [official/tier2] [KINGST LA2016 USER MANUAL - ManualsLib](https://www.manualslib.com/manual/3845892/Kingst-La2016.html) (2026-03-14) — primary: true
- [community/tier3] [Kingst LA2016 - sigrok Wiki](https://sigrok.org/wiki/Kingst_LA2016) (2026-03-14) — primary: false
- [community/tier3] [Kingst LA2016 - OpenTraceLab](https://opentracelab.github.io/website/hardware/logic-analyzer/kingst-la2016/) (2026-03-14) — primary: false

**Confidence（確信度）**: high

---

### 1-2. Kingst LA1010 ⚠️ 重要な注意点あり

**Claim（主張）**: LA1010は**ハードウェアメモリを持たないストリーミング専用モデル**であり、「100MHz 16ch」という表記はチャネル数に応じて大幅に制限される。

**Evidence（根拠）**:

| チャネル数 | 最大サンプリングレート |
|:---:|:---:|
| 最大3ch | 100MHz |
| 最大6ch | 50MHz |
| 最大9ch | 32MHz |
| 最大16ch | **16MHz** |

- **ハードウェアメモリ**: **非搭載**。全データはUSBでPCにリアルタイムストリーミング
- **実効メモリ深度**: PC搭載RAMとUSB帯域に依存（理論上無制限だが実際はUSB安定性に左右される）
- **最大圧縮深度**: 10G samples/ch（ソフトウェア圧縮、実態はPCリソース次第）
- **入力電圧範囲**: -50V ～ +50V
- **入力インピーダンス**: 220kΩ, 12pF
- **スレッショルド電圧**: -4V ～ +4V（0.01V刻み）
- **最小検出パルス幅**: 20ns
- **PWM出力**: 2ch（0.1〜10MHz、+3.3V出力）

> ⚠️ **実運用上の重大な制約**: 16ch全チャネル同時使用時は16MHzまで低下。例えばSPIの場合、MOSI・MISO・SCLK・CSの4信号解析には最低4ch必要であり、この構成では50MHzまでに制限される。MCU開発での典型的なSPI周波数（数MHz〜20MHz）には対応可能だが、余裕は少ない。

**Source（情報源）**:
- [official/tier2] [Kingst LA1010 Product Page - qdkingst.com](http://www.qdkingst.com/en/products/LA1010) (2026-03-14) — primary: true
- [official/tier2] [KINGST LA1010 USER MANUAL - ManualsLib](https://www.manualslib.com/manual/3845894/Kingst-La1010.html) (2026-03-14) — primary: true
- [official/tier2] [KingstVIS User Manual PDF - goldenchipset.com](https://goldenchipset.com/24download/KingstVIS_Manual.pdf) (2026-03-14) — primary: true

**Confidence（確信度）**: high

---

### 1-3. DSLogic Plus

**Claim（主張）**: DSLogic Plusは**バッファモードとストリームモードを切り替え可能な二刀流アーキテクチャ**を持ち、チャネル数に応じたサンプリングレートの制約がある。

**Evidence（根拠）**:

| モード | チャネル数 | 最大サンプリングレート |
|:---:|:---:|:---:|
| バッファモード（高速・短時間） | 4ch | **400MHz** |
| バッファモード | 8ch | 200MHz |
| バッファモード | 16ch | 100MHz |
| ストリームモード（長時間） | 3ch | 100MHz |
| ストリームモード | 6ch | 50MHz |
| ストリームモード | 12ch | 25MHz |
| ストリームモード | 16ch | **20MHz** |

- **ハードウェアメモリ**: **256Mbit（32MB）搭載**（LA2016の1/4）
- **最大ストリームサンプル深度**: 16G samples/ch（RLE）
- **入力電圧範囲（保護）**: **-30V ～ +30V**（LA2016の-50V〜+50Vより狭い）
- **安全な論理信号入力範囲**: -0.9V ～ +6V
- **スレッショルド電圧**: 0V ～ 5V（0.1V刻み）—LA2016の-4V〜+4Vとは異なり負電圧に非対応
- **入力インピーダンス**: 250kΩ, 13pF
- **最小検出パルス幅**: 5ns（3製品中最速）
- **接続**: USB 2.0 Type-C

> ⚠️ **注意点**: バッファモード400MHzはあくまで4ch限定。16ch全使用時のバッファモードは100MHz、ストリームモードでは20MHzまで低下する。スレッショルド電圧が0V以上のみのため、負電圧論理レベルの信号には対応不可。

**Source（情報源）**:
- [official/tier1] [DSLogic Plus Datasheet - DreamSourceLab](https://www.dreamsourcelab.com/doc/DSLogic_Plus_Datasheet.pdf) (2026-03-14) — primary: true
- [community/tier3] [DreamSourceLab DSLogic Plus - sigrok Wiki](https://sigrok.org/wiki/DreamSourceLab_DSLogic_Plus) (2026-03-14) — primary: false

**Confidence（確信度）**: high

---

### 1-4. Generic FX2 8ch 24MHz（汎用クローン）⚠️ 実効性能の大幅な制約

**Claim（主張）**: FX2チップベースの汎用クローンは、「24MHz」というカタログスペックが**持続的な実効レートではなく理論値**であり、実運用では大きく制限される。また入力保護回路が事実上存在しない。

**Evidence（根拠）**:
- **使用チップ**: Cypress EZ-USB FX2LP（CY7C68013A）
- **理論最大サンプリングレート**: 24MHz（8ch）
- **実効持続レート**: 8chで16〜24MHzが現実的上限。16ch時は8〜12MHzに低下
- **USB 2.0ボトルネック**: 24MB/s（8ch×24MHz）は理論USB 2.0 High-Speed上限（480Mbps実効30〜35MB/s）に近接し、他デバイス共存時や長時間キャプチャでサンプル欠落のリスクあり
- **ハードウェアメモリ**: **非搭載**（FX2内部FIFO数KB程度のみ）
- **入力電圧範囲**: **3.3V/5V TTL論理レベルのみ対応**（保護回路なし、過電圧で即破損リスク）
- **スレッショルド電圧**: 固定（調整不可）—通常1.4〜2.0V程度

> ⚠️ **重大な制約**:
> - **入力保護なし**: STM32やESP32の3.3V信号には問題ないが、誤接続やノイズで即座に破損する可能性が高い
> - **SPI解析の限界**: 実効24MHzでも、MCU通信によくある数MHz以上のSPI（例: 10MHz SPI）の解析では、ナイキスト定理から2.4倍しかサンプリング余裕がなく、エッジ捕捉精度が低下する

**Source（情報源）**:
- [community/tier3] [fx2lafw - sigrok Wiki](https://sigrok.org/wiki/Fx2lafw) (2026-03-14) — primary: false
- [community/tier3] [Using the USB Logic Analyzer with sigrok PulseView - SparkFun](https://learn.sparkfun.com/tutorials/using-the-usb-logic-analyzer-with-sigrok-pulseview/all) (2026-03-14) — primary: false
- [official/tier2] [EZ-USB FX2LP Technical Reference Manual - GitHub Mirror](https://github.com/ttopal/EZ-USB-FX2LP-CY7C68013A-56PVXC-mini-Board/blob/main/fx2_techrefmanual.pdf) (2026-03-14) — primary: true

**Confidence（確信度）**: high（スペック上限については high、実効値については medium ＊個体差・USB環境に依存）

---

### 1-5. Comparative Spec Matrix（スペック比較マトリクス）

| 項目 | LA2016 | LA1010 | DSLogic Plus | Generic FX2 |
|:---|:---:|:---:|:---:|:---:|
| チャネル数 | 16 | 16 | 16 | 8（〜16） |
| 最大サンプリングレート | **200MHz（全ch）** | 16MHz（全16ch）/ 100MHz（3ch） | 100MHz（16ch, Buffer）/ 400MHz（4ch, Buffer） | ~16-24MHz（8ch持続） |
| ハードウェアメモリ | **1Gbit（128MB）** | **なし** | 256Mbit（32MB） | **なし** |
| 入力電圧保護 | **±50V** | **±50V** | ±30V | **なし（3.3/5V のみ）** |
| スレッショルド範囲 | -4V〜+4V | -4V〜+4V | 0V〜5V | 固定 |
| スレッショルド分解能 | 0.01V | 0.01V | 0.1V | — |
| 最小パルス幅 | 12.5ns | 20ns | **5ns** | ~40ns |
| PWM出力 | ◎（2ch, 20MHz） | ◎（2ch, 10MHz） | × | × |
| 推定価格 | ¥27,000前後 | ¥13,000前後 | ¥28,000〜（在庫不安定） | ¥1,000〜2,000 |

---

## Section 2: Mac Apple Silicon Software Compatibility（Macソフトウェア互換性）

### 2-1. KingstVIS（Kingst LA1010 / LA2016 純正ソフトウェア）

**Claim（主張）**: KingstVISのmacOS版はIntel（x86_64）バイナリのみ提供されており、Apple Silicon上では**Rosetta 2経由でのみ動作する**。現時点では機能的に動作するが、Rosettaの将来的な廃止リスクを内包する。

**Evidence（根拠）**:
- KingstVIS公式ダウンロードページでは「macOS 10.12以上」と記載されているが、ARM64/Universal Binaryの提供は確認されていない
- Apple Silicon Mac上での動作報告はRosetta 2経由が前提（ユーザーコミュニティの報告）
- **Rosetta 2廃止リスク**: Appleは公式サポートページにおいてRosetta 2の継続サポートを「将来のmacOS」で保証しておらず、macOS 28以降での廃止可能性が技術系メディアで報告されている
- macOS Sonoma以降のアップデート後にRosetta 2アプリが起動しなくなる事例が散発的に報告されている（再インストールで通常は解決）

**Source（情報源）**:
- [official/tier2] [KingstVIS Download - qdkingst.com](http://www.qdkingst.com/en/download) (2026-03-14) — primary: true
- [official/tier1] [Using Intel-based apps on a Mac with Apple silicon - Apple Support](https://support.apple.com/en-us/102527) (2026-03-14) — primary: true
- [community/tier3] [Fixed! Rosetta 2 Support Issue After The macOS Sonoma Update - iboysoft.com](https://iboysoft.com/tips/rosetta-support-in-macos-sonoma.html) (2026-03-14) — primary: false

**Confidence（確信度）**: high（現在の動作については high、将来の廃止時期については low＝推測段階）

---

### 2-2. sigrok / PulseView（オープンソース）— 実態と誤解の整理

**Claim（主張）**: PulseViewに「Apple Silicon ARM64ネイティブバイナリ」が正式提供されているという前ステップの記述は**不正確**であり、現時点でも公式ARM64バイナリは存在しない。正しくは「x86_64ビルドをRosetta 2で動作させる」または「ソースからビルドする」のいずれかとなる。

**Evidence（根拠）**:
- sigrok公式ダウンロードページに記載のmacOS向けDMGはx86_64のみ
- sigrok公式macOS Wikiページでも、Apple Silicon向けには「x86_64版をRosetta 2で使う」または「Homebrewからビルド」と案内
- Apple Silicon向けHomebrew環境でのビルドは技術的には可能だが、Qt依存関係・gettextのパス解決に手動対応が必要
- 公式Nightlyビルドは存在するが、ARM64 Nightly DMGの提供は確認できず（前ステップの「ARM64 Nightlyビルド提供」記述は情報源不明）
- コミュニティ作成のGist（[Install PulseView on Apple M1](https://gist.github.com/mandrean/fbe00ac1fc11b1b2625e8e9e53c1f7dc)）がWorkaroundとして参照されるが、依存関係のパスが頻繁に変わり、メンテナンス追従が必要

**動作実績まとめ（2024年時点）**:

| アプローチ | 方法 | 難易度 | 安定性 |
|:---|:---|:---:|:---:|
| 公式DMG（x86_64）+ Rosetta 2 | 最も簡単（推奨） | ★☆☆ | ◎ |
| Homebrew（ARM64ネイティブビルド） | 依存関係の手動解決が必要 | ★★★ | △ |
| ソースコンパイル（ARM64） | 高度な知識が必要 | ★★★ | △ |

**Source（情報源）**:
- [official/tier2] [Downloads - sigrok](https://sigrok.org/wiki/Downloads) (2026-03-14) — primary: true
- [official/tier2] [Mac OS X - sigrok Wiki](https://sigrok.org/wiki/Mac_OS_X) (2026-03-14) — primary: true
- [community/tier4] [Install PulseView (& gettext) on Apple M1 Silicon - GitHub Gist](https://gist.github.com/mandrean/fbe00ac1fc11b1b2625e8e9e53c1f7dc) (2026-03-14) — primary: false
- [community/tier3] [Running Sigrok PulseView on an M1 Mac - nishtahir.com](https://nishtahir.com/running-pulseview-on-an-m1-mac/) (2026-03-14) — primary: false

**Confidence（確信度）**: high（公式DMGの状況については high、ARM64ビルドの安定性については medium）

---

### 2-3. sigrok対応状況：製品別の深掘り

#### LA2016 + sigrok/PulseView

**Claim（主張）**: LA2016はsigrokで**正式サポート**されているが、**プロプライエタリなファームウェアブロブの事前抽出が必須**であり、ゼロからのセットアップには追加作業が生じる。

**Evidence（根拠）**:
- sigrok公式ドライバ名: `kingst-la2016`（LA2016, LA1016, LA5016, LA5032をカバー）
- デバイス接続の度にプロプライエタリファームウェアをFPGAにアップロードが必要
- ファームウェアはKingstVISインストーラーから抽出ツール（OSS）で取得可能:
  - [kingst-sigrok-tool (AnotherWayIn)](https://github.com/AnotherWayIn/kingst-sigrok-tool)
  - [kingst-sigrok-tool (sageoffensive)](https://github.com/sageoffensive/kingst-sigrok-tool)
- 2022年初頭以降、ドライバの安定性は大幅に改善されメンテナンス継続中
- FPGA完全オープンソース化（ビットストリーム）は未完了（進行中）

**実用上の意味**: KingstVIS（Rosetta 2版）をまずインストールし、ファームウェアを抽出してからsigrokで使うという**2段階セットアップ**が必要。

**Source（情報源）**:
- [community/tier3] [Kingst LA2016 - sigrok Wiki](https://sigrok.org/wiki/Kingst_LA2016) (2026-03-14) — primary: false
- [community/tier3] [Kingst LA Series - sigrok Wiki](https://www.sigrok.org/wiki/Kingst_LA_Series) (2026-03-14) — primary: false
- [community/tier3] [kingst-sigrok-tool - GitHub](https://github.com/AnotherWayIn/kingst-sigrok-tool) (2026-03-14) — primary: false

**Confidence（確信度）**: high

---

#### LA1010 + sigrok/PulseView ⚠️ 重大な非互換

**Claim（主張）**: LA1010は**sigrokメインラインに正式サポートされていない**。使用可能な非公式フォークが存在するが、LA2016サポートとの共存は不可。

**Evidence（根拠）**:
- sigrokメインラインの`kingst-la2016`ドライバにおいてLA1010は「**untested（未テスト）**」ステータス
- LA1010はLA2016と異なるFPGA（Xilinxベース）を使用しており、別途ドライバ開発が必要
- 非公式フォーク:
  - [GitHub - AlexUg/sigrok](https://github.com/AlexUg/sigrok)（LA1010サポートあり、ただしLA2016サポートを除去）
  - [GitHub - Gatodillo/sigrok_la1010](https://github.com/Gatodillo/sigrok_la1010)
- sigrok開発者メーリングリスト（2023年1月）にてLA1010サポートの困難さが確認されている

**実用上の意味**: LA1010ユーザーがsigrokを使いたい場合、非公式フォークのビルドが必要。LA2016（メインライン対応）とは異なる手間が生じる。

**Source（情報源）**:
- [community/tier3] [Thread: [sigrok-devel] kingst la1010 - SourceForge](https://sourceforge.net/p/sigrok/mailman/sigrok-devel/thread/20230131184537.GC4396%40book.gsilab.sittig.org/) (2026-03-14) — primary: false
- [community/tier3] [GitHub - AlexUg/sigrok (LA1010 fork)](https://github.com/AlexUg/sigrok) (2026-03-14) — primary: false

**Confidence（確信度）**: high

---

#### Generic FX2 + sigrok/PulseView

**Claim（主張）**: FX2ベースの汎用クローンは、sigrokとの互換性が**最も良好**であり、Mac Apple Silicon環境でも最も摩擦の少ない運用が可能。

**Evidence（根拠）**:
- sigrokの`fx2lafw`ドライバは、FX2チップへの**オープンソースファームウェア**（プロプライエタリ不要）を書き込むことで動作
- ファームウェア抽出ツール不要、追加セットアップ作業不要
- Rosetta 2 + x86_64版PulseViewで動作実績が豊富
- USBデバイスクラスが標準的（libusb経由）なため、macOS上でのドライバ問題が発生しにくい
- sigrok対応ハードウェアリストに多数の類似製品が掲載

**Source（情報源）**:
- [official/tier2] [fx2lafw - sigrok Wiki](https://sigrok.org/wiki/Fx2lafw) (2026-03-14) — primary: false
- [official/tier2] [Supported hardware - sigrok](https://sigrok.org/wiki/Supported_hardware) (2026-03-14) — primary: false

**Confidence（確信度）**: high

---

#### DSLogic Plus + DSView / sigrok

**Claim（主張）**: DSViewはmacOS対応を公式に謳うが、ARM64ネイティブバイナリは正式リリースされていない。sigrokとの互換性はドライバレベルで確認されている。

**Evidence（根拠）**:
- DSView v1.3.2（2024年4月リリース）がmacOS向け最新版だが、ARM64/Universal Binary明示なし
- 公式サイトのmacOS用ダウンロードはx86_64が主体と見られる（ARM64判別の明示なし）
- ソースはCMake + Qtで構成されており、ARM64向けセルフビルドは技術的に可能
- sigrok Wikiに[DreamSourceLab DSLogic Plus](https://sigrok.org/wiki/DreamSourceLab_DSLogic_Plus)のエントリあり、一定のサポートが確認される

**Source（情報源）**:
- [official/tier2] [Download - DreamSourceLab](https://www.dreamsourcelab.com/download/) (2026-03-14) — primary: true
- [official/tier2] [DSView Releases - GitHub](https://github.com/DreamSourceLab/DSView/releases) (2026-03-14) — primary: true
- [community/tier3] [DreamSourceLab DSLogic Plus - sigrok Wiki](https://sigrok.org/wiki/DreamSourceLab_DSLogic_Plus) (2026-03-14) — primary: false

**Confidence（確信度）**: medium（ARM64対応状況の確認が間接的なため）

---

### 2-4. Software Compatibility Matrix（ソフトウェア互換性マトリクス）

| 製品 | 純正ソフト | macOS動作方式 | sigrok対応 | sigrokセットアップ難度 |
|:---|:---:|:---:|:---:|:---:|
| **LA2016** | KingstVIS | Rosetta 2（x86_64） | ◎ メインライン正式対応 | ★★☆（FW抽出が必要） |
| **LA1010** | KingstVIS | Rosetta 2（x86_64） | △ 非公式フォークのみ | ★★★（フォークビルド必要） |
| **DSLogic Plus** | DSView | Rosetta 2（x86_64）推定 | ○ 対応（要確認） | ★★☆ |
| **Generic FX2** | なし（OEM版Saleae互換） | Rosetta 2（PulseView x86_64） | ◎ 最良（OSSファームウェア） | ★☆☆（最も簡単） |

---

## Section 3: Risk Analysis（リスク分析）

### 3-1. Rosetta 2依存リスク

**Claim（主張）**: 全ての有償ロジックアナライザーの純正ソフトウェアがRosetta 2に依存しており、将来のmacOSアップデートによって動作不能になるリスクが存在する。

**Evidence（根拠）**:
- Appleの公式ページは「将来のバージョンのmacOSでRosettaが利用できなくなる可能性がある」と言及（明示的な廃止日程は未発表）
- 「macOS 28以降での廃止」という推測報道が複数のテック系メディアで取り上げられているが、**Apple公式の確定情報ではない**（推測・未確定）
- macOS Sonomaアップデート後にRosettaアプリ起動失敗の事例が散発的に報告されているが、再インストールで解決するケースが多い

**Source（情報源）**:
- [official/tier1] [Using Intel-based apps on a Mac with Apple silicon - Apple Support](https://support.apple.com/en-us/102527) (2026-03-14) — primary: true
- [community/tier3] [Fixed! Rosetta 2 Support Issue After The macOS Sonoma Update - iboysoft.com](https://iboysoft.com/tips/rosetta-support-in-macos-sonoma.html) (2026-03-14) — primary: false

**Confidence（確信度）**: medium（廃止リスクの存在は high、具体的な廃止時期は low＝推測）

> **実務的対処**: macOSのメジャーアップデート前に、使用中のロジックアナライザーソフトのARM64対応状況を確認してからアップデートするのが最も安全。

### 3-2. LA1010のsigrok非対応リスク

**Claim（主張）**: LA1010を購入した場合、sigrokエコシステムの恩恵（プロトコルデコーダーの豊富さ・オープンソース）が実質的に利用できず、KingstVIS（Rosetta 2）への完全依存が強制される。

**Evidence（根拠）**:
- sigrokメインラインの「untested」ステータスは、積極的な開発者がいない限り改善されない
- 非公式フォーク（AlexUg/Gatodillo）はLA2016サポートを犠牲にした専用ビルドであり、長期メンテナンスが保証されない
- LA2016はメインラインサポートあり、同等の追加作業（FW抽出）でsigrokが使える

**Source（情報源）**:
- [community/tier4] [Thread: kingst la1010 - sigrok-devel SourceForge](https://sourceforge.net/p/sigrok/mailman/sigrok-devel/thread/20230131184537.GC4396%40book.gsilab.sittig.org/) (2026-03-14) — primary: false

**Confidence（確信度）**: high

---

## Section 4: Synthesis and Recommendations（統合と提言）

### MCU開発用途（SPI/UART/I2C）に向けた判断

**コストパフォーマンス最優先（〜¥3,000）**:
- **Generic FX2クローン（fx2lafw対応品）**: sigrok互換性が最良、Mac Apple SiliconでRosetta 2経由のPulseViewが最も確実に動作。入力保護がないため、テスト対象がSTM32/ESP32の3.3V系信号のみであれば実用上の問題は少ない。ただし10MHz超のSPI解析では信頼性に限界。

**バランス重視（〜¥15,000）**:
- **Kingst LA2016**（¥27,000前後）が実質的な推奨。LA1010（¥13,000）はsigrok非対応・ハードウェアメモリなしという2重の制約があり、LA2016との価格差を正当化できる追加価値がない。LA2016はsigrokメインライン正式対応・128MBハードウェアメモリ・200MHz全ch対応という明確な優位性がある。

**性能最優先（〜¥30,000）**:
- **DSLogic Plus**: 最短5nsパルス検出・バッファモード400MHz（4ch）は3製品中最高スペック。ただし日本国内での在庫安定性と、ARM64ネイティブバイナリが未提供という点（Rosetta 2依存）は共通課題。

### 前提条件（Mac Mini M4, MCU開発）への補足

- **sigrokを長期的に活用したい場合**: LA2016（FW抽出要）またはGeneric FX2クローン（FW抽出不要）が選択肢。LA1010はこの観点で明確に不利。
- **純正ソフトウェアのみで運用する場合**: LA2016またはDSLogic PlusはRosetta 2でKingstVIS/DSViewが動作。ただしRosetta廃止時のリスクを意識した定期的な互換性確認を推奨（SHOULD）。
- **3.3V/5V系MCUのみを対象とする場合**: 全製品で入力電圧は問題なし。ただしGeneric FX2クローンは過電圧保護がなく誤接続リスクが高いため、LAのプローブケーブル管理に注意が必要。
