# ACP_USAGE.md - Coder

ACP is available for Coder when you explicitly want to delegate to the `claude` ACP harness.
It is not the default transport for local role-to-role communication.

## Use ACP when you need
- a Claude ACP harness session for coding assistance
- a long-running Claude coding stream outside normal local role routing

## Preferred behavior

1. Implement first within approved scope.
2. If blocked, escalate clearly to Leader.
3. Use native OpenClaw session routing for Leader / Reviewer communication.
4. Use ACP only for explicit `claude` harness delegation.
