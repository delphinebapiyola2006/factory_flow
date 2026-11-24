# FactoryFlow — Order-to-Audit Manufacturing ERP

A runnable prototype for the NIT Tiruchirappalli CDI Winter Internship 2026 benchmark.

## Stack

- FastAPI backend, SQLite database for a zero-friction local demo, vanilla JavaScript frontend.
- SQLite transactions and a source-linked stock movement ledger keep inventory traceable.
- Decimal arithmetic is used for business calculations; monetary values are stored as integer paise.
- SQLite is used for the packaged demo so evaluators can run it with one command. For deployment, migrate the schema to PostgreSQL and configure a persistent database volume.

## Run on Windows

```powershell
git clone <your-repo-url> FactoryFlow_NIT_Trichy
cd FactoryFlow_NIT_Trichy
py -m venv .venv
.\.venv\Scripts\Activate.ps1
pip install -r requirements.txt
uvicorn app.main:app --reload
Open http://127.0.0.1:8000 in your browser. Keep the PowerShell window running while using the application.

If PowerShell blocks activation:
Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass
.\.venv\Scripts\Activate.ps1
