---
title: "练习 2 - GitHub Copilot 云智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义指令][previous-lesson] |
|:--|

许多组织都深受技术债务困扰，例如未解决的安全问题、需要更新的旧代码，或因缺乏时间而长期搁置的功能请求。GitHub Copilot 云智能体可以自主执行更新代码和添加功能等任务。完成后，它会创建草稿 PR，供开发者审查。这样可以将繁琐任务交给智能体，加快开发进度，让开发者专注于更重要的工作。

本练习将了解 Copilot 云智能体的以下用法：

- 自定义代码生成环境。
- 确保操作安全。
- 了解明确界定议题范围的重要性。
- 将议题分配给 Copilot。

## 场景

Tailspin Toys 有一些需要处理的技术债务。最初受雇开发网站的承包商没有妥善编写文档，甚至可以说完全缺少文档。团队希望首先为应用程序中所有导出的函数添加 TSDoc 文档注释。

此外，设计团队已准备好改善游戏发现体验。他们希望在每款游戏的详情页上显示相关游戏，也就是同类别的其他游戏，方便用户继续浏览。现阶段，他们不需要精致的实现，只需要可以用于 UX 验收测试的版本。此需求目前是阻碍项，但还有其他优先级更高的议题。

这两项任务都可能被轻易延后处理，因此很适合交给 Copilot 云智能体。云智能体可以异步处理任务，让开发者专注于其他工作，之后再回来审查 Copilot 的成果并确认一切符合预期。

## 了解 GitHub Copilot 云智能体

[GitHub Copilot 云智能体](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent)可以像开发者一样在后台执行任务。你可以通过多种方式启动任务，例如[将 GitHub 议题分配给 Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)。分配任务后，Copilot 会创建草稿拉取请求以跟踪进度、设置环境并开始工作。你可以在会话进行中或完成后查看详情。解决方案准备好等待审查时，Copilot 会在拉取请求中提及你。

## 明确界定指令范围的重要性

GitHub Copilot 并非魔法，使用它并不意味着可以跳过思考。看似简单的操作往往也有复杂之处，因此请务必[谨慎确定 Copilot 云智能体任务的范围][cloud-agent-best-practices]。将它视为 AI 结对程序员：分阶段工作、学习、试验并逐步调整。加入生成式 AI 并不会改变软件开发的基本原则。

## 此仓库中的自定义指令

前面的练习介绍了自定义指令及其指导 Copilot 的方式。如果你已经完成自定义指令练习，就会看到 `.github/copilot-instructions.md` 的作用；如果还没有，现在可以快速查看。分配工作前，以只读方式检查仓库中已有的指令文件，稍后便能观察它们的影响。

在仓库的 GitHub 网页界面中打开以下文件；如果已有运行中的 Codespace，也可以在那里打开：

- `.github/copilot-instructions.md`：查看 **Code standards** 部分，尤其是 TypeScript 规范和 TSDoc 文档注释要求。
- `.github/instructions/unit-tests.instructions.md`：注意 `applyTo` frontmatter，它将指令限定在 `**/*.test.ts` 文件中。

在下一部分将 **Code lacks documentation** 议题分配给云智能体时，留意生成的拉取请求是否包含 TSDoc 文档注释、TypeScript 规范和文件注释头；这些都来自上述指令文件。稍后审查 PR 时还会再次提及。

## 为 Copilot 云智能体设置开发环境

无论由谁编写代码，通常都需要特定环境并运行一些设置脚本，以确保环境正常。将任务分配给 Copilot 时也是如此，因为它的工作方式类似于软件工程师。

云智能体使用 [GitHub Actions][github-actions] 执行任务。你可以在 `.github/workflows/copilot-setup-steps.yml` 中创建和配置[专用设置工作流][setup-workflow]，让它在开始工作前运行。这样可确保智能体获得所需的开发工具和依赖项。实验已预先配置此工作流，以便顺利完成课程。工作流会为这个 Astro 应用准备 Node.js、项目依赖项、端到端测试所需的浏览器二进制文件，以及已迁移并填充数据的 SQLite 数据库：

```yaml
name: "Copilot Setup Steps"

# 允许你从仓库的“Actions”选项卡测试设置步骤
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 仅设置步骤实际需要的最低权限。Copilot 会使用自己的令牌执行操作。
    permissions:
      # 例如，如果设置步骤需要克隆仓库以安装依赖项，则需要 `contents: read` 权限。如果设置步骤不克隆仓库，Copilot 会在步骤完成后自动克隆。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # 前端/应用设置 - Node.js（整个应用现在基于 Astro + Drizzle/libSQL）
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # 迁移并填充本地 SQLite 数据库，为构建和测试准备数据。
      - name: Set up the database
        run: npm run db:setup
```

此文件与其他 GitHub 工作流文件类似，但有几个关键点：

- 文件中只有一个名为 `copilot-setup-steps` 的作业。Copilot 开始处理拉取请求前，GitHub Actions 会先执行此作业。
- 注意 `workflow_dispatch` 触发器，它允许你在仓库的 **Actions** 选项卡中手动运行工作流。这有助于验证工作流是否成功，而不必等待 Copilot 启动它。

## 添加文档

大家都知道文档很重要，但多数项目的信息已经过时，或者完全缺少文档。这类技术债务经常无人处理，会拖慢工作效率、增加代码库维护难度，也让新成员难以加入。幸运的是，Copilot 擅长编写文档，因此很适合将此议题分配给 Copilot 云智能体。它会在后台生成所需文档。后续练习中，你会回来审查它的成果。

1. 在新的浏览器选项卡中打开 github.com 上的仓库。
2. 选择 **Issues** 选项卡。
3. 选择 **新建议题**，打开新议题对话框。
4. 选择 **空白议题**，创建新议题。
5. 将 **标题** 设为 `Code lacks documentation`。
6. 将 **描述** 设为：

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. 选择 **创建**，创建议题。
8. 在右侧选择 **分配给 Copilot**，打开分配对话框。

  ![将议题分配给 Copilot](../../../_images/shared-assign-copilot.png)

9. 选择 **分配**。

  ![Copilot 任务分配详情](../../../_images/ex4-assign-copilot-details.png)

10. 选择 **Pull Requests** 选项卡。
11. 打开刚创建的拉取请求（PR），其标题可能类似于 `[WIP]: Code lacks documentation`。如果列表中没有出现新 PR，请稍等片刻并刷新浏览器。
12. 几分钟后，Copilot 应会创建待办事项列表。

> [!NOTE]
> Copilot 可能需要几分钟才能在 PR 中显示待办事项列表。它正在创建环境（运行前面介绍的工作流）、分析项目并确定最佳解决方案。

13. 查看待办事项列表及其计划完成的任务。
14. 向下滚动拉取请求时间线，应能看到 Copilot 已开始处理议题的更新。
15. 选择 **查看会话** 按钮。

  ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 可能需要刷新页面才能看到更新后的状态指示器。

16. 浏览实时会话，观察 Copilot 如何解决问题，包括探索代码并了解当前状态、暂停思考并制定合适方案，以及编写代码。

这可能需要几分钟。Copilot 云智能体的主要目标之一是异步执行任务，让我们能专注于其他工作。现在就利用这一点：先给 Copilot 云智能体分配另一个任务，然后转而为应用编写代码、添加功能。

## 在游戏详情页添加相关游戏部分

如前所述，GitHub Copilot 云智能体的一大优势是可以拆分工作：你专注于一组任务，同时让它处理另一组任务。为设计团队添加相关游戏部分也许不会花很长时间，但仍可以把这段时间用于其他工作。现在将任务分配给 Copilot 云智能体！

1. 返回 github.com 上的仓库。
2. 选择 **Issues** 选项卡。
3. 选择 **新建议题**，打开新议题对话框。
4. 选择 **空白议题**，使用空白模板。
5. 将 **标题** 设为 `Show related games on the game details page`。
6. 将 **描述** 设为：

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. 选择 **创建**，创建议题。
8. 在右侧选择 **分配给 Copilot**，打开分配对话框。

  ![将议题分配给 Copilot](../../../_images/shared-assign-copilot.png)

9. 选择 **分配**。

不久后，你会在议题的第一条评论中看到一组 👀，表示 Copilot 已开始处理！

![Copilot 使用眼睛表情符号表示正在处理议题](../../../_images/ex4-issue-eyes-emoji.png)

Copilot 正在认真处理第二项请求！Copilot 云智能体的工作方式类似软件工程师，因此你不需要一直监控它；任务完成后再审查即可。现在转而了解如何创建和使用自定义智能体。

## 总结和后续步骤

本课介绍了 [GitHub Copilot 云智能体][copilot-agents]。你可以将议题分配给云智能体，让它异步处理。可以让 Copilot 处理技术债务、开发新功能，或协助将代码从一种框架迁移到另一种框架。

本课探索了以下内容：

- 自定义代码生成环境。
- 确保操作安全。
- 明确界定议题范围的重要性。
- 将议题分配给 Copilot。

Copilot 云智能体正在后台处理任务，现在可以转而创建和使用自定义智能体。[Copilot 云智能体也可以使用 MCP 服务器][cloud-agent-mcp]，并且可以读取自定义指令；我们已在前面的模块中介绍了这些内容。

## 资源

- [关于 Copilot 云智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流的最佳做法][cloud-agent-best-practices]

| [下一课：自定义智能体 →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
