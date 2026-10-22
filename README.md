# Sentinel AI — Agent Safety & Control MVP

A beginner-friendly Streamlit prototype for hackathon problem statement PS 2.1. Sentinel AI reviews proposed tool actions, applies deterministic permissions and safety policy, simulates allowed actions, and keeps a local audit dashboard.

## What it demonstrates

- Agent roles with different tool permissions.
- Deterministic blocks for unauthorized actions, sensitive external sharing, high refund amounts, record deletion, and admin grants.
- Human approval gate for high-impact actions such as external sharing and refunds.
- Optional OpenAI model commentary that explains contextual risks. The model cannot approve or run actions.
- SQLite audit history with decision, reason, AI note, simulated result, and approver.
- No real business-system integration; all outcomes are simulated.

## Setup (Windows PowerShell)

```powershell
py -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install -r requirements.txt
streamlit run app.py
```

Open the local URL printed by Streamlit (usually `http://localhost:8501`). Python 3.10+ recommended.

## AI key

For a live AI assessment, provide an OpenAI API key in the sidebar or set `OPENAI_API_KEY` before starting Streamlit. The default model is `gpt-4.1-mini`; change it in the sidebar if your account uses another model. API usage is billed according to your OpenAI account. Keep the key private and do not commit it.

Without a key, the app uses a clearly labeled local fallback; the deterministic policy and demo still work. The model receives the action, target, and request context when enabled, so use fictional demo data.

## Demo script (about 3 minutes)

1. Choose **Support Agent**. Request **Read public knowledge base**, target `returns policy`, context `Answer a user's question`. Submit: policy allows it and logs a simulated read.
2. Still as Support Agent, request **Issue refund**, target `order 123`, context `Refund $25 for duplicate charge`. Submit: blocked because the role lacks refund permission.
3. Choose **Finance Agent**, request **Issue refund**, target `order 123`, context `Refund $25 for duplicate charge`. Submit: requires approval. Enter `Demo Reviewer` and approve; the app records an approved simulated refund.
4. Choose **Admin Agent**, request **Share file externally**, target `payroll.csv`, context `Send this spreadsheet to the vendor`. Submit: blocked by sensitive-data policy even though the role can use the tool.
5. Request **Delete record** with any target: blocked by the prototype's hard-stop policy.
6. Open **Audit dashboard** to show all decisions, interventions, approver, and event detail. If an OpenAI key is set, show the model's risk note alongside the deterministic decision.

## Project files

- `app.py` — UI, AI commentary, deterministic policy, safe simulator, and SQLite audit trail.
- `requirements.txt` — Python packages.
- `audit.db` — created locally after the first event; excluded from version control.

## Scope and limitations

This is a hackathon MVP for demonstrating a control pattern, not a production agent-security product. Production deployment would need authenticated identities, tamper-resistant centralized audit storage, stronger data classification, integration-level authorization, secrets management, and tests against adversarial inputs.
