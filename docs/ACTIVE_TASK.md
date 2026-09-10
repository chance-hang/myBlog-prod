# ACTIVE TASK — myBlog Prod

## Status
Ready for Codex

## 当前任务
执行 Blog 内容协议 Phase A 的 Prod 受控兼容发布。

Plan：
`docs/plans/2026-09-10-content-protocol-phase-a-prod-compatibility.md`

Source Test commit：
`cba2f83406a7d0230786461218fe51d8b80b60c3`

## Codex 执行要求
1. 确认当前 Workspace / 仓库为 `myBlog-prod`，工作区干净并位于 `main`。
2. 执行 `git pull --ff-only origin main`；成功后重新读取最新 `AGENTS.md`、本文件、`docs/RELEASE.md` 和 Plan。
3. `git fetch test`，确认指定 Source Test commit 可读取。
4. 记录最新 Prod `origin/main` 精确 SHA 作为 rollback baseline。
5. 从最新 Prod main 创建 `codex/prod-content-protocol-phase-a-compatibility`。
6. 只受控引入 Source Test 已验收的读取/链接兼容能力；禁止 `git merge test/main` 和整库覆盖。
7. Prod `content.json` 必须从 Prod 当前 `content.js` 生成，禁止复制 Test 内容数据覆盖 Prod。
8. 同一逻辑内容复用 Test 冻结稳定 ID；Prod-only 内容分配新 ID；存在匹配歧义立即停止报告，不要猜。
9. 保留 Prod 正式 branding 和所有 Prod 环境差异；不得出现 `测试库 / TEST`。
10. 完成 Plan 全部验证后 commit + push 分支并停止，不合并 main。
11. 不使用 PAT，不执行 Admin 内容发布，不修改 Test。

## 完成报告
中文报告：
- 分支、Prod rollback baseline、Source Test SHA、最终 commit SHA
- 修改文件
- Test→Prod 代码适配差异
- Prod 三类内容数量及 stable ID 匹配/新增情况
- JSON 正常与 legacy 回退验证
- 旧链接与稳定 ID 链接验证
- Prod branding 检查
- `node --check` / 内容验证 / `git diff --check`
- push、工作区状态
- 明确未合并 main、未修改 Test/Admin、未使用 PAT

## 门禁
ChatGPT Review + 合并 Prod main + 用户正式站人工验收全部通过前，不得启动 Admin Phase B。
