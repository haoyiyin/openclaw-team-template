# OpenClaw Team Template

[中文说明](./README.zh-CN.md)

A sanitized, open-source template of my current OpenClaw multi-agent workflow.

This repository is **not** a dump of a live runtime directory. It is a **public template/spec repo** that captures the architecture, role boundaries, routing rules, and workflow conventions behind the system, while intentionally excluding secrets and private operational state.

## Why this repo exists

Most "multi-agent" demos focus on model orchestration, but the harder problem is usually **team design**:

- who is allowed to do what
- how work gets handed off
- how review stays independent
- how context is isolated
- how delivery moves through explicit gates

This repo packages those decisions into a reusable template.

## Core ideas

This OpenClaw setup is built around five principles:

1. **Leader-centric orchestration**  
   Specialists do not freely coordinate with each other. The Leader routes work, resolves ambiguity, and aggregates outcomes.

2. **Strict role boundaries**  
   Researcher researches, Coder implements, Reviewer reviews, Writer documents, Tester verifies. Approval is kept separate from production.

3. **Artifact-driven handoffs**  
   Work is passed through explicit deliverables such as `design_spec`, `patch`, `test_report`, and `approval_decision` rather than informal chat.

4. **Stage-gated workflow**  
   Delivery follows a canonical pipeline: `Discover -> Design -> Build -> Test -> Review -> Submit -> Learn`.

5. **Isolated workspaces and sessions**  
   Each role operates in its own workspace/session by default, with explicit cross-session access instead of implicit shared context.

## Team model

This template uses a six-role team:

- **Leader** — orchestration, routing, aggregation, escalation handling
- **Researcher** — discovery, research, design, task decomposition
- **Coder** — implementation and self-verification
- **Reviewer** — independent review, approval decisions, risk checks
- **Writer** — docs, release notes, outward-facing content
- **Tester** — E2E verification, coverage, failure analysis

Key references:

- `docs/team/ROLE_SPEC.md`
- `docs/team/PLAYBOOK.md`
- `docs/team/ARTIFACTS.md`
- `docs/team/TASK_SCHEMA.md`
- `workspaces/leader/WORKFLOW.md`

## Workflow model

Canonical flow:

```text
Discover -> Design -> Build -> Test -> Review -> Submit -> Learn
```

Typical path:

1. Leader dispatches Researcher for discovery or design clarification.
2. Leader dispatches Coder once the scope is implementation-ready.
3. Tester verifies the result and records evidence.
4. Reviewer receives a minimal review bundle and returns an explicit decision.
5. Writer produces user-facing material when needed.
6. Leader aggregates approved outputs and advances the task.

## Repository layout

```text
.
├── docs/team/           # role specs, collaboration rules, artifact schema
├── workspaces/          # per-role startup docs, identities, usage notes
├── completions/         # shell completions
└── examples/            # sanitized example config and env templates
```

## What is intentionally excluded

The original local OpenClaw runtime contains private operational state that is **not** published here, including:

- API keys and access tokens
- bot credentials
- device identity and pairing data
- local memory databases
- session transcripts and logs
- cron/task runtime state
- local `.env` files
- databases, caches, and build artifacts

See `.gitignore` for the publication denylist.

## How to adapt this template

1. Copy `examples/openclaw.example.json` into your own local config.
2. Replace placeholders with your own providers, tokens, and channel settings.
3. Create your own runtime-only files locally (`.env`, credentials, memory, logs, session storage, device identity, etc.).
4. Keep your public repository limited to templates, contracts, docs, and safe helper scripts.

## Publishing rule

Treat this repository as a **template/spec repo**, not a mirror of a live agent runtime.

Publish:

- role contracts
- workflow docs
- prompts and conventions
- sanitized examples
- non-sensitive helper scripts

Do not publish:

- sessions
- memory
- logs
- credentials
- device identity
- runtime databases
- real production config

## Notes

Some copied role docs still reference private runtime-only files such as `SOUL.md`, `USER.md`, or `WORKFLOW_STATE.yaml`. Those references are preserved to show the original workflow shape, but they are **not required public assets** and are intentionally not included here.

## License

This repository is released under the MIT License. See [LICENSE](./LICENSE).
