# 整理方案模板

## 方案输出格式

每次审计后生成如下结构的方案：

```
## GitHub 整理方案 — YYYY-MM-DD

### 审计摘要
- 仓库总数: N
- 公开: N / 私有: N
- 分类: 原创项目 N, 原创 Skill N, 配置基建 N
- 风险: P0 N, P1 N, P2 N, P3 N, P4 N

### 本次操作

| 优先级 | 仓库 | 当前状态 | 目标状态 | 操作 |
|--------|------|----------|----------|------|
| P0 | xxx | public, no README | private, with README | `gh repo edit --visibility private` |

### 操作命令

逐一列出准确的 gh 命令和 API 调用。
```

## 常见操作命令

### 转私有
```bash
gh repo edit wampeeHuang/<repo> --visibility private --accept-visibility-change-consequences
```

### 设置 description
```bash
gh repo edit wampeeHuang/<repo> --description "一句话描述"
```

### 添加 topics
```bash
gh repo edit wampeeHuang/<repo> --add-topic "skill" --add-topic "claude-code"
```

### 设置 homepage
```bash
gh repo edit wampeeHuang/<repo> --homepage "https://..."
```

### 归档
```bash
gh repo archive wampeeHuang/<repo> -y
```

### 检查仓库是否可安全公开
```bash
# 检查是否有 .env / 密钥
gh api repos/wampeeHuang/<repo>/contents --jq '.[].name' | grep -iE '\.env|secret|key|credential'
# 检查 README 是否存在
gh api repos/wampeeHuang/<repo>/readme --jq . 2>/dev/null || echo "NO_README"
```

### Fork 第三方仓库（保留归属）
```bash
gh repo fork <upstream-owner>/<upstream-repo> --clone=false
# 然后在 claude-skills 中用 submodule 引用
git submodule add https://github.com/wampeeHuang/<repo>.git <path>
```

## 操作记录

每次执行后在此记录：

| 日期 | 仓库 | 操作 | 结果 |
|------|------|------|------|
| 2026-07-22 | thinkprism-miniapp | → private | ✓ |
| 2026-07-22 | picture-remove-mark-google-ai | → private | ✓ |
| 2026-07-22 | thinkprism-miniapp | 补充 README | ✓ |
| 2026-07-22 | picture-remove-mark-google-ai | 重写 README 为中文 | ✓ |
| 2026-07-22 | 6个缺 description 仓库 | 补充 About | ✓ |
| 2026-07-22 | third-party-skills | 新建仓库 + 归属 README | ✓ |
| 2026-07-22 | claude-skills README | 更新结构说明 + 安装命令 | ✓ |
| 2026-07-22 | github-mgmt | 独立建仓 + submodule 入 claude-skills | ✓ |
| 2026-07-22 | code-review | 独立建仓 + submodule | ✓ |
| 2026-07-22 | huashu-design | 独立建仓 + submodule（174文件） | ✓ |
| 2026-07-22 | huashu-research | 独立建仓 + submodule | ✓ |
| 2026-07-22 | nuwa-skill | 独立建仓 + submodule（139文件） | ✓ |
| 2026-07-22 | project-scheme | 独立建仓 + submodule | ✓ |
| 2026-07-22 | internal-comms | 独立建仓 + submodule | ✓ |
| 2026-07-22 | smart-search | 独立建仓 + submodule | ✓ |
| 2026-07-22 | 9 perspective-* | 独立建仓 + submodule | ✓ |
| 2026-07-22 | douban-anli-writer | 空仓库（源目录不存在），无法删除（token缺delete_repo） | ⚠️ |
| 2026-07-22 | caveman | 第三方skill，gitlink存在但无.gitmodules映射 | ⚠️ 待修复 |
| 2026-07-22 | video-catwave | 独立建仓(catwave-station) + submodule 入 claude-skills | ✓ |
| 2026-07-22 | douban-anli-writer | 补充内容(SKILL.md + style-guide.md) + submodule | ✓ |
| 2026-07-22 | github-mgmt | CHECKPOINT.md 删除 + commit push | ✓ |
| 2026-07-22 | local-vision | llama-server 路径修复 commit push | ✓ |
| 2026-07-22 | perspective-router | BJ Fogg + LKS + Panluan perspective commit push | ✓ |
| 2026-07-22 | vercel-deploy | DNS→DNSPod 修复 commit push | ✓ |
| 2026-07-22 | claude-skills | parent repo commit push (5ce20b1)，README 合并列表 | ✓ |
| 2026-07-22 | 79 个第三方 skill | 批量迁移至 third-party-skills 仓库（commit 4396a34） | ✓ |
| 2026-07-22 | claude-skills | git rm -r 删除79个第三方目录（-523,146行），commit 3d0b731 | ✓ |
| 2026-07-22 | claude-skills README | 重写为纯自建 submodule 合集，第三方指向 third-party-skills | ✓ |
| 2026-07-22 | claude-skills | 清理残留目录 anysearch/ clone-study/ + CHECKPOINT.md | ✓ |
| 2026-07-22 | third-party-skills | 新建 SOURCES.md，79个skill逐一标注原始仓库来源 | ✓ |
