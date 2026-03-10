---
name: 5_Webアクセシビリティ評価
description: WikipediaのページをWCAG 2.1 Level AAの観点で網羅的に評価しアクセシビリティレポートを生成する
---

# Webアクセシビリティ評価

## 目的
日本語版Wikipediaの「アクセシビリティ」記事（`https://ja.wikipedia.org/wiki/アクセシビリティ`）を対象に、**Web Content Accessibility Guidelines (WCAG) 2.1 Level AA** の観点で網羅的なアクセシビリティ評価を実施し、評価レポートを生成する。

WCAG 2.1 の4つの原則（知覚可能・操作可能・理解可能・堅牢）に沿って、DOM情報の抽出・キーボード操作テスト・視覚的確認を行い、検出された問題と改善提案をまとめる。

## デモの注意事項
- ヘッドモードで実行し、ブラウザの操作が聴衆に見えるようにすること
- 各ステップで何をしているか説明しながら進めること
- WCAG 2.1の4原則（知覚可能 / 操作可能 / 理解可能 / 堅牢）を聴衆に紹介しながら評価を進めること
- 各チェック項目で何を確認しているか、なぜそれが重要かを補足すること

## 操作手順

### Phase 1: ページアクセスと全体確認

1. `https://ja.wikipedia.org/wiki/アクセシビリティ` にアクセスする
2. ページが完全に表示されたらスクリーンショットを取得する（ページ全体の記録）
3. `snapshot` コマンドでページのアクセシビリティツリーを取得し、ページ構造を把握する

### Phase 2: DOMベースのアクセシビリティ情報抽出

`eval` コマンドを使用して、以下の各項目をチェックする。各チェックの結果は後のレポート作成に使用するため、出力内容を記録すること。

#### 2-1. 言語設定の確認（WCAG 3.1.1 / 3.1.2）
- `eval "document.documentElement.lang"` で `lang` 属性の値を確認する
- `eval "document.querySelectorAll('[lang]').length"` で `lang` 属性が設定されている要素数を確認する

#### 2-2. 見出し構造の確認（WCAG 1.3.1 / 2.4.6）
- `eval "JSON.stringify([...document.querySelectorAll('h1,h2,h3,h4,h5,h6')].map(h => ({tag: h.tagName, text: h.textContent.trim().substring(0, 50)})))"` で見出し一覧を抽出する
- 見出しレベルの飛び（h1→h3 など）がないか確認する

#### 2-3. 画像のalt属性チェック（WCAG 1.1.1）
- `eval "JSON.stringify({total: document.images.length, withAlt: [...document.images].filter(i => i.alt).length, withoutAlt: [...document.images].filter(i => !i.alt).length, emptyAlt: [...document.images].filter(i => i.alt === '').length})"` で画像のalt属性の設定状況を確認する
- alt属性が未設定の画像がある場合、その `src` を記録する

#### 2-4. フォーム要素のラベル確認（WCAG 1.3.1 / 3.3.2）
- `eval "JSON.stringify([...document.querySelectorAll('input,select,textarea')].map(el => ({type: el.type, id: el.id, hasLabel: !!document.querySelector('label[for=\"' + el.id + '\"]'), hasAriaLabel: !!el.getAttribute('aria-label'), hasAriaLabelledBy: !!el.getAttribute('aria-labelledby')})))"` でフォーム要素とラベルの関連づけを確認する

#### 2-5. ARIAランドマークとセマンティクスの確認（WCAG 1.3.1 / 4.1.2）
- `eval "JSON.stringify({landmarks: [...document.querySelectorAll('[role]')].map(el => ({role: el.getAttribute('role'), tag: el.tagName})), semanticElements: {nav: document.querySelectorAll('nav').length, main: document.querySelectorAll('main').length, header: document.querySelectorAll('header').length, footer: document.querySelectorAll('footer').length, article: document.querySelectorAll('article').length, aside: document.querySelectorAll('aside').length}})"` でランドマークとセマンティクス要素を確認する

#### 2-6. リンクテキストの確認（WCAG 2.4.4）
- `eval "JSON.stringify({totalLinks: document.querySelectorAll('a[href]').length, emptyLinks: [...document.querySelectorAll('a[href]')].filter(a => !a.textContent.trim() && !a.querySelector('img[alt]')).length, genericLinks: [...document.querySelectorAll('a[href]')].filter(a => ['こちら','ここ','click here','here','more'].includes(a.textContent.trim().toLowerCase())).length})"` で不適切なリンクテキストを検出する

### Phase 3: キーボード操作テスト

1. ページの先頭に戻る（`goto https://ja.wikipedia.org/wiki/アクセシビリティ`）
2. `press Tab` を5〜10回繰り返し実行し、フォーカスの移動順序を確認する
3. フォーカスインジケータ（枠線やハイライト）が視認できる状態でスクリーンショットを取得する（WCAG 2.4.7）
4. スキップリンク（「本文へ移動」等のリンク）の有無を確認する（WCAG 2.4.1）
   - `eval "JSON.stringify([...document.querySelectorAll('a[href^=\"#\"]')].slice(0, 5).map(a => ({text: a.textContent.trim(), href: a.href})))"` でページ内リンクを確認する

### Phase 4: 視覚的確認

#### 4-1. 色コントラスト比の確認（WCAG 1.4.3）
- `eval` でページの主要なテキスト要素の文字色と背景色を取得する:
  ```
  eval "JSON.stringify([...document.querySelectorAll('p, h1, h2, h3, li, a, span')].slice(0, 10).map(el => {const s = getComputedStyle(el); return {tag: el.tagName, text: el.textContent.trim().substring(0, 30), color: s.color, bgColor: s.backgroundColor, fontSize: s.fontSize}}))"
  ```
- 取得した色情報からコントラスト比を算出し、Level AA の基準（通常テキスト 4.5:1、大きなテキスト 3:1）を満たしているか評価する

#### 4-2. テキスト拡大・リフローの確認（WCAG 1.4.4 / 1.4.10）
- `resize 320 800` でビューポート幅を320pxに変更する
- リフロー後のページ表示状態でスクリーンショットを取得する
- 水平スクロールバーが発生していないか確認する
- `resize 1280 800` で元のサイズに戻す

## 成果物の生成

### 作業ディレクトリ
最初に `5-accessibility-assessment/` ディレクトリを作成すること。すべての成果物はこのディレクトリ内に出力すること。
スクリーンショット画像は `5-accessibility-assessment/images/` ディレクトリ内に保存すること。

### アクセシビリティ評価レポート
評価結果を `5-accessibility-assessment/final-report.md` に生成すること。構成は以下のとおり:

#### 1. 評価概要
- **対象URL**: `https://ja.wikipedia.org/wiki/アクセシビリティ`
- **評価日時**: 評価実施日時を記載する
- **適用ガイドライン**: Web Content Accessibility Guidelines (WCAG) 2.1
- **適合レベル**: Level AA
- **評価ツール**: Playwright CLI（DOM情報抽出・キーボード操作テスト・スクリーンショット取得）

#### 2. 評価結果サマリ
- 適合 / 一部適合 / 不適合の判定を、WCAG 2.1の4原則ごとに記載する
- 全体の適合状況をアイコンで視覚的に表示する（✅ 適合 / ⚠️ 一部適合 / ❌ 不適合）

#### 3. 原則別 詳細評価

##### 3-1. 知覚可能 (Perceivable)
| チェック項目 | WCAG達成基準 | 結果 | 詳細 |
|---|---|---|---|
| 画像の代替テキスト | 1.1.1 | ✅/⚠️/❌ | alt属性の設定状況 |
| 色のコントラスト比 | 1.4.3 | ✅/⚠️/❌ | コントラスト比の計算結果 |
| テキストの拡大 | 1.4.4 | ✅/⚠️/❌ | 200%拡大時の表示状態 |
| リフロー | 1.4.10 | ✅/⚠️/❌ | 320px幅での表示状態 |

##### 3-2. 操作可能 (Operable)
| チェック項目 | WCAG達成基準 | 結果 | 詳細 |
|---|---|---|---|
| キーボード操作 | 2.1.1 | ✅/⚠️/❌ | Tab移動の可否 |
| フォーカスインジケータ | 2.4.7 | ✅/⚠️/❌ | フォーカスの視認性 |
| スキップリンク | 2.4.1 | ✅/⚠️/❌ | 本文へのスキップリンク |
| リンクの目的 | 2.4.4 | ✅/⚠️/❌ | リンクテキストの適切さ |

##### 3-3. 理解可能 (Understandable)
| チェック項目 | WCAG達成基準 | 結果 | 詳細 |
|---|---|---|---|
| ページの言語 | 3.1.1 | ✅/⚠️/❌ | lang属性の設定 |
| フォームラベル | 3.3.2 | ✅/⚠️/❌ | ラベルの関連づけ |

##### 3-4. 堅牢 (Robust)
| チェック項目 | WCAG達成基準 | 結果 | 詳細 |
|---|---|---|---|
| 見出し構造 | 1.3.1 | ✅/⚠️/❌ | h1-h6の階層構造 |
| ARIAランドマーク | 4.1.2 | ✅/⚠️/❌ | ランドマークの適切な使用 |
| セマンティクスHTML | 1.3.1 | ✅/⚠️/❌ | セマンティクス要素の使用状況 |

#### 4. 検出された問題一覧
問題ごとに以下を記載する:
- **重要度**: 高 / 中 / 低
- **WCAG達成基準**: 該当する達成基準番号と名称
- **問題の内容**: 具体的な問題の説明
- **該当箇所**: 問題が検出された要素やページ箇所
- **スクリーンショット**: 該当する場合はスクショを埋め込む
- **改善提案**: 具体的な修正方法

#### 5. 総合評価と改善提案
- 全体的な評価コメント
- 優先度の高い改善項目のまとめ
- 参考: WCAG 2.1 ガイドライン（https://www.w3.org/TR/WCAG21/）
