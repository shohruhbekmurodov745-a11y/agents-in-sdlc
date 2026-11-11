---
title: "练习 3 - 使用 Playwright MCP 服务器测试功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：智能体模式][previous-lesson] · [下一课：自定义智能体 →][next-lesson]

你刚刚使用智能体模式构建了筛选功能。在创建拉取请求之前，应确认它在浏览器中能够正常工作。无需亲自点击浏览应用，你可以连接 **Playwright MCP 服务器**，让 Copilot 驱动真实浏览器测试功能，然后发布分支并创建 PR。

本练习将完成以下内容：

- 了解模型上下文协议（MCP）是什么，以及 MCP 服务器如何为 Copilot 扩展工具。
- 将 Playwright MCP 服务器添加到工作区。
- 让 Copilot 使用该服务器在浏览器中手动测试筛选功能。
- 发布分支，并为筛选功能创建拉取请求。

## 什么是模型上下文协议（MCP）？

智能体模式能连接编辑器之外的资源时会更加强大。MCP 是 Copilot 实现这一点的方式，是智能体与外部工具和服务通信的标准。

![智能体模式内部工作原理示意图，展示其与上下文、LLM 和工具（包括 MCP 服务器和 VS Code 扩展提供的工具）的交互](../../../_images/ex1-mcp-diagram.png)

[模型上下文协议（MCP）](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/)为 AI 智能体与外部工具和服务通信提供了途径。使用 MCP 后，AI 智能体可以实时访问外部工具和服务，从而通过资源获取最新信息，并通过工具代表你执行操作。

这些工具和资源通过 MCP 服务器访问。MCP 服务器充当 AI 智能体与外部工具和服务之间的桥梁，负责管理智能体与外部工具（例如现有 API 或 NPM 包等本地工具）之间的通信。每个 MCP 服务器都提供一组 AI 智能体可以访问的工具和资源。

以下是几个常见的 MCP 服务器：

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**：提供一组用于管理 GitHub 存储库的 API，让 AI 智能体能够创建存储库、更新现有存储库，以及管理议题和拉取请求。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**：使用 Playwright 提供浏览器自动化功能，让 AI 智能体能够打开网页、填写表单和选择按钮。

还有许多 MCP 服务器可以访问不同工具和资源。GitHub 提供了 [MCP 注册表](https://github.com/mcp)，帮助用户发现并为生态系统做出贡献。

> [!CAUTION]
> 在安全方面，应像对待项目中的其他依赖项一样对待 MCP 服务器。使用前，请仔细审查其源代码、验证发布者并评估安全影响。只使用信任的 MCP 服务器，并谨慎授予其访问敏感资源或执行敏感操作的权限。

## 审查 MCP 配置

`.vscode/mcp.json` 文件用于配置此 VS Code 工作区可使用的 MCP 服务器。

1. 在 Codespace 中打开 `.vscode/mcp.json`。
2. 你会看到已经配置好的 `github` 服务器：

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

`github` 条目来自项目模板，因此 Copilot 才能在上一练习中读取议题待办列表。它使用[远程 GitHub MCP 服务器][remote-github-mcp-server]，无需本地安装任何内容。VS Code 会通过 HTTP 连接到它；Copilot 第一次使用其工具时，你需要通过 GitHub 身份验证。

## 添加 Playwright MCP 服务器

现在添加第二个服务器。[Playwright MCP 服务器][playwright-mcp-server]为 Copilot 提供可控的浏览器，正好可以用于测试功能。

1. 在 `.vscode/mcp.json` 中添加 `playwright` 条目，与 `github` 并列，使文件内容如下：

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. 保存文件。

与远程 `github` 服务器不同，`playwright` 是一个**本地**服务器：VS Code 会在你的计算机上运行 `npx @playwright/mcp@latest` 来启动它。`--headless` 标记要求 Playwright 在无可见窗口的情况下运行浏览器，这在无法显示桌面的 Codespace 中是必需的。

> [!NOTE]
> Tailspin Toys 项目已将 Playwright 用于端到端测试，因此通常已安装 Playwright 所需的浏览器。如果 Copilot 之后报告缺少浏览器，请让它运行 `npx playwright install chromium`，然后重试。

## 启动并信任服务器

VS Code 会**按需**启动 MCP 服务器：Copilot 第一次需要服务器工具时，VS Code 会启动服务器，并要求你确认是否信任它。

1. 在 `.vscode/mcp.json` 中，`playwright` 条目上方会出现 **Start** 操作。选择它以立即启动服务器，并确认配置正确。
2. 如果 VS Code 询问是否信任服务器，请审查配置并选择信任，以便启动服务器。

也可以不手动启动。跳过此步骤后，VS Code 会在下一步中 Copilot 第一次需要使用相关工具时启动服务器，并提示你信任它。

## 测试筛选功能

[Playwright MCP 服务器][playwright-mcp-server]为 Copilot 提供了可操作的真实浏览器。无需亲自点击应用检查工作，智能体可以打开页面、导航、应用筛选条件并读取结果，然后向你总结观察到的内容。无需离开对话即可快速确认功能是否符合预期。

Playwright MCP 服务器通过页面的[无障碍访问树][playwright-mcp-server]而非屏幕截图进行操作。这意味着智能体会像辅助技术一样，根据带标签的结构化元素（按钮、链接、列表项）进行推理，因此快速功能检查也能顺便验证基本无障碍访问情况。

服务器已连接且应用正在运行时，提示 Copilot 测试刚刚构建的筛选功能：

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot 会通过 Playwright MCP 服务器启动浏览器并执行各步骤，然后报告结果。将报告与议题中的验收标准进行对照。如果发现问题，可以继续询问或要求 Copilot 修复代码，然后再创建拉取请求。

> [!NOTE]
> 测试前，应用必须在 `http://localhost:4321` 运行。如果已停止开发服务器，请重新启动后再发送提示。Copilot 第一次使用 Playwright MCP 服务器时，可能需要下载浏览器。如果它报告缺少浏览器，请让它运行 `npx playwright install chromium` 并重试。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## 发布分支并创建拉取请求

确认功能正常后，就可以创建拉取请求（PR）供团队审查。首先发布 `filtering-vscode` 分支。

1. 在 Codespace 中打开 **Source Control** 面板，审查 Copilot 所做的更改。
2. 选择 **+** 图标暂存更改。
3. 使用 **Sparkle** 按钮生成提交消息。

   ![显示 Copilot 更改的 Source Control 面板截图](../../../_images/ex3-source-control-changes.png)

4. 选择 **Publish**，将分支推送到存储库。

## 创建拉取请求

创建拉取请求的方法有很多，包括使用 github.com 和 GitHub 命令行界面（CLI）。由于你已经在使用 GitHub Copilot，不妨让它代为创建 PR。它可以找到相关议题并创建与该议题关联的 PR。

1. 打开 Copilot Chat 面板并选择 **New Chat**，开始新会话。
2. 确认在智能体下拉菜单中选择 **Agent**，以便 Copilot 使用 GitHub 工具。
3. 让 Copilot 为你创建 PR：

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 根据需要选择 **Continue**，允许 Copilot 收集信息并执行操作。第一次使用 GitHub 工具时，系统可能会要求你通过 GitHub 身份验证，请按照提示操作。
5. 留意 Copilot 如何搜索议题、找到正确的议题并创建 PR。
6. 选择 Copilot 返回的链接以查看拉取请求，但**暂时不要合并**。

## 总结和后续步骤

恭喜！本练习中你：

- 了解了模型上下文协议（MCP）是什么，以及 MCP 服务器如何为 Copilot 扩展工具。
- 将 Playwright MCP 服务器添加到工作区。
- 使用它在浏览器中手动测试筛选功能，然后再发布。
- 发布分支并为筛选功能创建拉取请求。

你使用 MCP 服务器测试了功能，但 MCP 只是扩展 Copilot 的一种方式。下一步，[创建自定义智能体][next-lesson]，简化无障碍访问审查等专项任务。

## 资源

- [MCP 是什么，为什么大家都在讨论它？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP 注册表][mcp-registry]
- [VS Code 中的 MCP 服务器][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
