---
title: "练习 1 - 自定义指令（VS Code）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：代理模式 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，或 Copilot 需要了解一些背景信息，就要确保它能获取这些上下文。可以通过多种方式向 Copilot 提供特定上下文，其中一种重要方式是使用[指令文件][instruction-files]，指导代码生成。

本练习将：

- 了解项目专属上下文、编码指南和文档标准如何通过存储库自定义指令和按路径应用的指令文件传递给 Copilot；
- 在当前指令下生成筛选功能的第一个数据切片（publisher 辅助函数）；
- 在 `.github/copilot-instructions.md` 中添加一条适用于整个存储库的新标准；
- 重新发送相同提示词，观察生成的代码如何遵循新标准；
- 提交指令更新和筛选功能切片到 `main`，让 Copilot 在下一课中使用更新后的指引。

> [!CAUTION]
> 生成的代码可能不完全遵循你设置的标准。Copilot 具有非确定性。本练习旨在展示更新指令后行为的整体变化趋势，而不是要求输出逐字一致。

## 指令文件

### 场景

Tailspin Toys 有一套开发实践指南和要求，包括：

- 数据层始终需要单元测试。
- UI 应使用深色模式并具有现代感。
- 应通过 TSDoc 文档注释为代码添加文档。
- 每个文件开头都应添加一段注释，说明文件的用途。

使用指令文件，确保 Copilot 了解这些实践要求，并据此完成任务。

### 自定义指令

自定义指令可向 Copilot 提供上下文和偏好，帮助它理解你的编码风格和要求。它可以引导 Copilot 提供更相关的建议和代码片段。你可以指定编码约定、要使用的库，甚至偏好的注释形式。指令既可以适用于整个存储库，也可以针对特定文件类型或任务。

指令文件有两种：

- `.github/copilot-instructions.md`：每次针对该存储库的请求都会发送给 Copilot 的单个指令文件。它应包含与大多数 Copilot Chat 或 CLI 请求相关的项目级信息，例如技术栈、项目概述、最佳实践和全局指引。
- `.github/instructions/*.instructions.md`：针对特定任务或文件类型的指令文件。可为 TypeScript 或 Astro 等语言，或创建 UI 组件、单元测试等任务编写指引。

> [!NOTE]
> 在 IDE 中，指令文件仅用于 Copilot Chat 代码生成，不用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云智能体在生成代码时，都会使用存储库级指令文件和带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> 此外，Copilot [也支持其他标准的指令文件][custom-instructions-support]，包括 AGENTS.md 和 CLAUDE.md。

### 管理指令文件的最佳实践

详细讨论如何创建指令文件超出了本练习的范围，但示例项目展示了一种有代表性的方法。总体而言：

- 将 `copilot-instructions.md` 聚焦于项目级指引，例如项目用途、结构和全局编码标准。
- 使用 `*.instructions.md` 为文件类型（单元测试、Astro 组件、数据层）或特定任务提供指引。
- 使用自然语言，保持说明清晰，并提供代码应如何编写或不应如何编写的示例。

创建指令文件并没有唯一的方法，使用 AI 也是如此。通过尝试找到适合项目的做法。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应配备完善的指令文件。浏览此项目中的文件时，你会发现其中包含多种任务的指引，例如 [UI 更新][ui-instructions]和 [Astro][astro-instructions]。
>
> Copilot 也可以帮助生成指令文件。不同产品界面的操作方式有所不同（例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`）；相关练习会在适用时介绍具体方式。
>
> 想寻找模板或入门示例？可以浏览 [awesome-copilot][awesome-copilot]，其中收集了指令文件、自定义智能体和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 浏览项目中的自定义指令文件

先查看此存储库附带的指令文件：一个核心文件 `copilot-instructions.md`，以及一组适用于不同任务的 `*.instructions.md` 文件。可在编辑器或 GitHub 网页界面中打开这些文件。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览文件，留意项目简介以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。注意 **Code standards** 下还有 **GitHub Actions Workflows** 子部分。这些内容适用于与 Copilot 的各种交互。
3. 打开 `.github/instructions` 文件夹并浏览其中内容。注意其中有 Astro 文件、Drizzle 数据层、测试等方面的指引。
4. 打开 `.github/instructions/unit-tests.instructions.md`。留意文件顶部的 `applyTo` 字段；它用于设置一个相对于存储库根目录的 glob，决定指令适用于哪些文件。此处会匹配任何 TypeScript 测试文件，例如符合 `**/*.test.ts` 的文件。
5. 查看针对本项目单元测试的具体指引。
6. 最后，打开 `.github/instructions/drizzle.instructions.md` 并滚动到末尾。留意其中指向其他指令文件（如 `unit-tests.instructions.md`）和项目现有文件的链接。这样可以将较大的指令集拆分成较小、可复用的文件，并为 Copilot 提供可参考的示例。这里的路径是相对于指令文件所在位置，而不是存储库根目录。

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分介绍了项目编码标准，但目前尚未要求添加代码文档。接下来会添加 TSDoc 文档注释和文件注释头的规则。

## 为更改创建分支

按照最佳实践为更改创建分支。

1. 返回上一练习中使用的 codespace。
2. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开新终端。
3. 创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions
   ```

## 更新指令前先使用 Copilot Chat

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 codespace，打开 Copilot Chat 面板，然后选择 **New Chat** 开始一个新对话。模式和模型的选择因练习而异；每一步会在需要时说明。

先在现有指令下发送提示词，以观察自定义指令的影响。稍后会更新指令，再次发送相同提示词并对比结果。

1. 关闭之前练习留下的编辑器标签页，让 Copilot 只获取所需上下文。
2. 打开 `src/lib/publishers.ts`，让 Copilot 知道辅助函数应放在哪里。
3. 在 Chat 视图的智能体下拉列表中选择 **Agent**，以便 Copilot 能应用文件更改。

   ![Chat 视图中的智能体选择器截图。](../../../_images/shared-chat-mode-selector.png)

4. 发送以下提示词：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot 会探索项目并应用代码更新，通常会同时修改辅助函数文件及其测试。
6. 注意，生成的辅助函数是一个带类型的函数：它将 `db` 客户端作为第一个参数，并返回带类型的 publisher 数组。这源于 `.github/instructions/drizzle.instructions.md` 中的数据层约定，该文件适用于 `src/lib/*.ts`。
7. 注意生成的代码**缺少** TSDoc 文档注释和文件级注释头。

> [!CAUTION]
> Copilot 具有概率性，有时即使没有明确要求，也可能添加文档注释。如果发生这种情况也没关系；本练习的重点仍是观察添加指令后行为一致性的提升。

## 添加存储库标准

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在，为存储库编码标准添加规则，以改进代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分，通常在第 27 行附近。注意该部分介绍了项目编码标准，但尚无代码文档规则，因此刚才生成的辅助函数没有文档注释。
3. 在现有标准下方添加以下 Markdown 内容，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所见，指令文件可以放在存储库级别（`.github/copilot-instructions.md`），以提供全局指引；也可以使用 `*.instructions.md` 为特定语言、文件类型或任务提供指引。刚添加的文档注释规则属于项目级标准，因此存储库级文件是合适的位置。

## 重新发送提示词并观察变化

1. 返回 Copilot Chat，选择 **New Chat** 清除当前对话内容。
2. 重新选择 `src/lib/publishers.ts`，让 Copilot 聚焦于正确的文件。
3. 发送之前的**相同提示词**：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 注意生成的文件现在以类似下面的注释块开头：

   ```typescript
   /**
    * Tailspin Toys 众筹平台的发行商数据访问辅助函数。
    * 提供从数据库检索发行商信息的函数。
    */
   ```

5. 注意生成的函数现在包含类似下面的 TSDoc 注释：

   ```typescript
   /**
    * 返回包含所有发行商 ID 和名称的列表。
    *
    * @param db - Drizzle 数据库客户端。
    * @returns 解析为发行商对象数组的 promise。
    */
   ```

你刚才引导 Copilot 遵循一项新项目标准，并将它应用到下一课会继续使用的实际代码中。

## 提交指令并推送分支

指令文件和其他项目资源一样，都通过源代码管理进行维护。现在将更改提交并推送到存储库。

1. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开新终端。
2. 在终端中运行以下命令，确认指令和辅助函数更改均已存在：

   ```bash
   git status
   ```

3. 在终端中暂存并提交指令更新，以及 Copilot 为辅助函数基础所做的所有更改：

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 将分支推送到存储库：

   ```bash
   git push -u origin custom-instructions
   ```

## 创建并合并拉取请求

将分支推送到存储库后，创建拉取请求，并关联待办事项中的文档标准议题。Copilot 可以使用已连接到项目的 GitHub 工具代为查找议题、创建拉取请求并关闭议题。

> [!NOTE]
> 这是第一次让 Copilot 代表你在 GitHub 上执行操作。项目模板已配置 GitHub 工具，因此无需额外设置；Copilot 首次使用工具时，VS Code 可能会提示你登录 GitHub。按提示授权即可。后续练习会介绍这种连接所使用的模型上下文协议 (Model Context Protocol)。

1. 在 codespace 中打开 Copilot Chat。
2. 按 <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>（Windows/Linux）打开 Copilot Chat 视图，并确保在智能体选择器中选中 **Agent**。
3. 让 Copilot 查找与更新指令文件相关的议题，并从当前分支创建拉取请求，说明指令更新和为后续筛选工作添加的 publisher 辅助函数基础：

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot 将开始查找议题并创建拉取请求。
5. 如果 Copilot 提示允许其代表你执行 GitHub 操作，请先检查命令，再酌情选择 **Allow**。
6. 创建拉取请求后，要求 Copilot 合并它并将本地分支切回 `main`：

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. 如果 Copilot 提示允许其执行 GitHub 操作和 shell 命令，请先检查命令，再酌情选择 **Allow**。

你已借助 GitHub Copilot 创建并合并了拉取请求！

## 总结与后续步骤

你了解了 Copilot 如何从项目指令文件获取上下文，并在 VS Code 中使用 Copilot Chat 完成了以下任务：

- 发送代码生成提示词，观察 Copilot 如何使用**现有**指令生成代码；
- 在 `.github/copilot-instructions.md` 中添加一条适用于整个存储库的新标准；
- 再次发送相同提示词，观察生成的代码如何遵循新标准；
- 将指令更新和 publisher 辅助函数基础提交到 `main`，供下一课继续使用。

下一课将使用这些指令，让 Copilot 通过[代理模式][next-lesson]跨代码库添加新功能。

## 资源

- [用于自定义 GitHub Copilot 的指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更优 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [GitHub Copilot 的个人自定义指令][personal-instructions]
- [Awesome Copilot — 指令文件及其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
