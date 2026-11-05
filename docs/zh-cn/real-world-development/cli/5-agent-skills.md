---
title: "第 5 课 - 自定义并使用 quality-checks 技能"
description: "探索现有的 quality-checks 技能、自定义报告格式，并用它验证筛选功能。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

编写代码不只是写代码。我们已经手动验证了代码是否正常运行，也使用指令文件确保代码符合标准。那么测试、lint 以及持续集成 (CI) 中的其他环节该如何处理？

对于这类任务，**智能体技能**最适合！技能可以帮助 Copilot 正确执行这些操作。

本课将学习如何：

- 探索现有的 `quality-checks` 技能。
- 自定义技能结果的格式。
- 重新加载并运行技能。

## 场景

Tailspin Toys 使用 `quality-checks` 技能运行单元测试、lint 和类型检查。团队希望改进报告，让结果更容易阅读。

## 指令、脚本和资源

智能体技能将可复用的任务指令、可执行脚本和支持资源打包起来，供智能体按需加载。技能的核心形式是一个以技能名称命名的文件夹，其中包含 Markdown 文件 `SKILL.md`。该 Markdown 文件包含定义技能名称和描述的 frontmatter、技能功能概览，以及何时调用技能的指导。文件夹中也可以包含智能体调用技能时使用的脚本和其他资源子文件夹。

> [!NOTE]
> 技能不一定需要额外文件或文件夹。Tailspin Toys 的 `quality-checks` 技能只包含 `SKILL.md`，因为它使用项目现有命令。

技能可以放在项目的 `.github/skills` 文件夹中，作为可由团队共享和复用的存储库资源；也可以放在用户技能文件夹 `~/.copilot/skills` 中。

## 探索技能

现在来看看 Tailspin Toys 团队为运行单元测试、lint 和类型检查创建的 `quality-checks` 技能。

1. 返回 Codespace。在 Codespaces 编辑器中打开 `.github/skills/quality-checks/SKILL.md`。
2. 阅读文件顶部的 `name` 和 `description`。描述有助于 Copilot 确定何时调用技能。
3. 阅读指令，了解其如何引导 Copilot 执行测试和 lint 流程。
4. 注意技能目前还没有 **Results output formatting** 部分。

## 更改前先运行技能

技能可以通过 Copilot CLI 命令或自然语言直接调用。现在让 Copilot 运行这三项检查。

1. 返回筛选功能的 Interactive 模式对话。
2. 使用以下提示：

   ```plaintext
   运行 quality-checks 技能，执行单元测试、lint 和类型检查。
   ```

3. 查看最终报告。

## 自定义报告

我们希望报告更清楚地说明运行了什么、是否成功，以及工具实际报告了什么。现在更新技能，让它生成这种报告。

1. 返回 `.github/skills/quality-checks/SKILL.md`。
2. 在文件末尾添加以下部分：

   ```markdown
   ## Results output formatting

   完成后，报告运行的每条命令，并说明其通过、失败或受阻。只有工具报告了测试数量、耗时、错误、警告和其他指标时，才将这些信息包含在报告中。对于任何失败或阻塞项，请指出下一步操作；绝不要将跳过或未完成的检查描述为已通过。
   ```

3. 文件会自动保存。

## 运行更新后的技能

完成更改后，看看效果如何！Copilot CLI 无需重启对话即可重新加载编辑后的技能。

1. 输入：

   ```plaintext
   /skills reload
   ```

2. 使用与之前完全相同的提示：

   ```plaintext
   运行 quality-checks 技能，执行单元测试、lint 和类型检查。
   ```

3. 查看最终报告，并与首次运行的报告进行比较。

## 总结与后续步骤

你自定义并使用了现有智能体技能。本课中，你：

- 探索了用于单元测试、lint 和类型检查的 `quality-checks` 技能。
- 自定义了技能结果的格式。
- 重新加载并运行了技能。

这项更改将与筛选功能一起包含在功能 PR 中。接下来，让 Copilot 通过 [Playwright MCP 服务器][next-lesson]直接与网站交互。

## 更多技能示例

以下社区示例仅供参考，不是额外任务：

- [Agent Skills 规范][skill-spec]
- [贡献工作流：`make-repo-contribution`][contribution-example]
- [需求文档：`prd`][prd-example]
- [图表和捆绑导出脚本：`drawio`][drawio-example]
- [浏览器测试：`webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
