# GitHubCopilot-PlaywrightCli-Demo
GitHub Copilotを使用したPlaywright CLIを使った自動化デモのシナリオを集めたリポジトリ。

## Playwright CLIを使用する際の注意
- はじめにPlaywrightがインストールされているかを確認し、インストールされていない場合はnpxを使用してPlaywright CLIを実行すること。
  ```
  npx playwright-cli open --headed
  ```
- 本リポジトリはデモを目的としているためBrowserが操作されていることを聴衆に見せるために、ヘッドモードで実行すること。
  ```
  playwright open https://example.com --headed
  ```
- デモの際は聴衆に何をしているかわかるように説明しながら実行すること。
- sample-result/以下の成果物はあくまでサンプルであり、実際のデモでは変更や参照はしない。