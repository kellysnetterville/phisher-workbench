# Technical Feasibility

## Purpose

Validate the critical Jira and PhishER integration assumptions before
building the full investigation workflow.

## Feasibility Spikes

### TF-001: Jira Issue-Created Webhook

**Status:** Not started

**Question**

Can PhishER Workbench reliably receive Jira issue-created events and
identify the information required to retrieve and process the issue?

**Evidence to collect**

- Sanitized webhook payload
- Event identifier
- Issue identifier/key
- Authentication method
- Delivery/retry behavior
- Candidate deduplication fields

**Result**

TBD

**Conclusion**

TBD

---

### TF-002: Jira Issue Retrieval and Parsing

**Status:** Not started

### TF-003:  PhishER authentication / API access

**Status:** Not started

### TF-004:  PhishER tag catalog

**Status:** Not started

### TF-005:  Jira → PhishER message correlation

**Status:** Not started

### TF-006:  PhishER message/header retrieval

**Status:** Not started
