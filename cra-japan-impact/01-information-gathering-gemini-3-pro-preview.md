# EU Cyber Resilience Act (CRA) の日本の製造業への影響

EU Cyber Resilience Act (CRA) は、デジタル要素を含む製品（ハードウェア・ソフトウェア）に対し、設計から廃棄までのライフサイクル全体でサイバーセキュリティ要件を課すEU規則です。EU市場に製品を輸出する日本の製造業に直接的な法的義務が生じます。

## 1. タイムラインと施行状況
- **Claim**: 2024年12月に発効済み。完全適用は2027年後半から。
- **Evidence**:
  - 発効日: 2024年12月10日
  - 脆弱性報告義務開始: 2026年9月11日（発効から21ヶ月後）
  - 全要件適用（SBOM含む）: 2027年12月11日（発効から36ヶ月後）
- **Source**: [official_document] European Commission - Cyber Resilience Act (digital-strategy.ec.europa.eu, 2024-12); [news_article] JETRO - 新たなセキュリティー規則、サイバーレジリエンス法施行 (jetro.go.jp, 2024-12)
- **Confidence**: high

## 2. SBOM（ソフトウェア部品表）の義務化
- **Claim**: すべての対象製品で、機械可読なSBOMの作成と維持が必須となる。
- **Evidence**:
  - デジタル要素を含む製品は、少なくともトップレベルの依存関係を含むSBOMを作成する必要がある。
  - フォーマットはCycloneDXまたはSPDXなどの機械可読形式が求められる。
  - 製品のバージョンアップごとにSBOMも更新が必要。
- **Source**: [official_document] Regulation (EU) 2024/2847 (eur-lex.europa.eu); [industry_report] PwC Japan - 欧州サイバーレジリエンス法 製造業が今取るべき対策 (pwc.com/jp, 2024)
- **Confidence**: high

## 3. OSS（オープンソースソフトウェア）の責任範囲
- **Claim**: 商用製品に組み込まれたOSSについては、最終製品の製造者が全責任を負う。
- **Evidence**:
  - 純粋なOSSプロジェクト（非営利・開発者向け）は規制対象外だが、商用製品の一部として販売される場合、そのOSSコンポーネントのセキュリティ確保は製造者の義務となる。
  - 「OSSだから知らなかった」は通用せず、サプライチェーン全体のデューデリジェンスが求められる。
- **Source**: [official_document] CRA Final Text Recitals; [industry_report] BearingPoint - EU Cyber Resilience Act: Why SBOMs will be mandatory (bearingpoint.services, 2024)
- **Confidence**: high

## 4. 製品アップデートとサポート期間の義務
- **Claim**: 「期待される製品寿命」に基づいた期間中、セキュリティアップデートを無償で提供し続ける必要がある。
- **Evidence**:
  - 製造者は「サポート期間」を定義し、その期間中は脆弱性が発見され次第、迅速に対処・修正パッチを提供する義務がある。
  - 多くの工業製品・IoT機器では、最低5年間のサポートが実質的なベンチマークとされることが多いが、法律上は「製品の予想使用期間」が基準。
  - サポート終了日（EOL）を消費者に明示する必要がある。
- **Source**: [official_document] European Commission - Cyber Resilience Act Q&A; [news_article] Taylor Wessing - Cyber Resilience Act Overview (taylorwessing.com, 2024)
- **Confidence**: high

## 5. 日本企業への特有の影響と対応
- **Claim**: 日本の製造業は「CEマーク」取得の要件としてCRA準拠が必須化され、日欧間の基準調和が進められている。
- **Evidence**:
  - EUに輸出する全デジタル製品にCEマーク必須。CRA不適合ならCEマーク貼付不可＝輸出不可。
  - 違反時の制裁金は最大1500万ユーロまたは全世界売上高の2.5%の高い方。
  - 日本政府（METI/Digital Agency）とEUは、日本のIoTセキュリティ適合性評価制度「JC-STAR」とCRAの相互運用性について協議中。
- **Source**: [news_article] JETRO (jetro.go.jp); [government_press_release] METI - Japan-EU Digital Partnership Council (meti.go.jp, 2025)
- **Confidence**: medium (基準調和の具体着地点は協議中のため)

## 結論・推奨事項
日本の製造業にとって、CRAは単なる「努力目標」ではなく「市場参入条件」です。
1. **現状把握**: 自社製品に含まれるすべてのソフトウェアコンポーネント（OSS含む）の洗い出し。
2. **プロセス構築**: 2027年の期限に向け、開発プロセス（CI/CD）にSBOM自動生成と脆弱性監視を組み込む。
3. **契約見直し**: サプライヤーとの契約にセキュリティ要件と情報提供義務を追加する。
4. **情報収集**: METI/JETROからの最新のガイドライン（特にJC-STARとの整合性）を注視する。
