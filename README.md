# Nutrition-Management

個人用の栄養・体重・運動・食費を、同じ形式で継続記録するためのPrivateリポジトリです。
構造、テンプレート、レシピに加えて、本人の体重・食事・運動・食費などの実記録もGitで履歴管理します。実記録を追加する前に、GitHub上の可視性が必ず `Private` になっていることを確認してください。

## 管理目標

| 項目 | 目安 |
| --- | ---: |
| 1日の摂取カロリー | 1,800 kcal |
| 1日のタンパク質 | 115〜125 g |
| 自炊予算 | 30日で20,000円 |
| 自炊予算の日割り目安 | 約667円 |

外食費は自炊予算から除外します。主食は米、パスタ、そうめん、そばを使い、オートミールは使用しません。一人暮らしで続けやすい作り置きと冷凍保存を重視します。

## ディレクトリ構成

```text
.
├── AGENTS.md
├── README.md
├── config/
│   └── nutrition_targets.example.yaml
├── data/
│   ├── README.md
│   ├── records/
│   │   ├── exercise.csv
│   │   ├── expenses.csv
│   │   ├── meals.csv
│   │   └── weight.csv
│   └── templates/
│       ├── exercise.example.csv
│       ├── expenses.example.csv
│       ├── meals.example.csv
│       └── weight.example.csv
├── recipes/
│   ├── README.md
│   ├── TEMPLATE.md
│   └── examples/
│       └── chicken-mushroom-pasta.md
└── reports/
    ├── README.md
    ├── generated/
    │   └── README.md
    ├── monthly/
    │   └── TEMPLATE.md
    └── weekly/
        └── TEMPLATE.md
```

## 記録の始め方

GitHubでリポジトリの可視性が `Private` であることを確認してから、`data/records/` のCSVに実データを入力します。これらのCSVはGitの追跡対象です。初期状態はヘッダーのみで、本人の実データは入っていません。

CSVはUTF-8、日付は `YYYY-MM-DD`、小数点はピリオド、金額は円単位の整数で記録します。空欄を許す項目も列自体は削除しません。列の意味を確認したい場合は `data/templates/` の架空サンプルを参照します。

### 体重

`data/records/weight.csv` に1日1行を目安として記録します。同日に複数回測る運用へ変更する場合は、時刻列を追加する前にテンプレートと説明を同時に更新します。

### 食事と栄養

`data/records/meals.csv` に1食1行で、食事区分、料理名、カロリー、タンパク質、自炊かどうかを記録します。1日の合計値を1,800 kcal、タンパク質115〜125 gと比較します。

### 運動

`data/records/exercise.csv` に1回の運動を1行で記録します。消費カロリーは推定値として扱い、摂取目標から機械的に差し引きません。

### 食費

`data/records/expenses.csv` に支出1件を1行で記録します。`budget_scope` が `home_cooking` の行だけを30日20,000円の自炊予算に算入し、外食は `excluded` とします。

### レシピ

新しいレシピは [recipes/TEMPLATE.md](recipes/TEMPLATE.md) を複製して作成します。材料と分量、合計金額、1食あたりの金額・カロリー・タンパク質、何食分か、冷蔵・冷凍の保存期間を必ず記載します。価格と栄養値は購入商品や調理条件で変わるため、概算値にはその旨を添えます。

### 週次・月次レポート

[週次テンプレート](reports/weekly/TEMPLATE.md) と [月次テンプレート](reports/monthly/TEMPLATE.md) を使います。実データを含む完成レポートは `reports/generated/` に保存し、Gitで履歴管理します。

## commit・push前の確認

コミット前に次を確認します。

```bash
git status --short
git diff --check
git diff --cached
git remote get-url origin
gh repo view kaisei-szk/Nutrition-Management --json visibility
```

- 実データを含む場合、GitHub上の可視性が `PRIVATE` である
- 初回構築コミットには本人の実データが含まれていない
- APIキー、トークン、パスワード、秘密鍵、`.env` が含まれていない
- テンプレートの架空データには `is_sample=true` と明記されている
- push先が `kaisei-szk/Nutrition-Management` である

Privateリポジトリでも、Git履歴を共有した相手や誤設定による漏えいの可能性は残ります。認証情報はPrivateでも絶対にコミットせず、リポジトリをPublicへ戻す場合は実データを履歴ごと除去してから行います。

運用と更新時の恒常ルールは [AGENTS.md](AGENTS.md) にまとめています。
