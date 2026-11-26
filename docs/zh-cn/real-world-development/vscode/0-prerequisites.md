---
title: "练习 0：先决条件"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

开始 VS Code 练习前，需要完成准备工作。你将创建 Tailspin Toys 仓库的个人副本、启动一个用于开发的 [Codespace][codespaces]，并确认编辑器中的 GitHub Copilot Chat 正常运行。

## 设置实验仓库

要为实验代码创建仓库副本，请从[模板仓库][template-repository]创建一个实例。新实例将包含实验所需的全部文件，你将在后续练习中使用它。

1. 在新的浏览器窗口中，打开 [Tailspin Toys 模板][tailspin-template]。
2. 在实验仓库页面上选择 **使用此模板** 按钮，然后选择 **创建新仓库**，创建仓库副本。

    ![“使用此模板”按钮](../../../_images/ex0-use-template.png)

3. 如果你是在 GitHub 或 Microsoft 举办的活动中参加研讨会，请按照导师提供的说明操作。否则，可以在你有权使用 GitHub Copilot 的组织中创建新仓库。

    ![输入仓库模板设置](../../../_images/ex0-repository-settings.png)

4. 记下所创建仓库的路径（**organization-or-user-name/repository-name**），稍后会在实验中用到。

> [!NOTE]
> **待办事项已准备就绪**
>
> 从模板创建仓库时，系统会自动为你创建一组 GitHub 议题。整个研讨会都会围绕这些议题开展，无需自行创建。

## 创建 Codespace

接下来，使用 Codespace 完成实验练习。

[GitHub Codespaces][codespaces] 是一种基于云的开发环境，可让你直接在浏览器中编写、运行和调试代码。它提供功能齐全的 IDE，支持多种编程语言、扩展和工具。

1. 打开刚创建的仓库。
2. 选择绿色的 **Code** 按钮。

    ![选择 Code 按钮](../../../_images/ex0-code-button.png)

3. 选择 **Codespaces** 选项卡，然后选择 **+** 按钮创建新的 Codespace。

    ![创建新的 Codespace](../../../_images/ex0-create-codespace.png)

创建 Codespace 需要几分钟，但仍比手动安装所有服务快得多！你可以利用这段时间了解 GitHub Copilot 的其他功能，接下来我们会介绍这些内容。

> [!CAUTION]
> 后续练习还会用到此 Codespace。现在请先让它保持打开状态，并留在浏览器选项卡中。

> [!NOTE]
> 本研讨会可在 Codespace 或本地 [dev container][dev-containers] 中运行。两者都会确保已安装所有必需的先决条件，带来顺畅的体验。如果想在本地运行，请在 VS Code 中打开克隆的仓库；出现提示时选择 **在容器中重新打开**，VS Code 会构建 Codespace 使用的同款 dev container。

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## 使用 GitHub Copilot Chat 和智能体模式

若要使用 GitHub Copilot Chat 智能体模式，需要在 IDE 中安装 GitHub Copilot Chat 扩展。如果使用 GitHub Codespace，通常已经安装该扩展。

> [!TIP]
> 如果尚未安装 GitHub Copilot Chat 扩展，可以从 [Visual Studio Code Marketplace][copilot-chat-extension] 安装。也可以在 Visual Studio Code 中打开扩展视图，搜索 **GitHub Copilot Chat** 并选择 **安装**。

安装扩展后，可能需要使用 GitHub 帐户进行身份验证才能启用它。

1. 返回 Codespace。
2. 如果编辑器右侧没有显示 Copilot Chat，请选择 Codespace 顶部的 **Copilot Chat** 图标。
3. 在 Copilot Chat 窗口中输入类似 “Hello world” 的消息，然后按 <kbd>Enter</kbd>。这应会启动 Copilot Chat。
4. 如果尚未通过身份验证，系统会提示你登录 GitHub 帐户。按照说明完成身份验证。

    ![Copilot Chat 身份验证提示示例](../../../_images/ex1-copilot-authentication.png)

5. 身份验证完成后，Copilot Chat 窗口应会显示出来。

## 总结

恭喜！你已创建实验仓库的副本，并开始创建 Codespace。开始编写代码时，你会使用这个环境。

## 下一步

现在让 Copilot 开始工作。继续学习[练习 1 - 自定义指令][next-lesson]，向 Copilot 介绍项目规范。

## 资源

- [GitHub Codespaces 概述][codespaces]
- [从模板创建仓库][template-repository]
- [开始使用 Codespaces][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[copilot-chat-extension]: https://marketplace.visualstudio.com/items?itemName=GitHub.copilot-chat
[next-lesson]: ../1-custom-instructions/
