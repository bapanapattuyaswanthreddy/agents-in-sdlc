---
title: "课程 3 - 智能体模式：计划和自动驾驶"
description: "用计划模式确认方案、用自动驾驶模式根据议题构建筛选功能，再用交互模式审查结果。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

我们已经为项目添加了一项小功能，但更大的更改需要更完善的流程。GitHub Copilot CLI 可以配合组织现有的工作流程，帮助我们以正确的方式构建正确的功能。本课是若干课程中的第一课，将体验典型的智能体开发流程：从议题出发实现新功能，确保代码有效、功能符合预期，最终成功合并到项目中。

> [!NOTE]
> 在整个功能工作流中，你会继续使用同一个对话和分支。通常，不同类型的文件会分别使用不同分支或 PR，但本研讨会将简化流程，以便聚焦核心概念。

本课将学习：

- 从 GitHub 议题开始新的 Copilot 对话。
- 在计划模式中定义需求。
- 使用自动驾驶模式实现新功能。
- 审查代码。
- 在转发的浏览器中手动验证功能。

继续实现此功能时，还会更新存储库说明、自定义现有的 `quality-checks` 技能、添加 MCP 验证、创建 QA 智能体并打开功能 PR。

## 场景

Tailspin Toys 的游戏目录不断增长，访客需要按类别和发行商筛选游戏。待办事项中的议题描述了这项功能，但在编码之前，需要先确定类别组合等细节。你将使用计划模式确定这些决策，然后授权自动驾驶模式在明确的范围内实现功能。

## 背景

在开发流程中引入 AI 编码智能体，并不会改变软件开发的基本原则，反而让这些原则更加重要。大多数开发者会遵循类似以下的流程：

1. 从描述待完成工作的已创建议题开始。
2. 制定实现计划。
3. 编写并审查代码。
4. 运行测试验证代码。
5. 手动验证新功能。
6. 创建拉取请求（PR）。
7. 代码审查通过且持续集成成功后，合并代码。

> [!NOTE]
> 具体流程因团队和组织而异，但通常与上述流程类似。

遵循这种标准流程，可以确保 AI 生成的代码满足需求，并与手动编写的代码一样经过审核。

## 对话模式

**对话模式**控制智能体的自主程度。按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 可在模式间切换：

- **Interactive（交互）**：你和智能体协同工作。智能体提出更改建议，并等待你输入后再继续。
- **Plan（计划）**：智能体先制定计划，但不能编辑项目文件。
- **Autopilot（自动驾驶）**：智能体自主工作，包括编写代码、运行测试并持续迭代，直到任务完成。

先使用计划模式审查计划，然后切换到自动驾驶模式执行。

## 从议题开始

开始筛选功能前，返回 Codespace 并确认存储库和终端已准备就绪。

1. 返回 Codespace。如果它已停止，请先重启。
2. 确认星级评分 PR 已合并。
3. 如果终端尚未打开，请按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）。
4. 更新 `main`，然后创建筛选功能分支：

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. 启动 Copilot CLI：

   ```bash
   copilot --yolo
   ```

6. 按两次 <kbd>Tab</kbd> 打开 **Issues** 选项卡。
7. 按 <kbd>A</kbd> 显示全部议题。
8. 使用箭头键选中标题为 **Allow users to filter games by category and publisher** 的议题。
9. 按 <kbd>C</kbd> 将议题添加到提示中，并返回 **Session** 选项卡。

请注意，提示现在会以 `#7`（或类似编号）开头。通过 `#` 可将 GitHub 上的议题或拉取请求（PR）加入上下文。

## 规划筛选功能

规划可以先明确功能实现方案，再交由 Copilot 执行。对于复杂任务，花些时间制定计划总是值得的。现在切换到计划模式，让 Copilot 制定计划。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 切换到计划模式。确认提示下方的模式指示器显示 **Plan**。
2. 在上一步添加的议题引用后输入以下提示：

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot 会先探索项目，再确定最佳实现方案。

3. Copilot 可能会询问筛选功能应如何工作。请根据自己的偏好回答，没有唯一正确答案。
4. 计划完成后，按 <kbd>Control</kbd>+<kbd>E</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>E</kbd>（Windows/Linux）展开计划。
5. 浏览并审查计划。
6. 如果计划的任何部分不符合你的决定，请让 Copilot 修改。

## 批准自动驾驶模式

计划已编写并审查，现在可以开始实现。使用自动驾驶模式让 Copilot 执行计划。

自动驾驶模式会让 Copilot 持续迭代，直到它认为任务已经完成。

1. 选择 **Accept plan and build on autopilot (recommended)**，或已安装版本中含义相同的选项。
2. 确认提示下方的模式指示器显示 **Autopilot**。
3. 观察 Copilot 按计划迭代、生成代码并运行测试。

> [!NOTE]
> 批准后可能会立即开始实现，因此务必先审查计划。如果 Copilot 报告缺少依赖项或端口冲突，请先解决设置问题，再判断检查是否完成。

## 审查并验证实现

代码生成后，和其他代码一样，合并前必须审查。现在审查代码并运行网站，确认一切正常。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 进入交互模式。确认模式指示器不再显示 **Plan** 或 **Autopilot**。
2. 输入 `/diff`，检查筛选实现和测试。
3. 对照议题和规划时作出的决定检查结果。
4. 审查完代码后，按 <kbd>Esc</kbd> 退出差异视图。
5. 查看项目检查的输出，并让 Copilot 解决任何失败项。

## 体验新功能

代码看起来不错，但实际能运行吗？像之前一样启动应用，并通过 Codespaces 转发端口打开网站。

1. 让 Copilot 启动应用：

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces 报告端口 `4321` 可用时，选择 **Open in Browser**。
3. 测试类别筛选、发行商筛选，以及计划中确认的组合筛选。
4. 确认重置和无结果时的行为符合议题要求和你的决定。
5. 返回 Codespace，让 Copilot 停止它启动的开发服务器。

## 总结和后续步骤

你使用不同的对话模式构建并审查了一项功能。本课中，你：

- 从 GitHub 议题开始新的 Copilot 对话。
- 在计划模式中定义需求。
- 使用自动驾驶模式实现新功能。
- 审查代码。
- 在转发的浏览器中手动验证功能。

接下来将[使用自定义说明][next-lesson]深入了解代码生成方式，并确保代码遵循文档化的实践。

## 资源

- [GitHub Copilot CLI 中的自动驾驶模式][autopilot]
- [Copilot CLI 命令参考][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
