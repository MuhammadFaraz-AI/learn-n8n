# 01 — Expressions Basics

## Goal
Understand how n8n expressions work: pulling data from a previous node's JSON output and building a new value dynamically, instead of typing static text.

## Concepts covered
- `{{ }}` expression syntax
- `$json` — accessing the current item's data
- How data flows from one node to the next (each node only sees what the node before it output)
- Data types: String vs Number, and how n8n auto-converts them inside string expressions

## Workflow structure

```
Manual Trigger → Edit Fields (set name, age, city) → Edit Fields1 (build greeting via expression)
```

## Nodes

| Node | Purpose |
|------|---------|
| Manual Trigger | Starts the workflow on demand (for testing) |
| Edit Fields | Sets `name`, `age`, `city` as static values |
| Edit Fields1 | Builds a `greeting` field using an expression that reads the fields above |

## The expression used

```
"Hello {{ $json.name }}, you are {{ $json.age }} and live in {{ $json.city }}"
```

## Input

| name | age | city |
|------|-----|------|
| ali | 28 | karachi |

## Output

```json
{
  "greeting": "Hello ali, you are 28 and live in karachi"
}
```

## How to run this

1. Import `workflow.json` into n8n
2. Click **Execute Workflow**
3. Check the output on the last node — it should match the output above

## What I learned / notes

- Each node's output only carries forward what that node explicitly sets (or passes through)
- Numbers get auto-converted to text when used inside a string expression
- This is the foundation for every other concept in n8n — loops, conditionals, and API calls all depend on understanding this data flow

## Status
✅ Completed — [Date: 2 July 2026]
