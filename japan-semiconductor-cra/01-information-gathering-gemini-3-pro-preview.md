# Research Report: EU Cyber Resilience Act Impact on Japan Semiconductor Sector

## ## Executive Summary（概要）
EUサイバーレジリエンス法（CRA）は、デジタル要素を持つすべての製品にセキュリティ要件を義務付ける規制であり、日本の半導体製造装置メーカーにも甚大な影響を及ぼします。特に「重要製品」への該当可能性、SEMI E187標準とのギャップ（特に報告義務とライフサイクル管理）、およびレガシーOSを抱える長寿命製品のサポートが主要な課題として特定されました。

## ## Scope & Critical Products（適用範囲と重要製品定義）

- **Claim（主張）**: 半導体製造装置はCRAの適用対象であり、その制御システムは「重要製品（Important/Critical Products）」に分類される可能性が高い。
- **Evidence（根拠）**:
    - CRAは「デジタル要素を持つ製品（products with digital elements）」すべてに適用され、半導体製造装置も例外ではありません。
    - "Commission Implementing Regulation (EU) 2025/2392" および Annex I/II（旧Annex III/IV）において、産業用オートメーションおよび制御システム（IACS）、PLC、マイクロコントローラなどが「重要製品」としてリストされています。半導体製造装置の制御ユニットやネットワーク機能を持つコンポーネントはこれらに該当し、第三者認証を含む厳格な適合性評価が求められる可能性があります。
- **Source（情報源）**:
    - [official_document/tier1] [Regulation (EU) 2024/2847 (Cyber Resilience Act)](https://eur-lex.europa.eu/eli/reg/2024/2847/oj/eng) (2025-03-14) — primary: true
    - [official_document/tier1] [Commission Implementing Regulation (EU) 2025/2392](https://eur-lex.europa.eu/legal-content/EN/TXT/PDF/?uri=OJ:L_202502392) (2025-03-14) — primary: true
- **Confidence（確信度）**: high

## ## SEMI E187 Alignment & Gaps（SEMI E187との整合性とギャップ）

- **Claim（主張）**: SEMI E187への準拠だけではEU CRAの法的要件を完全には満たせない。特に「インシデント報告」「SBOM」「ライフサイクル管理」に明確なギャップが存在する。
- **Evidence（根拠）**:
    - **整合領域**: OSセキュリティ、ネットワークセキュリティ、エンドポイント保護などの技術的制御はSEMI E187でカバーされています。
    - **ギャップ**:
        1.  **報告義務**: CRAは脆弱性やインシデント発見から「24時間以内」にENISA（欧州ネットワーク・情報セキュリティ機関）へ報告することを義務付けていますが、SEMI E187にはこの規定はありません。
        2.  **SBOM**: CRAはSBOM（ソフトウェア部品表）の作成と管理を要求しますが、SEMI E187では詳細な仕様まで踏み込んでいません。
        3.  **ライフサイクル**: CRAは製品の予想使用期間（通常5年以上）にわたるセキュリティ更新を義務付けますが、SEMI E187は出荷時の状態に重点を置いています。
    - **推奨**: ENISAやJRCが発行するマッピングレポート（ETSI TR 103 990等）を用いた詳細なギャップ分析が必要です。
- **Source（情報源）**:
    - [industry_report/tier2] [Cyber Resilience Act Requirements Standards Mapping (ENISA/JRC)](https://www.enisa.europa.eu/publications/cyber-resilience-act-requirements-standards-mapping) (2025-03-14) — primary: false
    - [industry_report/tier3] [CRA Gap Analysis Checklist](https://www.securebydesignhandbook.com/docs/resources/checklists-and-worksheets/cra-gap-analysis) (2025-03-14) — primary: false
- **Confidence（確信度）**: high

## ## Specific Challenges for Japanese Firms（日本企業特有の課題）

- **Claim（主張）**: レガシーOSへの依存と長い製品ライフサイクルが、CRA準拠の最大の障壁となっている。
- **Evidence（根拠）**:
    - **レガシーOS**: 半導体製造装置は10〜20年稼働することが一般的ですが、Windows XP/7などのサポート終了OSが依然として利用されています。CRAは「利用期間中のセキュリティ更新」を求めるため、パッチが存在しないレガシーOSの使用はコンプライアンス違反となるリスクがあります。
    - **現場運用**: 日本の工場ではネットワーク分離（エアギャップ）を前提にアップデートを行わない運用が主流ですが、CRAは脆弱性修正の提供を義務付けるため、運用モデルの根本的な見直しが必要です。
    - **サプライチェーン**: 装置メーカーだけでなく、組み込みボードや部品のサプライヤーまでCRA対応が波及しており、サプライチェーン全体での管理コストが増大しています。
- **Source（情報源）**:
    - [official_document/tier1] [経済産業省: ソフトウェアの開発・供給・運用におけるサイバーセキュリティ確保とレジリエンス向上のためのガイドライン（案）](https://www.meti.go.jp/shingikai/mono_info_service/sangyo_cyber/wg_seido/wg_bunyaodan/software/cyber_infrastructure/pdf/202503_g2.pdf) (2025-03-14) — primary: true
    - [industry_report/tier3] [欧州サイバーレジリエンス法（CRA）が日本企業に突きつける現実](https://cybersecurity-info.com/news/2026-01-09/) (2025-03-14) — primary: false
- **Confidence（確信度）**: medium

## ## Conclusion & Recommendations（結論と推奨事項）

日本半導体装置メーカーは、SEMI E187準拠を基礎としつつ、CRA固有の「報告プロセス」「SBOM管理」「長期間サポート体制」を追加実装する必要があります。特にレガシー資産については、仮想化やコンテナ化による延命、あるいはハードウェア刷新を含む抜本的な対策計画が急務です。経済産業省のガイドラインやENISAのマッピング資料を参照し、PSIRT（製品セキュリティインシデント対応チーム）を中心とした全社的な対応体制を構築することを推奨します。
