# Specification-Driven Development (AI-Native TDD/BDD)

### Specification as Source of Truth (Spec-Driven Development)
**Claim**: 従来のコード中心の開発から、機械可読な仕様書（Spec）を正（Single Source of Truth）とする「Spec-Driven Development (SDD)」への移行が、AI開発における成功の鍵である。AI Agentの確率的な挙動を制御するため、自然言語の曖昧さを排除した構造化仕様（Markdown/YAML）が不可欠となっている。
**Evidence**: GitHub Spec Kit, SuperSpec, Kiroなどのツールが登場し、仕様書からコード、テスト、ドキュメントを一貫して生成・検証するフローが確立されつつある。これにより「Architecture Drift（仕様と実装の乖離）」を防ぐ。
**Source**: [tech_blog/tier3] [SpecDriven AI: Building Reliable Software with Precision](https://www.paulmduvall.com/specdriven-ai-combining-specs-and-tdd-for-ai-powered-development/) (2025-02-21) — primary: false
**Confidence**: high

### AI-Adapted TDD/BDD
**Claim**: AIによるコード生成において、TDDは「実装の正解判定器」としてMUSTな役割を果たす。BDD（Gherkin形式等）は、AIと人間の共通言語として機能し、"Given-When-Then" の形式がAIの推論精度（Chain of Thought）を向上させる。
**Evidence**: テストファーストで書かれたテストケースは、AIが生成したコードの幻覚（Hallucination）を即座に検知するガードレールとして機能する。
**Source**: [tech_blog/tier3] [TDD & BDD in the Age of AI](https://natshah.com/blog/tdd-bdd-age-ai-why-ai-agents-demand-100-more-test-first-development) (2025-02-21) — primary: false
**Confidence**: high

# Context Engineering (RAG & Memory Management)

### Shift from Prompt to Context Engineering
**Claim**: プロンプトエンジニアリング（単発の指示技術）から、コンテキストエンジニアリング（情報の構造化とフロー制御）へ主戦場が移行している。AIのミスの70%以上はモデル性能ではなくコンテキストの質に起因する。
**Evidence**: 企業におけるAI導入の失敗事例分析において、不適切なコンテキスト管理が主要因であることが示されている。
**Source**: [industry_report/tier2] [Context Engineering Guide](https://www.meta-intelligence.tech/en/insight-context-engineering) (2025-02-21) — primary: false
**Confidence**: high

### 3-Layer Memory Architecture
**Claim**: AI Agentのメモリ管理は、以下の3層構造で設計すべきである（SHOULD）。
1.  **Short-term (Working Memory)**: 現在のタスク実行に必要なコンテキスト（RAM相当）。
2.  **Long-term (Persistent Memory)**: ベクトルDB等を用いた外部記憶（Disk相当）。
3.  **Rulebook/Static Memory**: プロジェクトの命名規則や不変のルール（ROM相当）。
**Evidence**: 単なるログの蓄積（Dump）ではなく、情報の種類に応じた階層化が、長期間のプロジェクトにおける整合性維持に不可欠である。
**Source**: [tech_blog/tier3] [AI Agent Memory Systems: RAG vs Context Engineering](https://www.heyuan110.com/posts/ai/2026-02-21-ai-agent-memory-systems/) (2026-02-21) — primary: false
**Confidence**: medium

### Intelligent Chunking & Strategic Forgetting
**Claim**: コンテキストウィンドウの制限やノイズ低減のため、意味的なまとまりでの分割（Intelligent Chunking）と、古い情報の要約・破棄（Strategic Forgetting）が必要である。
**Evidence**: "Lost in the middle" 現象（コンテキスト中間情報の看過）を防ぐため、重要な指示は先頭と末尾に配置し、中間には検索（RAG）された関連情報のみを動的に注入する手法が推奨される。
**Source**: [tech_blog/tier3] [Deep Dive into Context Engineering for Agents](https://galileo.ai/blog/context-engineering-for-agents) (2025-02-21) — primary: false
**Confidence**: high

# AI Alignment (Thinking Synchronization)

### Structured Context & PRDs
**Claim**: AIとのアライメント（意図同期）には、自然言語のチャットよりも、構造化された「Product Requirement Documents (PRDs)」や「AGENTS.md」のようなプロジェクト定義ファイルが有効である。
**Evidence**: `AGENTS.md` のようなファイルにプロジェクトの構造、規範、制約を明記することで、AI Agentはセッションをまたいで一貫した振る舞いを維持できる。
**Source**: [community_data/tier4] [Techniques That Get More from AI Coding Assistants Today](https://www.geeky-gadgets.com/agentic-engineer-techniques/) (2025-02-21) — primary: false
**Confidence**: medium

### Human-in-the-Loop Verification
**Claim**: 特にセキュリティや安全性に関わるコード生成においては、人間による検証プロセスをループ内に組み込むことがMUSTである。
**Evidence**: AIはもっともらしいが誤った（または脆弱な）コードを生成する可能性があるため、自動テストだけでなく、人間のレビューをプロセスとして強制する必要がある。
**Source**: [industry_report/tier3] [Safety Alignment for Coding Agents](https://atoms.dev/insights/safety-alignment-for-coding-agents-risks-methodologies-and-future-trends/6537a36a8a8440c98832dcdbc20f938a) (2025-02-21) — primary: false
**Confidence**: high

# Hardware & Real-time Contexts (Embedded AI)

### Hardware-Aware AI Tools
**Claim**: 汎用LLMはハードウェア制約（メモリ、タイミング、ピン配置）に弱いため、"Embedder" のような特化型AIツールや、Flux Copilotのような回路図連動ツールの利用が推奨される（SHOULD）。
**Evidence**: Embedderはデータシートや回路図からMISRA C準拠のドライバを生成し、SIL/HILテストまでサポートする。Flux CopilotはPCB設計とファームウェアの不整合を解消する。
**Source**: [official_document/tier2] [Embedder | AI Firmware Engineer](https://embedder.com/) (2025-02-21) — primary: true
**Confidence**: high

### Edge AI & TinyML Frameworks
**Claim**: リアルタイム制御においては、クラウド依存を避けるため、TinyML（TensorFlow Lite for Microcontrollers等）を用いたエッジ推論の実装が進んでいる。TI等のベンダーはMCU内蔵NPU（TinyEngine）向けの最適化環境を提供している。
**Evidence**: Texas Instrumentsの新MCU（MSPM0G5187等）は、エッジAI推論を低遅延・省電力で実行するためのハードウェアアクセラレータと開発環境を統合している。
**Source**: [official_document/tier1] [TI expands microcontroller portfolio... to enable edge AI](https://www.ti.com/about-ti/newsroom/news-releases/2026/2026-03-10-ti-expands-microcontroller-portfolio-and-software-ecosystem-to-enable-edge-ai-in-every-device.html) (2026-03-10) — primary: true
**Confidence**: high

### Safety Standards Compliance (ISO 26262/DO-178C)
**Claim**: 機能安全規格（ISO 26262, DO-178C）が求められる領域では、AI生成コードのトレーサビリティと検証が厳格に要求される（MUST）。汎用AIツールの出力をそのまま使用することは規格違反となるリスクが高い。
**Standard Reference**: ISO 26262 (Automotive), DO-178C (Aerospace) — partial mapping (Human verification required)
**Evidence**: 認証プロセスにおいて、コードの生成根拠とテスト結果の紐付けが必須であり、ブラックボックスなAI生成は課題となる。専用ツールによるドキュメント生成支援が有効。
**Source**: [tech_blog/tier3] [AI Code Generation in Embedded Systems | Real Constraints](https://www.wedolow.com/resources/vibe-coding-ai-code-generation-embedded-systems) (2025-02-21) — primary: false
**Confidence**: medium

## Summary & Recommendations

1.  **仕様の構造化**: 自然言語の指示ではなく、Markdown/YAML等で構造化された「実行可能な仕様書」を作成し、これをAIへの入力の正とする (SDD)。
2.  **メモリ設計**: AIツールの導入に際し、短期記憶（コンテキスト）と長期記憶（RAG/Docs）を明確に分けたアーキテクチャを採用する。
3.  **ハードウェア連携**: 組み込み開発においては、汎用LLMのコードを鵜呑みにせず、ハードウェア特化型ツール（Embedder等）の併用や、HIL/SILテストによる物理的な検証ループを構築する。
