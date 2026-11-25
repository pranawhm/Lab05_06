# Permanent fix change record - DRAFT / NOT READY FOR APPROVAL

**Related correlation reference:** INC-2026-11-12-001 (not a verified ITSM ID)  
**Change ID, owner, environment and window:** TBD  
**Status:** No permanent fix defined, approved, scheduled or executed.

## What and why

Identify and correct the confirmed cause of the portal issue, then introduce
appropriate regression checks. The nature of the change and its affected
components remain unknown until the evidence is reviewed. Do not select an
image, deployment, registry setting or command based on the alerts alone.

## Prerequisites and design

- Obtain a valid incident ID and confirm target environment, current service
  state, affected workload and owner.
- Review pod events, image and registry evidence, deployment history and
  pipeline results; document the confirmed cause.
- Specify the proposed permanent change, exact artefacts and commands, intended
  revision and blast radius. Establish dependencies and customer notice.
- Test the fix and backout in an appropriate non-production environment.
- Submit a complete plan through the approval workflow for a named human
  decision; production execution requires approval and a second confirmation.

## Implementation and verification

**Implementation:** TBD after root cause and fix are established.  
**Pre-checks:** Confirm target, baseline health, current configuration and
backup/known-good version; agree measurable go/no-go thresholds.  
**Verification:** Confirm rollout and readiness, service endpoints, successful
login and reporting, error rate and business impact against agreed baselines
and observation window. Exact thresholds, test account and owner are TBD.
Record actual results; a command exit code alone is not proof of recovery.

## Backout

**TBD.** Define and test an exact reversal for the actual change and capture
the prior known-good state before approval. A rollback must receive its own
approval under the current approval workflow; do not assume undoing a
Deployment reverses changes to a Service or registry.

## Risk and CAB

Risk score and approval authority are **TBD** under
`data/changes/change_risk_rubric.md`: impact, complexity, change history,
backout and timing cannot be scored responsibly until the plan and window are
known. Obtain InfraSec sign-off if an internet-exposed rule is changed.

## Communication and go/no-go

Confirm the change window, impact, customer notice, bridge owner and next
update time. Do not go ahead until evidence, target, tested backout, measurable
verification criteria and required approvals are in place.
