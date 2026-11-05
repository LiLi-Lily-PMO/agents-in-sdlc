---
title: "练习 4 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前两节练习中，你让 Copilot 云智能体承担了三项不同任务，用于改进用户体验和添加功能。云智能体可以异步自主工作，但监控这些任务仍然很重要。

你可以使用多种工具管理分配给云智能体的任务，包括 GitHub.com 上的[智能体页面][agents-page]。在这个控制中心中，你可以查看所有已创建开放拉取请求 (PR) 的智能体任务，探索它执行的操作，甚至引导进行中的会话。

本课将学习如何：

- 浏览智能体页面，监控云智能体任务。
- 引导正在进行的会话，请求添加其他功能。

## 场景

分配高对比度模式任务后，团队意识到也可以顺便添加浅色模式。既然正在更新网站样式并添加切换功能，加入浅色模式就很合理。现在引导智能体，要求它同时添加浅色模式和高对比度模式。

## 查看 Copilot 云智能体任务

查看当前分配给 Copilot 云智能体的所有任务状态。

1. 打开 [Copilot 智能体页面](https://github.com/copilot/agents)。
2. 查看主区域和左侧区域中的任务列表。列表中应包含你分配给 Copilot 的任务，例如：
    - 更新代码库文档。
    - 生成用于修改游戏的 API。
    - 为网站添加高对比度模式。
3. 选择一个正在运行的任务，查看 Copilot 已执行的工作，例如：
    - 从存储库检出代码。
    - 创建 Copilot 工作环境。
    - 设置 MCP 服务器。
    - 执行完成分配任务所需的其他步骤。

> [!NOTE]
> 具体显示的步骤因 Copilot 当前的工作状态和处理方式而异。

4. 同时留意右侧显示的拉取请求 (PR) 面板，以便进一步监控 PR 和更改的文件。

## 引导云智能体

了解正在进行的任务后，现在要求 Copilot 在实现高对比度模式时也添加浅色模式切换功能。

1. 选择与添加高对比度模式相关的会话。确切标题因 Copilot 使用的名称和当前工作状态而异。

    ![控制中心中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 等待几分钟，直到会话显示已完成设置并开始工作。之后会看到类似下方的消息。
3. 在 **Steer active session while Copilot is working** 对话框中，添加以下提示：

    ```plaintext
    添加高对比度模式时，也请添加浅色模式。为此模式提供一个开关，让用户能够选择所需的显示模式。
    ```

    ![智能体页面中的云智能体任务截图，其中突出显示了 Steer active session while Copilot is working 对话框](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 留意 Copilot 如何确认提示并将其纳入工作流程。

## 让 Copilot 完成工作

与之前一样，Copilot 会继续处理更新后的任务！收到消息时，它会先完成当前步骤，然后将新请求纳入后续工作流程。

和之前一样，这需要几分钟。你可以利用这段时间停下来回顾目前学到和探索的内容。

## 总结与后续步骤

本课介绍了 Copilot 智能体页面，这是监控和引导 GitHub Copilot 云智能体任务的中心位置。你可以通过控制中心跟踪所有进行中和已完成的任务、审查正在执行的工作，甚至重定向正在运行的任务以调整范围或提供额外指导。

本课探索了以下概念：

- 使用智能体控制中心和智能体页面监控云智能体任务。
- 重定向正在进行的会话，请求添加其他功能。

Copilot 正在完成功能开发。接下来，我们将[迭代改进 Copilot 创建的拉取请求][next-lesson]。

## 资源

- [Copilot 智能体页面][agents-page]
- [关于自定义智能体][custom-agents]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代改进 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
