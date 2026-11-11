---
title: "第 4 课 - 使用自定义指令引导 Copilot"
description: "浏览存储库指令、添加文档标准，并将其应用于筛选功能代码。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，应确保 Copilot 能访问相关指导。[指令文件][instruction-files]不仅说明你想要什么代码，也说明代码应该如何组织。现在筛选功能已经构建完成，可以查看 Copilot 使用的指令、添加文档标准并将其应用于代码。

本课将完成以下内容：

- 了解存储库指令和路径范围指令文件如何传递给智能体。
- 更新指令文件，确保遵循编码标准。
- 查看指令文件对代码的影响。

## 场景

Tailspin Toys 作为一家优秀的软件开发公司，有一套开发实践指南和要求，包括：

- 注释应解释意图和不明显的决策，而不是复述代码。
- `db/` 和 `src/lib/` 中导出的函数应使用 TSDoc/JSDoc 说明用途、参数和返回值，并记录存在的可注入 `db` 参数。
- 可复用 Astro 组件应说明其 `Props` 接口，并在相关代码更改时保持注释更新。
- 应保留现有格式和 lint 指南。

通过使用指令文件，可以让 Copilot 获得正确的信息，并按照这些实践完成任务。

## 指令文件

自定义指令可向 Copilot 提供上下文和偏好设置，帮助它理解你的编码风格和要求。借助这一强大功能，可以引导 Copilot 提供更相关的建议和代码片段。你可以指定偏好的编码约定、库，甚至希望代码包含哪种注释。既可以为整个存储库创建指令，也可以为特定文件类型或任务提供指令。

指令文件分为两种：

- `.github/copilot-instructions.md` 是一个指令文件，会随存储库的**每个**请求一起发送给 Copilot。此文件应包含适用于大多数请求的项目级信息。
- `.github/instructions/*.instructions.md` 文件可针对特定语言、文件类型或任务提供指导。

> [!NOTE]
> 其他指令格式及其支持情况会因环境而异。依赖特定格式前，请查看[自定义指令支持参考][custom-instructions-support]。

## 浏览本项目中的自定义指令文件

入门项目已包含一组指令文件。先浏览这些文件，然后再进行修改，以了解指令的效果。

1. 返回 Codespace。
2. 在 Codespaces 编辑器（不是终端）中打开 `.github/copilot-instructions.md`。
3. 浏览项目简介和编码指导。此文件中的指令适用于 Copilot 在本存储库中的每次交互。
4. 打开 `.github/instructions` 文件夹并浏览其中的文件。注意其中有 Astro 文件、Drizzle 数据层、测试等方面的指令。
5. 打开 `.github/instructions/unit-tests.instructions.md`。留意顶部的 `applyTo` 字段，它用于确定指令适用于哪些文件。
6. 打开 `.github/instructions/drizzle.instructions.md`，查看它指向的其他指令文件和项目现有文件。这样可以将较大的指令集拆分成更小、可复用的文件，并为 Copilot 提供示例。

## 更新指令文件，使其符合团队指南

现有文件是不错的起点，但仍有缺失。修改核心 `copilot-instructions.md` 文件，确保新生成的 TypeScript 代码包含 TSDoc 注释。

1. 在 `.github/copilot-instructions.md` 中找到 **Code formatting guidance** 部分，通常在第 35 行附近。
2. 在该部分最后添加以下条目：

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

文件会自动保存。

## 使用更新后的指导

Copilot CLI 会在对话启动时加载存储库指令。保存更新后的指令后，退出并恢复筛选功能对话，以便 Copilot 在保留功能上下文的同时加载新指导。

1. 如果筛选功能对话仍然打开，请输入 `/exit` 关闭。Copilot CLI 会自动保存对话。
2. 在 `game-filters-cli` 分支的存储库根目录中，恢复已保存的对话：

   ```bash
   copilot --resume
   ```

   如果系统提示，请选择筛选功能对话，并确认当前检出的分支为 `game-filters-cli`。
3. 提示 Copilot 应用更新后的指导：

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. 输入 `/diff` 并查看更改的 TypeScript 文件。留意新生成的 TSDoc 注释，并确认它们准确说明了代码。

## 总结和后续步骤

你了解了 Copilot CLI 如何获取指令文件中的上下文，并将新标准应用于功能代码。具体来说，你：

- 了解了存储库指令和路径范围指令文件如何传递给智能体。
- 更新指令文件，确保遵循编码标准。
- 查看指令文件对代码的影响。

接下来，[定制并运行可复用的 quality-checks 技能][next-lesson]，确保一致地运行 lint 和测试。

## 资源

- [为 Copilot CLI 添加自定义指令][instruction-files]
- [自定义指令支持][custom-instructions-support]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [Awesome Copilot：指令文件及其他资源集合][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
