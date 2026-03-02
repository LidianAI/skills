# Security Requirements

This skill assumes agents operate on pre-existing wallets. Do not create or expose secrets in skill logs.

## Secret Handling

- Never include seed phrases/private keys in prompts, logs, or artifacts.
- Use environment variables or secure secret stores only.
- Redact wallet identifiers when full address is not required.

## Signing Safety

- Sign only requirements sourced from current Lidian responses.
- Do not sign arbitrary payloads outside the active consume flow.
- Enforce strict domain/path allowlist (`https://api.lidian.ai/v1/consume` or approved environment equivalent).

## Logging

- Log high-level status, not sensitive signing internals.
- If logging headers, omit or redact `PAYMENT-SIGNATURE`.
- Keep only minimal payment metadata: provider, chain, amount, endpoint id, status.

## Environment Discipline

- Explicitly set target env/base URL before signing.
- Do not mix staging signatures with production consume calls.
- Fail closed on env ambiguity.
