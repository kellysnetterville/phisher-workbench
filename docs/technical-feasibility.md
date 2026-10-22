# Technical Feasibility

## Purpose

Validate the critical Jira and PhishER integration assumptions before
building the full investigation workflow.

Start with PhishER API discovery to establish which capabilities and data
are actually available before relying on them in the Jira integration.
Record observed behavior rather than treating conceptual dependencies as
confirmed API capabilities.

## Feasibility Spikes

### TF-001: PhishER API Authentication/Connectivity

**Status:** Not started

**Question**

How do we authenticate, establish connectivity, and determine which
endpoints or operations are available to the integration?

**Evidence to collect**

- API base URL, version, and applicable documentation
- Authentication method, required permissions, and credential lifecycle
- Sanitized successful request/response without credentials or tokens
- Available endpoints/operations and any permission or licensing restrictions
- Authentication, authorization, and connectivity failure behavior
- Observed rate limits, throttling signals, and retry guidance

**Result**

TBD

**Conclusion**

TBD

---

### TF-002: PhishER Tag Catalog

**Status:** Not started

**Question**

Can we retrieve the PhishER tag catalog, and what tag information is exposed?

**Evidence to collect**

- Supported tag-listing endpoint/operation, or evidence that none is available
- Sanitized response containing tag identifiers, names, and available metadata
- Pagination behavior and whether all tags can be enumerated
- Permission restrictions, rate limits, and error behavior
- Distinction between catalog tags and tags attached to individual messages

**Result**

TBD

**Conclusion**

TBD

---

### TF-003: PhishER Message Retrieval

**Status:** Not started

**Question**

Can we retrieve a message by ID, and what identifiers, analysis/verdict
information, and tags does the returned object contain?

**Evidence to collect**

- Supported message lookup endpoint/operation and required ID format
- Sanitized request/response for a known message ID
- Returned identifiers and their documented or observed meaning
- PhishER analysis/verdict fields and message-associated tags
- Lookup behavior for unknown, inaccessible, or deleted messages
- Available listing/search operations and their filters and pagination behavior
- Observed rate limits and retry behavior

**Result**

TBD

**Conclusion**

TBD

---

### TF-004: Message/Header Data Availability

**Status:** Not started

**Question**

Are RFC email headers, `Message-ID`, sender, recipient, subject, and
timestamps available through the API?

**Evidence to collect**

- Supported operations for message metadata, headers, or raw source
- Sanitized examples showing available RFC headers and `Message-ID`
- Sender, recipient, subject, and timestamp fields, including formats and semantics
- Distinction between the reported message and any reporting/forwarding wrapper
- Missing, redacted, truncated, or permission-restricted fields
- Capability inventory marking each required field/operation as verified,
  unavailable, or unverified, with supporting evidence and workflow impact

**Result**

TBD

**Conclusion**

TBD

---

### TF-005: Jira → PhishER Correlation Candidates

**Status:** Not started

**Question**

Which identifiers present in Jira can be matched to data actually exposed
by the PhishER API to identify exactly one authoritative message?

**Evidence to collect**

- Sanitized representative Jira content and corresponding PhishER API objects
- Candidate PhishER IDs, message links, `Message-ID`, or other shared identifiers
- Verified lookup/search support for each candidate identifier
- Uniqueness, stability, and missing/ambiguous-match behavior
- Unsupported conceptual dependencies and their impact on correlation feasibility

**Result**

TBD

**Conclusion**

TBD

---

### TF-006: Jira Issue-Created Webhook

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

### TF-007: Jira Issue Retrieval/Parsing

**Status:** Not started

**Question**

Can we retrieve a Jira issue and deterministically parse the fields needed
for intake validation and the correlation candidates identified in TF-005?

**Evidence to collect**

- Authentication method and required Jira API permissions
- Sanitized issue retrieval request/response
- Field mappings for Customer, Components, Description, Message, Tags, and Action
- Actual field representations and parsing rules, including structured Description content
- Extraction of candidate correlation identifiers from representative issues
- Missing/malformed-field behavior, permission errors, and rate limits

**Result**

TBD

**Conclusion**

TBD
