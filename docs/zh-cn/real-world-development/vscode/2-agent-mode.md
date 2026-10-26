---
title: "练习 2 - 使用 Copilot 智能体模式添加新功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

即使是最简单的应用更新，通常也需要修改多个文件并执行一些操作，例如运行测试。开发者的工作流程通常包括查找所有相关文件、进行更改、运行测试、调试、找出遗漏的文件并再次更新……这样的工作还会不断重复。

这正是 Copilot 智能体模式的用武之地。

Copilot 智能体模式可以在 IDE 中更自主地工作。它的行为类似开发者：先探索现有项目结构，再执行所需更新、运行测试等任务，并自动修复发现的问题。接下来将了解如何使用智能体模式为网站添加新功能。

> [!NOTE]
> 智能体模式和云智能体名称相似，但提供的是两种不同体验。智能体模式在 IDE 中执行任务，支持快速反馈和交互。云智能体则像团队成员一样异步工作，通过议题和拉取请求与你协作。

本练习将学习：

- Copilot 智能体模式如何探索项目、识别相关文件并协调多文件更改。
- GitHub Copilot 智能体模式如何跨 UI 和数据层实现新功能。
- 合并代码前如何审查 Copilot 智能体模式生成的更改和测试。

## 场景

游戏列表不断增长，因此你希望用户可以按类别和发行商筛选。上一练习中已经添加发行商辅助函数，现在使用 Copilot 智能体模式完成剩余的数据层、UI 和测试工作。

## 运行 Tailspin Toys 网站

先探索 Tailspin Toys 网站，了解当前功能，再进行更改。

该网站是一个带有开发者主题的棋盘游戏众筹平台。用户可以浏览游戏列表并查看游戏详情。它是一个单独的 Astro 应用，在构建时将页面渲染为静态 HTML。页面通过 Drizzle ORM 在 frontmatter 中直接查询本地 SQLite 数据库，没有单独的后端 API 或客户端 UI 框架。可复用的数据访问辅助函数位于 `src/lib/`，交互功能使用标准 DOM API 编写的小型作用域 Astro `<script>` 实现。

### 启动网站

项目提供了 `npm run dev` 任务，用于启动单个 Astro 开发服务器。在 GitHub Codespace 中按以下步骤运行：

1. 返回 Codespace。上一练习结束后应已回到 `main`。
2. 按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux），在 Codespace 中打开新终端。
3. 创建并切换到筛选功能分支：

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 运行以下命令启动网站：

   ```bash
   npm run dev
   ```

   Astro 开发服务器就绪后，会显示类似以下 URL 的提示：

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> 如果出现询问是否打开 `http://localhost:4321` 浏览器窗口的对话框，请选择 **x** 将其关闭。

5. 在终端中按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），选择 Astro 服务器地址 `http://localhost:4321`，打开网站。

> [!NOTE]
> 使用 Codespace 时，从终端选择 localhost URL 会自动重定向到 `https://<your-codespace-name>-4321.app.github.dev/`。这是通向 Codespace 的私有隧道，网站服务器现在正在其中运行。

### 浏览网站

网站运行后，可以体验其功能。主要功能包括：

- **主页**：显示棋盘游戏列表，包括标题、图片和说明。
- **游戏详情页**：选择游戏后会打开详情页，显示游戏标题、说明、发行商和类别等信息。

## 使用 Copilot 浏览待办事项

> [!TIP]
> **打开 Copilot Chat**
>
> 开始下面的练习前，返回 Codespace 并打开 Copilot Chat 面板，选择 **New Chat** 开始全新对话。每项练习会说明所需模式和模型。

网站的初始实现已可正常使用，但还可以添加新功能。先查看待办事项。根据模板创建项目时，系统已自动创建一组议题，可以让 GitHub Copilot 帮你查看。

1. 在 Chat 视图的智能体下拉列表中选择 **Agent**。**Agent** 智能体可以自主规划并跨文件实现更改、运行终端命令和调用工具。

   ![Chat 视图中的智能体选择器截图](../../../_images/shared-chat-mode-selector.png)

2. 从可用模型列表中选择 **Claude Sonnet 4.5**。

> [!CAUTION]
> 本研讨会作者并非表示偏好某个模型。编写本研讨会时使用了 Claude Sonnet 4.5，因此在说明中列出该模型，希望代码建议相对一致，以便学习体验更好。由于大语言模型具有概率性，你收到的建议可能与示例不同，这是正常且预期的情况。

> [!NOTE]
> 由于大语言模型具有概率性，Copilot 可能会使用不同的 MCP 命令，但仍应能完成任务。

3. 向 Copilot 发送以下提示，询问议题待办事项：

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. 选择 **Continue**，运行列出所有议题的命令。
5. 查看生成的议题列表。

Copilot 还根据它认为对用户最有用的程度为事项排了优先级。

## 审查说明文件

让智能体生成代码前，先查看用于提供工作上下文的说明文件。我们将使用[用户界面 (UI) 说明文件](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)，其中包含如何为网站添加功能的指引。

1. 在 Codespace 中打开 `.github/instructions/ui.instructions.md`。
2. 查看如何添加功能的总体指引，包括：
   - 架构概述。
   - 组件设计、可测试性和无障碍访问原则。
   - 指向各种文件类型说明的链接，包括 Astro、Tailwind CSS 和 Drizzle。

> [!TIP]
> 说明文件可以引用其他说明文件和项目文件。路径相对于说明文件所在位置，因此可复用指引、拆分复杂说明并引用示例和模板。

## 实现筛选功能

完成筛选功能至少需要对应用进行三项更新：

- 添加或完善数据层中的筛选逻辑（`src/lib/`）。
- 添加或更新筛选行为测试。
- 更新游戏列表页，加入筛选 UI。

此外，合并前必须运行并通过测试。Copilot 智能体模式可以完成这些任务，下面开始添加功能。

1. 可以继续使用当前 Copilot 对话，也可以选择 **New Chat** 新建对话。
2. 选择 **Add Context**、**Instructions** 和 **ui** 说明文件。

   ![选择 UI 说明文件的示例截图](../../../_images/ex3-select-instructions-file.png)

3. 确认 Chat 视图的智能体下拉列表中仍选中 **Agent**。

   ![Chat 视图中的智能体选择器截图](../../../_images/shared-chat-mode-selector.png)

4. 确认仍选中 **Claude Sonnet 4.5** 模型。
5. 使用以下提示，让 Copilot 根据待办事项中的相关议题实现功能：

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. 观察 Copilot 探索项目并定位相关文件。它应找到数据层辅助函数、UI 和测试，然后修改文件并运行测试。

   ![Copilot 探索项目文件的截图](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot 会执行多个步骤，例如探索项目、修改文件和运行测试。根据任务和代码库的复杂程度，可能需要几分钟。过程中，代码编辑器可能会显示 **Keep** 和 **Undo** 按钮。Copilot 完成后，每项更改都会显示 **Keep** 或 **Undo**，因此无需在工作进行中选择。

7. Copilot 提示运行测试时，选择 **Continue**。

   ![Copilot Chat 面板要求确认运行测试的对话框截图](../../../_images/ex3-agent-mode-run-tests.png)

8. 过程中可能会暂停，甚至看到测试失败，这没有关系。Copilot 会在生成代码和运行测试之间来回迭代，直到完成任务且未检测到错误。

   ![完整 Copilot 智能体模式会话截图](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 检查生成的代码，查找潜在问题。

> [!CAUTION]
> 务必审查 Copilot 或其他 AI 工具生成的代码。

10. 返回正在运行网站的浏览器，体验新功能。
11. 确认功能正常并审查代码后，在 Copilot Chat 窗口中选择 **Keep**。

## 总结和后续步骤

恭喜！本练习中，我们使用 GitHub Copilot 智能体模式为 Tailspin Toys 网站添加了新功能。你学习了：

- GitHub Copilot 智能体模式如何跨 UI 和数据层实现新功能。
- Copilot 智能体模式如何探索项目、识别相关文件并协调多文件更改。
- 合并代码前如何审查 Copilot 智能体模式生成的更改和测试。

接下来使用 [Playwright MCP 服务器测试功能][next-lesson]，并为其创建拉取请求。

### 拓展探索练习 - 实现分页

随着游戏列表增长，需要支持分页。运用本练习学到的技能，提示 Copilot 更新网站并实现分页。编写提示时可以考虑：

- 遵循现有最佳实践，包括使用现有说明文件。
- 思考分页方式：允许用户选择每页大小，还是固定每页大小。
- 提供必要指引，让 Copilot 按预期实现功能。
- 可能需要迭代与 GitHub Copilot 协作，提出更改并补充上下文。这是使用 Copilot 时的正常流程。

## 资源

- [Copilot 的询问、编辑和智能体模式：功能及适用场景][choose-mode]
- [VS Code 中的智能体模式][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
