# STATE_TRANSITIONS.md - State Transition Rules

## Task Status Transitions

- pending -> running
- running -> blocked
- running -> done
- running -> failed
- blocked -> pending
- failed -> pending

## Artifact Status Transitions

- draft -> submitted
- submitted -> approved
- submitted -> rejected
- rejected -> draft

## Stage Progression

Discover -> Design -> Build -> Test -> Review -> Submit -> Learn

## Stage Ownership

- Discover: Researcher
- Design: Researcher
- Build: Coder or Writer
- Test: Coder self-verification
- Review: Reviewer
- Submit: Leader
- Learn: Leader

## Reviewer Decision Flow

- submitted artifact enters Review
- reviewer returns APPROVE / REQUEST_CHANGES / REJECT
- self-review is invalid
- verification evidence informs review, not replaces it
- reviewer should assess a minimal handoff bundle rather than the full backlog by default

## Artifact-Driven Task Flow

- when any artifact becomes submitted, task sets pending_reviewer_decision = true
- when all task artifacts are approved, task becomes done
- when any artifact is rejected, task returns to pending for rework
