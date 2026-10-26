---
title: "练习 2 - GitHub Copilot 云智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义说明][previous-lesson] |
|:--|

几乎所有组织都面临技术债务，例如尚未解决的安全问题、需要更新的旧代码，或因缺少时间而长期搁置的功能请求。GitHub Copilot 云智能体专为自主执行代码更新和添加功能等任务而构建。完成工作后，它会创建草稿 PR，供开发者审查。这样可以将繁琐的工作交给智能体，加快开发流程，让开发者专注于更重要的工作。

本课将使用 Copilot 云智能体探索以下内容：

- 自定义代码生成环境。
- 确保操作安全执行。
- 明确限定议题范围的重要性。
- 将议题分配给 Copilot。

## 场景

Tailspin Toys 有一些需要处理的技术债务。最初受聘开发网站的承包商没有留下完善的文档，实际上完全缺少文档。团队希望先为应用中的所有导出函数添加 TSDoc 文档注释。

此外，设计团队准备改进游戏发现体验。他们希望游戏详情页显示相关游戏，也就是相同类别中的其他游戏，方便用户继续浏览。现阶段不需要精致的实现，只需可用于用户体验验收测试的版本。这项工作目前受阻，但还有其他优先级更高的议题。

这两类任务都很容易被降级处理，适合分配给 Copilot 云智能体。云智能体可以异步处理，让开发者先做其他工作，之后再回来审查结果。

## 了解 GitHub Copilot 云智能体

[GitHub Copilot 云智能体](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) 可以像人类开发者一样在后台执行任务。你可以通过多种方式委派工作，例如[将 GitHub 议题分配给 Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)。分配后，Copilot 会创建草稿拉取请求以跟踪进度、设置环境并开始处理任务。你可以在会话运行期间或完成后查看详情。方案准备好供审查时，Copilot 会在拉取请求中提及你。

## 明确限定任务范围的重要性

GitHub Copilot 没有魔法，使用它并不能跳过思考。即使看似简单的操作，深入分析后也可能包含复杂性，因此请谨慎限定[分配给 Copilot 云智能体的任务范围][cloud-agent-best-practices]。将其视为 AI 结对编程伙伴：分阶段工作、学习、尝试并逐步调整。生成式 AI 不会改变软件开发的基本原则。

## 本存储库中的自定义说明

前面的练习介绍过自定义说明及其对 Copilot 的引导作用。如果已经完成自定义说明练习，就见过 `.github/copilot-instructions.md` 的实际效果；否则，现在可以快速浏览一下。将任务分配给云智能体前，先只读查看存储库现有的说明文件，以便稍后观察其影响。

可在 GitHub 网页界面或正在运行的 Codespace 中打开以下文件：

- `.github/copilot-instructions.md`：查看 **Code standards** 部分，尤其是 TypeScript 约定和 TSDoc 文档注释要求。
- `.github/instructions/unit-tests.instructions.md`：注意 `applyTo` frontmatter，它将说明应用于 `**/*.test.ts` 文件。

在下一节将 `Code lacks documentation` 议题分配给云智能体时，观察生成的拉取请求是否包含 TSDoc 文档注释、TypeScript 约定和文件注释头。这些要求来自上述说明文件，稍后的课程还会再次审查。

## 为 Copilot 云智能体设置开发环境

无论由谁编写代码，通常都需要特定环境和一些设置脚本，以确保环境状态良好。将任务分配给 Copilot 也一样；它会以类似软件工程师的方式工作。

云智能体通过 [GitHub Actions][github-actions] 环境执行工作。你可以创建[专用设置工作流][setup-workflow]来自定义环境，该工作流配置在 `.github/workflows/copilot-setup-steps.yml` 中，并在智能体开始工作前运行。这样可以提供所需开发工具和依赖项。本实验已预先配置好工作流，保证练习顺利进行。它会为 Copilot 安装 Node.js、项目依赖项、端到端测试所需的浏览器二进制文件，以及迁移并填充好数据的 SQLite 数据库（供单个 Astro 应用使用）：

```yaml
name: "Copilot Setup Steps"

# 可从存储库的“Actions”选项卡测试这些设置步骤
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 权限应仅限于*这些步骤*所需的最低范围。Copilot 会获得自己的令牌来执行操作。
    permissions:
      # 如果设置步骤需要克隆存储库（例如安装依赖项），则需要 `contents: read` 权限。如果不在设置步骤中克隆存储库，步骤完成后 Copilot 会自动为你克隆。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # 前端 / 应用设置 - Node.js（整个应用现为 Astro + Drizzle/libSQL）
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # 迁移并填充本地 SQLite 数据库，为构建和测试准备数据。
      - name: Set up the database
        run: npm run db:setup
```

该文件看起来与其他 GitHub 工作流类似，但有几个关键点：

- 它只包含一个名为 `copilot-setup-steps` 的作业。Copilot 开始处理拉取请求前，此作业会在 GitHub Actions 中运行。
- `workflow_dispatch` 触发器允许你从存储库的 **Actions** 选项卡手动运行工作流。这样可以在 Copilot 使用前验证工作流，而无需等待它开始任务。

## 添加文档

大多数项目都存在信息过时或完全缺少文档的问题。这类技术债务常被搁置，降低开发效率、增加维护难度，也使新开发人员难以加入团队。Copilot 擅长生成文档，因此很适合将这类任务分配给 Copilot 云智能体。它会在后台生成所需文档，之后的练习将回来审查结果。

1. 在新的浏览器标签页中打开 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue** 创建空白议题。
5. 将 **Title** 设置为 `Code lacks documentation`。
6. 将 **Description** 设置为：

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

    ![将 Copilot 分配给议题](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。
10. 选择 **Pull Requests** 选项卡。
11. 打开新生成的拉取请求（PR），其标题可能类似于 `[WIP]: Code lacks documentation`。如果列表中尚未出现新 PR，请稍等一两分钟，然后刷新浏览器。
12. 几分钟后，Copilot 应会在 PR 中创建待办事项列表。

> [!NOTE]
> Copilot 的待办事项列表可能需要几分钟才会显示在 PR 中。Copilot 正在设置环境（运行前面介绍的工作流）、分析项目并确定最佳处理方案。

13. 审查列表及 Copilot 将执行的任务。
14. 向下滚动拉取请求时间线，查看 Copilot 已开始处理议题的更新。
15. 选择 **View session** 按钮。

    ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 可能需要刷新窗口才能看到更新后的指示器。

16. 查看实时会话，观察 Copilot 如何探索代码并了解当前状态，如何暂停思考并确定合适方案，以及如何创建代码。

这可能需要几分钟。Copilot 云智能体的一项主要优势是能异步执行任务，让我们继续处理其他工作。现在利用这一优势，再分配一个任务给云智能体，然后转而编写代码，为应用添加功能。

## 在游戏详情页添加相关游戏区域

如前所述，GitHub Copilot 云智能体的一大优势是能够拆分工作，让你和智能体同时处理不同任务。为设计团队添加相关游戏区域可能并不需要很久，但这些时间仍可用于其他工作。现在将任务分配给 Copilot 云智能体。

1. 返回 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue** 使用空白模板。
5. 将 **Title** 设置为 `Show related games on the game details page`。
6. 将 **Description** 设置为：

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

    ![将 Copilot 分配给议题](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

稍后，议题中的第一条评论会出现一组 👀，表示 Copilot 已开始处理。

![Copilot 使用眼睛表情符号表示正在处理议题](../../../_images/ex4-issue-eyes-emoji.png)

Copilot 正在处理第二项请求。Copilot 云智能体的工作方式与软件工程师类似，因此无需持续监控；完成后再审查即可。接下来开始创建和使用自定义智能体。

## 总结和后续步骤

本课介绍了 [GitHub Copilot 云智能体][copilot-agents]。你可以将议题分配给云智能体，让它异步处理技术债务、新功能，或将代码从一个框架迁移到另一个框架。

本课探索了以下概念：

- 自定义代码生成环境。
- 确保操作安全执行。
- 明确限定议题范围的重要性。
- 将议题分配给 Copilot。

云智能体继续在后台工作时，现在可以开始创建并使用自定义智能体。[Copilot 云智能体还可以使用 MCP 服务器][cloud-agent-mcp]，也可以使用前面模块中介绍的自定义说明。

## 资源

- [关于 Copilot 云智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流最佳实践][cloud-agent-best-practices]

| [下一课：自定义智能体 →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
