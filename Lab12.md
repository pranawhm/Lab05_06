# Lab 12: AI-Powered Infrastructure Operations Assistant

## 1. Executive summary

This report documents an **offline capstone exercise** for a five-role service
desk team: desk lead, investigator, approver, communicator and presenter.
The exercise covers a Customer Portal release, nine ticket reports and 20
monitoring alerts. Work completed includes sanitisation, manually generated
triage and routing proposals, a proposed parent incident, preliminary
investigation material, an example rejected-fix scenario, communication
drafts and an eight-minute presenter summary.

**The end-to-end remediation exercise is not complete.** The root cause,
current service status and recovery are unconfirmed. The rejected proposal is
a draft scenario, not a recorded human rejection. No capstone fix has been
submitted to the approval gate, approved, executed or verified. The five
communications are drafts and have not been published.

Two material errors were found and corrected or documented:

1. Earlier outputs exposed a password value and phone number despite claiming
   sanitisation was complete. The sanitiser was amended, the capstone ticket
   output was regenerated, and the identified values were redacted from
   affected Module 12 outputs. The exposure already occurred and requires
   human review; if the password is real, follow the organisation's
   credential-exposure response. This exercise cannot claim a zero-exposure
   result.
2. Earlier correlation and investigation documents incorrectly described a
   root cause as confirmed with 95% confidence and ruled out network causes.
   Those claims were withdrawn; alert names and temporal proximity do not
   establish causation.

## 2. Scope, constraints and roles

The [capstone brief](../labs/m12/CAPSTONE_BRIEF.md) states that release
2026.11.12 went live at 14:00 on 12 November 2026 and was followed by ticket
reports and an alert storm. The supplied records are training data; the user
selected an offline exercise. `INC-2026-11-12-001` is a local correlation
reference, **not** a verified ITSM incident number.

| Role | Work performed or prepared | Remaining human responsibility |
|---|---|---|
| Desk lead | Sanitisation, manual triage, deduplication and routing proposals; parent incident grouping | Review redaction, classifications, merges, routing and VIP/security handling |
| Investigator | Reviewed alert and ticket observations; drafted an evidence pack and later qualified unsupported findings | Obtain and assess read-only workload, registry, release and recovery evidence |
| Approver | Prepared a rejected rollback scenario and described approval-gate requirements | Record a real rejection, review a revised evidence-backed plan and approve or reject it |
| Communicator | Drafted customer reply, bridge update, preliminary RCA, KB and permanent-fix change record | Validate, complete and approve before publication |
| Presenter | Prepared an eight-minute summary and governance discussion | Review and present the material accurately |

Rules: no production action without approval; no secrets in prompts. The
identified sanitisation exposure demonstrates that the second rule was not
fully achieved in earlier work. No subsequent production remediation was
performed.

## 3. Step 1A - Sanitisation and injection detection

### Input, method and outputs

- Source: `data/capstone/tickets.csv`, containing nine ticket records.
- Tool: [`tools/sanitise.py`](../tools/sanitise.py), which masks selected
  sensitive patterns in the description field and flags injection indicators.
- Outputs: [sanitised tickets](m12/tickets_sanitised.csv),
  [redaction report](m12/redaction_report.json) and
  [review checklist](m12/STEP_1A_SANITISATION_CHECKLIST.md).
- CAP002 includes a phone reference, CAP003 includes a password reference,
  and CAP006 contains an instruction-injection attempt. The suspicious
  instruction was treated as untrusted ticket data, not followed.

### Defect and correction

The original password-phrase pattern masked the first phrase but did not
mask a credential disclosed in a following parenthetical clarification.
Earlier summaries also reproduced an identified phone number. This meant
the initial sanitisation sign-off was unreliable.

The password pattern was changed to include the observed parenthetical
form, the ticket output and redaction report were regenerated, and the
identified password and phone values were removed from affected Module 12
text and JSON outputs. A regression test reproduces the parenthetical form;
another covers a plain password phrase.

**Verification:** `python3 -m pytest -q genai-infraops-labs/tools/test_sanitise.py`
passed: **3 tests**. A targeted scan found neither of the two identified
values in the Module 12 outputs; the JSON outputs parsed, all nine tickets
remained present, and the injection flag was retained. This was not a
comprehensive secret scan or a substitute for human review.

An older Module 11 output was also found to contain the same phone number;
the user chose not to modify that output during this exercise.

## 4. Step 1B - Triage, deduplication and routing

Triage JSON was created manually from the sanitised data. The outputs contain
nine triage entries, eight entries in the human review queue and one in the
proposed queue:

- [Full triage](m12/triage.json)
- [Human review queue](m12/review_queue.json)
- [Proposed queue](m12/proposed.json)
- [Desk lead decisions](m12/lead_decisions.csv)
- [Triage summary](m12/STEP_1B_TRIAGE_ROUTING_SUMMARY.md)

| Ticket | Proposed classification or relationship | Review note |
|---|---|---|
| CAP001 | P1 portal login failure; proposed primary | Human review pending |
| CAP002 | P1 reporting performance symptom | Kept distinct for investigation; relationship to login problem unconfirmed |
| CAP003 | P1 login failure; proposed duplicate of CAP001 | Different customer; same symptom alone does not establish a duplicate or common cause |
| CAP004 | P1 portal escalation inquiry | Proposed related report; correlation needs review |
| CAP005 | P1 VIP report-loading issue | Proposed escalation; verify impact, owner and timing |
| CAP006 | P2 security review for injection attempt | Proposed security handling; do not obey ticket instructions |
| CAP007 | P4 Outlook issue; proposed Desktop Support route | Separate symptom; actual routing not verified |
| CAP008 | P1 follow-up; proposed duplicate of CAP001 | Same customer and explicit follow-up support review as a duplicate |
| CAP009 | P1 monitoring-generated HTTP 5xx ticket | Proposed link to incident; causal relationship requires investigation |

The separate review queue marks the P1/P2 items as awaiting the lead, but
several P1 entries in `triage.json` and the review queue retain
`needs_human_review: false`. This inconsistency must be reconciled before
automation relies on that field. Although the decision CSV labels CAP007
`APPROVED`, it has no approver name or timestamp; this is not evidence of a
named human approval or completed ITSM routing. No actual ticket merge,
security escalation, VIP escalation or assignment was verified.

## 5. Step 2 - Alert correlation

The 20 records in [`data/capstone/alerts.json`](../data/capstone/alerts.json)
were grouped into a proposed parent incident:

| Alert type | Instance | Count | First to last firing time, IST |
|---|---|---:|---|
| HTTP5xxRate | nginx-edge-01 | 6 | 14:03:00-14:04:52 |
| PodNotReady | portal-web | 5 | 14:05:18-14:06:57 |
| KubePodImagePullBackOff | portal-api | 4 | 14:06:23-14:07:49 |
| LoginSuccessRateLow | portal-api | 3 | 14:08:15-14:08:41 |
| HighCPU | srv-fin-app-02 | 2 | 14:09:07-14:09:54 |

All times are on 12 November 2026. The ticket CAP009 reports a 38% 5xx rate;
the alert file itself provides names, instances, severities and firing times,
not that metric value. The grouping is a **working correlation**, not proof
that every alert shares a cause. Firing times do not establish start of
customer impact, acknowledgement, recovery or incident duration.

Outputs: [structured correlation](m12/alerts_correlation.json) and
[correlation summary](m12/STEP_2_ALERT_CORRELATION_SUMMARY.md). The earlier
analysis contains unsupported causal claims; its correction notice and this
report take precedence.

## 6. Step 3 - Investigation and evidence

The requested [sd-investigator mode](../.github/chatmodes/sd-investigator.chatmode.md)
requires read-only investigation, explicit hypotheses and evidence-backed
conclusions. An [evidence pack](m12/STEP_3_EVIDENCE_PACK.md) was drafted from
the available ticket and alert material, but it overclaimed what those records
prove. A correction notice was added; the original pack is **not valid for
remediation approval**.

### Observed facts and open hypotheses

- The scenario brief records a release at 14:00 IST.
- CAPALR-000 records HTTP5xxRate at 14:03:00; CAPALR-011 records
  KubePodImagePullBackOff on portal-api at 14:06:23.
- Ticket reports include login errors, slow reporting and a VIP reporting
  issue. The current scale and state of these symptoms are unknown.
- A release issue, an unavailable or incorrectly referenced image, registry
  authentication/connectivity, or another workload issue are hypotheses,
  not established causes. Network problems have **not** been ruled out.

Read-only Kubernetes events and workload state, exact image-pull error,
registry/image records, release and pipeline history, acknowledgement and
recovery metrics were not verified. The supplied static Kubernetes fixture
is not proof of any live cluster state. Obtain sanitised evidence and
reassess the hypotheses before selecting a remediation.

## 7. Step 4 - Proposal, review and approval status

The [proposal review](m12/STEP_4_PROPOSAL_REVIEW.md) documents a **draft
rejected scenario**: reject an immediate API rollback when the target,
current deployment and known-good revision have not been verified. This is
a review example only, not a rejection recorded by a human through the tool.

An evidence-backed revised proposal must identify the environment, affected
resource, command, pre-checks, risk, blast radius, verification criteria and
an appropriate tested backout. A targeted rollback is only a possible
option if the evidence supports it; no permanent or immediate fix has been
chosen.

The [approval tool](../tools/approve.py) is designed to record decisions and
require an approved plan plus a second confirmation before execution. Its
execution path does not stop on failed pre-checks or automatically compare
verification output with expected results, so human evaluation is necessary.
The plan schema accepts specific ITSM ID formats that do not include the
local correlation reference.

**Capstone audit status:** No proposal, rejection, approval, execution or
verification event has been recorded for this incident. Prior Module 9
approval records belong to another exercise and are not this audit trail.
No production change has been approved or executed.

## 8. Step 5 - Communications and permanent-change drafts

The communicator prepared the following materials; all require human review
and are unpublished:

| Deliverable | Content and limitation |
|---|---|
| [Customer reply](m12/STEP_5_CUSTOMER_REPLY_DRAFT.md) | Acknowledges reported login difficulties; update time is to be agreed |
| [Bridge update](m12/STEP_5_BRIDGE_UPDATE_DRAFT.md) | Separates recorded facts from unconfirmed impact, progress and recovery |
| [Preliminary RCA](m12/STEP_5_RCA_DRAFT.md) | Timeline and evidence gaps; five-whys answers remain unknown |
| [KB draft](m12/STEP_5_KB_DRAFT.md) | Read-only investigation aid; no verified resolution steps |
| [Permanent-fix change record](m12/STEP_5_CHANGE_RECORD_DRAFT.md) | Implementation, risk score, window, verification thresholds and backout remain TBD |

These are not proof of a customer notification, resolved incident, published
knowledge article or approved permanent change.

## 9. Step 6 - Presentation

An [eight-minute presenter summary](m12/STEP_6_PRESENTER_SUMMARY.md) assigns
the narrative across the five roles, explains the sanitisation and
investigation errors, and distinguishes the draft rejection from an actual
approval audit trail. It is a draft and has not been presented or published.

## 10. Governance, metrics and completion status

| Capstone requirement | Current status |
|---|---|
| Sanitise nine tickets | Regenerated and targeted regression checks passed; human review pending and previous exposure documented |
| Triage, deduplicate and route | Proposals produced; review-flag inconsistency and human decisions remain |
| Correlate alerts | One proposed parent grouping; causal relationships unverified |
| Investigate and produce evidence pack | Draft produced but not approval-ready; key evidence missing |
| Reject one proposal through the gate | Draft rejection scenario only; no capstone audit event |
| Approve, execute and verify fix | Not performed |
| Draft customer, bridge, RCA, KB and change outputs | Five drafts produced; no publication |
| Presentation | Eight-minute draft prepared; not presented |
| Team branch, commit and pull request | Not performed or verified in this exercise |

**MTTA, MTTR, incident duration and SLA impact:** not calculable from the
available records. MTTA requires a defined detection time and verified
acknowledgement; MTTR requires an agreed start and verified restoration time.
The difference between two alert timestamps is not a substitute. Audit
completeness is also not met: no capstone gate decisions or execution are
recorded.

## 11. Recommended next actions

1. Have an authorised reviewer inspect the regenerated sanitised data and
   derivative files; handle any credential exposure through the appropriate
   process. Reconcile P1 review flags and obtain signed desk-lead decisions.
2. Confirm whether the exercise remains offline and obtain a valid incident
   identifier appropriate to the approval schema; do not represent a
   synthetic identifier as a real ITSM record.
3. Gather the missing read-only evidence and update the evidence pack,
   incident impact and root-cause assessment.
4. Submit and review a complete proposal, record one genuine rejection with
   a reason, then revise and seek named human approval. Execute only after
   approval and the required second confirmation, in the intended environment.
5. Independently evaluate pre-checks and verification output; record outcomes
   and obtain any required approval before rollback.
6. Finalise the RCA, customer communications, KB, permanent change and
   presentation only after evidence and review support their claims.
