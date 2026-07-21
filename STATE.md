# Ledger-verified — Project State



**Folder:** `D:\Air BNB Ledger\`  

**Last updated:** 2026-06-20  

**Status:** Architecture + field demo v0.2 on disk; API sketch; loops library



---



## What this is



**Ledger-verified** = trust mark + evidentiary compliance ledger for high-value assets.  

**Not** PropertyLedger alone. **Not** `ledger_core` / The Ledger Series. See `LEDGER_VERIFIED.md` naming firewall.



**Face:** premium certified service (Ritz concierge). **Spine:** Phase 5 relational schema — fields from row one, verification gate before stamp.



---



## Documents (authority order)



| Priority | File | Status |

|----------|------|--------|

| 1 | `LEDGER_VERIFIED.md` | Vision, mark, naming firewall |

| 2 | `ARCHITECTURE_v0.md` | ER spec — 30+ entities, R/V/O/I/IDX |

| 3 | `STANDARD_PACK_ag-combine-oil-v1.md` + `.example.json` | Ag combine oil service pack |

| 3b | `STANDARD_PACK_property-str-turnover-v1.md` + `.example.json` | PropertyLedger STR turnover pack |

| 4 | `LEDGER_VERIFIED_Investor_Partner_Brief.md` | Generic multi-vertical pitch |

| 5 | `Air BNB_Business_Summary.md` | PropertyLedger STR wedge |

| 6 | `01`–`05_*.md`, `HoneyWellDo_Concept.md` | GTM lanes / sister concepts |

| 7 | `FIELD_CAPTURE_DEMO_v0.md` | PropertyLedger Snap demo spec |

| 8 | `CURSOR_KEEP_GOING.md` | How to run long agent tasks |

| 9 | `DEVICE_TIER.md` | Phone tier for local AI |

| 10 | `loops/README.md` | Local loop library (agent playbooks) |

| 11 | `API_SURFACE_v0.md` | Draft/submit/verify API sketch |



---



## Field capture demo (PropertyLedger Snap)



| Item | Path |

|------|------|

| Flutter project | `D:\dev\PropertyLedger-Snap-Demo\` |

| APK v0.2 | `D:\Beta Apps\PropertyLedger-Snap-v0.2.0.apk` |

| Build | `flutter build apk --release` |

| Spec | `FIELD_CAPTURE_DEMO_v0.md` |



**v0.2:** ML Kit + **Pocket Pal share → paste** + `ai_backend` in export.  

**Not done:** server upload, verification worker, geofence, embedded SmolVLM.



**Loops:** `loops/propertyledger-snap-verify-v0.md`, `loops/propertyledger-pocketpal-v1.md`, `loops/state-md-executor-v0.md`  

**Verifier:** `D:\dev\PropertyLedger-Snap-Demo\tools\verify_export.py`



---



## Done



- [x] Brand mark locked (**Ledger-verified**)

- [x] Naming firewall vs interview Ledger

- [x] Investor/partner brief (not Airbnb-centric)

- [x] Full data architecture v0 (competitor comparison, verification matrix)

- [x] Example standard pack — ag combine scheduled oil service

- [x] Example standard pack — property STR turnover (move-out / move-in / full)

- [x] Field Capture demo v0 — Flutter app + spec

- [x] Local loops v0 — STATE executor + Snap export verify + Python verifier

- [x] Pocket Pal v1 bridge — share + paste in Snap 0.2.0

- [x] API surface sketch v0 — draft/submit/verify worker



---



## Next (pick one when ready)

- [ ] **Dave on 7T:** sideload v0.2 APK → Pocket Pal loop OR ML Kit room → export JSON → `verify_export.py` pass

- [ ] Founder doc (Dave voice — separate from investor brief)

- [ ] Physical schema — PostgreSQL DDL from `ARCHITECTURE_v0`

- [ ] Expert partner one-pager template (Harley, marine, construction)



---



## Principle (do not drift)



Only `ledger_verified = true` rows feed buyer search and reports. Fail spec → draft/rejected, never silent stamp.

