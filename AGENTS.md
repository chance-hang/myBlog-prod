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

## Workflow 3.0 跨项目同步（2026-09-10）

本节把总控仓库 [`chance-hang/AI-Coding-Control-Center`](https://github.com/chance-hang/AI-Coding-Control-Center) 的跨项目规则同步到本仓库。它**不替代**本仓库既有 Prod 规则；冲突时本节服从上文"最高优先级规则"、"禁止操作"、"发布原则"、"代码发布与内容发布"，最终由 ChatGPT 按 `GLOBAL_RULES.md §19` 恢复权威顺序裁决。

权威来源：

- [GLOBAL_RULES.md](https://github.com/chance-hang/AI-Coding-Control-Center/blob/main/GLOBAL_RULES.md)
- [docs/WORKFLOW.md](https://github.com/chance-hang/AI-Coding-Control-Center/blob/main/docs/WORKFLOW.md)
- [docs/EXECUTOR_HANDOFF.md](https://github.com/chance-hang/AI-Coding-Control-Center/blob/main/docs/EXECUTOR_HANDOFF.md)
- [docs/MULTI_DEVICE.md](https://github.com/chance-hang/AI-Coding-Control-Center/blob/main/docs/MULTI_DEVICE.md)
- [docs/DISASTER_RECOVERY.md](https://github.com/chance-hang/AI-Coding-Control-Center/blob/main/docs/DISASTER_RECOVERY.md)

### 角色与执行器抽象

- 用户：提出需求、批准发布、执行关键人工验收。
- ChatGPT：总控。读取 GitHub 事实、维护治理文件、Review Executor 推送结果、批准 Prod 发布。
- Executor：在真实本地仓库中执行明确任务的工程层。**Codex 与 Workbuddy 都是可替换 Executor**；Prod 发布执行不绑定单一 Executor。
- GitHub：对 Executor 中立的长期共享状态中心。
- myBlog 三仓（Test / Prod / Admin）是三套独立 Workspace，不视为单一项目仓库。

### ACTIVE_TASK / Task Queue 优先级模型

本仓库 `docs/ACTIVE_TASK.md` 必须为每条任务标注优先级：

| 优先级 | 含义 | Executor 允许行为 |
| --- | --- | --- |
| `P0 Active` | 当前唯一允许执行的任务 | Implementation、commit、push |
| `P1 Queued` | 下一任务 | 读取、规划、写文档；**不得提前 Implementation** |
| `P2 Backlog` | 未来任务 | 不主动执行 |

Executor 不得自行把 P1 / P2 提升为 P0。

### STOP 状态机

| 状态 | 后续推进必须由谁激活 |
| --- | --- |
| `Awaiting ChatGPT Review` | ChatGPT |
| `Awaiting User Acceptance` | 用户 |
| `Blocked` | ChatGPT + 用户 |
| `Completed / Accepted` | ChatGPT 派发下一 Task 或执行发布 |

Prod 发布期间 Executor 完成 commit / push 后必须停在 `Awaiting ChatGPT Review`，不得自行推进到 Prod `main` 合并。Test 整库覆盖 Prod、force push、合并 `test/main` 等高风险操作必须经 ChatGPT 显式授权 + 用户人工验收。

### 上下文高效指令与汇报

- ChatGPT → Executor 默认指令只给三件事：仓库绝对路径、动作（`clean-handoff-and-execute` / `takeover-only` 等）、读取入口（`AGENTS.md` / `docs/ACTIVE_TASK.md` / `docs/RELEASE.md` / `docs/RELEASE_HISTORY.md`）。
- Executor 默认完成汇报只四件事：`commit SHA` / 测试验证 / `push 成功/失败` / `blocker`。
- 仅当新需求尚未进入 GitHub、高风险操作、异常恢复、需要用户决策时才展开长指令。

### 多电脑 / Executor 接管 / 云同步盘

- 每台电脑使用独立 Git clone；GitHub 负责跨设备同步。
- 同一个仓库同一时间只能有一个写入 Executor。
- dirty worktree 接管必须先保护前一执行器遗留工作；禁止直接 `reset --hard` / `clean -fd` / `checkout --`。
- 百度同步盘**不能视为 Git 状态同步机制**；不得让两个 Executor / 设备同时写同一 clone。
- 遇到 ref 异常先停止、检查 `git reflog` 与 `git fsck`，**不得** reset / clean / 重写历史。详细恢复流程见 [docs/DISASTER_RECOVERY.md §场景 E](https://github.com/chance-hang/AI-Coding-Control-Center/blob/main/docs/DISASTER_RECOVERY.md)。

### 与本仓库既有 Prod 规则的关系

- 上文"最高优先级规则"、"禁止操作"、"发布原则"、"代码发布与内容发布"等仍然有效且优先。
- 本节只在不冲突的范围内补充跨项目同步要求；冲突时按权威顺序由 ChatGPT 显式裁决。
