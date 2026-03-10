# GitHub Copilot x Playwright CLI デモ

GitHub CopilotとPlaywright CLIを使用したブラウザ自動化タスクのデモ集です。

## 事前準備
Playwright CLI のインストール
```bash
npm install -g playwright-cli
```

または npx で直接実行することもできます（インストール不要）

```bash
npx playwright-cli open
```

## 実行方法

1. VS Code で本リポジトリを開く
2. Copilot Chat を開きagentモードで、`/` を入力してプロンプト一覧を表示する
3. 実行したいシナリオのプロンプトを選択する（例: `1_手順書自動生成`）
4. Copilot が playwright-cli を使って操作が自動実行され、適宜許可ダイアログが表示されるので確認して許可/却下する。
5. 成果物を確認する

> **💡 Tip:** Playwright CLI のターミナル実行を毎回手動で許可するのが手間な場合は、[.vscode/settings.json](.vscode/settings.json) 内の `"playwright-cli": true` のコメントアウトを外すと、自動許可されるようになります。

## デモシナリオ一覧

| No. | シナリオ名 | 対象者 | 概要 | 成果物サンプル |
|:---:|---|---|---|---|
| 1 | **手順書自動生成** | 業務担当者・ヘルプデスク | 手順書作成を自動化。MDN Web Docsのトップページから`Array.prototype.map()`のリファレンスページまで、階層を辿って遷移する操作手順を自動実行し、各ステップでスクリーンショットを取得する。 | [final-report.md](sample-result/1-manual-generation/final-report.md)（操作手順書）<br>[operation-log.md](sample-result/1-manual-generation/operation-log.md)（操作ログ） |
| 2 | **レスポンシブ対応手順書**（端末別） | QA・UIデザイナー | デバイス別の見え方を1回の操作で網羅。シナリオ1と同じ操作をPC幅（1280×800）とスマホ幅（375×667）の両方で実行し、デバイスごとの画面差異を記録する。 | [final-report.md](sample-result/2-responsive-manual/final-report.md)（PC版／スマホ版スクショ並列手順書） |
| 3 | **多言語差分レポート**（翻訳漏れ検知） | ローカライズ担当・PM | 多言語対応の漏れを自動検出。Wikipediaの「人工知能」記事を日本語版・英語版の両方で開き、目次から「歴史／History」セクションへ遷移しながらスクリーンショットを取得。取得した情報をもとに言語間のUI構造・用語の差異を検出し、差分レポートを生成する。 | [final-report.md](sample-result/3-multilingual-manual/final-report.md)（言語間差分レポート） |
| 4 | **重要文言の証拠化** | コンプライアンス・法務・監査 | 文言の存在証明をスクショで残せる。デジタル庁の重点計画ページを開き、「閣議決定されました」の文言が画面に表示されていることをスクリーンショットで証拠として保存する。 | [final-report.md](sample-result/4-evidence-capture/final-report.md)（操作手順書）<br>[evidence-section.md](sample-result/4-evidence-capture/evidence-section.md)（証拠ページ：文言ハイライト＋スクショ＋取得日時） |
| 5 | **Webアクセシビリティ評価** | QA・アクセシビリティ担当 | WCAG 2.1 Level AAに基づくアクセシビリティ評価を自動化。Wikipediaの「アクセシビリティ」記事を対象に、DOM情報抽出・キーボード操作テスト・視覚的確認を行い、4原則（知覚可能・操作可能・理解可能・堅牢）に沿った評価レポートを生成する。 | [final-report.md](sample-result/5-accessibility-assessment/final-report.md)（WCAG 2.1 Level AA評価レポート） |
