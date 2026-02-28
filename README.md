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
2. Copilot Chat を開き、`/` を入力してプロンプト一覧を表示する
3. 実行したいシナリオのプロンプトを選択する（例: `1_手順書自動生成`）
4. Copilot が playwright-cli を使って操作が自動実行され、適宜許可ダイアログが表示されるので確認して許可/却下する。
5. 成果物を確認する

> **💡 Tip:** Playwright CLI のターミナル実行を毎回手動で許可するのが手間な場合は、[.vscode/settings.json](.vscode/settings.json) 内の `"playwright-cli": true` のコメントアウトを外すと、自動許可されるようになります。

## デモシナリオ一覧

| No. | カテゴリ | シナリオ名 | 対象者・訴求ポイント | 概要（何をするか・目的） | 成果物サンプル | 使用サイト |
|:---:|:---:|---|---|---|---|---|
| 1 | 手順書生成 | **手順書自動生成** | **業務担当者・ヘルプデスク**：手作業の手順書作成を自動化できる | MDN Web Docsのトップページから`Array.prototype.map()`のリファレンスページまで、階層を辿って遷移する操作手順を自動実行し、各ステップでスクリーンショットを取得する。 | [final-report.md](sample-result/1-manual-generation/final-report.md)（操作手順書）<br>[operation-log.md](sample-result/1-manual-generation/operation-log.md)（操作ログ） | [MDN Web Docs](https://developer.mozilla.org/ja/docs/Web/JavaScript) |
| 2 | 環境チェック | **レスポンシブ対応手順書**（端末別） | **QA・UIデザイナー**：デバイス別の見え方を1回の操作で網羅できる | シナリオ1と同じ操作をPC幅（1280×800）とスマホ幅（375×667）の両方で実行し、デバイスごとの画面差異を記録する。 | [final-report.md](sample-result/2-responsive-manual/final-report.md)（PC版／スマホ版スクショ並列手順書） | [MDN Web Docs](https://developer.mozilla.org/ja/docs/Web/JavaScript) |
| 3 | 多言語 | **多言語差分レポート**（翻訳漏れ検知） | **ローカライズ担当・PM**：多言語対応の漏れを自動検出できる | Wikipediaの「人工知能」記事を日本語版・英語版の両方で開き、目次から「歴史／History」セクションへ遷移しながらスクリーンショットを取得。取得した情報をもとに言語間のUI構造・用語の差異を検出し、差分レポートを生成する。 | [final-report.md](sample-result/3-multilingual-manual/final-report.md)（言語間差分レポート） | [Wikipedia 日本語版](https://ja.wikipedia.org/wiki/人工知能)<br>[Wikipedia 英語版](https://en.wikipedia.org/wiki/Artificial_intelligence) |
| 4 | 証拠化 | **重要文言の証拠化** | **コンプライアンス・法務・監査**：文言の存在証明をスクショで残せる | デジタル庁の重点計画ページを開き、「閣議決定されました」の文言が画面に表示されていることをスクリーンショットで証拠として保存する。 | [final-report.md](sample-result/4-evidence-capture/final-report.md)（操作手順書）<br>[evidence-section.md](sample-result/4-evidence-capture/evidence-section.md)（証拠ページ：文言ハイライト＋スクショ＋取得日時） | [デジタル庁 重点計画](https://www.digital.go.jp/policies/priority-policy-program) |