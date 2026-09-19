# Unathi Port — Reference Inventory & Feature Diff

Working notes for converting SS14 `Reptilian` → Paradise `Unathi`.

- **SS14 source:** `Resources/Prototypes/Species/reptilian.yml`, `Resources/Prototypes/Body/Species/reptilian.yml`
- **SS13 source:** `code/modules/mob/living/carbon/human/species/unathi.dm` (Paradise)

> **The single most important thing in this doc:** SS14 uses **two** vocabularies for this species — `Reptilian` **and** `Lizard`. `lizard` outnumbers `reptilian` 62:23 in `Resources/Prototypes` alone. Any rename that only chases "reptilian" will miss more than half the surface. See §2.3.

---

## Part 1 — Feature differences

### 1.1 Summary table

| Feature | SS14 Reptilian | Paradise Unathi | Verdict |
|---|---|---|---|
| Hiss accent | `LizardAccent` — `s`→`sss`, `x`→`kss`/`ecks` | `autohiss` — `s`→`ss`/`sss`/`ssss` (random), `x`→`ks`/`kss`/`ksss` | Close. Paradise randomises length |
| Claw attack | `MeleeWeapon` Slash 5, `AlienClaw` sound | `unarmed_type = /datum/unarmed_attack/claws` | **Match** |
| Tail wagging | `Wagging` | `TAIL_WAGGING` bodyflag | **Match** |
| Tail dragging | `Puller: needsHands: false` | *not present* | SS14 extra — decide keep/cut |
| Cold slowdown | `TemperatureSpeed` 301/295/285 → 0.9/0.8/0.7 | *not present* | SS14 extra — decide keep/cut |
| Cold vulnerability | `coldDamageThreshold: 285` | `cold_level_1 = 280` (human 260) | Close enough |
| Heat resistance | `heatDamageThreshold: 400` (SS14 human 325) | `heat_level_1 = 505` (Paradise human 360) | **Already correct — do NOT copy 505.** See §1.5 |
| Diet | Fruit, Meat, ReptilianFood, Pill, Crayon, Paper | `dietflags = DIET_CARN` (pure carnivore) | **Paradise stricter — no fruit** |
| Taste | default | `TASTE_SENSITIVITY_SHARP` | Minor gap |
| Butchers into | `FoodMeatLizard` ×5 | `meat_type = human meat`, hide = lizard | Divergent, cosmetic |
| **Darksight** | ✗ none | `eyes = /organ/internal/eyes/unathi` — 3 darksight | **MISSING** |
| **Ignite action** | ✗ none | `/datum/action/innate/unathi_ignite` | **MISSING** |
| **Eat small mobs** | ✗ none | `allowed_consumed_mobs` — 9 species | **MISSING** |
| **Racial language** | ✗ none | `Sinta'unathi` | **MISSING — no language system exists** |
| **Alt heads** | ✗ (has 5 Snout markings) | `HAS_ALT_HEADS` bodyflag | Partial — snouts are the analogue |
| Head accessories | 12 HeadTop + 9 HeadSide markings | `HAS_HEAD_ACCESSORY`, default `"Simple"` | Probably fine, verify |
| Scream sound | `MaleReptilian`/`FemaleReptilian` | `unathiscream.ogg` (both sexes) | Different asset |

### 1.2 The four real gaps

- [ ] **Darksight / night vision.**
  Paradise Unathi eyes grant 3 darksight. **Good news:** `Content.Shared/NightVision/` exists in this fork and `- type: NightVision` is already applied directly to mobs (`Entities/Mobs/base.yml:278`, `changeling.yml:101`, `observer.yml:100`), so this is achievable by adding the component to `MobReptilian` rather than building anything. Check whether it's permanent or toggleable first.

- [ ] **Ignite action.**
  `unathi_ignite` in `unathi.dm:79-118`. Drink welding fuel → spawn a lit match in hand. 20s cooldown, 3u fuel required, blocked if mouth is covered. Needs a new action + a match entity. Self-contained; good first mechanic to port.

- [ ] **Eating small mobs whole.**
  `allowed_consumed_mobs` — mouse, lizard, chick, chicken, crab, butterfly, parrot, bee, small isopod. No SS14 equivalent. Needs a new component + verb.

- [ ] **Sinta'unathi racial language.**
  ⚠️ **There is no language system in this fork.** The only thing resembling one is `RatvarianLanguageComponent`, which is a cult speech-scrambler, not a multi-language system. This is a fork-wide feature, not an Unathi task — either descope for v1 or schedule it separately. It will also block Tajaran (Siik'maas), Skrell (Skrellian) and every other Paradise species later.

### 1.3 Numeric values to change

In `Resources/Prototypes/Body/Species/reptilian.yml` (~line 175):

- [x] ~~`heatDamageThreshold: 400` → raise substantially~~ **RETRACTED — see §1.5. Leave 400 as-is.**
- [ ] `coldDamageThreshold: 285` → roughly right (Paradise 280).
- [ ] Stomach `specialDigestible` (~line 289): remove `Fruit` for pure carnivore parity. **Consider carefully** — this makes Unathi meaningfully harder to feed on an SS14 station, which may be a balance decision rather than a parity one.
- [ ] `defaultSkinTone: "#34a223"` vs Paradise `flesh_color = "#34AF10"` / `base_color = "#066000"`.

### 1.5 Heat thresholds: why 400 ≠ "worse than 505"

Both games compare the mob's **body temperature** (not ambient) against the threshold, and both set normal body temp to **310.15 K**. So they're directly comparable — but only against each game's own human baseline, which differ a lot.

| | Threshold | Headroom over 310.15 | vs own human |
|---|---:|---:|---:|
| SS14 human (`Mobs/base.yml:211`) | 325 | +14.85 K | 1.00× |
| **SS14 Reptilian** | 400 | +89.85 K | **6.05×** |
| Paradise human | 360 | +49.85 K | 1.00× |
| **Paradise Unathi** | 505 | +194.85 K | **3.91×** |

**SS14 Reptilian is already proportionally *more* heat-resistant than Paradise Unathi** (6.05× vs 3.91× their own human). SS14 humans only get ~15 K of headroom where Paradise humans get ~50 K, so raw Kelvin comparisons are misleading.

Corroborating: Reptilian at 400 is already the most heat-resistant species in this codebase by a wide margin — Moth and Vulpkanin sit at **320**, *below* human's 325; Skeleton is 373.15.

**Damage past the threshold differs in shape, not just position:**

- **Paradise — flat tiers**, applied per 2 s life tick (`SSmobs` default `wait = 20`):
  505–540 K → 2 burn (**1.0/s**) · 540–600 K → 3 burn (**1.5/s**) · >600 K → 3 burn, 5 if on fire (**2.5/s**)
  A cliff: at 505.1 K you immediately take the full rate.
- **SS14 — logistic curve**, per second: `1.5 × (16/(1+e^(−0.005·diff)) − 8)`, capped at 8× = **12/s**
  +10 K → 0.30/s · +50 K → 1.49/s · +100 K → 2.94/s · +200 K → 5.55/s
  A soft shoulder near the threshold, but scales to ~8× Paradise's worst case at extremes.

**Verdict: leave `heatDamageThreshold: 400` alone.** Copying 505 would give Unathi 195 K of headroom in a game where humans get 15 K — roughly **13× human**, wildly beyond anything Paradise intended. If you want to match Paradise's *ratio* exactly, the equivalent is ≈ **368 K**, which would mean *lowering* the current value. Since 400 already reads as "notably heat-resistant" within SS14's own balance, the cheapest correct action is no action.

### 1.4 Already correct — leave alone

Claws, tail wagging, the hiss accent, cold vulnerability direction, `damageModifierSet: Scale`, `Metabolizer: Animal`, and the marking coverage (Tail 12, HeadTop 12, HeadSide 9, Snout 5, Chest 4, one per limb).

---

## Part 2 — Reference inventory

### 2.0 Already converted (done in the previous pass)

- [x] `Resources/Locale/en-US/species/species.ftl` — key + value → `species-name-unathi = Unathi`
- [x] `Resources/Prototypes/Species/reptilian.yml` — `name:` repointed to new key
- [x] `Resources/Prototypes/Guidebook/species.yml` — child ref fixed, repositioned
- [x] `Resources/ServerInfo/Guidebook/Mobs/Reptilian.xml` → `Unathi.xml` (git mv)
- [x] `Resources/ServerInfo/Guidebook/Mobs/Species.xml` — embed caption
- [x] `Resources/ServerInfo/Guidebook/NewPlayer/YourFirstCharacter.xml` — 3 prose mentions
- [x] `Resources/ServerInfo/Guidebook/Service/SavoryRecipes.xml` — 1 prose mention
- [x] `Resources/Locale/en-US/administration/smites.ftl` — 2 strings
- [x] `Resources/Locale/en-US/chat/managers/chat-manager.ftl` — speech verb display name
- [x] `Resources/Locale/en-US/markings/gauze.ftl` — 4 eyepatch names
- [x] `Resources/Locale/en-US/tips.ftl` — cryopod tip
- [x] `Resources/Prototypes/Body/Species/reptilian.yml` — `name: unathi appearance`, `suffix: Unathi`
- [x] `Resources/Prototypes/Entities/Objects/Fun/Figurines/figurines.yml` — cargo tech figurine

> Verified: no FTL **value** and no guidebook prose still says "reptilian".

### 2.1 Guidebook content — needs a rewrite, not a rename

- [ ] `Resources/ServerInfo/Guidebook/Mobs/Unathi.xml` — still carries your `[TODO -UPDATE ME]` marker and SS14's feature list. Paradise lore blurb is in `unathi.dm:11-13` (Moghes, Uuosa-Eso system, feudal clan structure). The mechanical claims in this file must also be re-checked against whatever you change in §1.3.

### 2.2 "Reptilian" references — remaining

**Player-visible text (none left — all converted).**

**Prototype IDs — only touch if doing a full ID rename (breaks character saves):**

| File | Hits | Notes |
|---|---:|---|
| `Prototypes/Body/Species/reptilian.yml` | 83 | 20× `OrganReptilian*`, `MobReptilian`, `AppearanceReptilian` |
| `Prototypes/Entities/Mobs/Customization/Markings/reptilian.yml` | 92 | marking IDs + sprite paths |
| `Prototypes/Entities/Clothing/Head/hardsuit-helmets.yml` | 33 | per-species helmet sprite fitting |
| `Prototypes/Entities/Mobs/Customization/Markings/gauze.yml` | 17 | |
| `Prototypes/Entities/Mobs/Customization/Markings/undergarments.yml` | 13 | |
| `Prototypes/Entities/Mobs/Customization/Markings/scars.yml` | 10 | |
| `Prototypes/Species/reptilian.yml` | 10 | `id:`, `prototype:`, `dollPrototype:`, name datasets, voices |
| `Prototypes/Voice/speech_emote_sounds.yml` | 6 | |
| `Prototypes/Voice/speech_verbs.yml` | 6 | |
| `Prototypes/tags.yml` | 5 | `ReptilianFood` tag |
| `Prototypes/Entities/Mobs/NPCs/animals.yml` | 5 | **kobolds** reuse reptilian speech/markings |
| `Prototypes/Entities/Mobs/Customization/Markings/tattoos.yml` | 4 | |
| `Prototypes/Entities/Objects/Consumable/Food/snacks.yml` | 3 | `ReptilianFood`-tagged items |
| `Prototypes/Entities/Objects/Consumable/Food/Baked/pizza.yml` | 2 | |
| `Prototypes/Entities/Objects/Fun/Plushies/plushies.yml` | 2 | |
| `Prototypes/Datasets/Names/reptilian_{male,female}.yml` | 2+2 | |
| `Prototypes/Entities/Objects/Consumable/Food/Baked/bread.yml` | 1 | |
| `Prototypes/Actions/types.yml` | 1 | sprite path |
| `Prototypes/Loadouts/Miscellaneous/survival.yml` | 1 | |
| `Prototypes/Polymorphs/admin.yml` | 1 | |
| `Prototypes/Species/species_weights.yml` | 1 | roundstart weight `Reptilian: 4` |

**C# — identifiers and one sprite path:**

- [ ] `Content.Server/Administration/Systems/AdminVerbSystem.Smites.cs` — 5 hits (smite definition)
- [ ] `Content.Shared/Body/SharedVisualBodySystem.Modifiers.cs:43` — `reptilian_parts.rsi` path

**Name datasets — 491 loc keys, values are lizard-style names:**

- [ ] `Resources/Locale/en-US/datasets/names/reptilian_male.ftl` (328) + `reptilian_female.ftl` (163)
- Keys are `names-reptilian-*`. **The values are the real question** — if Unathi should use Paradise's own name lists, that's a *content* task, not a rename. Paradise names live in `code/datums/` name lists.

**Audio:**

- [ ] `Resources/Audio/Voice/Reptilian/` — 3 files (note the existing typo: `attritbutions.yml`)

**Textures — 160 files:**

- [ ] `Resources/Textures/Mobs/Customization/reptilian_parts.rsi`
- [ ] `Resources/Textures/Mobs/Species/Reptilian/parts.rsi`
- [ ] `Resources/Textures/Mobs/Species/Reptilian/displacement.rsi`
- [ ] 152 hardsuit-helmet sprites under `Resources/Textures/Clothing/Head/Hardsuits/*.rsi/*-reptilian.png` — these are per-species helmet fittings, referenced by filename suffix. **Renaming these means touching `hardsuit-helmets.yml` in lockstep.** Highest-risk rename in the whole list for the least player-visible benefit; recommend leaving.

### 2.3 "Lizard" references — the parallel vocabulary

⚠️ **Read this section before deciding on any rename.** These are the same species under a different name. Sifting required — the word covers three unrelated things.

**(a) Species-related — same entity as Reptilian:**

- [ ] `Content.Shared/Speech/{Components/LizardAccentComponent.cs, EntitySystems/LizardAccentSystem.cs}` — the hiss accent
- [ ] `Resources/Locale/en-US/markings/reptilian.ftl` — 85 hits, marking display names
- [ ] `Resources/Prototypes/Entities/Mobs/Customization/Markings/reptilian.yml` — 47 lizard hits (sprite states)
- [ ] `Resources/Locale/en-US/markings/gauze.ftl` — 10 (`gauze_lizard_*` sprite states)
- [ ] `Resources/Prototypes/Voice/speech_sounds.yml` — 4 (`speechSounds: Lizard`)
- [ ] `Resources/Prototypes/typing_indicator.yml` — 3 (`proto: lizard`)
- [ ] `Resources/Prototypes/Entities/Mobs/Player/clone.yml` — 2
- [ ] `Resources/Prototypes/Traits/speech.yml` — 1
- [ ] `Content.Shared/Humanoid/Prototypes/SpeciesPrototype.cs` — 1
- [ ] `Resources/Textures/Effects/speech.rsi/lizard{0..3}.png`, `creampie.rsi/creampie_lizard.png`
- [ ] Plushies: `plushielizard_jobs.yml` (159), `plushies.yml` (58) — "lizardperson" plushies, species-themed merch
- [ ] Food from the species: `Food/meat.yml` (25) — `FoodMeatLizard` is what `MobReptilian` butchers into

**(b) The actual pet animal — NOT the species, leave alone:**

- `Resources/Prototypes/Entities/Mobs/NPCs/animals.yml:2190` — `MobLizard`, `name: lizard`
- `Resources/Textures/Mobs/Animals/lizard.rsi`
- `Resources/Prototypes/Catalog/Cargo/cargo_livestock.yml`, `Entities/Markers/Spawners/Mobs/animals.yml:180`
- `Resources/Audio/Animals/lizard_happy.ogg`
- ⚠️ **Note the collision:** `MobLizard` (pet) butchers into `FoodMeatLizard`, the *same* item `MobReptilian` butchers into. Paradise keeps these separate (`meat_type = human meat` for Unathi). Worth a decision.

**(c) Unrelated idiom — leave alone:**

`arcade_villain.ftl`, `story-generation.ftl`, `book-authorbooks.ftl`, `delivery-spam.ftl`, `holiday-greet.ftl` + `holidays.yml` (Mister Lizard), `operation_prefix.ftl`, `janidrobe.ftl`, `posters.yml` (conspiracy poster), `immovable_rod.yml`, `ion_storm.yml`, `XenoArch/effects.yml`, kobold descriptions ("cousins to the sentient race of lizard people" — lore-adjacent, your call).

### 2.4 Excluded from all counts

Build artifacts: `Content.*/obj/`, `Content.*/bin/`, `*.dll`, `*.pdb`. These regenerate — never edit.

---

## Part 3 — Suggested order

1. [ ] Rewrite `Unathi.xml` guidebook with Paradise lore + corrected mechanics (clears your TODO)
2. [ ] Numeric parity pass — §1.3 (heat threshold, diet, skin tone)
3. [ ] Add `NightVision` to `MobReptilian` — cheapest of the four real gaps
4. [ ] Port the Ignite action — self-contained, good scope
5. [ ] Decide on name datasets (§2.2) — content task
6. [ ] Eat-small-mobs component — new work
7. [ ] Language system — fork-wide, schedule separately, blocks all later species

**Recommend deferring the full ID rename indefinitely.** Display names already read "Unathi" everywhere. Renaming IDs breaks character saves, touches 300+ prototype lines and 152 helmet sprites, and changes nothing a player sees.
