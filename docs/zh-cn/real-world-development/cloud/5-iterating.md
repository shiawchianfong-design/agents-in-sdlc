---
title: "练习 5：迭代改进 GitHub Copilot 的工作"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

## 审查工作

在本学习路径中，你已与 GitHub Copilot 合作完成多项任务，以改善用户体验并添加功能：为代码添加文档、构建供设计团队迭代改进的相关游戏功能，以及实现包含高对比度和浅色模式切换的无障碍功能。现在来查看代码更改；必要时向 Copilot 提供反馈，进一步改进成果。

### 场景

如前所述，引入生成式 AI 并不会改变软件设计和 DevOps 的基本原则。我们始终需要审查生成的代码，并遵循常规 DevOps 流程。接下来，在为团队其他成员启用审查之前，先审查 GitHub Copilot 对文档、相关游戏功能和无障碍功能的建议。

## 安全性与 GitHub Copilot cloud agent

Copilot cloud agent 会在无人监督的情况下异步执行任务，因此它受到一些安全限制，以确保操作安全。这些限制包括：

- Copilot 对存储库只有读取权限，对用于处理代码的分支才有写入权限。
- Cloud agent 在 GitHub Actions 中运行，并在单独的临时环境中工作。
- 任何 GitHub Actions 工作流都必须经过人工批准后才能运行。
- [默认情况下，外部资源访问受到限制](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)，MCP 服务器也包括在内。

## 审查生成的文档

先通过 GitHub.com 的标准 PR 界面，查看 GitHub Copilot cloud agent 创建的第一个拉取请求 (PR)：为代码添加文档。

> [!NOTE]
> 查看 PR 时，可能会看到 GitHub Copilot 被防火墙阻止的警告。这是**预期行为**，因为 Copilot 默认只能有限访问外部资源，包括外部 MCP 服务器调用。如有需要，可以[自定义或禁用 Copilot cloud agent 防火墙][agent-firewall]。

1. 返回 github.com 上的存储库。
2. 选择 **Pull Requests**，打开拉取请求列表。
3. 打开标题类似 **Add missing documentation** 或更完整标题的拉取请求。

> [!NOTE]
> 如果 Copilot 仍在处理任务，拉取请求会带有 **[WIP]** 标记。请等待 Copilot 完成任务。这可能需要几分钟；你可以休息一下，或回顾目前学到的内容。

4. PR 准备就绪后，选择 **Files changed** 选项卡并查看更改。

    ![Files changed 选项卡](../../../_images/shared-pr-files-changed.png)

5. 查看新添加了 TSDoc 文档注释和其他文档的代码。具体更改会有所不同。

    查看更改时，留意 TSDoc 文档注释、TypeScript 规范和文件头注释。这些要求来自练习 2 开始时查看的自定义指令文件。

6. 确认更新符合预期后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有等待批准的工作流提示。
8. 如果有等待批准的工作流，选择 **Approve and run workflows**。

   ![批准并运行工作流](../../../_images/shared-approve-workflows.png)

9. 你应能看到工作流已加入 PR 的检查队列。如果一切正常，这款 Astro 单体应用的项目检查就会通过。完成检查可能需要几分钟。

## 请求 GitHub Copilot 更改

与 Copilot 协作并非单向过程。你可以像 @提及团队成员一样，在拉取请求评论或代码行内评论中提及 Copilot。Copilot 会看到这些评论，并启动新的会话来处理请求。由于结果具有非确定性，此处无法规定具体的请求文本。可以尝试要求 Copilot：

- 在每个代码文件开头添加简短的文件头注释，说明文件用途。
- 为 TypeScript 和 Astro 文件添加 TSDoc 文档注释。
- 创建 README，介绍 Astro 应用的结构。

1. 添加评论，请求修改生成的文档，并像提及其他用户一样提及 **@copilot**。可以使用上面的任一想法，也可以针对代码库中的文档提出其他建议。
2. 选择 **View Session**，观察 Copilot 执行修改。留意 Copilot 如何启动新会话来应用更改。
3. 选择 **Back to pull request**，返回拉取请求。

    ![返回拉取请求](../../../_images/ex7-back-to-pr.png)

4. Copilot 完成更改后，拉取请求中应会出现新的提交。
5. 选择 **Files changed** 选项卡，审查更改。

如果对结果还不满意，可以继续迭代。满意后，可以将 PR 从草稿转换为可审查状态，然后合并到主分支。

![将 PR 转为可审查状态](../../../_images/ex7-ready-for-review.png)

## 审查相关游戏功能

现在来查看 Copilot 为解决游戏详情页显示相关游戏这一议题所创建的 PR。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Show related games on the game details page** 或更完整标题的 PR。
4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 确认更新符合预期后，返回 **Conversation** 选项卡并向下滚动。
6. 查看是否有等待批准的工作流提示。
7. 如果有等待批准的工作流，选择 **Approve and run workflows**。

   ![批准并运行工作流](../../../_images/shared-approve-workflows.png)

8. 你应能看到工作流已加入 PR 的检查队列。如果一切正常，这款 Astro 单体应用的项目检查就会通过。完成检查可能需要几分钟。
9. **可选：**也可以在 Codespace 中切换到此分支，手动测试相关游戏功能。转到 Codespace，打开终端，并运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称，例如 **copilot/fix-8**）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot 已实现相关游戏功能！和之前一样，可以通过迭代请求 Copilot 更新，例如调整显示的相关游戏数量，或添加文件头注释和 TSDoc 文档注释（请记住，分配此任务时，你还没有更新自定义指令）。与之前一样，可以在 **Conversation** 选项卡中发表评论；Copilot 会看到评论并启动新会话。

## 审查无障碍功能

最后，审查使用无障碍自定义智能体实现的功能。这个 PR 应包含练习 3 中分配的高对比度模式，以及练习 4 中在任务控制中心请求的浅色模式。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Add high contrast mode to website** 或更完整标题的 PR。

> [!NOTE]
> 如果 Copilot 仍在处理任务，拉取请求会带有 **[WIP]** 标记。请等待 Copilot 完成任务，这可能需要几分钟。

4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 查看实现，重点关注：
   - 用于切换模式的 UI 组件。
   - 使用本地存储持久保存用户偏好的方式。
   - 高对比度和浅色模式的 CSS 或样式更改。
   - 无障碍属性（ARIA 标签、键盘导航等）。
   - 管理模式切换的 JavaScript/TypeScript 代码。

6. 确认更新符合预期后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有等待批准的工作流提示。
8. 如果有等待批准的工作流，选择 **Approve and run workflows**。

   ![批准并运行工作流](../../../_images/shared-approve-workflows.png)

9. 你应能看到工作流已加入 PR 的检查队列。如果一切正常，这款 Astro 单体应用的项目检查就会通过。完成检查可能需要几分钟。
10. **可选：**可以在 Codespace 中切换到此分支，手动测试无障碍功能。转到 Codespace，打开终端，并运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    然后启动应用，在浏览器中测试高对比度和浅色模式切换，确认它们可以正常运行，并且页面重新加载后仍会保留设置。

留意无障碍自定义智能体如何引导 Copilot 遵循无障碍最佳实践来实现这些功能。如果发现无障碍方面的问题或改进空间，可以像对之前的 PR 一样，在评论中提及 **@copilot** 并请求更新。

## 可选练习 — 继续委派任务

Cloud agent 最适合处理实际积压工作，让你能专注于其他任务。可以在自己的存储库中创建更多议题并分配给 Copilot，培养这种工作方式。例如：

- 在游戏详情页创建支持者意向表单。
- 在游戏列表页实现分页。
- 为数据访问辅助函数添加输入验证和错误处理。

## 总结

你已完成 Cloud agent 学习路径。通过这些课程，你：

- **查看了存储库自带的自定义指令文件**，以便之后观察它们对 cloud agent 输出的影响。
- **将议题分配给 Copilot cloud agent**，观察它设置环境、制定计划并异步执行任务。
- **创建并使用自定义智能体**，实现无障碍功能，添加高对比度和浅色模式切换。
- **使用 Copilot agents 页面作为任务控制中心**，监视并引导无障碍会话。
- **审查并迭代改进 cloud agent 的拉取请求**，通过提及 `@copilot` 请求更改，并批准工作流。

## 回顾和下一步

你已完成 Cloud agent 学习路径。如果想继续探索，其他学习路径会以不同方式介绍这些实践：

- 🖥️ **[VS Code 学习路径](../../vscode/)** — 直接在 IDE 中探索 Copilot Chat 智能体模式和 MCP 集成。
- 💻 **[CLI 学习路径](../../cli/)** — 在终端中交付两项经过审查的更改，使用计划模式和自动模式、智能体技能、自定义 QA 智能体、Playwright MCP、Agent Merge，以及可选的 `/delegate` 桥接功能，将工作交回 cloud agent。

在自己的存储库中，还可以尝试以下后续任务：

- 将重构或测试覆盖率任务分配给 cloud agent。
- 为其他领域创建自定义智能体。
- 为不同技术栈设置 `copilot-setup-steps.yml`。

## 资源

- [GitHub Copilot][github-copilot]
- [关于 Copilot 智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot cloud agent 设置工作流最佳实践][cloud-agent-best-practices]
- [配置 Copilot cloud agent 防火墙][agent-firewall]

---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
