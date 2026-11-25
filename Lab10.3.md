# Lab 10.3: Runbook QA Prompt - Summary of Work

## Objective
Create a prompt file `runbook-qa` (mode: ask) that:
- Answers questions using ONLY files in `runbooks/` directory via `@workspace`
- Quotes runbook file names and step numbers in responses
- Returns "No runbook covers this - escalate to L2" when no match exists
- Does not invent answers for unknown topics

## Deliverable Created

**File:** `genai-infraops-labs/runbook-qa.md`

```markdown
# runbook-qa
mode: ask

You are a runbook QA assistant. Answer the user's question: ${input:question}

**Instructions:**
1. Search ONLY files in the `runbooks/` directory via @workspace
2. When you find matching information, quote the **runbook file name** and **step numbers** explicitly
3. If no runbook covers the question, respond exactly with: "No runbook covers this - escalate to L2"
4. Be concise and step-focused in your answers

@workspace runbooks/
```

## How It Works

1. **Input Variable:** `${input:question}` accepts user queries
2. **Search Scope:** Restricted to `runbooks/` directory only
3. **Response Format:** Cites specific runbook file and step numbers
4. **Fallback:** Admits gaps without inventing procedures
5. **Mode:** `ask` enables interactive use with user prompts

## Available Runbooks

The prompt can reference these runbooks:
- `disk_cleanup.md` — Low disk space on /var (5 steps)
- `k8s_pod_restart.md` — Restart Kubernetes deployment (4 steps)
- `nginx_restart.md` — Restart nginx on web node (7 sections)
- `password_reset.md` — Password reset (4 steps)
- `portal_unreachable.md` — Customer portal unreachable (6 steps)
- `vpn_troubleshooting.md` — VPN disconnects or fails (4 steps)

## Test Execution Results

### Test 1: Disk Space Troubleshooting
**Question:** "How do I fix low disk space on /var?"
- **Result:** ✅ PASS
- **Citations:** `disk_cleanup.md` Steps 1-5
- **Verification:** Correctly cites df checks, logrotate status, config fixes, log compression, and verification

### Test 2: Kubernetes Restart
**Question:** "What's the procedure to restart a Kubernetes deployment?"
- **Result:** ✅ PASS
- **Citations:** `k8s_pod_restart.md` Steps 1-4
- **Verification:** Correctly references pre-checks, remediation plan, approval execution, and verification

### Test 3: Nginx Restart
**Question:** "How do I restart nginx when a site is down?"
- **Result:** ✅ PASS
- **Citations:** `nginx_restart.md` Sections 2-4
- **Verification:** Properly cites pre-checks, restart steps, and verification procedures with hostname confirmation

### Test 4: Portal Unreachable
**Question:** "How do I troubleshoot when the customer portal is unreachable?"
- **Result:** ✅ PASS
- **Citations:** `portal_unreachable.md` Steps 1-6
- **Verification:** Correctly references scope confirmation, ingress, endpoints, pods, events, and change checks

### Test 5: Out-of-Scope Question (Critical Test)
**Question:** "How do I reset the SAN switch?"
- **Result:** ✅ PASS
- **Response:** "No runbook covers this - escalate to L2"
- **Verification:** No invented procedures, no hallucinated steps, proper escalation message

## Verification Checklist

- [x] **Prompt file created** at `genai-infraops-labs/runbook-qa.md`
- [x] **Mode is `ask`** — interactive prompt with user input support
- [x] **Input variable correct** — uses `${input:question}`
- [x] **Workspace scoped** — searches only `runbooks/` directory
- [x] **Citations included** — all answers quote runbook file names and step numbers
- [x] **No invention** — out-of-scope question returns escalation message without fabricating answers
- [x] **All 5 test cases pass** — 4 valid questions route to correct runbooks, 1 unknown question escalates

## Key Features

1. **Grounded Responses** — Only cites actual runbook content
2. **Transparent Sourcing** — Every answer includes file and step references
3. **Safe Escalation** — Admits knowledge gaps rather than guessing
4. **L1/L2 Ready** — Appropriate for first-line support with clear escalation paths
5. **Maintenance Ready** — New runbooks automatically included via `@workspace`

## Status

✅ **Lab 10.3 COMPLETE**
- Prompt file created and tested
- All requirements met
- Production-ready for integration
