---
title: "第 6 课 - 使用 Playwright MCP 验证功能"
description: "在 Copilot CLI 中查看或配置 Playwright MCP，并在浏览器中检查筛选功能。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

如前所述，编写代码不只是写代码。我们还需要处理数据、外部服务，并允许 Copilot 使用更多自动化工具。MCP 服务器可以扩展 Copilot CLI 内置的能力，为它提供更多工具和服务。

本课将：

- 了解模型上下文协议 (MCP) 以及 Copilot CLI 如何使用它；
- 如果尚不可用，将 Playwright MCP 服务器添加到配置中；
- 要求智能体操作浏览器并检查筛选功能。

## 场景

单元测试和端到端测试固然重要，但验证 UI 更新还需要实际与界面交互。我们希望 Copilot 能像用户一样使用正在开发的网站，进一步自动化更改验证，并增强对功能符合预期的信心。

## 什么是模型上下文协议 (MCP)？

[模型上下文协议 (MCP)][mcp-blog-post] 让 AI 智能体能够与外部工具和服务通信。使用 MCP 后，AI 智能体可以实时访问最新信息，并代表你执行操作。

这些工具和资源通过 MCP 服务器访问。MCP 服务器在 AI 智能体与外部工具和服务之间充当桥梁。每个 MCP 服务器都代表 AI 智能体可访问的一组工具和资源。

以下是两个常见的 MCP 服务器：

- **[GitHub MCP Server][github-mcp]**：提供用于管理 GitHub 存储库、议题和拉取请求的 API。
- **[Playwright MCP Server][playwright-mcp-server]**：使用 Playwright 提供浏览器自动化能力。

还有许多其他 MCP 服务器。GitHub 提供了一个 [MCP 注册表][mcp-registry]，方便发现服务器并参与生态系统建设。

> [!CAUTION]
> 对待 MCP 服务器时，应像对待项目中的其他依赖项一样谨慎。使用前请审查其源代码、验证发布者，并评估安全影响。

## 添加 Playwright MCP 服务器

如果尚未配置 Playwright MCP 服务器，请将它添加到设置中，让 Copilot 能够像用户一样与网站交互。

> [!NOTE]
> 如果 Copilot CLI 中已配置 Playwright MCP，请跳过这些设置步骤，直接继续下一节。

1. 返回 Codespace。
2. 在 Copilot CLI 中输入以下命令，打开添加 MCP 服务器对话框：

   ```plaintext
   /mcp add
   ```

3. 名称输入 `playwright`，然后按 <kbd>Tab</kbd>。
4. 按 <kbd>Enter</kbd> 确认服务器类型为 **STDIO**，然后按 <kbd>Tab</kbd>。
5. 在 **Command** 对话框中粘贴以下内容：

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. 按 <kbd>Control</kbd>+<kbd>S</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>S</kbd>（Windows/Linux）保存新的 MCP 服务器。
7. 按 <kbd>Esc</kbd> 退出 MCP 对话框。

## 让 Copilot 使用 Playwright 检查功能

之前已手动确认功能行为符合预期。现在使用 Copilot CLI 中的 Playwright MCP 服务器执行同样的测试。

1. 使用以下提示词，要求 Copilot 使用 Playwright MCP 服务器验证功能：

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> 通常无需明确要求 Copilot 使用 MCP 服务器，因为它会自行判断。但既然知道该使用什么工具，明确指出方向也有帮助，可以获得更一致的结果并节省一些 token。

2. 观察 Copilot 如何列出浏览器操作步骤并确认功能正常。
3. 查看报告，确认每项行为均符合预期。

Copilot 会启动服务器、使用 Playwright 与网站交互、停止服务器并提供报告。

## 总结与后续步骤

恭喜你通过 Copilot CLI 使用 Playwright MCP 服务器在真实浏览器中检查了功能。本课中你：

- 了解模型上下文协议 (MCP) 以及 Copilot CLI 如何使用它；
- 如果尚未配置，则添加 Playwright MCP 服务器；
- 要求智能体操作浏览器并检查筛选功能。

接下来将[创建 QA 自定义智能体][next-lesson]，把技能和浏览器工具整合到专用角色中。

## 资源

- [MCP 是什么，为什么大家都在讨论它？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [向 Copilot CLI 添加 MCP 服务器][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
