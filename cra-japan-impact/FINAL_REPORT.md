書き込み権限がないため、レポートを直接出力します。

---

# Research Report: EU Cyber Resilience Act（CRA）が日本の製造業に与える影響

## Executive Summary

EU Cyber Resilience Act（CRA）は2024年12月10日に発効し、2027年12月11日にSBOM義務を含む全要件が完全適用される。EU市場に製品を輸出する日本の製造業はCEマーク取得の前提条件として準拠が必須化されており、法的義務を回避することはできない。商用製品に組み込まれたOSSについても最終製造者が全責任を負い、サプライチェーン全体のデューデリジェンスが求められる。一方、日本独自の対応策であるJC-STARとCRAの相互認証については概念的整合性は確認されているものの、具体的な制度的着地点は2025年3月時点で**未確定**である。日本の製造業は2027年の期限を見据え、今すぐ対応プロセスの構築に着手する必要がある。

---

## Research Question

EU Cyber Resilience Act（CRA）が日本の製造業ソフトウェアに与える具体的な義務と影響は何か。特にSBOM義務化、OSS責任の所在、製品アップデート義務の3点を中心に調査する。

---

## Key Findings

- **CRA施行タイムライン**: 2024年12月10日発効済み。脆弱性報告義務は2026年9月11日から、SBOM含む全要件は2027年12月11日から完全適用。（Confidence: **high**）
- **SBOM義務化**: 2027年12月11日以降、対象製品すべてで機械可読SBOMの作成・維持・更新が法的義務となる。CycloneDX・SPDXなど標準フォーマットが必要。（Confidence: **high**）
- **OSSの責任所在**: 純粋なOSSプロジェクト自体は規制対象外だが、商用製品に組み込んだ場合、最終製品の製造者がOSSコンポーネントのセキュリティについて全責任を負う。（Confidence: **high**）
- **製品アップデート義務**: 製品の予想使用期間中（工業製品・IoTでは最低5年が実質的目安）、脆弱性対応のセキュリティアップデートを無償提供する義務があり、サポート終了日（EOL）の明示も必須。（Confidence: **high**）
- **制裁リスク**: 違反時の制裁金は最大1,500万ユーロまたは全世界年間売上高の2.5%の高い方。CEマーク未取得製品はEU市場に流通不可。（Confidence: **high**）
- **JC-STARとCRAの関係**: 2025年3月にMETIがJC-STARを正式ローンチ。CRAとの概念的整合性は「高い」と評価されたが、制度的相互認証の着地点は未確定。（Confidence: **medium**）

---

## Evidence Overview

| # | Claim | Evidence | Source | Confidence |
|---|-------|----------|--------|------------|
| 1 | CRAは2024年12月に発効済みであり、段階的に義務が強化され2027年末に完全適用となる。 | 発効日: 2024年12月10日。脆弱性・インシデント報告義務: 2026年9月11日（発効21ヶ月後）。SBOM含む全要件の完全適用: 2027年12月11日（発効36ヶ月後）。 | Regulation (EU) 2024/2847; EC - Horizontal Cybersecurity Requirements Summary (eur-lex.europa.eu, 2024-12) | high |
| 2 | 2027年12月11日以降、対象製品すべてで機械可読SBOMの作成・維持・更新が法的義務となる。 | 少なくともトップレベルの依存関係を含むSBOM作成が必須。対応フォーマット: CycloneDX、SPDX 1.5+。記載必須項目: コンポーネント名・バージョン、サプライヤー識別子、依存関係、暗号チェックサム、ライセンス情報。SBOMの公開義務はないが、EU当局からの求めに応じて提出義務あり。製品・ソフトウェアの更新に伴いSBOMも随時更新が必要。 | Regulation (EU) 2024/2847 (eur-lex.europa.eu); EU CRA SBOM Compliance Guide (sbomgenerator.com, 2025-03); SBOM and the EU CRA (safedep.io, 2025-03) | high |
| 3 | 商用製品に組み込まれたOSSについては、純粋なOSSプロジェクト自体は規制対象外だが、最終製品の製造者が当該OSSコンポーネントのセキュリティについて全責任を負う。 | 非営利・開発者向けの純粋なOSSプロジェクトは規制対象外（CRA Recitals明記）。商用製品の一部として販売・配布された場合、製造者の義務。「OSSを使ったから知らなかった」はCRA違反の免責事由にならない。サプライチェーン全体のデューデリジェンスが求められる。 | CRA Final Text Recitals (eur-lex.europa.eu); BearingPoint - EU CRA: Why SBOMs will be mandatory (bearingpoint.services, 2024) | high |
| 4 | 製造者は製品の予想使用期間（最低5年が実質的指標）にわたり、脆弱性対応のセキュリティアップデートを無償提供する義務を負い、EOLの明示も必須。 | 製品の「予想使用期間」中は脆弱性発見次第、迅速なパッチ提供が義務。工業製品・IoT機器では最低5年が業界実務上の実質的基準（法律上の明文規定ではなくガイダンス上の目安）。EOL日時の消費者への明示義務あり。 | European Commission - CRA Q&A (digital-strategy.ec.europa.eu); Taylor Wessing - Cyber Resilience Act Overview (taylorwessing.com, 2024) | high |
| 5 | 2025年3月、METIがJC-STARを正式ローンチ。CRAとの概念的整合性は「高い」と評価されたが、相互認証の制度的着地点は未確定。 | 2025年3月: METI/IPA、JC-STARを正式公開（任意参加、4段階ティア制）。EU-日本当局間の技術的対話で「概念的整合性が高い」と確認。ただしCRAは法的強制義務、JC-STARは任意の認証スキームであり執行メカニズムが異なる。JC-STAR認証はCRA義務の代替にはならない。 | METI Press Release - Launch of JC-STAR (meti.go.jp/english/press/2025/0325_006.html, 2025-03); INSTAR Standards - EU & Japan dialogue on CRA and JC-STAR (instarstandards.org, 2025-03) | medium |
| 6 | CRA違反時の制裁金は最大1,500万ユーロまたは全世界年間売上高の2.5%の高い方であり、違反はCEマーク取得不可によりEU市場からの事実上の排除につながる。 | 制裁金上限: €15,000,000 または全世界売上高の2.5%（高い方が適用）。重要製品（Annex I Class II）など高リスク製品は第三者認証機関（Notified Body）による適合性評価が義務。CEマーク未取得製品のEU市場流通は不可。 | Regulation (EU) 2024/2847, Article 64 (eur-lex.europa.eu); FOSSA - SBOM Requirements in the EU's CRA (fossa.com, 2025-03) | high |

---

## Detailed Findings

### Finding 1: CRAの施行タイムライン

CRAは2024年12月10日に発効済みであり、義務は段階的に強化される。

| マイルストーン | 日付 |
|--------------|------|
| 発効日 | 2024年12月10日 |
| 脆弱性・インシデント報告義務開始 | 2026年9月11日（発効21ヶ月後） |
| SBOM含む全要件の完全適用 | 2027年12月11日（発効36ヶ月後） |

---

### Finding 2: SBOM義務化の要件

2027年12月11日以降、対象製品すべてで機械可読SBOMの作成・維持・更新が法的義務となる。

- **必須フォーマット**: CycloneDX、SPDX 1.5+など標準的な機械可読形式
- **記載必須項目**: コンポーネント名・バージョン、サプライヤー識別子、依存関係、暗号チェックサム、ライセンス情報
- **公開義務**: なし（ただしEU当局からの求めに応じて提出義務あり）
- **更新義務**: 製品・ソフトウェアの更新に伴い、SBOMも随時更新が必要

---

### Finding 3: OSSの責任所在

- 非営利・開発者向けの純粋なOSSプロジェクトは規制対象外（CRA Recitals明記）
- 商用製品の一部として販売・配布された場合、そのOSSコンポーネントのセキュリティ確保は製造者の義務
- 「OSSを使ったから知らなかった」はCRA違反の免責事由にならない
- サプライチェーン全体のデューデリジェンスが求められる

---

### Finding 4: 製品アップデート・サポート期間の義務

- 製品の「予想使用期間」中は脆弱性発見次第、迅速なパッチ提供が義務
- 工業製品・IoT機器では最低5年が業界実務上の実質的基準とされることが多い（法律上の明文規定ではなくガイダンス上の目安。製品カテゴリ別の具体的解釈は今後の委任規則等で確定見込み）
- EOL日時の消費者への明示義務あり

---

### Finding 5: 日本独自の対応策（JC-STARとCRAの関係）

- 2025年3月: METI/IPA、IoT製品セキュリティラベリングスキーム「JC-STAR」を正式公開（任意参加、4段階ティア制）
- EU-日本当局間の技術的対話で「概念的整合性が高い」と確認
- ただしCRAは法的強制義務、JC-STARは任意の認証スキームであり執行メカニズムが異なる
- JC-STAR認証はEU当局からの審査において補強材料となる可能性があるが、**CRA義務の代替にはならない**
- 制度的相互認証の着地点は交渉継続中（**未確定**）

---

### Finding 6: 不適合時の制裁リスク

- **制裁金上限**: €15,000,000 または全世界売上高の2.5%（高い方が適用）
- 重要製品（Annex I Class II）など高リスク製品は第三者認証機関（Notified Body）による適合性評価が義務
- CEマーク未取得製品のEU市場流通は不可

---

## Confidence Assessment

| Level | 件数 | 対象 Finding |
|-------|------|-------------|
| **High** | 9件（Claim総数11件中） | Finding 1, 2, 3, 4（主要部分）, 6 |
| **Medium** | 2件 | Finding 5（JC-STAR相互認証の着地点）、Finding 1のJETRO情報 |
| **Low** | 0件 | — |

全体的に高品質な一次情報（EU公式文書・METI公式発表）に裏付けられており、信頼性は高い。唯一の不確実点はJC-STARとCRAの制度的相互認証の着地点のみ。

### Confidence 判定基準

| Level | 基準 |
|-------|------|
| **High** | 一次情報または複数独立ソースで裏付けあり |
| **Medium** | 信頼できるソースあり、ただし補強不足 |
| **Low** | 推測を含む、または情報不足 |

---

## Limitations

1. **情報の鮮度**: CRAに関連する委任規則・実施規則は2025年時点で多くが起草・協議段階であり、最終確定した技術仕様・評価基準は一部まだ存在しない。
2. **JC-STAR情報の限定性**: JC-STARは2025年3月正式ローンチと非常に新しいスキームであり、独立した評価・事例が極めて少ない。
3. **日本語一次情報へのアクセス**: METI・IPAの詳細な技術指針（日本語文書）は本調査では一部しか参照できていない可能性がある。
4. **製品カテゴリの多様性**: 製造業の対象製品幅が広く（家電IoT〜産業用制御システム〜医療機器除外等）、本調査は一般論にとどまる。

---

## Unresolved Issues

1. **JC-STAR ↔ CRA の正式相互認証協定**: 2026年初頭時点で「概念的整合性高い」とされるが、正式なMRAや同等性認定の締結時期・条件は未確定。（**未確定**）
2. **Annex I製品の分類基準の詳細化**: 重要製品（Class I / Class II）のカテゴリ境界線を確定する委任規則の発行スケジュール。（**未確定**）
3. **純粋OSS ↔ 商用OSSの境界解釈**: 「開発段階でのOSS利用」と「商用製品への組み込み」の判断基準について、実務上の解釈ガイダンスが不足している。

### Information Gaps（情報不足）

1. **製品カテゴリ別の具体的サポート年数**: 委任規則（Delegated Act）による製品分類ごとの最低サポート期間の確定値（2025年3月時点で未発行）
2. **JC-STAR第三者認証のCRAへの適用可能性**: EU認定Notified BodyによるJC-STAR認証の相互利用可否に関する公式決定
3. **日本中小製造業向けの支援策**: METI/JETROによるSBOM実装支援プログラムや補助金スキームの詳細
4. **特定の産業機械・制御システム（ICS）への適用範囲**: 一部の産業用制御機器がMachinery Regulationなど他規制との重複適用を受ける場合の優先関係

---

## Conclusion

EU Cyber Resilience Actは、EU市場に製品を輸出する日本の製造業にとって無視できない法的強制義務である。SBOM義務化（2027年12月）、OSS組み込み責任、長期セキュリティアップデート義務の3点はいずれも**high confidence**で確認されており、準拠は「努力目標」ではなく「EU市場参入の必要条件」である。

日本固有の対応策であるJC-STARはCRAとの概念的整合性が高く、将来的な制度連携に向けた有望な基盤となりうるが、2025年時点での相互認証的効力は**未確定**であり、CRA義務の代替手段とはなり得ない。2027年12月の期限まで実質2年を切っており、対応プロセスの構築は喫緊の経営課題である。

---

## Source Registry

| # | 種別 | タイトル | ロケーター | 取得日 |
|---|------|----------|-----------|--------|
| 1 | official_document | Regulation (EU) 2024/2847 (CRA Full Text) | eur-lex.europa.eu | 2024-12 |
| 2 | official_document | EC - Horizontal Cybersecurity Requirements Summary | eur-lex.europa.eu | 2024-12 |
| 3 | official_document | European Commission - CRA Q&A | digital-strategy.ec.europa.eu | 2024-12 |
| 4 | official_document | CRA Final Text Recitals | eur-lex.europa.eu | unknown |
| 5 | official_document | METI Press Release - Launch of JC-STAR | meti.go.jp/english/press/2025/0325_006.html | 2025-03 |
| 6 | news_article | JETRO - サイバーレジリエンス法施行 | jetro.go.jp | 2024-12 |
| 7 | news_article | INSTAR Standards - EU & Japan dialogue on CRA and JC-STAR | instarstandards.org/news/facilitating-eu-japan-dialogue-cyber-resilience-act-and-jc-star | 2025-03 |
| 8 | news_article | Taylor Wessing - Cyber Resilience Act Overview | taylorwessing.com | 2024 |
| 9 | industry_report | PwC Japan - 欧州サイバーレジリエンス法 製造業が今取るべき対策 | pwc.com/jp | 2024 |
| 10 | industry_report | BearingPoint - EU CRA: Why SBOMs will be mandatory | bearingpoint.services | 2024 |
| 11 | industry_report | FOSSA - SBOM Requirements in the EU's CRA | fossa.com/blog/sbom-requirements-cra-cyber-resilience-act/ | 2025-03 |
| 12 | industry_report | SafeDep - SBOM and the EU CRA | safedep.io/sbom-and-eu-cra-cyber-resilience-act/ | 2025-03 |
| 13 | industry_report | sbomgenerator.com - EU CRA Compliance Guide for SBOM Planning | sbomgenerator.com/compliance/eu-cyber-resilience-act | 2025-03 |
| 14 | industry_report | Anchore - EU CRA SBOM Requirements | anchore.com/sbom/eu-cra/ | 2025-03 |

---

## Validation Summary

| 指標 | 値 |
|------|-----|
| **バリデーション結果** | ✅ PASS |
| **実施日時** | 2026-03-13T15:17:31Z |
| **総 Claim 数** | 11 |
| **Evidence なし** | 0 |
| **Source なし** | 0 |
| **Confidence なし** | 0 |

### Confidence 分布

| Level | 件数 |
|-------|------|
| High | 9 |
| Medium | 2 |
| Low | 0 |

> ⚠️ **注意**: AI生成のため、重要な意思決定には人間による検証を推奨。Evidence JSON（`.evidence.json`）で機械的な監査が可能。

---

*Generated by AI Research Agent v2.1.0 — Evidence-based Multi-Model Pipeline*
