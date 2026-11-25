# Presenter summary - DRAFT

**Format:** Eight-minute capstone presentation  
**Scenario:** Offline exercise; correlation reference INC-2026-11-12-001 is
not a verified ITSM incident ID. No production change has been approved or
executed. Human review is required before sharing the materials.

## 0:00-1:00 - Desk lead: Intake and triage

- Nine capstone tickets cover portal login, reporting and other issues.
- The sanitiser masked identified phone and password values and flagged an
  instruction-injection attempt in CAP006. Ticket text is data, not an
  instruction to the assistant.
- CAP008 is a follow-up to CAP001; CAP007 concerns Outlook. Triage and
  routing outputs are proposals requiring human review.
- **AI error caught:** Earlier generated outputs exposed a password and a
  phone number despite claiming sanitisation was complete. The sanitiser was
  corrected, identified values in the Step 12 outputs were redacted, and
  regression tests passed. Human review remains essential.

## 1:00-2:00 - Desk lead: Alert correlation

- Twenty recorded alerts were grouped under one proposed parent incident.
- On 12 November, the scenario reports release at 14:00 IST; HTTP5xxRate
  fired at 14:03:00 and ImagePullBackOff for portal-api at 14:06:23.
- Correlation is a working grouping, not proof that all alerts share a cause.
  Alert firing times alone do not establish recovery or incident duration.

## 2:00-3:30 - Investigator: Evidence and uncertainty

- Login failures and slow reporting are recorded in tickets; alerts report
  5xx errors, web readiness, API image pull, login success and finance CPU.
- **Correction:** Previous claims that a missing image was the confirmed root
  cause, with 95% confidence, and that network causes were ruled out were
  unsupported and have been withdrawn.
- Pod events, image/registry details, deployment history, recovery metrics
  and pipeline evidence are still needed. The static Kubernetes fixture does
  not establish live cluster state.

## 3:30-5:00 - Approver: Proposal and safety gate

- The review draft rejects an immediate API rollback without confirming the
  target, current state and a known-good revision. This is a **scenario**, not
  a recorded human rejection.
- A revised fix cannot be selected until the evidence identifies the fault.
  It must include target, checks, risk, verification and an appropriate tested
  backout.
- The intended workflow requires a named human approval and a second execution
  confirmation. A rollback requires a new approval.
- **Audit status:** No capstone proposal, rejection, approval or execution
  has been recorded. Existing Module 9 records concern another incident and
  must not be presented as this exercise's trail. The approval tool does not
  enforce successful pre-checks or evaluate verification expectations; a
  human must check the actual results.

## 5:00-6:15 - Communicator: Drafts

- Show the customer reply and bridge update, with update times to be agreed.
- Show the preliminary RCA, investigation-only KB and permanent change
  record. Root cause, impact scope, recovery and implementation remain
  unconfirmed.
- All five are drafts; no customer message has been sent and no KB or change
  record has been published.

## 6:15-7:15 - Presenter: Governance and remaining work

1. Human-review the sanitised data; use the credential-exposure process if the
   exposed password is real.
2. Obtain an appropriate incident ID and read-only evidence for the intended
   environment. Establish current service state and root cause.
3. Submit an evidence-backed plan; record an actual human rejection and revised
   proposal, obtain approval and execute only with the required confirmation.
4. Verify outcomes, update the RCA, finalise communications and change record,
   and obtain required publication approvals.

## 7:15-8:00 - Presenter: Demonstration and close

Show `STEP_4_PROPOSAL_REVIEW.md` and the five Step 5 drafts in `outputs/m12/`.
Explain the difference between the rejected draft scenario and a genuine
approval audit trail. Do not claim a completed fix, verified recovery, SLA,
MTTA or MTTR: the required evidence is not available.

**Takeaway:** Human checkpoints, evidence-backed claims and explicit
uncertainty are essential; the remaining approval and verification work must
be completed before presenting the workflow as end-to-end.
