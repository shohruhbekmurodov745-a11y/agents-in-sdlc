---
title: "第 3 课 - 智能体模式：Plan 和 Autopilot"
description: "使用 Plan 确定方案、使用 Autopilot 根据议题构建筛选功能，并在 Interactive 模式下审查结果。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

我们先对项目进行了小幅功能更新，但更大的更改需要更完善的流程。GitHub Copilot CLI 可以配合组织现有的开发流程，帮助我们以正确的方式构建正确的功能。接下来几课将遵循典型的智能体驱动开发流程：从议题出发构建新功能、确认代码有效且功能符合预期，最后成功合并到项目中。

> [!NOTE]
> 后续功能工作将沿用同一对话和分支。通常会针对不同文件类型使用不同分支或 PR，但本研讨会为了聚焦核心概念，采用了简化方式。

本课将完成以下内容：

- 从 GitHub 议题开始新的 Copilot 对话。
- 在 Plan 模式下明确需求。
- 使用 Autopilot 模式实现新功能。
- 审查代码。
- 在转发的浏览器中手动验证功能。

继续实现此功能时，你还会更新存储库指令、定制现有 `quality-checks` 技能、添加 MCP 验证、创建 QA 智能体并打开功能 PR。

## 场景

Tailspin Toys 的游戏目录不断增长，访客需要按类别和发行商缩小范围。待办列表中的议题描述了此功能，但组合类别等细节需要在编码前确定。你将使用 Plan 模式确定这些决策，然后授权 Autopilot 执行范围明确的实现。

## 背景

将 AI 编码智能体引入开发流程并不会改变基本原则。事实上，这些原则变得更加重要！大多数开发人员的工作流程大致如下：

1. 从描述需求的已登记议题开始。
2. 制定实现计划。
3. 编写并审查代码。
4. 运行测试以验证代码。
5. 手动验证新功能。
6. 创建拉取请求（PR）。
7. 代码审查完成且持续集成流程成功后，合并代码。

> [!NOTE]
> 具体流程会因团队和组织而异，但大多数流程都与上述方式类似。

遵循标准流程可以确保 AI 生成的代码满足需求，并经历与手写代码相同的审查过程。

## 对话模式

**对话模式**控制智能体的自主程度。按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 可在模式之间切换：

- **Interactive**：你与智能体协作。智能体提出更改建议，并等待你的输入后再继续。
- **Plan**：智能体先制定计划，但不能编辑项目文件。
- **Autopilot**：智能体自主工作，编写代码、运行测试并不断迭代，直到完成任务。

先使用 Plan 模式并审查计划，然后再使用 Autopilot 实现。

## 从议题开始

开始筛选工作前，返回 Codespace，确认存储库和终端已准备就绪。

1. 返回 Codespace。如果已停止，请重新启动。
2. 确认星级评分 PR 已合并。
3. 如果终端尚未打开，按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）。
4. 更新 `main`，然后为筛选工作创建分支：

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. 启动 Copilot CLI：

   ```bash
   copilot --yolo
   ```

6. 按两次 <kbd>Tab</kbd> 打开 **Issues** 选项卡。
7. 按 <kbd>A</kbd> 显示所有议题。
8. 使用方向键选中标题为 **Allow users to filter games by category and publisher** 的议题。
9. 按 <kbd>C</kbd> 将议题加入提示，并返回 **Session** 选项卡。

此时会看到提示以 `#7`（或类似编号）开头。使用 `#` 可以将 GitHub 议题或拉取请求（PR）加入上下文。

## 规划筛选功能

规划可以先明确功能或任务的实现方法，然后再交给 Copilot。对于复杂任务，花些时间规划通常很有帮助。现在切换到 Plan 模式，让 Copilot 制定计划。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 切换到 Plan 模式。确认提示下方显示 **Plan**。
2. 在上一步加入的议题引用后输入以下提示：

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot 会开始制定计划：它将先探索项目，再确定最佳方案。

3. 过程中 Copilot 可能会询问筛选功能的具体行为。根据偏好回答，没有错误答案。
4. 计划完成后，按 <kbd>Control</kbd>+<kbd>E</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>E</kbd>（Windows/Linux）展开计划。
5. 滚动查看并审查计划。
6. 如果计划中的任何部分不符合你的决定，请让 Copilot 修改。

## 批准 Autopilot

计划已经制定并审查完毕，现在开始实现。使用 Autopilot 让 Copilot 执行工作。

Autopilot 会让 Copilot 持续迭代，直到它认为任务完成。

1. 选择 **Accept plan and build on autopilot (recommended)**，或已安装版本中名称相近的选项。
2. 确认提示下方显示 **Autopilot**。
3. 观察 Copilot 如何依据计划持续迭代、生成代码并运行测试。

> [!NOTE]
> 批准计划后，实现可能会立即开始，因此务必先审查计划。如果 Copilot 报告缺少依赖项或端口冲突，请先解决设置问题，再将检查视为已完成。

## 审查并验证实现

代码生成后，和其他代码一样，合并前必须审查。现在检查代码并运行网站，确认一切正常。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 进入 Interactive 模式。确认模式指示器不再显示 **Plan** 或 **Autopilot**。
2. 输入 `/diff`，检查筛选功能实现和测试。
3. 对照议题和规划时做出的决定审查结果。
4. 完成代码审查后，按 <kbd>Esc</kbd> 退出差异视图。
5. 查看项目检查的输出，并要求 Copilot 解决任何失败项。

## 浏览新功能

代码看起来不错，但能否正常运行？像之前一样启动应用，并通过 Codespaces 转发端口打开网站。

1. 提示 Copilot 启动应用：

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces 报告端口 `4321` 可用时，选择 **Open in Browser**。
3. 尝试按类别筛选、按发行商筛选，以及计划中确定的组合方式。
4. 确认重置筛选和无结果时的行为符合议题和你的决定。
5. 返回 Codespace，并让 Copilot 停止它启动的开发服务器。

## 总结和后续步骤

你使用不同的对话模式构建并审查了功能。本课中你：

- 从 GitHub 议题开始新的 Copilot 对话。
- 在 Plan 模式下明确需求。
- 使用 Autopilot 模式实现新功能。
- 审查了代码。
- 在转发的浏览器中手动验证了功能。

接下来，通过[使用自定义指令][next-lesson]进一步了解如何按照文档化的实践生成代码。

## 资源

- [GitHub Copilot CLI 中的 Autopilot][autopilot]
- [Copilot CLI 命令参考][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
