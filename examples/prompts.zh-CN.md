# 示例提示词

## 示例 1：历史归档

请把 `src/features/analytics-dashboard` 和相关 `src/api`、`src/components` 的现有实现，反推成一个 archived OpenSpec change，并补齐 proposal、design、tasks 和 `analytics-dashboard` 的 delta spec。

## 示例 2：修复未走 OpenSpec 的提交

有人直接改了 `src/features/incident-center/components` 和 `src/api/incident-center.ts`，但没走 OpenSpec。请只针对这些目录做修复补档，生成一个 repair change，并指出主 spec 哪些地方需要追平。

## 示例 3：按 capability 拆分历史模块

帮我把 `asset-lifecycle` 这块旧代码补成 OpenSpec。要求按 capability 拆分，不要把无关页面塞到一个 change 里，所有历史实现任务默认标成已完成。

## 示例 4：只针对一个文件夹逆向

只针对 `src/features/knowledge-base` 和它的相关 API 做逆向，不要扫描整个仓库。请根据当前实现补齐 OpenSpec。

## 示例 5：修复 spec 漂移

代码行为已经改了，但我不确定到底是代码对还是 spec 对。请检查目标目录，识别漂移点，并生成一个偏修复用途的 OpenSpec change，不要修改业务代码。

## 示例 6：按配置输出的修复补档

请先读取 `openspec/config.yaml`，并严格遵循里面的输出语言、项目术语和限制参数。然后只检查 `src/features/incident-center`，为这块缺失的 OpenSpec 生成一个 repair change，不要扫描仓库里的其他模块。
