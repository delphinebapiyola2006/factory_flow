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

PowerShell
Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass
.\.venv\Scripts\Activate.ps1
Demo logins
All demo passwords are factory123.

Admin: admin (admin)

Salesman A: salesa (salesman)

Salesman B: salesb (salesman)

Production: production (production)

Auditor: auditor (auditor, read-only)

Owner: owner (owner)

This prototype uses demo credentials and must be hardened before public production deployment.

Benchmark walkthrough
Login as admin and click Reset benchmark to load the exact master data.

Record the 60 kg flour purchase at ₹45/kg.

Login as production and create a Murukku 200 g run: flour 40 kg, oil 10.5 L, spice 1.2 kg, good output 188, damaged pouches 6, cartons 9.

Login as Salesman A and submit 150 packets to Retailer A.

Login as admin, approve, invoice and dispatch the order.

Login as Salesman B and submit 120 packets to Distributor B. Admin approval is blocked for shortage.

Record ₹4,000 against Retailer A's invoice.

Add ₹1,200 labour and ₹600 gas/power to the production run (run details panel).

Open Reports and export CSV / Excel / PDF.

Benchmark assumptions and notes
The expected benchmark balances are reproduced: flour 120 kg; oil 19.5 L; spice 3.8 kg; pouches 306; cartons 21; finished goods 88 after dispatch.

Pouch closing stock is calculated as 500 opening minus 194 consumed = 306 (no pouch purchase is part of this scenario).

The 6 damaged pouches are included in consumption, so 194 pouches are issued in total.

Cartons: 30 minus 9 = 21.

The system records a production issue of 40 kg flour, 10.5 L oil, 1.2 kg spice, 194 pouches and 9 cartons. The scenario does not specify actual carton count per packet; 9 cartons is accepted as actual packaging usage.

Oil variance = (10.5 - 10) / 10 = +5%. Yield variance = (188 - 200) / 200 = -6%.

Tax is 5%. Tamil Nadu intra-state sales split into CGST 2.5% + SGST 2.5%. Karnataka interstate sales use IGST 5% if invoiced.

Weighted-average flour rate after purchase = (100×42 + 60×45) / 160 = ₹43.125/kg. Other opening valuation rates use the supplied benchmark rates.

Costing includes the consumed raw materials and packaging plus labour and gas/power, divided by good packets. With the benchmark rates, production cost is calculated by the app. This is a prototype approximation; freight, GST input credit, returns, and detailed overhead allocation are not modeled.

Orders are not invoiced or dispatched until sufficient finished goods exist. Partial dispatch is not implemented.

This is a demonstrator, not a certified GST accounting product. Invoice numbering, tax treatment, statutory formats, and security need professional review before real business use.

Implemented
Enhanced dashboard with live monthly invoiced-sales trend, order-status distribution, raw-material balances and top-customer ranking.

Visible Logout action in the signed-in header; logout invalidates the current in-memory session token and clears the browser session.

Improved sales order entry with customer GST details, product pricing and a live estimated taxable value / GST / total preview.

Role-based demo login and role-specific navigation.

Product, customer, raw material and BOM masters seeded from benchmark.

Purchase receipt, BOM production run, planned-vs-actual variance, stock ledger.

Salesman order entry, admin approval, GST invoice PDF, dispatch and payment recording.

Owner dashboard, sales and stock reports, CSV/Excel/PDF exports.

Audit events for key operations.

Known gaps / next hardening steps
Replace demo authentication with hashed passwords, secure sessions/JWT, CSRF protection and rate limiting.

Move SQLite to PostgreSQL for multi-user hosted deployment.

Implement offline-first PWA sync, partial dispatch, returns, supplier master UI, credit notes and full GST statutory reconciliation.

Add per-user audit history for all edits, immutable posted invoices, invoice cancellation via credit notes.

Add unit conversion catalogue, FEFO batch/expiry tracking, automated tests and deployment secrets.
