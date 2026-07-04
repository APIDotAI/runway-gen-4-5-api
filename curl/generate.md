# Runway Gen-4.5 cURL Quickstart

## What this example shows

This example shows how to submit a Runway Gen-4.5 generation task through APIDot, store the returned `task_id`, and poll the shared status endpoint for the result.

## When to use it

Use this example when you need a server-side cURL quickstart in a product backend, prototype, or media workflow. For production apps, submit the task from your backend, persist `task_id`, and add webhooks after your callback receiver is deployed.

## Requirements

- An APIDot account.
- An APIDot API key stored server-side.
- `curl` installed locally.
- A backend or terminal environment that does not expose API keys to browser code.

## Environment variables

Use placeholders only. Do not commit real credentials.

```env
APIDOT_API_KEY=YOUR_API_KEY_HERE
```

## How to run

Add `callback_url` only when you have a real webhook receiver. See the [webhooks docs](https://apidot.ai/docs/webhooks) for the production callback flow.

```bash
export APIDOT_API_KEY="YOUR_API_KEY_HERE"

curl --fail-with-body --request POST \
  --url https://api.apidot.ai/api/generate/submit \
  --header "Authorization: Bearer $APIDOT_API_KEY" \
  --header "Content-Type: application/json" \
  --data '{
  "model": "runway-gen-4.5",
  "input": {
    "prompt": "A luxury watch rotates on black marble as water droplets ripple outward, macro cinematic lighting, slow push-in, realistic reflections",
    "duration": 5,
    "aspect_ratio": "16:9"
  }
}'
```

Store the returned `data.task_id`, then poll status:

```bash
curl --fail-with-body --request GET \
  --url https://api.apidot.ai/api/generate/status/task-unified-example \
  --header "Authorization: Bearer $APIDOT_API_KEY"
```

## Request variants

### Text to video

```json
{
  "model": "runway-gen-4.5",
  "callback_url": "https://your-domain.com/callback",
  "input": {
    "prompt": "A luxury watch rotates on black marble as water droplets ripple outward, macro cinematic lighting, slow push-in, realistic reflections",
    "duration": 5,
    "aspect_ratio": "16:9"
  }
}
```

### Single-image guided video

```json
{
  "model": "runway-gen-4.5",
  "callback_url": "https://your-domain.com/callback",
  "input": {
    "prompt": "Animate the product into a premium launch shot with a slow camera orbit, soft studio reflections, and subtle atmospheric motion",
    "duration": 10,
    "aspect_ratio": "1:1",
    "image_urls": [
      "https://your-domain.com/reference-image.webp"
    ],
    "seed": 12345
  }
}
```

## Expected response

Submit response:

```json
{
  "code": 200,
  "data": {
    "task_id": "task-unified-example",
    "status": "not_started",
    "created_time": "2026-07-04T10:00:00"
  }
}
```

Shortened status response:

```json
{
  "code": 200,
  "data": {
    "task_id": "task-unified-example",
    "status": "finished",
    "output": {
      "files": [
        {
          "file_url": "https://example.com/generated-video.mp4",
          "file_type": "video"
        }
      ]
    },
    "error_message": null
  }
}
```

## Production notes

- Store `data.task_id` before polling or waiting for a webhook.
- Keep APIDot API keys on the server side only.
- Poll at a moderate interval and avoid hot loops.
- Treat `finished` and `failed` as terminal states.
- Store selected model, request payload, user ID, and task ID together for support and retries.
- Use `callback_url` only after your webhook receiver can process duplicate callbacks idempotently.

## Common mistakes

- Committing a real API key or `.env` file.
- Calling APIDot directly from browser code.
- Losing the returned `task_id` before the task reaches a terminal state.
- Polling continuously without delay.
- Assuming every video model accepts the same duration, resolution, or aspect ratio fields.

## Related links

- Website: https://apidot.ai
- Docs: https://apidot.ai/docs
- Runway Gen-4.5 docs: https://apidot.ai/docs/runway-gen-4-5
- Video models: https://apidot.ai/models/video
- Quickstart: https://apidot.ai/docs/quickstart
- Webhooks: https://apidot.ai/docs/webhooks
- GitHub: https://github.com/APIDotAI
- Examples: https://github.com/APIDotAI/apidot-examples
- Related landing page: https://apidot.ai/models/runway-gen-4-5
