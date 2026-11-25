# Step 4: Remediation proposal review - blocked pending evidence

Parent incident: INC-2026-11-12-001 (correlation identifier, not a verified
ITSM incident number).

## Evidence correction

The earlier Step 2 and Step 3 documents overstate the evidence. The capstone
brief says the release went live at 14:00 IST. The alerts file records
`HTTP5xxRate` on `nginx-edge-01` at 14:03:00 IST and
`KubePodImagePullBackOff` on `portal-api` at 14:06:23 IST. These are alert
observations, not proof of causation or the image-pull error's underlying
reason. No registry, pipeline, live Kubernetes or recovery evidence has been
verified. A missing image, configuration error and registry connectivity
problem remain possibilities; none is confirmed or ruled out. Prior numeric
confidence estimates are withdrawn.

The supplied static `k8s/broken/portal.yaml` is a lab fixture, not evidence of
the current running cluster or its rollout history. Its portal-api image is
`nginx:1.27-alpine`; its Service selector matches the web template label.

## Rejected proposal scenario - review draft, not an audit event

**Candidate:** Immediately roll back `portal-api` to the previous Deployment
revision without checking the current deployment, revision history, error or
rollback target.

**Review outcome:** Reject this draft: the current state and a known-good
revision are not verified; a rollback could affect the wrong workload or
revision. The evidence does not establish that the image-pull alert caused
the portal symptoms. This is a proposed rejection scenario, not a human
rejection recorded by `tools/approve.py`.

## Revised proposal - not yet ready for submission or approval

Consider a targeted rollback of `portal-api` only if read-only checks establish
that the affected workload is in the intended environment, the current rollout
is faulty, and a known-good revision is available. If those checks do not
support rollback, develop a different plan from the actual failure evidence.
The eventual plan must specify the exact command and target, pre-checks,
blast radius, risk, rollback and explicit verification criteria. The named
human approver must review it and execution requires a second confirmation.

Collect first:

1. The target environment and live `portal-api` Deployment, pod status, events,
   exact image-pull error and rollout history, using read-only tools.
2. Registry/image availability and authorization evidence, without exposing
   credentials; release and deployment records, including a known-good revision.
3. Service endpoints, customer-impact metrics and current recovery status.
4. A valid ITSM incident identifier: the remediation schema accepts
   `INC` followed by seven digits or `INC-MAJ-` followed by four digits,
   but not this correlation identifier.

## Approval gate limitations

No capstone proposal, rejection, approval or execution was recorded. Existing
Module 9 approval records are for a different incident and are not this
scenario. The resolver's `propose_remediation` tool was not available in this
session. `tools/approve.py` checks command policy, but its execution path does
not stop on failed pre-checks or automatically compare verification output with
the stated expectations; a human must evaluate those results, and rollback
requires a new approval.

The originally sanitised ticket output and several Step 12 summaries exposed
a password value and/or phone number. The identified values were redacted
and the ticket output regenerated; human review is still required before use.
If the password is real, follow the organisation's credential exposure response.

**Status:** Awaiting environment/incident details and read-only evidence. No
production action has been performed or approved.
