# AI Research Results

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
