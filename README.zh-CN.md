# OpenClaw 团队模板

[English README](./README.md)

这是我当前 OpenClaw 多 Agent 工作流的一份**脱敏开源模板**。

这个仓库**不是**一个正在运行的本地 runtime 目录镜像，而是一个适合公开分享的**模板 / 规范仓库**。它保留了系统背后的架构、角色边界、路由规则和工作流约定，同时主动移除了 secrets 和隐私运行态。

## 为什么要开这个仓库

很多所谓的多 Agent 项目，重点都放在“怎么调模型”，但真正难的是**团队设计**：

- 谁可以做什么，谁不可以做什么
- 任务如何交接
- 如何保证 review 独立
- 如何隔离上下文
- 如何让交付通过明确的流程门禁

这个仓库想分享的正是这部分方法论。

## 核心设计思想

这套 OpenClaw 配置围绕五个原则构建：

1. **Leader 中心化编排**  
   specialist 默认不自由互相协调，而是由 Leader 负责路由、澄清和汇总。

2. **强角色边界**  
   Researcher 负责研究设计，Coder 负责实现，Reviewer 负责审查，Writer 负责文档，Tester 负责验证。审批权与生产权分离。

3. **以 artifact 驱动交接**  
   交接依赖 `design_spec`、`patch`、`test_report`、`approval_decision` 等明确产物，而不是依赖随意聊天。

4. **阶段化工作流**  
   整个交付流程遵循固定主线：`Discover -> Design -> Build -> Test -> Review -> Submit -> Learn`。

5. **隔离 workspace / session**  
   每个角色默认在独立 workspace/session 中运行，跨角色访问必须显式发生，而不是默认共享上下文。

## 团队模型

这个模板使用六个角色：

- **Leader** —— 编排、路由、聚合、升级处理
- **Researcher** —— 调研、澄清、设计、任务拆解
- **Coder** —— 实现与自验证
- **Reviewer** —— 独立审查、批准决策、风险检查
- **Writer** —— 文档、发布说明、对外内容
- **Tester** —— E2E 验证、覆盖说明、失败分析

关键文档：

- `docs/team/ROLE_SPEC.md`
- `docs/team/PLAYBOOK.md`
- `docs/team/ARTIFACTS.md`
- `docs/team/TASK_SCHEMA.md`
- `workspaces/leader/WORKFLOW.md`

## 工作流模型

标准流程：

```text
Discover -> Design -> Build -> Test -> Review -> Submit -> Learn
```

一个典型任务通常这样流转：

1. Leader 先派给 Researcher 做 discovery 或 design 澄清。
2. 范围清楚后，再派给 Coder 进入实现。
3. Tester 负责验证结果并记录证据。
4. Reviewer 只接收最小必要 review bundle，并给出明确结论。
5. 如果需要对外表达，再交给 Writer。
6. 最后由 Leader 汇总已批准产物并推进任务。

## 仓库结构

```text
.
├── docs/team/           # 角色定义、协作规则、artifact schema
├── workspaces/          # 各角色启动说明、身份定义、使用约定
├── completions/         # shell 补全脚本
└── examples/            # 脱敏后的配置示例与环境变量模板
```

## 有哪些内容被刻意排除

原始本地 OpenClaw runtime 中有大量不适合公开的运行态信息，这里都没有包含：

- API keys 和 access tokens
- bot 凭据
- device identity 和 pairing 数据
- 本地 memory 数据库
- session transcript 和日志
- cron / task runtime 状态
- 本地 `.env` 文件
- 数据库、缓存和构建产物

发布排除规则可直接看 `.gitignore`。

## 如何基于这个模板改造

1. 把 `examples/openclaw.example.json` 复制成你自己的本地配置。
2. 用你自己的 provider、token 和 channel 配置替换占位符。
3. 在本地创建运行时文件（`.env`、credentials、memory、logs、session storage、device identity` 等）。
4. 公开仓库尽量只保留模板、契约、文档和无敏感信息的脚本。

## 开源发布建议

把这个仓库当作一个**模板 / spec 仓库**，不要把它当作 live runtime 的镜像。

适合公开的内容：

- 角色契约
- workflow 文档
- prompts 与协作约定
- 脱敏示例配置
- 非敏感辅助脚本

不适合公开的内容：

- sessions
- memory
- logs
- credentials
- device identity
- runtime 数据库
- 真实生产配置

## 说明

部分复制过来的角色文档仍然引用了 `SOUL.md`、`USER.md`、`WORKFLOW_STATE.yaml` 等私有运行时文件。这些引用被保留，是为了展示原始工作流形状，但它们**不是**这个公开模板必须附带的内容。

## License

本仓库采用 MIT License，见 [LICENSE](./LICENSE)。
