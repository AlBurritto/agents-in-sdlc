---
title: "第 7 课 - 创建并使用 QA 智能体"
description: "创建 QA 自定义智能体，整合议题需求、quality-checks 技能和 Playwright MCP。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

你已使用 `quality-checks` 技能运行自动检查，并通过 Playwright MCP 在浏览器中观察筛选体验。现在将这些能力整合到具有明确定义 QA 流程的自定义智能体中。

本课将：

- 了解自定义智能体如何结合指令、技能和 MCP 工具；
- 创建并检查可复用的质量保证配置；
- 选择 QA 智能体，并根据筛选议题审查其发现。

## 场景

Tailspin Toys 希望在创建拉取请求 (PR) 前，使用一致的流程检查需求、代码质量、自动化检查、测试覆盖率和浏览器行为。自定义智能体可以协调 QA 流程并生成可复用的报告。

## 什么是自定义智能体？

自定义智能体是通过 Markdown 配置文件定义的 Copilot 专用版本。配置文件说明智能体的用途、指令和可用工具。本研讨会将在 `.github/agents/qa.agent.md` 中定义 QA 角色，并在 Copilot CLI 中选择它。

前面使用的自定义项各有不同用途：存储库指令说明团队标准；`quality-checks` 技能封装可重复运行的检查；Playwright MCP 提供浏览器工具；QA 配置则指示 Copilot 如何使用这些能力检查需求并报告发现。QA 配置不会取代它们，也不需要启动另一个对话。

## 创建 QA 配置

创建功能 PR 前，先要求 Copilot 创建可复用的 QA 配置，明确 QA 要执行的检查和必须遵循的限制。

1. 返回 Codespace，确认筛选对话处于 Interactive 模式。
2. 要求默认智能体创建新的自定义智能体：

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## 检查配置

使用新智能体前，先审查其配置，确认 Copilot 捕获了预期 QA 流程和边界。这样可避免智能体在你只想验证功能时修改实现，或执行范围过大的操作。

1. 输入 `/diff`，打开 `.github/agents/qa.agent.md`。
2. 阅读 frontmatter。`description` 为必填项；`name` 为可选项，但添加它可以为智能体指定清晰的显示名称。
3. 阅读配置指令，确认 QA 会从需求开始、遵循存储库指令、运行 `quality-checks` 技能并使用 Playwright MCP。
4. 确认 QA 会提供支持证据、修改实现代码前会先询问，而且不会提交更改或创建拉取请求。
5. 如果生成的配置遗漏上述职责或边界，要求默认智能体修改后再继续。

## 根据议题运行 QA

Copilot CLI 会在对话启动时加载项目智能体。创建配置后，恢复同一筛选对话并选择 QA，以便它使用已有的议题和计划决定。

1. 使用以下命令启用智能体：

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> 刚创建的智能体可能不会立即出现在可用智能体列表中，但仍然可以使用上述命令将其启用。

2. 使用以下提示词要求 QA 智能体审查功能：

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA 智能体将开始工作。
4. 完成后阅读它提供的报告。

## 总结与后续步骤

你为工作流添加了可复用的专用角色，并审查了其工作。本课中你：

- 了解自定义智能体如何结合指令、技能和 MCP 工具；
- 创建并检查可复用的质量保证配置；
- 选择 QA 智能体并根据筛选议题审查其发现。

现在，实现、技能更新、QA 配置、测试和验证报告都已准备好等待审查。接下来将[在功能 PR 中整合这些内容并使用 Agent Merge][next-lesson]。

## 资源

- [为 Copilot CLI 创建自定义智能体][create-agents]
- [自定义智能体配置][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
