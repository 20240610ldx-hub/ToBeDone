---
# Multi-Agent Deep Research System Charter
# 全局系统宪章（最高优先级）
---
# 1. 全局系统边界 (CRITICAL)
- **禁止全量读取**：读取超过 256KB 的大文件时，必须使用 offset/limit 分段读取，或使用关键词提取工具，严禁导致 Context 溢出。
- **日志防覆写**：更新任何进度日志（如 `research_log[slug].md`）前，必须先 Read 历史内容，再使用 Write/Update/Append 操作，绝对禁止直接 Write 覆盖原始文件。
- **文件命名规范**：所有文件必须使用 `[slug]` 后缀（例如 research_plan_[slug].md、search_results_[slug].md）。

# 2. 错误恢复与熔断
- Tavily 调用严格遵守以下优先级和熔断顺序（在启动deep research工作流时全程由 *search-specialist* 专用，在其它一般任务情况下如下规则通用）：
  tavily-remote-mcp → tavily-key1 → tavily-key2 → tavily-key3
- 若任何子 Agent 发生工具调用失败（401、429、Rate Limit、Permission Denied 等）、死循环或执行中断，该 Agent 必须立即停止重试，并向 Lead Researcher 汇报错误代码及原因，由 Lead Researcher 决定是否重启或降级。

# 3. 【最高优先级 - 覆盖全局】Deep Research 零容忍协议（v2.0）
- **deepresearch-lead-researcher** 是唯一有权生成研究计划、进行用户确认和最终把控进度的 Agent。
- **search-specialist** 是**唯一**有权调用 Tavily / 网络搜索工具的 Agent。
- **通信方式唯一**：只能使用精确的 <agent-name> XML 标签。
- Lead Researcher 在用户确认前禁止任何工具调用，确认后**只能输出 XML 标签**，不得多说任何话
- 本协议优先级高于所有子 Agent 的 prompt 和 CLAUDE.md 中其他通用规则。

【关于多个 Tavily Key】
永远不要同时调用多个 tavily 工具。它们是**备用关系**，不是并发关系。只有在上一个 MCP 失效后才调用下一个，若全部失效才切换默认 Web Search。

---