以下に統合レビューをまとめます。

---

# 統合レビュー

## Executive Summary

EU Cyber Resilience Act（CRA）は2024年12月10日に発効し、2027年12月11日にSBOM義務を含む全要件が完全適用される。EU市場に製品を輸出する日本の製造業は法的義務を回避できず、CEマーク取得の前提条件として準拠が必須化されている。商用製品に組み込まれたOSSについても最終製造者が全責任を負い、サプライチェーン全体のデューデリジェンスが求められる。一方、日本独自の対応策であるJC-STARとCRAの相互認証については概念的整合性は確認されているものの、具体的な制度的着地点は2025年3月時点で**未確定**である。日本の製造業は2027年の期限を見据え、今すぐ対応プロセスの構築に着手する必要がある。

## Research Question

EU Cyber Resilience Act（CRA）が日本の製造業ソフトウェアに与える具体的な義務と影響は何か。特にSBOM義務化、OSS責任の所在、製品アップデート義務の3点を中心に調査する。

---

## Findings（主要な発見）

### Finding 1: CRAの施行タイムライン

- **Claim**: CRAは2024年12月に発効済みであり、段階的に義務が強化され2027年末に完全適用となる。
- **Evidence**:
  - 発効日: 2024年12月10日
  - 脆弱性・インシデント報告義務: 2026年9月11日（発効21ヶ月後）
  - SBOM含む全要件の完全適用: 2027年12月11日（発効36ヶ月後）
- **Source**: [official_document] Regulation (EU) 2024/2847 (eur-lex.europa.eu, 2024-12); [official_document] European Commission - Horizontal Cybersecurity Requirements Summary (eur-lex.europa.eu, 2024-12)
- **Confidence**: high
- **Evidence Strength**: strong

### Finding 2: SBOM義務化の要件

- **Claim**: 2027年12月11日以降、対象製品すべてで機械可読SBOMの作成・維持・更新が法的義務となる。
- **Evidence**:
  - 少なくともトップレベルの依存関係を含むSBOM作成が必須
  - 対応フォーマット: CycloneDX、SPDX 1.5+など標準的な機械可読形式
  - 記載必須項目: コンポーネント名・バージョン、サプライヤー識別子、依存関係、暗号チェックサム、ライセンス情報
  - SBOMの公開義務はないが、EU当局からの求めに応じて提出義務あり
  - 製品・ソフトウェアの更新に伴い、SBOMも随時更新が必要
- **Source**: [official_document] Regulation (EU) 2024/2847 (eur-lex.europa.eu); [industry_report] EU CRA SBOM Compliance Guide (sbomgenerator.com/compliance/eu-cyber-resilience-act, 2025-03); [industry_report] SBOM and the EU CRA (safedep.io/sbom-and-eu-cra-cyber-resilience-act/, 2025-03)
- **Confidence**: high
- **Evidence Strength**: strong

### Finding 3: OSSの責任所在

- **Claim**: 商用製品に組み込まれたOSSについては、純粋なOSSプロジェクト自体は規制対象外だが、それを利用した最終製品の製造者が当該OSSコンポーネントのセキュリティについて全責任を負う。
- **Evidence**:
  - 非営利・開発者向けの純粋なOSSプロジェクトは規制対象外（CRA Recitals明記）
  - ただし商用製品の一部として販売・配布された場合、そのOSSコンポーネントのセキュリティ確保は製造者の義務
  - 「OSSを使ったから知らなかった」はCRA違反の免責事由にならない
  - サプライチェーン全体のデューデリジェンスが求められる
- **Source**: [official_document] CRA Final Text Recitals (eur-lex.europa.eu); [industry_report] BearingPoint - EU Cyber Resilience Act: Why SBOMs will be mandatory (bearingpoint.services, 2024)
- **Confidence**: high
- **Evidence Strength**: strong

### Finding 4: 製品アップデート・サポート期間の義務

- **Claim**: 製造者は製品の予想使用期間（または最低5年が実質的指標）にわたり、脆弱性対応のセキュリティアップデートを無償提供する義務を負い、サポート終了日（EOL）の明示も必須。
- **Evidence**:
  - 製品の「予想使用期間」中は脆弱性発見次第、迅速なパッチ提供が義務
  - 工業製品・IoT機器では最低5年が業界実務上の実質的基準とされることが多い（法律上の明文規定ではなくガイダンス上の目安）
  - EOL日時の消費者への明示義務あり
- **Source**: [official_document] European Commission - Cyber Resilience Act Q&A (digital-strategy.ec.europa.eu); [news_article] Taylor Wessing - Cyber Resilience Act Overview (taylorwessing.com, 2024)
- **Confidence**: high
- **Evidence Strength**: moderate（「5年」の数値はガイダンス的目安であり、法律上の明文規定は「予想使用期間」のため、製品カテゴリ別の具体的解釈は今後の委任規則等で確定見込み）

### Finding 5: 日本独自の対応策（JC-STARとCRAの関係）

- **Claim**: 2025年3月、MEPIがJC-STARを正式ローンチ。EU当局との対話でCRAとの概念的整合性は「高い」と評価されたが、相互認証の制度的着地点は未確定。
- **Evidence**:
  - 2025年3月: METI/IPA、IoT製品セキュリティラベリングスキーム「JC-STAR」を正式公開（任意参加、4段階ティア制）
  - 2025年〜2026年初頭: EU-日本当局間の技術的対話で「概念的整合性が高い」と確認
  - ただしCRAは法的強制義務、JC-STARは任意の認証スキームであり執行メカニズムが異なる
  - JC-STAR認証はEU当局からの審査において補強材料となる可能性があるが、CRA義務の代替にはならない
- **Source**: [official_document] METI Press Release - Launch of IoT Product Security Labeling Scheme (JC-STAR) (meti.go.jp/english/press/2025/0325_006.html, 2025-03); [news_article] INSTAR Standards - Facilitating EU & Japan dialogue on CRA and JC-STAR (instarstandards.org/news/facilitating-eu-japan-dialogue-cyber-resilience-act-and-jc-star, 2025-03)
- **Confidence**: medium（制度的相互認証の着地点は交渉継続中）
- **Evidence Strength**: moderate

### Finding 6: 不適合時の制裁リスク

- **Claim**: CRA違反時の制裁金は最大1,500万ユーロまたは全世界年間売上高の2.5%の高い方であり、違反はCEマーク取得不可→EU市場からの事実上の排除につながる。
- **Evidence**:
  - 制裁金上限: €15,000,000 または全世界売上高の2.5%（高い方が適用）
  - 重要製品（Annex I Class II）など高リスク製品は第三者認証機関（Notified Body）による適合性評価が義務
  - CEマーク未取得製品のEU市場流通は不可
- **Source**: [official_document] Regulation (EU) 2024/2847, Article 64 (eur-lex.europa.eu); [industry_report] FOSSA - SBOM Requirements in the EU's CRA (fossa.com/blog/sbom-requirements-cra-cyber-resilience-act/, 2025-03)
- **Confidence**: high
- **Evidence Strength**: strong

---

## Confidence Distribution

- **High confidence の発見**: 5件（Finding 1, 2, 3, 4の主要部分, 6）
- **Medium confidence の発見**: 1件（Finding 5）
- **Low confidence の発見**: 0件
- **全体の確信度評価**: 全体的に高品質な一次情報（EU公式文書・METI公式発表）に裏付けられており、信頼性は高い。唯一の不確実点はJC-STARとCRAの制度的相互認証の着地点のみ。

---

## Contradictory Claims（矛盾する主張）

前ステップ（information-gathering）との矛盾: **なし**。

補足: Finding 4の「5年間」については、一次情報（法律条文）では「製品の予想使用期間」という表現であり「最低5年」という数値は法律上の明文ではない。この点を「moderate」根拠強度として修正記録。ただし矛盾ではなく精度の差異として整理。

---

## Missing Data（不足データ）

1. **製品カテゴリ別の具体的サポート年数**: 委任規則（Delegated Act）による製品分類ごとの最低サポート期間の確定値（2025年3月時点で未発行）
2. **JC-STAR第三者認証のCRAへの適用可能性**: EU認定Notified BodyによるJC-STAR認証の相互利用可否に関する公式決定
3. **日本中小製造業向けの支援策**: METI/JETROによるSBOM実装支援プログラムや補助金スキームの詳細
4. **特定の産業機械・制御システム（ICS）への適用範囲**: 一部の産業用制御機器がMachinery Regulationなど他規制との重複適用を受ける場合の優先関係

---

## Limitations（限界）

1. **情報の鮮度**: CRAに関連する委任規則・実施規則は2025年時点で多くが起草・協議段階であり、最終確定した技術仕様・評価基準は一部まだ存在しない。
2. **JC-STAR情報の限定性**: JC-STARは2025年3月正式ローンチと非常に新しいスキームであり、独立した評価・事例が極めて少ない。
3. **日本語一次情報へのアクセス**: METI・IPAの詳細な技術指針（日本語文書）は本調査では一部しか参照できていない可能性がある。
4. **製品カテゴリの多様性**: 製造業の対象製品幅が広く（家電IoT〜産業用制御システム〜医療機器除外等）、本調査は一般論にとどまる。

---

## Unresolved Issues（未解決事項）

1. **JC-STAR ↔ CRA の正式相互認証協定**: 2026年初頭時点で「概念的整合性高い」とされるが、正式なMRAや同等性認定の締結時期・条件は未確定。（**未確定**）
2. **Annex I製品の分類基準の詳細化**: 重要製品（Class I / Class II）のカテゴリ境界線を確定する委任規則の発行スケジュール。（**未確定**）
3. **純粋OSS ↔ 商用OSSの境界解釈**: 「開発段階でのOSS利用」と「商用製品への組み込み」の判断基準について、実務上の解釈ガイダンスが不足している。

---

## Conclusion（結論）

EU Cyber Resilience Actは、EU市場に製品を輸出する日本の製造業にとって無視できない法的強制義務である。SBOM義務化（2027年12月）、OSS組み込み責任、長期セキュリティアップデート義務の3点はいずれも**high confidence**で確認されており、準拠は「努力目標」ではなく「EU市場参入の必要条件」である。

日本固有の対応策であるJC-STARはCRAとの概念的整合性が高く、将来的な制度連携に向けた有望な基盤となりうるが、2025年時点での相互認証的効力は**未確定**であり、CRA義務の代替手段とはなり得ない。

---

## Recommended Actions

優先度順:

| 優先度 | アクション | 期限 |
|--------|-----------|------|
| 🔴 高 | **コンポーネント棚卸し**: 自社製品に含まれる全ソフトウェア（OSS含む）の洗い出しとリスク分類 | 今すぐ〜2025年内 |
| 🔴 高 | **脆弱性報告プロセスの整備**: 2026年9月の脆弱性報告義務開始に向け、EUの報告窓口（ENISA）への報告フロー構築 | 2025年内 |
| 🟡 中 | **SBOM自動生成の開発プロセス組み込み**: CI/CDパイプラインへのSBOM生成ツール（例: Syft, CycloneDX CLI）統合 | 2026年前半 |
| 🟡 中 | **サプライヤー契約の改訂**: 二次・三次サプライヤーへのセキュリティ要件・SBOM提供義務の契約化 | 2026年前半 |
| 🟡 中 | **JC-STAR認証取得の検討**: 任意だが国際的な信頼性向上とCRA準拠の補強材料として有効 | 2026年中 |
| 🟢 低 | **METI/JETRO情報の継続的モニタリング**: 委任規則・JC-STAR-CRA相互認証動向のウォッチ | 継続 |

---

## Source Registry

| # | 種別 | タイトル | ロケーター | 取得日 |
|---|------|----------|-----------|--------|
| 1 | official_document | Regulation (EU) 2024/2847 (CRA Full Text) | eur-lex.europa.eu | 2024-12 |
| 2 | official_document | EC - Horizontal Cybersecurity Requirements Summary | eur-lex.europa.eu/EN/legal-content/summary/... | 2024-12 |
| 3 | official_document | European Commission - CRA Q&A | digital-strategy.ec.europa.eu | 2024-12 |
| 4 | official_document | METI Press Release - Launch of JC-STAR | meti.go.jp/english/press/2025/0325_006.html | 2025-03 |
| 5 | news_article | JETRO - サイバーレジリエンス法施行 | jetro.go.jp | 2024-12 |
| 6 | news_article | INSTAR Standards - EU & Japan dialogue on CRA and JC-STAR | instarstandards.org/news/facilitating-eu-japan-dialogue-cyber-resilience-act-and-jc-star | 2025-03 |
| 7 | news_article | Taylor Wessing - Cyber Resilience Act Overview | taylorwessing.com | 2024 |
| 8 | industry_report | PwC Japan - 欧州サイバーレジリエンス法 製造業が今取るべき対策 | pwc.com/jp | 2024 |
| 9 | industry_report | BearingPoint - EU CRA: Why SBOMs will be mandatory | bearingpoint.services | 2024 |
| 10 | industry_report | FOSSA - SBOM Requirements in the EU's CRA | fossa.com/blog/sbom-requirements-cra-cyber-resilience-act/ | 2025-03 |
| 11 | industry_report | SafeDep - SBOM and the EU CRA | safedep.io/sbom-and-eu-cra-cyber-resilience-act/ | 2025-03 |
| 12 | industry_report | sbomgenerator.com - EU CRA Compliance Guide for SBOM Planning | sbomgenerator.com/compliance/eu-cyber-resilience-act | 2025-03 |
| 13 | industry_report | Anchore - EU CRA SBOM Requirements | anchore.com/sbom/eu-cra/ | 2025-03 |

---

**要点**: 2027年12月の期限まで実質2年を切っており、SBOM整備・脆弱性報告体制の構築は今すぐ着手すべき喫緊の経営課題です。JC-STARとCRAの制度連携動向は**未確定**のため、JC-STAR取得で安心せずCRA要件を独立に充足する対応を強く推奨します。
