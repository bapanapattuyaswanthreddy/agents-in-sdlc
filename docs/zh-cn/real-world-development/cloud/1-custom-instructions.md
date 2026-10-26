---
title: "练习 1 - 自定义说明（云智能体）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：Copilot 云智能体 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要按特定方式完成，或 Copilot 需要了解背景信息，就要确保它能获取这些上下文。[说明文件][instruction-files]可以提供相应指导，让 Copilot 不仅了解要做*什么*，还了解应当*如何*完成。

本练习将学习：

- 了解项目上下文、编码指南和文档标准如何通过存储库自定义说明及按路径指定的说明文件传递给 Copilot。
- 向 `.github/copilot-instructions.md` 添加一项新的全局存储库标准。

> [!NOTE]
> 与 VS Code 和 CLI 系列课程不同，本课程不会通过前后对比提示词来演示效果。Copilot 云智能体会异步处理 GitHub 议题，因此难以实时并排展示影响。之后审查云智能体创建的拉取请求时，可以看到说明文件的作用。

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

花些时间阅读存储库附带的说明文件：其中有一个核心 `copilot-instructions.md` 文件，以及一组用于不同任务的 `*.instructions.md` 文件。请在编辑器或 GitHub 网页界面中打开。

1. 打开 `.github/copilot-instructions.md`。
2. 查看其中的项目简介，以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。注意 **Code standards** 下嵌套的 **GitHub Actions Workflows** 指引。这些说明适用于与 Copilot 的所有交互。
3. 打开 `.github/instructions` 文件夹并浏览其中的内容。注意其中包含 Astro 文件、Drizzle 数据层、测试等说明。
4. 打开 `.github/instructions/unit-tests.instructions.md`。注意顶部的 `applyTo` 字段，它定义了相对于存储库根目录的 glob 模式，决定说明适用于哪些文件。例如，匹配 `**/*.test.ts` 的 TypeScript 测试文件会应用这些说明。
5. 查看项目中有关创建单元测试的说明。
6. 最后，打开 `.github/instructions/drizzle.instructions.md` 并滚动到末尾。注意指向其他说明文件（例如 `unit-tests.instructions.md`）和项目现有文件的链接。这可以将大型说明拆分为可复用的小文件，并为 Copilot 提供示例。（其中的路径相对于说明文件，而不是存储库根目录。）

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分说明了项目编码标准，但尚未要求在代码中添加文档。接下来的步骤会添加 TSDoc 文档注释和文件注释头规则。

## 添加一项新的存储库标准

接下来只需编辑一处：添加一项全局项目规则，要求通过 TSDoc 文档注释和文件级注释头为代码添加说明。之后本系列中分配议题时，云智能体会读取这项规则。

编辑前，先创建分支（云智能体会读取议题目标分支中的说明，但仍建议单独提交更改）：

1. 在 Codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

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

## 提交、推送并合并说明更新

云智能体会从议题的目标分支读取说明。下一练习中将议题分配给 Copilot 时，它会从 `main` 创建分支，因此说明更改必须先合并到 `main`，云智能体才能使用。

1. 暂存并提交：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 到 `main` 的拉取请求并合并。之后云智能体处理下一练习中的议题时就会读取这些说明。

> [!TIP]
> 如果想直接在 `main` 上完成本研讨会，也可以跳过分支，直接提交到 `main`。创建分支是为了模拟真实项目中的工作方式。

## 总结和后续步骤

你了解了 Copilot 如何从项目说明文件获取上下文，并向 `.github/copilot-instructions.md` 添加了新的全局标准。之后审查云智能体创建的拉取请求时，可以看到该标准的应用情况。

接下来将[把第一个议题分配给 Copilot 云智能体][next-lesson]。

## 资源

- [GitHub Copilot 的说明文件][instruction-files]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义说明的 5 个技巧][copilot-instructions-five-tips]
- [Awesome Copilot — 说明文件和其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
