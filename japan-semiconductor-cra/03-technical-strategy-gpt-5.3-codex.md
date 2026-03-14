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
  - [official_document/tier1] [ENISA CRA Requirements Standards Mapping](https://www.enisa.europa.eu/publications/cyber-resilience-act-requirements-standards-mapping) (2026-03-14) — primary: true  
- **Confidence（確信度）**: high

- **Claim（主張）**: OT/ファブ環境では「安全な遠隔保守」と「制御されたオフライン更新」の両立アーキテクチャが現実解。  
- **Evidence（根拠）**: NIST SP 800-82r3はOT向けにセグメンテーション、遠隔アクセス制御、監査ログを推奨。SEMI E187関連資料も装置サイバー対策を要求。  
- **Source（情報源）**:  
  - [official_document/tier1] [NIST SP 800-82r3](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-82r3.pdf) (2026-03-14) — primary: true  
  - [official_document/tier1] [METI: 半導体セキュリティ規格「SEMI E187」について](https://www.meti.go.jp/shingikai/mono_info_service/sangyo_cyber/wg_seido/wg_kojo/pdf/004_04_00.pdf) (2026-03-14) — primary: true  
- **Confidence（確信度）**: high

## Strategy Principles（戦略設計原則）

- **Claim（主張）**: 「報告トラック」と「現場展開トラック」を分離する二層運用が必要。  
- **Evidence（根拠）**: CRA期限は法定で固定。現場展開は顧客・設備安全の検証工程で時間を要するため、同一SLAで管理すると破綻しやすい。  
- **Source（情報源）**:  
  - [official_document/tier1] [Regulation (EU) 2024/2847](https://eur-lex.europa.eu/eli/reg/2024/2847/oj/eng) (2026-03-14) — primary: true  
  - [unknown] 装置変更管理の実務工程（前段分析）(unknown) (unknown) — primary: false  
- **Confidence（確信度）**: medium

## Strategy 1: Legacy SBOM Factory（レガシーコードSBOM自動生成・管理）

### 1) 目標アーキテクチャ
`Source/Binary/3rd-party SBOM Ingest` → `Normalize (SPDX/CycloneDX)` → `Versioned SBOM Registry` → `Vulnerability Correlator` → `VEX Publisher`

- **Claim（主張）**: レガシー対策は「ソース起点SBOM」だけでは不十分で、バイナリ解析・サプライヤSBOM取り込みを含むハイブリッド方式が必要。  
- **Evidence（根拠）**: NTIA最低要件は依存関係・更新可能な機械可読SBOMを要求。古い組込み資産はソース欠落/不完全が発生しやすい。  
- **Source（情報源）**:  
  - [official_document/tier1] [NTIA: Minimum Elements for SBOM (PDF)](https://www.ntia.gov/files/ntia/publications/sbom_minimum_elements_report.pdf) (2026-03-14) — primary: true  
  - [industry_report/tier2] [SPDX Specifications](https://spdx.dev/use/specifications/) (2026-03-14) — primary: true  
  - [official_document/tier1] [CycloneDX Specification Overview](https://cyclonedx.org/specification/overview/) (2026-03-14) — primary: true  
- **Confidence（確信度）**: high

- **Claim（主張）**: SBOMは「生成」だけでなく「改ざん耐性のある管理（署名・版管理・監査証跡）」まで設計すべき。  
- **Evidence（根拠）**: CRA適合性評価では技術文書の再現可能性が重要。in-toto/TUFは供給網メタデータの検証・改ざん検知に有効。  
- **Source（情報源）**:  
  - [industry_report/tier2] [in-toto Specifications](https://in-toto.io/docs/specs/) (2026-03-14) — primary: true  
  - [industry_report/tier2] [The Update Framework Specification](https://theupdateframework.github.io/specification/latest/) (2026-03-14) — primary: true  
  - [official_document/tier1] [Regulation (EU) 2024/2847](https://eur-lex.europa.eu/eli/reg/2024/2847/oj/eng) (2026-03-14) — primary: true  
- **Confidence（確信度）**: medium

### 2) 実装ステップ（具体）
1. 装置ソフト資産台帳（実行ファイル、ライブラリ、FWイメージ、スクリプト）を型番/リビジョン単位で作成。  
2. 新規開発系はCIでSBOM自動生成、レガシー系はバイナリ構成解析でバックフィル。  
3. SPDX/CycloneDXに正規化し、製品版ごとに差分SBOM（delta）を保存。  
4. 出荷アーティファクト・ハッシュ・SBOM・VEXを相互参照可能に保管。  
5. 調達契約に「サプライヤSBOM提供」と「更新時再提出」を明記。  

- **Claim（主張）**: この方式で、CRA当局照会時の提出リードタイム短縮が期待できる。  
- **Evidence（根拠）**: 機械可読・版管理済みSBOMは再収集作業を削減する（効果量は推測）。  
- **Source（情報源）**:  
  - [official_document/tier1] [NTIA: Minimum Elements for SBOM (PDF)](https://www.ntia.gov/files/ntia/publications/sbom_minimum_elements_report.pdf) (2026-03-14) — primary: true  
  - [unknown] 提出リードタイム短縮効果の定量値 (unknown) (unknown) — primary: false  
- **Confidence（確信度）**: medium（効果量は未確定）

## Strategy 2: VEX-Centric Vulnerability Operations（VEX中心の脆弱性対応効率化）

- **Claim（主張）**: 外部開示はCSAF VEX、内部運用はCycloneDX VEX連携の二層が実務的。  
- **Evidence（根拠）**: CISAはVEX最小要件を公開。OASIS CSAF 2.0は機械可読アドバイザリ標準。CycloneDXはVEX表現をサポート。  
- **Source（情報源）**:  
  - [official_document/tier1] [CISA: Minimum Requirements for VEX](https://www.cisa.gov/resources-tools/resources/minimum-requirements-vulnerability-exploitability-exchange-vex) (2026-03-14) — primary: true  
  - [official_document/tier1] [OASIS CSAF 2.0](https://docs.oasis-open.org/csaf/csaf/v2.0/os/csaf-v2.0-os.html) (2026-03-14) — primary: true  
  - [official_document/tier1] [CycloneDX VEX Capability](https://cyclonedx.org/capabilities/vex/) (2026-03-14) — primary: true  
- **Confidence（確信度）**: high

- **Claim（主張）**: 優先度付けは`KEV + EPSS + 装置安全影響 + 顧客露出`の合成が有効。  
- **Evidence（根拠）**: KEVは「実際に悪用済み」を示す一次情報。EPSSは将来悪用確率の補助指標。OTでは安全停止・歩留まり影響を重み付けすべき。  
- **Source（情報源）**:  
  - [official_document/tier1] [CISA Known Exploited Vulnerabilities Catalog](https://www.cisa.gov/known-exploited-vulnerabilities-catalog) (2026-03-14) — primary: true  
  - [industry_report/tier2] [FIRST EPSS](https://www.first.org/epss/) (2026-03-14) — primary: true  
  - [unknown] OT安全影響の重み係数設計（企業個別）(unknown) (unknown) — primary: false  
- **Confidence（確信度）**: medium

### CRA期限に合わせたVEX運用SLA（提案）
- **T+24h**: `under_investigation`暫定VEX + ENISA早期警戒  
- **T+72h**: `affected / not_affected`更新、回避策提示  
- **Fix提供後14日以内**: `fixed`反映、最終報告整合

- **Claim（主張）**: 上記SLAはArticle 14の法定期限と整合し、報告品質の平準化に寄与。  
- **Evidence（根拠）**: CRA報告期限とCSAF VEX状態モデルを時系列で対応付け可能。  
- **Source（情報源）**:  
  - [official_document/tier1] [Regulation (EU) 2024/2847](https://eur-lex.europa.eu/eli/reg/2024/2847/oj/eng) (2026-03-14) — primary: true  
  - [official_document/tier1] [CRA reporting obligations (EU Digital Strategy)](https://digital-strategy.ec.europa.eu/en/policies/cra-reporting) (2026-03-14) — primary: true  
  - [official_document/tier1] [OASIS CSAF 2.0](https://docs.oasis-open.org/csaf/csaf/v2.0/os/csaf-v2.0-os.html) (2026-03-14) — primary: true  
- **Confidence（確信度）**: high

## Strategy 3: OT Secure Remote Maintenance & Update Architecture（OT適合のセキュア遠隔保守/更新）

### 参照アーキテクチャ（提案）
`Vendor Secure Build Zone`  
→ `Release Signing (HSM/鍵管理)`  
→ `Update Control Plane (metadata + policy)`  
→ `Online path: DMZ Update Gateway` **or** `Offline path: Media Transfer Station`  
→ `Plant Jump Host (MFA/JIT/session log)`  
→ `Equipment Update Agent (signature verify, anti-rollback, A/B fallback)`

- **Claim（主張）**: OT遠隔保守はDMZ分離、強認証、最小権限、監査ログを必須コントロールとして設計すべき。  
- **Evidence（根拠）**: NIST SP 800-82r3はOT向け遠隔アクセス制御・セグメンテーション・監視を重視。  
- **Source（情報源）**:  
  - [official_document/tier1] [NIST SP 800-82r3](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-82r3.pdf) (2026-03-14) — primary: true  
- **Confidence（確信度）**: high

- **Claim（主張）**: 更新機構は「署名付きマニフェスト」「整合性検証」「ロールバック耐性」を標準機能化すべき。  
- **Evidence（根拠）**: NIST SP 800-193はファームウェア更新の認証・耐改ざん・ロールバック対策を推奨。RFC 9019は安全なFW更新アーキテクチャを規定。  
- **Source（情報源）**:  
  - [official_document/tier1] [NIST SP 800-193](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-193.pdf) (2026-03-14) — primary: true  
  - [official_document/tier1] [RFC 9019](https://www.rfc-editor.org/info/rfc9019) (2026-03-14) — primary: true  
- **Confidence（確信度）**: high

- **Claim（主張）**: エアギャップ環境では、制御されたオフライン搬入（媒体サニタイズ・署名検証・チェーンオブカストディ）でCRAの「安全な更新」要件を実質充足できる可能性が高い。  
- **Evidence（根拠）**: OT標準実務（SEMI E187系）とNIST OTガイドは制御された更新運用を要求。  
- **Source（情報源）**:  
  - [official_document/tier1] [METI: SEMI E187解説](https://www.meti.go.jp/shingikai/mono_info_service/sangyo_cyber/wg_seido/wg_kojo/pdf/004_04_00.pdf) (2026-03-14) — primary: true  
  - [official_document/tier1] [NIST SP 800-82r3](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-82r3.pdf) (2026-03-14) — primary: true  
  - [official_document/tier1] [Regulation (EU) 2024/2847](https://eur-lex.europa.eu/eli/reg/2024/2847/oj/eng) (2026-03-14) — primary: true  
- **Confidence（確信度）**: medium（法解釈は一部未確定）

- **Claim（主張）**: 「自動更新デフォルト」のエアギャップ適用解釈は未確定で、法務・認証機関との事前整理が必要。  
- **Evidence（根拠）**: 条文は要件を示すが、産業用エアギャップ運用への具体適用は実装ガイダンス依存部分が残る。  
- **Source（情報源）**:  
  - [official_document/tier1] [Regulation (EU) 2024/2847](https://eur-lex.europa.eu/eli/reg/2024/2847/oj/eng) (2026-03-14) — primary: true  
  - [unknown] エアギャップ例外運用の最終法解釈 (unknown) (unknown) — primary: false  
- **Confidence（確信度）**: low（未確定）

## Implementation Roadmap（実行ロードマップ）

- **Claim（主張）**: 12–18か月で「SBOM基盤→VEX運用→OT更新基盤」の順に段階導入するのが現実的。  
- **Evidence（根拠）**: 法定報告SLA（先行）と現場適用安全性（後追い）を分離すると実装リスクを抑制できる。期間値は推測。  
- **Source（情報源）**:  
  - [official_document/tier1] [CRA reporting obligations (EU Digital Strategy)](https://digital-strategy.ec.europa.eu/en/policies/cra-reporting) (2026-03-14) — primary: true  
  - [unknown] 導入期間12–18か月の実行見積り (unknown) (unknown) — primary: false  
- **Confidence（確信度）**: medium（期間は推測）

## Conclusions & Priority Recommendations（結論・優先推奨）

- **Claim（主張）**: 最優先は「法定期限に間に合わせる運用層（PSIRT+VEX+報告）」の先行実装で、次に「現場安全を守る更新アーキテクチャ」を重ねる二段階戦略。  
- **Evidence（根拠）**: CRA Article 14は時限性が高く、OT更新は安全検証が必須で時間を要するため。  
- **Source（情報源）**:  
  - [official_document/tier1] [Regulation (EU) 2024/2847](https://eur-lex.europa.eu/eli/reg/2024/2847/oj/eng) (2026-03-14) — primary: true  
  - [official_document/tier1] [ENISA SRP](https://www.enisa.europa.eu/topics/product-security-and-certification/single-reporting-platform-srp) (2026-03-14) — primary: true  
  - [official_document/tier1] [NIST SP 800-82r3](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-82r3.pdf) (2026-03-14) — primary: true  
- **Confidence（確信度）**: high

**優先実行項目（技術）**  
1. SBOMファクトリー（ハイブリッド生成＋版管理）を全EU向け製品で標準化。  
2. CSAF VEX発行基盤をPSIRT運用に統合し、24h/72h/14日SLAを自動化。  
3. KEV/EPSS連携で「直ちに対応すべき脆弱性」を機械判定。  
4. OT更新は署名検証・反ロールバック・A/B復旧を必須化。  
5. エアギャップ向けにオフライン搬入手順（媒体サニタイズ/監査証跡）を標準手順書化。  
6. 未確定論点（自動更新デフォルトの適用解釈）は法務・NBとの事前合意パッケージ化。
