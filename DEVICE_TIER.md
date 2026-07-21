# Device Tier — Field Capture & Local AI

**Last updated:** 2026-06-17

---

## Current lab rig

| Device | RAM | OS | Role |
|--------|-----|-----|------|
| OnePlus 7T | ~7.8 GB | Android 15, rooted (Magisk) | Demo sideload, Pocket Pal / SmolVLM R&D |
| OnePlus 8T (broken screen) | 12 GB | — | Chassis reference; rebuy candidate for slim/cool daily |

**Production field reps should use non-rooted devices** with Play Integrity. Rooted 7T = great lab, bad trust anchor.

---

## OnePlus upgrade ladder (local AI + thermals)

| Model | Chip | Thermals (rough) | Local AI |
|-------|------|------------------|----------|
| 9 Pro | SD 888 | Ran hot | OK for 500M VLM |
| **10** | SD 8 Gen 1 | **Weakest** — skip | Not worth upgrade |
| **11** | SD 8 Gen 2 | **Big improvement** | Good for SmolVLM 500M–2.2B |
| **12** | SD 8 Gen 3 | Best cooling in line | **16 GB** option; best phone tier for on-device vision |

**Recommendation:** Skip 10. **11** if budget tight. **12 (16 GB)** if field AI is a product pillar.

---

## Model ↔ device matrix

| Model | Min practical phone | Notes |
|-------|---------------------|-------|
| ML Kit labels | Any | PropertyLedger Snap v0 demo |
| SmolVLM2 500M Q8 | 7T / 8 GB class | Pocket Pal sweet spot today |
| SmolVLM2 2.2B Q4 | 12 GB RAM | OnePlus 8T, 11, 12 |
| Gemma 4 12B vision | Desktop GPU (M40) | QA / dispute review, not primary capture |

---

## App targets

| Tier | APK | AI backend |
|------|-----|------------|
| v0 | PropertyLedger Snap (Flutter) | ML Kit |
| v1 | Same + intent hook | Pocket Pal / SmolVLM |
| v2 | Upload + verify | Server rules engine |
