# FactoryFlow enhancement notes

This update preserves the original FastAPI + SQLite + vanilla JavaScript single-page application and enhances the existing modules rather than replacing them with a static mockup.

## What changed

- Added `/api/analytics` to provide monthly invoice totals, order-status counts, and top customers from the live SQLite database.
- Reworked the Overview dashboard with a responsive SVG sales trend, order pipeline bars, live KPIs, raw-material availability, quick actions, top customers, and recent orders.
- Added a clearly labelled Logout button in the signed-in header. Logout calls `/api/logout`, invalidates the server-side in-memory token, clears the browser token/session, and returns to the login screen.
- Improved sales order entry with customer GSTIN/state details, product price, quantity, and a live estimated taxable value, GST and total preview. The estimate is explicitly labelled as an estimate; final tax is calculated by the invoice workflow.
- Fixed the Cancel action in the sales-order form so it no longer submits the form accidentally.
- Updated responsive styling for analytics panels, order summary, and logout control.

## Validation performed

- `node --check app/static/app.js` passed.
- `python -m py_compile app/main.py` passed.
- Started the FastAPI app against a fresh temporary SQLite database.
- Verified homepage, login, `/api/me`, `/api/overview`, `/api/analytics`, `/api/masters`, and `/api/orders` endpoints.
- Verified `/api/logout` invalidates the token and subsequent `/api/me` returns HTTP 401.

## Important prototype limitations

- The seeded demo master currently contains one product and two customers, so dropdown options are limited by the actual demo data. Add more product/customer master records to expand those choices; the UI does not invent catalogue entries.
- The sales trend is based on invoice dates and therefore displays an empty-state message until invoices exist.
- Demo passwords remain plaintext and shared (`factory123`) in this internship prototype. Do not expose this build as a public production service without password hashing, secure persistent sessions, HTTPS, database hardening, and proper user administration.
- GST/invoice outputs are demonstrator calculations and require professional statutory review before real commercial use.
