## Executive Summary（概要）

Model Context Protocol (MCP) は LLM と外部ツールを接続する標準プロトコルとして普及しているが、従来の API リスクに加え、プロンプトインジェクションによる自律的な攻撃やプロトコル固有の構造的脆弱性を抱えている。特に「Lethal Trifecta（信頼できないコンテンツ、プライベートデータ、外部通信チャネルの組み合わせ）」によるデータ流出や、過剰な権限付与による「Confused Deputy（混乱した代理人）」問題が主要な懸念事項である。

## Security Risks（セキュリティリスク）

### Prompt Injection & Tool Poisoning（プロンプトインジェクションとツール汚染）
- **Claim**: 攻撃者はツール定義や外部データを操作し、エージェントに意図しないツール実行を強制できる。
- **Evidence**:
    - **Tool Poisoning**: 悪意のあるツール説明文（description）を用いて、LLM にそのツールを不適切なタイミングで呼び出させる手法。
    - **Indirect Prompt Injection**: メールやカレンダー、ファイルなどの外部入力に隠された命令により、ユーザーの同意なしにデータを送信したりツールを実行したりする。
- **Source**:
    - [industry_report] Protecting against indirect prompt injection attacks in MCP (Microsoft Developer Blog, 2025)
    - [official_document] MCP Security Cheat Sheet (OWASP, 2025)
- **Confidence**: high

### Data Leakage & Exfiltration（データ漏洩と流出）
- **Claim**: LLM が機密データと外部送信可能なツール（メール送信など）の両方にアクセスできる場合、データ流出のリスクが極めて高い。
- **Evidence**:
    - **Lethal Trifecta**: 「信頼できないコンテンツへのアクセス」「機密データへのアクセス」「外部への送信手段」の3要素が揃うと、プロンプトインジェクションにより機密情報が外部に送信される危険性が生じる。
    - 具体例として、ファイル読み込みツールとメール送信ツールを持つエージェントが、悪意あるファイルの内容を読み込んだ結果、その内容を攻撃者にメール送信させられるケースが報告されている。
- **Source**:
    - [industry_report] The Lethal Trifecta: Securing MCP against data flow attacks (Glama.ai, 2025)
    - [industry_report] Model Context Protocol: How to Reduce Security Risks (Splx.ai, 2025)
- **Confidence**: high

### Authorization & Access Control（認証とアクセス制御）
- **Claim**: MCP サーバーの権限管理が不十分な場合、ユーザーの意図を超えた操作が可能になる「Confused Deputy」問題が発生する。
- **Evidence**:
    - **Over-privileged Tokens**: MCP サーバーがユーザーごとの細かいスコープではなく、広範な OAuth トークン（例：読み取り専用でなくフルアクセス）で動作する場合、攻撃者はサーバーの権限を悪用できる。
    - **Granularity Issues**: クライアントごとの同意（Consent）が粒度細かく制御されていない場合、一度承認されたサーバーが想定外のデータにアクセスする可能性がある。
- **Source**:
    - [official_document] Security Best Practices (Model Context Protocol Documentation, 2025)
    - [industry_report] MCP Security Cheat Sheet (OWASP, 2025)
- **Confidence**: high

### Supply Chain Vulnerabilities（サプライチェーンの脆弱性）
- **Claim**: 信頼できないサードパーティ製 MCP サーバーやプラグインの導入が、システム侵害の直接的な経路となる。
- **Evidence**:
    - MCP サーバーはオープンソース（npm, pip 等）で配布されることが多く、悪意のあるパッケージや、正規パッケージへの侵害がリスクとなる。
    - ローカルで実行される侵害された MCP サーバーは、ファイルシステムの読み書きや任意のコード実行（RCE）につながる恐れがある。
- **Source**:
    - [industry_report] 11 Emerging AI Security Risks with MCP (Checkmarx, 2025)
    - [industry_report] MCP Tools: Attack Vectors and Defense Recommendations (Elastic Security Labs, 2025)
- **Confidence**: high

### Protocol-Level Flaws（プロトコルレベルの欠陥）
- **Claim**: MCP の仕様自体に、認証や信頼性の検証機能が不足している点が指摘されている。
- **Evidence**:
    - **Lack of Capability Attestation**: サーバーが宣言する機能や権限を検証する仕組みがない。
    - **Unauthenticated Sampling**: 双方向通信において送信元の認証保証がなく、サーバー側からクライアントへのプロンプトインジェクション（Server-to-Client）が可能。
    - **Implicit Trust Propagation**: 複数の MCP サーバーが連鎖する場合、一つの悪意あるサーバーがダウンストリーム全体の判断に影響を及ぼす。
- **Source**:
    - [academic_paper] Breaking the Protocol: Security Analysis of the MCP Specification (arXiv:2601.17549v1, 2026)
    - [industry_report] New Prompt Injection Attack Vectors Through MCP Sampling (Palo Alto Networks Unit 42, 2025)
- **Confidence**: medium (特定の研究に基づく指摘)

## Mitigation Strategies（推奨される緩和策）

- **最小権限の原則**: MCP サーバーには必要最小限の権限と短命なトークンのみを付与する。
- **Human-in-the-loop**: 重要なツール実行（データ送信、変更操作）の前には必ずユーザー承認を求める。
- **サンドボックス化**: MCP サーバーをコンテナや隔離環境で実行し、ローカルシステムへのアクセスを制限する。
- **アテステーション**: 将来的には AttestMCP 拡張などの導入により、接続先サーバーの正当性を検証する仕組みが求められる。
