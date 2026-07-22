# 仓库分类细则

## 判定树

```
仓库
├── 包含 SKILL.md 或 Claude Code skill 结构？
│   ├── 是 → 谁写的？
│   │   ├── wampeeHuang → 原创 Skill
│   │   └── 别人 → 第三方 Skill（需 fork 标记或 submodule）
│   └── 否 → 继续
├── 是配置/合集/备份性质？
│   ├── 是 → 配置/基建
│   └── 否 → 继续
├── 主要代码是谁写的？
│   ├── wampeeHuang → 原创项目
│   └── 别人 → 归属风险，标记 flagged
└── 不确定 → 标记 unknown，不强行分类
```

## 分类特征对照

### 原创项目
- 有业务逻辑代码（非纯配置）
- 独立可运行或有明确产品形态
- 有 CLAUDE.md 或项目结构
- 示例：agentboard, thinkprism-miniapp, layout-factory

### 原创 Skill
- 根目录有 SKILL.md（Claude Code skill 规范）
- 目录名通常是 kebab-case
- 功能聚焦：一个 skill 做一件事
- 示例：arch-diagram, screenshot-vision, vercel-deploy

### 配置/基建
- 主要是 .md / .json / .ps1 等配置文件
- 无独立业务逻辑
- 本质是"组织工具"，不是"产品"
- 示例：claude-config, backup-warehouse

## 第三方代码识别

以下信号标记第三方代码：
- 文件头注释包含其他人的 GitHub 用户名或版权
- SKILL.md 的 author/license 指向别人
- 目录结构与已知开源项目一致
- README 语言和风格与 wampeeHuang 不一致（英文 AI Studio 模板等）
- `.gitmodules` 中 url 指向别人仓库但无 fork 标记

## 公开/私有决策

私有（默认）：
- 含 AppID / API 端点结构 / 内部架构细节
- 微信小程序源码
- 实验项目、半成品
- 用户指定

公开（需满足全部条件）：
- README 完整
- 无敏感信息
- 有展示价值
- 用户同意
