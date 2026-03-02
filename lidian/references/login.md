# Login

Authenticate with your API key:

```bash
lidian login --key ld_...
```

Or use interactive browser flow:

```bash
lidian login
```

Optionally set API base URL:

```bash
export LIDIAN_API_BASE="https://api.lidian.ai"
```

Or select environment:

```bash
export LIDIAN_ENV="staging" # production|staging
```

For non-interactive agents, prefer passing `--key` and setting `LIDIAN_ENV`.
