# Privy Agentic Wallet Runbook

Sources:
- https://docs.privy.io/recipes/agent-integrations/agentic-wallets
- https://github.com/privy-io/privy-agentic-wallets-skill

## 1) When To Use

Use Privy Agentic Wallet when the agent already operates with Privy-managed wallets and can perform signing in runtime.

## 2) Wallet Prerequisites

- Existing Privy wallet linked to agent identity
- Privy credentials configured as runtime secrets
- Chain/token support for current requirement payload

## 3) CLI-First Flow

```bash
lidian discover --q "invoice OCR" --pageSize 1 --json
```

Get requirement via consume challenge:

```bash
curl -i -sS -X POST "https://api.lidian.ai/v1/consume" \
  -H "Content-Type: application/json" \
  -d '{"endpointId":"<endpoint_uuid>","params":{"document_url":"https://example.com/invoice.pdf"}}'
```

## 4) API-Second Flow

- Parse `accepts[0]`.
- Use Privy wallet signing flow.
- Retry consume with `PAYMENT-SIGNATURE`.

```bash
curl -sS -X POST "https://api.lidian.ai/v1/consume" \
  -H "Content-Type: application/json" \
  -H "PAYMENT-SIGNATURE: <privy_signature>" \
  -d '{"endpointId":"<endpoint_uuid>","params":{"document_url":"https://example.com/invoice.pdf"}}'
```

## 5) Signing Mapping (Input/Output)

Input: `network`, `asset`, `amount`, `payTo`, `maxTimeoutSeconds`

Output: `paymentSignature`

## 6) Failure Handling

- Refresh requirement and re-sign for any `invalid_payment` response.
- Reject flows where signer network differs from requirement network.
- Emit precise error when wallet authorization/session expired.

## 7) Minimal Example Contract

```json
{
  "provider": "privy_agentic_wallet",
  "paymentSignature": "<opaque_header_value>",
  "consumeStatus": "success"
}
```
