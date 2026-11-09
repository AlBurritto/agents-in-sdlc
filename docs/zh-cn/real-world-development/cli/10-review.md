---
title: "第 10 课 - 总结与后续步骤"
description: "回顾 Copilot CLI 工作流、两个拉取请求、可复用自定义功能和更多资源。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

你已在 Tailspin Toys 的连续工作流中使用 GitHub Copilot CLI。你：

- 准备了 Codespace、安装 Copilot CLI、探索项目并找到已创建的筛选议题；
- 添加星级评分，在转发的浏览器中审查结果，并手动合并第一个拉取请求 (PR)；
- 从筛选议题开始，在 Plan 模式中确定方案、用 Autopilot 模式实现，再用 Interactive 模式审查；
- 使用自定义指令指导智能体，然后自定义现有的 `quality-checks` 技能，并用它运行单元测试、lint 和类型检查；
- 添加 Playwright Model Context Protocol (MCP) 服务器，并在真实浏览器中检查筛选功能；
- 创建并选择质量保证 (QA) 自定义智能体，检查需求、覆盖率、技能结果和浏览器证据；
- 审查完整的筛选功能更改，并授权 Agent Merge 合并筛选 PR；
- 探索用于管理上下文、模型和共享的斜杠命令，以及可选的云委派功能。

## 交付内容

本研讨会包含两个 PR 里程碑，分别从更新后的 `main` 分支创建独立分支：

1. **星级评分：**在游戏卡片上显示已有 `starRating`，并明确显示未评分状态。
2. **筛选和质量工作流：**实现筛选、更新指令并将其应用于功能、自定义 `quality-checks` 报告、创建 QA 配置，并添加相关测试。

从规划筛选功能到创建 PR，你始终使用同一个对话和分支。为简化研讨会，我们将这些工作合并到一个 PR 中。

## 不同类型的验证

你使用多种方式检查代码：自动化测试、亲自在浏览器中检查，以及通过 MCP 让 Copilot 浏览器探索。`quality-checks` 技能使用新报告格式运行了单元测试、lint 和类型检查。QA 还结合这些结果，检查了需求和测试覆盖范围，然后再创建 PR。

新增测试应解决实际覆盖缺口；如果 QA 检查后认为无需新测试，也可能是合理结果。授权合并前，应审查代码和证据；更改后应重新运行受影响的验证。

## 最佳实践

提供给 Copilot 的上下文和工具会影响它的工作。在本研讨会中，你更新了指令、自定义了技能、创建了 QA 配置并设置 MCP 服务器。可以在多个会话中复用这些自定义设置，并随着团队需求变化进行调整。指令用于设定标准，技能描述可重复执行的任务，自定义智能体定义专用角色，MCP 服务器连接外部工具。应审查实际更改和工具结果，而不只是智能体的总结。

根据任务选择合适的**模式和模型**。使用 **Plan** 在实现前考虑方案；使用 **Interactive** 参与聚焦更改的全过程；对范围明确的任务使用 **Autopilot**。日常修改可选择速度较快的模型，复杂工作则选择能力更强的模型。

上下文与基础设施同样重要。清晰说明要构建**什么**、**为什么**构建，以及**如何**构建，能够显著影响输出。

## 更多可探索的功能

核心工作流已完成。还可以继续了解一些 CLI 功能：

- 使用 `/review` 请求代码审查智能体分析更改；
- 使用 `/rubber-duck` 讨论问题并获得另一种观点；
- 使用 `/fleet` 并行协调独立子任务；
- 使用 `/worktree` 隔离其他任务；
- 使用 `/delegate` 将任务发送给 Copilot 云智能体。

## 后续步骤

提升工具使用能力的最佳方式就是持续练习！可以将它用于生产代码、兴趣项目，或多年来一直想构建却没开始的小应用。与团队分享经验，也向团队成员学习。别忘了继续浏览文档。

如果想继续从终端扩展 Tailspin Toys，可以学习可选的 [Foundry Backer Concierge 系列][foundry]。若想比较其他工作环境，可以探索 [VS Code 研讨会][vscode]、[GitHub Copilot app 研讨会][app]或 [Copilot 云智能体研讨会][cloud]。

## 资源

- [关于 GitHub Copilot CLI][about-cli]
- [Copilot CLI 命令参考][cli-reference]
- [自定义 Copilot CLI][customize-cli]
- [使用 Copilot CLI 管理拉取请求][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
