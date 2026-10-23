<h1 align="center">百智云云浏览器</h1>

<p align="center">Baizhi Cloud Browser · 为 Agent 提供可复用的云端浏览器，支持网页操作、登录状态保存与人工介入。</p>

<p align="center">
  <a href="https://capybrowser.app.baizhi.cloud/">服务控制台</a> ·
  <a href="https://capybrowser.app.baizhi.cloud/docs/installation.md">接入指南</a> ·
  <a href="#客户端接入">客户端集成</a> ·
  <a href="https://github.com/chaitin/baizhi-cloud-browser/issues">问题反馈</a>
</p>

## 项目介绍

百智云云浏览器让 Agent 按需启动独立的云端浏览器，继续使用 Playwright、Puppeteer 等熟悉的工具访问和操作网页，无需在本机启动对应的 Chromium。

Agent 操作时，你可以打开它发来的观看链接看进展，无需登录管理后台，也不抢占它的控制。需要本人登录或确认时，再接管当前页面，完成后交还；有效且允许接管的链接还支持在同一窗口再次接管。

按接入指南保存登录后的浏览器状态，下次运行可继续复用；网站是否继续认可登录状态，取决于其自身规则。网页中下载或导出的文件，也可以通过接口取回。需要更换网络出口时，可让 Agent 在创建浏览器运行时选择平台提供的指定线路，无需自行配置。

本仓库由 **`ctcbyzx` 维护**，整理 agent-browser 插件、browser-use 示例与 DeerFlow 配置说明，**不包含托管服务的后端源码**。在线服务的账号权限、使用规则和计费以[控制台](https://capybrowser.app.baizhi.cloud/)及[服务文档](https://capybrowser.app.baizhi.cloud/docs/installation.md)为准。

## 快速开始

把下面的提示词交给你正在使用的 AI 编程助手或 Agent，并补充要访问的网址和任务：

```text
请先阅读百智云云浏览器最短接入说明：
https://capybrowser.app.baizhi.cloud/docs/installation.md

沿用已有 MCP 和安全凭据配置，为我给出的网址与任务读取真实网页内容。
缺少页面客户端时按正式说明准备最小 playwright-core，不下载本地浏览器；
先确认执行能力、身份和价格，再为本任务创建一次限时运行。
我想看进展时，取得真实观看链接后发给我；需要本人操作时，
由我在页面确认接管，完成并交还后把页面提示发回原对话，再继续任务。
取回需要的文件，结束自己创建的运行，分别核对释放、保存与最终费用。
```

在 Agent 的安全配置中提供自己的 API Key；平台个人中心 Key 和云浏览器应用内 Key 均以 Bearer 方式调用。已有可用配置时直接复用，不要把真实 Key 粘贴进上述提示词、代码或公开反馈。

**MCP／HTTP API 管理浏览器运行；网页导航、点击和读取由 CDP 客户端执行。** 只接通 MCP 尚不代表 Agent 已具备页面操作能力，接入指南会引导它检查现有环境。

## 看进展，需要时亲自操作

- 对 Agent 说“让我看看当前页面”，打开它返回的只读观看链接即可；观看不会抢占 Agent 控制。
- 需要本人操作时，明确提出并在页面确认接管。完成后点击“完成并交还 Agent”，把页面回复发回原对话；Agent 接回同一次运行、检查页面后继续。
- 链接仍有效且允许接管时，可在同一窗口再次接管，每轮都需要确认。双方轮流操作，不是同时输入。

也可以告诉 Agent“这次用指定线路”，让它先查当前可用目录，在创建运行时选择；只有明确要求长期使用时才保存偏好。未指定时沿用已有偏好或默认出口，原有普通调用不变。详见[观看、交接与网络线路](docs/live-view-and-routes.md)。

这些是当前服务的 HTTP／MCP 能力。下表中的 agent-browser v0.1.3 插件继续使用其既有命令，不表示该版本已原生新增观看或线路选择命令。

## 客户端接入

| 入口 | 本仓库提供的能力 | 状态与限制 |
| --- | --- | --- |
| [直接接入与首次任务](docs/direct-cdp.md) | 最小 Playwright 客户端或已有兼容客户端；无需下载本地浏览器 | HTTP／MCP 管理运行，CDP 客户端操作页面 |
| [agent-browser](integrations/agent-browser/README.md) | 可安装的独立插件；管理云端运行，沿用 agent-browser 网页命令 | [v0.1.3 已发布](https://github.com/ctcbyzx/baizhi-cloud-browser/releases/tag/v0.1.3)；对应 Node.js 24+、agent-browser 0.38.2 |
| [browser-use](docs/browser-use.md) | 网页读取示例、环境准备与异常恢复指南 | [PR #6100](https://github.com/browser-use/browser-use/pull/6100) 正式待审、尚未合并；示例可从维护者分支获取 |
| [DeerFlow](docs/deerflow.md) | HTTP MCP 配置指南与插件目录入口说明 | [PR #6976](https://github.com/bytedance/deer-flow/pull/6976) 正式待审、尚未合并；可先手动配置 HTTP MCP |
| [Cline](docs/integrations.md#cline-与-kilo) | 远程 MCP 市场条目与手动配置入口 | [PR #308](https://github.com/cline/marketplace/pull/308) 正式待审、尚未合并 |
| [Kilo](docs/integrations.md#cline-与-kilo) | 远程 MCP 市场条目与 API Key 配置入口 | [PR #454](https://github.com/Kilo-Org/kilo-marketplace/pull/454) 正式待审、尚未合并 |

接入状态核对于 2026-10-23，最新状态以发布页或 PR 为准。agent-browser 插件的 v0.1.3 安装包保留在维护者原仓库的 GitHub Release 中；browser-use 和 DeerFlow 的待审内容在各自上游仓库维护。更多说明见[生态集成说明](docs/integrations.md)。

## 连接信息

| 配置项 | 值 |
| --- | --- |
| 服务控制台 | `https://capybrowser.app.baizhi.cloud` |
| HTTP API 根路径 | `https://capybrowser.app.baizhi.cloud/openapi/v1` |
| MCP 地址 | `https://capybrowser.app.baizhi.cloud/mcp` |
| MCP 传输协议 | Streamable HTTP |
| 鉴权方式 | `Authorization: Bearer <API Key>` |
| 本仓库插件使用的环境变量 | `CAPYBROWSER_BASE_URL`、`CAPYBROWSER_API_KEY` |
| 页面操作连接 | 为已就绪的运行申请一次性 CDP 控制地址 |

完整参数与返回结构见 [HTTP OpenAPI](https://capybrowser.app.baizhi.cloud/docs/browser-session-openapi.yaml)，Agent 执行流程见 [Skill 文档](https://capybrowser.app.baizhi.cloud/docs/skills.md)。

## 运行与费用

指定线路在当前体验期免线路附加费，浏览器启动及运行仍按现行标准计费。具体可用线路与价格由 Agent 查询当前服务确认；不承诺固定或独立 IP，也不保证所有网站允许访问。

创建浏览器运行前，请先在控制台或通过 [`GET /openapi/v1/pricing`](https://capybrowser.app.baizhi.cloud/openapi/v1/pricing) 查询当前价格；接口需要 API Key。查询版本、验证身份和查询价格本身不启动浏览器。

等待 Agent、人工处理和连接断开期间仍计入运行时长。任务结束后主动停止自己创建的运行；**断开 CDP 连接不代表云端运行已结束**。停止受理或到期后停止运行计费，资源释放与账单结算分别核对，实际费用以账单为准。

## 贡献与支持

欢迎完善客户端适配、接入示例和使用文档。贡献方式见[贡献指南](CONTRIBUTING.md)，插件开发与验证命令见[插件说明](integrations/agent-browser/README.md)。

- 接入或插件问题：提交 [Issue](https://github.com/chaitin/baizhi-cloud-browser/issues)，附客户端版本、复现步骤与脱敏错误。
- 账号、计费和私密问题：通过服务控制台提供的支持渠道反馈。
- 安全问题与凭据泄漏：按[安全说明](SECURITY.md)处理，不在公开 Issue 中贴出敏感信息。

agent-browser 插件采用其目录中的 [MIT License](integrations/agent-browser/LICENSE)。在线服务的使用权限与计费规则独立于插件许可证。
