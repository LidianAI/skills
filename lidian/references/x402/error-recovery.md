# Error Recovery

Use deterministic recovery logic for x402 payment orchestration.

## Invalid Signature (`invalid_payment`)

1. Rebuild signer input from fresh requirement object.
2. Ensure provider signer uses exact `network`, `asset`, `amount`, `payTo`.
3. Re-sign and retry consume once.

## Expired Requirement / Destination

1. Fetch fresh requirements.
2. Discard previous signature.
3. Re-sign with new requirement.

## Wrong Network or Asset

1. Compare signed payload against requirement fields.
2. Correct signer chain/token config.
3. Re-sign and retry.

## Insufficient Stablecoin Balance

1. Stop retries.
2. Emit actionable error including required amount and chain.
3. Suggest delegated CLI fallback only if policy allows.

## Endpoint Not Found (`not_found`)

1. Re-run `lidian discover --q "<task>" --json`.
2. Select a new endpoint id.
3. Rebuild full payment flow.

## Generic Execution Failure

1. Preserve `requestId`/response payload for telemetry.
2. Retry only if failure is upstream/transient and payment intent remains valid.
3. Otherwise stop and surface normalized failure output.
