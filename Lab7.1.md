# Lab 7.1: Multi-service log investigation

## Work completed

- Created [the log filter](../tools/log_filter.py) to select WARN and ERROR entries from the bundle and sort them by timestamp.
- Ran the following command from the project root, producing 20 entries in [filtered.log](m07/filtered.log):

  ```bash
  python3 tools/log_filter.py --from 09:00 --to 09:10 > outputs/m07/filtered.log
  ```

- Analyzed the filtered entries as data, and created a [reusable investigation checklist](m07/log_investigation.md).

## Ranked hypotheses

Times below are on 2026-11-09 in UTC+05:30. Confidence is qualitative, not a measured probability.

1. **Database connection pool exhaustion — high confidence as the immediate failure mechanism.** At 09:06:05, `db.log` reports `pool=portal active=50/50 waiting=42`; at 09:06:06 it reports `no more connections allowed (max_client_conn)`. At 09:06:08, `portal-app.log` reports `/login failed: timeout acquiring DB connection after 5000ms`. The auth service reports a slow upstream at 09:06:09 and nginx reports an upstream timeout at 09:06:10. This sequence supports a cascading failure but does not establish why the pool became exhausted.
2. **High load or slow database operations — low confidence.** The 09:06:05 pool entry shows 42 waiting, and at 09:06:09 `auth-service.log` reports `upstream portal-api slow, retrying (attempt 3)`. These are consistent with pressure on the pool, but the filtered logs contain neither request rates nor query durations. Check traffic, query latency, connection hold times, and retry rates.
3. **Pool capacity or connection limits insufficient for demand — low confidence.** At 09:06:05, `db.log` reports `active=50/50 waiting=42`, followed by `no more connections allowed (max_client_conn)` at 09:06:06. Limits may contribute, but the logs do not establish that the configuration is undersized or that raising it would be safe. Review configured limits, normal demand, and database capacity.

## Limitations and follow-up

- Compare the filtered timeline against the original logs, including earlier INFO entries, to investigate what changed before the observed errors. Correlation and ordering alone do not prove the ultimate cause.
- The filter reads `.log` files and uses the date and time zone of the first parsed entry. The end boundary is inclusive at the exact minute (09:10:00); it does not include the remainder of that minute. Consult original logs for the component field, which the filter omits.
- The checklist was saved under `outputs/m07/` as requested; the lab suggests a separate location under `runbooks/`.
