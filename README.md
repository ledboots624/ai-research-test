# AI Research Results

> **Notice**: 本リポジトリの分析結果にはAI生成内容が含まれます。
> 最終判断には原典の確認を推奨します。

> **Warning**: 本リポジトリは [AI Research Agent](https://github.com/ledboots624/ai-research-agent) のテスト・検証目的で運用しています。
> 調査結果の正確性・網羅性は保証されません。内容は予告なく変更・削除される場合があります。

[AI Research Agent](https://github.com/ledboots624/ai-research-agent) によるマルチモデル分業リサーチの結果を公開するリポジトリです。

## 構成

```
<topic>/
  .plan.json              # パイプライン定義
  01-<role>-<model>.md    # 各ステップの出力
  02-<role>-<model>.md
  ...
  README.md               # 自動生成サマリー
```

## 生成方法

```bash
./research.sh -o /path/to/this/repo "<topic>" "<description>"
```
