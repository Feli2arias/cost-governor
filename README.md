# cost-governor

> A Claude Code skill that enforces lean, token-efficient behavior — without sacrificing output quality.

![Claude Code](https://img.shields.io/badge/Claude_Code-skill-blue)
![License](https://img.shields.io/badge/license-MIT-green)

---

## The Problem

Claude Code is powerful. It's also expensive when left unchecked.

Without guardrails, Claude will:

- Read 800-line files to answer a question about one function
- Open every response with *"Sure! Let me help you with that..."*
- Summarize everything it just did — at the end of every message
- Ask clarifying questions when the intent was obvious
- Rewrite entire files when editing 3 lines would do

Every one of those habits burns tokens you're paying for.

---

## Before / After

**Without cost-governor**
```
You: What does the `parseDate` function do?

Claude: Sure! I'd be happy to help you understand that function. Let me start
by reading the full file to get the complete context first...
[reads 743 lines]
The `parseDate` function, which I found on line 47, parses date strings into
Date objects. It supports several formats including ISO 8601, Unix timestamps...
[180 more words]
In summary, I've reviewed the file and the function handles date parsing.
```

**With cost-governor**
```
You: What does the `parseDate` function do?

Claude: [Grep → 12 lines]
Parses ISO 8601 strings into Date objects. Returns null on invalid input.
```

---

## Install

```bash
# 1. Create the skill directory
mkdir -p ~/.claude/skills/cost-governor

# 2. Download the skill
curl -o ~/.claude/skills/cost-governor/SKILL.md \
  https://raw.githubusercontent.com/Feli2arias/cost-governor/main/SKILL.md

# 3. Activate in Claude Code
/cost-governor
```

---

## What It Enforces

| Rule | Saves |
|------|-------|
| Grep/Glob before Read | Input tokens — avoids reading whole files |
| Read with `offset`+`limit` | Input tokens — reads only what's needed |
| No preamble or trailing summary | Output tokens — removes filler |
| Execute when intent is clear | Turns — skips unnecessary back-and-forth |
| Edit over Write for existing files | Output tokens — sends diffs, not full content |

---

## What It Doesn't Change

Correctness, security checks, and error reporting are never compressed.

---

## License

MIT
