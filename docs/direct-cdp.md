# 让当前 Agent 完成第一个网页任务

给正在使用的 Agent 一个网址和明确目标，先读[最短接入说明](https://capybrowser.app.baizhi.cloud/docs/installation.md)，复用已有 MCP、凭据与兼容页面客户端。无需为了首次任务新建工程、配置第二套 MCP 或在本机启动 Chromium。

例如：

```text
用百智云云浏览器打开 https://example.com/，读取真实标题和说明正文。
确认页面包含 Example Domain 和 documentation examples，再用中文报告读取结果。
复用已有安全配置；结束自己创建的运行并核对资源释放与最终费用。
```

MCP／HTTP 负责管理运行，页面读取由 CDP 客户端执行。只有管理 MCP、没有页面操作能力时，先补齐客户端，不先创建计费运行。

## 缺少客户端时

在已有可写的 Node.js 20+ 工作目录中，安全配置自己的 `CAPYBROWSER_API_KEY`。已有兼容客户端时跳过安装。下面的命令取自 0.1.17 正式接入说明；以线上文档后续更新为准。

```sh
PLAYWRIGHT_SKIP_BROWSER_DOWNLOAD=1 npm install --no-save --package-lock=false --ignore-scripts playwright-core@1.64.1
curl -fsS https://capybrowser.app.baizhi.cloud/docs/first-run.mjs -o first-run.mjs
curl -fsS https://capybrowser.app.baizhi.cloud/docs/agent-guide.mjs -o agent-guide.mjs
export CAPYBROWSER_BASE_URL="https://capybrowser.app.baizhi.cloud"
# CAPYBROWSER_API_KEY 通过安全环境提供，不在命令中填写真实值。
CAPYBROWSER_EXPECT_TITLE='Example Domain' CAPYBROWSER_EXPECT_TEXT='documentation examples' node first-run.mjs 'https://example.com/'
```

不用运行 `playwright install` 或 `chromium.launch()`。如果已有 MCP 管理流程，就沿用该流程，不同时运行这个 HTTP 示例，以免为一个任务创建两次运行。

执行示例会产生正常的浏览器费用。脚本先检查身份和价格，保存幂等创建记录，再读取页面并核对预期标题及正文。只看到 Session ready、连接成功、挑战页或登录页，都不能当成读取任务完成。

脚本在正常或异常后尝试停止本次创建的运行；资源释放和最终结算分别核对。默认私有状态文件 `.capybrowser-first-run.json` 属于同一次逻辑任务，重复执行只处理原运行的恢复与收尾，不重新导航。新任务使用新的 `CAPYBROWSER_STATE_FILE` 路径；原运行与费用未核清前，不删除记录换键重开。

需要[观看、人工接管或指定线路](live-view-and-routes.md)时，按任务扩展管理流程。最小首次读取示例不包含这些操作，也不要求预先建立 Profile。

[生态集成说明](integrations.md) · [返回首页](../README.md)
