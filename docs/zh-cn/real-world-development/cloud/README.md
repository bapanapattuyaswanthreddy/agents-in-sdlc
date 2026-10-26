---
slug: zh-cn/real-world-development/cloud
title: "GitHub Copilot 云智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

**[GitHub Copilot 云智能体](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent)** 可在云端异步处理任务。你可以在 GitHub 上分配工作，云智能体会在后台探索存储库、进行更改并创建拉取请求，让你继续处理其他工作。

本系列练习将先添加云智能体会遵循的自定义说明，然后分配 GitHub 议题并让它完成任务。你还将审查并使用自定义智能体、在智能体面板中监控并引导会话，最后审查云智能体创建的拉取请求并迭代改进结果。

## 练习

| 练习 | 主题 | 说明 |
|----------|-------|-------------|
| [0. 先决条件][ex0] | 设置 | 创建存储库和 codespace |
| [1. 自定义说明][ex1] | 上下文 | 添加云智能体会遵循的自定义说明 |
| [2. 云智能体][ex2] | 异步智能体 | 将议题分配给 Copilot 云智能体 |
| [3. 自定义智能体][ex3] | 专业智能体 | 审查并使用自定义智能体 |
| [4. 管理智能体][ex4] | 监控 | 监控并引导智能体会话 |
| [5. 迭代改进][ex5] | 审查 | 审查拉取请求、迭代改进 Copilot 的工作并选择后续步骤 |

## 先决条件

参加本研讨会前，请确保已具备以下条件：

- [ ] 拥有已启用 **Copilot Student、Pro、Pro+、Business 或 Enterprise** 计划的 GitHub 账户
- [ ] 已为账户或组织启用 Copilot 云智能体

> [!TIP]
> 没有付费计划？已验证的学生可以通过 [GitHub Education][callout-student-plan-education] 免费使用 GitHub Copilot。**Copilot Student** 计划包含本系列课程使用的智能体、MCP、代码审查和 Copilot CLI 功能，因此可以完成全部练习。

[callout-student-plan-education]: https://github.com/education/students

> [!NOTE]
> Copilot 云智能体适用于 **Copilot Student、Pro、Pro+、Business 和 Enterprise**，所有计划均支持，Copilot Free 除外。对于 Copilot Business 或 Enterprise，管理员必须为组织启用该功能。

> [!NOTE]
> 本系列课程不介绍 MCP。要了解如何将 MCP 服务器与 Copilot 配合使用，请参阅 [CLI 系列课程](../cli/)或 [VS Code 系列课程](../vscode/)。

## 开始

**[从练习 0：先决条件开始 →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-cloud-agent/
[ex3]: 3-custom-agents/
[ex4]: 4-managing-agents/
[ex5]: 5-iterating/
