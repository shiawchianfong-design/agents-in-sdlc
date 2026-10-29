---
slug: zh-cn/real-world-development/cloud
title: "GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

**[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent)** 可在云端异步处理 GitHub Copilot 任务。你在 GitHub 上分配工作后，cloud agent 会在后台接手任务，探索存储库、进行更改并创建拉取请求；你可以同时处理其他工作。

在这些练习中，你将添加 cloud agent 会遵循的自定义指令，然后分配 GitHub 议题，让它实现相应工作。你还将审查并使用自定义智能体来调整其工作方式，通过智能体控制面板监视并引导会话，最后审查它创建的拉取请求并迭代改进结果。

## 练习

| 练习 | 主题 | 说明 |
|----------|-------|-------------|
| [0. 先决条件][ex0] | 设置 | 创建存储库和 Codespace |
| [1. 自定义指令][ex1] | 上下文 | 添加 cloud agent 会遵循的自定义指令 |
| [2. Cloud Agent][ex2] | 异步智能体 | 将议题分配给 GitHub Copilot cloud agent |
| [3. 自定义智能体][ex3] | 专用智能体 | 审查并使用自定义智能体 |
| [4. 管理智能体][ex4] | 监视 | 监视并引导智能体会话 |
| [5. 迭代][ex5] | 审查 | 审查 PR、迭代改进 Copilot 的工作并选择后续步骤 |

## 先决条件

参加本工作坊前，请确保具备以下条件：

- [ ] 拥有有效的 **Copilot Student、Pro、Pro+、Business 或 Enterprise** 计划的 GitHub 帐户
- [ ] 已为帐户或组织启用 Copilot cloud agent

> [!TIP]
> 没有付费计划？经过验证的学生可以通过 [GitHub Education][callout-student-plan-education] 免费使用 GitHub Copilot。**Copilot Student** 计划包含本工作坊使用的智能体、MCP、代码审查和 Copilot CLI 功能，因此可以完成所有开发环境的课程。

[callout-student-plan-education]: https://github.com/education/students

> [!NOTE]
> Copilot cloud agent 适用于 **Copilot Student、Pro、Pro+、Business 和 Enterprise**，不适用于 Copilot Free。使用 Copilot Business 或 Enterprise 时，管理员必须为组织启用此功能。

> [!NOTE]
> 本开发环境不介绍 MCP。若要了解如何在 Copilot 中使用 MCP 服务器，请参阅 [CLI 开发环境](../cli/)或 [VS Code 开发环境](../vscode/)。

## 开始学习

**[从第 0 课：先决条件开始 →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-cloud-agent/
[ex3]: 3-custom-agents/
[ex4]: 4-managing-agents/
[ex5]: 5-iterating/
