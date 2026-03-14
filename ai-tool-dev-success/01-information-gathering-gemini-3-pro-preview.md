## Specification-Driven Development (SDD)

*   **Claim**: AIツール開発の成功率は、曖昧なプロンプト駆動（Vibe Coding）から、明確な仕様書を正解とする「仕様駆動開発」への移行によって劇的に向上する。
*   **Evidence**:
    *   自然言語プロンプトのみの指示では、エッジケースや非機能要件（セキュリティ、エラー処理）が欠落しやすく、手戻りが多発する。
    *   Markdown形式やEARS記法などで記述された構造化仕様書（Requirements, Behavior, Constraints）を「正解（Source of Truth）」としてAIに与えることで、生成コードの信頼性と再現性が確保される。
    *   GitHub Spec KitやAWS Kiroなどのツールは、仕様策定→計画→実装のフェーズを分離し、AIエージェントが仕様書に基づいて自律的にコードを検証するワークフローを採用している。
*   **Source**:
    *   [industry_report/tier3] [AI 101: From Vibe Coding to Spec-Driven Development](https://www.turingpost.com/p/sdd) (2025-03-14) — primary: false
    *   [official_document/tier2] [GitHub Spec Kit](https://github.com/github/spec-kit) (2025-03-14) — primary: true
    *   [community_data/tier3] [Spec-Driven Development: Write the Spec, Not the Code](https://dev.to/bobbyblaine/spec-driven-development-write-the-spec-not-the-code-2p5o) (2025-03-14) — primary: false
*   **Confidence**: high

## AI Thought Synchronization (Human-AI Alignment)

*   **Claim**: 開発者の「メンタルモデル」とAIの「コンテキスト」を同期させることが、意図通りのツールを作成するために不可欠である (MUST)。
*   **Evidence**:
    *   "AI Thought Synchronization" という単一の確立された技術用語は存在しないが、Human-AI Alignmentの文脈で「思考パートナー（Thought Partner）」としての同期技術が研究されている。
    *   成功するAI開発フローでは、開発者の暗黙知（背景、制約、好み）を明示的なデータ（Notes, Context Files）として外部化し、AIが常に参照可能な状態にする「同期」プロセスが必要となる。
    *   ハードウェア開発における「クロック同期」のように、人間の思考の変化をリアルタイムでAIのコンテキストに反映させる仕組み（Mutual Update）が推奨される (SHOULD)。
*   **Source**:
    *   [industry_report/tier3] [Reflect Notes: AI Thought Partner](https://reflect.app/) (2025-03-14) — primary: true
    *   [unknown/tier4] [Search Result: AI Thought Synchronization landscape](https://www.bing.com/search?q=search%28%22%22AI+Thought+Synchronization%22+human-AI+alignment+software+development%22%29) (2025-03-14) — primary: false
*   **Confidence**: medium

## Context Engineering

*   **Claim**: 高度なAIツール開発では、プロンプトエンジニアリング以上に、AIに提供する情報の構造設計（コンテキストエンジニアリング）が重要である (MUST)。
*   **Evidence**:
    *   **Transient vs. Persistent**: 一時的な会話（Transient）と、プロジェクト全体の不変の知識（Persistent）を明確に分離して管理するアーキテクチャが必須である。
    *   **Modular Context**: `CONTEXT.md` や `.context/architecture.md` のように、コンテキストをモジュール化・ファイル化し、タスクに応じて動的に注入する手法が標準化しつつある。
    *   **Context Compaction**: トークン制限と推論精度維持のため、過去の履歴を単純に積み上げるのではなく、要約・圧縮（Compaction）して「現在の重要な状態」のみを維持する戦略が必要。
    *   LangChainなどのフレームワークは、このコンテキスト管理をミドルウェア層で自動化する機能を提供している。
*   **Source**:
    *   [official_document/tier2] [Context engineering in agents - Docs by LangChain](https://docs.langchain.com/oss/python/langchain/context-engineering) (2025-03-14) — primary: true
    *   [academic_paper/tier2] [Building Effective AI Coding Agents for the Terminal](https://arxiv.org/abs/2603.05344) (2025-03-14) — primary: true
    *   [community_data/tier3] [Why Context Engineering Will Replace Traditional Architecture Thinking](https://dev.to/jaideepparashar/why-context-engineering-will-replace-traditional-architecture-thinking-4e39) (2025-03-14) — primary: false
*   **Confidence**: high

## Strategic Recommendations

*   **Specification First (MUST)**: 実装前に必ず Markdown で詳細仕様書（`spec.md`等）を作成し、AIに「この仕様に基づいて実装せよ」と指示するフローを確立すること。
*   **Explicit Context (SHOULD)**: プロジェクトルートに `CONTEXT.md` を配置し、設計思想・技術選定理由・コーディング規約を明文化して、AIが常に参照できるようにすること（ハードウェアのDatasheetのように扱う）。
*   **State Sync (MAY)**: 開発セッションの開始時と終了時に、現状のタスク状態と次の目標をAIと共有・更新する時間を設け、思考のズレ（非同期状態）を防ぐこと。
