# Agile Team Rules & Governance

## Purpose

This document serves as the centralized rules and templates for all agile roles in the v2-agile system. It ensures consistency, compliance, and effective collaboration across the team.

- **Location**: This file is located at [`rules.md`](rules.md:1) and must be referenced by all agile role definitions.
- **Scope**: Applies to all roles: agile-orchestrator, developer, product-owner, qa-engineer, scrum-master, tech-lead, ux-designer.

## Artifact Storage Guidelines

All project artifacts must be stored in the `.agile` directory for traceability and collaboration.

- **Planning & Design**: `.agile/planning` - sprint plans, roadmaps, feature specifications
- **Discussions & Decisions**: `.agile/discussions` - design debates, RFCs, technical decisions
- **Reviews & Acceptance**: `.agile/reviews` - sprint review notes, acceptance decisions
- **Testing**: `.agile/tests` - test plans, results, automated test manifests
- **Retrospectives**: `.agile/retrospectives` - retro notes, action items, improvement plans

Example paths: [`.agile/planning/feature-spec.md`](.agile/planning/feature-spec.md:1), [`.agile/discussions/architecture-rfc.md`](.agile/discussions/architecture-rfc.md:1)

## Core Rules (Mandatory for All Roles)

1. **Reference Requirement**: Each role's `customInstructions` must include a reference to [`rules.md`](rules.md:1).
2. **Documentation Standard**: All planning, discussion, and review content must be stored as Markdown files under `.agile`.
3. **Handoff Protocol**: All role-to-role handoffs MUST use the [`new_task()`](new_task:1) tool with required fields (see Handoff Template below).
4. **Task Breakdown**: Break complex tasks into logical subtasks before delegating, including estimated time-to-complete for each.
5. **Testing Integrity**:
   - All tests must execute against actual targeted functions or code paths.
   - Fabricated or simulated test results are strictly prohibited.
   - Test reports must include: test input, expected output, actual output, and pass/fail verdict.
6. **Escalation Process**: Report violations or major blockers to [`agile-orchestrator.yaml`](agile-orchestrator.yaml:1) via [`new_task()`](new_task:1).
7. **Role Usage**: Use only agile-specific roles; map built-in roles as follows:
   - Code → Developer
   - Architect → Tech Lead
   - Orchestrator → Agile Orchestrator

## Recommended Folder Structure

```
.agile/
├── planning/          # Sprint plans, roadmaps, feature specs
├── discussions/       # Design debates, decisions, RFCs
├── reviews/           # Sprint review notes, acceptance decisions
├── tests/             # Test plans, results, automated manifests
└── retrospectives/    # Retro notes, action items
```

## Handoff Protocol

### Requirements
- **Tool**: Use [`new_task()`](new_task:1) for all inter-role handoffs.
- **Mandatory Fields**: Each handoff must include:
  - **Mode**: Target agile role (e.g., "developer", "qa-engineer")
  - **Message**: Brief task description for the receiving role
  - **Context**: Summary of completed work and current state
  - **Next Action**: Explicit, actionable task for the receiving role
  - **Artifacts**: Links to relevant `.agile` files or code references
  - **Timeout Estimate**: Reasonable time estimate (e.g., `30m`, `2h`, `1d`)
  - **Acceptance Criteria**: Specific, testable conditions for success

### Handoff Template (Markdown)

```markdown
# Handoff — [Short Title]

**Context:**
- [Summary of what was completed]

**Next Action:**
- [Explicit task for receiving role]

**Artifacts:**
- [Links to .agile files or code references]

**Timeout Estimate:**
- [e.g., 2h]

**Acceptance Criteria:**
- [Specific, testable success conditions]
```

**Example**:
```markdown
# Handoff — User Authentication Design

**Context:**
- Completed initial user research and requirements gathering for login feature.

**Next Action:**
- Create wireframes and mockups for login screen.

**Artifacts:**
- [.agile/planning/login-requirements.md](.agile/planning/login-requirements.md)

**Timeout Estimate:**
- 4h

**Acceptance Criteria:**
- Wireframes approved by product-owner, stored in .agile/discussions
```

## Testing Template

Store under `.agile/tests` with filename: `YYYYMMDD-[short-title]-tests.md`

**Required Fields**:
- **Test Objective**: Purpose of the test
- **Target**: Function/module/endpoint being tested
- **Test Inputs**: Explicit inputs used
- **Expected Outputs**: Predicted results
- **Actual Outputs**: Recorded results from test execution
- **Verdict**: PASS / FAIL
- **Repro Steps** (if failed): Steps to reproduce the issue
- **Test Runner**: Commands used to execute tests

**Example**:
- **Test Objective**: Verify login API handles valid credentials
- **Target**: `/api/auth/login` endpoint
- **Test Inputs**: `{"username": "testuser", "password": "validpass"}`
- **Expected Outputs**: `{"token": "jwt-token", "status": 200}`
- **Actual Outputs**: `{"token": "abc123", "status": 200}`
- **Verdict**: PASS
- **Test Runner**: `curl -X POST /api/auth/login -d '{"username":"testuser","password":"validpass"}'`

## Example `new_task()` Usage

Use [`new_task()`](new_task:1) to delegate subtasks with all required parameters:

```javascript
new_task({
  mode: "developer",
  message: "Implement user authentication API with JWT tokens",
  context: "Completed initial design and requirements gathering",
  next_action: "Write JWT authentication logic and API endpoints",
  artifacts: ".agile/planning/auth-spec.md",
  timeout: "3h",
  acceptance_criteria: "Unit tests pass with 90% coverage and API returns valid tokens"
})
```

## Governance & Ownership

- **Compliance Monitoring**: Each role must reference [`rules.md`](rules.md:1) in their `customInstructions`.
- **Enforcement**: The Agile Orchestrator monitors adherence and may create [`new_task()`](new_task:1) tickets for non-compliance.
- **Updates**: This document should be updated collaboratively through the agile process when rules need refinement.
