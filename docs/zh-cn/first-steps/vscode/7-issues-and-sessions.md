---
title: "第 7 课 - 规划并实现议题"
description: "使用 GitHub 工具从 Copilot Chat 创建议题，然后在新的聊天会话中实现其中一个。"
authors:
  - jamesmontemagno
lastUpdated: 2026-09-28
---

启用 GitHub MCP 后，Copilot 可以直接从聊天中创建并读取议题。

本课将：

- 从 Copilot Chat 创建三个范围明确的 GitHub 议题。
- 为其中一个议题启动新的聊天会话。
- 实现并验证该议题。

## 创建议题

发送以下提示词：

```plaintext
审查太空问答游戏，并提出三个目标明确的功能想法。使用 GitHub 工具为每个想法创建单独的议题，标题清晰、描述以用户为中心且包含验收标准。暂时不要实现这些功能。
```

## 实现一个议题

1. 打开活动栏中的 **GitHub** 视图，检查新建的议题。
2. 选择一个议题，然后选择 Copilot Chat 中的 **+** 按钮，为它启动新会话。
3. 使用 Agent 模式实现该议题，同时保留原始会话以供比较。
4. 创建拉取请求之前，运行[第 3 课][lesson-3]中的集成浏览器冒烟测试。

## 总结与后续步骤

已经创建议题，并在独立会话中实现其中一个。继续学习[第 8 课：审查与合并][next-lesson]。

[lesson-3]: ../3-inspect-and-test/
[next-lesson]: ../8-review-and-merge/
