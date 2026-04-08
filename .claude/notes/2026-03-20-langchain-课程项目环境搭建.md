# LangChain 课程项目环境搭建
> 2026-03-20 23:55 | langchain

## 代码真相
- 项目是 NestJS + LangChain 分主题学习 Monorepo，9 个子项目
- cron-job-tool 是唯一生产级代码（Agent + 6 Tool + DB + 定时任务）
- 其余是分主题 demo：prompt-template / output-parser / runnable / rag / memory / tool / milvus
- LangChain 本质就是"大模型流水线框架"，类比 Express——Chain = middleware pipeline

## 设计决策
- API 选了硅基流动（SiliconFlow）：注册送额度，Qwen/Qwen2.5-7B-Instruct 免费
- Embedding 模型选 BAAI/bge-m3（硅基流动免费）
- MySQL 密码改成了本机实际密码 Xihe@2026（代码硬编码的 admin 已改）

## 环境状态
- ✅ Node.js v24.14.0 + npm 11.9.0
- ✅ 9/9 子项目 node_modules 已安装（用了 --legacy-peer-deps）
- ✅ 9/9 子项目 .env 已配置
- ✅ MySQL 8.4 运行中，hello 数据库已创建
- ✅ hello-nest-langchain 已跑通（/ai/chat 接口正常）
- ⏳ 其余子项目待验证
- ❌ Docker 未启动（Milvus 相关项目需要）
- ❌ cron-job-tool 邮件/搜索配置未填（MAIL_*, BOCHA_API_KEY）

## 陷阱记录
- Kimi API Key 验证失败，换了硅基流动才通
- npm install 在 Node 24 下部分包有 peer dep 冲突，需 --legacy-peer-deps
- NestJS 后台进程不 kill 会占端口 3000 导致 EADDRINUSE

## 私人笔记
- 课程值得学但不是终点，关键是改造成垂直场景产品
- 近钱方向：企业 RAG 知识库（2-10万）、AI Agent 外包、Tool calling 集成
- "技术门槛低，工程经验门槛高"——会调通 demo 容易，稳定上线是另一回事
