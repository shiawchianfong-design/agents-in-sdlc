---
title: "练习 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

几乎所有组织都面临技术债务问题。这可能是尚未解决的安全问题、需要更新的旧代码，或因缺少时间而积压的功能请求。GitHub Copilot cloud agent 专为自动执行更新代码、添加功能等任务而设计。完成工作后，它会生成草稿拉取请求，供开发人员审查。这样可以将繁琐任务交给代理处理，加快开发进程，让开发人员专注于更重要的工作。

本练习将介绍如何使用 Copilot cloud agent：

- 自定义代码生成环境。
- 确保安全地执行操作。
- 了解清晰界定议题范围的重要性。
- 将议题分配给 Copilot。

## 场景

Tailspin Toys 有一些技术债务需要处理。最初受雇开发网站的承包商没有留下文档——你会发现代码完全缺少文档注释。团队首先希望为应用程序中所有导出的函数添加 TSDoc 文档注释。

此外，设计团队准备改进游戏发现体验。他们希望每个游戏详情页都显示相关游戏（即同一类别中的其他游戏），让用户可以继续浏览。他们暂时不需要完善的实现，只需要可以用于 UX 验收测试的功能。这项工作目前是一个阻碍，不过当前还有优先级更高的议题。

这两项任务都可能很快被搁置，非常适合交给 Copilot cloud agent。它可以异步处理任务，让开发人员先处理其他工作，之后再回来审查 Copilot 的成果并确认一切符合预期。

## 认识 GitHub Copilot cloud agent

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) 可以像人类开发人员一样在后台执行任务。就像与人类开发人员协作一样，你可以通过多种方式分配工作，包括[将 GitHub 议题分配给 Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)。分配后，Copilot 会创建草稿拉取请求以跟踪进度、设置环境并开始处理任务。你可以在 Copilot 会话进行期间或完成后查看详情。建议方案准备好供审查时，Copilot 会在拉取请求中提及你。

## 清晰界定指令范围的重要性

GitHub Copilot 并非万能——使用它并不意味着可以跳过思考。即使看似简单的操作，深入分析后也会发现复杂性，因此要[谨慎界定分配给 Copilot cloud agent 的任务范围][cloud-agent-best-practices]。把它当作 AI 结对程序员：分阶段工作，边做边学，不断试验和调整。引入生成式 AI 并不会改变软件开发的基本原则。

## 本存储库中的自定义指令

前面的练习介绍了自定义指令及其作用。如果你已完成自定义指令练习，就会看到 `.github/copilot-instructions.md` 的实际效果；如果还没看过，现在正好快速了解一下。将工作分配给 Copilot cloud agent 之前，先只读查看本存储库现有的指令文件，稍后便能观察它们的影响。

在存储库的 GitHub 网页界面中，或在已启动的 codespace 中，打开以下文件：

- `.github/copilot-instructions.md`：查看 **Code standards** 部分，尤其留意 TypeScript 规范和 TSDoc 文档注释的要求。
- `.github/instructions/unit-tests.instructions.md`：留意 `applyTo` frontmatter，它将指令限定在 `**/*.test.ts` 文件。

下一部分将 **Code lacks documentation** 议题分配给 cloud agent 时，请留意生成的拉取请求是否包含 TSDoc 文档注释、TypeScript 规范和文件头注释；这些要求来自上述指令文件。稍后的一项练习还会再次介绍如何审查拉取请求。

## 为 Copilot cloud agent 设置开发环境

无论由谁编写代码，通常都需要特定环境和一些设置脚本，以确保环境就绪。将任务分配给 Copilot 时也是如此，因为它会像软件工程师一样完成工作。

Copilot cloud agent 使用 [GitHub Actions][github-actions] 执行任务。你可以通过创建特殊的[设置工作流][setup-workflow]，在 `.github/workflows/copilot-setup-steps.yml` 中进行配置。该工作流会在 Copilot 开始工作前运行，为它安装所需的开发工具和依赖项。此处已预先配置好工作流，以配合实验并提供学习机会。它会确保 Copilot 能使用 Node.js、项目依赖项、端到端测试所需的浏览器，以及这款 Astro 单体应用已迁移并填充数据的 SQLite 数据库：

```yaml
name: "Copilot Setup Steps"

# Allows you to test the setup steps from your repository's "Actions" tab
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Set the permissions to the lowest permissions possible needed for *your steps*. Copilot will be given its own token for its operations.
    permissions:
      # If you want to clone the repository as part of your setup steps, for example to install dependencies, you'll need the `contents: read` permission. If you don't clone the repository in your setup steps, Copilot will do this for you automatically after the steps complete.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Frontend / app setup - Node.js (the whole app is now Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migrate + seed the local SQLite database so builds and tests have data.
      - name: Set up the database
        run: npm run db:setup
```

它看起来与其他 GitHub 工作流类似，但有几个关键点：

- 其中只有一个名为 `copilot-setup-steps` 的作业。Copilot 开始处理拉取请求之前，GitHub Actions 会先运行这个作业。
- 留意 `workflow_dispatch` 触发器。它允许你从存储库的 **Actions** 选项卡手动运行工作流，方便测试，而不必等到 Copilot 执行任务时再运行。

## 添加文档

大家都知道文档很重要，但多数项目的信息要么过时，要么完全缺失。这类技术债务常常得不到处理，降低了开发效率，也增加了维护代码或帮助新开发人员熟悉代码库的难度。Copilot 擅长编写文档，因此很适合将这项工作作为议题交给 Copilot cloud agent。它会在后台生成所需的文档。后续练习会审查它的成果。

1. 在新的浏览器选项卡中，转到 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue**，打开新议题对话框。
4. 选择 **Blank issue**，创建空白议题。
5. 将 **Title** 设置为 `Code lacks documentation`。
6. 将 **Description** 设置为：

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

  ![将议题分配给 Copilot](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。
10. 选择 **Pull Requests** 选项卡。
11. 打开新生成的拉取请求 (PR)，其标题类似 `[WIP]: Code lacks documentation`。如果列表中没有新 PR，请稍等片刻并刷新浏览器窗口。
12. 几分钟后，Copilot 应会创建待办事项列表。

> [!NOTE]
> Copilot 的待办事项列表可能需要几分钟才会出现在 PR 中。Copilot 正在创建环境（运行前面介绍的工作流）、分析项目并确定解决问题的最佳方案。

13. 查看列表及 Copilot 准备完成的任务。
14. 向下滚动拉取请求时间线，应能看到 Copilot 已开始处理此议题的动态。
15. 选择 **View session** 按钮。

  ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 可能需要刷新窗口才能看到更新后的状态指示器。

16. 浏览实时会话，了解 Copilot 如何解决问题。你会看到它如何探索代码并了解当前状态，如何停下来思考并决定合适的计划，以及如何创建代码。

整个过程可能需要几分钟。Copilot cloud agent 的主要优势之一是能异步执行任务，让我们专注于其他工作。接下来会利用这一点：先把另一项任务也分配给 Copilot cloud agent，然后编写代码为应用添加功能。

## 为游戏详情页添加相关游戏部分

如前所述，GitHub Copilot cloud agent 的一大优势是可以拆分工作：你专注于一组任务，它同时处理另一组。即使为设计团队添加相关游戏部分不一定需要太长时间，也可以把时间用于其他工作。现在就将此任务分配给 Copilot cloud agent。

1. 返回 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue**，打开新议题对话框。
4. 选择 **Blank issue**，使用空白模板。
5. 将 **Title** 设置为 `Show related games on the game details page`。
6. 将 **Description** 设置为：

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

  ![将议题分配给 Copilot](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

不久后，议题的第一条评论中会出现一组 👀，表示 Copilot 已开始处理任务。

![Copilot 使用眼睛表情表示正在处理议题](../../../_images/ex4-issue-eyes-emoji.png)

Copilot 正在认真处理第二项请求。它的工作方式类似软件工程师，因此你不需要一直监视它；等工作完成后再审查即可。现在可以转向创建和使用自定义智能体。

## 总结和下一步

本课介绍了 [GitHub Copilot cloud agent][copilot-agents]。使用 cloud agent，可以将议题分配给 Copilot，让它异步处理技术债务、创建新功能，或协助将代码迁移到另一个框架。

你了解了以下概念：

- 自定义代码生成环境。
- 确保安全地执行操作。
- 清晰界定议题范围的重要性。
- 将议题分配给 Copilot。

Copilot cloud agent 正在后台处理任务；现在可以转而创建和使用自定义智能体。[Copilot cloud agent 也可以使用 MCP 服务器][cloud-agent-mcp]，并支持此前介绍过的自定义指令。

## 资源

- [关于 Copilot cloud agent][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot cloud agent 设置工作流最佳实践][cloud-agent-best-practices]

| [下一课：自定义智能体 →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
