# RELEASE HISTORY — myBlog Prod

本文件只记录已经完成并通过正式站人工验收的前台代码发布/架构迁移。

## 2026-09-10 — 内容协议 Phase A Prod 兼容发布
- 类型：架构迁移 / 兼容发布
- Source Test commit：`cba2f83406a7d0230786461218fe51d8b80b60c3`
- Prod release commit：`eff24641c04a8c58fc18575f37b80d3cecf56661`
- 发布范围：受控引入严格 `content.json` 读取、稳定 ID、旧数组下标链接兼容、legacy `content.js` 安全回退及对应静态资源版本更新/验证；Prod `content.json` 基于 Prod 自身内容生成
- Prod rollback SHA：`fa6823441c47fd1ed90b25b986aace2152f364f9`
- ChatGPT Review：通过
- 正式站人工验收：2026-09-10 用户确认通过
- 特殊说明：保留 Prod 正式 branding 与内容边界；本次为前台代码协议兼容发布，不是 Admin 内容发布；Admin Phase B 在本记录完成后方可另行启动

## 2026-09-09 — Prod 正式环境文案边界基线
- 类型：修复 / 正式环境边界建立
- Source Test commit：不适用；本次为 Prod 独有环境差异修复，Test 必须继续保留测试环境标识
- Prod release commit：`3d2b03901bbe0bf182e8920b3dc83b53e1ca316d`
- 发布范围：仅 6 个正式前台 HTML 的 Test-only branding / 页面标题文案
- Prod rollback SHA：`f35c7bab7a1c6536d3655db1b999ddf881e52458`
- ChatGPT Review：通过；确认无额外业务文件变化，JS、CSS、`content.js`、assets、Admin 未进入本次业务修改
- 正式站人工验收：2026-09-09 用户确认通过
- 特殊说明：自此 `测试库 / TEST`、`Chance（测试库）` 等 Test-only branding 属于 Prod 禁止泄漏项；未来 Test → Prod 代码发布必须保留这一正式环境边界

后续每条记录至少包含：
- 日期
- 类型（功能发布 / 修复 / 架构迁移）
- Source Test commit
- Prod release commit
- 发布范围
- Prod rollback SHA
- ChatGPT Review 结果
- 正式站人工验收结果
- 特殊说明

注意：通过 `myBlog-admin` 发布的普通文章、短记、专题和图片属于内容发布，不在这里逐篇记录。
