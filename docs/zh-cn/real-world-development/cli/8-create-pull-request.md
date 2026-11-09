---
title: "第 8 课 - 创建并合并功能 PR"
description: "一并审查筛选功能、指令、技能更新、QA 配置和测试，然后创建 PR 并使用 Agent Merge。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

筛选功能实现、指令更新、技能更新、质量保证 (QA) 配置和测试都已保存在同一分支。现在将它们一起审查并创建拉取请求。星级评分 PR 是手动合并的；这一次将使用 **Agent Merge** 管理合并流程。

> [!NOTE]
> 通常会将功能、指令更新、技能更新和 QA 智能体拆分到几个 PR 中。为简化本研讨会，筛选和质量工作流共用一个对话和分支，并将所有更改放入这个 PR。

本课将：

- 了解 Agent Merge 及其如何自动处理合并流程；
- 检查完整的功能更改和验证证据；
- 创建筛选功能 PR；
- 审查后才启用 Agent Merge，并确认 PR 已合并。

## 场景

整个筛选工作流中，你使用 Copilot 规划、实现并验证了功能。Tailspin Toys 现在希望自动化剩余 PR 工作，同时让开发人员保留合并授权权。

## 认识 Agent Merge

**Agent Merge** 会自动处理合并拉取请求所需的剩余工作。启用后，Copilot 会处理阻止 PR 合并的问题，例如修复失败的持续集成 (CI) 检查、回应审查评论，以及在需要时变基；随后，在存储库允许的情况下启用 GitHub 自动合并。

到目前为止，都是由你亲自选择 **Merge pull request**。Agent Merge 可以代为处理，但启用之前仍要先审查更改并确认可以合并。

## 使用 Agent Merge 管理 PR

所有代码完成后，先一起审查，再创建 PR 并让 Agent Merge 管理后续流程。

1. 返回 Codespace。
2. 输入 `/agent` 打开智能体对话框。
3. 从选项列表中选择 **Default**，然后按 <kbd>Enter</kbd>。
4. 输入 `/pr create` 创建新的 PR。
5. 输入 `/pr agentmerge` 启用 Agent Merge。
6. Copilot 会监控 PR 的持续集成流程。所有检查通过后，它会执行合并。
7. 确认 Copilot 显示类似“PR #14 was squash-merged successfully.”的消息。

> [!IMPORTANT]
> Agent Merge 不会绕过必需的审批、分支保护、合并队列、存储库设置或权限限制。如果合并受阻，请阅读报告的原因，并在存储库允许的情况下手动完成审查后的合并。

## 总结与后续步骤

你已自动化开发流程中的多个部分，包括代码生成、代码测试和验证，以及拉取请求处理。本课中你：

- 了解了 Agent Merge 及其如何自动处理合并流程；
- 检查了完整的功能更改和验证证据；
- 创建了筛选功能 PR；
- 审查后启用了 Agent Merge，并确认 PR 已合并。

接下来将[探索更多实用的 Copilot CLI 斜杠命令][next-lesson]，了解上下文、模型、共享和可选的云委派功能。

## 资源

- [使用 Copilot CLI 管理拉取请求][manage-prs]
- [Copilot CLI 命令参考][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
