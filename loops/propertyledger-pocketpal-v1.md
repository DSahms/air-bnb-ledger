# Loop: Pocket Pal describe v1 (manual bridge)

**Slug:** `propertyledger-pocketpal-v1`  
**Category:** Field capture / on-device AI  
**Requires:** Pocket Pal AI installed, SmolVLM2 (or vision model) loaded on phone  
**App version:** PropertyLedger Snap ≥ 0.2.0

---

## Prompt

Run a single-room PropertyLedger capture using Pocket Pal for describe-this-picture. Follow the share → describe → paste → confirm → export → desktop verify sequence. Stop after one room passes `verify_export.py` or document blocker.

---

## Steps (on 7T)

1. Install/update APK: `D:\Beta Apps\PropertyLedger-Snap-v0.2.0.apk`
2. Open Pocket Pal — confirm SmolVLM2 500M (or vision model) loaded.
3. PropertyLedger Snap → start session → open one room (e.g. kitchen).
4. **Take photo**
5. Tap **Share → Pocket Pal** — pick Pocket Pal from share sheet.
6. In Pocket Pal: send the pre-filled turnover prompt with image; wait for response.
7. **Copy** Pocket Pal response to clipboard.
8. Return to PropertyLedger Snap → **Paste clipboard** → edit description → **Confirm**
9. Export session JSON → copy to PC.
10. Desktop:

    ```powershell
    python D:\dev\PropertyLedger-Snap-Demo\tools\verify_export.py "PATH\TO\export.json"
    ```

---

## Verify

| Check | Pass |
|-------|------|
| Export JSON | `ai_backend` is `pocket_pal_paste` or `pocket_pal_share` |
| Human gate | `human_description` edited from raw paste |
| Verifier | Exit code 0 |
| Stamp | `ledger_verified` still false everywhere |

---

## Stop

- **Success:** Verifier pass + `ai_backend` proves Pocket Pal path used
- **Blocked:** Pocket Pal not in share sheet, vision model greyed out, or paste empty twice

---

## Why not full intent callback?

Pocket Pal does not expose a documented return intent to third-party apps. v1 is **share + paste** — still proves human gate + richer describe than ML Kit. v2 options: embedded llama.cpp, Pocket Pal deep link if added upstream, or M40 batch QA.
