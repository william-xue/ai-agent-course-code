# Agent 技术学习地图
> 2026-06-08 | 范围：`ai-agent-course-code` | 目标：先学会 Agent 工程基本功，再迁移到 TEAP Agent

## 一句话定位

本仓库是 **Agent 技术训练场**，不是 TEAP 业务仓库。这里先把 tool calling、structured output、RAG、memory、LangGraph、eval、change set 等能力练熟，再把成熟模式迁移到 `/Users/xueyuan/Desktop/teap-agent`。

学习原则：

- 每次只学一个能力点。
- 先跑课程最小示例，再解释机制。
- 学完必须写“迁移到 TEAP Agent 时怎么用”。
- 不在这里接 TEAP 真实业务数据，不写 TEAP 专用实现。
- 不追框架名，优先掌握可迁移机制。
- 评估 agent 时不看“像不像”，看成功路径是否可解释、失败原因是否可定位、危险动作是否可拦截、质量是否可评测。

## 我们的 Agent 工程标准

标准笔记：

- `.claude/notes/2026-06-10-agent-engineering-standards.md`

核心判断：

```text
Agent 的难点不是“做得像人”，而是“失败时不像随机人”。
```

后续学习和 TEAP Agent 迁移都按这个标准看：

- LLM 负责理解人话、拆解意图、提出候选动作、解释结果、生成草案。
- 代码、规则引擎、仿真器、优化器、校验器负责业务事实、确定性计算、权限和落盘动作。
- tool calling 必须有 schema 校验、权限边界、`toolCallId` trace 和错误回填。
- RAG 必须返回可追踪的证据包，不把大量原始资料直接塞给模型。
- 没有 eval / trace / 失败复现的 agent，只能算 demo，不能算可靠系统。

## 三条仓库分工

| 仓库 | 定位 | 不做什么 |
|---|---|---|
| `ai-agent-course-code` | Agent 技术训练场 | 不承载 TEAP 业务落地 |
| `cloudpss_reverse` | X6 / CloudPSS 图编辑器内功场 | 不再作为 TEAP 主产品仓库 |
| `teap-agent` | TEAP Agent 业务落地仓库 | 不从零教 Agent 框架，不做 CloudPSS clone |

## 学习顺序

### 1. Tool Calling / 工具调用

目标：

- 搞懂 LLM 为什么不能直接改数据。
- 搞懂工具定义、参数 schema、工具结果、错误处理。
- 搞懂“判断交给 LLM，确定性动作交给代码”。

课程入口：

- `tool-test/`
- `output-parser-test/src/tool-calls-*.mjs`
- `hello-nest-langchain/`

迁移到 TEAP Agent：

- `validateTeapModel`
- `locateIssue`
- `explainIssue`
- `createChangeSet`
- `validateChangeSet`

学习产物：

- 一张 tool calling 调用链图。
- 一个“工具定义应该怎么写”的规则卡。
- 一个 TEAP 工具候选清单。

### 2. Structured Output / 结构化输出

目标：

- 让 LLM 输出可解析、可验证、可测试的数据。
- 区分“自然语言解释”和“机器可执行结构”。
- 学会失败时重试、修正和降级。

课程入口：

- `output-parser-test/`
- `output-parser-test/src/structured-json-schema.mjs`
- `output-parser-test/src/with-structured-output.mjs`
- `output-parser-test/src/stream-with-structured-output.mjs`

迁移到 TEAP Agent：

- 校验结果 `Issue[]`。
- 修复建议 `FixProposal[]`。
- 变更集 `ChangeSet`。
- 回写前审批摘要。

学习产物：

- TEAP `Issue` / `ChangeSet` 的草案 schema。
- 一份“什么时候必须结构化输出”的判断表。

### 3. Memory / 记忆

目标：

- 区分 conversation memory、project memory、user preference、long-term facts。
- 搞懂 memory 不是聊天记录堆积，而是可检索的决策和事实。
- 学会 memory 的写入边界：哪些该记，哪些不该记。

课程入口：

- `memory-test/`
- `redis-test/`
- `deepagents-test/src/deepagents/memory-agent.mjs`

迁移到 TEAP Agent：

- 记住工程级校验历史。
- 记住用户确认过的批量修改偏好。
- 记住常见错误和修复策略。
- 不记真实敏感工程数据。

学习产物：

- TEAP Agent memory 分层表。
- “哪些内容不进 memory”的红线清单。

### 4. RAG / 上下文检索

目标：

- 搞懂 naive RAG、query routing、multi-hop、web fallback、hybrid retrieval 的差别。
- 学会把结构化数据转成可检索上下文，而不是直接把大 JSON 塞给 LLM。
- 学会 rerank 和 query augment 的价值边界。

课程入口：

- `rag-test/`
- `advanced-rag/`
- `milvus-test/`
- `es-test/src/rag/`
- `neo4j-graphrag/`

迁移到 TEAP Agent：

- 从 TEAP Semantic IR 渲染 contexts。
- 查询“这个错误影响哪些设备/场景/约束”。
- 检索历史修复案例。
- 图谱检索只在真实需要路径查询时再上，不提前复杂化。

学习产物：

- 一份 TEAP contexts 目录设计草案。
- 一张 RAG 技术选择表：Milvus / ES / Neo4j / 纯内存。

### 5. LangGraph / Agent Loop

目标：

- 搞懂 graph node、state、conditional routing、interrupt、retry、checkpointer。
- 学会把 agent 从“单轮问答”升级成“有状态流程”。
- 学会人工确认点怎么插入流程。

课程入口：

- `langgraph-test/`
- `langgraph-test/src/basic-graph.mjs`
- `langgraph-test/src/conditional-routing.mjs`
- `langgraph-test/src/graph-interrupt.mjs`
- `langgraph-test/src/loop-retry.mjs`
- `langgraph-test/src/multi-agent-supervisor.mjs`

迁移到 TEAP Agent：

```text
load model
-> build IR
-> validate
-> explain issues
-> propose fixes
-> create change set
-> user approve
-> apply / reject
```

学习产物：

- TEAP QA Agent 流程图。
- interrupt / approve / rollback 节点设计。

### 6. Eval / 质量评估

目标：

- 学会用测试证明 agent 没有乱编、越权、漏检。
- 区分代码测试、工具测试、LLM 输出评估。
- 学会把常见错误固化成回归集。

课程入口：

- `langsmith-test/`
- `langsmith-test/src/eval/`

迁移到 TEAP Agent：

- 校验结果必须稳定。
- 相同工程输入不能随机输出不同错误码。
- LLM 解释可以变化，但结构化 issue/change set 必须可测。

学习产物：

- TEAP Agent eval 清单。
- 5 个最小回归样例。

### 7. DeepAgents / Deep Research

目标：

- 学会 subagent、skills、workspace、summarization 的边界。
- 判断什么时候需要多 agent，什么时候一个工具链足够。
- 避免为了“高级”而过度编排。

课程入口：

- `deepagents-test/`
- `deep-research-assistant/`

迁移到 TEAP Agent：

- 只有出现多个独立任务时才考虑 subagent：
  - 一个 agent 读数据格式。
  - 一个 agent 归纳校验规则。
  - 一个 agent 生成报告。
- 主流程仍由确定性工具控制。

学习产物：

- 多 agent 使用门槛表。
- TEAP Agent 暂不使用多 agent 的理由。

## 每次学习固定输出

每次只选一个主题，产出一张学习卡：

```markdown
# 主题

## 我学到的机制

## 课程代码入口

## 调用链

## 关键坑

## 迁移到 TEAP Agent

## 下次继续
```

建议放到：

```text
docs/learning-notes/YYYY-MM-DD-topic.md
```

## 学习机制：每天一个文件夹

目标不是把所有代码背下来，而是每天把一个文件夹学成一张可恢复、可迁移的能力卡。

每天固定流程：

1. 按 `AGENTS.md` 先检查远端更新。
2. 选定当天文件夹，只学一个主机制。
3. 先读入口、依赖、核心调用链，再决定要不要运行脚本。
4. 用自己的话回答三个问题：
   - 这个文件夹解决什么工程问题？
   - 核心调用链是什么？
   - 这个机制迁移到 TEAP Agent 时对应什么能力？
5. 写入 `docs/learning-notes/YYYY-MM-DD-topic.md`。
6. 更新下面的学习台账和 `.claude/todo/.last-session.md`。

学习状态分级：

| 状态 | 含义 | 下一步 |
|---|---|---|
| `理解通过` | 能复述机制和调用链，但还没亲手复现 | 做一个 30-60 分钟小实操 |
| `实战通过` | 能独立写最小 demo，并看懂运行结果 | 进入下一个文件夹 |
| `待补` | 主线还没讲清或依赖没跑通 | 下次继续同一文件夹 |

## 学习台账

| 日期 | 文件夹 / 主题 | 实际学习内容 | 学习成果 | 待完成 | 状态 |
|---|---|---|---|---|---|
| 2026-06-10 | `tool-test/` / Tool Calling + MCP | 梳理 `hello-langchain.mjs`、`tool-file-read.mjs`、`all-tools.mjs`、`mini-cursor.mjs`、`my-mcp-server.mjs`、`langchain-mcp-test.mjs`、`mcp-test.mjs`；理解 `tool_calls`、`ToolMessage`、agent loop、MCP tool 与普通 tool 的关系；讨论业务函数如何改造成模型可调用 tool。 | 能复述“模型只返回工具调用请求，Node.js 执行工具，ToolMessage 回填结果”；能区分普通 tool 与 MCP 接入层；能说出 tool 设计需要 `name` / `description` / `schema` / 稳定返回 / 副作用边界。 | 补一次实操：新建 `query_user_by_id` 练习 tool，打印真实 `response.tool_calls`，跑通工具结果回填和最终回复。 | `理解通过` |

## Resume 入口

触发词：

- `继续 Agent 学习`
- `agent 学习仓库`
- `学 tool calling`
- `学 LangGraph`
- `学 RAG`
- `学 eval`

恢复顺序：

1. 先按 `AGENTS.md` 跑 `git fetch origin` 并检查远端新提交。
2. 读本文件。
3. 选一个主题。
4. 跑课程最小示例。
5. 写学习卡。
6. 标注怎么迁移到 `/Users/xueyuan/Desktop/teap-agent`。

## 当前推荐下一步

继续 **Tool Calling / 工具调用** 的实操复现。

原因：

- 2026-06-10 已完成 `tool-test/` 的概念理解，状态是 `理解通过`。
- 还需要补一次最小实操，把 `queryUserById` 包装成 `query_user_by_id` tool。
- 实操通过后，再进入 `output-parser-test/` 学 structured output。
