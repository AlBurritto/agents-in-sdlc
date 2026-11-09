---
title: "练习 5：迭代 GitHub Copilot 的工作"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

整个练习过程中，你与 GitHub Copilot 一起完成了多项任务，以改善用户体验并添加功能。你要求 Copilot 为代码添加文档、构建供设计团队迭代的相关游戏功能，以及实现包括高对比度和浅色模式切换开关在内的无障碍功能。现在查看代码更改；必要时向 Copilot 提供反馈，进一步完善工作。

### 场景

如前面多次强调的，使用生成式 AI 并不会改变软件设计和 DevOps 的基本原则。始终需要审查生成的代码，并遵循常规 DevOps 流程。现在先审查 GitHub Copilot 为代码文档、相关游戏功能和无障碍功能提出的更改，然后再启用团队其他成员的审查。

## GitHub Copilot 云智能体与安全性

由于 Copilot 云智能体会在无人监督的情况下异步执行任务，系统设置了一些安全限制：

- Copilot 对存储库只有读取权限，对它用于编写代码的分支才有写入权限。
- 云智能体在 GitHub Actions 中运行，并创建独立的临时环境进行工作。
- 运行任何 GitHub Actions 工作流前，都需要人工批准。
- 默认情况下，[对外部资源的访问受到限制](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)，MCP 服务器也包括在内。

## 审查生成的文档

先查看 GitHub Copilot 云智能体创建的第一个拉取请求 (PR)，其中包含代码文档。通过 GitHub.com 上的标准 PR 界面完成审查。

> [!NOTE]
> 查看 PR 时，可能会看到 GitHub Copilot 被防火墙阻止的警告。这是**预期行为**，因为 Copilot 默认无法访问包括外部 MCP 服务器在内的外部资源。如有需要，可以[自定义或禁用 Copilot 云智能体防火墙][agent-firewall]。

1. 返回 github.com 上的存储库。
2. 选择 **Pull Requests**，打开拉取请求列表。
3. 打开标题类似 **Add missing documentation** 的拉取请求。

> [!NOTE]
> 如果 Copilot 仍在处理任务，拉取请求标题会包含 **[WIP]**。此时请等 Copilot 完成工作。这可能需要几分钟，可以稍作休息或回顾已学内容。

4. 拉取请求准备好后，选择 **Files changed** 选项卡并审查更改。

   ![Files changed 选项卡](../../../_images/shared-pr-files-changed.png)

5. 查看新代码，包括新建的 TSDoc 文档注释和其他文档。具体更改可能会有所不同。

   查看更改时，留意 TSDoc 文档注释、TypeScript 约定和文件注释头。这些内容来自第 2 课中查看的自定义指令文件。

6. 审查更新并确认无误后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有工作流等待批准的指示器。
8. 如果有工作流等待批准，请选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. 工作流会在拉取请求的检查区域中排队运行。如果一切正常，单个 Astro 应用的项目检查应该会通过。检查可能需要几分钟。

## 请求 GitHub Copilot 更改

与 Copilot 协作并非单向过程。你可以像 @ 提及团队成员一样，在拉取请求评论或代码行内评论中提及 Copilot。Copilot 会看到这些评论，并启动另一个会话来处理更改。由于结果具有非确定性，无法规定应使用的确切文本。可以要求 Copilot：

- 在每个代码文件顶部添加简要说明文件用途的注释头；
- 为 TypeScript 和 Astro 文件添加 TSDoc 文档注释；
- 创建介绍 Astro 应用结构的 README。

1. 添加评论并提及 **@copilot**，要求它修改生成的文档。可以采用上面的建议，也可以提出其他文档改进要求。
2. 选择 **View Session**，观察 Copilot 如何工作。注意它会启动新会话来完成更新。
3. 可以选择 **Back to pull request** 返回拉取请求。

   ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot 完成更改后，拉取请求中会出现新的提交。
5. 选择 **Files changed** 选项卡审查更改。

可以继续迭代，直到满意为止。完成后，可以将 PR 从草稿改为就绪状态，并合并到 main 分支。

![将 PR 转为就绪状态](../../../_images/ex7-ready-for-review.png)

## 审查相关游戏功能

现在查看 Copilot 为解决“在游戏详情页显示相关游戏”议题而创建的 PR。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Show related games on the game details page** 的 PR。
4. 选择 **Files changed** 选项卡审查生成的代码。
5. 审查更新并确认无误后，返回 **Conversation** 选项卡并向下滚动。
6. 查看是否有工作流等待批准的指示器。
7. 如果有工作流等待批准，请选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. 工作流会在拉取请求的检查区域中排队运行。如果一切正常，单个 Astro 应用的项目检查应该会通过。检查可能需要几分钟。
9. **可选：**可以在 Codespace 中切换到此分支，手动测试相关游戏功能。打开 Codespace 和终端，运行以下命令，并将 `<branch-name>` 替换为 Copilot 创建的分支名，例如 **copilot/fix-8**：

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

Copilot 已构建相关游戏功能！与之前一样，可以通过迭代让 Copilot 云智能体进行更新。例如，可以要求它调整显示的相关游戏数量，或添加文件注释头和 TSDoc 文档注释（请记住：分配此任务时，你还没有更新自定义指令）。与之前一样，在 **Conversation** 选项卡中发表评论即可提出请求，Copilot 会看到评论并启动新会话。

## 审查无障碍功能

最后，审查通过无障碍自定义智能体实现的功能。此 PR 应该同时包含第 3 课中分配的高对比度模式，以及第 4 课中在任务控制中心要求添加的浅色模式。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Add high contrast mode to website** 的 PR。

> [!NOTE]
> 如果 Copilot 仍在处理任务，拉取请求标题会包含 **[WIP]**。此时请等待 Copilot 完成，这可能需要几分钟。

4. 选择 **Files changed** 选项卡审查生成的代码。
5. 审查实现，特别留意：
   - 用于切换模式的 UI 组件；
   - 用于保存用户偏好的本地存储；
   - 高对比度和浅色模式的 CSS 或样式更改；
   - 无障碍属性（ARIA 标签、键盘导航等）；
   - 用于管理模式切换的 JavaScript/TypeScript 代码。
6. 审查更新并确认无误后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有工作流等待批准的指示器。
8. 如果有工作流等待批准，请选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. 工作流会在拉取请求的检查区域中排队运行。如果一切正常，单个 Astro 应用的项目检查应该会通过。检查可能需要几分钟。
10. **可选：**可以在 Codespace 中切换到此分支，手动测试无障碍功能。打开 Codespace 和终端，运行以下命令，并将 `<branch-name>` 替换为 Copilot 创建的分支名：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    然后启动应用，在浏览器中测试高对比度和浅色模式切换开关，并确认页面重新加载后设置仍然有效。

注意，无障碍自定义智能体如何帮助 Copilot 按照无障碍最佳实践实现功能。如果发现无障碍问题或改进点，可以像处理之前的 PR 一样，在评论中提及 **@copilot** 并要求更新。

## 可选练习 - 继续委派工作

云智能体最适合处理真实待办事项，让你能将注意力转移到其他工作。可以在自己的存储库中创建更多议题并分配给 Copilot，培养这种工作习惯。以下是一些建议：

- 在游戏详情页创建支持者兴趣表单；
- 为游戏列表实现分页；
- 为数据访问辅助函数添加输入验证和错误处理。

## 总结

你已完成云智能体练习路径。在这些课程中，你：

- **查看了存储库附带的自定义指令文件**，了解它们之后如何影响云智能体的输出；
- **将议题分配给 Copilot 云智能体**，观察它设置环境、制定计划并异步执行；
- **创建并使用自定义智能体**处理无障碍任务，添加高对比度和浅色模式切换开关；
- **使用 Copilot 智能体页面作为任务控制中心**，监控并引导无障碍会话；
- **审查并迭代云智能体的拉取请求**，提及 `@copilot` 要求更改并批准工作流。

## 回顾与后续步骤

云智能体练习路径已完成。如果想继续探索，其他练习路径可以补充本次学到的内容：

- 🖥️ **[VS Code 练习路径](../../vscode/)** — 直接在 IDE 中探索 Copilot Chat 代理模式和 MCP 集成。
- 💻 **[CLI 练习路径](../../cli/)** — 在终端中交付两项经过审查的更改，体验 Plan 和 Autopilot 模式、智能体技能、自定义 QA 智能体、Playwright MCP、Agent Merge，以及可选的 `/delegate` 云智能体桥接。

也可以在自己的存储库中尝试以下后续任务：

- 将重构或测试覆盖率议题分配给云智能体；
- 为其他领域创建自定义智能体；
- 为不同技术栈设置 `copilot-setup-steps.yml`。

## 资源

- [GitHub Copilot][github-copilot]
- [关于 Copilot 智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流最佳实践][cloud-agent-best-practices]
- [配置 Copilot 云智能体防火墙][agent-firewall]

---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
