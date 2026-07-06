# 04 — Loop Over Items (Explicit Batching)

## ⚠️ Before you look at workflow.json
Try building this yourself first from the instructions below. Only open `workflow.json` afterward to verify.

## Goal
Understand explicit, controlled looping — as opposed to the automatic per-item processing seen in workflows 02/03. Used when you need to control batch size/order, e.g. calling a rate-limited API one item at a time.

## Concepts covered
- **Loop Over Items** (Split In Batches) node and its two outputs: `loop` and `done`
- Building a loop by connecting a node's output back into the Loop node's input
- Understanding that the `done` output carries **all accumulated items**, not just a single signal

## Workflow structure
```
Manual Trigger → Code (5 items) → Loop Over Items ─┬─(loop)─→ Edit Fields (status: processed) ──┐
                                                     │                                            │
                                                     └────────────── (loops back in) ◄────────────┘
                                                     └─(done)─→ Edit Fields (summary: "all tasks processed")
```

## Code node content
```javascript
return [
  { json: { id: 1, task: "Send email A" } },
  { json: { id: 2, task: "Send email B" } },
  { json: { id: 3, task: "Send email C" } },
  { json: { id: 4, task: "Send email D" } },
  { json: { id: 5, task: "Send email E" } }
];
```

## Expected output
5 items on the "done" branch, each with:
```json
{ "summary": "all tasks processed" }
```

## Screenshot
![Workflow 4 result](PASTE_YOUR_IMAGE_URL_HERE)

## What I learned / notes
- The loop body ran once per item (5 times), visible via execution highlighting
- The `done` output isn't a single "finished" signal — it carries every item that passed through the loop
- This node matters when order/pacing matters (e.g. avoiding API rate limits), unlike IF/Switch which process everything in parallel

## Status
✅ Completed — 5 items, all with `summary: "all tasks processed"` — [Date: 6 July 2026]
