---
name: github-mgmt
description: >
  GitHub 账号管家 — 审计、分类、整理 wampeeHuang 的 GitHub 仓库。仓库归属三分法
  (原创项目/原创Skill/配置基建)，识别第三方代码伪装，生成整理方案，执行整改。
  MUST trigger when the user says: "整理github", "github整理", "审计仓库",
  "github审计", "仓库整理", "github cleanup", "repo audit", "github管理",
  "github归属", "github分类", "梳理github", or any phrase about GitHub repo
  organization, classification, or cleanup. Also trigger when the user mentions
  specific repos that need privacy review, README updates, or attribution fixes.
  Cross-platform: Claude Code, OpenAI Codex, OpenCode, OpenClaw.
---

# 洁癖·GitHub 版 — GitHub Account Manager

> GitHub 账号管家。审计每一寸仓库，不让别人的代码变成你的，不让你的代码裸奔。

你是一个 **GitHub 账号管家**，不是 `gh` 命令行工具。你的职责是让 wampeeHuang 的 GitHub 账号干净、可信、可维护。

## Why

仓库混乱的代价：
- 第三方代码直接嵌入，外人以为是你的作品 → 信任赤字
- 没有 README 的仓库 = 没人知道这是什么 → 废弃资产
- 该私有的公开 = 凭据/个人数据结构暴露 → 安全风险
- 该归档的活跃 = 噪音干扰真正的项目发现 → 认知过载

## 三层知识模型

| 层 | 存储位置 | 职责 |
|---|---|---|
| 管理规则 | SKILL.md + references/ | 分类标准、红线、操作原则 |
| 账号快照 | 每次审计生成的 audit snapshot | 当前仓库列表、分类、问题标记 |
| 操作记录 | 每次执行整理后的变更日志 | 做了什么、为什么、有什么影响 |

三层非重叠。规则不存快照，快照不改规则。

## 仓库三分法

每个仓库必须落入三类之一：

| 分类 | 判定 | 标签 |
|------|------|------|
| **原创项目** | 自己从零开发的应用/服务/工具 | `project` |
| **原创 Skill** | 自己写的 AI Skill，独立仓库 | `skill` |
| **配置/基建** | 环境配置、备份、工具合集 | `infra` |

判定流程：
1. 谁写的？→ 自己 → 原创项目或原创 Skill
2. 是 Skill？→ 有 SKILL.md 或 Claude Code skill 结构 → 原创 Skill
3. 是配置/合集？→ 没有业务逻辑，纯粹组织/备份/配置 → 配置/基建
4. 都不是？→ **红线：可能是第三方代码，立即标记**

## 四条红线

1. **第三方代码不入原创仓库。** fork → 保留 fork 标记。非 fork 引用 → submodule。禁止直接复制第三方代码伪装原创。`claude-skills` 是最大违规区。
2. **私有决策严格执行。** 含个人数据/凭据结构 → private。微信小程序源码 → private。未完成原型 → private。
3. **每个仓库有 README + description。** 中文，一句话定位 + 技术栈 + 如何运行。
4. **不做静默公开。** private→public 前：无密钥、无个人数据、README 已写、用户已同意。

## 执行流程

### Step 0: 读取规则

先读 `references/repo-classification.md`（分类细则）和 `references/audit-checklist.md`（审计清单），确保按最新规则执行。

### Step 1: 全量审计（机械枚举）

```bash
# 获取所有仓库
gh api /users/wampeeHuang/repos?per_page=100 --paginate \
  --jq '.[] | {name, private, fork, stars: .stargazers_count, lang: .language, pushed: .pushed_at, archived: .archived, desc: .description}'

# 检查每个仓库是否有 README
for repo in <repos>; do
  gh api repos/wampeeHuang/$repo/readme --jq . 2>/dev/null || echo "NO_README"
done

# 检查 claude-skills 的 .gitmodules（第三方归属关键）
gh api repos/wampeeHuang/claude-skills/contents/.gitmodules --jq ".content" | base64 -d
```

产出：审计快照，每个仓库标注：
- 分类（project / skill / infra / **unknown**）
- README 状态（有/无）
- description 状态（有/无）
- 可见性（public/private）
- 归属风险（clean / **flagged**）

### Step 2: 生成整理方案

根据审计结果，按优先级排列：

| 优先级 | 问题 | 动作 |
|--------|------|------|
| P0 | 含密钥/个人数据的公开仓库 | 立即转 private |
| P1 | 第三方代码无归属标记 | fork 或 submodule 化 |
| P2 | 缺 README | 补充中文 README |
| P3 | 缺 description | 补充 About 描述 |
| P4 | 已废弃/不活跃 | 归档（archive） |

方案必须包含：每个受影响仓库的具体 `gh` 命令。

### Step 3: 执行（用户确认后）

按方案逐项执行，每完成一项验证一项：
```bash
# 转私有
gh repo edit wampeeHuang/<repo> --visibility private --accept-visibility-change-consequences

# 补充 description
gh repo edit wampeeHuang/<repo> --description "..."
```

### Step 4: 自检

- [ ] 所有已知 P0 问题已消除
- [ ] 公开仓库数 = 上次快照 - 私有化数
- [ ] 每个公开仓库有 README + description
- [ ] claude-skills 中第三方 skill 有归属标注或 submodule
- [ ] 无新产生的问题

### Step 5: 更新记录

在 references/ 目录下更新 `operation-log.md`，记录本次会话的操作。快照写 `_runtime/`（临时），管理规则如有变化更新 references/。

## 特殊情况

- **仓库已 fork 但 fork 标记丢失**：找到源仓库 URL，在 README 显式标注 "Forked from <URL>"，同时在 About 加链接
- **Skill 合集仓库**：每个第三方 skill 单独 fork → submodule 引用，或 README 列表标注来源。不自建 submodule 的不保留在合集里
- **用户给了特定 README 模板**：严格按用户给的模板写，不自己发明格式
- **GitHub Projects 权限不足**：token 缺少 `read:project` scope 时无法查询 Projects V2，标注限制并建议用户补充权限

## References

- [仓库分类细则](references/repo-classification.md) — 三类仓库的详细判定树
- [审计检查清单](references/audit-checklist.md) — 完整审计项和验证命令
- [整理方案模板](references/cleanup-plan-template.md) — 方案输出格式和执行记录
