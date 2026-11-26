---
title: "练习 2 - 使用 Copilot 智能体模式添加新功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

即使是最简单的应用更新，通常也需要修改多个文件并执行多项操作，例如运行测试。开发者的工作流程往往包括找到所有相关文件、进行修改、运行测试、调试、发现遗漏的文件、再次修改……需要做的事情非常多。

Copilot 智能体模式正是为此而设计的。

Copilot 智能体模式可以在 IDE 中更自主地工作。它的行为类似开发者：先探索现有项目结构，再执行必要的更新、运行测试等任务，并自动修复发现的问题。现在了解如何使用智能体模式为网站添加新功能。

> [!NOTE]
> 智能体模式和云智能体的名称相似，但适用于两种不同的体验。智能体模式在 IDE 中执行任务，可以快速反馈并随时交互；云智能体则像团队成员一样异步工作，通过议题和拉取请求与你协作。

本练习将学习如何：

- 使用 Copilot 智能体模式探索项目、查找相关文件并协同修改。
- 使用 GitHub Copilot 智能体模式跨 UI 和数据层实现新功能。
- 在将代码合并到代码库之前，审查智能体模式生成的更改和测试。

## 场景

随着游戏列表不断增长，你希望用户能够按类别和发行商筛选。上一练习中，你已添加发行商辅助函数；现在将使用 Copilot 智能体模式完成剩余的数据层、UI 和测试工作。

## 运行 Tailspin Toys 网站

进行任何更改前，先探索 Tailspin Toys 网站，了解其现有功能。

该网站是一个以开发者为主题的桌游众筹平台。用户可以浏览游戏列表并查看游戏详情。它是一个单独的 Astro 应用，在构建时将页面渲染为静态 HTML。页面通过 Drizzle ORM 在 frontmatter 中直接查询本地 SQLite 数据库，没有独立的后端 API 或客户端 UI 框架。可复用的数据访问辅助函数位于 `src/lib/`，交互功能则通过使用标准 DOM API 的小型 Astro `<script>` 实现。

### 启动网站

项目提供了 `npm run dev` 任务，用于启动单个 Astro 开发服务器。按照以下步骤在 GitHub Codespace 中运行网站：

1. 返回 Codespace。上一练习后，你应该处于 `main` 分支。
2. 按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux），在 Codespace 中打开新终端窗口。
3. 创建并切换到筛选功能分支：

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 运行以下命令启动网站：

   ```bash
   npm run dev
   ```

   Astro 开发服务器就绪后，会显示包含 URL 的提示，类似如下内容：

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> 如果弹出对话框，提示你打开 `http://localhost:4321` 的浏览器窗口，请选择 **x** 将其关闭。

5. 在 Mac 上按住 <kbd>Command</kbd>，或在 Windows/Linux 上按住 <kbd>Ctrl</kbd>，然后在终端中选择 Astro 服务器地址 `http://localhost:4321`，打开网站。

> [!NOTE]
> 在 Codespace 中，从终端选择 localhost URL 后，系统会自动将你重定向到 `https://<your-codespace-name>-4321.app.github.dev/`。这是指向 Codespace 的专用隧道，网站服务器正在其中运行！

### 探索网站

网站启动后，可以探索其功能。主要功能包括：

- **主页**：显示桌游列表，包括标题、图像和描述。
- **游戏详情页**：选择游戏后会打开详情页，其中包含游戏标题、描述、发行商和类别等详细信息。

## 使用 Copilot 探索待办列表

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 Codespace，打开 Copilot Chat 面板并选择 **新建聊天**，开始一个干净的对话。不同练习使用的模式和模型可能不同；相关步骤会在需要时说明。

网站的初始实现已经可用，但我们想添加新功能。先查看待办列表。使用模板设置项目时，系统已自动创建一组议题；现在请 GitHub Copilot 显示这些项目。

1. 在 Chat 视图的智能体下拉列表中选择 **Agent**。**Agent** 智能体会自主规划并跨文件实现更改、运行终端命令并调用工具。

   ![Chat 视图中的智能体选择器截图](../../../_images/shared-chat-mode-selector.png)

2. 从可用模型列表中选择 **Claude Sonnet 4.5**。

> [!CAUTION]
> 本研讨会作者并未偏好某个模型。编写研讨会时使用的是 Claude Sonnet 4.5，因此将其列入说明，希望你获得的代码建议相对一致，体验更顺畅。不过，由于 LLM 具有概率性，你收到的建议可能与研讨会示例不同，这是正常且预期的情况。

> [!NOTE]
> 由于 LLM 具有概率性，Copilot 可能会使用不同的 MCP 命令，但仍应能完成任务。

3. 向 Copilot 发送以下提示，询问待办议题：

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. 选择 **继续**，运行列出所有议题的命令。
5. 查看生成的议题列表。

注意，Copilot 还根据它认为对用户最有用的程度为项目排定了优先级。

## 查看指令文件

开始让智能体生成代码前，先查看将用于提供上下文的指令文件。我们将使用[用户界面（UI）](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)指令文件，其中包含了如何为网站添加功能的上下文。

1. 在 Codespace 中打开 `.github/instructions/ui.instructions.md`。
2. 留意其中关于如何添加功能的整体指导，包括：
   - 架构概述。
   - 组件设计、可测试性和无障碍原则。
   - 指向各类文件专用指令的链接，包括：
     - Astro
     - Tailwind CSS
     - Drizzle

> [!TIP]
> 指令文件可以引用其他指令文件和项目中的文件。路径相对于指令文件所在位置。这样可以复用内容、将复杂指令拆分成更易管理的部分，并提供示例和模板。

## 实现筛选功能

完成筛选功能至少需要对应用进行三项不同的更新：

- 在数据层（`src/lib/`）添加或完善剩余筛选逻辑。
- 添加或更新筛选行为测试。
- 更新游戏列表页，添加筛选 UI。

此外，将更改合并到代码库前，还需运行并通过测试。Copilot 智能体模式可以完成这些任务！现在开始添加此功能。

1. 可以继续当前 Copilot 对话，也可以选择 **新建聊天** 启动新对话。
2. 选择 **添加上下文**、**指令**，然后选择 **ui** 指令文件。

   ![选择 UI 指令文件的示例截图](../../../_images/ex3-select-instructions-file.png)

3. 确认 Chat 视图的智能体下拉列表中仍然选中 **Agent**。

   ![Chat 视图中的智能体选择器截图](../../../_images/shared-chat-mode-selector.png)

4. 确认当前模型仍为 **Claude Sonnet 4.5**。
5. 使用以下提示，请 Copilot 根据待办列表中的相关议题实现功能：

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. 观察 Copilot 如何先探索项目并找到相关文件。你应该会看到它找到数据层辅助函数、UI 和测试，然后开始修改文件并运行测试。

   ![Copilot 探索项目文件的截图](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot 会执行多个任务，例如探索项目、修改文件和运行测试。根据任务和代码库的复杂程度，这可能需要几分钟。过程中，代码编辑器中可能会出现 **保留** 和 **撤销** 按钮。Copilot 完成后，所有更改都会显示相应的 **保留** 或 **撤销** 按钮，因此无需在它工作时提前选择。

7. Copilot 提示时，选择 **继续** 运行测试。

   ![Copilot Chat 面板中的对话框截图，请用户确认是否运行测试](../../../_images/ex3-agent-mode-run-tests.png)

8. 过程中可能会出现停顿，甚至有测试暂时失败。这没有关系！Copilot 会在生成代码和运行测试之间反复迭代，直到任务完成且未检测到错误。

   ![已完成的 Copilot 智能体模式聊天会话截图](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 检查生成的代码，查找潜在问题。

> [!CAUTION]
> 记住，始终都应审查 Copilot 或其他 AI 工具生成的代码。

10. 返回正在运行网站的浏览器，体验新功能！
11. 确认功能正常并审查代码后，在 Copilot Chat 窗口中选择 **保留**。

## 总结和后续步骤

恭喜！本练习介绍了如何使用 GitHub Copilot 智能体模式为 Tailspin Toys 网站添加新功能。你已经了解如何：

- 使用 GitHub Copilot 智能体模式跨 UI 和数据层实现新功能。
- 使用智能体模式探索项目、查找相关文件并协同修改。
- 在将代码合并到代码库之前，审查智能体模式生成的更改和测试。

接下来，使用 [Playwright MCP 服务器测试功能][next-lesson]并为其创建拉取请求。

### 加分探索练习 - 实现分页

随着游戏列表不断增长，网站需要支持分页。运用本练习中学到的技能，请 Copilot 更新网站以实现分页。编写代码时可考虑：

- 遵循现有最佳做法，包括使用现有指令文件。
- 考虑分页方式：允许用户选择页面大小，还是将其固定。
- 编写提示时，向 Copilot 提供所需指导，确保实现符合你的预期。
- 可能需要与 GitHub Copilot 迭代，要求它更改代码并补充上下文。这是使用 Copilot 的正常流程！

## 资源

- [Copilot 的 ask、edit 和 agent 模式：功能及适用场景][choose-mode]
- [VS Code 中的智能体模式][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
