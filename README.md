# Appflare 自建应用目录

这是与 [Appflare 管理面板](https://github.com/yys9253462-gif/appflare)（部署站：[cf.hejige.com](https://cf.hejige.com/)）配套的自建应用目录。应用清单保存在本仓库，构建产物发布到本仓库的 GitHub Releases，并使用本目录的自有密钥签名。

本仓库共收录了 140 份应用清单，涵盖网站分析、短链接、文件、邮件、AI 与开发工具等类别；其中 **139 个可构建应用**已完成自动构建、Ed25519 签名（密钥 ID：`yys9253462-gif-2026-10`）并发布到本仓库 Releases 与目录索引中（个别上游源码已失效的清单未纳入发布索引）。

- [已发布的目录索引](https://yys9253462-gif.github.io/appflare-catalog/index.json)
- [构建和发布记录](https://github.com/yys9253462-gif/appflare-catalog/actions/workflows/publish.yml)
- [已发布的应用包](https://github.com/yys9253462-gif/appflare-catalog/releases)
- [应用清单](apps/)

## 添加到管理器

在已安装的 Appflare 管理器中（可通过 [cf.hejige.com/deploy](https://cf.hejige.com/deploy/) 一键部署），打开 **设置 → 应用目录 → 添加目录**，填写索引地址和公钥：

**索引地址**

```text
https://yys9253462-gif.github.io/appflare-catalog/index.json
```

**公钥**

```json
{"keyId":"yys9253462-gif-2026-10","publicKeyBase64":"/VioFbEWodpbJifio6zNSyKUzBnLLuwkC7Tetjxlizw="}
```

添加时，对话框会显示密钥指纹。保存前，请与以下指纹核对：

```text
SHA256:bdzib5njKeZ4hoh+Kief68VK/nGdFjfLhG7/xZNZIsM
```

选择**校验并添加**后，管理器会读取索引，并使用所填公钥验证其中一个已发布应用。添加成功后，可在应用中心浏览并安装本目录中全部已发布的应用。只有管理员可以添加或修改目录；如需仅展示本目录中的应用，可在列表开关中关闭其他目录来源（关闭不会卸载已经安装的应用）。

## 应用如何安装和运行

1. 本仓库每个 `apps/<slug>/appflare.jsonc` 指明应用的上游 GitHub 仓库和固定提交。
2. 发布工作流从该提交构建应用，用本目录的密钥签名，并发布到本仓库的 GitHub Releases。
3. 管理器读取目录索引，从 Releases 下载并校验应用包，再安装到你的 Cloudflare 账户。
4. 安装后的应用运行在你的 Cloudflare 账户中。所需的 Worker、数据库、存储及其他资源取决于应用自身的配置。

应用的构建依赖其上游源码和构建时所需的依赖；已发布的应用包由本仓库提供。不同应用可能需要 Cloudflare 付费套餐、域名、R2 或其他服务，安装前请查看管理器显示的要求。

## 添加或更新应用

应用清单位于 `apps/<slug>/`，目录名必须与清单中的 `slug` 一致。新增或更新时，请检查上游仓库、固定的完整提交 SHA、许可证、构建命令和资源要求；然后运行本地校验，并根据修改更新维护者文件。相关格式和流程可参考 [CONTRIBUTING.md](CONTRIBUTING.md) 和 [TEMPLATE.md](TEMPLATE.md)。

推送符合发布条件的变更后，发布工作流会处理尚未发布的应用版本、更新索引并部署目录页面。若构建或发布失败，请先查看对应的 [Actions 记录](https://github.com/yys9253462-gif/appflare-catalog/actions/workflows/publish.yml)；不要将清单数量当作已发布或已验证的应用数量。

## 仓库结构

| 路径 | 内容 |
| --- | --- |
| [`apps/`](apps/) | 应用清单、图片及打包说明 |
| [`index.json`](index.json) | 工作流生成的目录索引 |
| [`schema/`](schema/) | 清单格式定义 |
| [`scripts/`](scripts/) | 校验、打包和维护工具 |
| [`.github/workflows/`](.github/workflows/) | 构建、签名和发布工作流 |

## 许可证

本目录的工具遵循 [Apache-2.0](LICENSE)。各应用及其素材分别遵循各自上游的许可；收录在目录中不会改变它们的许可条件。

Appflare 是独立开源项目，与 Cloudflare, Inc. 没有隶属、背书或赞助关系。
