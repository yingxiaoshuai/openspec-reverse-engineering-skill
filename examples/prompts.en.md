# Example Prompts

## Example 1: Retro Archive

Please reverse engineer the existing `src/features/analytics-dashboard`, related `src/api`, and `src/components` implementation into an archived OpenSpec change, and reconstruct the proposal, design, tasks, and `analytics-dashboard` delta spec.

## Example 2: Repair Missing OpenSpec After Direct Push

A teammate changed `src/features/incident-center/components` and `src/api/incident-center.ts` without creating an OpenSpec change. Please only inspect those paths, create a repair change, and list which main spec sections now drift from code.

## Example 3: Historical Backfill

Help me convert the historical `asset-lifecycle` module into OpenSpec. Split by capability, do not force unrelated pages into one change, and mark tasks as completed by default.

## Example 4: Folder-Scoped Reverse Engineering

Only use `src/features/knowledge-base` and its related API files. Do not scan the entire repository. Reverse engineer that scope into OpenSpec and keep unrelated modules out of the result.

## Example 5: Repair Spec Drift

The code behavior changed, but I am not sure whether the code is correct or the spec is correct. Please inspect the target folder, identify the drift, and create a repair-oriented OpenSpec change without changing business code.

## Example 6: Config-Aware Repair

Read `openspec/config.yaml` first and follow its language, terminology, and constraint settings while generating the result. Then inspect only `src/features/incident-center` and create a repair change for the missing OpenSpec without scanning the rest of the repository.
