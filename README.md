# Teebots plugins

Official plugin sources for [Teebots](https://teebots.ai). The desktop store reads a pinned commit of this repository for skills and for plugins that connect directly. Metered plugins still need a catalog entry on the Teebots server before they can be called.

# Teebots 插件

这是 Teebots 官方插件的仓库。第 2、3、4 类由桌面直接读这里的某一次提交。第 1 类还要在 Teebots 后台发布之后才能调用，因为价格和上游密钥不在这个仓库里。

一个插件是装进 Bot 的一包东西，里面可以有技能说明，也可以有一个 MCP 服务。技能不是商店里的另一种商品。

## 目录

清单在 `.teebots-plugin/marketplace.json`。每个插件一个目录：

```text
.teebots-plugin/marketplace.json
plugins/xhs/plugin.json
plugins/xhs/mcp.json
plugins/xhs/skills/xhs-search/SKILL.md
```

这个布局参考了常见插件仓库的写法，例如 [cursor/plugins](https://github.com/cursor/plugins)：一个目录、`skills/<名字>/SKILL.md`、一份 `mcp.json`。差异是：

- 清单和插件说明用我们自己的文件名，不用 `.cursor-plugin/`。
- 不收录那个仓库里的插件，也不使用其中的商标和图标。
- 本仓库不放密钥、上游地址和价格。

## 四类

`plugin.json` 的 `kind` 取下面四个值之一。

| kind | 含义 | 仓库里放什么 |
| --- | --- | --- |
| `metered` | 调用经过 Teebots，按次计费。用户要登录，没有积分要先充值 | 技能，以及指向 `https://api.teebots.ai/mcp/<id>` 的 `mcp.json` |
| `byo` | 用户填写自己的编号或密钥。桌面直连对方，不经过 Teebots | `mcp.json` 里用 `${变量}` 留出空位 |
| `oauth` | Teebots 已在对方平台登记应用，用户点登录。桌面直连对方 | 可以写公开的客户端编号。不写客户端密钥 |
| `skill` | 只有技能，没有 MCP | `skills/`，不要 `mcp.json` |

`metered` 的上游地址和厂商密钥在 Teebots 后台，不在这个仓库。`oauth` 若对方要求客户端密钥，密钥留在桌面程序里，不提交到这里。

## plugin.json

```json
{
  "name": "xhs",
  "displayName": "小红书",
  "version": "1.0.0",
  "description": "搜索笔记。",
  "license": "MIT",
  "kind": "metered",
  "category": "research",
  "author": { "name": "Teebots" }
}
```

`name` 是插件 id，小写字母、数字和短横线。`version` 用语义化版本。改了技能或 `mcp.json` 就要提高版本。价格不写在这里，改价格不改版本。

`license` 写 `MIT`。本仓库只收 MIT、Apache-2.0、BSD-2-Clause、BSD-3-Clause。

## marketplace.json

```json
{
  "name": "teebots-plugins",
  "owner": { "name": "Teebots" },
  "plugins": [
    {
      "name": "xhs",
      "source": "plugins/xhs",
      "description": "搜索笔记。"
    }
  ]
}
```

`source` 是仓库内的目录，不以 `/` 开头，也不含 `..`。

## mcp.json

第 1 类只写类型和地址：

```json
{
  "mcpServers": {
    "xhs": {
      "type": "http",
      "url": "https://api.teebots.ai/mcp/xhs"
    }
  }
}
```

第 2 类把要用户填写的值写成占位符，例如 `"CLIENT_SECRET": "${CLIENT_SECRET}"`。不要写成真实密钥。

第 3 类可以写 `"CLIENT_ID"` 和 `scopes`。不要写 `CLIENT_SECRET`。

## 技能

`skills/<名字>/SKILL.md` 是给模型看的用法说明。`<名字>` 是小写字母、数字和短横线，一层目录，不要再嵌套。

## 不要提交

- 客户端密钥、访问令牌、刷新令牌、API 密钥
- 第 1 类的上游地址
- 价格
- `.env` 和任何本地配置

## 贡献

欢迎提交插件。请同时改 `plugin.json`、对应文件和 `.teebots-plugin/marketplace.json`。合并由维护者完成。桌面钉住某一次提交，不会跟着分支自动更新。第 1 类合并后还要在 Teebots 后台发布才能调用。

## 许可

本仓库以 [MIT](LICENSE) 许可证发布。Copyright (c) 2026 CODEDISCO LTD.
