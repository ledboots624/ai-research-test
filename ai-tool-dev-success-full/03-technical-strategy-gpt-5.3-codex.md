調査の根拠を固めるため、まず公式ドキュメント中心に一次情報をまとめて取得します。続けて、この情報をあなたの実装者プロファイル向けアーキテクチャに落とし込みます。

# Technical Strategy Blueprint（技術戦略ブループリント）

## Findings（主要な発見）

### Claim 1（主張）
フルスタック（Web/モバイル）＋ファームウェア＋回路設計の横断スキルを持つため、**「センサー取得→エッジ推論→クラウド推論→UI反映」までを単一設計原則で実装**できる。これは本テーマで最重要の成功条件。

**Evidence（根拠）**  
提示プロフィールはソフト・ハード両面を含み、分断されがちな「データ取得層」と「AI推論層」と「アプリ層」を同時に設計可能。

**Source（情報源）**  
- source_type: unknown; source_title: User-provided person_profile; source_locator: User input in current prompt; access_date: 2026-03-14; reliability_tier: tier4; primary_source: false

**Confidence（確信度）**: high
### Claim 2（主張）
センサーデータとユーザー操作ログは、**MQTT + Trace Context + OpenTelemetry属性**で統合するのが実装効率と可観測性の両面で最適。

**Evidence（根拠）**  
MQTTは制約デバイス向け軽量Pub/Sub標準。W3C Trace Contextは分散トレース伝搬を標準化。OpenTelemetry semantic conventionsはログ/トレース/メトリクスの属性統一を提供。

**Source（情報源）**  
- [official_document/tier1] [MQTT Version 5.0 - OASIS](https://docs.oasis-open.org/mqtt/mqtt/v5.0/os/mqtt-v5.0-os.html) (2026-03-14) — primary_source: true  
- [official_document/tier1] [Trace Context - W3C](https://www.w3.org/TR/trace-context/) (2026-03-14) — primary_source: true  
- [official_document/tier2] [Semantic Conventions - OpenTelemetry](https://opentelemetry.io/docs/concepts/semantic-conventions/) (2026-03-14) — primary_source: true

**Confidence（確信度）**: high
### Claim 3（主張）
AI実装は**Schema-first（JSON Schema 2020-12）+ Strict Structured Output/Function Calling**を採用しないと、運用で破綻しやすい。

**Evidence（根拠）**  
OpenAI/Anthropic/Geminiはいずれも構造化出力・ツール呼び出し時のスキーマ拘束を公式提供。JSON Schema 2020-12は基盤仕様。実装時にZod/Pydanticで二重検証すると型乖離を抑制できる。

**Source（情報源）**  
- [official_document/tier1] [JSON Schema Draft 2020-12](https://json-schema.org/draft/2020-12) (2026-03-14) — primary_source: true  
- [official_document/tier2] [Structured model outputs - OpenAI API](https://developers.openai.com/api/docs/guides/structured-outputs/) (2026-03-14) — primary_source: true  
- [official_document/tier2] [Function calling - OpenAI API](https://developers.openai.com/api/docs/guides/function-calling) (2026-03-14) — primary_source: true  
- [official_document/tier2] [Structured outputs - Claude API Docs](https://platform.claude.com/docs/en/build-with-claude/structured-outputs) (2026-03-14) — primary_source: true  
- [official_document/tier2] [Structured outputs - Gemini API](https://ai.google.dev/gemini-api/docs/structured-output) (2026-03-14) — primary_source: true  
- [official_document/tier2] [Function calling - Gemini API](https://ai.google.dev/gemini-api/docs/function-calling) (2026-03-14) — primary_source: true  
- [official_document/tier2] [Pydantic JSON Schema](https://docs.pydantic.dev/latest/concepts/json_schema/) (2026-03-14) — primary_source: true  
- [official_document/tier2] [Zod Docs](https://zod.dev/) (2026-03-14) — primary_source: true

**Confidence（確信度）**: high
### Claim 4（主張）
ハイブリッド構成は**エッジ=低遅延・安全系、クラウド=高文脈推論**に責務分離するのが実装者視点で最も再現性が高い。

**Evidence（根拠）**  
TFLite Micro/ONNX Runtimeはエッジ推論向け。AWS Greengrass/Azure IoT Edgeはクラウド管理下のエッジ配備・更新を公式サポート。

**Source（情報源）**  
- [official_document/tier2] [TensorFlow Lite for Microcontrollers](https://www.tensorflow.org/lite/microcontrollers) (2026-03-14) — primary_source: true  
- [official_document/tier2] [Deploy on IoT and edge - ONNX Runtime](https://onnxruntime.ai/docs/tutorials/iot-edge/) (2026-03-14) — primary_source: true  
- [official_document/tier2] [Perform ML inference - AWS IoT Greengrass](https://docs.aws.amazon.com/greengrass/v2/developerguide/perform-machine-learning-inference.html) (2026-03-14) — primary_source: true  
- [official_document/tier2] [Azure IoT Edge documentation](https://learn.microsoft.com/en-us/azure/iot-edge/) (2026-03-14) — primary_source: true

**Confidence（確信度）**: high
## Reference Architecture（参照アーキテクチャ）

```text
[MCU Sensors/FW] --MQTT5--> [Edge Gateway] --TLS--> [Cloud Ingestion]
      |                         |                       |
      | local features          | traceparent           | stream join
      v                         v                       v
 [Edge AI Runtime]         [Policy Gate]         [Context Assembler]
 (TFLM/ONNX RT)            (deterministic)       (sensor + UX logs)

 [Web/Mobile Apps] --OTel Logs/Traces--> [Telemetry Pipeline] ---+
                                                                  |
                                                                  v
                                                         [Context Store]
                                                   (hot cache + timeline + summary)
                                                                  |
                                                                  v
                                                         [AI Orchestrator]
                                          (Function Calling / Structured Output strict)
                                                                  |
                                                                  v
                                                       [Action API / UI / Alert]
```

## Context Backbone Design（コンテキスト管理基盤設計）

### Claim 5（主張）
コンテキスト基盤は「Write/Select/Compress/Isolate」方式で設計し、**生ログを直接LLMに渡さず、選別済みスナップショットを渡す**べき。

**Evidence（根拠）**  
Anthropicのcontext engineeringで、長期タスクでは選別・圧縮・分離が性能維持に重要と整理されている。

**Source（情報源）**  
- [official_document/tier2] [Effective context engineering for AI agents - Anthropic](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents) (2026-03-14) — primary_source: true

**Confidence（確信度）**: high

**実装仕様（あなた向け具体案）**  
- `context_event`共通スキーマ（最低必須）  
  - `event_id`, `timestamp`, `device_id`, `user_id`, `session_id`, `trace_id`, `event_type`, `payload`, `privacy_tag`, `source`
- 取り込み
  - センサー: MQTT topic `plant/{device_id}/telemetry/*`
  - UXログ: Web/Mobile SDKからOTelで送信
- 結合キー
  - 第1キー: `trace_id`
  - 第2キー: `session_id`
  - 第3キー: 時間窓（例: ±3秒）
- 3層ストア
  - Hot（5〜30分）: 直近判断用
  - Warm（24時間）: 行動文脈用
  - Cold（監査）: 法令/障害解析用

## Spec-Driven AI Pattern（仕様駆動AI実装パターン）

**推奨パターン**  
1. `schemas/*.json` をSingle Source of Truth化（JSON Schema 2020-12）  
2. 型生成  
   - FE/BFF: Zod/TypeScript  
   - AI Gateway/Batch: Pydantic/Python  
3. 実行時検証  
   - 入力: API Gatewayでschema validate  
   - モデル出力: `strict` structured output + 再validate  
4. 決定論ゲート  
   - 閾値・禁止操作・安全制約は**LLM外**で判定

```json
{
  "type": "object",
  "properties": {
    "action": { "type": "string", "enum": ["notify", "throttle", "ignore"] },
    "reason": { "type": "string" },
    "risk_score": { "type": "number", "minimum": 0, "maximum": 1 }
  },
  "required": ["action", "risk_score"],
  "additionalProperties": false
}
```

## Hybrid Edge-Cloud AI Design（エッジAIとクラウドAIのハイブリッド構成）

### 役割分担（実装者視点）
- **Edge MUST**: 100ms以下応答、通信断時継続、機器保護ロジック
- **Cloud SHOULD**: 長文脈推論、モデル改善、横断最適化
- **Fallback MUST**: Cloud失敗時はedge deterministicルールへ退避
- **MUST_NOT**: 安全クリティカル制御をLLM直接出力で実行しない

**Source（情報源）**  
- [official_document/tier2] [TensorFlow Lite for Microcontrollers](https://www.tensorflow.org/lite/microcontrollers) (2026-03-14) — primary_source: true  
- [official_document/tier2] [Deploy on IoT and edge - ONNX Runtime](https://onnxruntime.ai/docs/tutorials/iot-edge/) (2026-03-14) — primary_source: true  
- [official_document/tier2] [AWS IoT Greengrass ML inference](https://docs.aws.amazon.com/greengrass/v2/developerguide/perform-machine-learning-inference.html) (2026-03-14) — primary_source: true  
- [official_document/tier2] [Azure IoT Edge](https://learn.microsoft.com/en-us/azure/iot-edge/) (2026-03-14) — primary_source: true

**Confidence（確信度）**: high

## Compliance Mapping（規格・法令マッピング）

### Claim 6（主張）
この構成はEU CRAに対しては直接対応しやすい。一方、IEC 62443 / ISO/SAE 21434 / SEMI E187は**条文粒度で追加マッピングが必要**。

**Evidence（根拠）**  
EU CRAはArticle 13とAnnex Iで製品特性・脆弱性対応を要求。NIST CSF 2.0はGovern機能を提供。IEC/ISO/SEMIは標準本文の詳細確認が必要（有償・[未確定]箇所あり）。

**Source（情報源）**  
- [official_document/tier1] [Regulation (EU) 2024/2847 - EUR-Lex](https://eur-lex.europa.eu/eli/reg/2024/2847/oj/eng) (2026-03-14) — primary_source: true  
- [official_document/tier1] [NIST CSF 2.0 (CSWP 29)](https://nvlpubs.nist.gov/nistpubs/CSWP/NIST.CSWP.29.pdf) (2026-03-14) — primary_source: true  
- [official_document/tier1] [ISA/IEC 62443 Series Overview](https://www.isa.org/standards-and-publications/isa-standards/isa-iec-62443-series-of-standards) (2026-03-14) — primary_source: true  
- [official_document/tier1] [ISO/SAE 21434:2021](https://www.iso.org/standard/70918.html) (2026-03-14) — primary_source: true  
- [official_document/tier1] [SEMI E187](https://store-us.semi.org/products/e18700-semi-e187-specification-for-cybersecurity-of-fab-equipment) (2026-03-14) — primary_source: true

**Confidence（確信度）**: medium（IEC/ISO/SEMIの節番号は一部[未確定]）

| Control | Requirement Level | Standard Reference | Mapping |
|---|---|---|---|
| セキュア設計・既知脆弱性排除・更新機構 | MUST | EU CRA Article 13, Annex I Part I/II | direct |
| アクセス制御・監視ログ・改ざん検知 | MUST | EU CRA Annex I, NIST CSF 2.0 (GV/DE/PR) | direct |
| 組織ガバナンス（責任・方針・供給網） | SHOULD→実務上MUST化推奨 | NIST CSF 2.0 GV.OC/GV.RM/GV.RR/GV.SC | partial |
| ゾーン/コンジット分離 | SHOULD（産業制御系はMUST寄り） | IEC 62443-2-1/3-x（節番号[未確定]） | partial |
| ライフサイクル脅威分析（車載等） | SHOULD（対象業界ではMUST） | ISO/SAE 21434:2021（節番号[未確定]） | partial |
| 半導体製造装置の機器要件 | MAY（対象業界ではMUST） | SEMI E187（節番号[未確定]） | gap/partial |

### SHOULD解釈の差異（重要）
- **EU CRA文脈**: 法規要件は市場投入条件に直結し、実装上はMUST扱いが安全。  
- **SEMI/業界標準文脈**: SHOULDは通常推奨だが、顧客調達仕様でMUST化されうる。  

**Source（情報源）**  
- [official_document/tier1] [Regulation (EU) 2024/2847 - EUR-Lex](https://eur-lex.europa.eu/eli/reg/2024/2847/oj/eng) (2026-03-14) — primary_source: true  
- source_type: unknown; source_title: User-provided requirement note on SHOULD interpretation; source_locator: User input in current prompt; access_date: 2026-03-14; reliability_tier: tier4; primary_source: false

**Confidence（確信度）**: medium（SEMI本文での厳密運用は[未確定]）

## Implementation Roadmap（実装ロードマップ）

1. **Phase 1（2週間）**: イベント契約統一  
   - MQTT topic規約、`trace_id`統一、OTel属性辞書を固定。

2. **Phase 2（2〜3週間）**: Context Assembler実装  
   - センサー×UXログ結合、Hot/Warm/Coldストア、要約生成。

3. **Phase 3（2週間）**: AI Gateway厳格化  
   - JSON Schema SSoT、Function Calling/Structured Output strict、Zod/Pydantic再検証。

4. **Phase 4（2週間）**: Edge/Cloudルータ本番化  
   - エッジ判定閾値、クラウド委譲条件、通信断フォールバック、監査ログ。

## Final Recommendations（結論・推奨事項）

- 最初に作るべきはモデルではなく、**統合コンテキスト契約（event schema + trace）**。  
- AI導入は必ず **Schema-first + strict出力 + 決定論ゲート** で進める。  
- あなたの強みを最大活用するなら、**エッジ安全系（Firmware主導）+クラウド知能系（Full-stack主導）** の二層分離が最短。  
- 規格対応はEU CRAを起点に直対応し、IEC 62443/ISO 21434/SEMI E187は対象業界に応じて差分実装する。
