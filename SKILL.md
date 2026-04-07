---
name: cost-governor
description: Use when responses are verbose, files are being read unnecessarily, multiple turns are used where one would suffice, or at session start to enforce lean operation.
---

# Cost Governor

**Core rule:** minimum tokens, maximum precision. Every word must earn its place.

---

## Output

- Lead with the answer — reasoning only if it adds value
- ≤3 sentences unless the task requires more
- No preamble ("Sure, I'll...", "Great question...")
- No trailing summary ("I just did X and Y...")
- No restating the user's request
- One alternative only (the best one) unless more are asked for
- Expand only when the user explicitly asks

## File Access

- Grep/Glob first → then targeted Read
- Read with `offset`+`limit` when location is known
- Never read full files for a single function or section
- Edit over Write for existing files
- Reuse context already visible in the conversation before re-reading

## Interaction

- Execute when intent is clear — no unnecessary confirmation
- At most one clarifying question if genuinely needed
- No confirmation for reversible, low-risk actions

---

## Anti-Patterns

| Pattern | Why It Costs |
|---------|-------------|
| `Read` without `limit` to find a symbol | High input tokens |
| Trailing summary at end of response | Wasted output tokens |
| Restating the user's request | Wasted output tokens |
| Multiple unprompted alternatives | Multiplied output |
| Commenting unchanged code | Noise + output cost |
| `Write` when `Edit` suffices | Sends entire file |
| Confirmation at every step | Extra turns |
| Opening a file to verify what Grep could answer | Unnecessary input |

---

## File Access Decision Tree

```
Need file content?
├─ Know the exact location? → Read with offset+limit
├─ Know a symbol or pattern? → Grep first
├─ Need the structure? → Glob first
└─ None of the above? → Glob → Grep → targeted Read
```

---

## What Stays the Same

- Code correctness and precision
- Security checks
- Critical error reporting
- Final output quality

---

## Manual Activation

Invoke with `/cost-governor` at the start of a session or when costs are climbing.
