# OpenClaw Team Template

English README. For Chinese, see [README.zh-CN.md](./README.zh-CN.md).

A sanitized, open-source template of my current OpenClaw multi-agent workflow.

This repository does **not** contain runtime state, chat history, memory databases, secrets, API keys, bot tokens, device credentials, or local machine data. It is meant to show the **team design**, **role boundaries**, and **workflow protocol** behind the setup.

## Team structure

This template uses a six-role model:

- **Leader** — orchestration, routing, aggregation
- **Researcher** — discovery, research, design, task breakdown
- **Coder** — implementation and self-verification
- **Reviewer** — independent review and approval decisions
- **Writer** — docs, release notes, outward-facing content
- **Tester** — verification, E2E testing, failure analysis

See:

- `docs/team/ROLE_SPEC.md`
- `docs/team/PLAYBOOK.md`
- `docs/team/ARTIFACTS.md`
- `workspaces/leader/WORKFLOW.md`

## Workflow model

Canonical flow:

```text
Discover -> Design -> Build -> Test -> Review -> Submit -> Learn
```

Key ideas:

1. **Leader-centric routing** by default
2. **Specialists do not freely message each other**
3. **Artifacts drive handoffs**
4. **Approval is separated from production**
5. **Each role runs in an isolated workspace/session**

## Repository layout

```text
.
├── docs/team/           # role specs, playbook, artifact schema
├── workspaces/          # per-role startup docs and identities
├── completions/         # shell completions
└── examples/            # sanitized example config and env templates
```

## What was removed from the original local setup

The original local OpenClaw instance contains private operational state that is intentionally excluded here:

- API keys and tokens
- bot credentials
- device keys and pairing data
- local memory databases
- session transcripts and logs
- cron/task runtime state
- build artifacts and caches
- local application `.env` files

See `.gitignore` for the publish-time denylist.

## Adapting this template

1. Copy `examples/openclaw.example.json` to your own local config file.
2. Fill in your own providers, tokens, and channel settings.
3. Create your own runtime-only files (`.env`, credentials, memory, logs, session storage) locally.
4. Keep the public repo limited to templates, docs, and non-sensitive scripts.

## Recommended publishing rule

Treat this repository as a **template/spec repo**, not a mirror of a live agent runtime directory.

That means:

- publish docs, role contracts, prompts, and safe helper scripts
- do **not** publish sessions, memory, logs, device identity, or credentials

## Notes

Some copied role docs still reference local runtime files such as `SOUL.md`, `USER.md`, or `WORKFLOW_STATE.yaml`. Those are part of the original private runtime and are left as references to illustrate the workflow shape, not as complete runnable public assets.
