# Blog 内容协议 Phase A — Prod 兼容发布

状态：Ready for Codex
日期：2026-09-10

## 来源
Test 已完成并人工验收：
- Source Test commit：`cba2f83406a7d0230786461218fe51d8b80b60c3`
- Test `docs/RELEASE.md` 已标记 Release Ready。

本任务是受控前台代码发布，不是 Admin 内容发布。

## 目标
让 Prod 获得已在 Test 验收通过的内容读取兼容能力：严格 `content.json`、稳定 ID、旧数组下标链接兼容、legacy `content.js` 安全回退，同时完整保留 Prod 当前正式内容与正式环境差异。

## 开始前必须记录
1. 最新 `origin/main` 精确 SHA，作为 rollback baseline。
2. `git fetch test` 后确认 Source Test commit `cba2f834...` 可读取。
3. 对比 Prod 与 Source Test 中本阶段相关文件，先识别环境差异再改。

## 发布原则
- 不允许 `git merge test/main`。
- 不允许整份 Test 文件无脑覆盖 Prod。
- 代码逻辑应来自已验收 Test commit，只做保持 Prod 环境边界所需的最小适配。
- **不得复制 Test `content.json` 作为 Prod 数据。** Prod `content.json` 必须从 Prod 当前 `content.js` 内容基线生成。
- 不改变 Prod 当前内容、顺序、文案、正式环境 branding。

## 稳定 ID
- 对 Prod 与 Test 可明确判定为同一逻辑条目的内容，复用 Test 已冻结 ID。
- Prod-only 条目分配新的不可变 `<kind>_<ULID>`。
- 不得仅凭数组位置认定是同一条目；应基于类型及足够稳定的内容字段/文本进行确定性匹配。
- 任一匹配存在歧义时停止并报告，等待 ChatGPT 决策。

## 允许范围
按实际差异最小修改：
- `app.js`
- `reader.js`
- `content.json`（新增，数据来自 Prod 当前内容）
- 前台 HTML 中仅与脚本 cache-bust/加载兼容直接相关的引用
- `scripts/verify-content.mjs` 或 Prod 等价验证脚本

## 禁止范围
- 不修改 Prod `content.js` 的内容数据
- 不引入 Test-only branding
- 不修改 Admin / Test
- 不执行 PAT/GitHub 内容发布
- 不做视觉改版、结构重构、无关清理
- 不改 `assets/uploads/` 内容

## 必须验证
1. Prod `content.json` 与 Prod legacy `content.js` 的三类内容数量、顺序、正文关键字段一致。
2. JSON 正常路径正确。
3. JSON 404/校验失败后 legacy 回退正确。
4. `article-0`、`topic-0`、`note-0` 指向原 Prod 条目。
5. 至少一个稳定 ID 链接正确；JSON 与 legacy 回退两条路径都验证。
6. 所有正式页面无 `测试库 / TEST` 等 Test-only branding 泄漏。
7. 修改 JS `node --check`、内容验证脚本、`git diff --check` 通过。
8. 明确列出 Test→Prod 代码适配差异和 Prod 内容 ID 匹配结果。

## 分支
`codex/prod-content-protocol-phase-a-compatibility`

完成后 commit + push 分支并停止，等待 ChatGPT Review。不要合并 main。正式站人工验收前不得开始 Admin Phase B。
