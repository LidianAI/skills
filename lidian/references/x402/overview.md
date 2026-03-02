# x402 Local Wallet Orchestration Overview

This runbook is for agents using an existing stablecoin wallet to pay for Lidian consumption via x402.

## Canonical Flow

1. Discover endpoint candidates (`lidian discover --json`).
2. Select an `endpointId`.
3. Prepare payment requirements.
4. Sign with a pre-existing provider wallet.
5. Consume with `PAYMENT-SIGNATURE`.
6. Return normalized result and payment metadata.

## Requirement Sources

- Preferred: `POST /v1/consume` without API key and without `PAYMENT-SIGNATURE`.
  - Returns `402 Payment Required` with `accepts` requirements.
- Alternate: `POST /v1/payments/requirements`.

Use the returned requirement values as source of truth for signing.

## Stablecoin Notes

- Treat payment amounts as cent-based accounting units when reported by Lidian.
- Use the requirement's `asset`, `network`, `amount`, and `payTo` exactly as returned.
- Do not hardcode token or chain values when requirements disagree.

## Agent Output Contract

Return these fields after execution:

```json
{
  "endpointId": "uuid",
  "requirementSource": "consume_402|payments_requirements",
  "provider": "paysponge|coinbase_agentic_wallet|frames_ag|privy_agentic_wallet",
  "consumeStatus": "success|failure",
  "paymentHeaderSent": true,
  "spentCents": 0,
  "balanceCents": 0,
  "raw": {}
}
```

If the upstream path does not include balance values, set numeric fields only when present.
