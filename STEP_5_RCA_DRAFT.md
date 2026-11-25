# Preliminary RCA / post-incident review - DRAFT

**Correlation reference:** INC-2026-11-12-001 (not a verified ITSM ID)  
**Status:** Investigation incomplete; human review required. No root cause,
recovery time, or permanent fix has been confirmed.

## Summary

Following a portal release, the exercise records login and performance
complaints and multiple alerts. Their proximity supports investigation of a
possible common incident, but does not establish a causal chain.

## Impact and SLA

CAP001, CAP003 and CAP008 report login failures; CAP002 reports slow
reporting and CAP005 reports a board report not loading. The number of
affected users, outage duration, recovery time and SLA impact are unknown.
No SLA result was supplied; do not calculate one from the alert timestamps.

## Timeline (12 November 2026, IST)

| Time | Recorded event | Source |
|---|---|---|
| 14:00 | Portal release went live | `labs/m12/CAPSTONE_BRIEF.md` |
| 14:03:00 | HTTP5xxRate alert fired on nginx-edge-01 | `data/capstone/alerts.json`, CAPALR-000 |
| 14:05 | CAP001 reports login failure since this time | `outputs/m12/tickets_sanitised.csv`, CAP001 |
| 14:05:18 | PodNotReady alert fired on portal-web | `data/capstone/alerts.json`, CAPALR-006 |
| 14:06:23 | ImagePullBackOff alert fired on portal-api | `data/capstone/alerts.json`, CAPALR-011 |
| 14:08:15 | LoginSuccessRateLow alert fired on portal-api | `data/capstone/alerts.json`, CAPALR-015 |
| 14:09:54 | HighCPU alert fired on srv-fin-app-02 | `data/capstone/alerts.json`, CAPALR-018 |

These are firing times, not proof of onset, duration, recovery or causality.

## Detection

The first recorded alert occurred three minutes after the time given for the
release. Whether it was the earliest actual symptom or when anyone
acknowledged it is unknown.

## Five whys - incomplete, blameless

1. Why did users report login errors? **Unknown:** service and request evidence
   are needed.
2. Why was the API image-pull alert raised? **Unknown:** pod event details and
   registry access evidence are needed.
3. Why did any pull error occur? **Unknown:** image availability,
   configuration, credentials and connectivity must be checked.
4. Why was the failure not prevented or detected earlier? **Unknown:** build,
   deployment and monitoring records are needed.
5. Why did any relevant process gap exist? **Unknown:** investigate after
   identifying a supported contributing factor.

## Contributing factors

None established. **Hypotheses:** release-related configuration or image
availability; registry access or connectivity. These may coexist or be
unrelated to the reported login issues.

## Corrective and preventive actions

| Action | Type | Proposed owner team | Due date |
|---|---|---|---|
| Collect pod events, rollout history and service health evidence | CA | Platform Engineering | TBD |
| Check image availability and release pipeline records | CA | Release Engineering | TBD |
| Define a permanent fix and regression checks after establishing cause | PA | Platform and Release Engineering | TBD |
| Review redaction outputs and credential-exposure response | CA | Security / Service Desk | TBD |

All owners and due dates require confirmation. No action is asserted complete.

## What went well

The exercise contains timestamped alerts and ticket reports for review. Actual
response effectiveness and acknowledgements have not been verified.

## Evidence gaps

Live deployment and pod events, registry errors, pipeline history, impact and
recovery metrics, acknowledgement times, and a valid ITSM incident ID remain
unavailable. The earlier evidence pack's root-cause claims are withdrawn; see
`STEP_4_PROPOSAL_REVIEW.md`.
