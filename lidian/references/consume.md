# Consume

Consume a specific endpoint returned by `lidian discover`.

Do not guess endpoint IDs. Discover them first with `discover`.

For full flags and defaults, see [cli-flags-matrix.md](cli-flags-matrix.md).
For local wallet payment orchestration, see [x402/overview.md](x402/overview.md) and [x402/signature-handoff-contract.md](x402/signature-handoff-contract.md).

Consume an endpoint:

```bash
lidian consume \
  --endpoint-id "<uuid>" \
  --params '{"document_url":"https://example.com/invoice.pdf"}' \
  --json
```

**Parameters:**
- `--endpoint-id`: UUID of the endpoint to call
- `--params`: JSON object with endpoint parameters
- `--payment-rail`: Payment method (optional). Default behavior:
  - with API key/token: `prepaid_credits`
  - without auth: `x402`
- `--network`: Optional x402 network hint (`base` or `ethereum`)
- `--json`: Output as JSON

**Expected output:** Object with `executionId`, `data`, and `credits` fields (`credits` are cent-based units).

Use `executionId` to submit feedback:

```bash
lidian feedback --execution-id "<execution_uuid>" --rank 9 --json
```

If execution fails due to auth or balance issues, run `lidian account --json` to diagnose.

See [payments.md](payments.md) for payment options.
