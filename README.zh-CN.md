# openspec-retro-archive

[English](./README.md) | 简体中文

这是一个用于把“现有代码 / 旧代码 / 已上线功能”反向沉淀成 OpenSpec 的 skill，同时也支持修复“团队成员没有走 OpenSpec 流程就直接提交代码”的情况。

它主要覆盖两类场景：

1. **逆向归档**
   - 把历史模块或已上线功能整理成 archived OpenSpec change
2. **修复补档**
   - 团队有人没按 OpenSpec 提交流程走，代码已经提交或合并，需要补齐和修复 OpenSpec

并且它支持明确指向范围：

- 某个功能
- 某个 capability
- 某个文件夹
- 某几个文件
- 某段 git 历史

也就是说，你可以让它只处理 `src/features/knowledge-base`，而不是默认扫整个仓库。

在生成结果之前，它还可以先读取 `openspec/config.yaml` 或 `.openspec.yaml`，继承仓库里定义的输出语言、schema、术语和限制参数。

## 这个项目解决什么问题

很多团队会出现这几种情况：

- 先写代码，后补规范
- 功能已经上线，但从来没写过 OpenSpec
- 团队成员直接提交代码，没有创建 OpenSpec change
- 代码已经改了，但 spec 没同步

这个 skill 的目标就是把真实代码、文档、mock 数据和 git 历史，恢复成可维护的 OpenSpec 产物。

## 核心能力

- 从现有实现反推 change 边界，而不是把所有旧代码硬塞进一个归档
- 基于代码证据、文档证据和 git 历史生成 OpenSpec 归档包
- 修复团队成员绕过 OpenSpec 直接提交代码后的缺失文档
- 读取 `openspec/config.yaml` / `.openspec.yaml`，遵循仓库配置中的语言、术语、schema 和限制参数
- 支持按功能、按文件夹、按文件集合定点逆向
- 区分“直接证据”“合理推断”“待确认空白”
- 在历史补档时默认把已实现任务写成完成，在修复模式下允许保留待补 OpenSpec 项

## 项目结构

```text
openspec-retro-archive/
├─ SKILL.md
├─ LICENSE
├─ README.md
├─ README.zh-CN.md
├─ .gitignore
├─ .gitattributes
├─ evals/
│  └─ evals.json
├─ examples/
│  ├─ prompts.en.md
│  ├─ prompts.zh-CN.md
│  └─ prompts.md
└─ docs/
   └─ github-metadata.md
```

## 安装

通过 Agent Skills CLI 安装：

```bash
npx @agentskill.sh/cli@latest setup
npx skills add yingxiaoshuai/openspec-reverse-engineering-skill
```

也可以手动复制到对应 agent 的 skills 目录：

```bash
# Claude Code
cp -r openspec-retro-archive ~/.claude/skills/openspec-retro-archive

# Cursor
cp -r openspec-retro-archive ~/.cursor/skills/openspec-retro-archive

# OpenAI Codex
cp -r openspec-retro-archive ~/.agents/skills/openspec-retro-archive
```

你也可以在 [Agent Skills Directory](https://skills.sh) 上浏览和安装此 skill。

## 示例提示词

- 英文示例: [prompts.en.md](./examples/prompts.en.md)
- 中文示例: [prompts.zh-CN.md](./examples/prompts.zh-CN.md)

常见请求方式：

- “请把 `src/features/analytics-dashboard` 的现有实现反推成一个 archived OpenSpec change。”
- “有人直接改了 `src/features/incident-center/components`，但没走 OpenSpec。请帮我生成一个 repair change，把缺失的 OpenSpec 补回来。”
- “只针对 `src/features/knowledge-base` 和相关 API 做逆向，不要扫描整个仓库。”
- “请先读取 `openspec/config.yaml`，按里面的语言和限制参数输出，再修复 `src/features/incident-center` 缺失的 OpenSpec。”

## 评估

测试 prompts 位于 [evals.json](./evals/evals.json)，现在已经覆盖：

- 逆向归档
- 漏流程提交后的修复补档
- 按功能逆向
- 按文件夹逆向
- 按配置继承输出语言与限制参数

## License

本仓库采用 Apache License 2.0，详见 [LICENSE](./LICENSE)。
