# PropertyLedger / Ledger-verified — Morning Research Brief

**For:** Dave Sahms · AI Integration and Consulting LLC  
**Date:** 2026-07-09 (compiled overnight)  
**Read with:** `LEDGER_VERIFIED.md`, `DEVICE_TIER.md`, `01_PropertyManagement_AnchorClient.md`, `STATE.md`

---

## Executive summary (30 seconds)

1. **You're building the documenting layer first** — photo → SmolVLM describe → human confirm → hash/GPS → JSON. That is correct. Service + archive come later.
2. **Tonight's GitHub work did not break beta apps** on anyone's phone. NURA, StoryKeeper, Refactor Room exes are unchanged.
3. **Ledger-verified / Ledger-certified** = trust mark on an **append-only event record**, not honor-system photos. Worth something when buyers/insurers can **search property ID history** — after density, not day one.
4. **Field hardware:** You do **not** need to buy upgrades tonight. **Minimum spec** for employees = 8 GB + good camera (500M model). **Quality spec** = 12 GB non-rooted phone (2.2B model, embedded app later).
5. **Best sales targets tomorrow:** Property management companies with **20–475 units** in your geography — list below with links.

---

## Part 1 — What "Ledger-certified" means (and when it's worth money)

### The mark (from your docs)

> **Ledger-verified** — evidence-backed condition/maintenance history, not self-reported.

Same energy as certified pre-owned or Carfax, but for **property turnover events** (and later ag/marine/fleet packs).

### What makes it worth something

| Stakeholder | Why they pay later |
|-------------|-------------------|
| **Owner / seller** | Higher confidence at sale; portable history survives ownership change |
| **Buyer / investor** | Due diligence report — "what happened between tenants?" |
| **Insurer** | Pre-loss condition evidence; maintenance compliance |
| **Property manager** | Neutral third party wins deposit disputes in housing court |

### The moat (your line)

> *The moat is not the walkthrough. The moat is the archive.*

Nobody can backfill 3 years of GPS-timestamped, standard-pack events tied to **property ID**. Carfax became wallpaper because everyone has access; **Ledger-verified** only works if the mark means **checked against a standard pack**, not owner-uploaded photos.

### Phases (don't skip)

| Phase | You sell | Data goal |
|-------|----------|-----------|
| **Now** | Documenting app + pilot with PM companies | Standardized JSON exports, human-confirmed |
| **Next** | Per-turnover fee ($75–200/unit) or subscription | Continuous archive per property |
| **Later** | Pay-per-report (D&B of STR) | Institutional search — needs **density** (think 50–200+ properties with steady events) |

---

## Part 2 — Documenting stack (SmolVLM + Pocket Pal)

### What you have today

| Layer | Tool | Role |
|-------|------|------|
| Capture | **PropertyLedger Snap** v0.2 APK | Camera, room walkthrough, export JSON |
| Quick labels | **Google ML Kit** | On-device tags ("wall", "furniture") — built in |
| Rich describe | **Pocket Pal** + **SmolVLM2 500M** | Share photo → AI paragraph → paste back → **you edit** → confirm |
| Standard | `property-str-turnover-v1` | Move-out / move-in / full turnover taxonomy |
| Verify (desktop) | `verify_export.py` | Proves export shape before any "stamp" |

**Loop doc:** `loops/propertyledger-pocketpal-v1.md`

### Model sizes (Hugging Face / your DEVICE_TIER)

| Model | RAM (model only, approx.) | Phone class | Quality |
|-------|----------------------------|-------------|---------|
| SmolVLM2 **256M** | ~0.5 GB | Very low-end | Bare minimum captions |
| SmolVLM2 **500M** Q8 | ~0.8 GB + OS overhead | **8 GB** (OnePlus 7T) | **Today — demo & R&D** |
| SmolVLM2 **2.2B** Q4 | ~3.4 GB + overhead | **12 GB** (OnePlus 11/12) | **Quality app target** — richer room descriptions |

**Rule:** More RAM + better chip → bigger model → better "scuffed baseboard, water stain on ceiling" text.

### Roadmap (from FIELD_CAPTURE_DEMO_v0)

- **v0** — ML Kit only ✅  
- **v1** — Pocket Pal share + paste ✅ (v0.2)  
- **v2** — **Embedded SmolVLM** in Snap (no app switching) ← build target  
- **v3** — Upload + server verification worker (rules engine → stamp)

### Live video vs photo

Your architecture is **photo per surface/room**, not live narration. That is **correct** for legal chain-of-custody and turnover standard packs. "Live" later = grab frame from preview → same describe pipeline.

---

## Part 3 — Hardware policy for employees (minimum vs quality)

### Non-negotiables (every field rep)

| Requirement | Why |
|-------------|-----|
| **Non-rooted** | Rooted lab phone (7T) invalidates trust anchor for certification |
| **Android 11+** (target 13+) | Modern camera APIs, security |
| **GPS** | Every photo/event geotagged |
| **64 MP or excellent 48 MP camera** | Condition detail for disputes |
| **Company-owned phone** | Standard app stack, no personal clutter |
| **Human confirm step** | AI suggests; rep attests — required for Ledger-verified |

### Tier A — Minimum spec (certified to operate, 500M model)

**Publish as:** *Ledger Field Capture — Minimum*

| Spec | Target |
|------|--------|
| RAM | **8 GB** |
| Storage | 128 GB+ |
| Chip | Snapdragon 778G class or better |
| Examples | Used OnePlus 9/10, Samsung Galaxy A54/A55, Pixel 6a (check RAM) |
| AI | SmolVLM2 **500M** via Pocket Pal **or** embedded |
| Cost (used/refurb) | **~$150–250** per rep |

**Good for:** Pilot crews, seasonal shore turnover volume, proof of workflow.

### Tier B — Quality spec (recommended employee standard, 2.2B model)

**Publish as:** *Ledger Field Capture — Standard*

| Spec | Target |
|------|--------|
| RAM | **12 GB minimum**, **16 GB preferred** |
| Chip | Snapdragon 8 Gen 2 or Gen 3 |
| Examples | **OnePlus 11 (12 GB)**, **OnePlus 12 (16 GB)**, OnePlus 8T (12 GB refurb), Samsung S23 FE 8GB borderline — prefer 12 |
| AI | SmolVLM2 **2.2B** embedded in app (v2) |
| Cost (new/refurb) | **~$350–700** per rep |

**Skip:** OnePlus **10** (weak thermals — your own DEVICE_TIER note).

**Your own doc recommendation:** 11 if budget tight; **12 (16 GB)** if field AI is a product pillar.

### Tier C — Rugged (optional later — not required for STR turnover)

Thermal/rugged phones (JCB Toughphone P20, Ulefone Armor 19T/25T, Oukitel WP500):

- **12 GB RAM**, IP68/IP69K, drop-rated, some have **FLIR thermal**
- **~$400–700+**
- **Use case:** moisture intrusion, electrical hot spots, ag/industrial packs — **overkill for Airbnb room walkthrough** unless you productize " moisture scan add-on"

**Honest take:** Start Tier B consumer phones with good cases (OtterBox). Rugged when insurance/ag vertical pays for it.

### Employee kit checklist (ship with phone)

- [ ] PropertyLedger Snap (or v2 embedded build)
- [ ] Pocket Pal preloaded with SmolVLM2 (until embedded)
- [ ] Written **standard pack** card (room order, severity taxonomy)
- [ ] Company Google account / MDM (optional later)
- [ ] **No** personal Apple/Google photos backup of job photos without policy

---

## Part 4 — Anchor clients: 20+ properties (research list)

**Strategy (from your `01_PropertyManagement_AnchorClient.md`):**  
Sell **liability transfer + neutral documentation**, not software.  
Pitch hook: *"What do you hand your attorney when a tenant disputes a deposit?"*

**Pilot offer idea (steep discount for first anchor):**

- **"Founding Partner"** — first **25 properties** documented at **50% off** per turnover **or** first **10 turnovers free** in exchange for:
  - Logo/testimonial rights
  - Permission to use anonymized export samples
  - Commitment to **standard pack** (no custom chaos)
- Goal: **one** signed PM company → case study → density

### Tier 1 — Whales (100+ units) — one conversation changes the business

| Company | Est. scale | Markets | URL | Notes |
|---------|------------|---------|-----|-------|
| **Sosuite** | **~475 units** | Philadelphia | https://comparent.com/str/pa/philadelphia/sosuite · https://sosuite.com | Multifamily + flexible stay; hospitality-driven; **#1 anchor target** |
| **Galvanized Management** | **175+** | Poconos (Monroe, Pike, Carbon) | https://galvanizedmanagement.com | In-house team, STR + LTR; founder Jeremiah Noll, local |
| **Pocono Mountain Rentals** | **Large portfolio** (20+ years, many named units) | Lake Harmony, Poconos | https://www.poconomountainrentals.com/property-management/ | Luxury/large-group cabins; long track record |

### Tier 2 — Strong regional (20–100+ units, shore + Philly)

| Company | Est. scale | Markets | URL | Notes |
|---------|------------|---------|-----|-------|
| **HostPro Properties** | Multi-market portfolio | Philly, Wildwood, Jersey Shore | https://www.hostproproperties.com | "Commercial-grade operations" — speaks your language |
| **Cozy Co-Host** | **"Dozens"** of STRs | Atlantic City → Cape May | https://www.cozycohost.com | Airbnb Superhost brand; growth since 2020 |
| **Ocean Property Management** | Multi-town shore portfolio | Wildwood, OC, Cape May, Avalon, etc. | https://oceanpmnj.com | Full-service vacation rental mgmt |
| **CarefreeNb Hosting** | Portfolio (AC, Avon, Wildwood) | Jersey Shore | https://www.carefreenbhosting.com/properties | Direct portfolio page |
| **HostAid** | Scale TBD (premium Philly) | Philadelphia | https://www.host-aid.com | White-glove; 901 Market St; +30% income claim |
| **Bespoke Stay** | Multi-city STR | Philly, Jersey Shore, Pittsburgh | https://bespokestay.com | Professional listing + ops |
| **Cape May MBK** | Cape May County | Cape May | https://www.capemaymbk.com | Revenue optimization focus; licensed/insured |
| **Cabrera Property Management** | Vacation homes + HOAs | Wildwood Crest, Cape May County | https://cabrerapm.com | Certified manager; walkthroughs mentioned |

### Tier 3 — Smaller but valid (20+ or growth trajectory)

| Company | Markets | URL |
|---------|---------|-----|
| **Set Apart Solutions** | Philly, DE, PA (since 2021) | https://setapartsolutionsllc.com |
| **Three Daughters Properties** | Poconos co-hosting | (see onefinebnb Poconos list) |

### Markets you already named in PropertyLedger docs

- Philadelphia / South Jersey (Gloucester Township HQ)
- Atlantic City / Wildwood / Ocean City / Cape May
- Poconos (year-round + seasonal)

### Who to call first (recommended order)

1. **Sosuite** (475) — if you get a meeting, stop hunting for a week and prepare  
2. **Galvanized** (175+) — Poconos; similar ops pain, local story  
3. **HostPro** or **Cozy Co-Host** — shore; turnover-heavy  
4. **HostAid** — Philly premium; legal/deposit angle  
5. One **Wildwood/Cape May** operator (Ocean PM or Cabrera) for summer season pilot  

### 30-second pitch (housing court angle)

> "When a tenant fights a security deposit in housing court, what packet do you give your lawyer? We produce neutral, timestamped, GPS-tagged turnover documentation on a standard taxonomy — every unit, same language. We're looking for one founding partner in [market] to pilot at half price on the first 25 turnovers in exchange for helping us set the standard."

---

## Part 5 — Pricing math (why 20+ units matters)

From your anchor-client doc:

| Volume | Per turnover | Monthly (20 turnovers) |
|--------|--------------|------------------------|
| Move-out + move-in bundle | $150–200 | **$3,000–4,000** / one client |
| 10 clients × 20 turnovers | — | **$30,000–40,000/mo** (theoretical at scale) |

**Steep discount pilot** on 25 units at $100 off still buys you **$2,500+ of proof** and irreplaceable archive rows.

Retail STR owner pricing ($300–350/mo/property) is Phase 1B — **PM companies multiply faster**.

---

## Part 6 — Tomorrow action list (pick 2, not 10)

### Research / sales

- [ ] Pick **3 names** from Tier 1–2 table; find **ops contact** (not info@ — LinkedIn "Director of Operations" or owner)
- [ ] Draft **one-page Founding Partner PDF** (half-price pilot, what they get, what you need)
- [ ] Run Snap on 7T through **one full room** → export → `verify_export.py` pass (proof for demos)

### Product (documenting only)

- [ ] Do **not** rebuild platform/search yet
- [ ] Optional: spec **embedded SmolVLM v2** (remove Pocket Pal hop) — next engineering sprint
- [ ] Write published **Minimum Hardware Requirements** page (Tier A/B from Part 3) for website/deck

### Do not do tomorrow

- [ ] Don't merge Ledger-verified into `ledger_core` interview engine  
- [ ] Don't push new beta APKs to NURA/StoryKeeper testers  
- [ ] Don't buy 5 phones until **one** anchor client signs a pilot letter  

---

## Part 7 — How this fits the rest of your portfolio

| Product | Status | Focus now? |
|---------|--------|------------|
| **PropertyLedger / Ledger-verified** | Architecture + Snap demo | **YES — primary** |
| StoryKeeper | Beta on phone | Maintain only |
| NURA Connect | Beta on phone | Maintain only |
| Refactor Room | Shipped + GitHub | Shelf |
| Interview Ledgers (insurance, EMS, med students) | Ideas | Shelf until PropertyLedger proves documenting |
| Arden / Consigliere | Shipped | Separate track |

---

## Part 8 — Honest opinion (unchanged from last night)

**You're right to lead with PropertyLedger.** It is the most sellable near-term path and the only one with a credible **data moat** story (D&B of STR / property condition).

**Caveats:**

- Phase 1 is still **ops + capture** — someone walks the unit unless PM outsources entirely to you  
- Institutional "they come for the data" is **Phase 2+** — needs archive density  
- **Sosuite-scale anchor** is a lottery ticket worth buying a stamp for — one yes beats 50 host cold calls  

---

## Quick links (your disk)

| Item | Path |
|------|------|
| Project state | `D:\Air BNB Ledger\STATE.md` |
| Brand / mark | `D:\Air BNB Ledger\LEDGER_VERIFIED.md` |
| Device tiers | `D:\Air BNB Ledger\DEVICE_TIER.md` |
| Anchor strategy | `D:\Air BNB Ledger\01_PropertyManagement_AnchorClient.md` |
| Snap demo | `D:\dev\PropertyLedger-Snap-Demo\` |
| APK v0.2 | `D:\Beta Apps\PropertyLedger-Snap-v0.2.0.apk` |
| Combine pack (future vertical) | `STANDARD_PACK_ag-combine-oil-v1.md` |

---

*Sleep. Review this with coffee. Tomorrow: three phone calls, not three new products.*
