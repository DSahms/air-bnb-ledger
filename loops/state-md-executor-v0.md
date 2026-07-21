# Loop: STATE.md executor v0

**Slug:** `state-md-executor-v0`  
**Category:** Operations / handoff  
**Author:** Dave (Ledger-verified)  
**Related:** `CURSOR_KEEP_GOING.md`, Forward Future `fresh-clone-loop` (concept cousin)

---

## Prompt (paste to agent)

Whenever work continues on Ledger-verified, read `D:\Air BNB Ledger\STATE.md` and execute **exactly one** unchecked item under **Next**. Do not start a second item in the same run. Verify the deliverable on disk, update STATE.md (move item to Done or document blocker), then stop.

---

## Goal

Ship one finishable artifact per agent run without re-debating vision or mixing unrelated tasks.

---

## Verify

**Pass when all are true:**

1. The chosen STATE.md item has a concrete output path or command result on disk.
2. STATE.md **Last updated** date changed and the item is checked Done or Blocked with reason.
3. No naming drift vs `LEDGER_VERIFIED.md` firewall (not `ledger_core`, not interview Ledger).
4. Git was not committed unless Dave explicitly asked.

**Fail when:** item is vague, blocked >10 minutes with no blocker note, or output cannot be found at the path claimed.

---

## Steps

1. Read `STATE.md`, `LEDGER_VERIFIED.md` (naming firewall only if touching brand).
2. Pick the **first** unchecked **Next** item unless Dave named a different one in the message.
3. Execute with sensible defaults; record assumptions in STATE.md if needed.
4. Verify output exists (file, APK, script exit 0, etc.).
5. Update STATE.md: check Done, or add **Blocked:** line with what Dave must decide.
6. Stop. Do not chain the next checkbox.

---

## Stop conditions

| Stop | When |
|------|------|
| **Success** | One item Done + STATE.md updated |
| **Blocked** | Blocker written in STATE.md; no guessing on business decisions |
| **Scope creep** | Second Next item started — abort and revert narrative |

---

## Why this works

Open-ended “keep going” chats die. One checkbox + disk verify + explicit stop matches how verification workers will behave in production.

---

## Example invocation

```
Run loop state-md-executor-v0.
Read D:\Air BNB Ledger\loops\state-md-executor-v0.md.
If Dave did not specify a task, take the first unchecked Next item in STATE.md.
```
