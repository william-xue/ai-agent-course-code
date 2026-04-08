# LangChain 课程项目全部跑通
> 2026-03-20 | langchain / env-setup

## 目标
- [x] 安装所有子项目 node_modules
- [x] 配置所有子项目 .env（硅基流动 API）
- [x] 创建 MySQL hello 数据库 + 修正密码
- [x] 跑通 hello-nest-langchain
- [x] 跑通 output-parser-test
- [ ] 跑通 prompt-template-test
- [x] 跑通 runnable-test
- [x] 跑通 tool-test
- [x] 跑通 memory-test
- [x] 跑通 rag-test
- [ ] 跑通 cron-job-tool（需补邮件/搜索配置）
- [ ] 跑通 milvus-test（需 Docker + Milvus）
- [ ] 跑通 tts-stt-test（需腾讯云 SECRET_ID/KEY/APP_ID）
- [ ] 跑通 asr-and-tts-nest-service（需腾讯云凭证）

## 验收标准
- 每个子项目至少有一个脚本/接口能正常返回 AI 回答或音频
- 不要求所有功能都通（邮件、Milvus、腾讯云等可选功能可后补）

## 验证命令
- `cd hello-nest-langchain && npx nest start`，然后 `curl localhost:3000/ai/chat?query=hi`
- `cd output-parser-test && npm test`
- `cd runnable-test && npm test`
- `cd tool-test && npm test`
- `cd memory-test && npm test`
- `cd rag-test && npm test`
- `cd cron-job-tool && npx nest start`，然后 `curl localhost:3000`
- `cd tts-stt-test && node src/tts-test.mjs`
- `cd asr-and-tts-nest-service && npm run start:dev`

## 当前阶段
- 脚本类项目基本跑通：output-parser-test / runnable-test / tool-test / memory-test / rag-test
- 剩余：prompt-template-test（部分依赖 Milvus）、cron-job-tool（邮件/搜索配置）、milvus-test（Docker）、tts-stt-test / asr-and-tts-nest-service（腾讯云凭证）

## 已完成
- 2026-03-20: node_modules 全部安装（--legacy-peer-deps）
- 2026-03-20: .env 全部配置（硅基流动 Qwen/Qwen2.5-7B-Instruct）
- 2026-03-20: MySQL hello 库创建，密码改为 Xihe@2026
- 2026-03-20: hello-nest-langchain /ai/chat 接口正常返回
- 2026-03-29: 拉取远程新提交，新增 tts-stt-test 和 asr-and-tts-nest-service 两个子项目
- 2026-03-29: 跑通 output-parser-test / runnable-test / tool-test / memory-test / rag-test

## 当前阻塞
- Docker 未启动：milvus-test、prompt-template-test 部分脚本依赖 Milvus
- cron-job-tool 邮件配置未填（MAIL_*）、搜索 API 未填（BOCHA_API_KEY）
- tts-stt-test / asr-and-tts-nest-service 需要腾讯云 SECRET_ID / SECRET_KEY / APP_ID

## 下一步
- 确认腾讯云凭证是否有，有则先跑 tts-stt-test/src/tts-test.mjs
- 没有腾讯云凭证则跳去跑 output-parser-test / runnable-test / tool-test

## 关键文件
- cron-job-tool/src/app.module.ts: MySQL 密码已改为 Xihe@2026
- 各子项目/.env: 硅基流动配置
- tts-stt-test/src/tts-test.mjs: 腾讯云 TTS 基础测试
- tts-stt-test/src/streaming-tts-test.mjs: 流式 TTS（最核心）
- tts-stt-test/src/asr-test.mjs: 语音识别测试
- asr-and-tts-nest-service/src/speech/tts-relay.service.ts: TTS relay 核心

## 验证记录
- 2026-03-20: curl 直接测试硅基流动 API：成功
- 2026-03-20: hello-nest-langchain /ai/chat?query=hi：返回正常回答

## 陷阱记录
- Kimi API Key 无效，浪费了一轮调试
- Node 24 + NestJS peer dep 冲突，必须 --legacy-peer-deps
- 后台 NestJS 进程不 kill 会 EADDRINUSE
- tts-stt-test 需要等 WebSocket ready=1 后才能发文字（有 pendingChunks 缓冲机制）

## 会话记录
| 日期 | 干了什么 |
|------|----------|
| 2026-03-20 | 环境搭建：安装依赖、配 .env、建库、跑通 hello-nest-langchain |
| 2026-03-29 | 拉取远程新代码（tts-stt-test + asr-and-tts-nest-service），学习 ASR/TTS 链路，配置 SessionStart hook 自动检查远程更新，跑通 output-parser-test / runnable-test / tool-test / memory-test / rag-test |

## 交接摘要
- 环境已就绪，硅基流动 API Key 可用
- 4/14 子项目已跑通，下一步跑腾讯云 TTS 或脚本类项目
- Docker/Milvus/邮件/腾讯云凭证是后续阻塞点
