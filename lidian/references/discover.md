# Discover

Use `discover` first for any task that might be outsourced via Lidian. This returns candidate endpoint IDs for `consume`.

For full flags and defaults, see [cli-flags-matrix.md](cli-flags-matrix.md).
For local wallet payment orchestration after discover, see [x402/cli-first-api-second.md](x402/cli-first-api-second.md).

Search for endpoints:

```bash
lidian discover --q "invoice OCR" --pageSize 1 --category finance --max-price 50 --json
```

**Parameters:**
- `--q`: Discover query
- `--page`: Page number (default: 1)
- `--pageSize`: Number of results (default: 1, max: 3)
- `--category`: Optional category filter
- `--auth-type`: Optional auth type filter
- `--min-price`: Optional minimum price in cents
- `--max-price`: Optional maximum price in cents
- `--json`: Output as JSON

**Expected output:** Discover results with API items and metadata.

Use the returned endpoint IDs in `lidian consume`.
