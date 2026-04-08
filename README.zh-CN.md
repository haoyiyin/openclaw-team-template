# OpenClaw 团队模板

中文说明。English version: [README.md](./README.md)

这是我当前 OpenClaw 多 Agent 工作流的一份**脱敏开源模板**。

这个仓库**不包含**运行时状态、聊天历史、memory 数据库、密钥、API Key、bot token、设备凭据或本地机器隐私信息。它的目标是展示这套系统背后的**团队设计**、**角色边界**和**工作流协议**。

## 团队结构

这个模板采用六角色模型：

- **Leader** —— 编排、路由、汇总
- **Researcher** —— 调研、澄清、设计、任务拆解
- **Coder** —— 实现与自验证
- **Reviewer** —— 独立审查与批准决策
- **Writer** —— 文档、发布说明、对外内容
- **Tester** —— 验证、E2E 测试、失败分析

参见：

- `docs/team/ROLE_SPEC.md`
- `docs/team/PLAYBOOK.md`
- `docs/team/ARTIFACTS.md`
- `workspaces/leader/WORKFLOW.md`

## 工作流模型

标准流程：

```text
Discover -> Design -> Build -> Test -> Review -> Submit -> Learn
```

核心思想：

1. 默认采用 **Leader 中心化路由**
2. **specialist 不自由互聊**
3. **以 artifact 驱动交接**
4. **生产与审批分离**
5. **每个角色运行在独立 workspace/session 中**

## 仓库结构

```text
.
├── docs/team/           # 角色定义、协作 playbook、artifact schema
├── workspaces/          # 各角色启动说明与身份定义
├── completions/         # shell 补全脚本
└── examples/            # 脱敏后的示例配置和环境变量模板
```

## 从原始本地系统中移除了什么

原始的本地 OpenClaw 目录包含大量不应公开的运行态信息，这里都已排除：

- API keys 和 tokens
- bot 凭据
- device keys 和 pairing 数据
- 本地 memory 数据库
- session transcript 和日志
- cron / task 运行态
- 构建产物与缓存
- 本地应用 `.env` 文件

发布时的排除规则见 `.gitignore`。

## 如何基于这个模板改造

1. 将 `examples/openclaw.example.json` 复制为你自己的本地配置文件。
2. 填入你自己的 provider、token 和 channel 设置。
3. 在本地创建你自己的运行时文件（`.env`、credentials、memory、logs、session storage`）。
4. 公开仓库只保留模板、文档和无敏感信息的辅助脚本。

## 推荐的开源方式

把这个仓库视为一个**模板/spec 仓库**，而不是一个正在运行的 agent runtime 目录镜像。

这意味着：

- 可以公开 docs、角色契约、prompt、非敏感脚本
- 不要公开 session、memory、logs、device identity 或 credentials

## 说明

部分复制过来的角色文档仍然会引用 `SOUL.md`、`USER.md`、`WORKFLOW_STATE.yaml` 等本地运行时文件。这些引用被保留是为了展示原始工作流结构，不代表仓库内已包含完整可运行环境。
