# AGENTS.md — myBlog Prod

## 仓库角色
本仓库是博客正式前台发布源，不是常规功能开发区。对应开发仓库为 `myBlog-test`；内容维护台为 `myBlog-admin`。

## 最高优先级规则
- 常规功能开发必须先在 Test 完成。
- Prod 不独立重新实现 Test 已完成的功能。
- 只有 Test 已通过 ChatGPT Review、人工验收，并且 Test `docs/RELEASE.md` 明确为 `Release Ready` 时，才执行代码发布。
- 发布必须基于明确的 Source Test commit，不允许模糊地“同步最新 Test”。

## Codex 启动任务前必须先同步 GitHub
GitHub 是任务、治理规则和发布状态的权威来源；本地 `AGENTS.md`、`docs/ACTIVE_TASK.md`、Plan 或 Release 文件可能已经过期。

因此每次用户要求“读取 AGENTS.md 和 docs/ACTIVE_TASK.md，执行当前任务”时，Codex 必须先：
1. 确认当前 Workspace / Git 仓库正确。
2. 执行 `git status`；若工作区不干净，不得直接 pull，先停止并报告。
3. 确认当前位于预期基线分支；常规任务启动基线为 `main`。
4. 执行 `git pull --ff-only origin main`，确保拿到 GitHub 最新治理文件与任务指针。
5. pull 成功后，**重新读取** `AGENTS.md`、`docs/ACTIVE_TASK.md`，以及 ACTIVE_TASK 指定的 Plan / `docs/RELEASE.md`。
6. 只执行重新读取后的最新任务；不得依据 pull 前缓存/旧版本的 ACTIVE_TASK 判断当前状态。

如果 `git pull --ff-only` 失败、存在本地未提交修改、当前分支不适合更新 main，或文件状态有歧义：停止并用中文报告，不自行 merge/rebase/reset/覆盖。

## 活跃任务分支期间的 main 变更规则
- 一旦 Codex 已从 main 创建并 push 当前任务分支，原则上不要再直接推进 main 的治理/任务文档，避免任务分支与 main 在 Review 前无谓分叉。
- 如果确有必要在任务进行中更新 main，必须把这件事视为显式的“主线前进事件”。
- 在 ChatGPT 最终 Review 前，Codex 必须先 `git fetch origin`，确认最新 `origin/main`。
- 如果任务分支落后 main，应先把最新 `origin/main` 合入当前任务分支；默认优先普通 merge，不改写已 push 历史，不 force push。
- 同步 main 后必须重新验证：无冲突、无额外业务文件变化、任务范围未扩大，然后 push 当前任务分支，再进行最终 Review。
- 未完成上述同步前，不把任务分支视为可合并状态。

## 本地 Git 约定
Prod Workspace 使用独立 Git 仓库：
- `origin` → `myBlog-prod`
- `test` → `myBlog-test`

发布前读取本仓库：
1. `AGENTS.md`
2. `docs/ACTIVE_TASK.md`
3. `docs/RELEASE.md`

同步最新 GitHub 状态后，再检查工作区干净并执行 `git fetch test`（发布任务需要时）。

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
