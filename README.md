# Teebots plugins

Official plugin sources for [Teebots](https://teebots.ai). The desktop store reads a pinned commit of this repository for skills and for plugins that connect directly. Metered plugins still need a catalog entry on the Teebots server before they can be called.

Plugin text in this repository is English. A plugin is one package a bot can enable. It may contain skill instructions, an MCP server, or both. A skill is a file inside a plugin, not a separate product in the store.

## Layout

The catalog is `.teebots-plugin/marketplace.json`. Each plugin is one directory:

```text
.teebots-plugin/marketplace.json
plugins/research/plugin.json
plugins/research/skills/investigate/SKILL.md
plugins/research/assets/logo.svg
```

The shape follows common plugin repositories such as [cursor/plugins](https://github.com/cursor/plugins): one directory, `skills/<name>/SKILL.md`, and an `mcp.json` when the plugin has an MCP server. Differences:

- The catalog and the plugin manifest use our filenames, not `.cursor-plugin/`.
- This repository does not vendor plugins from that repo, and it does not use their trademarks or icons.
- This repository does not contain secrets, upstream URLs, or prices.

## Four kinds

`plugin.json` sets `kind` to one of these values.

| kind | Meaning | What belongs in the repo |
| --- | --- | --- |
| `metered` | Calls go through Teebots and spend credits. The user must sign in, and must top up when the balance is empty. | Skills, plus an `mcp.json` whose URL is `https://api.teebots.ai/mcp/<id>` |
| `byo` | The user supplies their own id or key. The desktop connects directly. | Placeholders such as `${API_KEY}` in `mcp.json` |
| `oauth` | Teebots has registered an app with the provider. The user signs in. The desktop connects directly. | A public client id is fine. Do not commit a client secret. |
| `skill` | Skills only, no MCP | `skills/`, and no `mcp.json` |

Upstream URLs and provider secrets for `metered` plugins stay in the Teebots admin backend. If an `oauth` provider requires a client secret, that secret stays in the desktop app and is not committed here.

## plugin.json

```json
{
  "name": "research",
  "displayName": "Research",
  "version": "1.0.2",
  "description": "Break a question into parts, find sources, and write a conclusion that can be checked.",
  "license": "MIT",
  "kind": "skill",
  "category": "research",
  "author": { "name": "Teebots" },
  "logo": "assets/logo.svg"
}
```

`name` is the plugin id: lowercase letters, digits, and hyphens. `version` is a semantic version. Raise it when a skill, `mcp.json`, or the logo changes. Prices are not stored here, and a price change does not need a new version.

`logo` is a relative path to an svg or png in the plugin directory. Draw that file as a full square. The desktop clips it into a rounded square, about 22% of the side, and does not follow a circle.

`license` must be `MIT`, `Apache-2.0`, `BSD-2-Clause`, or `BSD-3-Clause`.

## marketplace.json

```json
{
  "name": "teebots-plugins",
  "owner": { "name": "Teebots" },
  "plugins": [
    {
      "name": "research",
      "source": "plugins/research",
      "description": "Break a question into parts, find sources, and write a conclusion that can be checked."
    }
  ]
}
```

`source` is a directory inside this repository. It does not start with `/` and it does not contain `..`.

## mcp.json

A `metered` plugin names only the transport and the Teebots URL:

```json
{
  "mcpServers": {
    "example": {
      "type": "http",
      "url": "https://api.teebots.ai/mcp/example"
    }
  }
}
```

A `byo` plugin leaves user-supplied values as placeholders, for example `"CLIENT_SECRET": "${CLIENT_SECRET}"`. Do not write a real secret.

An `oauth` plugin may include `CLIENT_ID` and `scopes`. Do not include `CLIENT_SECRET`.

## Skills

`skills/<name>/SKILL.md` is the instruction the model reads. `<name>` is lowercase letters, digits, and hyphens, one directory deep.

Write the skill in English. The model can answer the user in the user's language.

## Do not commit

- Client secrets, access tokens, refresh tokens, or API keys
- Upstream URLs for `metered` plugins
- Prices
- `.env` files or other local config

## Contributing

Plugin changes need an update to `plugin.json`, the plugin files, and `.teebots-plugin/marketplace.json`. Maintainers merge pull requests. The desktop pins one commit and does not follow the branch automatically. A `metered` plugin also has to be published in the Teebots admin backend before it can be called.

## License

This repository is released under the [MIT](LICENSE) license. Copyright (c) 2026 CODEDISCO LTD.
