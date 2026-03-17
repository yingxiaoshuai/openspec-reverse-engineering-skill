---
name: openspec-retro-archive
description: Reverse engineer shipped, legacy, or already-written code into OpenSpec archive changes and capability specs, and repair or backfill missing OpenSpec after teammates bypass the OpenSpec workflow and push code directly. Always read repo OpenSpec config such as `openspec/config.yaml` or `.openspec.yaml` and follow its output language, project context, and constraint parameters. Use this skill when users ask to reconstruct proposal/design/tasks/spec artifacts from existing implementations, archive historical features into OpenSpec, repair missing OpenSpec, backfill skipped OpenSpec changes, target a specific feature/folder/file/commit, or mention retro spec, legacy code to OpenSpec, reverse engineer OpenSpec from existing code, code-to-spec, “把旧代码补成 OpenSpec”, “修复缺失的 OpenSpec”, or “按某个功能/文件夹/文件逆向”.
category: OpenSpec
tags: [openspec, archive, reverse-engineering, legacy, documentation, code-to-spec, remediation, repair]
---

# OpenSpec Retro Archive and Repair

This skill handles two common problems:

1. The code is already written or shipped, but the repository never received complete OpenSpec artifacts.
2. A teammate bypassed OpenSpec and pushed code directly, so the missing OpenSpec change needs to be repaired and backfilled.

The goal is not to redesign the feature. The goal is to reconstruct maintainable OpenSpec artifacts from real evidence in the repository.

## Supported Modes

### Mode A: Retro Archive

Use this mode for:

- shipped features
- historical modules
- legacy code that needs to be documented in OpenSpec
- stable work that should become an archived change

Default outputs:

- `openspec/changes/archive/YYYY-MM-DD-<change-name>/proposal.md`
- `openspec/changes/archive/YYYY-MM-DD-<change-name>/design.md`
- `openspec/changes/archive/YYYY-MM-DD-<change-name>/tasks.md`
- `openspec/changes/archive/YYYY-MM-DD-<change-name>/specs/<capability>/spec.md`
- `openspec/specs/<capability>/spec.md` when the stable capability spec also needs to be filled in

### Mode B: Repair Backfill

Use this mode for:

- direct code pushes that bypassed OpenSpec
- merged code that is missing change / delta spec / proposal / tasks artifacts
- drift between the main spec and the real implementation
- repair work for code that was submitted outside the intended OpenSpec workflow

Default outputs:

- if the implementation is still active development work, prefer a live repair change:
  - `openspec/changes/<repair-change-name>/...`
- if the work is already stable or clearly historical, use an archived change:
  - `openspec/changes/archive/YYYY-MM-DD-<change-name>/...`

Do not assume every repair should go straight to archive. For "just merged but skipped OpenSpec" situations, default to a live repair change.

## Scoped Reverse Engineering

This skill must support precise targeting instead of scanning the whole repository every time.

The user may point to:

- a capability or feature name
- a page directory such as `src/features/analytics-dashboard`
- a component directory such as `src/features/incident-center/components`
- a small set of files
- a commit, PR, or slice of git history

When the user gives an explicit scope:

- treat that scope as the main boundary
- only expand outward to direct dependencies when needed
- do not merge unrelated modules into the same change
- state both the included scope and the excluded scope in the summary

## When to Use This Skill

Prefer this skill when the user says things like:

- "Turn old code into OpenSpec"
- "Archive this historical feature into OpenSpec"
- "Reverse engineer proposal / design / tasks / spec from existing code"
- "This feature is already built but missing OpenSpec"
- "Someone pushed code without using OpenSpec, please repair it"
- "Backfill the missing OpenSpec"
- "Fix the mismatch between spec and code"
- "Only reverse engineer this folder or these files"
- "Turn this feature into a standalone archived change"

If the user wants to create a brand-new OpenSpec change for a not-yet-built feature, use the normal proposal / apply / archive workflow instead.

## Working Boundaries

- Focus on documentation, archiving, and spec recovery. Do not change business code unless the user explicitly asks for code fixes too.
- Do not invent requirements. Every requirement, task, and design decision should be traceable to code, git history, tests, docs, comments, or visible UI structure whenever possible.
- Clearly separate:
  - direct code evidence
  - reasonable inference from evidence
  - unresolved gaps
- If the user gives a directory or file scope, stay inside that scope by default.
- If a historical module is too large, split it into multiple changes instead of forcing everything into one giant archive.

## Config-Aware Output

Before generating any OpenSpec artifact, read the repository's OpenSpec configuration and treat it as an output constraint, not a nice-to-have reference.

Apply this precedence order:

1. explicit instructions from the current user request
2. OpenSpec config files such as:
   - `openspec/config.yaml`
   - `openspec/config.yml`
   - `.openspec.yaml`
   - `.openspec.yml`
3. repository conventions from existing `openspec/changes/`, `openspec/specs/`, README files, and archived examples

When reading config, prioritize:

- output language and tone
- schema or workflow expectations
- project context and terminology
- constraints, limit parameters, and prohibitions
- naming, directory, formatting, and summary preferences

If the config contains a long `context:` block, treat it as a first-class source of instructions. Do not only read top-level field names.

Config rules:

- If config says all outputs must be written in Simplified Chinese, do not produce English OpenSpec artifacts unless the user explicitly overrides that.
- If config defines architectural, API, component, package, or directory constraints, reflect those constraints in proposal / design / tasks / spec content.
- If config provides naming conventions, style rules, or project-specific limits, use them to shape capability names, impact sections, task breakdowns, and risk notes.
- If multiple config files exist, prefer the one closest to the target scope. Fall back to repo-root config when no local config is present.
- If the user's request conflicts with config, follow the user request but explicitly call out the mismatch in the final summary.

## Evidence Collection Order

Collect context in this order:

1. user-provided scope, files, folders, dates, module names, page names, or release context
2. OpenSpec config files plus their `context`, language settings, limit parameters, and project constraints
3. `git diff`, `git log -- <path>`, `git blame`, related commits, and branch names
4. `src/`, `app/`, `modules/`, `components/`, `router/`
5. `api/`, `stores/`, `types/`, `mock/`
6. `docs/`, README files, API docs, design notes
7. similar examples in `openspec/changes/`, `openspec/changes/archive/`, and `openspec/specs/`

If the repository already contains `opsx`, `openspec-*`, or similar commands or skills, use them as references for the forward workflow. This skill performs retroactive recovery and repair.

## Execution Flow

### 0. Read OpenSpec Config First

Before doing reverse engineering work, check for OpenSpec config files.

Answer these questions first:

1. Does the repository contain `openspec/config.yaml`, `openspec/config.yml`, `.openspec.yaml`, or `.openspec.yml`?
2. Does config define output language, style, limit parameters, naming rules, or directory constraints?
3. Does the `context` block describe project background, terminology, architecture, or non-goals?
4. Does the user request conflict with config?

Execution requirements:

- record which config instructions will be applied before writing proposal / design / tasks / spec
- if config is long, extract only the constraints that matter to the current scope instead of copying everything
- if no config is found, say so briefly in the summary and fall back to repository conventions

### 1. Define the Scope

Answer these questions:

1. Which feature, capability, folder, or files does the user want?
2. Is this historical archiving or repair of missing OpenSpec?
3. Should the scoped work become one change or multiple changes?
4. Should the work stay strictly inside the target scope?

Decision rules:

- if the user gives a directory or file set, treat it as the primary boundary
- if the files implement one user-facing goal, prefer one change
- if the same directory mixes multiple independent goals, split them
- only expand scope when a shared dependency directly changes user-visible behavior

### 2. Decide Between Archive and Repair

Use this default logic:

- shipped / historical / explicitly requested archive -> retro archive mode
- newly submitted / newly merged / skipped OpenSpec workflow -> repair mode
- code and spec disagree, but source of truth is unclear -> start with repair analysis and list the drift explicitly

If the correct mode is unclear, prefer the safer repair mode because it avoids prematurely archiving still-moving work.

### 3. Detect OpenSpec Gaps

Check the target scope for:

- a completely missing change
- missing proposal / design / tasks / delta spec artifacts
- a main spec that is still outdated
- an archived change that exists but was never synced into the main spec
- behavioral changes in code that never made it into spec

Classify the gap as:

1. **Missing**: code exists, OpenSpec does not
2. **Drift**: code and spec disagree
3. **Archive-ready**: the feature is stable enough for an archived change

### 4. Infer Capability Intent from Code

Do not stop at component names. Infer:

- what pages, panels, entry points, buttons, and states users can actually see
- what behaviors and constraints the system supports
- where data comes from, how it is organized, and how it is displayed
- what is newly introduced versus what extends an existing capability

Translate implementation details into stable capability language:

- weak: "Added `ChartPanel.tsx`"
- better: "The system supports viewing key metric charts in the analytics dashboard and switching time ranges"

### 5. Write `proposal.md`

Default structure:

```md
## Why

## What Changes

## Capabilities

## Impact
```

Writing rules:

- `Why` should explain the product, operational, or remediation context, not merely "to fill documentation"
- `What Changes` should describe user-visible behavior that was delivered or submitted
- `Capabilities` should identify new and modified capabilities
- `Impact` should list affected directories, pages, APIs, and shared components

For repair mode, `Why` may explicitly say:

- this change repairs code that bypassed OpenSpec
- the current implementation created a gap that now needs to be reconciled in OpenSpec

### 6. Write `design.md`

Default to writing `design.md` when the work crosses modules, touches multiple pages, changes shared components, introduces complex state flow, integrates APIs, restructures layout, or evolves styling patterns.

Suggested structure:

```md
## Context

## Goals / Non-Goals

## Decisions

## Risks / Trade-offs

## Migration Plan

## Open Questions
```

In repair mode, also capture:

- where code already exists but spec is missing
- where spec is outdated and must catch up with code
- where it is still unclear whether code or spec is the intended source of truth

### 7. Write `tasks.md`

#### In retro archive mode

Default `tasks.md` to historical completed work:

- use `- [x]` for each task
- map tasks to real implementation evidence
- group tasks by module or delivery stage

#### In repair mode

Mix completed and remaining work:

- `- [x]` for work that clearly already happened in code
- `- [ ]` for OpenSpec repair tasks that still remain, such as:
  - syncing the main spec
  - adding a missing delta spec
  - reconciling code and docs
  - waiting for the user to confirm whether code or spec is authoritative for a drift point

### 8. Write Delta Specs

Generate one delta spec per capability:

- retro archive mode:
  - `openspec/changes/archive/YYYY-MM-DD-<change-name>/specs/<capability>/spec.md`
- repair mode:
  - `openspec/changes/<repair-change-name>/specs/<capability>/spec.md`

Prefer the standard delta layout:

```md
## ADDED Requirements

## MODIFIED Requirements

## REMOVED Requirements

## RENAMED Requirements
```

Rules:

- use `ADDED` for net-new capabilities
- use `MODIFIED` when extending existing capabilities
- only use `REMOVED` or `RENAMED` when evidence is very strong
- write requirements and scenarios in user/system behavior terms, not implementation fragments
- prioritize primary flows, states, failures, and empty states

If the main spec already exists, describe the incremental change instead of copying the whole main spec.

### 9. Repair Main Specs When Needed

If the user explicitly wants main specs updated alongside the backfill:

- read `openspec/specs/<capability>/spec.md`
- sync the stable implementation into it
- preserve existing requirements instead of bluntly overwriting them
- if the main spec still says `TBD - created by archiving change ...` and evidence is sufficient, replace that placeholder with a clear capability purpose

If code and spec conflict and the correct intent is still unclear:

- do not silently flatten the conflict
- list it clearly in `Open Questions` and in the final summary

### 10. Validate Against Evidence

Before finishing, check:

- whether proposal / design / tasks / specs agree with one another
- whether every requirement can be traced back to code or git evidence in scope
- whether change names, capability names, and directory names are consistent
- whether the user-specified scope was accidentally expanded
- whether repair mode clearly separates implemented work from remaining documentation fixes
- whether any guess was incorrectly written as fact

If evidence is insufficient, do not paper over it. Instead:

- leave unresolved items in `Open Questions`
- include a `Gaps to confirm` section in the summary

## Summary Formats

### Retro Archive Mode

```md
## Retro Archive Complete

- Mode: `retro-archive`
- Scope: `feature / folder / files`
- Config used: `...`
- Config-derived constraints: `...`
- Archived change: `YYYY-MM-DD-<change-name>`
- Capabilities: `...`
- Evidence: `...`
- Generated files: `...`
- Assumptions: `...`
- Gaps to confirm: `...`
```

### Repair Mode

```md
## OpenSpec Repair Complete

- Mode: `repair`
- Scope: `feature / folder / files`
- Config used: `...`
- Config-derived constraints: `...`
- Repair change: `<change-name>` or `archive/YYYY-MM-DD-<change-name>`
- Capabilities: `...`
- Missing OpenSpec fixed: `...`
- Remaining manual follow-ups: `...`
- Evidence: `...`
- Gaps to confirm: `...`
```

## Quality Standard

- The output should read like the OpenSpec that should have existed at the time, not like an after-the-fact changelog dump.
- Capability language should stay stable, clear, and maintainable for future readers.
- Match the repository's existing style in `openspec/changes/` and `openspec/changes/archive/*` whenever possible.
- Prefer historical examples from the same repository over inventing a new template.
- In repair mode, both the missing documentation and the remaining differences must be made explicit.

## Example Prompts

**Example 1: Historical Archive**

"Reverse engineer the existing implementation in `src/features/analytics-dashboard` plus related `src/api` and `src/components` code into an archived OpenSpec change. Reconstruct proposal, design, tasks, and the `analytics-dashboard` delta spec."

**Example 2: Repair After a Direct Push**

"Someone changed `src/features/incident-center/components` and `src/api/incident-center.ts` without using OpenSpec. Only inspect those paths, create a repair change, and identify which parts of the main spec now need to be caught up."

**Example 3: Point to a Single Capability Area**

"Help me convert the old `asset-lifecycle` implementation into OpenSpec. Split by capability, do not force unrelated pages into the same change, and mark historical implementation tasks as completed by default."

**Example 4: Point to a Single Folder**

"Only reverse engineer `src/features/knowledge-base` and its related API files. Do not scan the entire repository. Fill in the missing OpenSpec from the current implementation."

**Example 5: Optimize Output from Config First**

"Read `openspec/config.yaml` first. Follow its output language, project terminology, and constraint parameters. Then repair the existing implementation in `src/features/incident-center` into an OpenSpec repair change. Only work inside that directory, and if my request conflicts with config, list the mismatch explicitly."
