# Decision Log

## ADR-001: Use Jira Issue Creation as the Initial Workflow Trigger

**Status:** Accepted

### Context

PhishER-generated work enters the existing analyst workflow through Jira.
The workbench requires a reliable event that can initiate processing while
preserving the existing operational workflow.

### Decision

Use a Jira issue-created webhook as the initial workflow trigger.

### Rationale

...

### Consequences

...

### Validation

Implementation remains subject to successful Jira webhook feasibility testing.