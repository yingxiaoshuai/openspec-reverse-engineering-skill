# 示例提示词

## 示例 1：历史归档

请把 `src/features/analytics-dashboard` 以及相关的 `src/api`、`src/components` 现有实现逆向整理成一个 archived OpenSpec change，并补齐 proposal、design、tasks 和 `analytics-dashboard` 的 delta spec。

## 示例 2：补修未走 OpenSpec 的提交

有人直接改了 `src/features/incident-center/components` 和 `src/api/incident-center.ts`，但没有创建 OpenSpec change。请只检查这些路径，生成一个 repair change，并指出主 spec 哪些部分已经和代码漂移。

## 示例 3：历史模块补档

帮我把历史上的 `asset-lifecycle` 模块补成 OpenSpec。要求按 capability 拆分，不要把无关页面塞进同一个 change，历史实现任务默认标记为已完成。

## 示例 4：按文件夹定点逆向

只使用 `src/features/knowledge-base` 和相关 API 文件，不要扫描整个仓库。请把这部分现有实现逆向成 OpenSpec，并把范围外模块排除掉。

## 示例 5：修复 spec 漂移

代码行为已经变了，但我不确定是代码对还是 spec 对。请检查目标目录，识别漂移点，并生成一个偏 repair 的 OpenSpec change，不要修改业务代码。

## 示例 6：先按配置补修

请先读取 `openspec/config.yaml`，严格遵循里面的输出语言、项目术语和限制参数。然后只检查 `src/features/incident-center`，为这块缺失的 OpenSpec 生成一个 repair change，不要扫描仓库里的其他模块。

## 示例 7：同步过期 capability spec

OpenSpec 好久没更新了。请根据当前 layout 相关代码和 `openspec/specs/layout/spec.md`，把 `layout` capability 同步到最新状态，说明哪里发生了 drift；如果发现之前有人绕过 OpenSpec 提交，也顺手补齐需要的 repair artifacts，而不是把这件事当成普通文档润色。

## 示例 8：间接说法的定点请求

帮我更新一下 `@layout`。只检查 layout 相关实现和现有 OpenSpec 文件，判断应该走 repair backfill 还是稳定 spec sync，并把不在范围内的模块排除掉。

## 示例 9：大仓安全模式

这个仓库很大，而且 OpenSpec 已经落后代码了。不要扫描整个项目。请先建立候选路径清单，再收敛到 `layout-shell` capability 最可能相关的目录，只读取有代表性的实现文件和对应 spec，并在总结里说明哪些路径因为范围或上下文预算被刻意延后处理。

## 示例 10：触发硬阈值时先停

请把这个仓库里所有缺失的 OpenSpec 都补上。但在读大量文件之前，先判断这个请求是否已经超过阈值；如果超过，就先输出 scope plan，把工作拆成更小的 capability 切片，而不是一次性试图处理整个仓库。
