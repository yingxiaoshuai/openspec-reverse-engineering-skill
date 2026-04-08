# openspec-retro-archive

English | [简体中文](./README.zh-CN.md)

An OpenSpec skill for:

- reverse engineering existing code into OpenSpec
- repairing missing OpenSpec after direct code pushes
- reconstructing proposal/design/tasks/spec artifacts from real implementation
- backfilling historical features into OpenSpec archive changes
- reading `openspec/config.yaml` or `.openspec.yaml` to honor repo language, schema, and constraint settings
- targeting a single feature, folder, or file set instead of scanning the whole repository

## What This Skill Solves

Many teams do not consistently follow OpenSpec. Sometimes features are shipped first and specs are written later. Sometimes teammates push code directly without creating an OpenSpec change.

This skill covers both cases:

1. **Retro archive mode**
   - turn existing or shipped code into archived OpenSpec changes
2. **Repair mode**
   - repair or backfill missing OpenSpec after someone bypasses the OpenSpec workflow

It can work from:

- a feature name
- a capability name
- a folder such as `src/features/analytics-dashboard`
- a small set of files
- a commit range or recent git history

Before generating output, it can also read OpenSpec configuration such as `openspec/config.yaml` and `.openspec.yaml` to align language, terminology, formatting, and project-specific constraints.

## Core Capabilities

- infer change boundaries from existing implementation instead of stuffing all old code into one archive
- generate OpenSpec artifacts from code evidence, docs, and git history
- repair missing OpenSpec after teammates submit code without using OpenSpec
- honor repo-level OpenSpec config for output language, schema, terminology, and constraint parameters
- support folder-scoped and file-scoped reverse engineering
- separate direct evidence, reasonable inference, and unresolved gaps
- write historical tasks as completed by default, while allowing remediation tasks to stay open

## Project Structure

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

## Installation

Install with the Agent Skills CLI:

```bash
npx @agentskill.sh/cli@latest setup
npx skills add yingxiaoshuai/openspec-reverse-engineering-skill
```

Or install manually by copying into your agent's skills directory:

```bash
# Claude Code
cp -r openspec-retro-archive ~/.claude/skills/openspec-retro-archive

# Cursor
cp -r openspec-retro-archive ~/.cursor/skills/openspec-retro-archive

# OpenAI Codex
cp -r openspec-retro-archive ~/.agents/skills/openspec-retro-archive
```

Browse this skill on the [Agent Skills Directory](https://skills.sh).

## Example Prompts

- English prompts: [prompts.en.md](./examples/prompts.en.md)
- 中文提示词: [prompts.zh-CN.md](./examples/prompts.zh-CN.md)

Typical requests:

- "Please reverse engineer the existing `src/features/analytics-dashboard` implementation into an archived OpenSpec change."
- "A teammate pushed code directly to `src/features/incident-center/components` without OpenSpec. Please create a repair change and fix the missing spec coverage."
- "Only use `src/features/knowledge-base` and its related API files. Do not scan the whole repository."
- "Read `openspec/config.yaml` first, follow its language and constraint settings, then repair the missing OpenSpec for `src/features/incident-center`."

## Evaluation

Evaluation prompts live in [evals.json](./evals/evals.json). They now cover:

- retro archive workflows
- repair workflows after direct code pushes
- feature-scoped reverse engineering
- folder-scoped reverse engineering
- config-aware language and constraint handling

## License

Apache-2.0. See [LICENSE](./LICENSE).
