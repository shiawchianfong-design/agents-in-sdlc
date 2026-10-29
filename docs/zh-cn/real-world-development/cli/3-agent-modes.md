---
title: "第 3 课 - 智能体模式：Plan 和 Autopilot"
description: "使用 Plan 确定方案、使用 Autopilot 根据议题构建筛选功能，再使用 Interactive 审查结果。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

我们先为项目添加了一项小功能，但更大的更改需要更完善的流程。GitHub Copilot CLI 支持组织现有的工作流，帮助我们用正确的方法构建所需功能。从本课开始，你将通过几节课遵循典型的智能体驱动开发流程：从议题开始实现新功能，确保代码有效、行为符合预期，最后成功合并到项目中。

> [!NOTE]
> 继续完成此功能时，将沿用同一段对话和同一个分支。通常，不同类型的文件会使用不同分支或 PR；此处采用简化流程，以便专注于核心概念。

本课将介绍如何：

- 从 GitHub 议题启动新的 Copilot 对话。
- 在 Plan 模式中定义需求。
- 使用 Autopilot 模式实现新功能。
- 审查代码。
- 在转发的浏览器中手动验证功能。

继续完成此功能时，你将更新存储库指令、自定义现有的 `quality-checks` 技能、添加 MCP 验证、创建 QA 智能体并创建功能 PR。

## 场景

Tailspin Toys 的游戏目录不断扩大，访客需要按类别和发行商缩小游戏范围。待办议题描述了这项功能，但在编码前，需要就组合类别等细节达成共识。你将使用 Plan 模式明确这些决定，然后授权 Autopilot 在指定范围内实现功能。

## 背景

将 AI 编码智能体引入开发流程不会改变基本原则。事实上，这些原则反而更加重要。大多数开发人员遵循类似以下的流程：

1. 从描述需求的已创建议题开始。
2. 规划需要构建的内容。
3. 构建并审查代码。
4. 运行测试以验证代码。
5. 手动验证新功能。
6. 创建拉取请求 (PR)。
7. 代码通过审查且持续集成流程成功后，合并代码。

> [!NOTE]
> 具体流程会因团队和组织而异，但大多数流程都是以上做法的变体。

坚持这种标准方法，可以确保 AI 生成的代码满足既定要求，并经过与手写代码相同的审查流程。

## 对话模式

**对话模式**控制智能体的自主程度。按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 可循环切换模式：

- **Interactive**：你与智能体协同工作。智能体提出更改建议，并等待你的输入后再继续。
- **Plan**：智能体先创建计划，但不能编辑项目文件。
- **Autopilot**：智能体自主工作，包括编写代码、运行测试并不断迭代，直至任务完成。

先在 Plan 模式中审查计划，再使用 Autopilot 实现。

## 从议题开始

开始筛选功能工作前，返回 Codespace，确认存储库和终端已准备就绪。

1. 返回 Codespace。如果它已停止，请重新启动。
2. 确认星级评分 PR 已合并。
3. 如果终端尚未打开，按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）。
4. 更新 `main`，然后为筛选功能创建分支：

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. 启动 Copilot CLI：

   ```bash
   copilot --yolo
   ```

6. 按两次 <kbd>Tab</kbd> 打开 **Issues** 选项卡。
7. 按 <kbd>A</kbd> 显示所有议题。
8. 使用方向键选中标题为 **Allow users to filter games by category and publisher** 的议题。
9. 按 <kbd>C</kbd> 将议题添加到提示中，并返回 **Session** 选项卡。

现在，提示应以 `#7`（或类似编号）开头。`#` 可将 GitHub 上的议题或拉取请求 (PR) 加入上下文。

## 规划筛选功能

规划可以让你在交给 Copilot 实现前确定功能方案。对于复杂任务，花些时间规划通常很有帮助。切换到 Plan 模式，并要求 Copilot 制定计划。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 切换到 Plan 模式。确认提示框下方的模式指示器显示 **Plan**。
2. 在上一步添加的议题引用后输入以下提示：

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot 会先探索项目，再确定合适的实现方案。

3. Copilot 可能会询问筛选功能应如何工作。根据自己的偏好回答；不存在唯一正确的答案。
4. 计划完成后，按 <kbd>Control</kbd>+<kbd>E</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>E</kbd>（Windows/Linux）展开计划。
5. 滚动查看完整计划。
6. 如果计划中的任何内容与你的决定不符，请让 Copilot 修改。

## 批准 Autopilot

计划确定并审查完成后，就可以让 Copilot 开始实现。切换到 Autopilot 模式，让它持续处理任务，直到认为任务已完成。

1. 选择 **Accept plan and build on autopilot (recommended)**，或当前版本中类似的选项。
2. 确认提示框下方的模式指示器显示 **Autopilot**。
3. 观察 Copilot 如何按照计划迭代、生成代码并运行测试。

> [!NOTE]
> 批准后可能立即开始实现，因此请先审查计划。如果 Copilot 报告缺少依赖项或端口冲突，应先解决环境设置问题，再认定检查已完成。

## 审查并验证实现

与其他代码一样，生成的代码也需要在合并前审查。接下来检查代码并运行网站，确认一切正常。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 切换到 Interactive 模式。确认模式指示器不再显示 **Plan** 或 **Autopilot**。
2. 输入 `/diff`，检查筛选功能实现和测试。
3. 对照议题和规划时达成的决定检查结果。
4. 审查代码后，按 <kbd>Esc</kbd> 退出差异视图。
5. 查看项目检查结果，并让 Copilot 修复失败项。

## 探索新功能

代码看起来没有问题，但能否正常运行？像之前一样启动应用，并通过 Codespaces 转发端口打开网站。

1. 让 Copilot 启动应用：

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. 当 Codespaces 报告端口 `4321` 可用时，选择 **Open in Browser**。
3. 尝试类别筛选、发行商筛选，以及计划中约定的组合方式。
4. 确认重置筛选和空结果时的行为符合议题要求及你的决定。
5. 返回 Codespace，让 Copilot 停止它启动的开发服务器。

## 总结与后续步骤

你已使用不同的对话模式构建并审查功能。本课中，你：

- 从 GitHub 议题启动了新的 Copilot 对话。
- 在 Plan 模式中定义了需求。
- 使用 Autopilot 模式实现了新功能。
- 审查了代码。
- 在转发的浏览器中手动验证了功能。

接下来，将深入了解代码的生成方式，并[使用自定义指令][next-lesson]确保代码遵循已记录的实践。

## 资源

- [GitHub Copilot CLI 中的 Autopilot][autopilot]
- [Copilot CLI 命令参考][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
