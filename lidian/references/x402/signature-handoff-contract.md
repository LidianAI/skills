# Signature Handoff Contract

This defines the contract between the Lidian orchestration agent and a provider wallet signer.

## Signer Input

Pass this object to provider signing logic:

```json
{
  "scheme": "exact",
  "network": "eip155:8453",
  "asset": "0x...",
  "amount": "2000000",
  "payTo": "0x...",
  "maxTimeoutSeconds": 300,
  "resource": {
    "endpointId": "<endpoint_uuid>",
    "path": "/v1/consume",
    "method": "POST"
  }
}
```

Rules:

- `network`, `asset`, `amount`, and `payTo` must come from current requirements.
- Do not transform decimals/units beyond provider SDK requirements.
- Do not swap chain or token defaults.

## Signer Output

Signer must return a valid value for `PAYMENT-SIGNATURE` header:

```json
{
  "paymentSignature": "<opaque_signature_header_value>"
}
```

## Consume Request Contract

```bash
curl -sS -X POST "https://api.lidian.ai/v1/consume" \
  -H "Content-Type: application/json" \
  -H "PAYMENT-SIGNATURE: <opaque_signature_header_value>" \
  -d '{"endpointId":"<endpoint_uuid>","params":{}}'
```

## Validation Expectations

- Invalid signature -> `invalid_payment` (400/402)
- Stale/expired destination -> refresh requirements and re-sign
- Amount mismatch -> re-read requirements and re-sign
