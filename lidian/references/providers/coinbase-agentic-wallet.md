# Coinbase Agentic Wallet Runbook

Source: https://docs.cdp.coinbase.com/agentic-wallet/welcome

## 1) When To Use

Use Coinbase Agentic Wallet when the agent already runs in CDP wallet context and can perform payment signing.

## 2) Wallet Prerequisites

- Pre-existing Coinbase agentic wallet
- Required CDP credentials in runtime secrets
- Stablecoin/network support for returned requirement

## 3) CLI-First Flow

```bash
lidian discover --q "invoice OCR" --pageSize 1 --json
```

Then fetch requirement via consume challenge:

```bash
curl -i -sS -X POST "https://api.lidian.ai/v1/consume" \
  -H "Content-Type: application/json" \
  -d '{"endpointId":"<endpoint_uuid>","params":{"document_url":"https://example.com/invoice.pdf"}}'
```

## 4) API-Second Flow

- Extract requirement from `accepts[0]`.
- Sign via Coinbase Agentic Wallet API/SDK.
- Retry consume with `PAYMENT-SIGNATURE`.

```bash
curl -sS -X POST "https://api.lidian.ai/v1/consume" \
  -H "Content-Type: application/json" \
  -H "PAYMENT-SIGNATURE: <coinbase_signature>" \
  -d '{"endpointId":"<endpoint_uuid>","params":{"document_url":"https://example.com/invoice.pdf"}}'
```

## 5) Signing Mapping (Input/Output)

Input to signer:
- `network`, `asset`, `amount`, `payTo`, `maxTimeoutSeconds`

Output:
- `paymentSignature` for `PAYMENT-SIGNATURE`

## 6) Failure Handling

- `invalid_payment`: refresh requirement and re-sign.
- Amount mismatch: do not reuse stale requirement.
- Unsupported chain: re-discover endpoint or change execution target.

## 7) Minimal Example Contract

```json
{
  "provider": "coinbase_agentic_wallet",
  "paymentSignature": "<opaque_header_value>",
  "consumeStatus": "success"
}
```
