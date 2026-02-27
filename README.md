# Wallet Detection System with iExec

A full-stack prototype for **wallet risk assessment** that combines:
- a Python AI/rule-based analysis engine,
- optional LLM reasoning,
- an iExec secure-compute escalation path (mocked in this repo), and
- a Next.js dashboard for visualization.

## What this project does

The system evaluates wallet behavior and context signals, then returns a risk decision:

- `NO_ACTION`
- `MONITOR`
- `REQUEST_SEVERITY_ANALYSIS`
- `ENFORCE_ACTION`

For medium/high-risk scenarios, it can escalate analysis to an iExec-like secure processing stage and then (in the integration demo) submit outcomes to a smart-contract bridge.

---

## Architecture

```text
Frontend (Next.js)
   ↓ calls
FastAPI backend (`/analyze`)
   ↓ builds AgentInput
Agent Orchestrator (rules + optional LLM)
   ↓ when needed
iExec integration layer (mock TEE workflow)
   ↓
Smart contract bridge (mock submission)
```

### Main folders

- `agent/` — core decision engine, schemas, rules, and LLM reasoning.
- `integration/` — end-to-end orchestration for Agent → iExec → Contract flow.
- `api_server.py` — FastAPI endpoint used by the UI.
- `frontend/` — Next.js dashboard with wallet connection and risk views.
- `examples/` — runnable demos for local and end-to-end flows.

---

## How iExec is used in this project

This repository currently uses a **mock iExec integration** to model secure-compute behavior.

### Current iExec flow (implemented as simulation)

1. Local agent performs baseline risk analysis.
2. If risk is elevated (or decision is `REQUEST_SEVERITY_ANALYSIS`), the orchestrator escalates.
3. The iExec layer simulates:
   - sending payload to TEE,
   - encryption/decryption steps,
   - secure processing delay,
   - returning an enhanced result.
4. Enhanced result is optionally forwarded to the contract bridge when risk remains significant.

### Why this is useful

- Demonstrates where confidential wallet analytics would run under iExec.
- Separates local fast screening from high-assurance secure analysis.
- Provides a clear integration seam to replace mocks with real iExec SDK/task execution.

---

## Quick start

### 1) Install Python dependencies

```bash
pip install -r requirements.txt
```

### 2) Run backend API

```bash
python api_server.py
```

API runs on `http://localhost:8000` by default.

### 3) Run frontend

```bash
cd frontend
npm install
npm run dev
```

Open `http://localhost:3000`.

### 4) Run integration demo

From repo root:

```bash
python examples/e2e_integration_demo.py
```

---

## API overview

### `POST /analyze`

Minimal request shape:

```json
{
  "walletAddress": "0x...",
  "portfolio": {
    "walletAge": 120,
    "transactions": 321,
    "totalValue": 15000
  }
}
```

Response includes decision, confidence, reasoning, risk score, recommendations, flags, and metadata.

---

## Notes

- iExec and contract components are mocked for demo purposes.
- The repository is structured so production adapters can replace mocks with real iExec tasks and on-chain writes.
