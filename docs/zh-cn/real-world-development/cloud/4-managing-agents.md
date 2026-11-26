---
title: "练习 4 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前两项练习中，你让 Copilot 云智能体处理了三个不同任务，用于改善用户体验并添加功能。云智能体虽然能够异步自主工作，但监控这些任务仍然很重要。

你可以使用多种工具管理分配给云智能体的任务，包括 GitHub.com 上的[智能体页面][agents-page]。在这个控制中心，你可以查看所有具有未关闭拉取请求（PR）的智能体任务，探索它执行的操作，甚至引导进行中的会话。

本课将学习如何：

- 使用智能体页面监控云智能体任务。
- 在会话进行中引导智能体，要求它添加功能。

## 场景

将任务分配给智能体以创建高对比度模式后，团队意识到此时也适合添加浅色模式。由于智能体已经在更新网站样式并添加切换功能，因此同时实现浅色模式也很合理。现在，你将引导智能体的工作，确保它同时添加浅色和高对比度模式。

## 查看 Copilot 云智能体任务

先查看分配给 Copilot 云智能体的所有任务的当前状态。

1. 打开 [Copilot 智能体页面](https://github.com/copilot/agents)。
2. 查看主窗格和左侧窗格中的任务列表。你应该能看到已分配给 Copilot 的任务，包括：
    - 更新代码文档。
    - 生成用于修改游戏的 API。
    - 为网站添加高对比度模式。
3. 选择一个正在运行的任务，查看 Copilot 已执行的操作，包括：
    - 从仓库中检出代码。
    - 为 Copilot 创建工作环境。
    - 设置 MCP 服务器。
    - 执行完成分配任务所需的各种步骤。

> [!NOTE]
> 具体显示的步骤会因 Copilot 当前的工作状态和处理方式而异。

4. 同时留意右侧出现的拉取请求（PR）窗格。你可以在那里查看 PR 和已更改的文件，以便进一步监控。

## 引导云智能体

现在你已了解当前活动任务，可以要求 Copilot 在处理高对比度模式的同时添加浅色模式。

1. 选择与添加高对比度模式相关的会话。确切标题会因 Copilot 使用的名称和当前工作状态而异。

    ![控制中心中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 观察会话几分钟，直到 Copilot 完成设置并开始工作。出现类似下方的消息时，就说明已经开始处理。
3. 在 **Copilot 工作时引导活动会话** 对话框中，添加以下提示：

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![智能体页面中的云智能体任务截图，突出显示“Copilot 工作时引导活动会话”对话框](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 留意 Copilot 如何确认收到提示并将其纳入工作流程。

## 让 Copilot 继续工作

与之前一样，Copilot 会继续处理更新后的任务！在完成收到消息时正在执行的具体步骤后，它会把新请求纳入工作流程。

和之前一样，这需要几分钟。你可以趁此机会停下来，回顾目前学到和探索的内容。

## 总结和后续步骤

本课介绍了 Copilot 智能体页面，这是监控和引导 GitHub Copilot 云智能体任务的中心位置。在此控制中心，你可以跟踪所有活动和已完成的任务、审查正在执行的工作，甚至重定向进行中的任务以调整范围或补充指导。

本课探索了以下内容：

- 使用控制中心和智能体页面监控云智能体任务。
- 重定向进行中的会话，要求添加功能。

Copilot 正在完成无障碍功能的工作。接下来，我们将[迭代 Copilot 创建的拉取请求][next-lesson]。

## 资源

- [Copilot 智能体页面][agents-page]
- [关于自定义智能体][custom-agents]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
