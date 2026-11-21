# Lab 5.1: Generate and deploy manifests

## Cluster and namespace

- `kubectl get nodes` showed `imss-lab-control-plane` was Ready.
- Namespace `demo` already existed and was Active.

## Manifests

The existing files in [labs/m05/app](../labs/m05/app/) were verified and not overwritten:

- [deployment.yaml](../labs/m05/app/deployment.yaml): nginx Deployment `web` with three replicas, `app=web` pod labels, readiness and liveness probes, and CPU and memory requests and limits.
- [service.yaml](../labs/m05/app/service.yaml): ClusterIP Service `web` selecting pods with `app=web`.
- [configmap.yaml](../labs/m05/app/configmap.yaml): nginx configuration mounted by the Deployment.

The Deployment requests 100m CPU and 128Mi memory per container, with limits of 500m CPU and 512Mi memory.

## Validation and deployment

The following commands succeeded:

```bash
kubectl apply --dry-run=server -f /home/pranawkumarpathak/Downloads/IMSS-SK/genai-infraops-labs/labs/m05/app
kubectl apply -f /home/pranawkumarpathak/Downloads/IMSS-SK/genai-infraops-labs/labs/m05/app
kubectl rollout status deployment/web -n demo
```

The manifests were unchanged when applied. The rollout completed successfully, and Deployment `web` reported 3/3 replicas ready and available.

## Endpoint verification

Service `web` initially had six ready endpoints because both the `web` and older `nginx-web` Deployment pods matched `app=web`. The older Deployment was scaled to zero:

```bash
kubectl scale deployment/nginx-web -n demo --replicas=0
```

Afterward, `web` remained 3/3 ready and its Service had three endpoints:

- `web-959dbc655-5zz7z`
- `web-959dbc655-d89tn`
- `web-959dbc655-pbrdq`

The older `nginx-web` Service also selects `app=web`, so it still points to the remaining web pods.

## Rollout explanation

`deployment "web" successfully rolled out` means Kubernetes completed the Deployment rollout. The subsequent check showed all three desired replicas ready and available. The explanation was provided in chat.

## Scope note

The lab objective mentions an API, but the listed steps specify only the nginx app. No separate API was deployed or verified in this session.
