---
title: "课程 8 - 创建并合并功能 PR"
description: "统一审查筛选功能、说明、技能更新、QA 配置和测试，然后创建 PR 并使用 Agent Merge。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

筛选功能实现、说明更新、技能更新、质量保证（QA）配置和测试都保存在同一分支中。现在需要统一审查这些内容并创建拉取请求。之前你手动合并了星级评分拉取请求（PR），这次将使用 **Agent Merge** 管理流程。

> [!NOTE]
> 通常会将功能、说明更新、技能更新和 QA 智能体拆分为几个 PR。为简化研讨会流程，你将筛选和质量相关工作保留在同一个对话和分支中，并合并到一个 PR。

本课将学习：

- 了解 Agent Merge 及其如何自动处理合并流程。
- 检查完整功能更改和验证证据。
- 创建筛选功能 PR。
- 审查后启用 Agent Merge，并确认 PR 已合并。

## 场景

在筛选功能工作流中，你使用 Copilot 规划、实现并验证了功能。Tailspin Toys 现在希望自动处理剩余 PR 工作，同时由开发者掌握合并授权。

## 了解 Agent Merge

**Agent Merge** 会自动处理合并拉取请求所需的剩余工作。启用后，Copilot 会处理阻塞 PR 的问题，例如修复持续集成（CI）检查失败、回应审查意见以及在必要时变基，然后在存储库允许时启用 GitHub 自动合并。

此前一直由你选择 **Merge pull request**。Agent Merge 可以接手这项工作。启用前，请先审查更改并确认已准备好合并。

## 使用 Agent Merge 管理 PR

代码全部完成后，统一审查、创建 PR，并让 Agent Merge 处理后续流程。

1. 返回 Codespace。
2. 输入 `/agent` 打开智能体对话框。
3. 从选项中选择 **Default**，然后按 <kbd>Enter</kbd>。
4. 使用 `/pr create` 创建 PR。
5. 使用 `/pr agentmerge` 启用 Agent Merge。
6. Copilot 会监控 PR 的持续集成流程。所有检查成功后，它会执行合并。
7. 确认 Copilot 显示了类似“PR #14 was squash-merged successfully.”的消息。

> [!IMPORTANT]
> Agent Merge 不会绕过必需的审批、分支保护、合并队列、存储库设置或权限限制。如果流程受阻，请查看报告的原因，并在存储库允许时手动完成已审查的合并。

## 总结和后续步骤

你已自动化开发流程中的多个环节，包括生成代码、测试和验证代码，以及处理拉取请求。本课中，你：

- 了解了 Agent Merge 及其如何自动处理合并流程。
- 检查了完整功能更改和验证证据。
- 创建了筛选功能 PR。
- 仅在审查后启用 Agent Merge，并确认 PR 已合并。

接下来将[探索更多实用的 Copilot CLI 斜杠命令][next-lesson]，了解上下文、模型、共享和可选的云端委派。

## 资源

- [使用 Copilot CLI 管理拉取请求][manage-prs]
- [Copilot CLI 命令参考][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
