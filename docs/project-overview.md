# PhishER Workbench

## Overview

PhishER Workbench is an analyst-focused project intended to streamline phishing investigation and triage by integrating Jira, PhishER, and supporting security data into a structured investigation workflow.

The project begins with the existing Jira and PhishER workflow and focuses first on proving the underlying integrations, data relationships, and deterministic processing required to reliably identify and normalize reported-email cases. AI-assisted investigation is a planned capability, but it is intentionally introduced only after the conventional integration path has been validated.

The long-term goal is to reduce repetitive analyst effort while improving consistency, evidence traceability, and measurable investigation quality without prematurely introducing autonomous remediation.

## Problem

Reported phishing investigations involve repetitive collection, validation, correlation, analysis, and documentation across multiple systems.

The project explores whether these activities can be progressively streamlined by building a purpose-specific workflow that can:

- Reliably identify eligible phishing cases.
- Correlate Jira work items with their authoritative PhishER messages.
- Normalize relevant message and investigation data.
- Gather supporting evidence from approved sources.
- Apply consistent investigation logic.
- Produce structured, traceable results for analysts.
- Eventually assist analysts with evidence summaries, classifications, and recommended next steps.

The project is intentionally narrow. It is not intended to become a general-purpose SOC agent.

## Current Architecture Direction

The initial workflow is based on Jira issue creation rather than direct PhishER-triggered processing.

A candidate processing path is:

Jira issue created
        |
        v
Webhook received
        |
        v
Deterministic intake validation
        |
        v
Retrieve and parse Jira issue
        |
        v
Validate PhishER context and tags
        |
        v
Correlate Jira ticket to authoritative PhishER message
        |
        v
Normalize and persist investigation data
        |
        v
Evidence collection / investigation
        |
        v
Structured assessment
        |
        v
Evaluation / analyst workflow

Before any future LLM-based investigation occurs, the system must establish through deterministic checks that the Jira issue represents a valid PhishER work item and can be reliably correlated to its source message.

PhishER remains the authoritative source for the reported-email object.

## Design Principles

The project follows several core principles:

### Evidence before assumptions

Important integration and architecture decisions should be based on observed payloads, API behavior, and executable tests rather than documentation or assumptions alone.

### Deterministic controls before probabilistic reasoning

Source validation, schema validation, correlation, authorization, state management, and other control-plane decisions should be implemented using conventional deterministic software.

AI reasoning must not independently determine whether an action is authorized.

### Narrow vertical slice first

The first implementation should prove the smallest useful end-to-end workflow before additional integrations or sophisticated investigation capabilities are added.

### Fail closed

Cases that cannot be reliably validated or correlated should not silently advance through the investigation pipeline.

### Earn automation through evidence

The project begins without autonomous destructive actions. Increased authority should only be introduced after measured performance demonstrates that a specific capability is safe and useful.

### Preserve the analyst as the authority

Initial operational stages are designed to observe and assist the existing analyst workflow rather than replace analyst judgment.

## Current Project Stage

The project has completed substantial problem, workflow, and scope discovery and has established an initial architectural direction.

Current work is transitioning from planning and gap analysis into technical feasibility validation.

The immediate priority is to validate the real Jira and PhishER integration path using representative data and APIs.

Key questions include:

- Can Jira issue-created webhooks be reliably received and deduplicated?
- Can the required Jira fields and Description content be retrieved and parsed deterministically?
- What stable identifier or combination of attributes can correlate a Jira ticket to exactly one PhishER message?
- What PhishER message, tag, header, and related data are available through supported APIs?
- What authentication and permissions are required?
- What failure and retry behavior is required for unavailable or indeterminate dependencies?

These questions should be answered through focused technical spikes rather than additional speculative design.

## Near-Term Technical Plan

### 1. Jira webhook feasibility

Capture and inspect a representative Jira issue-created webhook.

Determine:

- Event and issue identifiers.
- Authentication requirements.
- Delivery and retry behavior.
- Fields useful for deduplication and correlation.

### 2. Jira issue retrieval and parsing

Retrieve the corresponding Jira issue through the API and establish deterministic mappings for the required PhishER workflow fields.

Current observed signals include:

- Customer
- Components
- Description
- Message
- Tags
- Action

Exact parsing behavior should be based on real Jira API responses rather than assumptions about their representation.

### 3. PhishER API discovery

Determine what information can be retrieved from PhishER and how Jira data can be correlated with the authoritative reported message.

Particular attention should be given to identifying stable correlation identifiers and determining whether message headers and related source data can be retrieved programmatically.

### 4. Walking skeleton

After basic feasibility is established, build the smallest real end-to-end path:

1. Receive a Jira issue-created event.
2. Retrieve the Jira issue.
3. Validate and parse the required fields.
4. Create an investigation identifier.
5. Normalize and persist the case.
6. Correlate the case with PhishER.
7. Produce an observable test result.

The initial walking skeleton should not require an LLM.

AI-assisted assessment should be added only after the integration, correlation, state, and observability path is trustworthy.

## MVP Direction

The first meaningful operational version is expected to operate in read-only or shadow mode.

The MVP is intended to demonstrate that the system can:

- Reliably ingest and normalize eligible reported-email cases.
- Correlate Jira tickets with authoritative PhishER messages.
- Retrieve and structure relevant investigation evidence.
- Execute a consistent investigation workflow.
- Produce traceable structured assessments.
- Record sufficient telemetry for evaluation.
- Compare system results with analyst dispositions.

The MVP will not perform destructive actions.

## Out of Scope for Initial MVP

The following capabilities are intentionally deferred:

- Automatic message deletion or quarantine.
- Automatic sender, domain, or URL blocking.
- Tenant-wide search and remediation.
- Account disablement or session revocation.
- Broad autonomous SOC investigation.
- Unrestricted external browsing.
- Fully automated reporter communication.
- Campaign-wide autonomous remediation.

These capabilities may be reconsidered individually if earlier stages demonstrate sufficient value, reliability, and safety.

## Evaluation Strategy

Capability is intended to progress through evidence-driven stages:

1. Technical feasibility
2. Walking skeleton
3. Historical/offline evaluation
4. Live shadow operation
5. Analyst-assisted operation
6. Limited governed automation
7. Production hardening and controlled expansion

Historical evaluation should use representative prior phishing cases before the system is exposed to live workflow variability.

Live shadow operation should allow system and analyst conclusions to be compared without influencing the analyst's original decision.

Only after shadow evaluation demonstrates credible behavior should system-generated evidence and recommendations become visible to analysts.

## Success Measures

Relevant measures may include:

- Classification precision and recall.
- Analyst agreement and disagreement.
- Evidence traceability.
- Unsupported-claim rate.
- Tool and integration failure rate.
- Duplicate-processing rate.
- Processing latency.
- Analyst review and handling time.
- Override and correction reasons.
- Operational reliability.

Exact thresholds should be established using observed pilot data rather than selected prematurely.

## Technology Direction

TypeScript is the proposed primary implementation language for the initial service and feasibility work.

The workload is expected to center on:

- Webhook processing.
- API integration.
- Schema validation.
- Data normalization.
- Workflow state.
- Asynchronous network operations.
- Structured AI interactions.

Bun may be used for local development and technical feasibility work.

The production runtime, hosting platform, deployment architecture, and pipeline remain intentionally undecided. These decisions should be made after the walking skeleton provides evidence about the application's actual runtime and operational requirements.

Python remains an option for isolated evaluation, analytics, data-processing, or machine-learning workloads where its ecosystem provides a material advantage.

## Relationship to SOC Workbench

PhishER Workbench is deliberately being developed as a focused phishing workflow rather than as a general SOC platform.

If the project proves useful, components such as:

- Evidence schemas.
- Integration patterns.
- Tool adapters.
- Investigation state.
- Policy gates.
- Evaluation mechanisms.
- Observability patterns.

may become candidates for reuse within a broader SOC Workbench.

Reuse should emerge from proven implementations rather than requiring PhishER Workbench to conform prematurely to a future platform architecture.

The priority is to make the phishing workflow useful first.

## Current Priority

The project's immediate goal is to move from conceptual architecture to evidence-backed technical discovery.

The next executable work is:

1. Capture a representative Jira issue-created webhook.
2. Retrieve and parse the corresponding Jira issue through the API.
3. Identify the available Jira-to-PhishER correlation data.
4. Test PhishER API access and message retrieval.
5. Record the resulting interface evidence and architecture decisions.
6. Build the conventional, non-LLM walking skeleton.

Further architecture and AI design should be driven by what these tests reveal.