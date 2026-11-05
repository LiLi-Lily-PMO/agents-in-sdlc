---
title: "练习 2 - 使用 Copilot 代理模式添加新功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

即使是最简单的应用更新，通常也需要修改多个文件并执行一些操作，例如运行测试。开发人员的工作流程通常包括：查找所有相关文件、进行更改、运行测试、调试、发现遗漏的文件、再次更新……这样的工作会不断重复。

Copilot 代理模式正是为此而设计的。

Copilot 代理模式可以在 IDE 中更自主地工作。它的行为类似开发人员：先探索现有项目结构，执行必要的更新，运行测试等任务，并自动修复发现的问题。现在来看看如何使用代理模式为网站添加新功能。

> [!NOTE]
> 代理模式和云智能体的名称相似，但它们针对两种不同的体验。代理模式在 IDE 中执行任务，支持快速反馈和交互。云智能体则像团队成员一样异步工作，通过议题和拉取请求与你协作。

本练习将学习如何：

- 让 Copilot 代理模式探索项目、识别相关文件并协调更改。
- 使用 GitHub Copilot 代理模式跨 UI 和数据层实现新功能。
- 在将代码合并到代码库之前，审查 Copilot 代理模式生成的更改和测试。

## 场景

游戏列表不断增加，因此你希望用户可以按类别和发行商筛选。上一练习已添加发行商辅助函数，现在将使用 Copilot 代理模式完成剩余的数据层、UI 和测试工作。

## 运行 Tailspin Toys 网站

在修改代码前，先浏览 Tailspin Toys 网站，了解其当前功能。

该网站是一个以开发者为主题的桌游众筹平台，允许用户列出游戏并查看详情。网站是单体 Astro 应用，会在构建时将页面渲染为静态 HTML。页面通过 Drizzle ORM 在 frontmatter 中直接查询本地 SQLite 数据库；没有独立后端 API 或客户端 UI 框架。可复用的数据访问辅助函数位于 `src/lib/`，交互功能使用精简且限定作用域的 Astro `<script>` 和标准 DOM API 实现。

### 启动网站

项目提供了 `npm run dev` 任务，用于启动单体 Astro 开发服务器。按照以下步骤在 GitHub Codespace 中运行：

1. 返回 Codespace。上一练习结束后，你应该已切换回 `main`。
2. 在 Codespace 中按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）打开新的终端窗口。
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

   Astro 开发服务器就绪后，会显示类似以下内容的横幅，其中包含 URL：

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> 如果弹出对话框，询问是否要为 `http://localhost:4321` 打开浏览器窗口，请选择 **x** 将其关闭。

5. 在终端中按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），然后选择 Astro 服务器地址 `http://localhost:4321`，打开网站。

> [!NOTE]
> 在 Codespace 中，从终端选择 localhost URL 后，系统会自动将你重定向到 `https://<your-codespace-name>-4321.app.github.dev/`。这是连接到 Codespace 的私有隧道，此时 Web 服务器正在 Codespace 中运行。

### 浏览网站

网站运行后，可以探索其功能，主要包括：

- **主页**：显示桌游列表，包括标题、图片和描述。
- **游戏详情页**：选择游戏后会打开详情页，显示标题、描述、发行商和类别等信息。

## 使用 Copilot 探索待办事项

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始一个干净的对话。模式和模型的选择因练习而异；每个相关步骤都会说明。

网站的初始实现已可正常使用，但我们希望添加新功能。先查看待办事项列表。通过模板设置项目时，系统已自动创建议题待办列表。让 GitHub Copilot 显示这些议题。

1. 在 Chat 视图的智能体下拉列表中选择 **Agent**。Agent 智能体可以自主规划并跨文件实现更改、运行终端命令并调用工具。

   ![Chat 视图中的智能体选择器截图](../../../_images/shared-chat-mode-selector.png)

2. 从可用模型列表中选择 **Claude Sonnet 4.5**。

> [!CAUTION]
> 本工作坊作者并非推荐某一种模型。编写本工作坊时使用了 Claude Sonnet 4.5，因此在说明中列出了该模型。我们希望通过这种方式让代码建议相对一致，从而提供良好体验。不过，由于 LLM 具有概率性，你收到的建议可能与工作坊中的示例不同。这完全正常，也是预期行为。

> [!NOTE]
> 由于 LLM 结果具有概率性，Copilot 可能会使用不同的 MCP 命令，但仍应能完成任务。

3. 向 Copilot 发送以下提示，询问议题待办列表：

   ```plaintext
   请显示我的 GitHub 存储库中的待办事项列表，并根据哪些事项对用户最有用来帮助我确定优先级。
   ```

4. 选择 **Continue** 以运行列出所有议题的命令。
5. 查看生成的议题列表。

注意 Copilot 还根据它认为对用户最有用的事项为列表排序。

## 查看指令文件

让智能体生成代码前，先查看用于向 Copilot 提供工作上下文的指令文件。项目中的[用户界面 (UI)][ui-instructions]指令文件说明了如何为网站添加功能。

1. 在 Codespace 中打开 `.github/instructions/ui.instructions.md`。
2. 查看添加功能的总体指导，包括：
   - 架构概览。
   - 组件设计、可测试性和无障碍原则。
   - 指向各文件类型指令的链接，包括：
     - Astro
     - Tailwind CSS
     - Drizzle

> [!TIP]
> 指令文件可以引用其他指令文件以及项目中的文件。路径相对于指令文件所在位置，因此可以复用指导、将复杂指令拆分成更易管理的部分，并提供示例和模板。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md

## 实现筛选功能

完成筛选至少需要对应用进行三项独立更新：

- 在数据层 (`src/lib/`) 中添加或完善剩余的筛选逻辑。
- 添加或更新筛选行为测试。
- 更新游戏列表页，添加筛选 UI。

此外，在将所有内容合并到代码库之前，必须运行并通过测试。Copilot 代理模式可以代为完成这些任务！现在添加筛选功能。

1. 可以继续使用当前 Copilot 对话，也可以选择 **New Chat** 开始新对话。
2. 选择 **Add Context**、**Instructions** 和 **ui** 指令文件。

   ![选择 UI 指令文件的示例截图](../../../_images/ex3-select-instructions-file.png)

3. 确认 Chat 视图的智能体下拉列表中仍选中 **Agent**。

   ![Chat 视图中的智能体选择器截图](../../../_images/shared-chat-mode-selector.png)

4. 确认模型仍选择 **Claude Sonnet 4.5**。
5. 使用以下提示，让 Copilot 根据待办列表中的相关议题实现功能：

   ```plaintext
   请根据待办列表中相关 GitHub 议题的要求，更新网站以支持按发行商和类别筛选。上一练习已创建发行商辅助函数，请保留并视需要完善已有工作，同时完成剩余的数据层、UI 和测试。结束前确保所有测试通过。服务器已经在运行，无需再次启动。
   ```

6. 观察 Copilot 如何先探索项目并查找相关文件。它应会找到数据层辅助函数、UI 和测试，然后开始修改文件并运行测试。

   ![Copilot 探索项目文件的截图](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot 会执行多个任务，例如探索项目、修改文件和运行测试。根据任务和代码库的复杂程度，这可能需要几分钟。在此过程中，代码编辑器可能会出现 **Keep** 和 **Undo** 按钮。Copilot 完成后，所有更改都会提供 **Keep** 或 **Undo** 选项，因此工作进行中无需选择这些按钮。

7. 按 Copilot 的提示选择 **Continue** 运行测试。

   ![Copilot Chat 面板中的对话框，询问用户是否允许运行测试](../../../_images/ex3-agent-mode-run-tests.png)

8. 过程中可能会有暂停，甚至出现测试失败。这没有关系！Copilot 会在生成代码和运行测试之间反复迭代，直到完成任务且未检测到错误。

   ![Copilot 代理模式完整 Chat 会话截图](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 检查生成的代码，查找潜在问题。

> [!CAUTION]
> 请记住，始终需要审查 Copilot 或其他 AI 工具生成的代码。

10. 返回运行着网站的浏览器，探索新功能！
11. 确认一切正常并完成代码审查后，在 Copilot Chat 窗口中选择 **Keep**。

## 总结与后续步骤

恭喜！本练习介绍了如何使用 GitHub Copilot 代理模式为 Tailspin Toys 网站添加新功能。你学会了如何：

- 使用 GitHub Copilot 代理模式跨 UI 和数据层实现新功能。
- 让 Copilot 代理模式探索项目、识别相关文件并协调更改。
- 在将代码合并到代码库之前，审查 Copilot 代理模式生成的更改和测试。

接下来，使用 [Playwright MCP 服务器测试功能][next-lesson]，并为其创建拉取请求。

### 加分探索练习——实现分页

随着游戏列表不断增加，网站需要支持分页。使用本练习中学到的技能，提示 Copilot 更新网站并实现分页。编写提示和代码时，可以考虑：

- 遵循现有最佳实践，包括使用已有指令文件。
- 考虑分页的实现方式，例如是否允许用户选择每页大小，还是固定大小。
- 编写提示时，提供足够指导，让 Copilot 按照你的预期实现功能。
- 可能需要与 GitHub Copilot 多次迭代，请求更改并提供上下文。这正是与 Copilot 协作的常见方式！

## 资源

- [Copilot 的 ask、edit 和 agent 模式：功能及使用时机][choose-mode]
- [VS Code 中的代理模式][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
