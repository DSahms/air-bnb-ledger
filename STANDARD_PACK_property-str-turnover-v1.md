# Standard Pack — Property STR Turnover v1

**Pack code:** `property-str-turnover-v1` · **Version:** 1.0.0  
**Vertical brand:** PropertyLedger · **Enterprise mark:** Ledger-verified  
**Maps to:** `ARCHITECTURE_v0.md` · **JSON:** `standard_packs/property-str-turnover-v1.example.json`

First **property wedge** pack — between-tenant walkthroughs for Airbnb/VRBO in Philly, South Jersey, Atlantic City shore, and Poconos. Same spine as ag/marine; different taxonomy and legal story (**neutral third party**, deposit disputes, archive search).

---

## What this pack governs

| Item | Value |
|------|--------|
| Asset class | `property` |
| Asset subclass | `str_unit` |
| Event type | `inspection` |
| Subtypes | `move_out`, `move_in`, `turnover_full` |
| Face of service | PropertyLedger walkthrough crew |
| Stamp meaning | *This turnover was documented to standard — not landlord self-reporting* |

**Moat (from your brief):** walkthroughs feed the archive; **Ledger-verified** is what makes entries trustworthy at resale, insurance, and pay-per-report search.

---

## Three event shapes

| Subtype | When | Baseline required? |
|---------|------|-------------------|
| **move_out** | Guest departed | No — **creates** outgoing baseline |
| **move_in** | Before next guest | **Yes** — links to last verified `move_out` |
| **turnover_full** | Same visit out + in | Outgoing section creates baseline; incoming compares |

Property management **anchor clients** often bill **per event** ($75–$200/turnover); owner-direct STR uses **monthly subscription** ($300–350/property). Same pack, same ledger.

---

## How this maps to `ARCHITECTURE_v0`

| Pack concept | Table(s) |
|--------------|----------|
| Property as permanent record | `asset`, `asset_identifier` (parcel, listing ID), `site` |
| Owner / PM / tenant parties | `party`, `asset_ownership` |
| PropertyLedger field org | `certified_org`, `certified_tech` |
| Walkthrough event | `ledger_event` |
| Room-by-room findings | `event_condition_observation` (`taxonomy_code`, `severity`, `comparison_to_prior`) |
| Photos (GPS, timestamp, hash) | `evidence_object`, `event_evidence_link`, `chain_of_custody_log` |
| Neutral inspector sign-off | `event_attestation` (`attestor_type=tech`, neutrality statement) |
| Rules pass/fail | `verification_run`, `verification_rule_result` |
| Buyer/insurer report later | `report_order`, `report_snapshot` (verified events only) |

**Bundled cosmetic repair** (bulb, touch-up paint) during the same visit: optional rows in `event_material_use` + extra photos; above-threshold work → **separate** maintenance event or flagged only (no silent stamp extension).

---

## Example walkthrough — move-out, South Jersey STR

**Scene:** 3BR condo, Wildwood summer rental. Guest checked out 11:00 AM. PropertyLedger inspector **J. Torres** (certified org, neutral — not owner’s employee).

### 1. Registry

| Table | Example |
|-------|---------|
| `asset` | `asset_class=property`, `asset_subclass=str_unit`, `value_tier=premium`, `primary_site_id` → Wildwood address |
| `asset_identifier` | `listing_id` (Airbnb internal ref), optional parcel |
| `party` | Owner LLC; optional PM company as `operator` on `asset_ownership` |
| `certified_org` | PropertyLedger Gloucester Township HQ org |
| `certified_tech` | J. Torres, pack `property-str-turnover-v1` authorized |

### 2. Field event (draft → submitted)

| Table | Example |
|-------|---------|
| `ledger_event` | `event_type=inspection`, `event_subtype=move_out`, `standard_pack_id`, `started_at`/`completed_at` |
| | GPS at door, `geofence_match=true`, `capture_site_id` |
| `event_procedure_step` | `SITE_ARRIVAL`, `EXTERIOR_PASS`, `ROOM_ROUNDS`, … completed |
| `event_condition_observation` | Per room: `taxonomy_code=property.room.walls.condition`, `location_label=kitchen`, `severity=minor`, `comparison_to_prior=n/a` (move-out) |
| `event_evidence_link` | Wide photo each required room + exterior; phases `before`/`during` |
| `event_environment` | Weather at inspection time (enrichment) |
| `event_attestation` | Neutral inspector attestation + `signed_at` |

**Required rooms (standard tier):** entry, living, kitchen, bedroom_1, bathroom_1.  
**Premium tier:** adds bedrooms 2–3, baths 2, dining, exterior front/rear, mechanical closet.

### 3. Verification

| Rule | Move-out premium |
|------|------------------|
| `NEUTRAL_INSPECTOR_ATTESTATION` | ✓ |
| `TECH_CERTIFIED_FOR_PACK` | ✓ |
| `GEOFENCE_MATCH` | ✓ (or waiver event) |
| `REQUIRED_ROOMS_CAPTURED` | ✓ all room codes photographed |
| `TAXONOMY_COMPLETE` | ✓ observations for each room/surface set |
| `CHAIN_OF_CUSTODY_COMPLETE` | ✓ evidence log through upload |
| `PHOTO_BEFORE_AFTER` | ✓ arrival + completion |
| `DURATION_LOGGED` | ✓ |

**Pass →** `ledger_verified=true`. This event becomes the **baseline** for the next `move_in`.

### 4. Next guest — move-in on same asset

| Table | Example |
|-------|---------|
| `ledger_event` | `event_subtype=move_in`, links `prior_event_id` → move-out above |
| `event_condition_observation` | `comparison_to_prior=unchanged|new|worsened|improved` per finding |
| Verification | **`PRIOR_BASELINE_LINKED`** must pass (premium/ultra) |

That comparison row is what housing court and deposit disputes need — not a landlord’s phone photos.

---

## Property-specific verification rules

| Rule code | Purpose |
|-----------|---------|
| `REQUIRED_ROOMS_CAPTURED` | Every room in pack list has ≥1 hashed photo |
| `TAXONOMY_COMPLETE` | Required observation codes present per room |
| `PRIOR_BASELINE_LINKED` | Move-in references verified move-out on same `asset_id` |
| `CHAIN_OF_CUSTODY_COMPLETE` | `chain_of_custody_log` from capture → link → verify |
| `NEUTRAL_INSPECTOR_ATTESTATION` | Attestation text includes third-party neutrality |

Add these to the rules engine alongside global rules in `ARCHITECTURE_v0` verification matrix (document as pack-local extensions in v1.1 DDL notes).

---

## Field mapping — turnover vs your business summary

| PropertyLedger promise | Ledger field |
|------------------------|--------------|
| Timestamped, GPS photos | `evidence_object.captured_at`, lat/long, hash |
| Standard terminology | `event_condition_observation.taxonomy_code` |
| Outgoing vs incoming comparison | `comparison_to_prior` + link to prior `ledger_event` |
| Chain of custody | `chain_of_custody_log`, `event_attestation` |
| Property anchor survives sale | `asset_id` immutable; `asset_ownership` transfers |
| Neutral inspector | `certified_org` ≠ owner; attestation rule |
| Archive / pay-per-report (Phase 2) | `report_snapshot` on verified history only |

---

## Tier enforcement (STR owner direct)

| Capability | Standard | Premium | Ultra |
|------------|:--------:|:-------:|:-----:|
| Core rooms + photos + attestation | ✓ | ✓ | ✓ |
| Full room set + taxonomy | — | ✓ | ✓ |
| Prior baseline link on move-in | — | ✓ | ✓ |
| Chain of custody + before/after set | — | ✓ | ✓ |
| Insurer-ready export fields | — | — | ✓ (future pack flag) |

**Day 1 pilot:** standard tier still builds archive habit; tighten to premium as rep training completes — **no schema change**.

---

## Relation to GTM lanes

| Doc | How this pack supports it |
|-----|---------------------------|
| `Air BNB_Business_Summary.md` | Owner subscription + archive vision |
| `01_PropertyManagement_AnchorClient.md` | Per-event pricing, neutrality, legal defensibility |
| `02_TenantPaid_Option.md` | Same pack — tenant-funded move-out adds payer on `report_order` / `party` |
| Winterization | **Separate pack** later (`property-winterization-v1`) — never bundled per your pricing rule |

---

## Revision notes (v1.1)

- Bedroom/bath count driven by `asset` metadata (dynamic room list)
- Redacted public report profile for `public_report_excludes`
- Winterization + seasonal pack split
- Integration hook: deposit dispute export PDF template

---

*Ledger-verified · PropertyLedger · property-str-turnover-v1*
