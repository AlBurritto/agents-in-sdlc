---
title: "第 4 课 - 使用自定义指令指导 Copilot"
description: "查看存储库指令、添加文档标准，并将其应用于筛选代码。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

使用生成式 AI 时，上下文至关重要。任务需要以特定方式完成时，应确保 Copilot 能获取相应指引。[指令文件][instruction-files]不仅说明希望生成什么代码，也说明代码应如何组织。筛选功能已完成，现在查看 Copilot 使用的指令、添加文档标准并将其应用到代码中。

本课将：

- 了解存储库指令和按路径应用的指令文件如何传递给智能体；
- 更新指令文件，确保遵循编码标准；
- 观察指令文件对代码的影响。

## 场景

Tailspin Toys 有一套开发实践指南和要求，包括：

- 注释应解释意图和不明显的决定，而不是重复代码内容；
- `db/` 和 `src/lib/` 中的导出函数应使用 TSDoc/JSDoc 说明用途、参数和返回值（包括其中可注入的 `db` 参数）；
- 可复用的 Astro 组件应说明其 `Props` 接口契约，相关代码更改时也应更新注释；
- 保留现有格式和 lint 指引。

使用指令文件，确保 Copilot 了解这些实践要求，并据此完成任务。

## 指令文件

自定义指令可向 Copilot 提供上下文和偏好，帮助它理解你的编码风格和要求。它可以引导 Copilot 提供更相关的建议和代码片段。你可以指定编码约定、要使用的库，甚至偏好的注释形式。指令既可以适用于整个存储库，也可以针对特定文件类型或任务。

指令文件有两种：

- `.github/copilot-instructions.md`：每次针对该存储库的请求都会发送给 Copilot 的单个指令文件。它应包含与大多数请求相关的项目级信息。
- `.github/instructions/*.instructions.md`：为特定语言、文件类型或任务提供指引的文件。

> [!NOTE]
> 其他指令格式及其支持情况因环境而异。依赖特定格式前，请查看[自定义指令支持参考][custom-instructions-support]。

## 浏览项目中的自定义指令文件

起始项目已包含一组指令文件。先查看现有文件，再进行更改并观察效果。

1. 返回 Codespace。
2. 在 Codespaces 编辑器（不是终端）中打开 `.github/copilot-instructions.md`。
3. 浏览文件，留意项目简介和编码指引。此文件适用于该存储库中与 Copilot 的每次交互。
4. 打开 `.github/instructions` 文件夹并浏览其中的文件。注意其中有 Astro 文件、Drizzle 数据层、测试等方面的指引。
5. 打开 `.github/instructions/unit-tests.instructions.md`。留意顶部的 `applyTo` 字段，它用于设置指令适用文件的 glob。
6. 打开 `.github/instructions/drizzle.instructions.md`，查看它指向其他指令文件和项目现有文件的链接。这样可以将较大的指令拆分成较小、可复用的文件，并为 Copilot 提供示例。

## 更新指令文件以符合团队指南

现有文件是不错的起点，但仍有缺项。修改核心文件 `copilot-instructions.md`，确保新生成的 TypeScript 代码包含 TSDoc 注释。

1. 在 `.github/copilot-instructions.md` 中找到 **Code formatting guidance** 部分，通常在第 35 行附近。
2. 将以下内容添加为该部分的最后一项：

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

文件会自动保存。

## 使用更新后的指引

Copilot CLI 会在对话启动时载入存储库指令。保存指令后，退出并恢复筛选会话，让 Copilot 在不丢失功能上下文的情况下读取新指引。

1. 如果筛选对话仍处于打开状态，输入 `/exit` 关闭。Copilot CLI 会自动保存对话。
2. 在 `game-filters-cli` 分支的存储库根目录中，恢复已保存的对话：

   ```bash
   copilot --resume
   ```

   如果系统提示，请选择筛选对话，并确认检出的是 `game-filters-cli` 分支。
3. 要求 Copilot 应用更新后的指引：

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. 输入 `/diff`，检查更改的 TypeScript 文件。留意新增的 TSDoc 注释，并确认其准确说明了代码。

## 总结与后续步骤

你了解了 Copilot CLI 如何从指令文件获取上下文，并将新标准应用于功能代码。本课中你：

- 了解存储库指令和按路径应用的指令文件如何传递给智能体；
- 更新指令文件，确保遵循编码标准；
- 观察指令文件对代码的影响。

接下来将[自定义并运行可复用的 quality-checks 技能][next-lesson]，确保 lint 和测试能够一致地运行。

## 资源

- [为 Copilot CLI 添加自定义指令][instruction-files]
- [自定义指令支持][custom-instructions-support]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [Awesome Copilot — 指令文件及其他资源集合][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
