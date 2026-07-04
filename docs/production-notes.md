# Production notes for Runway Gen-4.5 API workflows

- Keep API keys server-side.
- Persist task_id, selected model, request payload, user ID, and status.
- Make webhook handlers idempotent.
- Validate source media URLs before submit.
- Do not log API keys, private prompts, or private media URLs.
- Retry transient failures with backoff, not unchanged invalid payloads.
