# ACP_USAGE.md - Tester

ACP is available for Tester when you explicitly want to delegate to the `claude` ACP harness.
It is not the default transport for local role-to-role communication.

## Use ACP when you need
- a Claude ACP harness session for test automation assistance
- a long-running Claude test execution stream outside normal local role routing

## Preferred behavior

1. Execute tests within approved scope first.
2. If blocked by environment or access issues, escalate clearly to Leader.
3. Use native OpenClaw session routing for Leader / Reviewer communication.
4. Use ACP only for explicit `claude` harness delegation.