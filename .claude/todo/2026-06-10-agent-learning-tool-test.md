# Agent 学习：`tool-test` 理解通过，待补实操

## 目标

- 建立“每天一个文件夹”的 Agent 学习机制。
- 记录 2026-06-10 实际学习的 `tool-test/` 内容、学习成果、待完成项和下次恢复入口。
- 下次从实操复现继续，不重新讲概念。

## 当前状态

- 已完成：
  - 梳理 `tool-test/` 的文件结构和主线。
  - 理解 `tool_calls`、`ToolMessage`、`bindTools`、agent loop。
  - 理解 MCP tool 与普通 LangChain tool 的区别和联系。
  - 理解业务函数改造成 tool 的设计要点：`name`、`description`、`schema`、稳定返回、失败结构、副作用边界。
  - 已写入学习卡：`docs/learning-notes/2026-06-10-tool-test.md`。
  - 已更新总学习地图：`docs/agent-learning-map.md`。
- 未完成：
  - 还没有亲手写 `query_user_by_id` 练习 tool。
  - 还没有运行并打印真实 `response.tool_calls`。
  - 还没有把工具结果通过 `ToolMessage` 回填后观察最终回复。
- 为什么暂停：
  - 今天 `tool-test/` 的概念理解已经通过，继续写代码会进入下一阶段“实操通过”。

## 不确定项

- 是否直接在 `tool-test/src/` 新增练习文件，还是先作为临时学习脚本单独运行。
- 是否使用现有 `.env` 调模型，还是先写离线 mock 版本观察工具循环。

## 已排除方案

- 方案：把学习进度只写在 todo。
- 排除原因：todo 适合恢复现场，不适合做长期学习台账。
- 证据：本仓库已有 `docs/agent-learning-map.md` 作为长期学习入口。

## Lessons 候选

- IF 每天学习一个课程文件夹 -> THEN 同时更新学习卡、学习台账和 `.claude/todo/.last-session.md`，避免下次从头问。

## 下一步

1. 新建或选择一个练习文件：`tool-test/src/query-user-tool.mjs`。
2. 定义普通函数 `queryUserById(userId)`。
3. 包装成 `query_user_by_id` LangChain tool。
4. 用模型请求“查一下用户 002 的信息”。
5. 打印 `response.tool_calls`、工具执行结果和最终回复。
6. 复盘这个 tool 的 `name`、`description`、`schema`、返回结构是否适合模型调用。

## 验收标准

- `query_user_by_id` tool 能被模型正确请求调用。
- 能观察到真实 `tool_calls`。
- 能观察到工具执行和 `ToolMessage` 回填。
- 能输出最终自然语言回答。
- 能说明这个工具是否只读、失败如何返回、为什么不会直接修改数据。

## 交接摘要

2026-06-10 已完成 `tool-test/` 的概念学习，状态为 `理解通过`。下次不要重讲 `tool_calls` / MCP 基础，直接做 `query_user_by_id` 最小实操，把理解升级成 `实战通过`。

## Resume 入口

- 触发词：
  - `继续 Agent 学习`
  - `继续 tool-test`
  - `todo 接着复习`
  - `把 tool-test 实战补完`
- 恢复顺序：
  1. 按 `AGENTS.md` 运行 `git fetch origin` 和 `git log HEAD..origin/main --oneline`。
  2. 读 `docs/agent-learning-map.md` 的“学习台账”和“当前推荐下一步”。
  3. 读 `docs/learning-notes/2026-06-10-tool-test.md`。
  4. 直接进入 `query_user_by_id` 实操。
- 默认下一步：
  - 在 `tool-test/` 里补一个最小业务 tool 练习。
