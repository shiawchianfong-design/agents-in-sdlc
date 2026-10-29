---
title: "练习 5 - 监视和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义智能体][previous-lesson] |
|:--|

当 GitHub Copilot 处于智能体模式时，它会自主探索代码库、提出更改建议、编辑文件并运行命令。由于这些工作会实时发生在你的计算机上，VS Code 中的 Copilot Chat 会持续显示每次工具调用和文件编辑。你可以直接在编辑器中审查每项差异、接受或拒绝单项更改，还可以通过后续提示引导对话，在不离开编辑器的情况下完善或扩展工作。

本练习将：

- 实时监视无障碍智能体的工作。
- 审查并接受建议的文件编辑。
- 在会话进行期间引导智能体，添加与高对比度模式并列的浅色模式切换功能。

## 场景

你在[练习 4][previous-lesson]中使用的无障碍自定义智能体正在实现高对比度模式。工作进行期间，设计团队又提出一项请求：他们还希望添加浅色模式切换功能，供认为默认主题过暗的用户使用。无需从头开始新对话，可以引导现有会话扩展工作范围。

## 监视 Copilot 的进度

Copilot Chat 视图会显示智能体执行的每个步骤，包括探索的文件、运行的命令和提出的编辑。每项文件编辑都会以内联方式显示在 Chat 中，并提供 **Keep** / **Undo** 控件。

1. 返回练习 4 中使用的 Copilot Chat 视图。
2. 智能体工作时，滚动查看每次工具调用。你会看到文件读取、文件编辑，以及它请求运行的终端命令。
3. 智能体提出文件编辑时，差异会以内联方式显示：
   - 选择 **Keep** 接受更改。
   - 选择 **Undo** 撤销更改，让智能体重新尝试。
4. 也可以打开 Chat 顶部的 **Files changed** 指示器，查看智能体在本次会话中修改的完整文件列表。

> [!NOTE]
> 如果工具调用请求权限（例如运行终端命令），智能体会暂停并显示 **Allow** 提示。只批准你信任的命令。

5. 等待智能体完成高对比度模式。完成后应会显示一条消息，总结所做的更改。

## 引导智能体添加浅色模式

高对比度模式完成后，可以在同一对话中添加浅色模式切换功能。继续使用同一 Chat 会话时，智能体会保留之前工作的上下文，包括修改的组件、使用的样式方案和本地存储模式。

1. 在 Copilot Chat 输入框中发送以下后续提示：

    ```plaintext
    Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
    ```

2. 智能体会接续之前的工作，探索已做的更改并提出浅色模式的修改方案。
3. 与高对比度模式一样，逐项审查新编辑。使用 **Keep** 或 **Undo** 处理每项差异。
4. 如果智能体的实现方向不符合预期（例如尝试重构切换 UI），可以选择 Chat 输入框旁边的 **Stop** 按钮停止任务，然后发送后续提示说明需求。

> [!TIP]
> 引导正在进行的会话比从头开始更快。可以使用后续提示提出调整（例如“简化 CSS，改用 CSS 变量，不要重复选择器”）、纠正方向（例如“不要添加第三个切换项，把浅色模式集成到现有设置面板”），或扩展范围（例如“为两个切换项都添加键盘快捷键”）。

## 审查工作集

提交工作之前，快速检查智能体修改的所有内容。

1. 在 VS Code 中打开 **Source Control** 视图。
2. 查看已更改文件的完整列表。应能看到 Astro 组件、样式和相关测试的更新。
3. 打开几个已更改文件并查看差异。确认其中包含自定义智能体要求的无障碍模式，包括 ARIA 属性、键盘导航、语义化 HTML 和本地存储持久化。
4. 确认满意后，在 Source Control 面板中暂存并提交更改。下一课会发布这些更改。

## 总结和下一步

你使用 Copilot Chat 智能体模式监视并引导正在进行的会话：逐项审查文件编辑、接受或拒绝更改，并通过后续提示在保留上下文的同时扩展工作。

本课介绍了以下概念：

- 实时监视智能体的工作。
- 接受或拒绝单项文件编辑。
- 使用后续提示引导正在进行的会话。

在[下一课][next-lesson]中，你会将无障碍工作发布为拉取请求，并审查本学习路径中本地自定义智能体生成的更改。

## 资源

- [VS Code 中的 Copilot Chat][copilot-chat-vscode]
- [使用智能体模式][agent-mode]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代改进 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
