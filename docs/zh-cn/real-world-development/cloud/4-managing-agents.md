---
title: "练习 4 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前几次练习中，你让 Copilot 云智能体处理了三个不同任务，以改进用户体验并添加功能。云智能体虽然能够自主异步工作，但监控任务仍然很重要。

你可以使用多种工具管理分配给云智能体的任务，包括 GitHub.com 上的[智能体页面][agents-page]。在这个任务控制中心，可以查看所有拥有未关闭拉取请求的智能体任务，查看它们执行的操作，甚至引导正在进行的会话。

本课将完成以下内容：

- 浏览智能体页面，监控云智能体任务。
- 引导正在进行的会话，请求添加其他功能。

## 场景

将任务分配给智能体以创建高对比度模式后，团队意识到也应该添加浅色模式。网站正在更新样式并添加切换功能，因此加入此功能也很合理。现在需要引导智能体，确保它同时添加浅色模式和高对比度模式。

## 查看 Copilot 云智能体任务

查看目前分配给 Copilot 云智能体的所有任务的状态。

1. 打开 [Copilot 智能体页面](https://github.com/copilot/agents)。
2. 查看主面板和左侧面板中的任务列表。列表中应包含分配给 Copilot 的任务，例如：
    - 更新代码库文档。
    - 生成用于修改游戏的 API。
    - 为网站添加高对比度模式。
3. 选择一个正在运行的任务，查看 Copilot 已执行的工作。可能包括：
    - 从存储库检出代码。
    - 为 Copilot 创建工作环境。
    - 设置 MCP 服务器。
    - 执行完成所分配任务的各种操作。

> [!NOTE]
> 具体步骤会根据 Copilot 当前的工作状态和采用的方法而有所不同。

4. 还要查看右侧显示的拉取请求（PR）面板。可以通过它查看 PR 和更改的文件，以便进一步监控。

## 引导云智能体

现在已经了解正在进行的任务，可以请求 Copilot 在实现高对比度模式时同时添加浅色模式。

1. 选择用于添加高对比度模式的会话。确切标题会根据 Copilot 使用的名称和当前工作状态而有所不同。

    ![任务控制中心中的无障碍访问会话](../../../_images/ex6-accessibility-session.png)

2. 观察会话几分钟，直到会话显示已完成设置并开始工作。出现类似下方的消息时，就表示已开始工作。
3. 在 **Steer active session while Copilot is working** 对话框中输入以下提示：

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![智能体页面中的云智能体任务截图，突出显示 Steer active session while copilot is working 对话框](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 留意 Copilot 如何确认提示并将其纳入工作流程。

## 让 Copilot 完成工作

与之前一样，Copilot 会继续处理更新后的任务！在完成收到消息时正在执行的具体步骤后，它会将新请求加入工作流程。

整个过程还需要几分钟。可以利用这段时间回顾目前为止学到和探索的内容。

## 总结和后续步骤

本课介绍了 Copilot 智能体页面，这是监控和引导 GitHub Copilot 云智能体任务的中心位置。在此任务控制中心，可以跟踪所有进行中和已完成的任务、审查工作，并重定向正在进行的任务以调整范围或提供额外指导。

本课探索了以下内容：

- 浏览任务控制中心和智能体页面，监控云智能体任务。
- 重定向正在进行的会话，请求添加其他功能。

Copilot 正在完成无障碍访问功能。接下来，[迭代优化 Copilot 创建的拉取请求][next-lesson]。

## 资源

- [Copilot 智能体页面][agents-page]
- [关于自定义智能体][custom-agents]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代优化 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
