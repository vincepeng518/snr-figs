# MSNR Playbook v3
# Full-map bias + Fresh-only entries
# Drop into AGENTS.md / Codex skill. Do not mix generic S/R folklore.

Status: operational spec, not a holy grail.
80–90% win-rate claims are marketing. Skip unclear charts.

---

## 0. Split that v2 got wrong

v2 treated Unfresh as “ignore”. That is only true for **entries**.

| Question | Uses Fresh | Uses Unfresh | Uses flips (RBS/SBR) |
|---|---|---|---|
| Where is price in the map? | Yes | Yes | Yes |
| What is Daily / H4 / intraday bias? | Yes | Yes | Yes |
| Where is the *destination*? | Yes (next same-TF Fresh) | No | Only if the flip is Fresh again |
| Where may I enter? | Yes | No | Only if Fresh in the new role |

Two layers. If they conflict, no trade.

| Layer | Job | Always on? |
|---|---|---|
| A. Marking + bias | Full map, storyline, destination, invalidation | Yes |
| B. Execution | Optional candle filter, ATR stop buffer, 1.5R | Optional house rules |

---

## 1. Definitions (locked)

### 1.1 Marking order: Daily → H4. H1 does not create levels.

1. Chart type **Line** (close-to-close). Placement ignores wicks.
2. **A-level** = peak on the line. Default role: resistance while price is below it.
3. **V-level** = valley on the line. Default role: support while price is above it.
4. Switch back to candles.
5. **Gap / OCL** = gap between candle-1 **close** and candle-2 **open** when both candles are the **same color**.
6. Draw **one price**. A stop buffer is Layer B, not a painted zone.

Do not promote a level because it bounced 2–3 times. Repeat bounces usually mean Unfresh.

### 1.2 State machine

| State | Meaning | Trigger on *this level’s TF* | Map role | Entry role |
|---|---|---|---|---|
| Fresh | Untouched | Created; no wick of this TF has tagged it | Wall | Allowed if bias agrees |
| Unfresh | Tested | Wick tagged the price; body did **not** close through | Road | Forbidden |
| Broken | Role change | **Body close** through | Relabel, keep on map | — |
| RBS | Resistance became support | Body close up through resistance | Support-type on map; may reset Fresh | Allowed only after reset to Fresh |
| SBR | Support became resistance | Body close down through support | Resistance-type on map; may reset Fresh | Allowed only after reset to Fresh |

Rules:
- Wick = test. Body close = break / flip.
- After a flip the level **may become Fresh again** until the next wick of that TF tags it.
- An H1 wick does **not** unfresh a Daily or H4 level.
- The level that just produced the storyline reject is usually Unfresh afterwards. **Keep the bias. Do not re-enter that same print.**

### 1.3 Tag format

```text
TF | price | A or V or GAP | origin or RBS or SBR | Fresh or Unfresh | above or below | wall or road
```

`wall` = Fresh, expected to matter as defense / destination.  
`road` = Unfresh, expected to be spent on the way to the next wall.

Example:

```text
D1 | 2648.5 | A | origin | Fresh   | above | wall
D1 | 2632.0 | A | origin | Unfresh | above | road
H4 | 2621.0 | V | RBS    | Fresh   | below | wall
```

### 1.4 Inventory

- Max **3 Daily + 3 H4** nearest to price, **including Unfresh**.
- H1 / M15: execution only. No new levels. No HTF freshness judged from LTF wicks.
- Relabel broken levels. Do not silently delete flip candidates.
- Weekly: drop levels that are no longer on the current swing’s path.

---

## 2. Bias engine (full map)

Bias answers: where did price come from, which way is the story, what is the magnet, what is only pavement.

### 2.1 Build the Daily map first

List every live Daily tag (Fresh + Unfresh + RBS/SBR). Then:

1. Nearest Daily **wall** above = destination if bias is up.  
   Nearest Daily **wall** below = destination if bias is down.
2. Unfresh Daily prints between price and that wall = **road**. Expect them to give way more often than they hold.
3. Last **Daily body** event:
   - Close away from a Daily level after a tag = reject (that level is now usually Unfresh; story can still start there).
   - Close through a Daily level = break / flip.
4. After a reject, did Daily **close** beyond the prior Daily swing? That confirms storyline follow-through.
5. If the path to the next Daily wall is only Unfresh noise and **no** Fresh destination exists in range → Daily bias **Neutral**, confidence low.

Daily bias =

- **Bullish** — last meaningful Daily event is support-type reject or bullish flip + follow-through, destination = next Daily Fresh A (or Fresh resistance-type) above.
- **Bearish** — mirror, destination = next Daily Fresh V (or Fresh support-type) below.
- **Neutral** — no reject + follow-through, or no Fresh Daily destination, or last Daily close killed the story.

Daily invalidation = Daily **body close** through the level that defined the story, or Daily close that removes the Fresh destination without a new one.

### 2.2 H4 vs Daily

H4 is the road surface. Daily is the city.

| H4 vs Daily | Label | How Unfresh is read |
|---|---|---|
| H4 structure agrees with Daily | Continuation | H4 Unfresh in front = pavement toward the Daily wall |
| H4 pulling into a Daily/H4 wall that agrees with Daily | Roadblock / pullback | Wait at the **Fresh** pullback wall; Unfresh pullbacks are not entries |
| H4 storyline opposite Daily | Conflict | Flat. Unfresh does not “vote” to override Daily |

H4 invalidation = H4 body close through the H4 print that justified the H4 label.

### 2.3 Intraday bias — pick exactly one

| Daily | H4 | Intraday bias | Map reading | Entry |
|---|---|---|---|---|
| Bullish | Continuation | Long bias | Drive toward next Daily wall; Unfresh overhead is road | Long only at Fresh V / Fresh RBS |
| Bullish | Roadblock pullback | Buy dips | Same destination; wait for the Fresh pullback wall | Long only at that wall |
| Bullish | Conflict | Flat | Still map Daily destination; do not act | None |
| Bearish | Continuation | Short bias | Drive toward next Daily wall below | Short only at Fresh A / Fresh SBR |
| Bearish | Roadblock pullback | Sell rallies | Wait for Fresh pullback wall | Short only at that wall |
| Bearish | Conflict | Flat | Map only | None |
| Neutral | Anything | Flat | Map only | None |

Valid until the earlier of Daily invalidation close or H4 invalidation close.

No “small size against trend”. Against-trend = Flat.

### 2.4 How Unfresh is allowed to influence bias (and how it is not)

Allowed:
- Treat Unfresh as **lower odds of holding**, so bias may look *through* it to the next Fresh wall.
- After a storyline reject, keep the bias even though that print is now Unfresh.
- If several Unfresh levels stack in front of price and the next Fresh wall is far / unclear → **downgrade confidence**, maybe Neutral.

Forbidden:
- “Unfresh therefore reverse here.”
- “Unfresh therefore I may enter at half size.”
- Using an Unfresh M15/H1 line to set Daily bias.
- Calling a Daily wick on an Unfresh level a fresh storyline start. Need a Daily close-away or Daily close-through.

---

## 3. Entry flowchart (Fresh only)

```text
0. Build full map (Fresh + Unfresh + flips).
   Compute Daily bias, H4 label, Intraday bias, destination wall.
   If Intraday bias = Flat -> STOP.

1. Price approaches a tagged Daily or H4 level
   on the correct side of the bias
   (longs at support-type, shorts at resistance-type).

2. State of THAT level on its own TF?
   Unfresh -> STOP. It is road, not an entry.
   Fresh   -> continue.

3. Interaction on the level’s own TF
   (Daily judged on Daily close; H4 on H4 close).

   A. Wick tags, body does not close through
      -> valid test of a wall. Go to confirmation.
      -> after this wick the level becomes Unfresh.
         You may still take THIS first reaction.
         You may not take the next tag of the same print.

   B. Body closes through
      -> leave reversal branch.
      -> Break-Retest branch.
      -> level flips (RBS/SBR) and may reset Fresh.

   C. Price never reaches the level
      -> no trade.

4. Confirmation
   Required: close back away from the Fresh level in the bias direction.
   Optional Layer B: engulfing / pin on H4 or H1 sitting ON the HTF level.
   Filter ON and missing -> no trade.
   Filter OFF -> HTF close-away is enough.

5. Entry
   SL: beyond the level + Layer-B ATR buffer (buffer is not a zone on the chart).
   TP1: next opposing Fresh wall on the setup TF.
   TP2: Daily destination wall.
   Path may pass through Unfresh prints; those are not extra TPs.
   Min R to TP1 after buffer: 1.5. Else skip.

Break-Retest:
   Body close through -> WAIT. No chase on the break candle.
   Pullback to the flipped print.
   Enter only if it is Fresh in the new role AND rejects.
   No retest within 2-3 candles of the TF that broke it
   (Daily break -> 2-3 Daily; H4 break -> 2-3 H4) -> abandon.
```

First-touch exception, written so Codex cannot stretch it:
- The touch that **starts** the reaction may be taken while the level is still Fresh at the open of that interaction.
- The moment the wick tags it, mark Unfresh for all **later** setups.
- Do not queue a second order on that same print.

---

## 4. Layer B — house rules (optional)

Default for Codex: **OFF** unless the user turns them on.

- Candle filter ON/OFF.
- SL buffer: FX 3–5 pips **or** `0.25 × ATR(14)` on the execution TF. XAU / indices: ATR only.
- Size: full size only on Fresh + aligned Daily and H4. Zero size on Unfresh. Zero size against bias.
- Skip red-news spikes and first minutes of London / NY as “normal touches”.
- Journal: tags, wall/road, bias, destination, interaction, result.

---

## 5. Codex I/O

### Input

```text
Symbol:
Timezone:
Last price:
Session: Asia / London / NY
Layer B filters: ON or OFF

DAILY tags (include Unfresh):
H4 tags (include Unfresh):
Last Daily reject or body-close break (or "unknown"):
Screenshots: Daily + H4 if available
```

### Output (mandatory sections)

```text
### 1. Verdict
- Price
- Daily bias
- Daily destination wall (Fresh only)
- Daily roads in the way (Unfresh list)
- H4 label: continuation / roadblock / conflict
- Intraday bias: long / buy-dips / short / sell-rallies / flat
- Confidence: high / mid / low — one reason
- Valid until

### 2. Daily map
- Storyline (one sentence)
- Walls
- Roads
- Invalidation close

### 3. H4 map
- Relation to Daily
- Walls
- Roads
- Invalidation close

### 4. Today
- Actionable Fresh level(s)
- Do-not-trade range (includes all Unfresh prints)
- What would flip bias

### 5. Plan (omit if flat)
- IF price reaches [Fresh level] AND [interaction]
  THEN [direction]
  SL [price]  TP1 [Fresh wall]  TP2 [Daily destination]
- Unfresh prints between entry and TP are path, not targets
- ELSE no trade

### 6. Disclaimer
Structure reading, not an order. Not financial advice.
```

Hard bans:
- Do not invent prices.
- Do not paint zones as the level.
- Do not drop Unfresh from the map.
- Do not enter Unfresh.
- Do not call a wick a Daily reject.
- Do not let H1/M15 create HTF levels or HTF freshness.
- Do not output against-trend “small size”.
- Missing tags → ask. Do not guess A/V from wicks.

---

## 6. Examples

**A. Bias through Unfresh, enter at Fresh**
- Daily bearish. Destination wall = Daily Fresh V at 2580.
- Overhead Daily 2610 A is Unfresh = road.
- H4 continuation. Intraday = short.
- H4 Fresh A at 2602 is the entry wall.
- Plan: short a first reaction at 2602. Expect 2610 may not hold if price spikes through it later. TP1 next H4 Fresh V, TP2 2580. Do not short 2610 itself.

**B. Storyline reject consumes the print**
- Daily V at 2620 was Fresh. Daily wicks it and closes away up. Story = bullish toward Daily Fresh A 2680.
- 2620 is now Unfresh. Bias stays bullish. Do **not** buy 2620 again.
- Next long is a Fresh H4 V / Fresh RBS on the pullback, or a new Fresh Daily support if one prints.

**C. Daily RBS**
- Daily body closes through resistance → RBS, state may reset Fresh.
- H4 later tags that RBS while it is still Fresh → buy-dips if Daily is bullish.
- If an H4 wick already spent that RBS before you arrived → Unfresh → map it as support-type road, do not buy it.

**D. Conflict stays flat even with pretty Unfresh**
- Daily bullish toward 2650 Fresh A.
- H4 making lower highs, closing through its own V.
- Intraday = flat. 2650 stays on the map as destination. No long in H4 weakness. No short just because H4 Unfresh supports are failing.

**E. No destination wall**
- Nearby Daily prints are all Unfresh. Next Fresh Daily is off the visible swing.
- Daily bias Neutral, confidence low, intraday flat. Chart the roads. Do not force a destination.

---

## 7. Mistakes

- Building bias only from Fresh (blind map).
- Entering Unfresh because “bias points through it”.
- Re-entering the level that just created the storyline.
- Treating A as “strong enough to fade Unfresh”.
- Drawing a wick-box and calling it the MSNR level.
- Using bounce count to decide a level is valid.
- Chasing the break candle.
- Letting M15 wicks unfresh Daily.
- Taking Unfresh as TP just because price will pass them.
- No journal of wall vs road, so the split cannot be audited.

---

## 8. Session header

```text
MSNR v3
Symbol:
Price:
Filters (Layer B): OFF
Daily tags (Fresh AND Unfresh):
H4 tags (Fresh AND Unfresh):
Map first. Bias from the full map. Entries Fresh only.
If no Fresh destination, bias is Neutral / flat.
```

---

## 9. One-liner for the model

```text
Full map = Fresh walls + Unfresh roads + flips.
Bias looks through roads toward the next same-TF Fresh wall.
Entries only on Fresh walls that agree with Daily×H4 bias.
The reject that starts a storyline usually leaves that print Unfresh;
keep the story, do not buy/sell that same print again.
```
