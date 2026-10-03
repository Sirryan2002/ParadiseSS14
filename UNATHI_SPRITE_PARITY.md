# Unathi Sprite Parity — Port Checklist

Sprite-by-sprite comparison of **SS13 Paradise Unathi** against **SS14 Reptilian**, so you can
tick off what needs porting.

- **SS13 source:** `Paradise/icons/mob/sprite_accessories/unathi/*.dmi`
- **SS14 source:** `Resources/Textures/Mobs/Customization/reptilian_parts.rsi/`
- Previews are the **south-facing** cell only, auto-cropped to content and scaled 4×.
  Checkerboard = transparency. These are greyscale masks; the game tints them.

**Status key** — ✅ SS14 already has it · ⚠️ probable match, eyeball it · ❌ missing, needs porting

---

## 1. Horns &amp; Crest

SS13 `unathi_hair.dmi` (16) → SS14 `HeadTop` markings (12)

| SS13 | name | SS14 | name | status |
|:--:|---|:--:|---|---|
| ![](sprite-parity/ss13/unathi_hair__simple_horns_s.png) | `simple_horns` | ![](sprite-parity/ss14/horns_simple.png) | `horns_simple` | ✅ |
| ![](sprite-parity/ss13/unathi_hair__short_horns_s.png) | `short_horns` | ![](sprite-parity/ss14/horns_short.png) | `horns_short` | ✅ |
| ![](sprite-parity/ss13/unathi_hair__curled_horns_s.png) | `curled_horns` | ![](sprite-parity/ss14/horns_curled.png) | `horns_curled` | ✅ |
| ![](sprite-parity/ss13/unathi_hair__ram_horns_s.png) | `ram_horns` | ![](sprite-parity/ss14/horns_ram.png) | `horns_ram` | ✅ |
| ![](sprite-parity/ss13/unathi_hair__ram2_horns_s.png) | `ram2_horns` | ![](sprite-parity/ss14/horns_argali.png) | `horns_argali` | ⚠️ |
| ![](sprite-parity/ss13/unathi_hair__drac_horns_s.png) | `drac_horns` | ![](sprite-parity/ss14/horns_double.png) | `horns_double` | ⚠️ |
| ![](sprite-parity/ss13/unathi_hair__cobrahood_s.png) | `cobrahood` | ![](sprite-parity/ss14/frills_hood_primary.png) | `frills_hood_primary` | ⚠️ |
| ![](sprite-parity/ss13/unathi_hair__cobrahood_webbing_s.png) | `cobrahood_webbing` | ![](sprite-parity/ss14/frills_hood_secondary.png) | `frills_hood_secondary` | ⚠️ |

### To port

- [ ] ![](sprite-parity/ss13/unathi_hair__lower_horns_s.png) `lower_horns` — small low studs
- [ ] ![](sprite-parity/ss13/unathi_hair__big_horns_s.png) `big_horns` — scattered brow spikes
- [ ] ![](sprite-parity/ss13/unathi_hair__small_horns_s.png) `small_horns` — fine brow row
- [ ] ![](sprite-parity/ss13/unathi_hair__chin_horns_s.png) `chin_horns` — paired chin blocks
- [ ] ![](sprite-parity/ss13/unathi_hair__adorns_horns_s.png) `adorns_horns` — ring of studs
- [ ] ![](sprite-parity/ss13/unathi_hair__spikes_horns_s.png) `spikes_horns` — triple crest spikes
- [ ] ![](sprite-parity/ss13/unathi_hair__hipbraid_s.png) `hipbraid` — hooded braid
- [ ] ![](sprite-parity/ss13/unathi_hair__hipbraid_beads_s.png) `hipbraid_beads` — braid beads (secondary layer)

---

## 2. Frills / Jaw

SS13 `unathi_facial_hair.dmi` (14) → SS14 `HeadSide` markings (9)

SS13 pairs each design with a `_webbing` variant — that is the same split as SS14's
`_primary` / `_secondary` two-tone markings, so port them together.

| SS13 | name | SS14 | name | status |
|:--:|---|:--:|---|---|
| ![](sprite-parity/ss13/unathi_facial_hair__aquaticfrills_s.png) | `aquaticfrills` | ![](sprite-parity/ss14/frills_aquatic.png) | `frills_aquatic` | ✅ |
| ![](sprite-parity/ss13/unathi_facial_hair__shortfrills_s.png) | `shortfrills` | ![](sprite-parity/ss14/frills_short.png) | `frills_short` | ⚠️ |
| ![](sprite-parity/ss13/unathi_facial_hair__longfrills_s.png) | `longfrills` | ![](sprite-parity/ss14/frills_big.png) | `frills_big` | ⚠️ |
| ![](sprite-parity/ss13/unathi_facial_hair__sidefrills_s.png) | `sidefrills` | ![](sprite-parity/ss14/frills_simple.png) | `frills_simple` | ⚠️ |

### To port

- [ ] ![](sprite-parity/ss13/unathi_facial_hair__longspines_s.png) `longspines` + ![](sprite-parity/ss13/unathi_facial_hair__shortspines_s.png) `shortspines`
- [ ] ![](sprite-parity/ss13/unathi_facial_hair__dracfrills_s.png) `dracfrills` + ![](sprite-parity/ss13/unathi_facial_hair__dracfrills_webbing_s.png) `dracfrills_webbing`
- [ ] ![](sprite-parity/ss13/unathi_facial_hair__dorsalfrills_s.png) `dorsalfrills` + ![](sprite-parity/ss13/unathi_facial_hair__dorsalfrills_webbing_s.png) `dorsalfrills_webbing`
- [ ] Webbing layers for any ⚠️ match you accept above:
      ![](sprite-parity/ss13/unathi_facial_hair__aquaticfrills_webbing_s.png) `aquaticfrills_webbing` ·
      ![](sprite-parity/ss13/unathi_facial_hair__shortfrills_webbing_s.png) `shortfrills_webbing` ·
      ![](sprite-parity/ss13/unathi_facial_hair__longfrills_webbing_s.png) `longfrills_webbing` ·
      ![](sprite-parity/ss13/unathi_facial_hair__sidefrills_webbing_s.png) `sidefrills_webbing`

---

## 3. Head &amp; Face Markings

SS13 `unathi_head_markings.dmi` (10) → SS14 `Head` + `Snout` markings

**This is the biggest gap.** SS14's `Head` layer has exactly one marking (`head_tiger`);
the rest of its head art is snout shapes, which cover only 3 of the 10 SS13 designs.

| SS13 | name | SS14 | name | status |
|:--:|---|:--:|---|---|
| ![](sprite-parity/ss13/unathi_head_markings__tigerhead_s.png) | `tigerhead` | ![](sprite-parity/ss14/head_tiger.png) | `head_tiger` | ✅ |
| ![](sprite-parity/ss13/unathi_head_markings__snoutsharp_s.png) | `snoutsharp` | ![](sprite-parity/ss14/snout_sharplight.png) | `snout_sharplight` | ⚠️ |
| ![](sprite-parity/ss13/unathi_head_markings__snoutround_s.png) | `snoutround` | ![](sprite-parity/ss14/snout_roundlight.png) | `snout_roundlight` | ⚠️ |
| ![](sprite-parity/ss13/unathi_head_markings__lowersnout_s.png) | `lowersnout` | ![](sprite-parity/ss14/snout_splotch_primary.png) | `snout_splotch_primary` | ⚠️ |

### To port

- [ ] ![](sprite-parity/ss13/unathi_head_markings__bandedface_s.png) `bandedface` — banded muzzle stripes
- [ ] ![](sprite-parity/ss13/unathi_head_markings__facenarrow_s.png) `facenarrow` — narrow mask
- [ ] ![](sprite-parity/ss13/unathi_head_markings__facesharp_s.png) `facesharp` — sharp mask
- [ ] ![](sprite-parity/ss13/unathi_head_markings__pointsface_s.png) `pointsface` — blocked muzzle
- [ ] ![](sprite-parity/ss13/unathi_head_markings__sharptiger_s.png) `sharptiger` — tiger, sharp snout
- [ ] ![](sprite-parity/ss13/unathi_head_markings__tigerface_s.png) `tigerface` — tiger, face only

> **Correction:** an earlier draft of this file said `Head: limit 1` had to be raised or "only one
> will ever be selectable". That was wrong. `MarkingsLimits.Limit` is *"How many markings this
> layer can take"* — i.e. how many a character may wear **at once**, not how many exist in the
> catalogue. With `limit: 1` you still pick freely from all head markings; you just cannot stack
> two. That matches Paradise, which also allows one head marking, so the limit is left alone.

---

## 4. Body Markings

SS13 `unathi_body_markings.dmi` (4) → SS14 `Chest` markings (4)

| SS13 | name | SS14 | name | status |
|:--:|---|:--:|---|---|
| ![](sprite-parity/ss13/unathi_body_markings__banded_s.png) | `banded` | ![](sprite-parity/ss14/body_tiger.png) | `body_tiger` | ✅ |
| ![](sprite-parity/ss13/unathi_body_markings__belly_s.png) | `belly` | ![](sprite-parity/ss14/body_underbelly.png) | `body_underbelly` | ✅ |

### To port

- [ ] ![](sprite-parity/ss13/unathi_body_markings__points_s.png) `points` — dark limb points
- [ ] `stripe` — **preview renders blank**; the south cell is empty. Open the DMI directly
      before deciding whether there is anything to port.

---

## 5. SS14-only — nothing to do

These have no Paradise counterpart. Listed so nobody tries to "fix" them by removing them,
and so you know what a Paradise player gains by moving to your fork.

**Horns (7):** `horns_angler` `horns_ayrshire` `horns_bighorn` `horns_demonic` `horns_myrsore`
`horns_kobold_ears` `horns_floppy_kobold_ears`

**Frills (3):** `frills_axolotl` `frills_divinity` `frills_neckfull`

**Snouts (2):** `visage_round` `visage_sharp`

**Body (2):** `body_backspikes` `body_fin`

**Limbs (4):** `l_arm_tiger` `r_arm_tiger` `l_leg_tiger` `r_leg_tiger` — Paradise has no per-limb layer at all.

**Tails (6 designs × static/behind/front/wagging):** `tail_smooth` `tail_large` `tail_spikes`
`tail_ltiger` `tail_dtiger` `tail_aquatic` — Paradise Unathi have exactly one fixed tail
(`tail = "sogtail"`), not a choice.

---

## Notes

**Blank previews.** 11 previews render as bare checkerboard because their south-facing cell is
genuinely empty: every `tail_*_front` (the front layer only draws when facing away),
`snout_round` / `snout_sharp` (art lives in the `*light` variants), `body_backspikes`,
`body_fin`, and SS13 `stripe`. Not extraction failures.

**Why `_webbing` and `_primary`/`_secondary` matter.** Both engines split two-tone markings into
two sprites so each half can be tinted separately. A Paradise `X` + `X_webbing` pair becomes an
SS14 `X_primary` + `X_secondary` pair — port both halves or the marking will be flat.

**Do not enable the Hair / FacialHair layers.** Reptilian sets `Hair: limit 0` and
`FacialHair: limit 0` deliberately. Paradise stores horns and frills in its *hair* slots because
that slot already existed; SS14 stores the same designs as `HeadTop` / `HeadSide` markings.
Turning those layers on gives Unathi human hairstyles.

**Regenerating these previews.** Scripts live in the session scratchpad:

```bash
node preview.js dmi <file.dmi> <state> <out.png>   # Paradise sheet → preview
node preview.js rsi <file.png>         <out.png>   # SS14 RSI → preview
node dmi2rsi.js  <file.dmi> <state> <out.png>      # full 4-direction extract, RSI layout
```

`dmi2rsi.js` is the one to use for the actual port — it was validated pixel-identical against
`goliath_hide.png` and `lizard_hide.png` already in the repo.

---

**Totals:** 20 sprites definitely need porting (8 horns, 4+ frills, 6 head, 2 body), plus up to 4
webbing layers depending on which ⚠️ matches you accept.
