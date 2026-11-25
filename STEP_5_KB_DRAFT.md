# Portal login errors or slow reporting - DRAFT

**Not approved for publication.** This is an investigation aid, not a
confirmed resolution. Review before use and do not request credentials.

## Symptoms (user's words)

- "Portal login fails with error 503" (CAP001).
- "Reports page takes 2 minutes" (CAP002).
- "Still cannot login" (CAP008).

## Applies to

Customer Portal login and reporting issues in this exercise; current scope
must be verified.

## Cause

Unknown. An API image-pull alert was recorded, but its cause and relationship
to the customer symptoms have not been established.

## Resolution steps (L1)

No verified resolution is available. The following read-only investigation
steps are **[VERIFY]** and are not sourced from resolved-ticket notes:

1. Check the current incident and service-status record for an existing
   investigation. [VERIFY]
2. Record the symptom, time and affected function without collecting a
   password or sensitive personal information. [VERIFY]
3. Link potentially related reports for human review; do not automatically
   close or merge them. [VERIFY]
4. Escalate unresolved issues to the service desk lead and the responsible
   portal support team. [VERIFY]

## Escalate when

Login or reporting symptoms are unresolved, multiple users may be affected,
or the issue requires deployment or registry investigation. Confirm the
correct assignment group before routing.

## Keywords

Portal, login, 503, reporting, ImagePullBackOff.

## Source tickets

CAP001, CAP002 and CAP008 in `outputs/m12/tickets_sanitised.csv`; these are
reports, not resolved-ticket resolution notes.
