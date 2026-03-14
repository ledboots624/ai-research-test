# AI Research Results

> **Notice**: 本リポジトリの分析結果にはAI生成内容が含まれます。
> 最終判断には原典の確認を推奨します。

> **Warning**: 本リポジトリは [AI Research Agent](https://github.com/ledboots624/ai-research-agent) のテスト・検証目的で運用しています。
> 調査結果の正確性・網羅性は保証されません。内容は予告なく変更・削除される場合があります。

[AI Research Agent](https://github.com/ledboots624/ai-research-agent) によるマルチモデル分業リサーチの結果を公開するリポジトリです。

## 調査一覧

| ディレクトリ | テーマ | バージョン | ステップ数 | 所要時間 | 前提条件 |
|------------|--------|----------|-----------|---------|---------|
| [japan-semiconductor-cra](./japan-semiconductor-cra/) | 日本半導体装置関連ソフトウェア開発企業のEU CRA対策の打開策 | v2.2.0 | 6 | 34m5s | なし |
| [iran-war-resolution-full](./iran-war-resolution-full/) | イラン戦争収束の鍵（関係国分析・日本への影響・世界大戦リスク） | v2.2.0 | 5 | 43m24s | あり |
| [ai-tool-dev-success-full](./ai-tool-dev-success-full/) | AIツール開発成功の鍵（仕様駆動開発、AI思考同期、コンテキストエンジニアリング） | v2.2.0 | 5 | 38m57s | あり |
| [ai-tool-dev-success](./ai-tool-dev-success/) | AIツール開発成功の鍵（クイックモード） | v2.2.0 | 2 | 8m46s | あり |
| [bass-mastery](./bass-mastery/) | ベース上達への秘訣（ロック・ファンク、初心者向け） | v2.2.0 | 6 | 41m56s | あり |
| [live-venue-tokyo-soka](./live-venue-tokyo-soka/) | 生演奏できるお店リサーチ（東京・草加周辺） | v2.2.0 | 5 | 40m44s | あり |
| [logic-analyzer-japan](./logic-analyzer-japan/) | 日本から購入可能なロジックアナライザーリサーチ（3万円以下・Mac M4対応） | v2.2.0 | 5 | 52m32s | あり |
| [shaktimat-review](./shaktimat-review/) | シャクティマットの効能・評価リサーチ（購入検討・科学的根拠） | v2.2.0 | 5 | 32m33s | あり |
| [iran-war-resolution](./iran-war-resolution/) | イラン戦争収束の鍵（クイックモード、v2.2.0以前） | — | 2 | — | なし |
| [japan-sw-regulation](./japan-sw-regulation/) | 日本のソフトウェア開発における法規制とガイドライン | — | 3 | — | なし |

## 構成

各調査ディレクトリの構成:

```
<topic>/
  README.md               # サマリー（主要ドキュメントへのリンク付き）
  .context.json            # 前提条件（--context 指定時のみ）
  .plan.json               # パイプライン定義
  .validation.json         # バリデーション結果
  .execution.json          # 実行メタデータ（時間・モデル・成果物）
  .research.log            # 実行ログ
  01-<role>-<model>.md     # 各ステップの出力
  01-<role>-<model>.evidence.json  # エビデンス（機械可読）
  ...
  sources.json             # 情報源レジストリ
  diagram.drawio / .svg / .png  # 関係図
  slides.md / .html / .pdf      # プレゼンスライド
```

## 生成方法

```bash
# 基本（フルモード）
./research.sh -o /path/to/this/repo "<topic>" "<description>"

# 前提条件付き
./research.sh -c context.json -o /path/to/this/repo "<topic>" "<description>"

# クイックモード
./research.sh -m quick -o /path/to/this/repo "<topic>" "<description>"
```

詳細は [AI Research Agent](https://github.com/ledboots624/ai-research-agent) を参照。
