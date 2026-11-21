# M06 Work Summary

## Scope

Work completed so far focused on Lab 6.1: running the local monitoring stack and improving and validating its node alert rules.

## Completed

- Installed Docker Engine and the Docker Compose v2 plugin for the lab environment.
- Started and verified the monitoring stack defined in [docker-compose.yml](../monitoring/docker-compose.yml). Grafana, Prometheus, and Node Exporter containers were up.
- Verified Grafana at `http://localhost:3000/api/health` returned HTTP 200 and reported its database was OK.
- Verified Prometheus at `http://localhost:9090/-/healthy` returned HTTP 200 and reported that the server was healthy.
- Updated [node_alerts.yml](../monitoring/rules/node_alerts.yml):
  - Added `HighMemory`, which fires when memory usage is above 90% for 5 minutes.
  - Added `NodeExporterDown`, which fires when the Prometheus `node` scrape target is down for 1 minute.
  - Added `impact`, `first_checks`, `owner_group`, and `runbook_url` annotations to the existing CPU and disk alerts, alongside their summaries. Unknown owner groups and runbooks are marked `TBD`.
- Validated the rules with `promtool`; all 4 rules passed.

## Remaining M06 Deliverables

- Generate and import a Grafana dashboard JSON.
- Create three alert playbooks under `runbooks/alerts/`.
- Complete Lab 6.2 alert correlation, incident enrichment, and SRE health report work.

## Environment Note

Docker commands in the agent shell required `sudo` because that user did not have permission to access the Docker socket. The Grafana and Prometheus web endpoints were nevertheless reachable and returned healthy responses during verification.
