# AGENTS.md — myBlog Prod

## 仓库角色
本仓库是博客正式前台发布源，不是常规功能开发区。对应开发仓库为 `myBlog-test`；内容维护台为 `myBlog-admin`。

## 最高优先级规则
- 常规功能开发必须先在 Test 完成。
- Prod 不独立重新实现 Test 已完成的功能。
- 只有 Test 已通过 ChatGPT Review、人工验收，并且 Test `docs/RELEASE.md` 明确为 `Release Ready` 时，才执行代码发布。
- 发布必须基于明确的 Source Test commit，不允许模糊地“同步最新 Test”。

## 本地 Git 约定
Prod Workspace 使用独立 Git 仓库：
- `origin` → `myBlog-prod`
- `test` → `myBlog-test`

发布前读取本仓库：
1. `AGENTS.md`
2. `docs/ACTIVE_TASK.md`
3. `docs/RELEASE.md`

然后检查工作区干净、同步 Prod main，并执行 `git fetch test`。

## 禁止操作
除非发布任务明确授权：
- 不执行 `git merge test/main`。
- 不用 Test 整库覆盖 Prod。
- 不自行扩大同步范围。
- 不在 Prod 重新开发功能。
- 不把 `测试库 / TEST`、`Chance（测试库）` 等 Test 环境文案带入正式站点。
- 不修改内容数据协议或 Admin 发布协议。

## 发布原则
每次发布必须明确：
- Source Test commit
- Prod 发布前 rollback SHA
- 允许修改文件
- 禁止修改文件/区域
- Prod 必须保留的正式环境差异
- 验证清单
- 发布分支
- commit message

如果同一个文件同时包含业务变化和环境差异，必须采用最小受控同步：保留 Prod 环境行为，只引入已经验收的 Test 功能。

## 代码发布与内容发布
本仓库存在两种变化来源：
- 前台代码发布：由 Test 的 Release Ready 驱动。
- 博客内容发布：由 `myBlog-admin` 在用户确认后写入/提升 `content.js` 与相关上传资源。

不要把内容更新误当成代码 Release，也不要为了发布内容覆盖前台代码。

## 完成报告
Codex 完成后用中文报告分支、commit SHA、修改文件、自测、push 状态，并在 Review 前停止，不自行合并 main。
