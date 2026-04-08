# PLAYBOOK.md - Multi-Agent Collaboration Playbook

## Goal

This playbook defines how the six-agent OpenClaw team collaborates using native session/subagent tools.

## Core rules

1. `agentId` determines routing and context isolation.
2. Every agent works in its own session/worktree by default.
3. All specialist communication flows through `leader`.
4. Specialists do not freely message each other.
5. Shared coordination happens through task state, artifact refs, and explicit transcript inspection.
6. `sessions_spawn` completion is push-based — do not poll.
7. ACP is only for explicit `coder -> claude` harness delegation.

## Tool selection

### Use `openclaw agent --agent <role> --message "..."`
Use when you want to hand work to a role from the CLI.

### Use `sessions_send`
Use when you already know the target session and want that role to continue in existing context.

### Use `sessions_spawn`
Use when you need an isolated child task or background subtask.

### Use `sessions_yield`
Use after `sessions_spawn` when you should stop the current turn and wait for completion.

### Use `sessions_history`
Use after completion when Leader needs to inspect the child session transcript.

## Orchestrator-centric routing

Allowed default paths:
- Leader -> Researcher
- Leader -> Coder
- Leader -> Tester
- Leader -> Reviewer
- Leader -> Writer
- Researcher -> Leader
- Coder -> Leader
- Tester -> Leader
- Reviewer -> Leader
- Writer -> Leader

Disallowed default paths:
- Researcher -> Coder
- Researcher -> Tester
- Researcher -> Reviewer
- Coder -> Tester
- Coder -> Writer
- Coder -> Reviewer
- Tester -> Coder
- Tester -> Reviewer
- Writer -> Reviewer
- Any specialist -> any other specialist without Leader mediation

## Common collaboration patterns

### Leader → Researcher
Use when scope, ambiguity, or design direction is unclear.

```text
Objective: Clarify API architecture direction
Stage: Discover
Expected output: architecture_note
Constraints:
- stay at design level
- surface risk and ambiguity early
Done condition: boundaries and decisions are explicit enough for implementation planning
Do not:
- implement handlers or endpoints
```

### Leader → Coder
Use when design is ready and implementation can start.

```text
Objective: Implement the approved login flow
Stage: Build
Expected output: patch + implementation_note + verification_note
Constraints:
- follow approved design
- avoid schema changes
Done condition: feature works and verification evidence is ready
Do not:
- self-approve
- expand scope without escalation
```

### Leader → Reviewer
Use when an artifact is ready for approval.

Reviewer should receive only:
- task objective
- acceptance criteria / constraints
- changed files or diff summary
- relevant artifact refs
- flagged risks

Do not send full session history by default.

```text
Objective: Review the submitted login flow change
Stage: Review
Expected output: review_note + approval_decision
Constraints:
- review only the stated scope
- focus on risks, mismatches, and missing evidence
Done condition: APPROVE / REQUEST_CHANGES / REJECT is explicit
Do not:
- modify implementation directly
- expand review to unrelated history
```

### Leader → Writer
Use when approved implementation or design work needs outward-facing docs.

```text
Objective: Draft release notes for the approved login flow
Stage: Build
Expected output: release_note
Constraints:
- use only approved implementation facts
- target user-facing clarity
Done condition: draft is ready for review
Do not:
- invent unsupported behavior
- self-approve publication
```

### Leader → Tester
Use when E2E testing or verification is needed.

```text
Objective: Run E2E tests for the approved login flow
Stage: E2E_Test
Expected output: test_report + coverage_note
Constraints:
- test only the approved scope
- cover main user flows and boundary cases
- record results objectively
Done condition: test report with clear pass/fail status
Do not:
- self-approve test results
- fabricate test evidence
- modify implementation code
```

## Full workflow example

1. Leader dispatches Researcher for discovery/design.
2. Leader dispatches Coder for implementation and self-verification.
3. Leader dispatches Tester for E2E testing and verification.
4. Leader dispatches Reviewer with a minimal review bundle.
5. Leader dispatches Writer if user-facing docs are needed.
6. Leader aggregates approved outputs and advances Submit/Learn.

## Pitfalls to avoid

- Do not poll after `sessions_spawn`.
- Do not hand implementation work to Researcher.
- Do not hand approval work to Coder, Tester, or Writer.
- Do not send specialists the full backlog if a minimal handoff is enough.
- Do not let specialists negotiate directly when Leader should mediate.

## Quick decision guide

- Need a role from CLI? -> `openclaw agent --agent ...`
- Continue an existing session? -> `sessions_send`
- Start an isolated subtask? -> `sessions_spawn`
- Wait for child completion? -> `sessions_yield`
- Read another session's output? -> `sessions_history`
