---
title: "练习 4 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前几课中，你让 Copilot 云智能体处理了三项改进用户体验和添加功能的任务。云智能体虽然能够异步自主工作，但监控这些任务仍然很重要。

可以使用多种工具管理分配给云智能体的任务，包括 GitHub.com 上的[智能体页面][agents-page]。在这个任务控制中心中，可以查看所有带有未关闭拉取请求 (PR) 的智能体任务，查看它执行的操作，甚至引导进行中的会话。

本课将：

- 浏览智能体页面，监控云智能体任务；
- 引导进行中的会话，让智能体添加新功能。

## 场景

团队要求添加高对比度模式后，意识到也可以顺便添加浅色模式。既然正在更新网站样式并添加切换功能，一并加入浅色模式就很合理。现在需要引导智能体，确保它同时实现高对比度和浅色模式。

## 查看 Copilot 云智能体任务

先查看分配给 Copilot 云智能体的任务状态。

1. 打开 [Copilot 智能体页面](https://github.com/copilot/agents)。
2. 查看主面板和左侧面板中的任务列表。列表中应包含已分配给 Copilot 的任务，例如：
   - 更新代码文档；
   - 为游戏修改功能生成 API；
   - 为网站添加高对比度模式。
3. 选择一个正在运行的任务，查看 Copilot 已执行的操作，例如：
   - 从存储库检出代码；
   - 为 Copilot 创建工作环境；
   - 设置 MCP 服务器；
   - 执行完成任务所需的各项步骤。

> [!NOTE]
> 列出的具体步骤会因 Copilot 当前的工作状态和采用的方案而异。

4. 同时留意右侧的拉取请求 (PR) 面板。可以通过它查看 PR 和更改的文件，以便进一步监控。

## 引导云智能体

查看正在进行的任务后，要求 Copilot 在实现高对比度模式时也添加浅色模式切换开关。

1. 选择与添加高对比度模式相关的会话。确切标题会因 Copilot 使用的名称和当前任务状态而异。

   ![任务控制中心中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 观察会话几分钟，直到设置完成并开始工作。此时会出现类似下文所示的消息。
3. 在 **Steer active session while Copilot is working** 对话框中，输入以下提示词：

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![智能体页面中的云智能体任务截图，突出显示“Steer active session while Copilot is working”对话框](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示词。
5. 注意 Copilot 会确认收到提示词，并将其纳入工作流程。

## 让 Copilot 继续工作

Copilot 会继续处理更新后的任务！发送消息时，它正在进行的步骤完成后，会将新要求加入工作流程。

与之前一样，这需要几分钟。此时可以暂停一下，回顾目前学到和探索过的内容。

## 总结与后续步骤

本课介绍了 Copilot 智能体页面——监控和引导 GitHub Copilot 云智能体任务的中心位置。通过任务控制中心，可以跟踪所有进行中和已完成的任务、审查正在进行的工作，并重定向执行中的任务以调整范围或补充指引。

本课介绍了以下概念：

- 使用任务控制中心和智能体页面监控云智能体任务；
- 重定向进行中的会话，以要求添加功能。

Copilot 正在完成无障碍功能，接下来将[迭代 Copilot 创建的拉取请求][next-lesson]。

## 资源

- [Copilot 智能体页面][agents-page]
- [关于自定义智能体][custom-agents]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
