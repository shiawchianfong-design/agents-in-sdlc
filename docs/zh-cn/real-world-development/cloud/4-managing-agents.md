---
title: "练习 4 - 监视和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前几项练习中，你让 Copilot cloud agent 承担了三项不同任务，改善用户体验并添加功能。cloud agent 虽然可以异步自主工作，但监视这些任务仍然很重要。

有多种工具可以管理分配给 cloud agent 的任务，包括 GitHub.com 上的[智能体页面][agents-page]。在这个任务控制中心，可以查看所有具有未关闭拉取请求 (PR) 的智能体任务，了解已执行的操作，还可以引导正在进行的会话。

本课将：

- 查看智能体页面，监视 cloud agent 的任务。
- 在会话进行期间引导代理，请求添加其他功能。

## 场景

将高对比度模式任务分配给代理后，团队意识到也可以顺便添加浅色模式。网站样式和切换功能正在更新，因此加入这项功能很合理。现在需要引导代理，让它同时添加浅色模式和高对比度模式。

## 查看 Copilot cloud agent 任务

查看当前分配给 Copilot cloud agent 的所有任务的状态。

1. 转到 [Copilot agents 页面](https://github.com/copilot/agents)。
2. 留意主窗格和左侧窗格中的任务列表。这里应显示分配给 Copilot 的任务，包括：
   - 更新代码文档。
   - 生成用于修改游戏的 API。
   - 为网站添加高对比度模式。
3. 选择一个正在运行的任务，查看 Copilot 已执行的操作，包括：
   - 从存储库检出代码。
   - 创建 Copilot 的工作环境。
   - 设置 MCP 服务器。
   - 完成任务所需的其他步骤。

> [!NOTE]
> 列出的具体步骤会根据 Copilot 的工作状态和执行方式而变化。

4. 同时留意右侧出现的拉取请求 (PR) 窗格。在此可以查看 PR 和更改的文件，以便进一步监视。

## 引导 cloud agent

查看正在进行的任务后，现在让 Copilot 在处理高对比度模式时也添加浅色模式切换功能。

1. 选择与添加高对比度模式相关的会话。确切的标题会因 Copilot 选择的名称和当前工作状态而异。

    ![任务控制中心中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 观察会话几分钟，直到显示设置已完成并开始执行任务。出现与下方类似的消息时，就说明已经开始工作。
3. 在 **Steer active session while Copilot is working** 对话框中，添加以下提示：

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Copilot 智能体页面上的任务，突出显示 Steer active session while copilot is working 对话框](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 留意 Copilot 如何确认提示并将其纳入工作流程。

## 等待 Copilot 完成工作

和之前一样，Copilot 会开始处理更新后的任务！它会先完成发送消息时正在执行的步骤，再将新请求纳入工作流程。

与之前一样，这需要几分钟。此时可以暂停一下，回顾目前学到和实践的内容。

## 总结和下一步

本课介绍了 Copilot agents 页面，它是监视和引导 GitHub Copilot cloud agent 任务的中心控制台。使用此控制台可以跟踪所有活动和已完成的任务、查看正在进行的工作，还可以调整正在进行的会话。

你了解了以下概念：

- 使用任务控制中心和智能体页面监视 cloud agent 任务。
- 重定向正在进行的会话，请求添加其他功能。

Copilot 正在完成无障碍功能，接下来将[迭代改进 Copilot 创建的拉取请求][next-lesson]。

## 资源

- [Copilot agents 页面][agents-page]
- [关于自定义智能体][custom-agents]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代改进 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
