---
title: "练习 4 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前几节练习中，你让 Copilot 云智能体执行了三项任务，旨在改善用户体验并添加功能。云智能体可以异步自主工作，但监控这些任务仍然很重要。

有多种工具可用于管理分配给云智能体的任务，包括 GitHub.com 上的[智能体页面][agents-page]。在这个任务控制中心中，可以查看所有具有未关闭拉取请求（PR）的智能体任务，了解执行的操作，还可以引导正在运行的会话。

本课将学习：

- 使用智能体页面监控云智能体任务。
- 引导正在运行的会话，请求添加其他功能。

## 场景

将高对比度模式任务分配给智能体后，团队发现也可以顺便添加浅色模式。既然已经要更新网站样式并添加切换功能，将浅色模式一并实现也很合理。现在引导智能体，确保它同时添加浅色和高对比度模式。

## 查看 Copilot 云智能体任务

查看分配给 Copilot 云智能体的所有任务当前状态。

1. 打开 [Copilot 智能体页面](https://github.com/copilot/agents)。
2. 查看主面板和左侧面板中的任务列表。应能看到自己分配给 Copilot 的任务，例如：
   - 更新代码文档。
   - 生成用于修改游戏的 API。
   - 为网站添加高对比度模式。
3. 选择一个正在运行的任务，查看 Copilot 已执行的工作，包括：
   - 从存储库检出代码。
   - 创建 Copilot 工作环境。
   - 设置 MCP 服务器。
   - 执行完成任务所需的其他步骤。

> [!NOTE]
> 具体步骤会根据 Copilot 的工作状态和采用的方法而异。

4. 同时查看右侧显示的拉取请求面板，可在其中查看 PR 和更改文件，以便进一步监控。

## 引导云智能体

了解当前活动任务后，现在要求 Copilot 在实现高对比度模式时一并添加浅色模式切换功能。

1. 选择与添加高对比度模式相关的会话。具体标题会因 Copilot 使用的名称和当前工作状态而异。

    ![任务控制中心中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 观察会话几分钟，直到它显示设置已完成并开始执行任务。开始出现类似下方的消息时，就表示已进入此阶段。
3. 在 **Steer active session while Copilot is working** 对话框中输入以下提示：

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![智能体页面中的云智能体任务截图，其中突出显示了 Steer active session while Copilot is working 对话框](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 注意 Copilot 如何确认收到提示并将其纳入工作流程。

## 让 Copilot 继续工作

和之前一样，Copilot 会继续处理更新后的任务。它会先完成收到消息时正在执行的步骤，然后将新请求纳入工作流程。

这同样需要几分钟，可以借此暂停并回顾目前学到的内容。

## 总结和后续步骤

本课介绍了 Copilot 智能体页面，这是监控和引导 GitHub Copilot 云智能体任务的中心。你可以跟踪所有活动和已完成任务、审查正在执行的工作，也可以重定向正在运行的任务以调整范围或提供更多指导。

本课探索了以下概念：

- 使用任务控制中心和智能体页面监控云智能体任务。
- 重定向正在运行的会话，请求添加其他功能。

Copilot 正在完成无障碍功能。接下来可以[迭代改进 Copilot 创建的拉取请求][next-lesson]。

## 资源

- [Copilot 智能体页面][agents-page]
- [关于自定义智能体][custom-agents]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代改进 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
