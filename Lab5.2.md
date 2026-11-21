# Lab 5.2: Kubernetes Fault Diagnosis and Remediation

## Work completed

Applied the lab manifests in `k8s/broken/` and inspected pods, Services, endpoints, pod descriptions, events, selectors, and labels in the `portal` namespace.

| Symptom | Evidence | Root cause | Fix applied |
|---|---|---|---|
| `portal-api` ImagePullBackOff | Image `nginx:1.27-alpine-hotfx` was reported not found | Invalid image tag | Changed the image to `nginx:1.27-alpine` |
| `portal-web` Service had no endpoints | Service selected `app=portal-web-v2`; web pods had `app=portal-web` | Service selector mismatch | Changed the selector to `app=portal-web` |
| `report-worker` CrashLoopBackOff | Container was `OOMKilled`, exit code `137`, with a `32Mi` memory limit | The placeholder ran the unbounded `tail /dev/zero` command under a restrictive limit | Replaced the command with `sleep 3600` |
| `analytics-batch` Pending | Scheduler reported insufficient CPU and memory | Requests of 64 CPUs and 256Gi exceeded available resources | Set requests to 100m CPU and 128Mi memory, with limits of 200m and 256Mi |

The worker's `sleep` command is a lab placeholder, not a real report-processing workload. The OOM evidence does not establish a suitable memory limit for a real worker.

## Verification

- Applied the corrected workload manifests.
- All three updated Deployments reported successful rollouts.
- At the last workload check, all five pods were `1/1 Running`, and the `portal-web` Service had two endpoints.
- Recorded the diagnosis and remediation in [diagnosis.md](../labs/m05/diagnosis.md).

## RBAC

Applied the [RBAC manifest](../k8s/rbac/overpermissive-role.yaml) in the `portal` namespace. The `desk-support` Role grants `get`, `list`, and `watch` on pods and `get` on pod logs to the `servicedesk-l2` group.

Impersonation checks confirmed pod and log read access and denied pod deletion.

## Remaining notes

- The diagnosis file was untracked and the latest workload manifest fixes were unstaged at the last Git check; the staging attempt was denied.
- The possibly misspelled Ingress hostname has not been verified. It is separate from the four expected Lab 5.2 faults.
