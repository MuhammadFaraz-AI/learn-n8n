# 07 — Webhook Trigger + Respond to Webhook

## ⚠️ Before you look at workflow.json
Try building this yourself first from the instructions below. Only open `workflow.json` afterward to verify.

## Goal
Learn to trigger a workflow from an outside HTTP request instead of manually — the foundation of every real n8n integration (forms, apps, other services calling into n8n).

## Concepts covered
- **Webhook node** as a trigger (GET method, custom path)
- **Test URL** vs **Production URL** — test only works right after clicking "Listen for test event"; production requires the workflow to be Activated
- **Respond to Webhook** node, and why the Webhook node's own "Respond" setting must be set to **"Using 'Respond to Webhook' Node"** (not "Immediately") when you're using a separate response node — otherwise n8n throws a `WorkflowConfigurationError: Unused Respond to Webhook node`

## Workflow structure
```
Webhook (GET, path: hello-test) → Edit Fields (message: "Hello, your webhook works!") → Respond to Webhook
```

## Webhook node settings
- HTTP Method: `GET`
- Path: `hello-test`
- Respond: `Using 'Respond to Webhook' Node`

## Respond to Webhook node settings
- Mode: `First Incoming Item`

## Expected output
Visiting the Test URL in a browser returns:
```json
{ "message": "Hello, your webhook works!" }
```

## Screenshot
![Workflow 7 result](PASTE_YOUR_IMAGE_URL_HERE)

## Common error & fix
**Error:** `Received request for unknown webhook` / `Unused Respond to Webhook node found in the workflow`
**Cause:** Either the Test URL expired (you have to click "Listen for test event" then immediately hit the URL), or the Webhook node's Respond setting was left on "Immediately" while a separate Respond to Webhook node also existed.
**Fix:** Set Webhook node's Respond to "Using 'Respond to Webhook' Node", re-click "Listen", then hit the Test URL right away.

## What I learned / notes
- A webhook isn't "always on" until the workflow is Activated — in test mode it's a one-shot listener
- The Webhook node and the Respond to Webhook node are two halves of the same conversation — only one of them should own the "how do we respond" decision at a time

## Status
✅ Completed — [Date: 7 July 2026]
