# 审计检查清单

每次 `github-mgmt` 触发时逐项检查。

## 全量扫描

```bash
gh api /users/wampeeHuang/repos?per_page=100 --paginate \
  --jq '.[] | {name, private, fork, lang, pushed, archived, desc}'
```

## 逐仓库检查

对每个仓库：

- [ ] **分类**：project / skill / infra / unknown / flagged
- [ ] **README**：有/无，中文/英文，是否完整（定位+技术栈+运行方式）
- [ ] **description**（About）：有/无，是否说清用途
- [ ] **可见性**：public / private，是否该调整
- [ ] **fork 标记**：有/无，源仓库是否标注
- [ ] **归档状态**：是否已废弃超过 6 个月
- [ ] **Topics**：有/无，是否合理
- [ ] **Homepage**：有/无，是否有效

## claude-skills 专项检查

```bash
gh api repos/wampeeHuang/claude-skills/contents/.gitmodules --jq ".content" | base64 -d
gh api repos/wampeeHuang/claude-skills/contents --jq '.[].name'
```

- [ ] submodule 数 vs 总目录数
- [ ] 非 submodule 目录中，有多少是第三方 skill
- [ ] 第三方 skill 在 README 中是否有归属标注
- [ ] 自建 skill 是否已独立建仓

## 风险标记

| 风险等级 | 条件 | 动作 |
|----------|------|------|
| **P0 紧急** | 公开仓库含 AppID/密钥/个人数据 | 立即转私有 |
| **P1 归属** | 第三方代码无 fork/submodule 标记 | 补 fork 或拆分 |
| **P2 裸奔** | 公开仓库无 README | 补中文 README |
| **P3 无描述** | 仓库无 description | 补 About |
| **P4 僵尸** | 6个月无更新，无 star，无描述 | 归档 |

## 总览统计

审计结束时输出：
- 仓库总数
- 公开/私有数
- 各分类数量（project/skill/infra/unknown/flagged）
- 各风险等级数量
- README 覆盖率
- description 覆盖率
- 与上次审计的差异
