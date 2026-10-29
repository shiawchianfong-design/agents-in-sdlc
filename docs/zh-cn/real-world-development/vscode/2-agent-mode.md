---
title: "练习 2 - 使用 Copilot 智能体模式添加功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

即使是最简单的应用更新，通常也需要修改多个文件并执行多项操作，例如运行测试。开发人员的工作流程通常包括：查找所有相关文件、进行更改、运行测试、调试、找出遗漏的文件、再次修改……如此反复。

Copilot 智能体模式正是为此而设计的。

Copilot 智能体模式可以在 IDE 中更自主地工作。它的工作方式类似开发人员：先探索现有项目结构，再执行必要的更新、运行测试等任务，并自动修复发现的问题。接下来看看如何使用智能体模式为网站添加新功能。

> [!NOTE]
> 智能体模式和 cloud agent 名称相似，但适用于两种不同的体验。智能体模式在 IDE 中执行任务，便于快速反馈和交互。Cloud agent 则像团队成员一样异步工作，通过议题和拉取请求与你协作。

本练习将介绍如何：

- 使用 Copilot 智能体模式探索项目并找到相关文件，协调多处更改。
- 使用 GitHub Copilot 智能体模式跨 UI 和数据层实现新功能。
- 在将代码合并到代码库之前，审查 Copilot 智能体模式生成的更改和测试。

## 场景

随着游戏列表不断增加，你希望允许用户按类别和 publisher 筛选。上一练习已添加 publisher 辅助函数，现在将使用 Copilot 智能体模式完成数据层、UI 和测试的剩余工作。

## 运行 Tailspin Toys 网站

更改网站之前，先了解其当前功能。

该网站是一个采用开发者主题的桌游众筹平台。用户可以浏览游戏列表并查看游戏详情。网站是一个 Astro 单体应用，在构建时将页面渲染为静态 HTML。页面在 frontmatter 中使用 Drizzle ORM 直接查询本地 SQLite 数据库，没有单独的后端 API 或客户端 UI 框架。可复用的数据访问辅助函数放在 `src/lib/` 中，交互功能则使用标准 DOM API，通过作用域限定的 Astro `<script>` 添加。

### 启动网站

项目提供了 `npm run dev` 任务，用于启动 Astro 开发服务器。请在 GitHub Codespace 中按以下步骤运行网站：

1. 返回 Codespace。上一项练习结束后，你应该已切回 `main`。
2. 在 Codespace 中按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开新终端。
3. 为筛选工作创建并切换到新分支：

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 运行以下命令启动网站：

   ```bash
   npm run dev
   ```

   Astro 开发服务器准备就绪后，会显示类似以下内容的横幅：

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> 如果弹出对话框，询问是否在浏览器窗口中打开 `http://localhost:4321`，请选择 **x** 将其关闭。

5. 按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），然后在终端中选择 Astro 服务器地址 `http://localhost:4321`，打开网站。

> [!NOTE]
> 在 Codespace 中选择终端的 localhost URL 时，它会自动重定向到 `https://<your-codespace-name>-4321.app.github.dev/`。这是连接到 Codespace 网站服务器的私有隧道。

### 浏览网站

网站启动后，可以浏览其功能。主要功能包括：

- **主页**：显示桌游列表、标题、图片和说明。
- **游戏详情页**：选择游戏后，会打开详情页，显示标题、说明、publisher 和类别等信息。

## 使用 Copilot 查看待办事项

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始一个干净的对话。不同练习所用模式和模型可能不同；每个步骤会说明具体要求。

网站的初始版本可以正常使用，但还需要添加更多功能。先查看待办事项。使用模板创建项目时，系统已自动创建一组议题；请让 GitHub Copilot 列出这些议题。

1. 在 Chat 视图的智能体下拉菜单中选择 **Agent**。**Agent** 智能体可以自主规划并跨文件实现更改、运行终端命令和调用工具。

   ![Chat 视图中的智能体选择器](../../../_images/shared-chat-mode-selector.png)

2. 从可用模型列表中选择 **Claude Sonnet 4.5**。

> [!CAUTION]
> 本工作坊作者并非推荐某个模型。编写本工作坊时使用的是 Claude Sonnet 4.5，因此在说明中采用该模型，希望代码建议相对一致，从而带来良好的体验。不过，大型语言模型具有概率性，你看到的建议可能与本工作坊中的说明有所不同。这完全正常。

> [!NOTE]
> 由于模型具有概率性，Copilot 可能会使用不同的 MCP 命令，但仍应能完成任务。

3. 向 Copilot 发送以下提示，询问待办事项中的议题：

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. 选择 **Continue**，运行列出所有议题的命令。
5. 查看生成的议题列表。

留意 Copilot 还根据其判断的用户价值为议题排定了优先级。

## 查看指令文件

让代理生成代码之前，先查看将用于向 Copilot 提供上下文的指令文件。本练习将使用 [UI 指令文件](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)，其中介绍了向网站添加功能的方法。

1. 在 Codespace 中打开 `.github/instructions/ui.instructions.md`。
2. 留意文件中有关如何添加功能的整体指导，包括：
   - 架构概述。
   - 组件设计、可测试性和无障碍原则。
   - 指向各类文件专用指令的链接，包括：
     - Astro。
     - Tailwind CSS。
     - Drizzle。

> [!TIP]
> 指令文件可以引用其他指令文件以及项目文件。路径相对于当前指令文件的位置。这种做法既可以重复使用指引、将较大的指令拆分为更易管理的部分，也可以引用示例和模板。

## 实现筛选功能

要完成筛选功能，至少需要对应用进行三项更改：

- 添加或完善数据层 (`src/lib/`) 中剩余的筛选逻辑。
- 添加或更新筛选行为测试。
- 更新游戏列表页，添加筛选 UI。

此外，在将更改合并到代码库之前，还需要运行并通过测试。Copilot 智能体模式可以完成这些工作。现在开始添加此功能。

1. 可以继续使用当前 Copilot 对话，也可以选择 **New Chat** 开始新对话。
2. 选择 **Add Context**、**Instructions**，然后选择 **ui** 指令文件。

   ![选择 UI 指令文件](../../../_images/ex3-select-instructions-file.png)

3. 确认 Chat 视图的智能体下拉菜单中仍选中 **Agent**。

   ![Chat 视图中的智能体选择器](../../../_images/shared-chat-mode-selector.png)

4. 确认仍选择 **Claude Sonnet 4.5** 模型。
5. 使用以下提示，让 Copilot 根据待办事项中的相关议题实现功能：

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. 观察 Copilot 开始探索项目并查找相关文件。它应该会找到数据层辅助函数、UI 和测试，然后开始修改文件并运行测试。

   ![Copilot 正在探索项目文件](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot 会执行多个任务，例如探索项目、修改文件和运行测试。根据任务和代码库的复杂度，这可能需要几分钟。在此过程中，代码编辑器中可能会出现 **Keep** 和 **Undo** 按钮。Copilot 完成后，所有更改都会有 **Keep** 或 **Undo** 按钮，因此无需在工作进行期间选择它们。

7. Copilot 提示运行测试时，选择 **Continue**。

   ![Copilot Chat 窗格中的对话框，提示用户确认是否运行测试](../../../_images/ex3-agent-mode-run-tests.png)

8. 过程中可能会出现暂停，甚至有些测试失败。这没有关系！Copilot 会在生成代码和运行测试之间反复处理，直到任务完成且没有检测到错误。

   ![完整的 Copilot 智能体模式 Chat 会话](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 检查生成的代码，查找潜在问题。

> [!CAUTION]
> 请记住，务必审查 Copilot 或其他 AI 工具生成的代码。

10. 返回运行着网站的浏览器，体验新功能。
11. 确认功能正常并审查代码后，在 Copilot Chat 窗口中选择 **Keep**。

## 总结和下一步

恭喜！本练习介绍了如何使用 GitHub Copilot 智能体模式为 Tailspin Toys 网站添加新功能。你了解了：

- 使用 GitHub Copilot 智能体模式跨 UI 和数据层实现新功能。
- 使用 Copilot 智能体模式探索项目、查找相关文件并协调多处更改。
- 在将代码合并到代码库之前，审查 Copilot 智能体模式生成的更改和测试。

接下来，使用 [Playwright MCP 服务器测试功能][next-lesson]，然后为其创建拉取请求。

### 额外探索练习 — 实现分页

游戏列表增加后，需要实现分页。使用本练习中学到的技能，提示 Copilot 更新网站以实现分页。请考虑以下事项：

- 遵循现有最佳实践，包括使用已有的指令文件。
- 规划分页方式，例如是否允许用户选择每页大小，还是使用固定值。
- 编写提示时，提供足够的指导，让 Copilot 按预期实现功能。
- 可能需要反复与 GitHub Copilot 协作，提出更改请求并提供上下文。这正是使用 Copilot 的常见方式。

## 资源

- [Copilot 的询问、编辑和智能体模式：功能及适用场景][choose-mode]
- [VS Code 中的智能体模式][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
