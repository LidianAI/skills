# CLI Flags Matrix

Agent-focused flag reference for `lidian`.

## Global Flags

| Flag | Type | Applies To | Required | Default | Values | Notes |
| --- | --- | --- | --- | --- | --- | --- |
| `--api-key` | string | discover, consume, feedback, account | No | resolved | `ld_...` | Highest-priority auth source for that command |
| `--api-base` | string URL | discover, consume, feedback, account | No | resolved | any valid base URL | Overrides env-based routing |
| `--env` | string | discover, consume, feedback, account | No | `production` | `production`, `staging` | Maps to canonical API base URLs |
| `--json` | boolean | all commands | No | `false` | `true` when present | Machine-friendly output |
| `--help` | boolean | all commands | No | `false` | `true` when present | Prints usage |

## `lidian login`

| Flag | Type | Required | Default | Values | Notes |
| --- | --- | --- | --- | --- | --- |
| `--key` | string | No | interactive prompt | `ld_...` | Use for non-interactive agent workflows |
| `--json` | boolean | No | `false` | presence flag | Returns config path in JSON envelope |

## `lidian discover`

| Flag | Type | Required | Default | Values | Notes |
| --- | --- | --- | --- | --- | --- |
| `--q` | string | Yes | none | free text | Discover query |
| `--page` | integer | No | `1` | positive integer | Pagination page |
| `--pageSize` | integer | No | `1` | `1..3` | CLI enforces bounds |
| `--category` | string | No | unset | category name | Optional filter |
| `--auth-type` | string | No | unset | `none`, `api_key`, `bearer`, `basic`, `oauth2`, `custom` | Optional filter |
| `--min-price` | number | No | unset | cents | Minimum cents |
| `--max-price` | number | No | unset | cents | Maximum cents |

## `lidian consume`

| Flag | Type | Required | Default | Values | Notes |
| --- | --- | --- | --- | --- | --- |
| `--endpoint-id` | UUID string | Yes | none | endpoint UUID | Required target endpoint |
| `--params` | JSON object string | No | `{}` | valid JSON object | Fails if invalid JSON/object |
| `--payment-rail` | string | No | `auto` | `prepaid_credits`, `x402` | Auto default: prepaid with auth, x402 without auth |
| `--network` | string | No | unset | `base`, `ethereum` | x402 network hint |

## `lidian account`

| Flag | Type | Required | Default | Values | Notes |
| --- | --- | --- | --- | --- | --- |
| `--json` | boolean | No | `false` | presence flag | Structured account response |

## `lidian feedback`

| Flag | Type | Required | Default | Values | Notes |
| --- | --- | --- | --- | --- | --- |
| `--execution-id` | UUID string | Yes | none | consume execution UUID | Returned from `lidian consume` |
| `--rank` | integer | Yes | none | `0..10` | Quality score |
| `--feedback` | string | No | unset | max 1000 chars | Optional freeform feedback |
| `--json` | boolean | No | `false` | presence flag | Structured feedback response |

## Auth Resolution Order

Auth is optional for `discover` and `consume` (x402 flow).
Auth is usually required for `account`.
`feedback` can be anonymous only for executions that were anonymous.

1. `--api-key`
2. `LIDIAN_API_KEY`
3. `~/.lidian/config.json` (saved by `lidian login`)

## API Base Resolution Order

1. `--api-base`
2. `LIDIAN_API_BASE`
3. `--env`
4. `LIDIAN_ENV`
5. default `https://api.lidian.ai`

`--env` mapping:
- `production` -> `https://api.lidian.ai`
- `staging` -> `https://staging-api.lidian.ai`

## Agent Defaults

For automated agents, prefer:

```bash
lidian discover --q "<task>" --pageSize 1 --json --env production
lidian consume --endpoint-id "<uuid>" --params '{"k":"v"}' --json --env production
lidian feedback --execution-id "<execution_uuid>" --rank 8 --json --env production
```

Use `--json` consistently for deterministic parsing.
