# ACTIVE TASK — myBlog Prod

## Status
Idle

本机 Prod Workspace 的 Test remote 配置已经完成：
- `origin` → `https://github.com/hb27bp49vk-source/myBlog-prod.git`
- `test` → `https://github.com/hb27bp49vk-source/myBlog-test.git`
- `git fetch test` 已完成

当前没有需要 Codex 执行的 Prod 任务。

## 当前治理状态
博客三仓库本地 Workspace 与 GitHub 治理骨架已经建立：
- `myBlog-test`：前台开发与验收
- `myBlog-prod`：正式代码发布
- `myBlog-admin`：独立内容维护与发布

后续 Prod 只有在 Test `docs/RELEASE.md` 明确为 `Release Ready` 且本文件被写入具体发布任务时，才允许创建 release branch 或引入 Test 变化。

## Codex 指令
如果没有新的任务写入本文件：
- 不创建发布分支
- 不 merge/cherry-pick Test
- 不修改业务代码
- 不自行同步 test/main
- 不执行代码发布

下一次正式代码发布时，ChatGPT 会在这里明确 Source Test commit、allowlist、Prod 保护项、发布分支和验证清单。
