---
title: "练习 3 - 自定义智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## 什么是自定义智能体？

GitHub Copilot 中的[自定义智能体][custom-agents-concept]是针对开发工作流中特定任务或领域创建的专用 AI 助手。你可以通过仓库 `.github/agents` 文件夹中的 Markdown 文件定义智能体，为 Copilot 提供有针对性的指令、最佳做法、编码模式和领域知识，帮助它更有效地完成特定类型的工作。团队可以将专业知识整理成可复用的智能体，例如执行 [WCAG][wcag] 合规检查的无障碍智能体、遵循安全编码实践的安全智能体，或维护一致测试模式的测试智能体。

自定义智能体由项目 `.github/agents` 文件夹或全局 `~/.copilot/agents` 中的 Markdown 文件定义。每个文件都包含 YAML frontmatter，至少需要 `name` 和 `description`，后面是用于定义智能体行为、专长和指令的 Markdown 提示。

### 自定义智能体与智能体技能的比较

自定义智能体和[智能体技能][agent-skills-concept]有些相似之处。两者主要通过 Markdown 文件定义，并告诉 AI 如何执行操作。最简单的区分方式是：**自定义智能体**是执行工作的人员，**技能**是工具。

自定义智能体有自己的上下文窗口，并负责协调技能（甚至其他智能体）来完成工作。在本实验中，无障碍自定义智能体会根据无障碍指南审查并更新网站；处理过程中，它可以调用拉取请求工作流技能或运行和管理测试的技能。

> [!NOTE]
> 自定义智能体没有唯一正确的编写方式。与 AI 的其他用法一样，需要在自己的环境和场景中测试并不断迭代。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

本练习将了解自定义智能体的以下用法：

- 了解自定义智能体的定义方式。
- 将任务分配给自定义智能体。

## 场景

Tailspin Toys 致力于确保所有用户都能访问其众筹平台，无论他们的视觉能力或偏好如何。近期用户反馈指出，目前的暗色主题因文字和背景颜色对比度不足而难以阅读。为解决这一无障碍问题，设计团队要求实现可由用户开启或关闭的高对比度模式。

由于无障碍访问至关重要，你希望尽快实现此功能。因此将使用自定义智能体生成相应功能。

## 查看无障碍自定义智能体

我们已为你创建了一个无障碍自定义智能体。先查看其内容，了解它将如何指导 Copilot。

返回 Codespace，然后检查无障碍自定义智能体文件：

1. 打开 `.github/agents/accessibility.md`。
2. 注意包含 `name` 和 `description` 字段的 YAML frontmatter。

> [!CAUTION]
> `name` 和 `description` frontmatter 是自定义智能体的必需项。

3. 接着浏览后续部分，了解其中介绍的内容：
   - 为无障碍网站生成代码时的核心职责。
   - 无障碍最佳做法。
   - HTML、CSS 和 JavaScript 代码示例。
   - 常见问题和错误列表。

## 创建并分配议题

智能体控制中心是管理环境中所有智能体的中心位置。你可以在此处将任务分配给 Copilot 云智能体、监控任务，甚至重定向并提供补充指导。现在将创建高对比度模式的任务分配给 Copilot。

1. 打开仓库。
2. 选择 **Issues** 选项卡。
3. 选择 **新建议题**，打开新议题对话框。
4. 选择 **空白议题**，创建新议题。
5. 将 **标题** 设为 `Add high contrast mode to website`。
6. 将 **描述** 设为：

    ```plaintext
    We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
    ```

7. 选择 **创建**，创建议题。
8. 在右侧选择 **分配给 Copilot**，打开分配对话框。
9. 从自定义智能体列表中选择 **Accessibility agent**。

    ![Copilot 云智能体分配界面，突出显示自定义智能体和 Accessibility](../../../_images/ex5-select-custom-agent.png)

10. 选择 **分配**。
11. Copilot 将在后台开始处理任务！

## 总结和后续步骤

本课介绍了 GitHub Copilot 中的[自定义智能体][custom-agents]，也就是为特定任务和领域设计的专用 AI 助手。借助自定义智能体，你可以将团队的专业知识和标准整理成可复用的智能体，指导 Copilot 更有效地完成特定类型的工作。

本课探索了以下内容：

- 了解自定义智能体的定义方式。
- 将任务分配给自定义智能体。

Copilot 正在实现高对比度模式。接下来，我们将了解如何从控制中心[监控并引导智能体会话][next-lesson]。

## 资源

- [关于自定义智能体][custom-agents]
- [为组织使用自定义智能体做好准备][org-custom-agents]
- [为企业使用自定义智能体做好准备][enterprise-custom-agents]

---

| [← 上一课：GitHub Copilot 云智能体][previous-lesson] | [下一课：监控和管理智能体 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
