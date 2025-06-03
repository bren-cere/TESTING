Below is GitHub-flavoured Markdown you can drop straight into a **README.md**.

````markdown
# Developer Console “Top-Up” RFP — 60-Second Overview

- **Problem → Goal**  
  Developer-Console accounts occasionally run out of the CERE tokens and storage credits they need to push data into the Decentralised Data Cloud (DDC). The **Top-Up** micro-service will detect a low-balance event (or a user request) and automatically fund the account, so builders never hit a hard stop.

- **Scope of Work**  
  1. **Event watcher** – listens to the Substrate-based DDC chain (or a message queue) for `BalanceLow` events and to an HTTP endpoint for manual top-up requests.  
  2. **Top-up engine** – computes how many $CERE or quota units to add, then calls the existing `ddcModule.topUp(AccountId, Amount)` extrinsic (or the equivalent REST/GraphQL method exposed by the Developer-Console backend).  
  3. **Idempotent retry & logging** – every attempted top-up is written to PostgreSQL and emits a Prometheus metric; duplicate events are safely ignored.  
  4. **Ops artefacts** – Dockerfile, Kubernetes Helm chart, CI job (GitHub Actions) that runs tests plus an end-to-end script against a local devnet.

- **Deliverables**  
  * Source code (TypeScript + Node 20) under `cluster-apps/apps/developer-console/topup-service/`  
  * Unit & e2e tests (≥ 90 % line coverage)  
  * Swagger/OpenAPI doc for the `/top-up` HTTP endpoint  
  * README quick-start (below) and change-log  

- **Success criteria**  
  * A fresh account with ≤ threshold balance is automatically replenished within **5 seconds** on a devnet.  
  * The full test suite passes in CI; container image publishes to GitHub Packages; Helm install works out-of-the-box.

---

## Quick-Start for External Contributors

> **Goal:** get the Top-Up service running locally, prove that an empty account is auto-funded, and run the test suite — all in < 10 minutes.

### 1. Clone & set-up

```bash
git clone https://github.com/Cerebellum-Network/cluster-apps.git
cd cluster-apps
pnpm i          # uses the workspace root for all packages
````

### 2. Spin up a local DDC devnet + Postgres

```bash
# Single-host dev stack
docker compose -f docker-compose.devnet.yml up -d   # Substrate node + PG + Grafana
```

### 3. Build & run the Top-Up service

```bash
cd apps/developer-console/topup-service
pnpm dev        # hot-reload
# or, for production-like:
docker build -t cere/topup .
docker run --env-file .env.local cere/topup
```

**Required env vars**

| Variable            | Example                                     | Purpose                            |
| ------------------- | ------------------------------------------- | ---------------------------------- |
| `RPC_ENDPOINT`      | `ws://localhost:9944`                       | WebSocket URL of the devnet node   |
| `TOPUP_AMOUNT`      | `10000000000`                               | Default \$CERE (in plancks) to add |
| `BALANCE_THRESHOLD` | `5000000000`                                | Trigger when balance < threshold   |
| `PG_URL`            | `postgres://cere:cere@localhost:5432/topup` | Event-log DB                       |

### 4. Run the acceptance tests

```bash
pnpm test           # unit + integration
pnpm test:e2e       # spins a disposable devnet, drains an account, asserts top-up
```

### 5. Verify it works

```bash
# Ask the faucet endpoint to top up `ALICE`
curl -X POST http://localhost:7070/top-up \
  -H 'Content-Type: application/json' \
  -d '{"account":"5GrwvaEF...", "reason":"manual"}'

# Tail the logs or hit Prometheus on :9090 to see metrics
```

### 6. One-line deploy to Kubernetes

```bash
helm upgrade --install topup ./charts/topup \
     --set image.tag=$(git rev-parse --short HEAD)
```

---

With this micro-service in place, every developer using the Cere **Developer Console** gets an uninterrupted DX; the on-call team gets metrics & retries; and the infrastructure remains token-economical thanks to a single, centralised Top-Up logic.

```
```
