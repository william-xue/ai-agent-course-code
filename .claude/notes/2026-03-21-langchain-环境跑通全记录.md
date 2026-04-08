# LangChain 课程环境跑通全记录
> 2026-03-21 18:35 | langchain

## 代码真相
- 9 个子项目全部跑通，用硅基流动免费模型（Qwen/Qwen2.5-7B-Instruct + BAAI/bge-m3）
- 脚本类项目的 npm test 是空的，实际入口是各 src/*.mjs 文件，用 node 直接跑
- Milvus standalone 需要 Docker，ARM Mac 必须用 v2.5.4+（v2.4.4 是 amd64 会 crash）
- Milvus 启动需加 `-e ETCD_USE_EMBED=true -e COMMON_STORAGETYPE=local`，否则退出码 134

## 设计决策
- API 选硅基流动而非 Kimi：Kimi key 验证失败，硅基流动注册送额度且有免费模型
- Embedding 用 BAAI/bge-m3（1024 维），硅基流动免费，兼容 OpenAI embeddings 接口
- Milvus 用 Docker standalone 而非 Milvus Lite：项目代码写死了 localhost:19530，改动最小

## 陷阱记录
- Kimi API Key (ak-f77smt44nfu111d6cgx1) 无法通过验证，直接废弃
- Node 24 下 npm install 必须加 `--legacy-peer-deps`，否则 peer dep 冲突
- milvus-test 缺 `@langchain/core` 依赖，需手动补装
- Milvus v2.4.4 镜像是 linux/amd64，ARM Mac 上 exit 134 直接挂
- NestJS 后台进程不 kill 占端口 3000，下次跑前先 `lsof -i:3000`
- output-parser-test 的 normal.mjs 中 JSON.parse 会报错——这是故意的，演示为什么需要 OutputParser

## 私人笔记
- 硅基流动 key: sk-atwyfbjemfufpzqdqslmyntpffizowsohvjnazobpzcrprts
- MySQL 密码: Xihe@2026，数据库: hello
- cron-job-tool 的邮件和搜索 API 没配，暂时不影响核心功能
- Docker 命令备忘：`docker run -d --name milvus-standalone -e ETCD_USE_EMBED=true -e COMMON_STORAGETYPE=local -v /tmp/milvus/data:/var/lib/milvus -p 19530:19530 milvusdb/milvus:v2.5.4 milvus run standalone`
