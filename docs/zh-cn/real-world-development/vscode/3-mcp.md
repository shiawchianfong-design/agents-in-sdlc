---
title: "练习 3 - 使用 Playwright MCP 服务器测试功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：智能体模式][previous-lesson] · [下一课：自定义智能体 →][next-lesson]

你刚刚使用智能体模式构建了筛选功能。在创建拉取请求之前，应该确认它确实可以在浏览器中运行。接下来将连接 **Playwright MCP 服务器**，让 Copilot 操控真实浏览器为你测试功能，而不是手动点击应用；然后发布分支并创建 PR。

本练习将：

- 了解模型上下文协议 (MCP)，以及 MCP 服务器如何使用新工具扩展 Copilot。
- 将 Playwright MCP 服务器添加到工作区。
- 让 Copilot 在浏览器中手动测试筛选功能。
- 发布分支，并为筛选工作创建拉取请求。

## 什么是模型上下文协议 (MCP)？

当智能体模式可以使用编辑器以外的工具时，它的能力会大幅增强。MCP 是 Copilot 实现这一点的方式，是一种让智能体与外部工具和服务通信的标准。

![智能体模式内部工作原理图，展示其与上下文、LLM 和工具（包括 MCP 服务器与 VS Code 扩展提供的工具）的交互](../../../_images/ex1-mcp-diagram.png)

[模型上下文协议 (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) 为 AI 智能体提供了与外部工具和服务通信的方式。使用 MCP，AI 智能体可以实时访问外部工具和服务，从而获取最新信息（使用资源）并代表你执行操作（使用工具）。

这些工具和资源通过 MCP 服务器访问。MCP 服务器充当 AI 智能体与外部工具和服务之间的桥梁，并负责管理双方之间的通信，例如使用现有 API 或 npm 包等本地工具。每个 MCP 服务器都提供一组 AI 智能体可访问的工具和资源。

以下是几个常见的 MCP 服务器：

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**：提供一组用于管理 GitHub 存储库的 API。AI 智能体可以使用它创建存储库、更新现有存储库，以及管理议题和拉取请求。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**：使用 Playwright 提供浏览器自动化能力。AI 智能体可以使用它访问网页、填写表单和选择按钮。

还有许多其他 MCP 服务器可以访问不同工具和资源。GitHub 提供了 [MCP 注册表](https://github.com/mcp)，方便大家查找和参与生态建设。

> [!CAUTION]
> 从安全角度来看，应像对待项目中的其他依赖项一样对待 MCP 服务器。使用前应仔细审查源代码、验证发布者，并评估安全影响。只使用信任的 MCP 服务器，并谨慎授予敏感资源或操作的访问权限。

## 查看 MCP 配置

`.vscode/mcp.json` 文件用于配置 VS Code 工作区可使用的 MCP 服务器。

1. 在 Codespace 中打开 `.vscode/mcp.json`。
2. 你应能看到已经配置好的 `github` 服务器：

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

项目模板已包含 `github` 配置，因此 Copilot 能在上一练习中读取议题待办事项。它使用[远程 GitHub MCP 服务器][remote-github-mcp-server]，无需在本地安装任何内容。VS Code 会通过 HTTP 连接到该服务器，并在 Copilot 首次使用其工具时要求你通过 GitHub 身份验证。

## 添加 Playwright MCP 服务器

现在添加第二台服务器。[Playwright MCP 服务器][playwright-mcp-server]可为 Copilot 提供可控的浏览器，正好用于测试新功能。

1. 在 `.vscode/mcp.json` 中添加 `playwright` 配置，与 `github` 并列，使文件如下所示：

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. 保存文件。

与远程 `github` 服务器不同，`playwright` 是**本地**服务器：VS Code 会在你的计算机上运行 `npx @playwright/mcp@latest` 来启动它。`--headless` 标记让 Playwright 在没有可见窗口的情况下运行浏览器。Codespace 中没有可显示浏览器的桌面环境，因此需要此选项。

> [!NOTE]
> Tailspin Toys 项目已在端到端测试中使用 Playwright，因此通常已经安装 Playwright 所需的浏览器。如果 Copilot 之后报告缺少浏览器，请让它运行 `npx playwright install chromium`，然后重试。

## 启动并信任服务器

VS Code 会**按需**启动 MCP 服务器：Copilot 首次需要访问服务器工具时，VS Code 会启动服务器并询问你是否信任它。

1. 在 `.vscode/mcp.json` 中，`playwright` 配置上方会出现 **Start** 操作。选择它以立即启动服务器，并确认配置正确。
2. 如果 VS Code 询问是否信任服务器，请检查配置并选择信任，以便启动服务器。

手动启动是可选步骤；如果跳过，VS Code 会在下一步首次需要使用服务器工具时启动服务器，并提示你信任它。

## 测试筛选功能

[Playwright MCP 服务器][playwright-mcp-server]可以让 Copilot 驱动真实浏览器。智能体可以打开页面、导航、应用筛选条件，并读取结果后向你汇报。无需离开当前对话，就能快速确认功能是否符合预期。

Playwright MCP 服务器底层使用页面的[无障碍树][playwright-mcp-server]，而不是截图。因此，智能体会根据带标签的结构化元素（按钮、链接和列表项）进行推理，与辅助技术的工作方式相同。这意味着快速功能测试也能顺便进行简单的无障碍检查。

连接服务器并启动应用后，提示 Copilot 测试刚实现的筛选功能：

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot 会通过 Playwright MCP 服务器启动浏览器并逐步执行测试，然后汇报测试结果。请根据议题中的验收标准审阅它的总结；如果发现异常，可以继续提问或让它修复代码，然后再创建拉取请求。

> [!NOTE]
> 测试时，应用必须在 `http://localhost:4321` 运行。如果开发服务器已停止，请先重新启动再发送提示。Copilot 首次使用 Playwright MCP 服务器时，可能需要下载浏览器；如果报告缺少浏览器，请让它运行 `npx playwright install chromium`，然后重试。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## 发布分支并创建拉取请求

确认功能正常后，就可以创建拉取请求 (PR)，供团队审查了。首先发布 `filtering-vscode` 分支。

1. 在 Codespace 中打开 **Source Control** 面板，查看 Copilot 所做的更改。
2. 选择 **+** 图标暂存更改。
3. 使用 **Sparkle** 按钮生成提交消息。

   ![Source Control 面板中的更改](../../../_images/ex3-source-control-changes.png)

4. 选择 **Publish**，将分支推送到存储库。

## 创建拉取请求

可以通过多种方式创建拉取请求，包括使用 github.com 和 GitHub 命令行界面 (CLI)。不过，既然正在使用 GitHub Copilot，不妨让它代为创建 PR！它可以查找相关议题并将其与 PR 关联。

1. 转到 Copilot Chat 面板并选择 **New Chat**，开始新会话。
2. 确认在智能体下拉菜单中选中 **Agent**，以便 Copilot 使用 GitHub 工具。
3. 提示 Copilot 创建 PR：

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 根据需要选择 **Continue**，允许 Copilot 执行必要的操作来收集信息并完成任务。首次使用 GitHub 工具时，系统可能会要求你通过 GitHub 身份验证；按照提示完成操作即可。
5. 留意 Copilot 如何搜索议题、找到相关议题并创建 PR。
6. 选择 Copilot 生成的链接，审查拉取请求，但**暂时不要合并**。

## 总结和下一步

恭喜！本练习中你：

- 了解了模型上下文协议 (MCP)，以及 MCP 服务器如何使用新工具扩展 Copilot。
- 将 Playwright MCP 服务器添加到工作区。
- 使用该服务器在浏览器中手动测试筛选功能。
- 发布分支，并为筛选工作创建拉取请求。

你已使用 MCP 服务器测试功能，不过 MCP 只是扩展 Copilot 的一种方式。接下来将[创建自定义智能体][next-lesson]，简化无障碍审查等专门任务。

## 资源

- [MCP 是什么？为什么大家都在讨论它？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP Registry][mcp-registry]
- [VS Code 中的 MCP 服务器][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
