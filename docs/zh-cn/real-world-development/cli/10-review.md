---
title: "课程 10 - 总结和后续步骤"
description: "回顾 Copilot CLI 工作流、两个拉取请求、可复用的自定义功能和更多资源。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

你使用 GitHub Copilot CLI 完成了一套连续的 Tailspin Toys 工作流。你：

- 准备了 Codespace、安装 Copilot CLI、探索项目并找到预置的筛选议题。
- 添加星级评分，在转发的浏览器中审查结果，并手动合并第一个拉取请求（PR）。
- 从筛选议题开始，在计划模式中确定方案、在自动驾驶模式中实现，并在交互模式中审查。
- 通过自定义说明引导智能体，然后自定义现有的 `quality-checks` 技能，并用它运行单元测试、lint 和类型检查。
- 添加了 Playwright Model Context Protocol (MCP) 服务器，并在真实浏览器中体验筛选功能。
- 创建并选择质量保证（QA）自定义智能体，用于评估需求、覆盖范围、技能结果和浏览器证据。
- 审查完整的筛选更改，并授权 Agent Merge 合并筛选 PR。
- 探索了用于上下文、模型、共享和可选云端委派的斜杠命令。

## 你完成的工作

本研讨会包含两个 PR 阶段，每个阶段都从更新后的 `main` 创建独立分支：

1. **星级评分：** 在游戏卡片上显示现有 `starRating` 和明确的未评分状态。
2. **筛选和质量工作流：** 实现筛选、更新说明并将其应用于功能、自定义 `quality-checks` 报告、创建 QA 配置，并添加相关测试。

从规划筛选功能到创建 PR，你都使用同一个对话和分支。为简化研讨会，我们将这些工作合并到一个 PR 中。

## 不同类型的验证

你通过多种方式检查代码：自动化测试、自己在浏览器中检查，以及通过 MCP 使用 Copilot 在浏览器中探索。`quality-checks` 技能运行了单元测试、lint 和类型检查，并按照新格式报告结果。QA 则将这些结果与需求和测试覆盖范围审查汇总在一起。

新增测试应弥补实际缺口；如果 QA 检查后发现无需添加测试，也可能是正确结果。授权合并前，请审查代码和证据；更改后应更新受影响的验证证据。

## 最佳实践

提供给 Copilot 的上下文和工具会影响其工作。在本研讨会中，你更新了说明、自定义技能、创建 QA 配置并配置 MCP 服务器。可以在不同对话中复用这些自定义内容，并根据团队需求调整。说明用于设定标准，技能描述可重复任务，自定义智能体定义专门角色，MCP 服务器连接外部工具。请检查实际更改和工具结果，而不只是智能体的摘要。

根据任务选择合适的**模式和模型**。使用 **Plan** 在构建前思考方案，使用 **Interactive** 参与聚焦更改，使用 **Autopilot** 处理范围明确的任务。日常编辑可选用更快的模型，复杂工作则选择能力更强的模型。

上下文仍然和基础设施同样重要。清楚描述希望构建*什么*、*为什么*以及*如何*构建，会显著影响输出。

## 更多探索

核心工作流已经完成。以下 CLI 功能也值得了解：

- `/review`：让代码审查智能体分析更改。
- `/rubber-duck`：讨论问题并获得另一种看法。
- `/fleet`：并行协调相互独立的子任务。
- `/worktree`：隔离独立任务。
- `/delegate`：向 Copilot 云智能体委派任务。

## 后续步骤

持续使用工具，是提升能力的最佳方式。将它用于生产代码、兴趣项目，或者多年来一直想做的小应用。与团队分享经验，也向他们学习。还可以继续探索文档。

如果要从终端扩展 Tailspin Toys，可以继续学习可选的 [Foundry Backer Concierge 系列][foundry]。要比较其他环境，可以探索 [VS Code 研讨会][vscode]、[GitHub Copilot app 研讨会][app]或 [Copilot 云智能体研讨会][cloud]。

## 资源

- [关于 GitHub Copilot CLI][about-cli]
- [Copilot CLI 命令参考][cli-reference]
- [自定义 Copilot CLI][customize-cli]
- [使用 Copilot CLI 管理拉取请求][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
