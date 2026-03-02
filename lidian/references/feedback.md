# Feedback

Submit feedback for a prior `consume` execution using its `executionId`.

Use this after reviewing output quality.

```bash
lidian feedback \
  --execution-id "<execution_uuid>" \
  --rank 8 \
  --feedback "Output matched expected schema and latency target." \
  --json
```

**Parameters:**
- `--execution-id`: UUID returned by `lidian consume` (`executionId`)
- `--rank`: required integer `0..10`
- `--feedback`: optional text, max 1000 chars
- `--json`: output as JSON

**Expected output:** Object with `executionId`, `rank`, `feedback`, `submittedBy`, `updatedAt`.

Rules:
- One feedback record per execution; re-submitting updates the existing record.
- Use `rank=10` for best outcome quality.
