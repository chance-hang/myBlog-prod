# RELEASE — myBlog Prod

## Status
Idle

当前没有前台代码发布任务。

## 仓库关系
- Prod：`myBlog-prod`
- Test：`myBlog-test`
- Admin：`myBlog-admin`

本机 Prod Workspace 约定：
- `origin` → Prod
- `test` → Test

## 前台代码发布门槛
只有 Test `docs/RELEASE.md` 为 `Release Ready` 才能开始。

每次发布前必须得到并记录：
- Source Test commit
- Prod rollback SHA
- release scope / allowlist
- forbidden files / protected areas
- Prod 环境差异
- 验证清单
- release branch
- commit message

## 标准步骤
1. 读取 AGENTS / ACTIVE_TASK / RELEASE。
2. 确认工作区干净。
3. `git fetch origin` 与 `git fetch test`。
4. 确认 Prod main 与指定 Source Test commit。
5. 从 Prod main 创建本次 release branch。
6. 仅按 allowlist 引入批准变化。
7. 保留 Prod 正式环境差异。
8. 本地验证。
9. commit + push release branch。
10. 停止，等待 ChatGPT Review。
11. Review 通过后才合并 Prod main。
12. 用户进行正式站人工验收。
13. 验收通过后追加 `docs/RELEASE_HISTORY.md`，ACTIVE_TASK 回到 Idle。

## 永久禁止的默认做法
- `git merge test/main`
- Test 整库覆盖 Prod
- 未指定 Source commit 就同步
- 把 Test 环境标识发布到 Prod
- 在 Prod 独立重新实现功能

## 当前已知 Prod 保护项
目前 Prod 首页仍存在测试环境文案泄漏，这是已知待修问题，而不是应继续保留的正式行为。后续发布治理应确保正式站使用正式环境文案。

## 内容发布边界
`myBlog-admin` 对 `content.js` 与 `assets/uploads/` 的内容发布/提升，不等于前台代码 Release。内容发布应遵循 Admin 自己的治理与人工确认规则。
