# ACTIVE TASK — myBlog Prod

## Status
Waiting for Local Remote Setup

## 当前任务
只配置本机 Prod Workspace 的 Test 远程仓库，不修改任何业务文件。

目标：
- `origin` → `https://github.com/hb27bp49vk-source/myBlog-prod.git`
- `test` → `https://github.com/hb27bp49vk-source/myBlog-test.git`

## Codex 执行要求
1. 读取 `AGENTS.md` 和本文件。
2. 检查当前目录确实是 `myBlog-prod` 独立 Git 仓库。
3. 检查 `git status`，不得借本任务修改业务代码。
4. 检查 `origin` URL；如果不正确，只修正 remote URL。
5. 如果不存在 `test` remote：添加上述 Test URL。
6. 如果已存在但 URL 不正确：修正为上述 Test URL。
7. 执行 `git fetch test`。
8. 输出 `git remote -v` 与 fetch 结果。

## 禁止
- 不修改任何仓库文件。
- 不创建业务提交。
- 不 merge/cherry-pick Test。
- 不切换或覆盖 Prod main。
- 不执行代码发布。

## 完成标准
只要本机 remote 配置正确且 `git fetch test` 成功，本任务即完成。由于 remote 配置属于本机 `.git/config`，不会产生 GitHub commit。
