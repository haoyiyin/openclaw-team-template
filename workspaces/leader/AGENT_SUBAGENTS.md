# Sub-agents Mapping

- researcher
  - path: ~/.openclaw/workspace_researcher
  - invoke: openclaw agent --agent researcher --message "..."
  - role: research and design only

- coder
  - path: ~/.openclaw/workspace_coder
  - invoke: openclaw agent --agent coder --message "..."
  - role: implement approved work and self-verify

- tester
  - path: ~/.openclaw/workspace_tester
  - invoke: openclaw agent --agent tester --message "..."
  - role: E2E testing and verification

- reviewer
  - path: ~/.openclaw/workspace_reviewer
  - invoke: openclaw agent --agent reviewer --message "..."
  - role: skeptical review only

- writer
  - path: ~/.openclaw/workspace_writer
  - invoke: openclaw agent --agent writer --message "..."
  - role: create docs and delivery content

Notes:
- 默认所有本地角色间通讯走 OpenClaw 原生 session/subagent 模型
- specialist 默认不直接互聊，统一经 Leader 编排
- ACP 只给 Coder 显式调用 Claude
- 不使用文件队列协作
- 任何角色不得批准自己的产出
