# 02 — Loops & Arrays Basics

## ⚠️ Before you look at workflow.json
Try building this from the instructions below **first, on your own**, without opening `workflow.json`.
Run it, check your own output against the "Expected Output" section.
Only open `workflow.json` afterward to verify your node setup / expressions were correct, or to see where you went wrong.
The value of this repo is in doing the exercise, not copying the file — treat `workflow.json` as the answer key, not the starting point.

## Goal
Understand that n8n processes data as a **list of items**, and that most nodes automatically run once per item — no manual loop required.

## Concepts covered
- Items as the core data unit in n8n (even 1 result = 1 item)
- Generating test data with the **Code** node
- **IF node** filtering items automatically, item by item
- Expressions referencing multiple fields on the same item (`$json.name`, `$json.price`, `$json.stock`)

## Workflow structure

```
Manual Trigger → Code (generate 4 test items) → IF (stock > 0) → Edit Fields (build label)
```

## Nodes

| Node | Purpose |
|------|---------|
| Manual Trigger | Starts the workflow on demand |
| Code | Generates 4 hardcoded items (Apples, Bananas, Mangoes, Grapes) with `name`, `price`, `stock` |
| IF | Keeps only items where `stock > 0` |
| Edit Fields | Adds a `label` field combining name, price, and stock into one string |

## Code node content

```javascript
return [
  { json: { name: "Apples", price: 3, stock: 10 } },
  { json: { name: "Bananas", price: 1, stock: 0 } },
  { json: { name: "Mangoes", price: 5, stock: 7 } },
  { json: { name: "Grapes", price: 4, stock: 0 } }
];
```

## IF node condition
`{{ $json.stock }}` **greater than** `0`

## Edit Fields expression
```
{{ $json.name }} - ${{ $json.price }} ({{ $json.stock }} left)
```

## Expected output

4 items go in → 2 items come out (Bananas and Grapes are dropped because `stock = 0`):

```json
[
  { "label": "Apples - $3 (10 left)" },
  { "label": "Mangoes - $5 (7 left)" }
]
```

## How to run this
1. Try building it yourself from the instructions above first
2. Compare your output to "Expected output"
3. Only then import `workflow.json` to double-check your node setup/expressions

## What I learned / notes
- You never wrote a "for loop" — the IF node and Edit Fields node each ran once per item, automatically
- This is different from typical programming, where you'd usually write an explicit loop
- Filtering happens per-item: n8n doesn't need to know the size of the list in advance
- Next concept (Loop Over Items / Split In Batches) is for when you need *explicit* control over batching — e.g. rate-limited API calls

## Status
✅ Completed — my output matched expected output exactly (2 items: Apples, Mangoes) — [Date: 4 July 2026]
