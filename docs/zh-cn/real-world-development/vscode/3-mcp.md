---
title: "练习 3 - 使用 Playwright MCP 服务器测试功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：智能体模式][previous-lesson] · [下一课：自定义智能体 →][next-lesson]

你刚刚使用智能体模式构建了筛选功能。创建拉取请求前，应在浏览器中确认功能确实正常。无需自己逐项操作应用，只需连接 **Playwright MCP 服务器**，让 Copilot 在真实浏览器中测试功能，然后发布分支并创建 PR。

本练习将学习：

- 了解 Model Context Protocol (MCP) 以及 MCP 服务器如何为 Copilot 添加新工具。
- 在工作区添加 Playwright MCP 服务器。
- 让 Copilot 在浏览器中手动测试筛选功能。
- 发布分支并为筛选工作创建拉取请求。

## 什么是 Model Context Protocol (MCP)？

当智能体能够访问编辑器以外的资源时，智能体模式会更强大。Copilot 通过 Model Context Protocol (MCP) 实现这一点：它是一种让智能体与外部工具和服务通信的标准方式。

![示意图：智能体模式的内部组件，以及它与上下文、LLM 和工具（包括 MCP 服务器和 VS Code 扩展提供的工具）的交互](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) 为 AI 智能体提供了与外部工具和服务通信的方式。使用 MCP，AI 智能体可以实时获取最新信息（通过资源），并代表你执行操作（通过工具）。

AI 智能体通过 MCP 服务器访问这些工具和资源。MCP 服务器充当 AI 智能体与外部工具和服务之间的桥梁，例如现有 API 或 NPM 包等本地工具。每个 MCP 服务器都代表一组 AI 智能体可以访问的工具和资源。

以下是几个常用的 MCP 服务器：

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**：提供用于管理 GitHub 存储库的 API。AI 智能体可以创建存储库、更新现有存储库，以及管理议题和拉取请求。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**：通过 Playwright 提供浏览器自动化能力。AI 智能体可以浏览网页、填写表单并选择按钮。

还有许多其他 MCP 服务器可以访问不同工具和资源。GitHub 提供了 [MCP 注册表](https://github.com/mcp)，帮助发现并参与生态系统。

> [!CAUTION]
> 请像对待项目的其他依赖项一样对待 MCP 服务器。使用前应仔细审查其源代码、验证发布者并考虑安全影响。只使用可信的 MCP 服务器，并谨慎授予其访问敏感资源或执行操作的权限。

## 查看 MCP 配置

`.vscode/mcp.json` 文件用于配置此 VS Code 工作区可用的 MCP 服务器。

1. 在 Codespace 中打开 `.vscode/mcp.json`。
2. 你会看到已配置好的 `github` 服务器：

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

该 `github` 条目来自项目模板，因此 Copilot 在上一练习中能够读取议题待办事项。它使用[远程 GitHub MCP 服务器][remote-github-mcp-server]，无需本地安装；VS Code 通过 HTTP 连接，Copilot 首次调用其工具时需要使用 GitHub 进行身份验证。

## 添加 Playwright MCP 服务器

现在添加第二个服务器。[Playwright MCP 服务器][playwright-mcp-server]为 Copilot 提供可控制的浏览器，正好适用于测试功能。

1. 在 `.vscode/mcp.json` 中，在 `github` 旁边添加 `playwright` 条目，使文件内容如下：

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. 保存文件。

与远程 `github` 服务器不同，`playwright` 是**本地**服务器：VS Code 会在你的计算机上运行 `npx @playwright/mcp@latest` 来启动它。`--headless` 标志让 Playwright 在没有可见窗口的情况下运行浏览器，这对于没有桌面环境的 Codespace 是必需的。

> [!NOTE]
> Tailspin Toys 项目已在端到端测试中使用 Playwright，因此通常已安装 Playwright 所需的浏览器。如果 Copilot 随后报告缺少浏览器，请让它运行 `npx playwright install chromium`，然后重试。

## 启动并信任服务器

VS Code 会**按需**启动 MCP 服务器：Copilot 首次需要服务器工具时，VS Code 会启动服务器并询问你是否信任它。

1. 在 `.vscode/mcp.json` 中，`playwright` 条目上方会出现 **Start** 操作。选择它以立即启动服务器并确认配置正确。
2. 如果 VS Code 询问是否信任服务器，请审查配置并选择信任，以便启动服务器。

手动启动是可选步骤。如果跳过，VS Code 会在下一步中 Copilot 首次需要该服务器工具时启动服务器，并提示你进行信任确认。

## 测试筛选功能

[Playwright MCP 服务器][playwright-mcp-server]为 Copilot 提供可操作的真实浏览器。智能体可以打开页面、浏览网站、应用筛选并读取结果，而无需你亲自操作，然后总结观察结果。这样无需离开对话就能快速确认功能是否符合预期。

在底层，Playwright MCP 服务器使用页面的[无障碍树][playwright-mcp-server]，而不是截图。因此，智能体基于结构化且带标签的元素（按钮、链接、列表项）进行推理，就像辅助技术一样。这样，快速的功能检查也能顺便验证基本的无障碍情况。

连接服务器并运行应用后，让 Copilot 测试刚构建的筛选功能：

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot 会通过 Playwright MCP 服务器启动浏览器，逐步执行测试并反馈结果。对照议题中的验收标准审查报告；如果有异常，可以继续提问，或让 Copilot 修复代码后再创建拉取请求。

> [!NOTE]
> 测试前，应用必须运行在 `http://localhost:4321`。如果开发服务器已停止，请在发送提示前重新启动。Copilot 首次使用 Playwright MCP 服务器时可能需要下载浏览器；如果报告缺少浏览器，请让它运行 `npx playwright install chromium` 并重试。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## 发布分支并创建拉取请求

确认功能正常后，就可以创建拉取请求（PR）供团队审查了。首先发布 `filtering-vscode` 分支。

1. 在 Codespace 中打开 **Source Control** 面板，审查 Copilot 所做的更改。
2. 选择 **+** 图标暂存更改。
3. 使用 **Sparkle** 按钮生成提交消息。

   ![显示 Copilot 更改的 Source Control 面板截图](../../../_images/ex3-source-control-changes.png)

4. 选择 **Publish**，将分支推送到存储库。

## 创建拉取请求

可以通过 github.com 和 GitHub 命令行界面（CLI）等多种方式创建拉取请求。既然已经在使用 GitHub Copilot，就让它来创建 PR 吧。它可以查找相关议题并创建与议题关联的 PR。

1. 打开 Copilot Chat 面板，选择 **New Chat** 开始新会话。
2. 确认智能体下拉列表中选中了 **Agent**，以便 Copilot 使用 GitHub 工具。
3. 让 Copilot 创建 PR：

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 根据需要选择 **Continue**，允许 Copilot 收集信息并执行操作。首次使用 GitHub 工具时，系统可能会提示你登录 GitHub；按照提示授权即可。
5. 注意 Copilot 如何搜索议题、找到正确议题并创建 PR。
6. 选择 Copilot 提供的链接，审查拉取请求，但**暂时不要合并**。

## 总结和后续步骤

恭喜！本练习中，你：

- 了解了 Model Context Protocol (MCP) 以及 MCP 服务器如何为 Copilot 添加新工具。
- 在工作区添加了 Playwright MCP 服务器。
- 在发布前使用它在浏览器中手动测试筛选功能。
- 发布分支并为筛选工作创建了拉取请求。

你使用 MCP 服务器测试了功能，但 MCP 只是扩展 Copilot 的一种方式。接下来将[创建自定义智能体][next-lesson]，简化无障碍审查等聚焦任务。

## 资源

- [什么是 MCP，为什么大家都在讨论它？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP 注册表][mcp-registry]
- [VS Code 中的 MCP 服务器][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
