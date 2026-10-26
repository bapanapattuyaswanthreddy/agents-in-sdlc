---
title: "课程 9 - GitHub Copilot CLI 中的斜杠命令"
description: "探索用于管理上下文、选择模型、共享会话和可选委派到云端的斜杠命令。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

和其他优秀的 CLI 工具一样，GitHub Copilot CLI 包含许多用于交互的斜杠命令。这些命令提供高级功能、幕后信息和其他配置选项。你已经使用过 `/diff`、`/mcp`、`/skills`、`/agent` 和 `/pr` 等命令。现在再来看看其他实用命令。

本课将学习：

- 使用 `/context` 和 `/compact` 了解 Copilot 如何管理对话上下文。
- 使用 `/model` 查看可用模型。
- 了解如何使用 `/share` 导出或共享会话。
- 探索并行工作、工作树和委派给云智能体的可选命令。

## 场景

核心 CLI 工作流已完成。现在来了解其他功能：管理上下文、切换模型、共享会话，以及将任务可选地委派给 [Copilot 云智能体][about-cloud-agent]。

## 探索 Copilot CLI 上下文

处理较大或较复杂的任务时，可能会达到模型的最大上下文窗口。Copilot CLI 会在需要时自动压缩对话；你也可以使用斜杠命令自行检查或压缩。

1. 返回 Codespace。如果 Copilot CLI 尚未打开，请从存储库根目录启动。
2. 输入：

   ```plaintext
   /context
   ```

3. 查看模型、当前令牌用量，以及上下文在系统说明、工具、消息和可用空间之间的分配情况。
4. 压缩对话：

   ```plaintext
   /compact
   ```

5. 再次输入 `/context` 并比较结果。如果对话本来就很短，变化可能不明显。

> [!NOTE]
> 上下文窗口接近满载时，Copilot CLI 会自动压缩上下文。想自行控制时机，可以使用 `/compact`。如果要切换到无关任务并开始全新对话，则使用 `/clear` 或 `/new`。

## 选择模型

不同模型各有所长，不同开发者也有各自偏好。Copilot CLI 可列出并选择要使用的模型。

1. 输入：

   ```plaintext
   /model
   ```

2. 浏览可用模型和用量信息。
3. 保留当前模型、选择其他模型，或按 <kbd>Esc</kbd> 关闭列表。

## 共享会话

团队协作和分享经验有助于大家更好地使用 AI 工具。`/share` 命令可将会话导出为 Markdown 或 HTML 文件、创建共享链接，或发布为 GitHub gist。

1. 输入 `/help`，查看已安装版本中的 `/share` 选项。
2. 如果要共享会话，请选择适合的目标，例如使用 `/share file` 导出本地 Markdown 文件。
3. 将导出的内容发送给他人或发布前先审查。会话导出内容可能包含提示、响应和项目详情。

发布链接或 gist 是可选操作。不要发布团队不打算公开的存储库或对话内容。

## 可选：扩展并行工作或委派任务

核心研讨会已完成。Copilot CLI 还提供了处理大型任务的命令：

- `/fleet` 可将相互独立的子任务分配给子智能体并行处理。
- `/worktree` 可为独立任务创建隔离的 Git 工作树。
- `/delegate` 可将任务发送给 Copilot 云智能体，由其异步处理并可能创建拉取请求。

这些命令是可选的，因为它们可能创建其他工作树或远程任务。尝试前请先创建范围明确的新任务，并通过常规工作流审查结果。要深入了解异步智能体工作，请继续学习[云智能体研讨会][cloud-workshop]。

## 总结和后续步骤

使用 Copilot CLI 斜杠命令可以调整配置、共享会话并了解幕后运行情况。本课中，你：

- 使用 `/context` 和 `/compact` 了解 Copilot 如何管理上下文。
- 使用 `/model` 查看可用模型。
- 了解如何使用 `/share` 导出或共享会话。
- 探索了并行工作、工作树和可选的云端委派命令。

Copilot CLI 还有更多斜杠命令值得探索。接下来将[回顾所学内容][next-lesson]并了解后续学习方向，为本次学习之旅画上句号。

## 资源

- [Copilot CLI 命令参考][cli-reference]
- [Copilot CLI 中的上下文管理][context-management]
- [关于 Copilot 云智能体][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
