# 統合レビュー

## Executive Summary（総括）

MCP（Model Context Protocol）は LLM とツールを繋ぐ事実上の標準プロトコルとして急速に普及しているが、そのアーキテクチャ設計上の構造的欠陥と実装上のリスクが複合的に顕在化している。特に「信頼できないコンテンツ・機密データ・外部通信手段」の三要素が揃う「Lethal Trifecta」状態は、プロンプトインジェクション経由のデータ流出を現実的な脅威とする。OWASP が 2025 年に MCP Top 10 を公開したことで脆弱性分類は体系化されたが、プロトコル仕様レベルの認証・アテステーション欠如は未解決のままである。CVE（CVE-2025-6514、CVE-2025-49596 等）として実被害が記録されており、理論的リスクから実害フェーズへ移行している。組織は最小権限・Human-in-the-Loop・サンドボックス化を軸とした多層防御を即時に実施すべき段階にある。

## Research Question（調査の問い）

MCP の普及に伴い顕在化しているセキュリティリスク（プロンプトインジェクション、ツール権限の過剰付与、データ漏洩、サプライチェーン攻撃、認証・認可の課題）は何か。それらのリスクの確信度・根拠の強さはどの程度か。また有効な緩和策は何か。

---

## Findings（主要な発見）

### Finding 1: プロンプトインジェクション・ツール汚染（Tool Poisoning）は実証済みの高リスク攻撃ベクター

- **Claim**: 悪意あるツール定義や外部データ（メール・ファイル・カレンダー）を介したプロンプトインジェクションにより、エージェントはユーザーの同意なしに任意のツールを実行させられる。OWASP MCP Top 10 では Intent Flow Subversion（#6）・Tool Poisoning（#3）として分類されている。
- **Evidence**:
  - ツール `description` フィールドに悪意ある命令を埋め込む Tool Poisoning は、LLM が自然言語処理の性質上ツール説明を信頼することを悪用する。
  - Indirect Prompt Injection：外部から取得したコンテンツ（ファイル読み込み結果など）に隠された命令が、後続のツール呼び出しを乗っ取る。
  - Microsoft Developer Blog（2025）が Indirect Prompt Injection への具体的な防御手法を公式に公開していることは、本攻撃の実用性を裏付ける。
  - OWASP Top 10 への収録は、業界横断的な合意形成がなされたことを意味する。
- **Source**: [official_document] OWASP MCP Top 10 (https://owasp.org/www-project-mcp-top-10/, 2026-03-14) / [industry_report] Protecting against indirect prompt injection attacks in MCP, Microsoft Developer Blog (https://techcommunity.microsoft.com/blog/microsoft-security-blog/understanding-and-mitigating-security-risks-in-mcp-implementations/4404667, 2025)
- **Confidence**: high
- **Evidence Strength**: strong

---

### Finding 2: Lethal Trifecta によるデータ流出リスクは構造的かつ再現性が高い

- **Claim**: MCP エージェントが「信頼できない外部コンテンツへのアクセス」「機密データへのアクセス」「外部送信手段（メール・HTTP 等）」の3要素を同時に持つ場合、プロンプトインジェクション一発で機密データが流出しうる。これは MCP の典型的な構成で発生しうる。
- **Evidence**:
  - ファイル読み込みツール＋メール送信ツールを持つエージェントが、悪意あるファイルの内容を読み込んだ結果、その内容を攻撃者アドレスに自動送信させられた事例が報告されている。
  - 3要素が同時に存在するシナリオは、汎用エージェント（メール読み書き・ファイル操作・Web 検索を束ねる構成）では日常的な設計となっており、リスクの常在化が問題。
  - OWASP Top 10 では Context Injection & Over-Sharing（#10）として分類。
- **Source**: [industry_report] The Lethal Trifecta: Securing MCP against data flow attacks, Glama.ai (https://glama.ai, 2025) / [industry_report] Model Context Protocol: How to Reduce Security Risks, Splx.ai (https://splx.ai, 2025) / [official_document] OWASP MCP Top 10 (https://owasp.org/www-project-mcp-top-10/, 2026-03-14)
- **Confidence**: high
- **Evidence Strength**: strong

---

### Finding 3: Confused Deputy 問題（過剰権限付与）は組織的なガバナンス不備から発生する

- **Claim**: MCP サーバーに広範な OAuth スコープが付与されている場合（読み取り専用ではなくフルアクセス等）、攻撃者はサーバーを代理人として悪用し、ユーザーの意図を超えた操作を実行できる（Confused Deputy 問題）。OWASP Top 10 の Privilege Escalation via Scope Creep（#2）・Insufficient Authentication & Authorization（#7）に相当する。
- **Evidence**:
  - MCP サーバーをデプロイする際、開発者の利便性優先でフルスコープトークンを採用するケースが多い（業界慣行の問題）。
  - クライアントごとの同意粒度が粗い場合、一度承認されたサーバーが想定外のリソースにアクセスする可能性がある。
  - CVE-2025-6514：認証設定の誤りによるセッションハイジャックが実際に記録されている。
- **Source**: [official_document] Security Best Practices, Model Context Protocol Documentation (https://modelcontextprotocol.io/docs/tutorials/security/security_best_practices, 2025) / [official_document] OWASP MCP Top 10 (https://owasp.org/www-project-mcp-top-10/, 2026-03-14) / [official_document] CVE-2025-6514, Known Vulnerabilities (https://modelcontextprotocol-security.io/known-vulnerabilities/, 2026-03-14)
- **Confidence**: high
- **Evidence Strength**: strong

---

### Finding 4: サプライチェーン攻撃はパッケージエコシステムの脆弱性を直接継承する

- **Claim**: npm・pip 等で配布されるサードパーティ MCP サーバーへの悪意ある改ざん・依存関係混乱（Dependency Confusion）攻撃は、ローカル実行環境でのファイルシステム侵害や RCE（任意コード実行）に直結する。
- **Evidence**:
  - CVE-2025-49596：MCP ツールエンドポイントへの入力検証不備による RCE が記録済み。
  - MCP サーバーはローカルプロセスとして実行されることが多く、OS レベルの権限でコード実行が可能になる構造。
  - オープンソース公開が主流のため、悪意ある fork や依存パッケージへの注入が容易。
  - OWASP Top 10 では Software Supply Chain Attacks & Dependency Tampering（#4）として分類。
- **Source**: [industry_report] 11 Emerging AI Security Risks with MCP, Checkmarx (https://checkmarx.com, 2025) / [industry_report] MCP Tools: Attack Vectors and Defense Recommendations, Elastic Security Labs (https://elastic.co, 2025) / [official_document] CVE-2025-49596, Known Vulnerabilities (https://modelcontextprotocol-security.io/known-vulnerabilities/, 2026-03-14)
- **Confidence**: high
- **Evidence Strength**: strong

---

### Finding 5: プロトコル仕様レベルでの認証・アテステーション欠如（構造的欠陥）

- **Claim**: MCP 仕様自体に、サーバーが宣言する機能・権限を検証するアテステーション機構が存在せず、サーバー間連鎖（Multi-hop MCP）における信頼伝播も未定義のまま。これは実装側での補完が不可能な根本的欠陥である。
- **Evidence**:
  - Capability Attestation の欠如：接続先サーバーが主張する機能・権限を独立的に検証する手段がない。
  - Unauthenticated Sampling：双方向通信で送信元を認証する仕組みがないため、Server-to-Client プロンプトインジェクションが成立しうる。
  - Shadow MCP Servers（OWASP Top 10 #9）：未管理・不正なサーバーが信頼済みとして登録されるリスク。
  - AttestMCP 拡張提案は存在するが、2026年3月時点で標準仕様には未採用（未確定）。
- **Source**: [academic_paper] Breaking the Protocol: Security Analysis of the MCP Specification, arXiv:2601.17549v1 (https://arxiv.org/abs/2601.17549, 2026) / [industry_report] New Prompt Injection Attack Vectors Through MCP Sampling, Palo Alto Networks Unit 42 (https://unit42.paloaltonetworks.com, 2025) / [official_document] OWASP MCP Top 10 #9 (https://owasp.org/www-project-mcp-top-10/, 2026-03-14)
- **Confidence**: medium（arXiv 論文は査読前の可能性あり、AttestMCP 採用状況は未確定）
- **Evidence Strength**: moderate

---

### Finding 6: 監査・テレメトリの欠如が侵害検知を困難にする（運用上の盲点）

- **Claim**: MCP ツール呼び出しの詳細なログが取得・保存されていない場合、侵害後の原因分析・フォレンジックが著しく困難になる。これはセキュリティインシデント対応能力の根本的な低下を意味する。
- **Evidence**:
  - OWASP MCP Top 10 の Lack of Audit and Telemetry（#8）として明示的に分類されている。
  - ツール呼び出しはモデルの内部判断によって実行されるため、従来の API ゲートウェイログだけでは追跡が困難。
  - 多くの MCP 実装でデフォルトログ設定が不十分であることが指摘されている。
- **Source**: [official_document] OWASP MCP Top 10 #8 (https://owasp.org/www-project-mcp-top-10/, 2026-03-14) / [industry_report] MCP Security Checklist: Complete Protection Guide 2026, Network Intelligence (https://www.networkintelligence.ai/blogs/model-context-protocol-mcp-security-checklist/, 2026-03-14)
- **Confidence**: medium（実装状況の調査は不十分）
- **Evidence Strength**: moderate

---

## Confidence Distribution（確信度分布）

| 確信度 | 件数 | 対象 Finding |
|--------|------|-------------|
| **High** | 4件 | Finding 1, 2, 3, 4 |
| **Medium** | 2件 | Finding 5, 6 |
| **Low** | 0件 | — |

**全体の確信度評価**: MCP セキュリティリスクの主要カテゴリは OWASP・CVE・複数の独立ソースにより高い確信度で裏付けられている。プロトコル仕様レベルの詳細欠陥と新規緩和技術の採用状況については、情報源の独立性・査読状況に限界がある。

---

## Contradictory Claims（矛盾する主張）

現時点で前ステップの調査結果と本統合レビューの間に直接的な矛盾は検出されなかった。ただし以下の張力が存在する：

- **AttestMCP の有効性**: arXiv 論文では AttestMCP を有力な解決策として提示しているが、標準化・採用の現実的な見通しは未公表。「有効な解決策」と「未採用の提案」という二面性がある。→ **未解決**
- **プロトコル仕様の改訂可能性**: MCP 仕様の欠陥は「プロトコルレベルの問題」と「実装レベルの問題」で意見が分かれる可能性あり。公式ドキュメントは実装側の Best Practices を強調するが、学術論文は仕様自体の改訂を訴えている。→ **未解決**

---

## Missing Data（不足データ）

1. **実被害の定量データ**: MCP 経由の実際のセキュリティインシデント件数・被害規模の統計データが公表されていない。
2. **AttestMCP の採用タイムライン**: AttestMCP 拡張の標準化ロードマップ・採用予定時期が不明。
3. **主要 MCP クライアント（Claude Desktop・Cursor 等）の実装セキュリティ評価**: 各クライアントが上記リスクに対してどの程度対処しているかの独立評価が不足。
4. **Multi-hop MCP（エージェント連鎖）での実証的攻撃事例**: 理論的には指摘されているが、実環境での再現実験データが限られている。
5. **OAuth 2.1 実装の実態調査**: MCP サーバー実装の何割が OAuth 2.1 + PKCE を適切に実装しているかのエコシステム調査データがない。

---

## Limitations（制約・限界）

- **情報の時間的限界**: MCP は急速に進化しており、2025〜2026年初頭の情報に基づく。仕様改訂・新規 CVE 発行により状況が変化している可能性がある。
- **非公開情報の欠如**: エンタープライズ環境での実際の被害事例は多くが非公開であり、公開情報のみに基づく分析には偏りがある。
- **arXiv 論文の査読状況**: Finding 5 の主要ソース（arXiv:2601.17549v1）は査読前論文の可能性があり、技術的詳細の正確性に不確実性が残る。
- **エコシステム多様性**: MCP サーバー実装は多数存在し、個別実装のセキュリティ品質のばらつきが大きく、一般化に限界がある。

---

## Unresolved Issues（未解決事項）

1. **AttestMCP 標準化の行方**（未確定）：プロトコルレベルのアテステーション機構がいつ標準仕様に組み込まれるか不明。
2. **MCP 仕様の責任帰属**：発見された脆弱性が「プロトコル仕様の欠陥」か「実装者の責任」かの線引きが曖昧であり、修正責任が不明確。
3. **Multi-hop MCP の信頼モデル**（未確定）：複数 MCP サーバーが連鎖するシナリオでの信頼境界の定義が仕様に存在しない。
4. **Token Mismanagement の実態**：OWASP Top 10 #1 として最高リスクに分類されているが、実際のインシデント率の定量データが未取得。

---

## Conclusion（結論）

MCP のセキュリティリスクは理論的な懸念段階を超え、CVE として実記録される実害フェーズに移行している（高確信度）。プロンプトインジェクション・Lethal Trifecta・過剰権限・サプライチェーン攻撃の4リスクは複数の独立ソースにより裏付けられており、即時対応が必要な現実的脅威である。一方、プロトコル仕様レベルのアテステーション欠如とその解決策（AttestMCP 等）の標準化状況については**未確定**であり、中長期的な仕様改訂の動向を継続監視する必要がある。監査・テレメトリの実装状況も組織によりばらつきが大きく、個別評価が必要である（中確信度）。

---

## Recommended Actions（推奨アクション）

**優先度：緊急（即時実施）**

1. **最小権限の原則の徹底**: 全 MCP サーバーのOAuthスコープを最小限に絞り込み、読み取り専用・書き込み専用スコープを分離する。フルアクセストークンの即時廃止。
2. **Lethal Trifecta の構成排除**: 機密データアクセスと外部送信手段を持つツールを同一エージェントに共存させない設計制約を導入する。
3. **Human-in-the-Loop の強制化**: データ送信・ファイル変更・外部 API 呼び出し等の非可逆操作は必ずユーザー承認フローを介するよう実装する。

**優先度：高（短期実施）**

4. **サードパーティ MCP サーバーの審査プロセス確立**: 使用するすべての MCP パッケージのバージョン固定・ソース透明性確認・署名検証を義務化。内部信頼済みレジストリの構築。
5. **MCP ツール呼び出しの全量ロギング**: ツール名・引数・レスポンス・呼び出し元コンテキストを含む詳細ログを収集・保管し、異常検知に活用する。
6. **サンドボックス実行環境の導入**: MCP サーバーをコンテナ・仮想マシン等の隔離環境で実行し、ホストシステムへのアクセスを最小化する。

**優先度：中（中期実施）**

7. **OAuth 2.1 + PKCE の全面採用**: すべてのリモート MCP サーバーに OAuth 2.1 + PKCE を実装し、セッションハイジャック（CVE-2025-6514 類似）を防止する。
8. **入力スキーマ検証の厳格化**: すべてのツールインターフェースに JSON スキーマ検証を実装し、コマンドインジェクション（CVE-2025-49596 類似）を防止する。
9. **AttestMCP 動向の継続監視**: プロトコルレベルのアテステーション標準化の進捗を追跡し、採用が確定次第優先導入する。

---

## Source Registry（情報源一覧）

| # | 種別 | タイトル | ロケーター | 取得日 |
|---|------|----------|------------|--------|
| 1 | [official_document] | OWASP MCP Top 10 | https://owasp.org/www-project-mcp-top-10/ | 2026-03-14 |
| 2 | [official_document] | OWASP MCP Top 10 (GitHub) | https://github.com/OWASP/www-project-mcp-top-10 | 2026-03-14 |
| 3 | [official_document] | Security Best Practices – Model Context Protocol | https://modelcontextprotocol.io/docs/tutorials/security/security_best_practices | 2025 |
| 4 | [official_document] | Known Vulnerabilities – MCP Security | https://modelcontextprotocol-security.io/known-vulnerabilities/ | 2026-03-14 |
| 5 | [industry_report] | Protecting against indirect prompt injection attacks in MCP – Microsoft | https://techcommunity.microsoft.com/blog/microsoft-security-blog/understanding-and-mitigating-security-risks-in-mcp-implementations/4404667 | 2025 |
| 6 | [industry_report] | The Lethal Trifecta: Securing MCP against data flow attacks – Glama.ai | https://glama.ai | 2025 |
| 7 | [industry_report] | Model Context Protocol: How to Reduce Security Risks – Splx.ai | https://splx.ai | 2025 |
| 8 | [industry_report] | 11 Emerging AI Security Risks with MCP – Checkmarx | https://checkmarx.com | 2025 |
| 9 | [industry_report] | MCP Tools: Attack Vectors and Defense Recommendations – Elastic Security Labs | https://elastic.co | 2025 |
| 10 | [industry_report] | A Practical Guide for Securely Using Third-Party MCP Servers – OWASP GenAI Security Project | https://www.aigl.blog/a-practical-guide-for-securely-using-third-party-mcp-servers-owasp-genai-security-project-v1-0-oct-23-2025/ | 2025 |
| 11 | [industry_report] | MCP Security Checklist: Complete Protection Guide 2026 – Network Intelligence | https://www.networkintelligence.ai/blogs/model-context-protocol-mcp-security-checklist/ | 2026-03-14 |
| 12 | [industry_report] | New Prompt Injection Attack Vectors Through MCP Sampling – Palo Alto Networks Unit 42 | https://unit42.paloaltonetworks.com | 2025 |
| 13 | [industry_report] | MCP Security: Best Practices & Implementation Guide – The Vulnerable MCP | https://vulnerablemcp.info/security.html | 2026-03-14 |
| 14 | [academic_paper] | Breaking the Protocol: Security Analysis of the MCP Specification | https://arxiv.org/abs/2601.17549 | 2026 |
