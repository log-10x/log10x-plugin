---
name: log10x-log-cost
description: Use when someone asks which log patterns drive their log bill on Splunk, Datadog, Elasticsearch, CloudWatch or ClickHouse, or wants a cost POC on their own log files. Calls the log10x MCP server, which runs on this machine.
---

# Log cost by pattern with log10x

The log10x MCP server runs on this machine over stdio. Its POC reads log files, pod logs or a SIEM sample on this machine, and log content stays here.

## Which tool answers which question

- An open-ended cost question, or where to start: `log10x_start`.
- A cost POC on a log file, a directory, a glob, or pod logs through kubectl: `log10x_poc_from_local`. It needs the 10x engine, either the `tenx` binary on PATH or Docker.
- A cost POC against the user's SIEM, with read-only credentials set in the server's environment: `log10x_poc_from_siem_submit`, then `log10x_poc_from_siem_status` with the returned `snapshot_id`.
- Ranking patterns by volume and cost in an environment that already runs log10x: `log10x_top_patterns`.
- Which patterns are new, gone or back between two windows: `log10x_pattern_diff`.
- Installation health: `log10x_doctor`.

## Rules

- Quote a figure only as the tool returns it, together with the sample it came from. Do not project savings beyond the tool's output.
- Without an API key the server answers read-only from a public demo dataset. Say so whenever a result comes from the demo.
- `log10x_poc_from_siem_submit` starts a POC run; the other tools listed here are read-only.
