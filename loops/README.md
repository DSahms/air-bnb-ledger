# Ledger-verified — Local Loop Library

**Folder:** `D:\Air BNB Ledger\loops\`  
**Philosophy:** Same spine as [Forward Future Loop Library](https://signals.forwardfuture.ai/loop-library/) — goal, verify, next step, stop.

These loops are **yours**. They are not published to Forward Future unless you submit them later.

---

## Install the catalog skill (optional)

```powershell
npx skills add Forward-Future/loop-library --skill loop-library -g
```

Use `$loop-library` to find or adapt published loops. Use **this folder** for Ledger-verified-specific playbooks.

---

## Loops on disk

| Slug | File | Use when |
|------|------|----------|
| `propertyledger-snap-verify-v0` | `propertyledger-snap-verify-v0.md` | After phone walkthrough — validate export JSON before trusting it |
| `propertyledger-pocketpal-v1` | `propertyledger-pocketpal-v1.md` | SmolVLM via Pocket Pal share + paste on phone |
| `state-md-executor-v0` | `state-md-executor-v0.md` | Agent handoff — read STATE.md, do one item, verify, update STATE |

---

## How to run a loop in Cursor

Paste into Agent (Composer 2.5):

```
Run loop: propertyledger-snap-verify-v0
Read: D:\Air BNB Ledger\loops\propertyledger-snap-verify-v0.md
Input: [path to exported JSON from phone]
Stop when: verifier passes or blocker documented in STATE.md
```

For unattended work, combine with **Background Agent** and one loop per run. See `CURSOR_KEEP_GOING.md`.

---

## Product mirror

| Agent loop | Production equivalent |
|------------|----------------------|
| Verify export JSON | `verification_run` + rule results |
| Stop on fail | `ledger_verified=false`, status rejected/draft |
| Stop on pass (demo) | Still draft until server — demo never stamps |
