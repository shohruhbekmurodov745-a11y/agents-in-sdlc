---
title: "第 10 课 - 回顾和后续步骤"
description: "回顾 Copilot CLI 工作流、两个拉取请求、可复用的自定义项和更多资源。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

你在 Tailspin Toys 的连续工作流中使用了 GitHub Copilot CLI。你：

- 准备了 Codespace、安装 Copilot CLI、探索项目并找到预填充的筛选议题。
- 添加星级评分，在转发的浏览器中检查结果，并手动合并了第一个拉取请求（PR）。
- 从筛选议题开始，在 Plan 模式中制定方法，在 Autopilot 模式中实现，并在 Interactive 模式中审查。
- 使用自定义指令引导智能体，随后定制现有的 `quality-checks` 技能，并使用它运行单元测试、lint 和类型检查。
- 添加 Playwright 模型上下文协议（MCP）服务器，并用它在真实浏览器中探索筛选功能。
- 创建并选择质量保证（QA）自定义智能体，以评估需求、覆盖范围、技能结果和浏览器证据。
- 审查完整的筛选功能更改，并授权 Agent Merge 合并筛选功能 PR。
- 探索用于管理上下文、模型、共享和可选云委派的斜杠命令。

## 交付的内容

本研讨会包含两个 PR 里程碑，各自基于更新后的 `main` 分支创建独立分支：

1. **星级评分**：在游戏卡片上显示现有的 `starRating`，并明确显示未评分状态。
2. **筛选和质量工作流**：实现筛选、更新指令并将其应用于功能、定制 `quality-checks` 报告、创建 QA 配置，并加入相关测试。

从规划筛选功能到创建 PR，你始终使用同一个对话和分支。为了简化本研讨会，我们将相关工作合并到一个 PR 中。

## 不同类型的验证

你通过多种方式检查代码：自动化测试、亲自在浏览器中检查，以及通过 MCP 使用 Copilot 探索浏览器。`quality-checks` 技能运行单元测试、lint 和类型检查，并按新格式报告结果。QA 智能体结合这些结果，在 PR 创建前检查需求和测试覆盖范围。

新增测试应解决实际缺口；如果 QA 检查后不需要增加测试，也可能是正确结果。授权合并前，请审查代码和证据；更改代码后，应重新生成受影响的证据。

## 最佳实践

提供给 Copilot 的上下文和工具会影响它的工作。在本研讨会中，你更新了指令、定制了技能、创建了 QA 配置并设置 MCP 服务器。可以在多个对话中复用这些自定义项，并根据团队需求调整。指令用于设定标准，技能用于描述可重复的任务，自定义智能体用于定义专家角色，MCP 服务器用于连接外部工具。请审查实际更改和工具结果，而不只是智能体的总结。

根据任务选择合适的**模式和模型**。使用 **Plan** 在构建前梳理方案，使用 **Interactive** 参与范围明确的更改，并使用 **Autopilot** 执行范围清晰的任务。常规编辑可以选择速度更快的模型，复杂工作则选择能力更强的模型。

上下文与基础设施同样重要。清晰说明要构建**什么**、**为什么**需要构建，以及**如何**实现，会显著影响输出。

## 更多可探索的功能

你已学习核心工作流。还可以探索以下 CLI 功能：

- 使用 `/review` 请求代码审查智能体分析更改。
- 使用 `/rubber-duck` 讨论问题并获得另一种思路。
- 使用 `/fleet` 并行协调独立子任务。
- 使用 `/worktree` 隔离其他任务。
- 使用 `/delegate` 将任务发送给 Copilot 云智能体。

## 后续步骤

提升任何工具使用能力的最佳方式都是持续实践！可以用它编写生产代码、兴趣项目，或者多年来一直想做但尚未开始的小应用。与团队分享经验并向他人学习；也别忘了继续探索文档。

如果想继续在终端中扩展 Tailspin Toys，可以接着学习可选的 [Foundry Backer Concierge 系列][foundry]。若要比较其他环境，可以探索 [VS Code 研讨会][vscode]、[GitHub Copilot app 研讨会][app]或 [Copilot 云智能体研讨会][cloud]。

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
