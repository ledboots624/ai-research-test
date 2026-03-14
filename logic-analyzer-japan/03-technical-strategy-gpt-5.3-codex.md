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
**Evidence（根拠）**: UM10204の立上り制約（Fm+: 120ns, Hs: 80ns）に対し、12–13pFプローブは無視しにくい追加負荷。  
**Requirement Level（要件レベル）**: **MUST**（UM10204タイミング準拠）  
**Standard Reference Mapping（規格参照）**: UM10204 timing（direct）  
**Source（情報源）**: S3, S9, S10, S11  
**Confidence（確信度）**: high
## Requirement Levels & Standard Mapping（要件レベルと規格マッピング）

| 項目 | 参照 | 対応度 | Requirement Level | 実務解釈 |
|---|---|---|---|---|
| 標本化下限 | Shannon 1949 | direct | MUST | 2x未満は原理的に不適格 |
| デコード実務余裕 | Saleae FAQ | partial | SHOULD / WANT | 4xは実務推奨、10xは高精度志向 |
| I2C速度・立上り | NXP UM10204 | direct | MUST | 規格値超過は非準拠 |
| MCU側SPI上限 | ST/ESP/Microchip公式資料 | direct | MUST | 測定器はDUT上限を下回ってはならない |

**Claim（主張）**: 本件のSHOULDは法令準拠用語ではなく測定品質ガイドであり、EU CRA等の規制SHOULDとは重みが異なる。  
**Source（情報源）**: S2（測定ガイド由来）, S3（規格MUSTとの対比）  
**Confidence（確信度）**: medium
## Protocol Demand Baseline（対象プロトコル要求帯域）

**Claim（主張）**: SPI要求帯域は対象MCUで広く、Arduino級8MHzからESP32級80MHz、STM32H7級では100MHz超まで到達しうる。  
**Evidence（根拠）**: ATmega328Pはfosc/2、ESP-IDFは80MHz級設定、STM32H743資料では100MHz超のSPI特性が示される。  
**Source（情報源）**: S4, S5, S6  
**Confidence（確信度）**: high（Arduino/ESP32）, medium（STM32全体一般化）
**Claim（主張）**: I2Cは規格上100k/400k/1M/3.4M（+UFm 5M）で、LA側サンプリング要求はSPIより低い。  
**Evidence（根拠）**: UM10204に各モードが規定される。  
**Source（情報源）**: S3  
**Confidence（確信度）**: high
**Claim（主張）**: UART高速域は環境依存が大きく、5Mbpsは**未確定（推測を含む）**として扱うのが安全。  
**Evidence（根拠）**: ESP-IDF APIは設定可能だが、上限の実運用安定性は配線/USB-UART等依存。  
**Source（情報源）**: S7, S8  
**Confidence（確信度）**: medium
## Sampling Fit Analysis（サンプリング適格性判定）

**判定基準**: MUST=2x, SHOULD=4x, WANT=10x（SPIは4ch: SCLK/MOSI/MISO/CS想定）

| Device | 実効fs（4ch想定） | 20MHz SPI | 40MHz SPI | 80MHz SPI | 133MHz SPI |
|---|---:|---|---|---|---|
| Kingst LA2016 | 200MHz | WANT（10x） | SHOULD（5x） | MUSTのみ（2.5x） | MUST未達（1.5x） |
| Kingst LA1010 | 50MHz | MUSTのみ（2.5x） | MUST未達（1.25x） | MUST未達 | MUST未達 |
| DSLogic Plus（Stream） | 50MHz | MUSTのみ（2.5x） | MUST未達 | MUST未達 | MUST未達 |
| DSLogic Plus（Buffer） | 400MHz | WANT（20x） | WANT（10x） | SHOULD（5x） | MUSTのみ（3.0x） |
| FX2系 24MHz | 24MHz | MUST未達（1.2x） | MUST未達 | MUST未達 | MUST未達 |

**Claim（主張）**: 40MHz以上のSPIを実務品質（SHOULD）で追うなら、事実上LA2016またはDSLogic Plus(Buffer)が必要。  
**Source（情報源）**: S2, S9, S10, S11, S12  
**Confidence（確信度）**: high
## Electrical Characteristics（電気特性・プローブ品質）

**Claim（主張）**: 抵抗性負荷（220kΩ/250kΩ）はMCU信号に対して概ね軽微だが、容量性負荷（12–13pF）はI2C高速モードで支配的。  
**Evidence（根拠）**: 入力Rは高く直流負荷は小さい。一方I2C立上りはRC依存で、追加容量がそのままtr増加につながる。  
**Source（情報源）**: S3, S9, S10, S11  
**Confidence（確信度）**: high
**Claim（主張）**: 4.7kΩプルアップ時、12pFプローブ1本でI2C立上りに約48ns追加（13pFで約52ns）となり、Fm+/Hsでは無視しにくい。  
**Evidence（根拠）**: UM10204式 `Rp(max)=tr/(0.8473*Cbus)` を変形し、`Δtr≈0.8473*R*ΔC` で算出。  
**Requirement Level（要件レベル）**: **MUST**（I2C規格タイミング遵守）  
**Standard Reference Mapping（規格参照）**: UM10204（direct）  
**Source（情報源）**: S3, S9, S10, S11  
**Confidence（確信度）**: high
**Claim（主張）**: 保護耐圧と閾値可変性は、現場デバッグの事故耐性と異常波形の判読性に直結する。  
**Evidence（根拠）**: LA2016/LA1010は±50V保護・細かい閾値設定、DSLogic Plusは±30V、FX2系は保護・閾値可変が弱い傾向。  
**Source（情報源）**: S9, S10, S11, S12, S13  
**Confidence（確信度）**: high（FX2系の個体差部分は medium）
## Device Verdicts（機種別技術判定）

**Kingst LA2016**  
**Claim（主張）**: 3万円級で最もバランスが良く、`SPI 40MHzまでの実用解析`に強い。  
**Evidence（根拠）**: 16ch同時200MHz、1Gbitメモリ、12pF入力、閾値可変。  
**Source（情報源）**: S9  
**Confidence（確信度）**: high
**Kingst LA1010**  
**Claim（主張）**: 低コストだが、高速SPIでは早期に頭打ち。I2C/UART中心なら有効。  
**Evidence（根拠）**: 4ch相当で50MHz、16chで16MHz。  
**Source（情報源）**: S10  
**Confidence（確信度）**: high
**DSLogic Plus**  
**Claim（主張）**: 高速SPI（80MHz級）を見たいなら最有力。ただし長時間はStream制約で実効が落ちる。  
**Evidence（根拠）**: Buffer時4ch 400MHz、Stream時はch数依存で低下。  
**Source（情報源）**: S11  
**Confidence（確信度）**: high
**FX2系クローン**  
**Claim（主張）**: I2C/UARTや低速SPIの学習用途には有効だが、高速SPI・耐故障性要求には不向き。  
**Evidence（根拠）**: 24MHz級・低価格・オープンソース対応だが、保護や実効帯域は限定。  
**Source（情報源）**: S12, S13  
**Confidence（確信度）**: medium
## Conclusion & Recommendations（結論・推奨）

**Claim（主張）**: 技術戦略上の第一候補は`LA2016`、高速SPI（80MHz級）を本格追跡するなら`DSLogic Plus`を優先。  
**Evidence（根拠）**: サンプリング適格性（MUST/SHOULD/WANT）と電気的余裕（保護・閾値・容量負荷）を総合評価。  
**Source（情報源）**: S2, S3, S9, S10, S11, S12, S13  
**Confidence（確信度）**: high
**前提条件プロファイル向け補足（Mac Mini M4 / STM32・ESP32）**  
- SPIが主戦場で`20–40MHz`中心: **LA2016**がコスパ最適。  
- `80MHz`近傍を継続的に追う: **DSLogic Plus（Buffer前提）**。  
- 予算最小でI2C/UART中心: **LA1010**。  
- FX2系は「予備機/入門機」として割り切るのが安全。  
## Source Register（情報源台帳）

- **S1**  
  source_type: academic_paper  
  source_title: Communication in the Presence of Noise  
  source_locator: [Shannon 1949 PDF](https://www.princeton.edu/~wbialek/rome/refs/shannon_1949.pdf)  
  access_date: 2026-03-14  
  reliability_tier: tier2  
  primary_source: true

- **S2**  
  source_type: official_document  
  source_title: What Sampling Rate Should I Use?  
  source_locator: [Saleae Support FAQ](https://support.saleae.com/getting-help/troubleshooting/technical-faq/what-sample-rate-is-required)  
  access_date: 2026-03-14  
  reliability_tier: tier2  
  primary_source: true

- **S3**  
  source_type: official_document  
  source_title: UM10204 I2C-bus specification and user manual  
  source_locator: [NXP UM10204](https://www.nxp.com/docs/en/user-guide/UM10204.pdf)  
  access_date: 2026-03-14  
  reliability_tier: tier1  
  primary_source: true

- **S4**  
  source_type: official_document  
  source_title: ATmega328P Datasheet  
  source_locator: [Microchip ATmega328P](https://ww1.microchip.com/downloads/en/DeviceDoc/Atmel-7810-Automotive-Microcontrollers-ATmega328P_Datasheet.pdf)  
  access_date: 2026-03-14  
  reliability_tier: tier2  
  primary_source: true

- **S5**  
  source_type: official_document  
  source_title: ESP-IDF SPI Master Driver (ESP32)  
  source_locator: [Espressif SPI Master](https://docs.espressif.com/projects/esp-idf/en/stable/esp32/api-reference/peripherals/spi_master.html)  
  access_date: 2026-03-14  
  reliability_tier: tier2  
  primary_source: true

- **S6**  
  source_type: official_document  
  source_title: STM32H743xI/G Datasheet  
  source_locator: [ST STM32H743BI Datasheet](https://www.st.com/resource/en/datasheet/stm32h743bi.pdf)  
  access_date: 2026-03-14  
  reliability_tier: tier2  
  primary_source: true

- **S7**  
  source_type: official_document  
  source_title: ESP-IDF UART API Reference  
  source_locator: [Espressif UART](https://docs.espressif.com/projects/esp-idf/en/stable/esp32/api-reference/peripherals/uart.html)  
  access_date: 2026-03-14  
  reliability_tier: tier2  
  primary_source: true

- **S8**  
  source_type: community_data  
  source_title: Maximum UART baud rate (ESP32 Forum)  
  source_locator: [ESP32 Forum Thread](https://esp32.com/viewtopic.php?t=12582)  
  access_date: 2026-03-14  
  reliability_tier: tier4  
  primary_source: false  
  community_score:  
  - discussion_age: "multiple years"  
  - participant_count: "unknown"  
  - expert_presence: false  
  - reproducibility: true

- **S9**  
  source_type: official_document  
  source_title: Kingst LA2016 Product Page  
  source_locator: [QD Kingst LA2016](http://www.qdkingst.com/en/products/LA2016)  
  access_date: 2026-03-14  
  reliability_tier: tier2  
  primary_source: true

- **S10**  
  source_type: official_document  
  source_title: Kingst LA1010 Product Page  
  source_locator: [QD Kingst LA1010](http://www.qdkingst.com/en/products/LA1010)  
  access_date: 2026-03-14  
  reliability_tier: tier2  
  primary_source: true

- **S11**  
  source_type: official_document  
  source_title: DSLogic Plus Datasheet  
  source_locator: [DreamSourceLab DSLogic Plus Datasheet](https://www.dreamsourcelab.com/doc/DSLogic_Plus_Datasheet.pdf)  
  access_date: 2026-03-14  
  reliability_tier: tier2  
  primary_source: true

- **S12**  
  source_type: community_data  
  source_title: fx2lafw (sigrok Wiki)  
  source_locator: [sigrok fx2lafw](https://sigrok.org/wiki/Fx2lafw)  
  access_date: 2026-03-14  
  reliability_tier: tier4  
  primary_source: false  
  community_score:  
  - discussion_age: "10+ years"  
  - participant_count: "unknown"  
  - expert_presence: true  
  - reproducibility: true

- **S13**  
  source_type: industry_report  
  source_title: Using the USB Logic Analyzer with sigrok PulseView  
  source_locator: [SparkFun Tutorial](https://learn.sparkfun.com/tutorials/using-the-usb-logic-analyzer-with-sigrok-pulseview/all)  
  access_date: 2026-03-14  
  reliability_tier: tier3  
  primary_source: false
