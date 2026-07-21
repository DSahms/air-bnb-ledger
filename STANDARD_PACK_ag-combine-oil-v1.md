# Standard Pack — Ag Combine Oil Service v1

**Pack code:** `ag-combine-oil-v1` · **Version:** 1.0.0  
**Maps to:** `ARCHITECTURE_v0.md` · **Machine-readable:** `standard_packs/ag-combine-oil-v1.example.json`

This is the **first concrete pack** — proof that the ER spec isn’t abstract. Expert partners (Deere tech, independent ag shop) would **author** revisions; we **enforce** them in the rules engine.

---

## What this pack governs

| Item | Value |
|------|--------|
| Asset class | `ag` |
| Asset subclass | `combine` |
| Event type | `maintenance` |
| Event subtype | `engine_oil_change` |
| Meter | Engine **hours** (not miles) |
| Resale story | Buyer sees verified oil history → **$10k–$20k** spread on high-hour combines (farmer word-of-mouth) |

**Ledger-verified stamp** on this event means: rules in this pack passed, not “shop said they changed the oil.”

---

## How the pack loads into the database

| JSON / pack section | Database home |
|---------------------|---------------|
| `pack.code`, `version`, `title`, dates | `standard_pack` row |
| Full JSON document | `standard_pack.rules_json` |
| `material_specs[]` | `standard_material_spec` rows (one per `spec_code`) |
| `procedure_steps[]` | `standard_procedure_step` rows |
| `verification_rules[]` | Evaluated by rules engine → `verification_run` + `verification_rule_result` |

**Day 1:** ship pack JSON + seed rows. **No new columns** when rules tighten — flip flags in pack v1.1.

---

## Example walkthrough — one verified oil change

**Scene:** 2022 John Deere S780, 1,850 engine hours, farm in Lancaster County. Certified shop **HoneyWellDo Ag** (example org). Tech **Mike R.**

### 1. Registry (before the job)

| Table | What gets written |
|-------|-------------------|
| `asset` | `asset_class=ag`, `asset_subclass=combine`, `manufacturer=John Deere`, `model=S780`, `value_tier=premium` |
| `asset_identifier` | `id_type=serial`, Deere serial number |
| `site` | Farm address + lat/long + `geofence_radius_m=200` |
| `party` | Owner (farmer LLC) |
| `asset_ownership` | Owner ↔ asset |
| `certified_org` | Shop, `certified_pack_ids` includes `ag-combine-oil-v1` |
| `certified_tech` | Mike R., linked to shop |

### 2. Procurement (day before — **predates** work)

| Table | What gets written |
|-------|-------------------|
| `procurement_record` | NAPA invoice 2026-06-10, receipt scanned |
| `procurement_line` | 12 qt Shell Rotella 15W-40, `spec_code=engine_oil_primary` |
| `procurement_line` | 1× OEM filter, `spec_code=engine_oil_filter` |
| `evidence_object` | Receipt PDF/photo, `content_hash_sha256`, `photo_phase=receipt` |

**Rule `PROCUREMENT_PREDATES_EVENT`:** `purchase_timestamp` **<** `ledger_event.started_at`.

### 3. Field event (draft → submitted)

| Table | What gets written |
|-------|-------------------|
| `ledger_event` | `event_type=maintenance`, `event_subtype=engine_oil_change`, `standard_pack_id`, `verification_status=draft` → `submitted` |
| | `started_at`, `completed_at`, `duration_minutes` |
| | `meter_type=hours`, `meter_reading=1850` |
| | `capture_site_id`, GPS, `geofence_match=true` |
| `event_procedure_step` | One row per pack step — `METER_CAPTURE`, `PHOTO_BEFORE`, … completion flags |
| `event_material_use` | Links to procurement lines; `quantity_used`, `brand`, `sku`, `spec_pass` |
| `event_evidence_link` | Before / during / after / meter photos → `evidence_object` |
| `event_environment` | Weather API enrichment for job date (additive) |
| `event_attestation` | Tech signature, `signed_at` |

### 4. Verification engine

| Table | What gets written |
|-------|-------------------|
| `verification_run` | `engine_version`, `overall_pass=true/false`, `rules_evaluated_json` |
| `verification_rule_result` | One row per rule: `GEOFENCE_MATCH`, `PROCUREMENT_PREDATES_EVENT`, … |

**If `overall_pass=true` and attestation present:**

```text
ledger_event.verification_status = verified
ledger_event.ledger_verified = true
ledger_event.verified_at = <timestamp>
```

**If any rule fails:** stay `rejected` or `draft` — **no stamp**. Waiver = separate attested event, never silent.

### 5. Buyer / resale (later)

| Table | What gets written |
|-------|-------------------|
| `report_order` | Auction house or buyer pays for history |
| `report_snapshot` | PDF/data package — **only** `ledger_verified=true` events |

---

## Verification rules for this pack (by tier)

| Rule | Standard | Premium | Ultra |
|------|:--------:|:-------:|:-----:|
| `TECH_CERTIFIED_FOR_PACK` | ✓ | ✓ | ✓ |
| `METER_READING_PRESENT` | ✓ | ✓ | ✓ |
| `DURATION_LOGGED` | ✓ | ✓ | ✓ |
| `GEOFENCE_MATCH` | — | ✓ | ✓ |
| `PROCUREMENT_PREDATES_EVENT` | — | ✓ | ✓ |
| `SKU_IN_ALLOW_LIST` | — | ✓ | ✓ |
| `PHOTO_BEFORE_AFTER` | — | ✓ | ✓ |

**Standard tier:** trusted tech + meter + duration — good for pilot.  
**Premium / ultra:** full evidentiary chain (your “nobody can dispute the oil” layer).

---

## Field mapping cheat sheet (oil change)

| Your requirement | Primary table.column |
|------------------|----------------------|
| What oil — brand, grade, SKU | `event_material_use` + `procurement_line` |
| How much — quarts | `event_material_use.quantity_used` |
| Purchased when / where | `procurement_record.purchase_timestamp`, vendor, receipt hash |
| Performed when / where | `ledger_event.started_at`, `capture_site_id`, GPS |
| Geofence at farm | `ledger_event.geofence_match` |
| By whom | `certified_tech`, `event_attestation` |
| How long | `started_at`, `completed_at`, `duration_minutes` |
| Photos before/during/after | `evidence_object.photo_phase`, `event_evidence_link` |
| Weather / region | `event_environment` |
| Met spec? | `event_material_use.spec_pass`, `verification_run.overall_pass` |

---

## Partner workflow (how an expert uses this)

1. **Review pack** — adjust `allowed_brands`, OEM filter SKUs, interval hours.  
2. **Sign off** — pack version bumps (1.0.0 → 1.1.0).  
3. **Train techs** — procedure steps = field app checklist.  
4. **Perform work** — 25-minute standard, not 15.  
5. **Ledger accepts or rejects** — no manual override without waiver event.

---

## Revision notes (v1.1 candidates)

- OEM-specific filter SKU allow-lists per `asset.manufacturer`  
- Interval waiver attestation template  
- `ultra` tier: oil sample lab result attachment (future `evidence_object` type)

---

*Ledger-verified · ag-combine-oil-v1 · See `ARCHITECTURE_v0.md` for full schema.*
