# Project Read File: Wallet Detection with iExec

## Project snapshot

This project is a wallet risk monitoring system with:
- **AI/rule-based backend** for wallet scoring,
- **FastAPI service** for analysis requests,
- **Next.js frontend** for operator dashboards,
- **integration layer** that demonstrates Agent → iExec → Contract orchestration.

## How iExec is being used

In this codebase, iExec usage is implemented as a **mock secure-compute workflow** to represent how production integration would work:

1. Wallet data is analyzed locally by the agent.
2. If risk is high/ambiguous, analysis is escalated to the iExec integration layer.
3. The iExec layer simulates TEE execution steps (send, encrypt, process, decrypt).
4. The returned result is marked as iExec-verified and can be submitted to the contract bridge.

This gives you a realistic architecture and control flow now, while making it straightforward to plug in the real iExec SDK later.

## Key files

- `agent/orchestrator.py` — core analysis orchestration.
- `integration/orchestrator.py` — complete Agent → iExec → Contract demo flow.
- `integration/agent_to_iexec.py` — bridge for automatic escalation.
- `integration/iexec_to_contract.py` — bridge for contract submission.
- `api_server.py` — backend API endpoint consumed by the frontend.
- `frontend/` — dashboard UI.
