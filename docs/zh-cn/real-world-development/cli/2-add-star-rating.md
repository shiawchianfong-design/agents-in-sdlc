---
title: "第 2 课 - 添加星级评分：快速上手"
description: "使用 Copilot CLI 对游戏卡片进行一项小改动，在转发的浏览器中检查结果，并通过第一个拉取请求合并更改。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

安装 Copilot CLI 并尝试过对话后，就可以首次更改项目了。此次改动会保持简单：游戏数据中已有星级评分，但主页的游戏卡片尚未显示。你将让智能体显示评分、审查更改，并通过第一个拉取请求合并更改。

本课将介绍如何：

- 在功能分支上启动专注于任务的 Copilot 对话。
- 请智能体对项目进行一项小改动。
- 使用 `/diff` 审查更改。
- 在转发的浏览器中运行应用并确认更改。
- 创建并合并第一个拉取请求。

## 场景

Tailspin Toys 中的每款游戏都可以有星级评分，该评分已显示在游戏详情页上。但主页上的游戏卡片只显示标题、类别、发行商和说明。作为热身，你将让智能体在每张卡片上显示现有评分。这项小型、独立的更改非常适合作为首次会话任务。

## 对话剖析

**对话**是你通过 Copilot CLI 完成任务的工作区。与 Copilot app 不同，普通 CLI 对话使用终端当前检出的存储库和 Git 分支，而不是创建专用工作树。保存的对话可让你稍后返回同一讨论，而文件和分支仍是磁盘上的普通 Git 状态。

对话中有三类内容：你的提示和智能体回复、智能体探索和编辑文件时的工具活动，以及使用 `/diff` 检查的更改。

## 启动对话并请求更改

新建对话，开始实现功能。

1. 返回 Codespace。
2. 如果终端尚未打开，按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）。
3. 如果 Copilot 尚未运行，使用以下命令启动：

   ```bash
   copilot --yolo
   ```

4. 输入斜杠命令 `/new` 并按 <kbd>Enter</kbd>，确保已启动新会话。
5. 使用以下提示请求更改：

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

Copilot 会探索项目，找到用于显示游戏详情的文件，并创建所需代码。现在，你已通过 Copilot CLI 添加了一项新功能。

## 审查差异

所有 AI 生成的更改在合并前都应接受审查，即使改动很小。下面直接在 Copilot CLI 中检查更改。

1. 输入 `/diff` 并检查每个已更改的文件。
2. 确认游戏卡片会在有评分时显示数值；当 `starRating` 为 `null` 时显示 `No rating yet`。
3. 确认测试覆盖了这两种状态。
4. 查看 Copilot 运行的检查结果，并让它修复失败项。
5. 审查完成后，按 <kbd>Esc</kbd> 退出差异视图。

> [!NOTE]
> Copilot 与所有生成式 AI 工具一样，具有概率性而非确定性，因此实际代码可能会有所不同。请审查其行为，而不是要求输出与某个固定实现完全一致。

## 检查更改

不能只阅读代码就假定它能正常运行。让 Copilot 启动网站，以便在 Codespaces 转发的浏览器中查看更新后的界面。

1. 让 Copilot 启动应用：

   ```plaintext
   Start the app so I can inspect the star-rating change in my browser. Tell me the URL and leave the server running.
   ```

2. 当 Codespaces 报告端口 `4321` 可用时，选择 **Open in Browser**。
3. 确认游戏卡片显示了满分为五分的评分。
4. 返回 Copilot，让它停止刚才启动的服务器：

   ```plaintext
   Stop the development server you started.
   ```

## 创建并合并第一个拉取请求

功能已完成。接下来创建拉取请求 (PR)，将新代码合并到项目中。

1. 让默认智能体提交更改：

   ```plaintext
   Commit the reviewed star-rating changes with an appropriate commit message.
   ```

2. 输入 `/pr create`。Copilot CLI 会在创建 PR 时推送现有提交，并显示 PR URL。
3. 按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），然后选择 Copilot CLI 显示的 URL，以打开 PR。
4. 审查已更改的文件和检查结果。
5. 准备好后，选择 **Merge pull request**，然后确认合并。
6. 返回 Codespace，输入 `/exit` 退出 Copilot CLI。
7. 更新本地 `main` 分支：

   ```bash
   git checkout main
   git pull
   ```

## 总结与后续步骤

恭喜！你已使用 GitHub Copilot CLI 交付首次更改。本课中，你：

- 在功能分支上启动了专注于任务的 Copilot 对话。
- 让智能体对游戏卡片进行一项小改动。
- 使用 `/diff` 审查更改。
- 在转发的浏览器中运行应用并确认星级评分。
- 创建并合并了第一个拉取请求。

接下来，你将[从筛选功能议题开始，并使用 Plan 和 Autopilot 模式][next-lesson]构建更大的功能。

## 资源

- [关于 GitHub Copilot CLI][about-copilot-cli]
- [Copilot CLI 命令参考][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
