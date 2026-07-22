# HANDOFF — 2026-07-22 (session 2)

## 本会话完成

- **claude-skills P1 拆分 — 自建部分完成。** 16个自建 skill 从内联目录抽成独立 GitHub 仓库 + submodule 引用
- 7个功能 skill：code-review, huashu-design, huashu-research, nuwa-skill, project-scheme, internal-comms, smart-search
- 9个 perspective-* skill：christopher-alexander, donella-meadows, hans-rosling, joseph-campbell, kevin-kelly, lks, nassim-taleb, norbert-wiener, panluan
- claude-skills README 已更新，移除"待抽独立仓库"分区
- github-mgmt 操作日志已更新

## 未完成

### P1 — 第三方 skill 物理迁移
75+ 个第三方 skill 目录仍在 claude-skills 仓库内。只有 README 做了归属标注，实际文件未迁走。
需要：逐批 fork → 移入 third-party-skills 仓库 → claude-skills 中删除目录。

### 修复项
- **caveman**：gitlink 存在但 .gitmodules 无映射。来源 KKKKhazix/khazix-skills。需 fork 后补 submodule 条目。
- **douban-anli-writer**：空仓库，源目录不存在。token 缺 delete_repo scope 无法删除。需用户手动删除或刷新 token。
- **video-catwave**：目录存在但 .gitmodules 无映射（可能是"猫波信号站"或独立项目）。

### P4 — 僵尸仓库确认
上次确认 loop-engine, source-rack, research-methods 均有用，不归档。

## 参考
- claude-skills 远程：https://github.com/wampeeHuang/claude-skills（commit 672eae9）
- third-party-skills 远程：https://github.com/wampeeHuang/third-party-skills
- github-mgmt 操作日志：references/cleanup-plan-template.md §操作记录
