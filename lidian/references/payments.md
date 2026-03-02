# Payments

## Prepaid Balance (default rail)

Use your existing prepaid balance:

```bash
lidian consume \
  --endpoint-id "<uuid>" \
  --params '{"document_url":"https://example.com/invoice.pdf"}' \
  --payment-rail prepaid_credits \
  --json
```

## x402 (delegated)

Use API-managed wallet with preflight + verify:

```bash
lidian consume \
  --endpoint-id "<uuid>" \
  --params '{"document_url":"https://example.com/invoice.pdf"}' \
  --payment-rail x402 \
  --json
```

**Note:** CLI uses API-managed preflight/verify endpoints. Wallet signing is handled server-side in v1.

In non-JSON CLI output, spend/balance are shown in cents with USD equivalents.

## x402 (local wallet orchestration)

For agents paying with a pre-existing stablecoin wallet, use:

1. Discover endpoint via CLI (`lidian discover --json`)
2. Obtain requirements via `POST /v1/consume` challenge or `/v1/payments/requirements`
3. Sign with provider wallet
4. Retry `POST /v1/consume` with `PAYMENT-SIGNATURE`

Runbooks:

- [x402/cli-first-api-second.md](x402/cli-first-api-second.md)
- [x402/payment-requirements-sources.md](x402/payment-requirements-sources.md)
- [providers/paysponge.md](providers/paysponge.md)
- [providers/coinbase-agentic-wallet.md](providers/coinbase-agentic-wallet.md)
- [providers/frames-ag.md](providers/frames-ag.md)
- [providers/privy-agentic-wallet.md](providers/privy-agentic-wallet.md)
- [providers/circle-patterns.md](providers/circle-patterns.md)
- [providers/vetted-options.md](providers/vetted-options.md)
