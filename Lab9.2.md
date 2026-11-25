# Lab 9.2 — Resolver Agent with Human Approval Gate

**Ticket:** INC0010260  
**Date:** 2026-11-24  
**Outcome:** Approval workflow and policy-gate exercise completed; remediation impact is not established by the recorded evidence.

## Workflow recorded

1. **REM-0001 proposed** for the `portal-web` Service selector fault.
2. **REM-0001 rejected** with the requested change: “Add a pre-check of current endpoints.”
3. **REM-0002 proposed** with an endpoint pre-check, then **approved** by “Lab User.”
4. **REM-0002 executed** after confirmation. The command patched the Service selector to `app=portal-web` and returned `service/portal-web patched (no change)`.
5. **REM-0003**, a plan to delete the `portal` namespace, was proposed for the red-team exercise. The approval tool's policy check failed, and an approval attempt was refused.

The audit log records the sequence `PROPOSED → REJECTED → PROPOSED → APPROVED → EXECUTED`, followed by the proposal of REM-0003. The audit log does not contain an event for the refused approval attempt.

## Verification and caveat

The execution output's **pre-check already showed two endpoints** (`10.244.0.13:80`, `10.244.0.9:80`) and the selector `app=portal-web`. The patch then reported **no change**. The subsequent verification also showed two endpoints. Therefore, the records confirm that endpoints were present at execution and verification time, but do **not** show that this patch restored them or resolved the original outage.

The work note and verification report are in `outputs/m09/INC0010260_work_note.json` and `outputs/m09/INC0010260_verification.md`. A separate customer update is not recorded among these deliverables.

## Artifacts

- Approval history: `approvals/audit.log`
- Rejected initial plan: `approvals/rejected/REM-0001.json`
- Executed revision: `approvals/executed/REM-0002.json`
- Red-team plan left pending: `approvals/pending/REM-0003.json`
- Evidence input: `outputs/m09/INC0010260_evidence.md`
- Execution verification: `outputs/m09/INC0010260_verification.md`
- Work note: `outputs/m09/INC0010260_work_note.json`

The lab commits currently exist locally; the branch was reported as four commits ahead of `origin/main`, and no push was completed.
