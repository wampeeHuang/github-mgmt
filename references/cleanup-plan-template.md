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
