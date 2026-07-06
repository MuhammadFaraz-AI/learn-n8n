# 05 — Error Handling (Continue on Fail / Error Output)

## ⚠️ Before you look at workflow.json
Try building this yourself first from the instructions below. Only open `workflow.json` afterward to verify.

## Goal
Learn to catch a node's failure gracefully instead of letting the whole workflow crash — the local, per-node version of error handling (as opposed to a global Error Trigger workflow).

## Concepts covered
- **Continue On Fail** node setting → "Continue using error output"
- The extra "Error" output this creates, alongside the normal "Success" output
- Reading `{{ $json.error.message }}` on the error branch

## Workflow structure
```
When clicking 'Execute workflow' → Code in JavaScript ─┬─(Success)─→ Edit Fields1 (result: success)
                                                         └─(Error)──→ Edit Fields  (errorMessage: {{ $json.error.message }})
```

## Code node content
```javascript
if (true) {
  throw new Error("Something went wrong on purpose");
}
return [{ json: { ok: true } }];
```

## Expected output
Error branch fires (1 item):
```json
{ "errorMessage": "Something went wrong on purpose [line 2]" }
```

## Screenshot
![Workflow 5 result](PASTE_YOUR_IMAGE_URL_HERE)

## What I learned / notes
- Without "Continue on Fail", this workflow would just stop entirely on error — no graceful fallback
- The error output includes extra debug info (line number) automatically
- This is **local** error handling (per node). A separate concept — the **Error Trigger** node — handles **global** failures: it lives in its own dedicated workflow and fires whenever *any* other workflow in the instance fails, useful for centralized alerting (e.g. Slack/email on any workflow crash)

## Status
✅ Completed — error branch fired correctly with expected message — [Date: 6 July 2026]
