---
title: "第 4 课 - 使用自定义指令引导 Copilot"
description: "探索存储库指令，添加文档标准，并将其应用于筛选代码。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

使用生成式 AI 时，上下文至关重要。如果某项任务需要按特定方式完成，就应让 Copilot 能够获取相应指导。[指令文件][instruction-files]不仅说明希望编写什么代码，也说明代码应该如何组织。现在已经实现筛选功能，接下来将探索 Copilot 使用的指令、添加文档标准，并将其应用于代码。

本课将介绍如何：

- 了解存储库指令和按路径限定范围的指令文件如何传递给智能体。
- 更新指令文件，确保遵循编码标准。
- 了解指令文件对代码的影响。

## 场景

Tailspin Toys 有一套开发实践准则和要求，包括：

- 注释应解释意图和不明显的决策，而不是复述代码。
- `db/` 和 `src/lib/` 中导出的函数应使用 TSDoc/JSDoc 说明用途、参数和返回值；如果有可注入的 `db` 参数，也应注明。
- 可复用的 Astro 组件应说明其 `Props` 接口；相关代码更改时，注释也应保持更新。
- 保留现有格式设置和 lint 指南。

通过使用指令文件，可以确保 Copilot 获得必要的信息，并按照这些实践完成任务。

## 指令文件

自定义指令可以向 Copilot 提供上下文和偏好，帮助它理解编码风格和需求，从而生成更相关的建议和代码片段。你可以指定偏好的编码约定、库以及注释风格，也可以为整个存储库或特定文件类型创建指令。

指令文件有两种：

- `.github/copilot-instructions.md`：每次针对该存储库的请求都会发送给 Copilot 的单个指令文件。该文件应包含适用于大多数请求的项目级信息。
- `.github/instructions/*.instructions.md`：为特定语言、文件类型或任务提供指导的文件。

> [!NOTE]
> 不同开发环境支持的其他指令格式可能有所不同。使用特定格式前，请查阅[自定义指令支持参考][custom-instructions-support]。

## 探索本项目中的自定义指令文件

示例项目已包含一组指令文件。先探索现有文件，再进行更改，以便观察其影响。

1. 返回 Codespace。
2. 在 Codespaces 编辑器（而不是终端）中打开 `.github/copilot-instructions.md`。
3. 浏览文件，了解其中对项目的简要说明和编码指导。此文件适用于与 Copilot 的每次交互。
4. 打开 `.github/instructions` 文件夹并浏览。可以看到其中包含针对 Astro 文件、Drizzle 数据层、测试等的指令。
5. 打开 `.github/instructions/unit-tests.instructions.md`。注意顶部的 `applyTo` 字段，它使用 glob 来确定指令适用于哪些文件。
6. 打开 `.github/instructions/drizzle.instructions.md`，查看它对其他指令文件和现有项目文件的链接。这样可以将较大的指令集拆分成更小且可复用的文件，并为 Copilot 提供可参考的示例。

## 更新指令以符合团队标准

这些文件已提供良好起点，但仍缺少一项指导。现在修改核心 `copilot-instructions.md`，要求为新生成的 TypeScript 添加 TSDoc 注释。

1. 在 `.github/copilot-instructions.md` 中找到 **Code formatting guidance** 部分，该部分应位于第 35 行附近。
2. 在列表末尾添加以下项目：

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

文件会自动保存。

## 使用更新后的指导

Copilot CLI 会在对话启动时加载存储库指令。保存更新后的指令后，退出并恢复筛选功能对话；这样既能保留功能上下文，又能让 Copilot 使用新指导。

1. 如果筛选功能对话仍在运行，输入 `/exit` 关闭它。Copilot CLI 会自动保存对话。
2. 在存储库根目录的 `game-filters-cli` 分支中恢复已保存的对话：

   ```bash
   copilot --resume
   ```

   如果出现提示，选择筛选功能对话，并确认检出的是 `game-filters-cli` 分支。
3. 让 Copilot 按照更新后的指导修改代码：

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. 输入 `/diff` 并查看已更改的 TypeScript 文件。注意新生成的 TSDoc 注释，并确认其准确说明了代码。

## 总结与后续步骤

你已了解 Copilot 如何获取指令文件中的上下文，并将一项新标准应用于功能代码。具体而言，你：

- 探索了存储库指令和按路径限定范围的指令文件如何传递给智能体。
- 更新了指令文件，要求为新生成的 TypeScript 添加 TSDoc 注释。
- 观察了指令文件对代码的影响。

接下来，将[自定义并运行可复用的 quality-checks 技能][next-lesson]，确保一致地运行 lint 和测试。

## 资源

- [为 GitHub Copilot CLI 添加自定义指令][instruction-files]
- [自定义指令支持][custom-instructions-support]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [Awesome Copilot：指令文件及其他资源集合][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
