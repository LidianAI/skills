# Act

Execute an endpoint:

```bash
lidian act \
  --endpoint-id "<uuid>" \
  --params '{"document_url":"https://example.com/invoice.pdf"}' \
  --json
```

**Parameters:**
- `--endpoint-id`: UUID of the endpoint to call
- `--params`: JSON object with endpoint parameters
- `--payment-rail`: Payment method (optional)
- `--json`: Output as JSON

**Expected output:** Object with `data` and `credits` fields.

See [payments.md](payments.md) for payment options.
