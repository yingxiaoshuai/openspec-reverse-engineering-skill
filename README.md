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

## Search Keywords

This repository intentionally includes the phrases below so GitHub search can match it for both English and Chinese queries:

- OpenSpec reverse engineering
- reverse engineer OpenSpec from existing code
- repair missing OpenSpec after direct code push
- OpenSpec remediation
- legacy code to OpenSpec
- code to OpenSpec
- code-to-spec
- reverse spec generation
- OpenSpec archive skill
- 从现有代码反推 OpenSpec
- OpenSpec 代码逆向
- 旧代码补 OpenSpec
- 修复缺失的 OpenSpec
- 未走 OpenSpec 的代码提交流程补救
- 按文件夹逆向 OpenSpec

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

If you want to try the skill locally:

```powershell
Copy-Item -Recurse .\openspec-retro-archive $HOME\.claude\skills\openspec-retro-archive
```

Then load it with:

```powershell
npx openskills read openspec-retro-archive
```

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

## GitHub SEO Recommendations

See [github-metadata.md](./docs/github-metadata.md) for:

- recommended repository description
- recommended GitHub topics
- recommended keyword phrases
- repository naming suggestions

These fields matter because GitHub search relies heavily on:

- repository name
- repository description
- README content
- topics

## Publishing to GitHub

Example commands:

```powershell
git add .
git commit -m "feat: add OpenSpec retro archive and repair skill"
git remote add origin <your-github-repo-url>
git push -u origin main
```

## License

No license is included yet. If you want to publish the repository publicly, add a `LICENSE` file that matches how you want others to use the project.
