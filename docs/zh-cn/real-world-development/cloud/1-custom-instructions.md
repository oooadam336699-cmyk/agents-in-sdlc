---
title: "练习 1 - 自定义指令（云智能体）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：Copilot 云智能体 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要按照特定方式完成，或者 Copilot 需要了解背景信息，就要确保它能够获取这些上下文。[指令文件][instruction-files]用于提供此类指导，让 Copilot 不仅了解你想让它做什么，还能了解你希望它如何完成。

本练习将学习如何：

- 了解项目上下文、编码指南和文档标准如何通过仓库自定义指令及按路径应用的指令文件传递给 Copilot。
- 向 `.github/copilot-instructions.md` 添加新的全仓库标准。

> [!NOTE]
> 与 VS Code 和 CLI 运行环境不同，这里不会运行更改前后的提示进行对比。Copilot 云智能体会在 GitHub 上异步处理议题，因此难以实时并排展示指令的影响。在本运行环境的后续课程中，你将通过审查云智能体创建的拉取请求，看到指令文件如何影响结果。

## 指令文件

### 场景

和其他优秀的开发团队一样，Tailspin Toys 也制定了一系列开发规范和要求，包括：

- 数据层始终需要单元测试。
- UI 应采用暗色模式并具有现代感。
- 应通过 TSDoc 文档注释为代码添加说明。
- 每个文件开头都应添加注释块，说明该文件的用途。

通过使用指令文件，你可以确保 Copilot 按照这些规范执行任务。

### 自定义指令

自定义指令可向 Copilot 提供上下文和偏好，帮助它更好地理解你的编码风格和要求。借助此功能，你可以引导 Copilot 提供更相关的建议和代码片段。你可以为整个仓库创建指令，也可以针对特定文件类型创建指令，以便在具体任务中提供上下文。

指令文件有两种：

- `.github/copilot-instructions.md`：每次针对该仓库的请求都会发送给 Copilot 的单个指令文件。内容应包括与大多数 Copilot 聊天或 CLI 请求相关的项目级信息，例如所用技术栈、项目概述、最佳做法和全局指导。
- `.github/instructions/*.instructions.md`：可针对特定任务或文件类型创建的文件。你可以为特定语言（例如 TypeScript 或 Astro）或任务（例如创建 UI 组件或一组新单元测试）提供指南。

> [!NOTE]
> 在 IDE 中，指令文件仅用于 Copilot Chat 生成代码，不用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云智能体在生成代码时，都会使用仓库级指令和包含 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> 此外，Copilot [还支持其他标准的指令文件][custom-instructions-support]，包括 AGENTS.md 和 CLAUDE.md 文件。

### 管理指令文件的最佳做法

全面讨论如何创建指令文件超出了本研讨会的范围。不过，示例项目展示了具有代表性的方法。总体而言：

- `copilot-instructions.md` 应聚焦于项目级指导，例如项目内容说明、项目结构和全局编码标准。
- 使用 `*.instructions.md` 为特定文件类型（单元测试、Astro 组件、数据层）或特定任务提供指令。
- 使用自然语言，确保指导清晰，并提供代码应该和不应该采用的形式示例。

指令文件没有唯一的创建方式，正如 AI 的使用方式也不止一种。通过实践，你会找到适合项目的方法。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应该有一套完善的指令文件。探索此项目中的文件时，你可能会发现有针对许多任务的指令，包括 [UI 更新][ui-instructions]和 [Astro][astro-instructions]。
>
> Copilot 也可以帮助你生成指令文件。不同产品界面的操作方式各异（例如 VS Code 中的 **配置聊天 → 生成智能体指令**，或 Copilot CLI 中的 `/init`）；相关课程会在适用时介绍具体方法。
>
> 想查找模板或起点？可以浏览 [awesome-copilot][awesome-copilot]，其中包含大量指令文件、自定义智能体和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 探索项目中的自定义指令文件

花点时间阅读仓库中自带的指令文件：一个核心文件 `copilot-instructions.md`，以及一组适用于各种任务的 `*.instructions.md` 文件。可以在编辑器或 GitHub 网页界面中打开这些文件。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览文件，了解其中的项目简介，以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。**Code standards** 下还有 **GitHub Actions Workflows** 指南。这些内容适用于你与 Copilot 的所有交互。
3. 打开 `.github/instructions` 文件夹并浏览其中的文件。你会看到针对 Astro 文件、Drizzle 数据层、测试等内容的指令。
4. 打开 `.github/instructions/unit-tests.instructions.md`。注意顶部的 `applyTo` frontmatter，它设置了一个相对于仓库根目录的 glob，用于决定文件适用范围。此处，任何匹配 `**/*.test.ts` 的 TypeScript 测试文件都会应用这些指令。
5. 查看此项目中创建单元测试的专用指令。
6. 最后，打开 `.github/instructions/drizzle.instructions.md` 并滚动到末尾。注意其中指向其他指令文件（例如 `unit-tests.instructions.md`）和项目现有文件的链接。这样可以将较大的指令集拆分成较小的可复用文件，并为 Copilot 提供生成代码时可参考的示例。（其中的路径相对于指令文件所在位置，而不是仓库根目录。）

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分介绍了项目编码标准，但尚未要求在代码中添加文档注释。接下来的步骤会添加 TSDoc 文档注释和文件注释头的规则。

## 添加新的仓库标准

接下来只需编辑一处：添加一条项目级规则，要求在代码中使用 TSDoc 文档注释和文件级注释头。后续练习中，云智能体处理你分配的议题时会遵循这条规则。

编辑前，先创建一个分支（云智能体会读取议题目标分支中的指令，但仍建议干净地提交更改）：

1. 在 Codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在为仓库记录编码标准，以改进代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分，该部分应位于第 27 行附近。注意其中记录了项目编码标准，但尚未包含代码文档规则，这也是生成的辅助函数没有文档注释的原因。
3. 在现有标准下方添加以下 Markdown 内容，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所示，指令文件可以创建在仓库级别（`.github/copilot-instructions.md`）以提供全局指导，也可以创建为 `*.instructions.md` 文件以针对特定语言、文件类型或任务提供指令。对于刚添加的文档注释规则等全项目标准，仓库级文件是合适的保存位置。

## 提交、推送并合并指令更新

云智能体会从议题目标分支读取指令。下一练习中将议题分配给 Copilot 时，它会从 `main` 创建分支。因此，必须先将指令更改合并到 `main`，云智能体才能使用。

1. 暂存并提交：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 合并到 `main` 的拉取请求，并合并该请求。下一练习中，云智能体处理议题时就会读取这些指令。

> [!TIP]
> 如果希望本次研讨会直接在 `main` 上操作，也可以跳过分支步骤，直接提交到 `main`。创建分支是为了让研讨会流程更贴近真实项目的工作方式。

## 总结和后续步骤

你已了解 Copilot 如何从项目中的指令文件获取上下文，并向 `.github/copilot-instructions.md` 添加了新的仓库级标准。在本运行环境剩余课程中，你会看到云智能体创建的拉取请求如何遵循此标准。

接下来，[将第一个议题分配给 Copilot 云智能体][next-lesson]。

## 资源

- [GitHub Copilot 自定义指令文件][instruction-files]
- [创建自定义指令的最佳做法][instructions-best-practices]
- [编写更好的 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [Awesome Copilot — 指令文件和其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
