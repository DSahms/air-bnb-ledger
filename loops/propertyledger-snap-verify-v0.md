# Loop: PropertyLedger Snap export verify v0

**Slug:** `propertyledger-snap-verify-v0`  
**Category:** Evaluation / field capture  
**Author:** Dave (Ledger-verified)  
**App:** `D:\dev\PropertyLedger-Snap-Demo\`  
**Standard pack:** `property-str-turnover-v1`  
**Related:** `FIELD_CAPTURE_DEMO_v0.md`, Forward Future `full-product-evaluation-loop` (cousin)

---

## Prompt (paste to agent)

Validate a PropertyLedger Snap session export JSON against demo v0 rules. Run the desktop verifier. If it fails, list each rule failure and the minimum fix (app bug vs operator error). If it passes, record pass in STATE.md and stop. Do not set `ledger_verified=true` — demo exports are always draft.

---

## Goal

Prove the phone → export pipeline produces structurally correct draft evidence before any API or PostgreSQL work.

---

## Verify

**Pass when:**

```powershell
python D:\dev\PropertyLedger-Snap-Demo\tools\verify_export.py "PATH\TO\export.json"
```

Exit code **0** and stdout ends with `VERIFY PASS`.

**Rule summary (verifier enforces):**

| Rule | Requirement |
|------|-------------|
| `PACK_CODE` | `standard_pack == property-str-turnover-v1` |
| `EXPORT_VERSION` | `export_version` present |
| `NEVER_STAMP_DEMO` | `event.ledger_verified == false`, top-level and per-observation if present |
| `DRAFT_STATUS` | `event.status == draft` |
| `EVENT_SHAPE` | `event_type`, `event_subtype`, `property_ref`, `inspector_name` |
| `OBSERVATIONS_EXIST` | At least one observation |
| `ROOM_CODE` | Each `room_code` in standard tier set |
| `TAXONOMY` | Each `taxonomy_code` starts with `property.room.` |
| `SEVERITY` | One of none, minor, moderate, major |
| `HUMAN_GATE` | `human_description` non-empty |
| `SHA256` | 64-char hex on each evidence block |
| `TIMESTAMP` | `captured_at` parseable ISO-8601 |

Optional phone-side checks (human, not automated here):

- GPS granted and lat/lon plausible for property
- Photo visually matches room label

---

## Steps

1. Dave sideloads `D:\Beta Apps\PropertyLedger-Snap-v0.1.0.apk` (or rebuild from project).
2. Run one walkthrough: capture **at least one room**, export JSON (share to Files/Drive/USB).
3. Copy export to desktop, e.g. `D:\Air BNB Ledger\samples\`.
4. Run verifier (agent or Dave):

   ```powershell
   python D:\dev\PropertyLedger-Snap-Demo\tools\verify_export.py "D:\Air BNB Ledger\samples\your-export.json"
   ```

5. On **PASS** — note in STATE.md: “Snap export verified on [date]”.
6. On **FAIL** — fix app or re-capture; re-run loop once. If still fail, document blocker in STATE.md.

---

## Stop conditions

| Stop | When |
|------|------|
| **Success** | Verifier exit 0; STATE.md updated |
| **Fail (fixable)** | Verifier lists rules; one fix attempt allowed per run |
| **Fail (blocked)** | Same rule fails twice — stop and assign to Dave (permissions, GPS, camera) |

---

## Why this works

Mirrors production `verification_run`: rules engine, pass/fail, no silent stamp. Demo stays honest — `ledger_verified` never flips true in v0.

---

## Example invocation

```
Run loop propertyledger-snap-verify-v0.
Export file: D:\Air BNB Ledger\samples\propertyledger_snap_2026-06-20.json
Read loop: D:\Air BNB Ledger\loops\propertyledger-snap-verify-v0.md
Update STATE.md on pass.
```
