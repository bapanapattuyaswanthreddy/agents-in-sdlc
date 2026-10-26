---
title: "练习 1 - 自定义说明（VS Code）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：智能体模式 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要按特定方式完成，或 Copilot 需要了解背景信息，就要确保它能获取这些上下文。可以通过多种方式向 Copilot 提供特定上下文，其中重要的一种是[说明文件][instruction-files]，用于指导代码生成。

本练习将学习：

- 了解项目上下文、编码指南和文档标准如何通过存储库自定义说明及按路径指定的说明文件传递给 Copilot。
- 在当前说明的基础上，生成筛选功能的第一个数据层切片（发行商辅助函数）。
- 向 `.github/copilot-instructions.md` 添加一项新的全局存储库标准。
- 重新运行相同提示，观察生成的代码如何应用新标准。
- 将说明更新和筛选功能切片提交到 `main`，以便 Copilot 在下一练习中使用更新后的指引。

> [!CAUTION]
> 生成的代码可能不完全符合你设置的标准。Copilot 具有不确定性。本练习的目标是观察更新说明后行为变化的*趋势*，而不是让输出逐字一致。

## 说明文件

### 场景

Tailspin Toys 制定了一系列开发实践指南和要求，包括：

- 数据层必须始终编写单元测试。
- UI 应采用深色模式并具有现代感。
- 代码应通过 TSDoc 文档注释添加说明。
- 每个文件开头都应添加说明文件用途的注释块。

通过说明文件，可以为 Copilot 提供必要信息，让它按照这些实践完成任务。

### 自定义说明

自定义说明可以向 Copilot 提供上下文和偏好，帮助它了解你的编码风格和要求。你可以指定编码约定、库以及注释规范，从而引导它生成更相关的建议和代码片段。说明可以应用于整个存储库，也可以针对特定文件类型或任务。

说明文件有两种：

- `.github/copilot-instructions.md`：Copilot 在存储库中的**每个**请求都会收到的单个说明文件。它应包含适用于大多数聊天或 CLI 请求的项目级信息，例如技术栈、项目概述、最佳实践和全局指引。
- `.github/instructions/*.instructions.md`：针对特定任务或文件类型的说明文件。可用于 TypeScript 或 Astro 等语言，也可用于创建 UI 组件或单元测试等任务。

> [!NOTE]
> 在 IDE 中，说明文件仅用于 Copilot Chat 代码生成，不用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云智能体生成代码时，都会使用存储库级说明和包含 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> Copilot 还支持其他规范的说明文件，例如 AGENTS.md 和 CLAUDE.md，详见[自定义说明支持][custom-instructions-support]。

### 管理说明文件的最佳实践

完整探讨如何创建说明文件超出本研讨会范围。示例项目展示了具有代表性的方法。总体来说：

- 将 `copilot-instructions.md` 聚焦于项目级指引，例如项目说明、结构和全局编码标准。
- 使用 `*.instructions.md` 为特定文件类型（单元测试、Astro 组件、数据层）或任务提供说明。
- 使用自然语言，让指引清晰易懂，并提供代码示例说明应采用和避免的写法。

说明文件没有唯一的组织方式，AI 的使用方法也一样。通过实践可以找到最适合项目的方法。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应有完善的说明文件集合。浏览项目中的文件时，可能会看到 UI 更新和 [Astro][astro-instructions] 等多种任务的说明。
>
> Copilot 也可以帮助生成说明文件。不同产品入口的方式有所不同，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`。本系列课程会在相关处说明具体入口。
>
> 需要模板或起点？可以探索包含说明文件、自定义智能体和其他资源的 [awesome-copilot][awesome-copilot]。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 探索项目中的自定义说明文件

项目已附带一组说明文件，包括核心 `copilot-instructions.md` 和一组适用于不同任务的 `*.instructions.md` 文件。打开并浏览这些文件。

1. 打开 `.github/copilot-instructions.md`。
2. 查看项目简介，以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。注意 **Code standards** 下嵌套的 **GitHub Actions Workflows** 指引，这些内容适用于与 Copilot 的所有交互。
3. 打开 `.github/instructions` 文件夹并浏览其中的内容。注意其中包含 Astro 文件、Drizzle 数据层、测试等说明。
4. 打开 `.github/instructions/unit-tests.instructions.md`。注意顶部的 `applyTo` 字段，它定义了说明适用于哪些文件的 glob 模式。例如，匹配 `**/*.test.ts` 的 TypeScript 测试文件会应用这些说明。
5. 查看项目中有关创建单元测试的说明。
6. 最后，打开 `.github/instructions/drizzle.instructions.md`，查看其中指向其他说明文件（例如 `unit-tests.instructions.md`）和现有项目文件的链接。这可以将较大的说明拆分为可复用的小文件，并为 Copilot 提供代码示例。（其中的路径相对于说明文件，而不是存储库根目录。）

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分说明了项目编码标准，但尚未要求在代码中添加文档。接下来的步骤会添加 TSDoc 文档注释和文件注释头规则。

## 创建分支

按照最佳实践，为更改创建分支。

1. 返回上一个练习使用的 Codespace。
2. 按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）打开新终端。
3. 创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions
   ```

## 更新说明前先使用 Copilot Chat

> [!TIP]
> **打开 Copilot Chat**
>
> 开始下面的练习前，返回 Codespace 并打开 Copilot Chat 面板，选择 **New Chat** 开始全新对话。每项练习会说明所需模式和模型。

为了了解自定义说明的影响，先在当前说明文件的基础上发送提示。之后更新说明并重新发送相同提示，比较结果。

1. 关闭之前练习留下的编辑器标签页，确保 Copilot 只获取所需上下文。
2. 打开 `src/lib/publishers.ts`，让 Copilot 了解辅助函数应放置的位置。
3. 在 Chat 视图的智能体下拉列表中选择 **Agent**，以便 Copilot 修改文件。

   ![Chat 视图中的智能体选择器截图](../../../_images/shared-chat-mode-selector.png)

4. 发送以下提示：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot 会探索项目并应用代码更新，通常也会修改辅助函数文件和测试。
6. 注意建议的辅助函数是一个类型化函数，接受 `db` 客户端作为第一个参数，并返回类型化的发行商数组。这来自 `.github/instructions/drizzle.instructions.md` 中适用于 `src/lib/*.ts` 的数据层约定。
7. 注意生成的代码**没有** TSDoc 文档注释和文件级注释头。

> [!CAUTION]
> Copilot 具有概率性，因此即使没有明确要求，也可能添加文档注释。即使出现这种情况，也不影响本练习的重点：更新说明后，行为应更加一致。

## 添加一项新的存储库标准

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在添加编码标准，以改进代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分，应该在第 27 行附近。注意这里说明了项目编码标准，但尚无代码文档规则，这也是生成的辅助函数没有文档注释的原因。
3. 在现有标准下方添加以下 Markdown 行，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所述，说明文件可以放在存储库级 `.github/copilot-instructions.md` 中提供全局指引，也可以通过 `*.instructions.md` 为特定语言、文件类型或任务提供说明。刚添加的文档注释规则属于全局项目标准，适合放在存储库级文件中。

## 重新运行提示并观察变化

1. 返回 Copilot Chat，选择 **New Chat** 清空对话上下文。
2. 再次选择 `src/lib/publishers.ts`，让 Copilot 聚焦在正确文件上。
3. 发送与之前相同的提示：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 注意新生成的文件现在以类似以下注释块开头：

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 注意新生成的函数现在包含类似以下的 TSDoc 注释：

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

你刚刚引导 Copilot 遵循新的项目标准，并将其应用于实际代码，下一练习将在此基础上继续。

## 提交说明并推送分支

说明文件与存储库中的其他资源一样，使用相同的版本控制方式管理。现在提交并推送分支。

1. 按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）打开新终端。
2. 在终端中运行以下命令，确认说明更新和辅助函数更改均已存在：

   ```bash
   git status
   ```

3. 在终端中暂存并提交说明更新及 Copilot 为辅助函数基础所做的所有更改：

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # 如果 git status 显示了其他配套更改（例如测试），也要添加这些文件。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 将分支推送到存储库：

   ```bash
   git push -u origin custom-instructions
   ```

## 创建并合并拉取请求

分支已推送，现在创建拉取请求并关联待办事项中的文档标准议题。也可以手动操作，但 Copilot 可以使用项目中已连接的 GitHub 工具来完成。提示 Copilot 查找议题、创建 PR 并关闭议题，然后合并 PR。

> [!NOTE]
> 这是首次让 Copilot 代表你在 GitHub 上执行操作。项目模板已为工作区配置 GitHub 工具，无需额外设置。Copilot 首次使用工具时，VS Code 可能会提示你登录 GitHub。按照提示授权即可。后续课程会介绍这种连接方式（Model Context Protocol）。

1. 在 Codespace 中打开 Copilot Chat。
2. 按 <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>（Windows/Linux）打开 Copilot Chat 视图，并确认智能体选择器中选中了 **Agent**。
3. 让 Copilot 查找与更新说明文件有关的议题，并从当前分支创建 PR，说明说明文件更新和新的发行商辅助函数基础：

   ```text
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot 会查找议题并创建 PR。
5. 如果 Copilot 提示允许它代表你执行 GitHub 操作，请审查命令，并酌情选择 **Allow**。
6. PR 创建后，让 Copilot 合并 PR 并返回 `main`：

   ```text
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. 如果 Copilot 提示允许它执行 GitHub 操作和 shell 命令，请审查命令并酌情选择 **Allow**。

现在，你已借助 GitHub Copilot 创建并合并了拉取请求。

## 总结和后续步骤

你了解了 Copilot 如何从说明文件获取上下文，然后在 VS Code 中使用 Copilot Chat 完成以下工作：

- 发送代码生成提示，观察 Copilot 如何根据*现有*说明生成内容。
- 向 `.github/copilot-instructions.md` 添加新的全局标准。
- 重新发送相同提示，观察生成代码如何应用新标准。
- 将说明更新和辅助函数基础提交到 `main`，为下一练习做准备。

接下来将在[智能体模式][next-lesson]中使用这些说明，让 Copilot 跨代码库添加新功能。

## 资源

- [GitHub Copilot 自定义说明文件][instruction-files]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义说明的 5 个技巧][copilot-instructions-five-tips]
- [GitHub Copilot 个人自定义说明][personal-instructions]
- [Awesome Copilot — 说明文件和其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
