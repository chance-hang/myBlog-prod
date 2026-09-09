# ACTIVE TASK — myBlog Prod

## Status
Ready for Codex

## 当前任务
Stage C：建立 Prod 正式环境文案边界，清理此前直接同步 Test 页面造成的 Test branding 泄漏。

完整计划：
`docs/plans/2026-09-09-production-branding-boundary.md`

## 任务性质
这是 **Prod 独有环境差异修复**，不是新的业务功能发布。

Test 必须继续保留测试环境标识，因此本任务直接在 Prod 做最小修复。未来真正的业务功能仍必须走 Test → Review → 人工验收 → Release Ready → Prod 受控发布。

## Codex 执行要求
1. 先确认当前 Workspace 是 `myBlog-prod`。
2. `git status` 必须干净；先 `git pull --ff-only origin main` 拉取本任务和计划。
3. 读取 `AGENTS.md`、本文件、`docs/RELEASE.md`、上述 Plan。
4. 从最新 Prod main 创建：`codex/prod-branding-boundary`。
5. 只做 Plan 允许的正式环境文案最小修复。
6. 对正式前台 HTML 全文搜索 Test-only branding，按 Plan 处理。
7. 不修改 Test，不 merge/cherry-pick `test/main`。
8. 不修改 JS、CSS、`content.js`、assets、Admin 或数据协议。
9. 完成验证后提交：`fix: 清理正式博客测试环境文案`
10. push 分支后停止，等待 ChatGPT Review；不要合并 main。

## 当前 Prod rollback 基线
任务开始时 GitHub Prod main：
`70458179d3ad059e76f35c28dbd3f50b6300d07f`

Codex pull 后如果 main 因本任务文档提交前进，这是正常的；业务修改仍必须从 pull 后的最新 main 建分支，并在报告中给出实际 base SHA。

## 完成报告
中文报告：
- 分支
- 实际 base SHA
- commit SHA
- 修改文件
- 清理了哪些 Test-only branding
- 全文搜索结果
- 验证结果
- push 状态
- 明确说明未合并 main
