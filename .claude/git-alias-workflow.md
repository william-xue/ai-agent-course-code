# Git 双远程工作流（本仓库）

本文档记录两个常用命令：`git sync-main` 和 `git new-study`。

## 前提

- `origin` 指向你的 Fork 仓库
- `upstream` 指向原始上游仓库
- `main` 跟踪 `upstream/main`

## 1) git sync-main

用途：把本地 `main` 同步到上游最新，并推送到你的 Fork。

执行逻辑：

1. `git switch main`
2. `git fetch upstream`
3. `git merge --ff-only upstream/main`
4. `git push origin main`

适用场景：

- 开始新任务前先同步主干
- 每天开工前更新一次主干

## 2) git new-study <name>

用途：基于最新主干创建学习/实验分支。

示例：

```bash
git new-study asr-experiment
```

执行逻辑：

1. 同步 `main`（与 `git sync-main` 相同）
2. 创建并切换到 `study/<name>` 分支

执行后分支示例：

- `study/asr-experiment`

## 推荐日常流程

```bash
git sync-main
git new-study feature-a
# 开发与提交...
git push -u origin study/feature-a
```

## 备注

- `--ff-only` 可以避免在 `main` 产生额外 merge commit，保证主干线性和干净。
- 若 `git new-study <name>` 报分支已存在，请改一个新名字，或先切到已有分支继续开发。
