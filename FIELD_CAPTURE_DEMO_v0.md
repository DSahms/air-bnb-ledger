# Field Capture Demo v0 — PropertyLedger Snap

**Last updated:** 2026-06-17  
**App path:** `D:\dev\PropertyLedger-Snap-Demo\`  
**Standard pack:** `property-str-turnover-v1`  
**Enterprise mark:** Ledger-verified (export is **draft only**)

---

## Purpose

Prove the **field wedge** on a real phone before PostgreSQL, API, or verification workers exist:

> Photo → on-device describe → **human confirms** → hash + GPS + timestamp → draft JSON aligned with `ARCHITECTURE_v0`.

This is **not** a maintenance photo app. AI text is **input to human judgment**, not a trust stamp.

---

## Demo APK

| Item | Value |
|------|--------|
| Project | `D:\dev\PropertyLedger-Snap-Demo\` |
| Sideload | `D:\Beta Apps\PropertyLedger-Snap-v0.2.0.apk` |
| Build | `flutter build apk --release` |
| On-device AI (v0) | Google ML Kit image labeling |
| On-device AI (v1) | Pocket Pal — **Share → SmolVLM → Paste clipboard** |
| On-device AI (v2 target) | Embedded SmolVLM or Pocket Pal return intent |

---

## User flow

1. **Start session** — property ref, inspector name, event subtype (`move_out` / `move_in` / `turnover_full`).
2. **Room rounds** — standard tier rooms from pack: entry, living, kitchen, bedroom_1, bathroom_1.
3. **Per room capture**
   - Take photo
   - **ML Kit** *or* **Share → Pocket Pal** (SmolVLM) → **Paste clipboard**
   - Inspector edits text, picks `severity` and `taxonomy_code`
   - App records SHA-256, UTC timestamp, GPS if granted, `ai_backend`
4. **Export** — single JSON file via share sheet. All rows: `ledger_verified: false`, `status: draft`.

---

## JSON export (draft)

```json
{
  "export_version": "0.1.0",
  "standard_pack": "property-str-turnover-v1",
  "event": {
    "event_type": "inspection",
    "event_subtype": "move_out",
    "property_ref": "STR-Wildwood-Unit-12",
    "inspector_name": "Demo Inspector",
    "ledger_verified": false,
    "status": "draft"
  },
  "observations": [
    {
      "room_code": "kitchen",
      "taxonomy_code": "property.room.appliances.condition",
      "severity": "minor",
      "human_description": "Stove, countertop, kitchenware visible",
      "evidence": {
        "sha256": "...",
        "captured_at": "2026-06-17T...",
        "gps": { "lat": 39.0, "lon": -74.8 }
      },
      "ai_labels": [{ "text": "Stove", "confidence": 0.82 }]
    }
  ]
}
```

### Maps to architecture tables (future)

| Export field | Table / column |
|--------------|----------------|
| `event.*` | `ledger_event` |
| `observations[].taxonomy_code`, `severity`, `room_code` | `event_condition_observation` |
| `evidence.sha256`, GPS, timestamp | `evidence_object` + `event_evidence_link` |
| `ai_labels` | Provenance metadata (audit trail, not verification) |
| Human edit + later sign-off | `event_attestation` |
| Rules engine pass | `verification_run` → `ledger_verified=true` |

---

## Verification loop (desktop)

After phone export, run:

```powershell
python D:\dev\PropertyLedger-Snap-Demo\tools\verify_export.py "PATH\TO\export.json"
```

Full agent playbook: `D:\Air BNB Ledger\loops\propertyledger-snap-verify-v0.md`

Example pass file: `D:\Air BNB Ledger\samples\propertyledger_snap_example_pass.json`

---

Only server-side rules + attestation set `ledger_verified=true`. The demo never stamps.

Required rules for production move-out (from standard pack): neutral inspector attestation, geofence, required rooms photographed, taxonomy complete, chain of custody, duration logged.

---

## AI ladder (phone)

| Tier | Model | Role |
|------|-------|------|
| v0 demo | ML Kit labels | Fast, offline, proves capture + human gate |
| Field v1 | SmolVLM2 500M Q8 | “Describe this picture” on 7T / 8T class phones |
| Field v2 | SmolVLM2 2.2B Q4 | Richer captions; 12 GB RAM phones |
| Desktop batch | Gemma 4 12B vision (M40) | QA review, dispute resolution, not primary capture |

---

## Device policy

| Device | Role |
|--------|------|
| OnePlus 7T rooted | R&D, Pocket Pal experiments, demo sideload |
| Non-rooted rep phone | Production capture + Play Integrity |
| OnePlus 11 / 12 | Upgrade path for local AI + thermals (skip 10) |

See `DEVICE_TIER.md`.

---

## Next build steps

- [ ] POST export to draft API endpoint
- [ ] Pocket Pal intent / SmolVLM bridge for description field
- [ ] Premium room checklist from pack JSON
- [ ] Compare-to-prior UI for `move_in` subtype
