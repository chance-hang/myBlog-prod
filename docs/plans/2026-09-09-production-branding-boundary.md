# Stage C — 正式环境文案边界修复

状态：Ready for Codex
日期：2026-09-09

## 背景

当前 `myBlog-test` 与 `myBlog-prod` 的核心前台文件高度一致。历史上 Prod 曾在 commit `32e8a5a2cb9d3c3a1f130494db44a4101cddb6e7` 清理过 Test branding，但随后又通过 `sync ... from test` 类提交把 Test 页面直接同步回 Prod，导致测试文案重新泄漏。

这次任务用于建立博客治理机制后的第一个正式 Prod 环境边界基线。

## 为什么本任务直接在 Prod 修复

这不是新的业务功能，也不是把 Test 功能发布到 Prod；它是在恢复/建立 **Prod 独有环境差异**。

Test 必须继续保留测试标识用于验收，因此不能先把 Test 的测试标识删除再发布。完成本任务后，未来真正的业务功能仍严格遵循：Test 开发 → Review → 人工验收 → Release Ready → Prod 受控发布。

## 当前已确认的泄漏

Prod 当前至少存在：
- `index.html`：`<title>Chance（测试库）</title>`
- `index.html`：页面可见 `测试库 / TEST`
- `about.html`：title 含 `（测试库）`
- `articles.html`：title 含 `（测试库）`
- `notes.html`：title 含 `（测试库）`
- `topics.html`：title 含 `（测试库）`
- `content.html`：title 含 `（测试库）`

Codex 还必须对正式前台 HTML 做一次全文搜索，若发现同类 Test-only branding，一并在本任务范围内清理并报告。

## 目标

1. Prod 正式前台不再出现 `测试库`、`TEST ENVIRONMENT`、测试环境 badge/label 等 Test-only branding。
2. 不改变页面结构、样式、脚本、内容数据和交互逻辑。
3. Test 仓库保持原样，继续明确显示测试环境。
4. 把“Prod 环境文案”建立为未来 Test → Prod 发布时必须保护的差异。

## 允许修改

只允许修改 Prod 前台 HTML 中与环境标识直接相关的文字/标签：
- `index.html`
- `content.html`
- `about.html`
- `articles.html`
- `notes.html`
- `topics.html`
- 如全文搜索确认其他前台 `.html` 存在同类 Test-only branding，可做最小清理并在报告中列出。

## 禁止修改

- `app.js`
- `reader.js`
- `style.css`
- `content.js`
- `assets/**`
- 任何 Admin 代码
- 数据协议
- 页面布局/DOM 结构（删除纯 Test 环境 label 元素除外）
- GitHub Pages / 部署设置
- Test 仓库任何文件

## 正式文案基线

建议最小替换：
- 首页 `<title>`：`Chance`
- 首页 `测试库 / TEST` 环境 label：删除该 Test-only label；保留同一 header 中 `CHANCE`
- 其他页面 title：仅删除 `（测试库）` 后缀，保留原页面名称

不要为了“优化文案”重写其他内容。

## 分支与提交

- Branch：`codex/prod-branding-boundary`
- Commit：`fix: 清理正式博客测试环境文案`

## 验证

1. 对 Prod 前台 HTML 搜索：`测试库`、`TEST ENVIRONMENT`、`这是测试库`。
2. 上述 Test-only branding 应无残留；如果某处确有合理非环境用途，停止并在报告中说明，不自行猜测删除。
3. 首页可正常加载，loader、关于抽屉、内容入口正常。
4. `content.html`、项目、AI、生活、关于页面可打开。
5. 文章详情仍可读取。
6. `content.js`、JS、CSS SHA/工作区内容不因本任务改变。

完成后 commit + push 分支，停止等待 ChatGPT Review，不合并 main。
