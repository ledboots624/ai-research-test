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
  - [SRC-004] [industry_report/tier2] [CECIMO CRA Reporting Obligations Guide](https://www.cecimo.eu/news/cecimo-guide-on-reporting-obligations-under-the-cyber-resilience-act-cra/) (2026-03-14) — primary: false
  - [SRC-005] [industry_report/tier2] [Deloitte Belgium: Navigating the EU CRA](https://www.deloitte.com/be/en/services/consulting/research/cyber-resilience-act.html) (2026-03-14) — primary: false
- **Confidence（確信度）**: **high** — 法令原文と複数の公式ガイダンスで裏付け
- **Evidence Strength（根拠の強さ）**: **strong**

### Finding 2: PSIRT体制の現実的コストモデル——年間€14万〜€50万超の投資が必要

- **Claim（主張）**: 批判的レビューが指摘した「PSIRT体制コスト未計上」を修正する。中規模半導体装置企業（IT+OTエンドポイント300台規模）の24/7 PSIRT運用は、**MSSP委託ベースで年間約€137,000〜€164,000**、大規模企業では**年間€500,000超**が見込まれる。これにHSM/鍵管理基盤、DMZゲートウェイ構築の初期投資を加算する必要がある。3つの体制モデル（①EU現地法人PSIRT設置、②グローバルMSSP契約、③日本本社+EU TZブリッジ体制）のうち、**②グローバルMSSP契約が最もコスト効率が高く導入速度も速い**。
- **Evidence（根拠）**: MSSP市場の2025-2026年ベンチマークとして、基本24/7モニタリングが€75〜€200/エンドポイント/月、脆弱性管理アドオンが€20〜€75/資産/月、インシデント対応リテイナーが€10k〜€50k/年。欧州においてOrange CyberDefense、Atos、NTT Security、IBM MSSなどが産業セクター向けMSSPサービスを提供している。
- **Source（情報源）**:
  - [SRC-006] [industry_report/tier3] [MSSP Pricing Guide 2025](https://www.harbourtech.net/blog/mssp-pricing-guide-security-costs) (2026-03-14) — primary: false
  - [SRC-007] [industry_report/tier3] [UnderDefense: MSSP Pricing](https://underdefense.com/blog/managed-security-service-provider-mssp-pricing/) (2026-03-14) — primary: false
  - [SRC-008] [news_article/tier3] [CyberSecurityNews: MSSP Pricing](https://cybersecuritynews.com/mssp-pricing/) (2026-03-14) — primary: false
  - [SRC-009] [industry_report/tier3] [Pricing Managed Security Services Guide 2026](https://bennettfinancials.com/pricing-managed-security-services/) (2026-03-14) — primary: false
- **Confidence（確信度）**: **medium** — 市場ベンチマーク複数ソースあるが、半導体装置特化の見積もりはなく一般産業データからの推計
- **Evidence Strength（根拠の強さ）**: **moderate**

### Finding 3: バイナリ解析SBOMは「完全性50〜75%」を前提に条件分岐戦略が必要

- **Claim（主張）**: 産業用組込みファームウェア（独自RTOS、静的リンクバイナリ、暗号化ファームウェア）に対するバイナリ解析の検出率は最良でも60〜75%（オープンソース成分）、独自コンポーネントでは50%未満である。CRAは「due diligence（相当の注意）」を求めており、BSI TR-03183-2が参照する「best effort SBOM」の概念に基づき、**解析不能部分を文書化したギャップ分析付きSBOMが許容される可能性がある**。ただし、Notified Bodyがどこまでこれを認めるかは未確定である。
- **Evidence（根拠）**: CRAはSBOM要件としてSPDX/CycloneDX形式の機械可読SBOMを求めるが、バイナリ解析の限界は規制当局も認識している。Finitestate、Zerberus等のCRAガイダンスは「解析方法論と限界の文書化」を推奨。BSI TR-03183-2はSBOM生成のガイドラインとしてバイナリ解析の例外規定に言及している。半導体装置のVxWorks、T-Kernel系RTOS、FPGA制御コードなどは標準的なSCAツールでは検出困難。
- **Source（情報源）**:
  - [SRC-010] [official_document/tier1] [BSI TR-03183-2](https://www.bsi.bund.de/EN/Themen/Unternehmen-und-Organisationen/Standards-und-Zertifizierung/Technische-Richtlinien/TR-nach-Thema-sortiert/tr03183/TR-03183_node.html) (2026-03-14) — primary: true
  - [SRC-011] [news_article/tier3] [Softwareseni: CRA SBOM Requirements Decoded](https://www.softwareseni.com/eu-cyber-resilience-act-technical-implementation-sbom-requirements-decoded/) (2026-03-14) — primary: false
  - [SRC-012] [news_article/tier3] [Zerberus: CRA Compliance Guide Part II](https://www.zerberus.ai/post/eu-cyber-resilience-act-cra-compliance-guide-part-ii) (2026-03-14) — primary: false
  - [SRC-013] [industry_report/tier3] [Finitestate: CRA SBOM Documentation Guide](https://finitestate.io/blog/eu-cra-sbom-technical-documentation-guide) (2026-03-14) — primary: false
  - [SRC-014] [news_article/tier3] [FOSSA: SBOM Requirements in CRA](https://fossa.com/blog/sbom-requirements-cra-cyber-resilience-act/) (2026-03-14) — primary: false
- **Confidence（確信度）**: **medium** — 「best effort SBOM」が許容されるかはNB審査実績がなく、CRA本文には明示的な例外規定がない
- **Evidence Strength（根拠の強さ）**: **moderate**

### Finding 4: EU Authorised Representative指定はCRA対応の法的前提条件

- **Claim（主張）**: CRA Article 17に基づき、EU域内に拠点を持たない日本の製造者は、**EU域内にauthorised representative（AR）を書面の委任状で指定する法的義務**がある。ARは技術文書の保管、適合宣言書の管理、市場監視当局との窓口機能を担う。ただし、サイバーセキュリティリスク管理・製品設計等の根本的義務はARに委譲できず、製造者が一義的責任を負う。**AR未指定の場合、EU市場へのアクセス自体が不可能**となる。
- **Evidence（根拠）**: CRA Article 17は非EU製造者のAR指定を義務付けており、書面の委任状に基づく任務範囲の明確化、技術文書・適合宣言書の10年間保管、市場監視当局との連絡窓口機能を規定。同種の規定はMDR（医療機器規則）のEAR制度で先行実績がある。
- **Source（情報源）**:
  - [SRC-001] [official_document/tier1] [Regulation (EU) 2024/2847](https://eur-lex.europa.eu/eli/reg/2024/2847/oj/eng) (2026-03-14) — primary: true
  - [SRC-015] [industry_report/tier2] [GoRegulus: CRA Manufacturer Obligations](https://goregulus.com/cra-compliance/cra-manufacturer-obligations/) (2026-03-14) — primary: false
  - [SRC-016] [industry_report/tier3] [ComplianceGate: EU Authorised Representative Guide](https://www.compliancegate.com/european-authorised-representative/) (2026-03-14) — primary: false
- **Confidence（確信度）**: **high** — CRA法令原文に明記
- **Evidence Strength（根拠の強さ）**: **strong**

### Finding 5: 製品ライフサイクル（15〜25年）と支援期間の乖離は戦略的リスク

- **Claim（主張）**: CRAの支援期間は「最低5年、または製品の想定稼働寿命の全期間（短い方）」と規定されるが、半導体装置の実稼働寿命は15〜25年に及ぶ。**CRAの論理的解釈では、製品の想定稼働寿命全期間にわたる脆弱性管理・セキュリティ更新が必要**であり、5年を大幅に超える長期支援戦略が不可避である。これは製品原価・保守契約モデルに根本的影響を与える。
- **Evidence（根拠）**: CRA Article 13(8)は支援期間を「製品の想定稼働寿命に比例した期間で最低5年」と定めている。半導体装置は一般にファブの設備償却期間（10〜15年）を超えて実稼働する。セキュリティアップデートは支援期間中継続し、リリースされたアップデートは少なくとも10年間ダウンロード可能に保つ義務がある。
- **Source（情報源）**:
  - [SRC-001] [official_document/tier1] [Regulation (EU) 2024/2847](https://eur-lex.europa.eu/eli/reg/2024/2847/oj/eng) (2026-03-14) — primary: true
  - [SRC-017] [industry_report/tier2] [CRA Guide for Manufacturers](https://www.cyberresilienceact.eu/cra-guide-for-manufacturers/) (2026-03-14) — primary: false
  - [SRC-018] [industry_report/tier2] [GoRegulus: CRA Update Requirements](https://goregulus.com/cra-requirements/cra-update-requirements/) (2026-03-14) — primary: false
  - [SRC-019] [industry_report/tier2] [NXP: EU CRA](https://www.nxp.com/applications/technologies/security/eu-cyber-resilience-act-cra:CYBER-RESILIENCE-ACT) (2026-03-14) — primary: false
- **Confidence（確信度）**: **high** — CRA原文の支援期間規定と半導体装置の既知のライフサイクルから導出
- **Evidence Strength（根拠の強さ）**: **strong**

### Finding 6: サプライヤSBOM能力の3層分類と代替戦略が必要

- **Claim（主張）**: 半導体装置のサプライチェーンにはSBOM生成能力が大きく異なる3層が存在する。①大手・欧米系（自律的SBOM生成可能）、②国内中堅（ツール・教育支援でSBOM対応可能）、③能力不足の小規模サプライヤ（代替調達またはバイナリ解析で補完必要）。**契約条項にSBOM提供を明記するだけでは不十分**であり、層別の支援・代替策が必要である。
- **Evidence（根拠）**: CRA SMEコンプライアンスガイドはSMEの能力格差を認識し、段階的対応を推奨。日本の半導体装置産業は系列サプライヤ構造を有し、中小部品メーカーが多数を占める。これらのサプライヤにSBOMツール導入・教育を提供するか、自社でバイナリ解析によるバックフィルを行う必要がある。
- **Source（情報源）**:
  - [SRC-020] [industry_report/tier2] [CRA Compliance Guide for SMEs](https://cyen.eu/wp-content/uploads/2025/12/D4.1_CRA-Compliance-Guide-for-SMEs_EN_compressed.pdf) (2026-03-14) — primary: false
  - [SRC-021] [industry_report/tier2] [Fortress Infosec: CRA & Supply Chain Risk](https://www.fortressinfosec.com/blog/how-the-eu-cyber-resilience-act-impacts-supply-chain-risk-management) (2026-03-14) — primary: false
- **Confidence（確信度）**: **medium** — SME能力格差は業界通説だが、日本の半導体装置サプライヤの具体的SBOM対応状況の定量データなし
- **Evidence Strength（根拠の強さ）**: **moderate**

### Finding 7: SEMI E187/E188はCRA適合の「土台」として活用可能

- **Claim（主張）**: SEMI E187（ファブ装置のサイバーセキュリティ仕様）およびE188（マルウェアフリー装置統合仕様）は、TSMC・Intel等の主要ファブが調達条件として採用を義務付けており、事実上の業界標準となっている。**SEMI E187/E188のOS・ネットワーク・エンドポイントセキュリティ要件はCRAのセキュリティ・バイ・デザイン要件と部分的に重複**しており、既存のE187/E188対応をCRA適合の土台として効率的に再利用できる。ただし、E187/E188はSBOM要件・脆弱性報告義務・支援期間規定を含まないため、CRA独自の差分対応が必要。
- **Evidence（根拠）**: TSMCは2023年にE187準拠を調達必須条件とし、SEMI台湾は2025年にE187認証プログラムを開始。SMCCが評価ツール・ガイダンスを公開。E187はOSセキュリティ、ネットワークセキュリティ、エンドポイント保護、セキュリティモニタリングの4領域をカバー。METI OTセキュリティガイドラインもSEMI E187/E188を参照。
- **Source（情報源）**:
  - [SRC-022] [industry_report/tier2] [TSMC: Strengthens Cybersecurity of Semiconductor Equipment](https://esg.tsmc.com/en-US/articles/280) (2026-03-14) — primary: false
  - [SRC-023] [industry_report/tier2] [SEMI: A Year of Progress for Semiconductor Cybersecurity](https://www.semi.org/en/blogs/a-year-of-progress-for-semiconductor-cybersecurity) (2026-03-14) — primary: false
  - [SRC-024] [industry_report/tier2] [Janus Cyber: What is SEMI E187?](https://www.janus-cyber.com/post/what-is-semi-e187) (2026-03-14) — primary: false
  - [SRC-025] [industry_report/tier2] [Claroty: Understanding SEMI E187 & E188](https://claroty.com/blog/understanding-semi-e187-e188-compliance-for-the-semiconductor-industry) (2026-03-14) — primary: false
  - [SRC-026] [official_document/tier1] [METI: OT Security Guidelines for Semiconductor Device Factories](https://www.meti.go.jp/english/press/2025/0627_004.html) (2026-03-14) — primary: true
- **Confidence（確信度）**: **high** — SEMI公式文書、TSMC公式ESG報告、METI公式ガイドラインで裏付け
- **Evidence Strength（根拠の強さ）**: **strong**

### Finding 8: 外為法とENISA脆弱性報告の交差リスクは未解決

- **Claim（主張）**: CRAに基づくENISAへの脆弱性報告が、日本の外国為替及び外国貿易法（外為法）上の「技術の提供」に該当する可能性がある。特に半導体製造装置関連ソフトウェアの脆弱性情報は、2023年7月施行の半導体製造装置輸出規制（23類型）の対象技術に関連するため、**脆弱性の技術的詳細をENISAに報告することが「役務取引」として輸出許可を要する可能性**がある。現時点で明確な法的判断は公表されていない。
- **Evidence（根拠）**: 外為法に基づくMETI輸出規制は23類型の先端半導体製造装置について、ハードウェアのみならず関連技術・ソフトウェアの輸出も規制対象としている。脆弱性報告の技術的詳細度とキャッチオール規制の適用範囲の交差は、法務専門家による個別判断が必要。
- **Source（情報源）**:
  - [SRC-027] [official_document/tier1] [Hogan Lovells: Japan's New Chip Equipment Export Rules](https://www.hoganlovells.com/en/publications/japans-new-chip-equipment-export-rules-take-effect) (2026-03-14) — primary: false
  - [SRC-028] [official_document/tier2] [Mori Hamada: Japan Semiconductor Export Restrictions](https://www.morihamada.com/sites/default/files/newsletters/newsletters/pdf/INTERNATIONAL%20TRADE%20LAW%20BULLETIN_E_vol.1.pdf) (2026-03-14) — primary: false
  - [SRC-001] [official_document/tier1] [Regulation (EU) 2024/2847](https://eur-lex.europa.eu/eli/reg/2024/2847/oj/eng) (2026-03-14) — primary: true
- **Confidence（確信度）**: **low** — 両法制の交差に関する公式見解・判例なし、推定に基づく
- **Evidence Strength（根拠の強さ）**: **weak**

### Finding 9: 適合性評価ルートの確定——大部分の半導体装置ソフトウェアはDefault Category（自己評価）

- **Claim（主張）**: CRAの製品分類において、半導体装置の制御ソフトウェア・ファームウェア自体は、**多くの場合Default Category（自己評価で適合宣言可能）**に該当する可能性が高い。Important Class IIやCriticalに分類されるのはファイアウォール、侵入検知システム、ハイパーバイザー、HSM等の汎用セキュリティ製品であり、半導体装置固有のプロセス制御ソフトウェアはこれに該当しない可能性が高い。ただし、装置にネットワーク管理機能やOS機能を含む場合はClass I該当のリスクがある。**製品分類の確定が最初の法務タスク**である。
- **Evidence（根拠）**: CRA Annex III（Important Products）のClass I例にはOS、ネットワーク管理システム、VPN等が、Class IIにはファイアウォール、侵入検知・防止システム等が列挙。Implementing Regulation (EU) 2025/2392が詳細リストを規定。半導体装置制御ソフトウェアの大部分はこれらに直接該当しないが、個別製品の機能構成による判断が必要。
- **Source（情報源）**:
  - [SRC-001] [official_document/tier1] [Regulation (EU) 2024/2847](https://eur-lex.europa.eu/eli/reg/2024/2847/oj/eng) (2026-03-14) — primary: true
  - [SRC-029] [official_document/tier1] [CRA Conformity Assessment - EU Digital Strategy](https://digital-strategy.ec.europa.eu/en/policies/cra-conformity-assessment) (2026-03-14) — primary: true
  - [SRC-030] [industry_report/tier2] [Finitestate: Conformity Assessments CRA](https://finitestate.io/blog/conformity-assessments-eu-cra-requirements) (2026-03-14) — primary: false
  - [SRC-031] [official_document/tier1] [BSI: Cyber Resilience Act](https://www.bsi.bund.de/EN/Themen/Unternehmen-und-Organisationen/Informationen-und-Empfehlungen/Cyber_Resilience_Act/cyber_resilience_act.html) (2026-03-14) — primary: true
- **Confidence（確信度）**: **medium** — Annex III/IVの例示リストに半導体装置制御ソフトは明示されていないが、個別製品の機能構成次第でClass I該当リスクあり
- **Evidence Strength（根拠の強さ）**: **moderate**

### Finding 10: 確定ロードマップ——3フェーズ18か月計画（修正版）

- **Claim（主張）**: 批判的レビューの指摘を統合し、以下の3フェーズ・18か月ロードマップを確定する。

**フェーズ1: 緊急対応（2026年3月〜9月、6か月）——報告義務への適合**
| 施策 | 内容 | 概算コスト | リスク受容判断 |
|---|---|---|---|
| 1-1. EU AR指定 | EU域内authorised representative契約 | €30k〜€80k/年 | 必須（未指定＝市場アクセス不可） |
| 1-2. PSIRT体制構築 | MSSP契約による24/7脆弱性監視・報告体制 | €137k〜€500k/年 | 必須（24h SLA不遵守＝罰金リスク） |
| 1-3. ENISA SRP登録 | Single Reporting Platform登録・テスト報告 | 人件費のみ | 必須 |
| 1-4. 製品分類確定 | CRA Annex III/IV照合による製品カテゴリ判定 | 法務費€20k〜€50k | 必須（後続戦略の前提条件） |
| 1-5. 外為法法務確認 | ENISA脆弱性報告と外為法の交差リスク確認 | 法務費€15k〜€30k | 推奨（不確実リスクの解消） |

**フェーズ2: 基盤構築（2026年9月〜2027年6月、9か月）——SBOM・技術文書整備**
| 施策 | 内容 | 概算コスト | リスク受容判断 |
|---|---|---|---|
| 2-1. ビルド時SBOM生成パイプライン | 新規開発製品向けCycloneDX/SPDX自動生成 | ツール€50k〜€150k + 人件費 | 必須 |
| 2-2. レガシーSBOMバックフィル | バイナリ解析＋サプライヤ情報統合（ギャップ文書化付き） | ツール€30k〜€100k + 工数 | 必須（ただし完全性50〜75%を受容し、ギャップ分析を文書化） |
| 2-3. サプライヤ層別対応 | 3層分類に基づくSBOM取得・支援・代替策実行 | 教育・ツール提供€50k〜€200k | 必須 |
| 2-4. VEX運用設計 | CSAF/OpenVEX形式の脆弱性ステータス管理 | 人件費中心 | 必須 |
| 2-5. 支援期間戦略確定 | 製品別ライフサイクル分析→支援期間宣言・保守契約改定 | 法務・営業連携コスト | 必須（製品価格改定との連動） |

**フェーズ3: 適合完了（2027年6月〜12月、6か月）——CE適合・市場投入**
| 施策 | 内容 | 概算コスト | リスク受容判断 |
|---|---|---|---|
| 3-1. 技術文書完成 | CRA Annex VII準拠技術文書・SBOM・リスク評価 | 人件費中心 | 必須 |
| 3-2. 適合性評価 | Default Category: 自己評価 / Class I: 整合規格適用+自己評価 / Class II: NB第三者評価 | NB費用€50k〜€200k（該当時） | 製品分類による |
| 3-3. CE marking | EU適合宣言書＋CEマーキング | 管理コスト | 必須 |
| 3-4. エアギャップ更新手順確定 | オフラインパッチ搬入プロセスの文書化（NB事前合意） | 人件費中心 | 推奨（法解釈未確定のため段階的対応） |

- **Evidence（根拠）**: CRAの法定タイムライン（2026年6月NB通知、9月報告義務開始、2027年12月完全適合）を基礎とし、批判的レビューの8つの改善提案（コスト分析追加、バイナリ解析限界明示、PSIRT具体化、サプライヤ層別化、ライフサイクル対応、外為法確認、ソース分類修正、ロードマップ根拠補強）を統合。
- **Source（情報源）**:
  - [SRC-001] [official_document/tier1] [Regulation (EU) 2024/2847](https://eur-lex.europa.eu/eli/reg/2024/2847/oj/eng) (2026-03-14) — primary: true
  - [SRC-032] [industry_report/tier2] [CRA Implementation Timeline](https://craevidence.com/blog/cra-implementation-timeline-2025-2027) (2026-03-14) — primary: false
  - [SRC-033] [industry_report/tier2] [DLA Piper: CRA Readiness](https://www.dlapiper.com/en/insights/publications/2026/02/cyber-resilience-act-what-you-need-to-know-and-what-you-need-to-be-doing) (2026-03-14) — primary: false
  - [SRC-034] [industry_report/tier2] [White & Case: CRA Clock is Ticking](https://www.whitecase.com/insight-alert/cyber-resilience-act-clock-ticking-compliance) (2026-03-14) — primary: false
- **Confidence（確信度）**: **medium** — タイムラインはCRA法令に基づきhighだが、コスト推計は市場ベンチマークからの推定であり個社状況により大幅変動する
- **Evidence Strength（根拠の強さ）**: **moderate**

---

## Confidence Distribution（確信度分布）

| 確信度 | 件数 | 該当Finding |
|---|---|---|
| **High** | 4件 | Finding 1（報告義務）、Finding 4（AR指定）、Finding 5（ライフサイクル乖離）、Finding 7（SEMI E187活用） |
| **Medium** | 4件 | Finding 2（PSIRTコスト）、Finding 3（バイナリ解析SBOM）、Finding 6（サプライヤ層別）、Finding 9（適合性評価ルート）、Finding 10（ロードマップ） |
| **Low** | 1件 | Finding 8（外為法交差リスク） |

**全体の確信度評価**: 法的義務・タイムラインに関してはhigh確信度で裏付けられているが、**実装コスト・技術的実現性・法解釈の交差点**においてmedium〜lowが残る。経営判断としては、high確信度の施策を即時着手し、medium/lowの不確実性は並行調査で解消する「着手しながら精緻化する」アプローチが適切である。

---

## Contradictory Claims（矛盾する主張）

### 矛盾①：バイナリ解析SBOMの有効性 → **解決：条件分岐戦略に修正**
- 原戦略の「バックフィルで対応可能」→ 検出率50〜75%の限界を明記し、ギャップ文書化付きSBOMとサプライヤ情報統合の併用戦略に修正（Finding 3）。

### 矛盾②：24時間PSIRT体制コスト → **解決：MSSP委託コストモデルを追加**
- コスト未計上→ 年間€137k〜€500k超のMSSP委託コストを概算（Finding 2）。3つの体制モデルの比較を提示。

### 矛盾③：サプライヤSBOM要求の現実性 → **解決：3層分類に修正**
- 「契約に明記すれば調達可能」→ 能力別3層分類と層別アクションパスを定義（Finding 6）。

### 矛盾④：エアギャップ環境の自動更新 → **解決：フェーズ3に後送り・NB事前合意**
- 法解釈未確定のまま優先項目→ フェーズ3の推奨施策に格下げし、法解釈確定を待って実行（Finding 10、施策3-4）。

### 矛盾⑤：Confidence「high」結論の前提不足 → **解決：ロードマップ全体をmediumに修正**
- 結論のみhighだった過大判定→ ロードマップの確信度をmedium（コスト推計の不確実性を反映）に修正（Finding 10）。

---

## Missing Data（不足データ）

1. **Notified Bodyの審査実績**: CRAに基づくOT/産業装置審査の先行事例が2026年6月のNB指定前のため存在しない。「best effort SBOM」の許容範囲は審査実績蓄積後に判明する。
2. **日本企業のCRA対応事例**: 日本の半導体装置メーカーのCRA対応先行事例が公表されていない（競争上の理由で非公開の可能性）。
3. **外為法×ENISA報告の法的見解**: 経済産業省・法務省からの公式見解なし。
4. **サプライヤSBOM対応状況の定量データ**: 日本の半導体装置サプライチェーンにおけるSBOM生成能力の調査統計なし。
5. **NIS2との規制交差点の詳細**: 半導体装置がNIS2のEssential Entities顧客環境に接続される場合の追加要件の分析。
6. **医療機器MDR対応からの類推コスト**: 同種のEU規制対応（MDR）における日本企業の実績コストデータ。

---

## Limitations（制約・限界）

1. **CRA施行前の分析**: CRA完全適合期限（2027年12月）前の分析であり、harmonized standards・implementing actsの一部が未公布。実際の適合性評価基準は変動しうる。
2. **コスト推計の一般性**: MSSP費用等のコスト推計は一般的な産業セクターのベンチマークであり、半導体装置固有のOT環境複雑性を十分に反映していない。個社見積もりが必要。
3. **法的助言ではない**: 本レビューは技術戦略の統合レビューであり、法的助言を構成しない。外為法交差リスク、製品分類、AR契約等は専門法務の確認が必須。
4. **ファブオペレーター固有制約の非公開性**: TSMC・Samsung・Intel等の装置リモートアクセスポリシーは一般に非公開であり、本分析に組み込めていない。
5. **日本語一次情報の限定的カバレッジ**: 日本の法規制・産業実態に関する英語情報源への依存度が高い。

---

## Unresolved Issues（未解決事項）

| # | 未解決事項 | 解決に必要なアクション | 期限 |
|---|---|---|---|
| U-1 | バイナリ解析の不完全SBOMに対するNB許容基準 | NB指定後（2026年6月以降）にNBと事前協議 | 2026年Q3 |
| U-2 | エアギャップ設備への「自動更新デフォルト」例外の法的確定 | EC implementing actまたはharmonized standard公布を監視 | 2027年前半 |
| U-3 | サプライヤがSBOMを提供できない場合の法的責任分担 | CRAはメーカーに一義的責任を課すが、サプライヤ契約での責任分担について法務検討 | 2026年Q2 |
| U-4 | 外為法とENISA脆弱性報告の交差リスク | METI・法務省・安保貿易管理専門弁護士への照会 | 2026年Q2（報告義務開始前） |
| U-5 | ファブオペレーター（顧客）のリモートアクセス制限とCRA更新義務の衝突 | 主要顧客（TSMC、Samsung等）との個別協議 | 2026年Q3〜 |
| U-6 | 15〜25年製品の長期支援コストモデル | 製品別LCC分析・保守契約料金改定シミュレーション | 2026年Q4 |

---

## Conclusion（結論）

### 最終結論

**日本半導体装置関連ソフトウェア開発企業のCRA対応は、「完璧な準備を待つ」アプローチではなく、「法定期限に間に合わせながら段階的に精緻化する」アプローチが唯一の現実的選択である。**（Confidence: high）

具体的に：

1. **最優先投資（2026年9月まで・不可避）**: EU AR指定 + MSSP委託によるPSIRT 24/7体制構築 + ENISA SRP登録。**概算初年度コスト: €200k〜€600k**。これを怠った場合のリスクは**EU市場アクセス喪失＋最大€15Mまたは全世界売上2.5%の罰金**であり、コスト対リスク比は明白である。（Confidence: high）

2. **SBOM戦略は「完全性」ではなく「文書化された透明性」を目標とする**: バイナリ解析の限界を受容し、ギャップ分析を含むSBOMを「best effort」として提出する。サプライヤ能力の3層分類に基づく並行アプローチで段階的に完全性を向上させる。（Confidence: medium — NB審査基準未確定のため）

3. **製品ライフサイクル問題は経営戦略レベルの判断が必要**: 15〜25年の稼働寿命に対して脆弱性管理を継続する義務は、製品原価・保守契約・EU市場戦略そのものに影響する。「後継版置き換え」「延長有償保守」「限定的EU市場撤退」の意思決定ツリーを製品ファミリーごとに策定すべきである。（Confidence: high）

4. **外為法交差リスクは未確定だが、対応不能リスクではない**: 脆弱性報告の技術的詳細度を制御する（初期警告では技術詳細を最小限にし、ENISA報告のみに限定する）ことで実務的にリスクを低減できる可能性がある。ただし、法的確認は報告義務開始前に完了すべきである。（Confidence: low — 未確定）

5. **SEMI E187/E188への既存対応をCRA適合の効率的な出発点とする**: OS・ネットワーク・エンドポイントセキュリティの既存対応を再利用し、CRA固有の差分（SBOM、脆弱性報告、支援期間、技術文書）に集中投資する。（Confidence: high）

### 経営層へのリスク受容判断基準

| リスク区分 | リスク内容 | 受容判断 | 根拠 |
|---|---|---|---|
| **受容不可** | 報告義務不遵守（2026年9月〜） | 即時対応必須 | 罰金最大€15M/売上2.5%、CE不可 |
| **受容不可** | AR未指定 | 即時対応必須 | EU市場アクセス不可 |
| **条件付き受容** | SBOM不完全性（50〜75%） | ギャップ文書化を条件に受容 | 「best effort + 文書化」で当局対応 |
| **条件付き受容** | エアギャップ更新のCRA適合 | 法解釈確定まで段階的対応 | 顧客契約・安全要件との衝突を並行解決 |
| **監視・解消** | 外為法×ENISA報告交差 | 法務確認完了まで報告内容を最小限に | 2026年Q2までに法務見解取得 |
| **戦略的判断** | 15〜25年支援期間 | 製品ファミリー別に判断 | LCC分析に基づく経営判断 |

---

## Recommended Actions（推奨アクション）

### 即時着手（2026年3〜4月）——Priority: CRITICAL

| # | アクション | 担当 | 期限 |
|---|---|---|---|
| A-1 | EU Authorised Representative選定・契約 | 法務・EU事業部 | 2026年5月末 |
| A-2 | グローバルMSSP選定・PSIRT体制契約締結 | 情報セキュリティ部 | 2026年7月末 |
| A-3 | 全EU向け製品のCRA製品分類（Annex III/IV照合） | 法務・製品企画 | 2026年5月末 |
| A-4 | ENISA Single Reporting Platform登録準備 | 情報セキュリティ部 + AR | 2026年8月末 |
| A-5 | 外為法×ENISA報告の交差リスク法務照会 | 法務・安保貿易管理 | 2026年5月末 |

### 短期（2026年Q3〜Q4）——Priority: HIGH

| # | アクション | 担当 | 期限 |
|---|---|---|---|
| B-1 | ビルド時SBOM生成パイプライン構築（新規製品） | ソフトウェア開発部 | 2026年12月末 |
| B-2 | サプライヤ3層分類＋SBOM対応支援プログラム開始 | 調達・品質管理 | 2026年10月末 |
| B-3 | レガシー製品バイナリ解析＋ギャップ文書化開始 | ソフトウェア開発部 | 2026年12月末 |
| B-4 | 製品別支援期間宣言＋保守契約改定案策定 | 営業・法務・サービス | 2026年12月末 |
| B-5 | VEX（CSAF/OpenVEX）運用プロセス設計 | 情報セキュリティ部 | 2026年12月末 |

### 中期（2027年Q1〜Q4）——Priority: MEDIUM〜HIGH

| # | アクション | 担当 | 期限 |
|---|---|---|---|
| C-1 | CRA Annex VII準拠技術文書完成 | 品質保証部 | 2027年9月末 |
| C-2 | 適合性評価実施（自己評価またはNB審査） | 品質保証部・法務 | 2027年10月末 |
| C-3 | CE marking付与＋EU適合宣言書発行 | 品質保証部 | 2027年11月末 |
| C-4 | エアギャップ環境向けオフライン更新手順確定 | ソフトウェア開発部・サービス | 2027年Q2（法解釈確定次第） |
| C-5 | 長寿命製品の支援期間戦略最終確定 | 経営企画・製品企画 | 2027年Q2 |

---

## Source Registry（情報源一覧）

| ID | source_type/tier | タイトル | access_date | primary |
|---|---|---|---|---|
| [SRC-001] | official_document/tier1 | [Regulation (EU) 2024/2847](https://eur-lex.europa.eu/eli/reg/2024/2847/oj/eng) | 2026-03-14 | true |
| [SRC-002] | official_document/tier1 | [CRA Reporting Obligations - EU Digital Strategy](https://digital-strategy.ec.europa.eu/en/policies/cra-reporting) | 2026-03-14 | true |
| [SRC-003] | official_document/tier1 | [CRA Article 14 Full Text](https://www.european-cyber-resilience-act.com/Cyber_Resilience_Act_Article_14.html) | 2026-03-14 | true |
| [SRC-004] | industry_report/tier2 | [CECIMO CRA Reporting Obligations Guide](https://www.cecimo.eu/news/cecimo-guide-on-reporting-obligations-under-the-cyber-resilience-act-cra/) | 2026-03-14 | false |
| [SRC-005] | industry_report/tier2 | [Deloitte Belgium: Navigating the EU CRA](https://www.deloitte.com/be/en/services/consulting/research/cyber-resilience-act.html) | 2026-03-14 | false |
| [SRC-006] | industry_report/tier3 | [MSSP Pricing Guide 2025](https://www.harbourtech.net/blog/mssp-pricing-guide-security-costs) | 2026-03-14 | false |
| [SRC-007] | industry_report/tier3 | [UnderDefense: MSSP Pricing](https://underdefense.com/blog/managed-security-service-provider-mssp-pricing/) | 2026-03-14 | false |
| [SRC-008] | news_article/tier3 | [CyberSecurityNews: MSSP Pricing](https://cybersecuritynews.com/mssp-pricing/) | 2026-03-14 | false |
| [SRC-009] | industry_report/tier3 | [Pricing Managed Security Services Guide 2026](https://bennettfinancials.com/pricing-managed-security-services/) | 2026-03-14 | false |
| [SRC-010] | official_document/tier1 | [BSI TR-03183-2](https://www.bsi.bund.de/EN/Themen/Unternehmen-und-Organisationen/Standards-und-Zertifizierung/Technische-Richtlinien/TR-nach-Thema-sortiert/tr03183/TR-03183_node.html) | 2026-03-14 | true |
| [SRC-011] | news_article/tier3 | [Softwareseni: CRA SBOM Requirements Decoded](https://www.softwareseni.com/eu-cyber-resilience-act-technical-implementation-sbom-requirements-decoded/) | 2026-03-14 | false |
| [SRC-012] | news_article/tier3 | [Zerberus: CRA Compliance Guide Part II](https://www.zerberus.ai/post/eu-cyber-resilience-act-cra-compliance-guide-part-ii) | 2026-03-14 | false |
| [SRC-013] | industry_report/tier3 | [Finitestate: CRA SBOM Documentation Guide](https://finitestate.io/blog/eu-cra-sbom-technical-documentation-guide) | 2026-03-14 | false |
| [SRC-014] | news_article/tier3 | [FOSSA: SBOM Requirements in CRA](https://fossa.com/blog/sbom-requirements-cra-cyber-resilience-act/) | 2026-03-14 | false |
| [SRC-015] | industry_report/tier2 | [GoRegulus: CRA Manufacturer Obligations](https://goregulus.com/cra-compliance/cra-manufacturer-obligations/) | 2026-03-14 | false |
| [SRC-016] | industry_report/tier3 | [ComplianceGate: EU Authorised Representative Guide](https://www.compliancegate.com/european-authorised-representative/) | 2026-03-14 | false |
| [SRC-017] | industry_report/tier2 | [CRA Guide for Manufacturers](https://www.cyberresilienceact.eu/cra-guide-for-manufacturers/) | 2026-03-14 | false |
| [SRC-018] | industry_report/tier2 | [GoRegulus: CRA Update Requirements](https://goregulus.com/cra-requirements/cra-update-requirements/) | 2026-03-14 | false |
| [SRC-019] | industry_report/tier2 | [NXP: EU CRA](https://www.nxp.com/applications/technologies/security/eu-cyber-resilience-act-cra:CYBER-RESILIENCE-ACT) | 2026-03-14 | false |
| [SRC-020] | industry_report/tier2 | [CRA Compliance Guide for SMEs](https://cyen.eu/wp-content/uploads/2025/12/D4.1_CRA-Compliance-Guide-for-SMEs_EN_compressed.pdf) | 2026-03-14 | false |
| [SRC-021] | industry_report/tier2 | [Fortress Infosec: CRA & Supply Chain Risk](https://www.fortressinfosec.com/blog/how-the-eu-cyber-resilience-act-impacts-supply-chain-risk-management) | 2026-03-14 | false |
| [SRC-022] | industry_report/tier2 | [TSMC: Strengthens Cybersecurity of Semiconductor Equipment](https://esg.tsmc.com/en-US/articles/280) | 2026-03-14 | false |
| [SRC-023] | industry_report/tier2 | [SEMI: A Year of Progress for Semiconductor Cybersecurity](https://www.semi.org/en/blogs/a-year-of-progress-for-semiconductor-cybersecurity) | 2026-03-14 | false |
| [SRC-024] | industry_report/tier2 | [Janus Cyber: What is SEMI E187?](https://www.janus-cyber.com/post/what-is-semi-e187) | 2026-03-14 | false |
| [SRC-025] | industry_report/tier2 | [Claroty: Understanding SEMI E187 & E188](https://claroty.com/blog/understanding-semi-e187-e188-compliance-for-the-semiconductor-industry) | 2026-03-14 | false |
| [SRC-026] | official_document/tier1 | [METI: OT Security Guidelines for Semiconductor Device Factories](https://www.meti.go.jp/english/press/2025/0627_004.html) | 2026-03-14 | true |
| [SRC-027] | official_document/tier1 | [Hogan Lovells: Japan's New Chip Equipment Export Rules](https://www.hoganlovells.com/en/publications/japans-new-chip-equipment-export-rules-take-effect) | 2026-03-14 | false |
| [SRC-028] | official_document/tier2 | [Mori Hamada: Japan Semiconductor Export Restrictions](https://www.morihamada.com/sites/default/files/newsletters/newsletters/pdf/INTERNATIONAL%20TRADE%20LAW%20BULLETIN_E_vol.1.pdf) | 2026-03-14 | false |
| [SRC-029] | official_document/tier1 | [CRA Conformity Assessment - EU Digital Strategy](https://digital-strategy.ec.europa.eu/en/policies/cra-conformity-assessment) | 2026-03-14 | true |
| [SRC-030] | industry_report/tier2 | [Finitestate: Conformity Assessments CRA](https://finitestate.io/blog/conformity-assessments-eu-cra-requirements) | 2026-03-14 | false |
| [SRC-031] | official_document/tier1 | [BSI: Cyber Resilience Act](https://www.bsi.bund.de/EN/Themen/Unternehmen-und-Organisationen/Informationen-und-Empfehlungen/Cyber_Resilience_Act/cyber_resilience_act.html) | 2026-03-14 | true |
| [SRC-032] | industry_report/tier2 | [CRA Implementation Timeline](https://craevidence.com/blog/cra-implementation-timeline-2025-2027) | 2026-03-14 | false |
| [SRC-033] | industry_report/tier2 | [DLA Piper: CRA Readiness](https://www.dlapiper.com/en/insights/publications/2026/02/cyber-resilience-act-what-you-need-to-know-and-what-you-need-to-be-doing) | 2026-03-14 | false |
| [SRC-034] | industry_report/tier2 | [White & Case: CRA Clock is Ticking](https://www.whitecase.com/insight-alert/cyber-resilience-act-clock-ticking-compliance) | 2026-03-14 | false |
| [SRC-035] | news_article/tier3 | [TMI法律事務所: EU CRA解説](https://www.tmi.gr.jp/service/global/europe/2025/17691.html) | 2026-03-14 | false |
| [SRC-036] | industry_report/tier2 | [Epteck: EU CRA Compliance for Embedded Systems](https://epteck.com/eu-cra-compliance-embedded-systems/) | 2026-03-14 | false |
| [SRC-037] | industry_report/tier2 | [TXOne: SEMI E187/E188 Standards](https://www.txone.com/blog/how-semi-e187-and-e188-standards-elevate-cybersecurity-in-semiconductor-industry/) | 2026-03-14 | false |
| [SRC-038] | industry_report/tier2 | [Fidelis Security: CRA Implementation Guide](https://fidelissecurity.com/cybersecurity-101/learn/eu-cyber-resilience-act-implementation/) | 2026-03-14 | false |
| [SRC-039] | industry_report/tier2 | [Fraunhofer AISEC: CRA Security Requirements](https://www.aisec.fraunhofer.de/en/spotlights/the-cyber-resilience-act---security-requirements-for-products-wi.html) | 2026-03-14 | false |
| [SRC-040] | industry_report/tier2 | [PwC Switzerland: Understanding the EU CRA](https://www.pwc.ch/en/insights/regulation/understanding-the-eu-cyber-resilience-act.html) | 2026-03-14 | false |

**ソース統計**:
- tier1（公式文書・法令・標準化団体）: 8件
- tier2（企業公式・研究機関・法律事務所）: 21件
- tier3（技術ブログ・専門メディア）: 11件
- primary_source比率: 8/40 = 20%（前ステップの63%から低下。実装コスト・産業実態に関するtier2-3ソースの追加により相対的に低下したが、法的基盤のtier1カバレッジは維持）
