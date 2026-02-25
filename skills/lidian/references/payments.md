# Payments

## Prepaid Credits

Use your existing credits balance:

```bash
lidian act \
  --endpoint-id "<uuid>" \
  --params '{"document_url":"https://example.com/invoice.pdf"}' \
  --payment-rail prepaid_credits \
  --json
```

## x402 (delegated)

Use API-managed wallet with preflight + verify:

```bash
lidian act \
  --endpoint-id "<uuid>" \
  --params '{"document_url":"https://example.com/invoice.pdf"}' \
  --payment-rail x402 \
  --json
```

**Note:** CLI uses API-managed preflight/verify endpoints. Wallet signing is handled server-side in v1.
