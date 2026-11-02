---
title: Github 代码贡献流程
pubDate: '2026-08-27'
description: ''
image: ''
draft: false
slugId: Github 代码贡献流程
category: 日常
pinTop: 0
---

# GitHub 代码贡献流程手册

> 基于实际贡献 `Simple-Homepage` 项目总结的经验。本文章由AI编写。

## 核心概念

- **fork**：在 GitHub 上把别人的仓库复制一份到你名下，你才有推送（写）权限。
- **origin**：你 fork 的仓库，**可以 push**。
- **upstream**：原作者仓库，**只能拉，不能推**。
- **分支**：在 main 之外开一条工作线，隔离自己的改动，避免污染稳定的 main。

## 完整流程一句话

> fork → 配置 origin/upstream → 建功能分支 → 改代码 → add/commit/push → 网页开 PR

---

## ① 配置远端

```bash
git remote -v          # 查看当前有哪些远端
# fork 之后，把 origin 指向自己的 fork（本地重新 clone 的 fork 已是你的，可跳过）
git remote set-url origin https://github.com/<你的用户名>/<仓库名>.git
# 添加 upstream 指向原作者仓库
git remote add upstream https://github.com/<原作者>/<仓库名>.git
git fetch upstream     # 拉取原作者最新代码（只拉不合并）
```

**为什么**：origin 是你要**推送**的地方；upstream 用来**跟进原作者的最新改动**，保证分支基于最新代码。

## ② 建分支 + 改代码

```bash
git checkout -b <功能分支名>   # 新建并切换功能分支，如 feat/system-theme
git status                    # 查看哪些文件被改
git diff                      # 查看每一处改动的具体内容
```

**为什么**：`-b` 一次性完成“新建+切换”。功能分支让改动独立成线，好管理、好回退。

## ③ 提交

```bash
git add <file1> <file2> ...   # 把改动加入暂存区
git commit -m "feat: 说明本次改动内容"
```

**为什么**：`add` 挑选**这次提交哪些文件**（避免把 node_modules 等误提交）；`commit` 给这次改动**打一个带说明的快照**（`-m` 写提交信息）。

> 提交信息建议：`feat:` 新功能 / `fix:` 修复 / `docs:` 文档 / `refactor:` 重构。

## ④ 推送

```bash
git push -u origin <功能分支名>
```

**为什么**：推到**你 fork 的远端（origin）**；`-u` 记录追踪关系，之后直接 `git push` 即可。

## ⑤ 开 PR

在 GitHub 网页上点 **Compare & pull request**，把功能分支请求合并到 `upstream:main`。

---

## 本地验证（提交前必备，非 git 命令）

```bash
npm install       # 装依赖
npm run build     # 构建，确认代码能正常编译
```

构建通过后，再走 add/commit/push，避免提交带 bug 的代码。

## 常见情况处理

### 跟进原作者更新
```bash
git fetch upstream
git rebase upstream/main   # 把功能分支基于最新 main 重新排一次
```

### 误把东西提交或推错
```bash
git status                 # 先看状态
git restore <file>         # 撤销工作区的改动
git reset HEAD <file>      # 从暂存区撤出（保留改动）
```

### Fork PR 的 Vercel / 部署检查失败
若失败原因是 **Authorization required / 权限不足**，通常是原作者项目的部署 Team 未授权你的 fork，**与代码无关**，等原作者处理即可，不影响合并。

---

## 常见指令速查

| 指令 | 作用 |
| --- | --- |
| `git remote -v` | 查看远端 |
| `git checkout -b name` | 新建并切换分支 |
| `git add file` | 暂存文件 |
| `git commit -m "msg"` | 提交快照 |
| `git push -u origin br` | 推送并建立追踪 |
| `git fetch` | 拉取远端到本地（不合并） |
| `git rebase upstream/main` | 变基到最新 main |
| `git status` | 查看状态 |
| `git diff` | 查看改动 |
