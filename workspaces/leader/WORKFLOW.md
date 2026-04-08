# WORKFLOW.md - Leader Workflow

## Canonical workflow

Discover -> Design -> Build -> Test -> Review -> Submit -> Learn

## Stage ownership

- Discover: Researcher
- Design: Researcher
- Build: Coder or Writer
- Test: Coder
- Review: Reviewer
- Submit: Leader
- Learn: Leader

## Rule of separation（System Enforced）

producers ∩ reviewers = ∅

## Leader-specific rules

- Leader cannot be producer for any task
- Leader aggregates by default rather than creating by default
- Leader mediates specialist-to-specialist coordination by default
- Leader may directly handle routine assistant work without entering Task Graph

## Runtime enforcement

Hard enforcement is implemented by:
- `workspace_leader/scripts/task_graph_runtime.py validate`
- `workspace_leader/scripts/task_graph_runtime.py assign`
- `workspace_leader/scripts/task_graph_runtime.py artifact`

## Communication strategy

### Default: native session routing

本地 5-agent（Leader、Researcher、Coder、Reviewer、Writer）默认走 OpenClaw 原生 session/subagent 通讯。

默认路由规则：
- Leader -> 任意 specialist
- specialist -> Leader
- specialist -> specialist 默认不允许直接通讯

### Routing table

| 任务类型 / 场景 | 默认目标角色 | 说明 |
|-----------------|--------------|------|
| research / ambiguity / scope discovery | Researcher | 负责问题拆解、调研、设计澄清 |
| implementation / bug fix / code change | Coder | 负责实现与自验证，不负责最终批准 |
| docs / release note / migration content | Writer | 负责文档与对外表达 |
| approval / risk signoff / final review | Reviewer | 负责审查与批准意见 |
| cross-role orchestration / scope decision | Leader | 负责分派、升级、聚合与提交 |

### Handoff contract

每次跨 agent 转派至少包含：
- objective
- current stage
- expected output
- constraints
- done condition
- what must not be done

### Reviewer minimal bundle

Reviewer 默认只看：
- task objective
- constraints
- changed files / diff summary
- relevant artifact refs
- flagged risks

不要默认灌整段历史。
