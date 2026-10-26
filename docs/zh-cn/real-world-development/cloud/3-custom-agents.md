---
title: "练习 3 - 自定义智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## 什么是自定义智能体？

GitHub Copilot 中的[自定义智能体][custom-agents-concept]可针对开发工作流中的特定任务或领域创建专用 AI 助手。在存储库的 `.github/agents` 文件夹中通过 Markdown 文件定义智能体，可以为 Copilot 提供聚焦的说明、最佳实践、编码模式和领域知识，帮助它更有效地完成特定类型的工作。团队可以将专业知识整理为可复用的智能体，例如确保符合 [WCAG][wcag] 的无障碍智能体、遵循安全编码实践的安全智能体，或维护一致测试模式的测试智能体。

项目中的自定义智能体由 `.github/agents` 文件夹中的 Markdown 文件定义，也可以全局定义在 `~/.copilot/agents` 中。每个文件都包含至少具有 `name` 和 `description` 的 YAML frontmatter，后面是定义智能体行为、专业知识和说明的 Markdown 提示。

### 自定义智能体与智能体技能的比较

自定义智能体和[智能体技能][agent-skills-concept]有一些重叠：二者主要通过 Markdown 文件定义，并告诉 AI 如何执行操作。最简单的区分方式是：**自定义智能体是执行工作的角色，技能是工具。**

自定义智能体拥有自己的上下文窗口，并可在工作时协调技能甚至其他智能体。本实验中的无障碍自定义智能体会根据无障碍指南审查并更新网站，也可以调用拉取请求工作流技能或运行并管理测试的技能。

> [!NOTE]
> 编写自定义智能体没有唯一的正确方式。和 AI 的其他用途一样，应针对自己的环境和场景进行测试并持续迭代。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

本课将学习自定义智能体的定义方式，以及如何向自定义智能体分配任务。

## 场景

Tailspin Toys 致力于让所有用户都能使用其众筹平台，不受视觉能力或偏好影响。近期用户反馈指出，由于文字与背景颜色对比度不足，一些用户难以阅读当前深色主题。为解决此无障碍问题，设计团队要求实现可由用户开关的高对比度模式。

无障碍访问至关重要，因此希望尽快实现此功能。你将使用自定义智能体生成相应功能。

## 审查无障碍自定义智能体

已经为你创建了一个无障碍自定义智能体。先审查其内容，了解它将如何引导 Copilot。

返回 Codespace，然后查看无障碍自定义智能体文件：

1. 打开 `.github/agents/accessibility.md`。
2. 注意包含 `name` 和 `description` 字段的 YAML frontmatter。

> [!CAUTION]
> 自定义智能体必须包含 `name` 和 `description` frontmatter。

3. 浏览后续章节，查看其中的内容：
   - 生成无障碍网站代码时的核心职责。
   - 无障碍最佳实践。
   - HTML、CSS 和 JavaScript 代码示例。
   - 常见问题和错误列表。

## 创建并分配议题

任务控制中心是管理环境中所有智能体的中心位置。你可以将任务分配给 Copilot 云智能体、监控任务，还可以调整方向并提供更多指导。现在将任务分配给 Copilot，为网站创建高对比度模式。

1. 打开存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue** 创建空白议题。
5. 将 **Title** 设置为 `Add high contrast mode to website`。
6. 将 **Description** 设置为：

    ```plaintext
    We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。
9. 从自定义智能体列表中选择 **Accessibility agent**。

    ![云智能体分配界面截图，其中突出显示了自定义智能体和无障碍选项](../../../_images/ex5-select-custom-agent.png)

10. 选择 **Assign**。
11. Copilot 会在后台开始处理任务。

## 总结和后续步骤

本课介绍了 GitHub Copilot 中的[自定义智能体][custom-agents]，它们是针对特定任务和领域的专用 AI 助手。借助自定义智能体，可以将团队专业知识和标准整理为可复用的智能体，从而更有效地引导 Copilot 完成特定类型的工作。

本课探索了以下概念：

- 自定义智能体的定义方式。
- 将任务分配给自定义智能体。

Copilot 正在实现高对比度模式，接下来可以在任务控制中心[监控并引导智能体会话][next-lesson]。

## 资源

- [关于自定义智能体][custom-agents]
- [为组织准备自定义智能体][org-custom-agents]
- [为企业准备自定义智能体][enterprise-custom-agents]

---

| [← 上一课：GitHub Copilot 云智能体][previous-lesson] | [下一课：监控和管理智能体 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
