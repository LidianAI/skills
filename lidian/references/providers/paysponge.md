# PaySponge Provider Runbook

Source: https://paysponge.com

## 1) When To Use

Use PaySponge when an agent already has a PaySponge-managed wallet and needs programmatic x402 signing.

## 2) Wallet Prerequisites

- Pre-existing stablecoin wallet in PaySponge
- Signing credentials configured in agent runtime secrets
- Target network/token support matching Lidian requirement

## 3) CLI-First Flow

```bash
lidian discover --q "invoice OCR" --pageSize 1 --json
```

Select one `endpointId`, then request requirements through consume challenge:

```bash
curl -i -sS -X POST "https://api.lidian.ai/v1/consume" \
  -H "Content-Type: application/json" \
  -d '{"endpointId":"<endpoint_uuid>","params":{"document_url":"https://example.com/invoice.pdf"}}'
```

## 4) API-Second Flow

- Extract `accepts[0]` from 402 body.
- Pass it to PaySponge signing logic.
- Retry consume with generated `PAYMENT-SIGNATURE`.

```bash
curl -sS -X POST "https://api.lidian.ai/v1/consume" \
  -H "Content-Type: application/json" \
  -H "PAYMENT-SIGNATURE: <paysponge_signature>" \
  -d '{"endpointId":"<endpoint_uuid>","params":{"document_url":"https://example.com/invoice.pdf"}}'
```

## 5) Signing Mapping (Input/Output)

Input to PaySponge signer:
- `network`
- `asset`
- `amount`
- `payTo`
- `maxTimeoutSeconds`

Output from PaySponge signer:
- `paymentSignature` header value

## 6) Failure Handling

- If signature rejected, refresh requirement and sign again.
- If chain/token mismatch, force signer config to requirement values.
- If balance insufficient, stop retries and emit required amount.

## 7) Minimal Example Contract

```json
{
  "provider": "paysponge",
  "paymentSignature": "<opaque_header_value>",
  "consumeStatus": "success"
}
```
