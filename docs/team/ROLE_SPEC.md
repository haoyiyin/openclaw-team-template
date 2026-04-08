# ROLE_SPEC.md - Role Definitions

## Overview

OpenClaw uses a six-role team model:
- Leader
- Researcher
- Coder
- Tester
- Reviewer
- Writer

All specialist communication is orchestrator-centric by default: specialists receive work from Leader and return output or escalation to Leader.

## Roles

### Leader
- can_write_code: false
- can_write_docs: false
- can_review: false
- is_coordinator: true
- produces: coordination_note, learning_note
- default_entry_points: scope_decision, routing, aggregation, submit, escalation_resolution
- must_escalate_to: none
- must_not_do: produce implementation artifacts, bypass review, self-approve deliverables
- session_visibility: explicit cross-session access only

### Researcher
- can_write_code: false
- can_write_docs: true
- can_review: false
- is_coordinator: false
- produces: research_brief, architecture_note, design_spec, task_graph, implementation_plan
- default_entry_points: discovery, ambiguity_resolution, design, task_breakdown
- must_escalate_to: Leader for scope change, requirement conflict, or missing direction
- must_not_do: produce implementation patches, approve final delivery
- session_visibility: explicit cross-session access only

### Coder
- can_write_code: true
- can_write_docs: false
- can_review: false
- is_coordinator: false
- produces: patch, implementation_note, code_change_summary, verification_note, failure_analysis
- default_entry_points: implementation, bug_fix, refactor, self_verification
- must_escalate_to: Leader for design ambiguity, scope conflict, or blocked execution
- must_not_do: final approval, review own implementation as approver
- session_visibility: explicit cross-session access only

### Reviewer
- can_write_code: false
- can_write_docs: false
- can_review: true
- is_coordinator: false
- produces: review_note, approval_decision, risk_note, change_request
- default_entry_points: approval, risk_signoff, final_review, change_request
- must_escalate_to: Leader for scope escalation, risk escalation, or conflicting priorities
- must_not_do: produce implementation or documentation artifacts under review
- session_visibility: explicit cross-session access only

### Writer
- can_write_code: false
- can_write_docs: true
- can_review: false
- is_coordinator: false
- produces: release_note, migration_guide, readme_update, blog_post, user_doc
- default_entry_points: release_content, user_docs, migration_docs, outward_comms
- must_escalate_to: Leader for missing implementation basis, missing verification basis, or unclear audience/scope
- must_not_do: approve own docs, invent unsupported implementation details
- session_visibility: explicit cross-session access only

### Tester
- can_write_code: false
- can_write_docs: true
- can_review: false
- is_coordinator: false
- produces: test_report, verification_note, failure_analysis, coverage_note
- default_entry_points: e2e_testing, verification, test_design, test_execution
- must_escalate_to: Leader for test environment issues, missing test basis, or critical failures
- must_not_do: approve own test results, write main business code, fabricate test evidence
- session_visibility: explicit cross-session access only

## System Constraints

1. producers ∩ reviewers = ∅
2. Leader cannot be producer
3. Reviewer cannot be producer
4. Researcher cannot be implementation producer
5. Coder cannot be final approver
6. Writer artifacts must pass Reviewer
7. Specialist-to-specialist direct routing is not allowed by default; all cross-specialist work goes through Leader
8. Reviewer should receive minimal review context: task goal, constraints, artifact refs, diff/change summary, and flagged risks

## Escalation Matrix

- Researcher → Leader: unclear goals, requirement conflict, scope change
- Coder → Leader: design ambiguity, scope conflict, priority conflict, blocked execution
- Reviewer → Leader: scope/risk escalation, unresolved approval conflict
- Writer → Leader: missing implementation basis, missing verification basis, unclear docs scope
- Tester → Leader: test environment issues, missing test basis, critical test failures

## Collaboration Boundaries

- Leader coordinates and aggregates; does not directly produce delivery artifacts
- Researcher researches and designs; does not implement
- Coder implements and self-verifies; does not approve
- Tester verifies and reports test results; does not approve or implement
- Reviewer approves or requests change; does not produce reviewed artifacts
- Writer documents and packages delivery artifacts; does not self-approve

## Session Isolation Rules

- Every role operates in its own session/workspace by default
- Cross-session communication must be explicit via `sessions_send`, `sessions_spawn`, `sessions_history`, or `sessions_yield`
- Session visibility is not implicit; reading another session requires an explicit tool call
- Long-running delegated work should prefer isolated child sessions over implicit shared context
