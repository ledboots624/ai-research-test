# AI Research Agent Comparison: Design Philosophy & Architecture

## 1. LangChain
**Design Philosophy**: "Composability & Control" (構成可能性と制御)
**Architecture**: Layered & Chain-based (Chain, ReAct, LangGraph)

*   **Claim**: 最小単位のコンポーネント（Prompt, Model, Tool）を「Chain」として結合し、複雑なワークフローを構築する設計思想。
    *   **Evidence**: プロンプト管理、メモリ、ツール操作がモジュール化されており、開発者は必要な部品を組み合わせて独自のエージェントを設計可能。ステートフルなグラフ構造（LangGraph）により、ループや条件分岐を含む高度な制御を実現する。
    *   **Source**: [industry_report] AI Agent Framework Comparison: LangChain vs AutoGPT vs CrewAI (fast.io, 2026-03-13)
    *   **Confidence**: high

*   **Claim**: 汎用性とエコシステム連携を重視し、特定のモデルやプロバイダーに依存しない抽象化層を持つ。
    *   **Evidence**: "Model Layer"によるLLMの切り替え容易性と、LangSmithによる可観測性（Observability）の統合。
    *   **Source**: [official_document] Philosophy - Docs by LangChain (docs.langchain.com, 2026-03-13)
    *   **Confidence**: high

## 2. CrewAI
**Design Philosophy**: "Role-Based Team Collaboration" (役割ベースのチーム協調)
**Architecture**: Multi-Agent Orchestration (Agent, Task, Crew, Process)

*   **Claim**: 人間の組織構造を模倣し、各エージェントに明確な「役割（Role）」と「目標（Goal）」を与えることで協調動作させる思想。
    *   **Evidence**: "Crew"という単位でエージェントをグループ化し、Sequential（順次）またはHierarchical（階層的）なプロセスでタスクを実行する。各エージェントは独立したバックストーリーと専門性を持つ。
    *   **Source**: [industry_report] Framework Deep Dive: CrewAI (turion.ai, 2026-03-13)
    *   **Confidence**: high

*   **Claim**: 複雑なオーケストレーションを抽象化し、チーム編成のような直感的な定義でマルチエージェントシステムを構築可能にする。
    *   **Evidence**: LangChainのような低レイヤーの制御よりも、高レベルな「誰が何をやるか」の定義に注力する設計。
    *   **Source**: [community_data] CrewAI: The Revolutionary Multi-Agent Framework (blog.brightcoding.dev, 2026-03-13)
    *   **Confidence**: medium

## 3. AutoGPT
**Design Philosophy**: "Autonomous Recursive Loop" (自律的再帰ループ)
**Architecture**: Thought-Action-Observation Loop (Plan, Criticize, Act)

*   **Claim**: 人間の介入を最小限にし、目標達成まで自律的に思考・実行・修正を繰り返す「Set-and-Forget」型の設計。
    *   **Evidence**: "Plan, Criticize, Act, Read Feedback, Re-plan"というサイクルを回し、自己批判（Self-Correction）機能により軌道修正を行う。
    *   **Source**: [community_data] AutoGPT: An Introduction to Autonomous AI Agents (atoms.dev, 2026-03-13)
    *   **Confidence**: high

*   **Claim**: 短期・長期メモリ（Vector DB）を活用し、継続的なタスク実行とコンテキスト保持を行う。
    *   **Evidence**: 以前のアクション結果を記憶し、次の意思決定に活用する永続化の仕組みがアーキテクチャの中核にある。
    *   **Source**: [industry_report] AutoGPT: Architecture, Performance, & Applications (labellerr.com, 2026-03-13)
    *   **Confidence**: high

## 4. OpenAI Agents (Assistants API / Swarm)
**Design Philosophy**: "Managed Stateful Infrastructure" (マネージドなステートフル基盤)
**Architecture**: Server-side State Management (Assistant, Thread, Run)

*   **Claim**: 開発者がコンテキスト管理や履歴保持を行う必要がない「ステートフル（Stateful）」なAPI設計。
    *   **Evidence**: "Thread"オブジェクトが会話履歴をサーバー側で保持し、トークン管理や切り捨てを自動化する。これによりクライアント側の実装負荷を大幅に軽減している。
    *   **Source**: [official_document] Assistants API Overview (OpenAI Cookbook, 2026-03-13)
    *   **Confidence**: high

*   **Claim**: インフラ管理不要で、Code InterpreterやFile Searchなどのツールをサーバーサイドで実行・統合する。
    *   **Evidence**: 関数呼び出し（Function Calling）やサンドボックス実行環境がAPIの一部として提供され、スケーラビリティとセキュリティがプラットフォーム側で担保される。
    *   **Source**: [industry_report] OpenAI Assistants API Enterprise Application Guide (dev.to, 2026-03-13)
    *   **Confidence**: high

---

### 結論・推奨

*   **LangChain**: 細かい制御とカスタマイズが必要な**本番環境の独自アプリケーション**向け。学習コストは高いが自由度は最大。
*   **CrewAI**: 複数の専門家エージェントを連携させる**チーム・組織シミュレーション**向け。役割分担が明確なタスクに最適。
*   **AutoGPT**: 目標だけを与えて自律的に試行錯誤させる**実験的・探索的タスク**向け。完全自律型だが挙動の予測が難しい場合がある。
*   **OpenAI Agents**: インフラ管理を避け、**素早く安定したエージェント機能を組み込みたい**場合向け。カスタマイズ性はAPIの制限を受けるが、構築速度と安定性は高い。
