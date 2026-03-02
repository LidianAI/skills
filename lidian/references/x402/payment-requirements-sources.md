# Payment Requirement Sources

Use these sources to obtain the exact x402 requirement to sign.

## Source A (Preferred): Consume 402 Challenge

Request:

```bash
curl -i -sS -X POST "https://api.lidian.ai/v1/consume" \
  -H "Content-Type: application/json" \
  -d '{"endpointId":"<endpoint_uuid>","params":{}}'
```

Behavior:

- If no valid API key and no `PAYMENT-SIGNATURE`, endpoint returns `402`.
- Body includes `accepts` requirements.
- Header includes `PAYMENT-REQUIRED`.

Use `accepts[0]` as canonical signing payload.

## Source B: Payments Requirements Endpoint

Request:

```bash
curl -sS -X POST "https://api.lidian.ai/v1/payments/requirements" \
  -H "Content-Type: application/json" \
  -d '{"endpointId":"<endpoint_uuid>","network":"base"}'
```

Returns `payTo`, `amount`, `network`, `asset`, and metadata.

## Source Selection Rule

- Prefer Source A to mirror the exact consume challenge.
- Use Source B when orchestration needs explicit preflight step or challenge body is unavailable.
- Never sign against stale requirement objects; refresh requirements if timeout/expiry is reached.
