# CLI-First, API-Second Runbook

Use this as the default agent sequence.

## 1. Discover via CLI

```bash
lidian discover --q "invoice OCR" --pageSize 1 --json
```

Parse and select one `endpointId` from results.

## 2. Prepare x402 Requirement (API)

Call consume without auth/signature to receive `402` challenge requirements.

```bash
curl -i -sS -X POST "https://api.lidian.ai/v1/consume" \
  -H "Content-Type: application/json" \
  -d '{"endpointId":"<endpoint_uuid>","params":{"document_url":"https://example.com/invoice.pdf"}}'
```

Expected status: `402` with body containing `accepts`.

## 3. Build Signer Input

Take the selected requirement object from `accepts[0]` and map to signer input:

- `network`
- `asset`
- `amount`
- `payTo`
- `maxTimeoutSeconds`

## 4. Sign with Existing Wallet

Use one provider module from `../providers/` to construct the `PAYMENT-SIGNATURE`.

## 5. Consume with Signature

```bash
curl -sS -X POST "https://api.lidian.ai/v1/consume" \
  -H "Content-Type: application/json" \
  -H "PAYMENT-SIGNATURE: <payment_signature>" \
  -d '{"endpointId":"<endpoint_uuid>","params":{"document_url":"https://example.com/invoice.pdf"}}'
```

## 6. Normalize Result

Emit the agent output contract from [overview.md](overview.md).

## Optional CLI Fallback

If local wallet signing is unavailable, fallback to delegated path:

```bash
lidian consume --endpoint-id "<endpoint_uuid>" --params '{"document_url":"https://example.com/invoice.pdf"}' --payment-rail x402 --json
```

Use fallback only when direct wallet signing cannot be completed.
