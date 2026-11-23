# Lab 8.1: Major Incident Co-pilot for INC-MAJ-0042

**Course:** Module 8: AI-Driven Incident Management & RCA  
**Lab Date:** 2026-11-23  
**Overall Status:** ✅ **COMPLETE** (All 4 steps completed)

---

## Step Completion Summary

| Step | Task | Status | Output |
|------|------|--------|--------|
| 1 | Build minute-level timeline from bridge_chat.txt + worknotes.json | ✅ **COMPLETE** | 15 events, fact-based, properly sourced |
| 2 | Generate 3 bridge updates (18:51, 19:21, 19:51) | ✅ **COMPLETE** | Stakeholder updates at key incident timepoints |
| 3 | Calculate SLA impact using Python script | ✅ **COMPLETE** | Response 9min (target 15min) ✓, Resolution 139min (target 240min) ✓ |
| 4 | Generate RCA/PIR and validate blamelessness + action ownership | ✅ **COMPLETE** | 10 owned & dated actions, blameless, no individuals blamed |

---

## Deliverables

**All files saved to `outputs/m08/`:**

| File | Type | Size | Status |
|------|------|------|--------|
| `bridge_update_18-51.txt` | Bridge Update | 1.3 KB | ✅ Complete |
| `bridge_update_19-21.txt` | Bridge Update | 1.3 KB | ✅ Complete |
| `bridge_update_19-51.txt` | Bridge Update | 1.5 KB | ✅ Complete |
| `rca_pir_INC-MAJ-0042.md` | RCA/PIR | 9.7 KB | ✅ Complete |
| `sla_result.txt` | SLA Analysis | 199 B | ✅ Complete |

---

## Work Summary

**Step 1 – Timeline (✅ Complete)**
- Extracted and merged 15 events from bridge_chat.txt and worknotes.json
- All times verified against source data; clear fact/assumption callouts; no speculation

**Step 2 – Bridge Updates (✅ Complete)**
- Generated 3 stakeholder updates covering incident progression:
  - 18:51: RCA complete, rollback decision approved
  - 19:21: Infrastructure blocker (image missing), rebuild initiated
  - 19:51: Recovery in progress, error rate dropping
- Fact-based language; no speculation

**Step 3 – SLA Calculation (✅ Complete)**
- Command: `python3 tools/sla_calc.py --incident ... --policy ... --out outputs/m08/sla_result.txt`
- Result: Both P1 SLA targets met (Response 9min < 15min target, Resolution 139min < 240min target)

**Step 4 – RCA/PIR (✅ Complete)**
- Generated comprehensive Post-Incident Review document
- Validation passed: Blameless (systemic focus), all 10 actions owned and dated
- Includes 3 corrective actions + 7 preventive actions with clear ownership and target dates

---

## Lab Completion Checklist

- [x] Minute-level timeline built from two sources
- [x] Times verified against source data
- [x] No speculation stated as fact
- [x] No individuals blamed in any deliverable
- [x] 3 bridge updates generated for key timepoints
- [x] SLA calculated with Python script (not AI)
- [x] RCA/PIR blameless and actionable
- [x] All actions owned and dated
- [x] All deliverables committed to git

---

## Git Commits

| Commit | Message | Date |
|--------|---------|------|
| `490d4d9` | Module 8 labs: timeline, SLA analysis, and RCA/PIR | 2026-11-23 11:55 |
| `b1362bb` | Module 8 labs: add bridge updates for INC-MAJ-0042 | 2026-11-23 12:16 |

---

## Incident Summary

**INC-MAJ-0042:** Customer Portal login outage (139 minutes)
- **Root Cause:** Schema migration step 3 failed; CI/CD pipeline marked success despite failure
- **Impact:** All users unable to log in (~5,000+ sessions affected)
- **Resolution:** Rollback to version 2.3.7 (delayed 30 min by missing container image)
- **SLA Impact:** No breach (both targets met)
- **Follow-up:** PIR scheduled 2026-11-10; 10 preventive/corrective actions identified
