# Circle Patterns (Reference)

Source: https://www.circle.com/blog/autonomous-payments-using-circle-wallets-usdc-and-x402

This is a pattern reference, not a primary provider skill in this package.

## When To Use

Use these patterns when an agent wallet stack implements Circle-compatible USDC/x402 flows and needs architecture guidance.

## Pattern Mapping To Lidian

1. Discover endpoint (`lidian discover --json`).
2. Obtain requirements (prefer consume 402 challenge).
3. Map requirement fields to wallet-signing payload.
4. Attach `PAYMENT-SIGNATURE` and consume.

## Key Pattern Rules

- Sign exact requirement values returned at runtime.
- Keep chain/token constraints strict; do not auto-coerce.
- Use short-lived requirements and refresh on expiry.

## Minimal Consume Retry Example

```bash
curl -sS -X POST "https://api.lidian.ai/v1/consume" \
  -H "Content-Type: application/json" \
  -H "PAYMENT-SIGNATURE: <signature>" \
  -d '{"endpointId":"<endpoint_uuid>","params":{}}'
```

For concrete provider implementation details, use one of:
- [coinbase-agentic-wallet.md](coinbase-agentic-wallet.md)
- [privy-agentic-wallet.md](privy-agentic-wallet.md)
- [paysponge.md](paysponge.md)
- [frames-ag.md](frames-ag.md)
