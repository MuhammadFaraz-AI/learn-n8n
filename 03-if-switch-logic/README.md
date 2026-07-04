# 03 — IF vs Switch (multi-branch logic)

## ⚠️ Before you look at workflow.json
Try building this from the instructions below **first, on your own**, without opening `workflow.json`.
Run it, compare your output to "Expected Output" below.
Only open `workflow.json` afterward to verify your setup.

## Goal
Understand when to use **Switch** instead of **IF** — specifically, when you need 3+ possible outcomes instead of just true/false.

## Concepts covered
- Switch node in **Rules mode**, with multiple output branches
- Each branch is a separate physical output — items are routed, not duplicated
- Why you need one node per branch downstream (a single node can't "listen" to multiple outputs differently)
- Reading item counts on connections to verify routing logic

## Workflow structure

```
When clicking 'Execute workflow' → Code in JavaScript → Switch (mode: Rules) ─┬─→ Edit Fields   (small)
                                                                                ├─→ Edit Fields1  (medium)
                                                                                └─→ Edit Fields2  (large)
```

## Nodes

| Node | Purpose |
|------|---------|
| When clicking 'Execute workflow' | Manual trigger |
| Code in JavaScript | Generates 4 test orders with `customer` and `amount` |
| Switch (mode: Rules) | Routes each item to small / medium / large based on `amount` |
| Edit Fields | Runs only on "small" branch — sets `tier: "small"` |
| Edit Fields1 | Runs only on "medium" branch — sets `tier: "medium"` |
| Edit Fields2 | Runs only on "large" branch — sets `tier: "large"` |

## Code node content

```javascript
return [
  { json: { customer: "Sara", amount: 25 } },
  { json: { customer: "Bilal", amount: 120 } },
  { json: { customer: "Zara", amount: 550 } },
  { json: { customer: "Omar", amount: 15 } }
];
```

## Switch rules
- **small**: `amount < 50`
- **medium**: `amount >= 50 AND amount < 200`
- **large**: `amount >= 200`

## Expected output

| Branch | Items | Customers |
|--------|-------|-----------|
| small | 2 | Sara (25), Omar (15) |
| medium | 1 | Bilal (120) |
| large | 1 | Zara (550) |

## Screenshot

![Workflow 3 result]03-if-switch-logic/workflow-3-result.png

*(Replace the URL above with your uploaded screenshot's raw GitHub URL — see repo root README for instructions on how to get it.)*

## How to run this
1. Try building it yourself from the instructions above first
2. Compare your item counts per branch to "Expected output"
3. Only then import `workflow.json` to double-check your rules/expressions

## What I learned / notes
- Switch physically splits the data flow into separate wires — each downstream node only ever sees its own branch's items
- IF is for 2 outcomes; Switch is for 3+, and keeps the canvas readable instead of nesting IFs inside IFs
- You can verify routing logic just by reading the item-count labels on each connection line, without opening a single node

## Status
✅ Completed — output matched expected exactly (small: 2, medium: 1, large: 1) — [Date: 4 July 2026]
