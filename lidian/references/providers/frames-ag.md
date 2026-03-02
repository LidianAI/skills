# Frames.ag Provider Runbook

Source: https://frames.ag

## 1) When To Use

Use Frames.ag when an agent has access to a Frames-managed wallet/signing runtime for stablecoin payments.

## 2) Wallet Prerequisites

- Existing Frames wallet/session context
- Signing capability available to the agent
- Chain/token support matching Lidian requirement payload

## 3) CLI-First Flow

```bash
lidian discover --q "invoice OCR" --pageSize 1 --json
```

Fetch requirements by triggering consume challenge:

```bash
curl -i -sS -X POST "https://api.lidian.ai/v1/consume" \
  -H "Content-Type: application/json" \
  -d '{"endpointId":"<endpoint_uuid>","params":{"document_url":"https://example.com/invoice.pdf"}}'
```

## 4) API-Second Flow

- Extract `accepts[0]` requirement.
- Sign with Frames wallet tooling.
- Retry consume with returned signature.

```bash
curl -sS -X POST "https://api.lidian.ai/v1/consume" \
  -H "Content-Type: application/json" \
  -H "PAYMENT-SIGNATURE: <frames_signature>" \
  -d '{"endpointId":"<endpoint_uuid>","params":{"document_url":"https://example.com/invoice.pdf"}}'
```

## 5) Signing Mapping (Input/Output)

Signer input: `network`, `asset`, `amount`, `payTo`, `maxTimeoutSeconds`

Signer output: `paymentSignature`

## 6) Failure Handling

- Re-sign only with fresh requirements.
- Validate requirement chain/token equals signing chain/token.
- Emit explicit failure when wallet policy blocks the transfer.

## 7) Minimal Example Contract

```json
{
  "provider": "frames_ag",
  "paymentSignature": "<opaque_header_value>",
  "consumeStatus": "success"
}
```
