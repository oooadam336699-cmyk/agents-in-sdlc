---
title: "第 4 课 - 使用自定义指令指导 Copilot"
description: "探索仓库指令，添加文档规范，并将其应用于筛选功能代码。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

使用生成式 AI 时，上下文至关重要。如果任务需要按照特定方式完成，你需要让 Copilot 能够获取相应指导。[指令文件][instruction-files]不仅说明你想要什么代码，还说明代码应该如何组织。现在你已构建了筛选功能，接下来将探索 Copilot 使用的指令、添加一项文档规范，并将其应用到代码中。

本课将学习如何：

- 了解仓库指令和按路径应用的指令文件如何传递给智能体。
- 更新指令文件，确保遵循编码标准。
- 了解指令文件对代码的影响。

## 场景

和其他优秀的开发团队一样，Tailspin Toys 也制定了一系列开发规范和要求，包括：

- 注释应解释意图和不明显的决策，而不是复述代码。
- `db/` 和 `src/lib/` 中导出的函数应使用 TSDoc/JSDoc 说明其用途、参数和返回值；如果存在可注入的 `db` 参数，也应予以说明。
- 可复用的 Astro 组件应说明其 `Props` 接口约定；相关代码更改时，注释也应保持最新。
- 应保留现有格式和 lint 指南。

通过使用指令文件，你可以确保 Copilot 按照这些规范执行任务。

## 指令文件

自定义指令可向 Copilot 提供上下文和偏好，帮助它更好地理解你的编码风格和要求。借助此功能，你可以引导 Copilot 提供更相关的建议和代码片段。你可以为整个仓库创建指令，也可以针对特定文件类型创建指令，以便在具体任务中提供上下文。

指令文件有两种：

- `.github/copilot-instructions.md`：每次针对该仓库的请求都会发送给 Copilot 的单个指令文件。内容应与大多数请求相关，例如项目级信息。
- `.github/instructions/*.instructions.md`：针对特定语言、文件类型或任务提供指南。

> [!NOTE]
> 其他指令格式及其支持情况会因环境而异。依赖特定格式前，请查看[自定义指令支持参考][custom-instructions-support]。

## 探索项目中的自定义指令文件

入门项目已包含一组指令文件。先探索现有文件，再进行更改，了解其影响。

1. 返回 Codespace。
2. 在 Codespaces 编辑器（不是终端）中打开 `.github/copilot-instructions.md`。
3. 浏览文件，了解其中简要的项目说明和编码指导。这些指令适用于 Copilot 在此仓库中的每次交互。
4. 打开 `.github/instructions` 文件夹并浏览其中的文件。你会看到针对 Astro 文件、Drizzle 数据层、测试等内容的指令。
5. 打开 `.github/instructions/unit-tests.instructions.md`。注意顶部的 `applyTo` 字段，它用于设置决定指令适用文件范围的 glob。
6. 打开 `.github/instructions/drizzle.instructions.md`，查看它对其他指令文件和现有项目文件的链接。这样可以将较大的指令集拆分成较小的可复用文件，并为 Copilot 提供可参考的示例。

## 更新指令文件以符合团队规范

现有文件已经是很好的起点，但仍有遗漏。修改核心文件 `copilot-instructions.md`，确保新生成的 TypeScript 代码包含 TSDoc 注释。

1. 在 `.github/copilot-instructions.md` 中找到 **Code formatting guidance** 部分，该部分应位于第 35 行附近。
2. 在最后一个项目符号后添加以下内容：

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

该文件会自动保存！

## 使用更新后的指导

Copilot CLI 会在对话启动时加载仓库指令。保存更新后的指令后，退出并恢复筛选功能对话，这样 Copilot 就能在保留功能上下文的同时加载新指导。

1. 如果筛选功能对话仍处于打开状态，请输入 `/exit` 退出。Copilot CLI 会自动保存对话。
2. 在 `game-filters-cli` 分支的仓库根目录中，恢复已保存的对话：

   ```bash
   copilot --resume
   ```

   如果出现提示，请选择筛选功能对话，并确认当前检出的是 `game-filters-cli` 分支。
3. 请 Copilot 应用更新后的指导：

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. 输入 `/diff` 并查看已更改的 TypeScript 文件。注意新生成的 TSDoc 注释，并确认其准确说明了代码。

## 总结和后续步骤

你已了解 Copilot CLI 如何从指令文件获取上下文，并将新规范应用到功能中。具体来说，你已经：

- 了解仓库指令和按路径应用的指令文件如何传递给智能体。
- 更新指令文件，确保遵循编码标准。
- 查看了指令文件对代码的影响。

接下来，你将[自定义并运行可复用的 quality-checks 技能][next-lesson]，确保 lint 和测试以一致的方式运行。

## 资源

- [为 Copilot CLI 添加自定义指令][instruction-files]
- [自定义指令支持情况][custom-instructions-support]
- [创建有效自定义指令的最佳做法][instructions-best-practices]
- [Awesome Copilot — 指令文件和其他资源集合][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
