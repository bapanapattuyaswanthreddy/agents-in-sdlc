---
title: "练习 5 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义智能体][previous-lesson] |
|:--|

使用 GitHub Copilot 智能体模式时，它会自主探索代码库、提出更改、编辑文件并运行命令。由于这些工作会在你的计算机上实时进行，VS Code 中的 Copilot Chat 会持续显示每次工具调用和文件编辑。你可以直接在编辑器中审查每项差异、接受或拒绝单独更改，也可以通过后续提示引导对话，调整或扩展工作。

本练习将学习：

- 实时监控无障碍智能体的工作。
- 审查并接受建议的文件编辑。
- 在会话进行中引导智能体，在高对比度模式之外添加浅色模式切换。

## 场景

[练习 4][previous-lesson]中使用的无障碍自定义智能体正在实现高对比度模式。工作进行中，设计团队又提出了一个要求：也为觉得默认主题太暗的用户添加浅色模式切换。无需从头开始新对话，你将引导现有会话扩展工作范围。

## 监控 Copilot 进度

Copilot Chat 视图会显示智能体执行的每个步骤，包括探索的文件、运行的命令和提出的编辑。每项文件编辑都会在聊天中以内嵌差异显示，并提供 **Keep** 和 **Undo** 控件。

1. 返回练习 4 中使用的 Copilot Chat 视图。
2. 智能体工作时，在聊天中滚动查看每次工具调用。你会看到文件读取、文件编辑和它请求运行的终端命令。
3. 智能体提出文件编辑后，差异会以内嵌形式显示：
   - 选择 **Keep** 接受更改。
   - 选择 **Undo** 撤销更改，让智能体重新尝试。
4. 也可以打开聊天顶部的 **Files changed** 指示器，查看该会话中智能体修改的全部文件。

> [!NOTE]
> 如果工具调用请求权限（例如运行终端命令），智能体会暂停并显示 **Allow** 提示。只批准自己确认安全的命令。

5. 等待智能体完成高对比度模式。它会发送总结所做更改的最终消息。

## 引导智能体添加浅色模式

高对比度模式已就绪，现在在同一对话中添加浅色模式切换。由于继续使用相同的聊天会话，智能体会保留之前工作的上下文，包括修改过的组件、采用的样式方案和本地存储模式。

1. 在 Copilot Chat 输入框中发送以下后续提示：

    ```plaintext
    Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
    ```

2. 智能体会继续之前的工作，检查已有更改并提出浅色模式的补充内容。
3. 与高对比度模式相同，逐项审查新编辑，并使用 **Keep** 或 **Undo** 处理差异。
4. 如果智能体的方向不符合预期，例如试图以不合意的方式重构切换 UI，可以选择聊天输入框旁的 **Stop** 按钮停止工作，然后发送澄清提示。

> [!TIP]
> 通过后续提示引导正在运行的会话，比重新开始更快。可以要求调整（“简化 CSS，使用 CSS 变量而不是重复选择器”）、纠正方向（“不要添加第三个切换项，把浅色模式整合到现有设置面板中”）或扩展范围（“也为两个切换项添加键盘快捷键”）。

## 审查工作集

提交前，快速检查智能体修改的所有内容。

1. 在 VS Code 中打开 **Source Control** 视图。
2. 审查所有更改文件。应能看到 Astro 组件、样式和相关测试的更新。
3. 打开一些更改文件并检查差异。确认自定义智能体要求的无障碍模式已应用，例如 ARIA 属性、键盘导航、语义化 HTML 和本地存储持久化。
4. 确认无误后，在 Source Control 面板中暂存并提交更改。下一课将发布这些更改。

## 总结和后续步骤

你使用 Copilot Chat 智能体模式监控并引导正在运行的会话：审查每项文件编辑、接受或拒绝更改，并通过后续提示扩展工作，同时保留上下文。

本课探索了以下概念：

- 实时监控智能体工作。
- 接受或拒绝单独的文件编辑。
- 通过后续提示引导正在运行的会话。

在[下一课][next-lesson]中，你将通过拉取请求发布无障碍功能，并审查本实验中本地自定义智能体生成的更改。

## 资源

- [VS Code 中的 Copilot Chat][copilot-chat-vscode]
- [使用智能体模式][agent-mode]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代改进 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
