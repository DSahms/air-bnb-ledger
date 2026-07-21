# Cursor — How to Keep an Agent Working (No Fake “YOLO”)

**For:** Dave · **Last updated:** 2026-06-17

You asked for a mode where the agent **keeps going** while you’re at work without typing “continue” every five minutes. Here’s the honest map.

---

## What Cursor does *not* do today

There is **no true YOLO mode** in a normal chat where the agent runs for hours unattended with zero checkpoints. A single Agent chat can **pause, summarize, or hit turn limits** — especially after you leave. That’s why it feels like it stops ~5 minutes in.

**Composer 2.5 vs Auto**

| Use | Model |
|-----|--------|
| Building APKs, multi-file code, Flutter, docs with edits | **Composer 2.5** (pin it) |
| Quick questions, one-off lookups | **Auto** |

For “build the demo while I’m at work,” **Composer 2.5 + Background Agent** beats Auto.

---

## What actually works

### 1. Background / Cloud Agent (best for long builds)

1. Open **Agent** (not inline chat only).
2. Give **one concrete deliverable** in the first message, e.g.  
   *“Build PropertyLedger Snap demo APK at D:\dev\PropertyLedger-Snap-Demo, update FIELD_CAPTURE_DEMO_v0.md, build release APK, write result to STATE.md.”*
3. Start as **Background Agent** (runs while IDE closed / you’re away).
4. One task per agent — don’t mix “fix SSD + build APK + Gemma” in one thread.

### 2. STATE.md handoff (you already use this)

Every session should **read** `D:\Air BNB Ledger\STATE.md` first and **write** what finished + what’s next. When a chat dies, open a **new** agent with:

> Continue from STATE.md and FIELD_CAPTURE_DEMO_v0.md. Do not re-discuss vision. Execute next unchecked item.

That’s your disk-based “continue” — not typing continue in a dead thread.

### 3. Local loops + Cursor Automations

**Loops** = bounded playbooks with verify/stop (see `loops/README.md`).  
For recurring “check CI → fix → update STATE” workflows, use **Cursor Automations**. Good for babysitting PRs; use **loops** for field export verify and STATE handoffs.

Install Forward Future catalog (optional):

```powershell
npx skills add Forward-Future/loop-library --skill loop-library -g
```

### 4. Split work into finishable chunks

Instead of “do everything,” queue:

1. Scaffold app ✓  
2. Build APK ✓  
3. Pocket Pal hook  
4. API draft endpoint  

Each chunk = one Background Agent run = natural stop point with artifact on disk.

---

## Prompt template (copy/paste when leaving)

```
You have full control until this task is DONE on disk.

Read first:
- D:\Air BNB Ledger\STATE.md
- D:\Air BNB Ledger\LEDGER_VERIFIED.md (naming firewall only)

Task: [ONE DELIVERABLE]

Rules:
- Write all outputs to disk paths listed above
- Update STATE.md when finished
- Do not commit git unless I ask
- If blocked >10 min, document blocker in STATE.md and stop cleanly
- Do not ask me questions — pick sensible defaults and note assumptions in STATE.md

Done when: [measurable exit criteria]
```

---

## OnePlus upgrade (quick answer)

| Phone | Verdict |
|-------|---------|
| 9 Pro | Decent; Snapdragon 888 ran **hot** |
| **10** | Skip — thermal / value weak spot in the line |
| **11** | **Good step up** — 8 Gen 2, much better thermals than 9/10 |
| **12** | **Best for local AI** — 8 Gen 3, better NPU, **16 GB** option, improved cooling |

If budget allows and you care about on-device vision (Pocket Pal, future field app): **12 > 11 >> skip 10**. Your **8T 12 GB** is still a fine slim daily driver if you rebuy one for the chassis.

---

## This session delivered

- Flutter demo: `D:\dev\PropertyLedger-Snap-Demo\`
- Spec: `FIELD_CAPTURE_DEMO_v0.md`
- Device notes: `DEVICE_TIER.md`
- Build: run `flutter build apk --release` in project folder

When you’re back: sideload APK to 7T, walk one fake turnover, export JSON, confirm it matches the schema in the spec.
