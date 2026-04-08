# ai-agent-course-code

AI Agent 课程练习仓库：NestJS + LangChain 分主题示例集合。

## 会话开场强制规则（每次必须执行）

**每次会话开始，必须先执行以下步骤，不得跳过：**

1. 运行 `git fetch origin` 检查远程是否有新提交
2. 对比 `git log HEAD..origin/main --oneline`，如果有新提交：
   - 询问用户是否要 pull（本地有未提交改动时先 stash）
   - pull 完成后，读取新增/修改的核心文件，总结值得学习的内容
3. 如果没有新提交，一句话告知即可

## 10 秒工作卡

- 默认轻量：先做结论，按需展开
- 改代码前先读调用链；改 LangChain 链路先确认 model/provider 配置
- 脚本测试（`*-test/`）直接 `npm test`；NestJS 项目需先 `.env` 配置再 `npm run start:dev`
- 小任务直做，机械活分流

## 项目画像

- **类型**：学习 Monorepo（NestJS + LangChain 课程示例）
- **分类**：backend / AI agent
- **技术栈**：Node.js + TypeScript + NestJS + LangChain + TypeORM + MySQL + Zod
- **机器画像**：`.claude/project-profile.json`
- **主路由**：`/feature` / `/quick`
- **主 Skill**：`node-best-practices`、`claude-api`

## 子项目结构

| 目录 | 类型 | 说明 |
|------|------|------|
| `cron-job-tool/` | NestJS App | 完整应用：TypeORM+MySQL、定时任务、邮件、LangChain tools |
| `hello-nest-langchain/` | NestJS App | NestJS + LangChain 基础集成示例 |
| `*-test/` | 独立脚本 | 单主题练习：memory / milvus / rag / runnable / tool 等 |

## 常用命令

```bash
# NestJS 应用（cron-job-tool / hello-nest-langchain）
npm run start:dev     # 开发模式
npm run build         # 构建
npm run test          # 单元测试

# 脚本类子项目（*-test/）
npm test              # 直接运行
```

## 关键文件

| 文件 | 用途 |
|------|------|
| `cron-job-tool/src/` | NestJS 模块根目录 |
| `cron-job-tool/src/ai-tools/` | LangChain tool 定义 |
| `hello-nest-langchain/src/` | LangChain 基础链路示例 |

## 注意事项

- **环境变量**：NestJS 项目依赖 `.env`（OpenAI API Key、DB 连接），启动前必须配置
- **LangChain model 字段**：使用 `model` 而非旧版 `modelName`（见 commit e242544）
- **MySQL**：仅 `cron-job-tool` 需要数据库，其余子项目无 DB 依赖
- NEVER commit `.env` files
