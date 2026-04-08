# TASK_SCHEMA.md - Task Graph Schema

## Task Fields

- id
- name
- status
- owner
- stage
- type
- producers
- reviewers
- deps
- risks
- artifacts
- done_condition
- blocked_reason
- escalate_to
- handoff_contract
- pending_reviewer_decision
- reviewer_decision

## Status Values

- pending
- running
- blocked
- done
- failed

## Stage Values

- Discover
- Design
- Build
- Test
- Review
- Submit
- Learn

## Handoff Contract Fields

- objective
- current_stage
- expected_output
- constraints
- done_condition
- do_not

## Artifact Fields

- type
- ref
- status
- created_by
- created_at
- submitted_at
- reviewed_at
- reviewed_by
- review_decision
