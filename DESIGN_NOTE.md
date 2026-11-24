# FactoryFlow — Design Note

## 1. Architecture
Browser UI → FastAPI REST API → SQLite relational database. The packaged version uses SQLite to make the evaluation demo reproducible without external services. The schema is designed around master records, source documents, and append-only stock movements; a production deployment should use PostgreSQL with migrations.

## 2. Core entities
- `users`: username, role.
- `customers`: retailer/distributor, state, GSTIN.
- `products`: SKU, pack size, selling price, HSN, GST rate.
- `materials`: raw / packaging item, base unit, opening quantity and valuation rate.
- `bom_lines`: product, material, standard quantity per batch.
- `purchases`: supplier, item, quantity, unit rate.
- `production_runs`: product, batch count, expected output, good output, wastage, overheads.
- `stock_movements`: item, quantity delta, unit, source type, source id, timestamp.
- `orders`, `order_lines`, `invoices`, `payments`, `audit_events`.

## 3. Stock computation
On-hand quantity is the sum of all stock movement deltas for an item. The app does not treat a user-editable stock number as the source of truth. Purchases add raw/packaging stock. Production issues subtract materials; production output adds finished goods. Dispatch subtracts finished goods. Each movement stores a source document type and ID. Mutating operations run in database transactions.

## 4. BOM and variance
For a production run of `n` batches, expected material quantity is `BOM quantity × n`. Actual quantity is entered by the supervisor. Variance percentage is `(actual − planned) / planned × 100`. Yield variance is `(actual good output − expected output) / expected output × 100`. Alerts are raised when absolute variance exceeds 3%.

## 5. Costing
Raw material valuation uses weighted average. The flour rate after benchmark purchase is `(100×42 + 60×45)/160 = ₹43.125/kg`. Packaging and other materials use benchmark rates. Run material cost is the sum of actual issued quantity multiplied by the applicable weighted-average unit rate. Labour and utilities are entered per run and added to material cost. Cost per good packet is total run cost divided by actual good output.

## 6. Tax
For the benchmark, the factory is in Tamil Nadu and GST is 5%. A Tamil Nadu customer invoice splits tax into CGST 2.5% and SGST 2.5%. A customer in another state uses IGST 5%. Money is stored in paise and displayed in rupees.

## 7. Important assumptions
- No pouch purchase occurs in the benchmark; closing pouches = 500 − 194 = 306.
- Six damaged pouches are consumed in addition to 188 good packets.
- Cartons are issued as 9 actual pieces as specified.
- The app blocks approval/dispatch when finished goods are insufficient; no negative stock is allowed.
- The prototype tax invoice is illustrative and is not a substitute for a GST professional's validation.
