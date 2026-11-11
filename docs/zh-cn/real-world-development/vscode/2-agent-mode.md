---
title: "练习 2 - 使用 Copilot 智能体模式添加新功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

即使是最简单的应用更新，通常也需要修改多个文件并执行一些操作，例如运行测试。开发人员的工作流程通常包括：找到所有相关文件、进行更改、运行测试、调试、发现遗漏的文件、再次修改……类似步骤还有很多。

Copilot 智能体模式正是为此而设计的。

Copilot 智能体模式可以在 IDE 中更自主地工作。它的行为类似开发人员：先探索现有项目结构，再执行必要更新、运行测试等任务，并自动修复发现的问题。接下来看看如何使用智能体模式为网站添加新功能。

> [!NOTE]
> 智能体模式和云智能体的名称相似，但适用于不同的工作方式。智能体模式在 IDE 中执行任务，可以快速反馈和交互。云智能体则像团队成员一样异步工作，通过议题和拉取请求与你协作。

本练习将学习以下内容：

- Copilot 智能体模式如何探索项目、识别相关文件并协调多处更改。
- 如何使用 GitHub Copilot 智能体模式跨 UI 和数据层实现新功能。
- 合并到代码库之前，如何审查 Copilot 智能体模式生成的更改和测试。

## 场景

游戏列表不断增长，因此你希望允许用户按类别和 publisher 筛选。上一练习已添加 publisher 辅助函数，现在将使用 Copilot 智能体模式完成其余数据层、UI 和测试工作。

## 运行 Tailspin Toys 网站

开始更改之前，先浏览 Tailspin Toys 网站，了解它目前的功能。

该网站是一个采用开发主题的桌游众筹平台，用户可以浏览游戏列表和详情。网站是一个单体 Astro 应用，会在构建时将页面渲染为静态 HTML。页面通过 Drizzle ORM 在 frontmatter 中直接查询本地 SQLite 数据库，没有单独的后端 API 或客户端 UI 框架。可复用的数据访问辅助函数放在 `src/lib/` 中，交互功能使用小型、限定范围的 Astro `<script>` 和标准 DOM API 实现。

### 启动网站

项目提供了 `npm run dev` 任务，用于启动单体 Astro 开发服务器。可以按以下步骤在 GitHub Codespace 中运行：

1. 返回 Codespace。上一练习结束后，你应该已切回 `main`。
2. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）在 Codespace 中打开新终端。
3. 为筛选功能创建并切换到新分支：

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 运行以下命令启动网站：

   ```bash
   npm run dev
   ```

   Astro 开发服务器准备就绪后，会显示类似以下内容的横幅，其中包含 URL：

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> 如果弹出对话框，询问是否要打开 `http://localhost:4321` 的浏览器窗口，请选择 **x** 将其关闭。

5. 按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），然后在终端中选择 Astro 服务器地址 `http://localhost:4321`，打开网站。

> [!NOTE]
> 使用 Codespace 时，在终端中选择 localhost URL 会自动将你重定向到 `https://<your-codespace-name>-4321.app.github.dev/`。这是连接到 Codespace 的私有隧道；现在 Web 服务器已在该环境中运行。

### 浏览网站

网站运行后，可以浏览现有功能：

- **主页**：显示桌游列表、游戏标题、图片和说明。
- **游戏详情页**：选择游戏后会打开详情页，其中包含游戏标题、说明、publisher 和类别等信息。

## 使用 Copilot 浏览待办列表

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始新的对话。模式和模型选择因练习而异；每一步会在相关处说明。

网站初始版本可以正常运行，但我们希望增加新功能。先查看待办列表。通过模板设置项目时，系统已自动创建议题待办列表，可以让 GitHub Copilot 显示这些项目。

1. 在 Chat 视图的智能体下拉菜单中选择 **Agent**。Agent 智能体可以自主规划并跨文件实现更改、运行终端命令和调用工具。

   ![Chat 视图中的智能体选择器截图](../../../_images/shared-chat-mode-selector.png)

2. 从可用模型列表中选择 **Claude Sonnet 4.5**。

> [!CAUTION]
> 本研讨会作者并未偏好某个模型。编写研讨会时使用的是 Claude Sonnet 4.5，因此将其列入说明，希望代码建议相对一致，从而提供良好的体验。不过，大语言模型具有概率性，因此建议内容可能与课程示例不同。这是正常情况。

> [!NOTE]
> 由于大语言模型具有概率性，Copilot 可能会使用不同的 MCP 命令，但仍应能完成任务。

3. 向 Copilot 发送以下提示，询问议题待办列表：

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. 选择 **Continue** 以运行列出所有议题的命令。
5. 查看生成的议题列表。

留意 Copilot 还根据对用户的帮助程度为项目确定了优先级。

## 查看指令文件

让智能体生成代码前，先查看用于向 Copilot 提供上下文的指令文件。本练习将使用 [UI 指令文件](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)，其中包含向网站添加功能时的指导。

1. 在 Codespace 中打开 `.github/instructions/ui.instructions.md`。
2. 留意文件中的总体指导，包括：
   - 架构概述。
   - 组件设计、可测试性和无障碍访问原则。
   - 指向各类文件专用指令的链接，包括：
     - Astro
     - Tailwind CSS
     - Drizzle

> [!TIP]
> 指令文件可以引用其他指令文件和项目文件。路径相对于指令文件所在位置，因此可以复用内容、将复杂指令拆成更小的部分，并提供示例和模板。

## 实现筛选功能

要完成筛选功能，至少需要对应用进行三项更新：

- 在数据层（`src/lib/`）添加或完善其余筛选逻辑。
- 添加或更新筛选行为测试。
- 更新游戏列表页，添加筛选 UI。

此外，合并前还需要运行测试并确保通过。Copilot 智能体模式可以帮你处理这些任务。现在开始添加此功能。

1. 可以继续当前 Copilot 对话，也可以选择 **New Chat** 开始新对话。
2. 选择 **Add Context**、**Instructions** 和 **ui** 指令文件。

   ![选择 UI 指令文件的示例截图](../../../_images/ex3-select-instructions-file.png)

3. 确认 Chat 视图的智能体下拉菜单中仍选中 **Agent**。

   ![Chat 视图中的智能体选择器截图](../../../_images/shared-chat-mode-selector.png)

4. 确认模型仍选择 **Claude Sonnet 4.5**。
5. 使用以下提示，让 Copilot 根据待办列表中的相关议题实现功能：

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. 观察 Copilot 如何先探索项目，并找到与功能相关的数据层辅助函数、UI 和测试。然后它会修改文件并运行测试。

   ![Copilot 正在探索项目文件的截图](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot 会执行多个任务，例如探索项目、修改文件和运行测试。根据任务和代码库的复杂程度，整个过程可能需要几分钟。过程中可能会看到代码编辑器中出现 **Keep** 和 **Undo** 按钮。Copilot 完成后，所有更改都会显示相应的 **Keep** 或 **Undo** 按钮，因此无需在工作过程中立即选择。

7. Copilot 提示运行测试时，选择 **Continue**。

   ![Copilot Chat 面板提示用户确认运行测试的截图](../../../_images/ex3-agent-mode-run-tests.png)

8. 过程中可能会暂停，也可能会出现测试失败，这没有问题。Copilot 会在生成代码和运行测试之间反复迭代，直到完成任务且不再发现错误。

   ![完整 Copilot 智能体模式会话的截图](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 检查生成的代码，查找潜在问题。

> [!CAUTION]
> 始终需要审查 Copilot 或其他 AI 工具生成的代码。

10. 返回运行网站的浏览器，体验新功能。
11. 确认功能正常并审查代码后，在 Copilot Chat 窗口中选择 **Keep**。

## 总结和后续步骤

恭喜！本练习介绍了如何使用 GitHub Copilot 智能体模式为 Tailspin Toys 网站添加新功能。你了解了：

- GitHub Copilot 智能体模式如何跨 UI 和数据层实现新功能。
- Copilot 智能体模式如何探索项目、识别相关文件并协调多处更改。
- 合并到代码库之前，如何审查 Copilot 智能体模式生成的更改和测试。

接下来，[使用 Playwright MCP 服务器测试功能][next-lesson]并为其创建拉取请求。

### 加分探索练习 - 实现分页

游戏列表不断增长时，就需要启用分页。使用本练习中学到的技能，提示 Copilot 更新网站并实现分页。编写提示时可以考虑：

- 遵循现有最佳实践，包括使用已有指令文件。
- 规划分页的实现方式，考虑是否允许用户选择页面大小，还是使用固定页面大小。
- 在提示中提供必要的指导，帮助 Copilot 按预期实现功能。
- 可能需要与 GitHub Copilot 迭代沟通、提供上下文并请求更改。这是使用 Copilot 时的正常工作方式。

## 资源

- [Copilot 的 ask、edit 和 agent 模式：用途及适用场景][choose-mode]
- [VS Code 中的智能体模式][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
