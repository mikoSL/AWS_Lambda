# Lambda Introduction

## Limit
 1. Resource limit per invocation
 2. Account limit per region
 3. Deployment Limit

## Retries
 1. **Synchronous** failure (429 errors)
 2. **Asynchronous** failure: events queued before being used to invoke function, retried twice with delays, then send to **DLQ(Dead Letter Queue)** if configured.
 3. **Stream-based** failure: retry until data expires(diff per service), **block** and **not read** new records until retry succeeds. Order is ensured using the blocking principle.
