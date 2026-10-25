# claude-plugins

nekono-dev の Claude Code プラグインのマーケットプレイス（名前は `nekono-dev`）。プラグインの本体はそれぞれのリポジトリにあり、ここには一覧（`.claude-plugin/marketplace.json`）だけを置く。

| プラグイン | 内容 | リポジトリ |
| --- | --- | --- |
| `bootstrap-ci-builder` | ブランチ・タグごとにコミット固定の `install.sh` を CI で頒布し、`<アプリ名>-ctl` で更新・切り替え・削除する仕組みの整備 | [nekono-dev/bootstrap-ci-builder](https://github.com/nekono-dev/bootstrap-ci-builder) |
| `branch-release-workflow` | 作業ブランチ運用の強制（フック）と、プレビュー/本番の CI/CD の整備・案内 | [nekono-dev/branch-release-workflow](https://github.com/nekono-dev/branch-release-workflow) |
| `host-service-builder` | Docker で動くアプリを、bootstrap-ci-builder の上でホストのサービス（systemd・OpenRC）として動かす仕組みの整備 | [nekono-dev/host-service-builder](https://github.com/nekono-dev/host-service-builder) |
| `spec-driven-development` | 仕様駆動開発（specs の 3 点セット・architecture・PoC）の進め方 | [nekono-dev/spec-driven-development](https://github.com/nekono-dev/spec-driven-development) |

## 使い方

自分の環境に入れる:

```
/plugin marketplace add nekono-dev/claude-plugins
/plugin install spec-driven-development@nekono-dev
```

リポジトリで有効にする（`.claude/settings.json`。チーム・クラウドセッション向け）:

```json
{
  "extraKnownMarketplaces": {
    "nekono-dev": { "source": { "source": "github", "repo": "nekono-dev/claude-plugins" } }
  },
  "enabledPlugins": {
    "bootstrap-ci-builder@nekono-dev": true,
    "branch-release-workflow@nekono-dev": true,
    "host-service-builder@nekono-dev": true,
    "spec-driven-development@nekono-dev": true
  }
}
```

CI（`anthropics/claude-code-action`）:

```yaml
- uses: anthropics/claude-code-action@v1
  with:
    plugin_marketplaces: https://github.com/nekono-dev/claude-plugins.git
    plugins: |
      bootstrap-ci-builder@nekono-dev
      branch-release-workflow@nekono-dev
      host-service-builder@nekono-dev
      spec-driven-development@nekono-dev
```

更新は `claude plugin marketplace update nekono-dev` の後に `claude plugin update <プラグイン>@nekono-dev`（反映には再起動が要る）。各プラグインの導入の詳細は、それぞれのリポジトリの手順を参照する。

## プラグインを追加する

`plugins` に `{ "name", "source": { "source": "github", "repo": "nekono-dev/<リポジトリ>" }, "description" }` を足す。プラグイン側のリポジトリには `.claude-plugin/plugin.json` を置く。`claude plugin validate .` で検査する。
