# MSNR Playbook v4 — XAUUSD
# Daily rail survives Unfresh. Gold intraday bias lives on M5.
# Codex / AGENTS source. Not a holy grail.

Instrument: XAUUSD (gold). Do not copy this stack blindly onto FX majors.
80–90% win-rate claims are marketing.

---

## 0. What changed from v3

v3 killed Unfresh for entries on every TF. BTC Daily counterexample: an Unfresh Daily rail still launched the expansion after a tap. Freshness is a **quality score**, not an on/off switch at Daily.

Gold is liquid enough that a **5-minute storyline** is tradable. It is still nested under Daily and H4. M5 never votes down Daily.

| Layer | Job |
|---|---|
| A. Map + nested bias | D1 + H4 + M5 full maps. Unfresh stays on HTF rails. |
| B. Execution (optional) | Candle filter, ATR buffer, 1.5R. Default OFF. |

---

## 1. Gold stack (locked)

Only three marking TFs. Do not paint H1/M15 unless the user pastes them.

| TF | Role | Unfresh on map | Unfresh as bias rail | Unfresh as entry |
|---|---|---|---|---|
| **Daily** | Destination + storyline | Yes | **Yes — still a magnet** | Allowed if H4 confirms the tap |
| **H4** | Swing / session parent | Yes | Weak (road unless it sits on a Daily rail) | Skip unless it sits on a Daily rail |
| **M5** | Gold intraday bias + execution | Yes (nearest 3) | Only Fresh M5 walls define M5 story | **Skip** |

H1 / M15 = optional execution zoom. They do not create bias and they do not unfresh Daily/H4.

Price travels **same-TF Fresh wall → next same-TF Fresh wall**. Unfresh prints of that TF are pavement, except Daily Unfresh which remains a rail that can still reject.

---

## 2. Marking

1. Line chart (close-to-close) for A / V. Candles for Gap/OCL.
2. **A** = line peak (resistance while below). **V** = line valley (support while above). **Gap** = same-color close→next open hole.
3. One price, not a box. ATR buffer is Layer B, not the level.
4. Tag:

```text
TF | price | A or V or GAP | origin or RBS or SBR | Fresh or Unfresh | above or below | wall or road or rail
```

- `wall` = Fresh. Defense + destination + primary entry.
- `road` = Unfresh on H4 or M5. Look through it.
- `rail` = Daily Unfresh (or Daily Fresh). Stays in the story even after wicks. May still reject.

Inventory: max **3 Daily + 3 H4 + 3 M5** nearest to price.
M5 older than the current London/NY swing: drop. Do not archive a week of M5.

### State machine (same physics, different rights)

| State | Trigger on that TF | Daily rights | H4 rights | M5 rights |
|---|---|---|---|---|
| Fresh | No wick of this TF yet | wall + entry | wall + entry | wall + M5-bias + entry |
| Unfresh | Wick tagged; body did not close through | **rail + entry if H4 confirms** | road. Entry only if glued to a Daily rail | road. No entry. Does not set M5 bias |
| Broken | Body close through | flip, keep | flip, keep | flip, keep |
| RBS / SBR | Body close through, role reverse | may reset Fresh | may reset Fresh | may reset Fresh |

Wick ≠ break. Body close = break.
An M5 wick does **not** unfresh H4 or Daily.
The Daily print that just rejected is usually Unfresh/rail afterwards: **keep Daily bias, do not immediately re-tap that same Daily as a second entry without a new H4 confirm.**

---

## 3. Nested bias (this is the gold engine)

Compute bottom-up for the map, top-down for permission.

### 3.1 Daily

Full map: Fresh walls + Unfresh rails + flips.

- Destination = next Daily **Fresh wall** in the story direction. If none visible → confidence down, not auto-flat if a Daily **rail** is still holding price.
- Daily bias:
  - **Bullish** — last Daily body event is support-type reject (Fresh or rail) or bullish flip, and price is holding above the acting Daily rail.
  - **Bearish** — mirror.
  - **Neutral** — no Daily rail in play, or last Daily body close killed it, or price is stuck mid-range with both Daily rails already spent and no destination wall.
- Daily invalidation = Daily **body close** through the rail/wall that defined the story.

Daily Unfresh is allowed to **start or continue** bias. That is the v3 → v4 patch.

### 3.2 H4

H4 is the parent of M5.

| H4 vs Daily | Label |
|---|---|
| Agrees | Continuation |
| Pulling into Daily wall/rail in Daily direction | Roadblock |
| Opposite story | Conflict → **no gold directional book**. M5 may not invent one. |

H4 Unfresh in front of price = road toward the Daily destination, unless that H4 print is sitting on a Daily rail (then treat the Daily rail as the reason).

### 3.3 M5 — gold intraday bias

M5 bias exists only as a **child** of H4.

M5 storyline = last M5 Fresh reject or M5 body flip, aiming at the next M5 Fresh wall.

| Daily | H4 | M5 | Intraday (gold) | Allowed |
|---|---|---|---|---|
| Bull | Cont | Bull | **Long** | Buy M5 Fresh V / Fresh RBS that agree |
| Bull | Cont | Bear | **Buy dips** | Do not short the M5 story. Wait M5 Fresh V into H4/Daily rail |
| Bull | Roadblock | Bull/Bear | **Buy dips** | Only at the H4/Daily rail. M5 is timing |
| Bull | Conflict | any | **Flat** | No |
| Bear | Cont | Bear | **Short** | Sell M5 Fresh A / Fresh SBR |
| Bear | Cont | Bull | **Sell rallies** | Do not long M5. Wait M5 Fresh A into H4/Daily rail |
| Bear | Roadblock | any | **Sell rallies** | Only at the parent rail |
| Bear | Conflict | any | **Flat** | No |
| Neutral | any | any | **Flat** | Map only |

M5 Unfresh never sets M5 bias. If the only M5 prints in range are Unfresh → M5 bias **none**, fall back to H4 label (buy-dips / sell-rallies / flat).

M5 bias dies at the earliest of:
- M5 body close through the M5 wall that defined it
- H4 invalidation
- Daily invalidation
- scheduled gold news (FOMC / CPI / NFP / live minutes) — M5 bias off until the H4 after the event closes

---

## 4. Entry

```text
0. Map D1 (walls+rails), H4, M5.
   Nested bias table -> one label.
   Flat -> STOP.

1. Location must be a permitted print:
   - Daily Fresh wall, or
   - Daily Unfresh rail with H4 close-away in the bias direction, or
   - H4 Fresh wall, or
   - H4 Unfresh ONLY if price is also on a Daily rail, or
   - M5 Fresh wall that agrees with the nested label.

2. Forbidden locations:
   - M5 Unfresh
   - H4 Unfresh in the middle of nowhere
   - Any print against the nested label (no "small short" in a long book)

3. Interaction on the print's own TF
   (Daily judged on Daily close / H4 on H4 / M5 on M5).

   A. First reaction: wick tags, body does not close through
      -> take THIS reaction if step 1 allowed it.
      -> mark Unfresh/rail after the wick for later orders.

   B. Body close through
      -> no chase.
      -> wait flip retest. Enter only if the flip is Fresh
         (Daily Unfresh rail retest still allowed with H4 confirm).

   C. Never reaches the print -> no trade.

4. Gold confirmation
   Daily rail tap: require H4 close away (or M5 BOS in Daily direction
   if the user is scalping the tap — still no trade if H4 is in Conflict).
   M5 wall tap: M5 close away is enough when nested label already allows it.

5. Stops / targets
   SL: beyond the print + 0.25 ATR of the execution TF (M5 ATR if entering off M5).
   TP1: next opposing Fresh wall on the execution TF.
   TP2: parent rail/wall (H4 then Daily).
   Unfresh M5/H4 between entry and TP = path, not extra TPs.
   Daily Unfresh between entry and Daily destination = path that may still
   react; do not flatten just because it was tagged.
   Min 1.5R to TP1 after buffer, else skip.
```

---

## 5. Worked gold sketches

**A. Daily rail still works (the BTC lesson, applied to XAU)**  
Daily V Unfresh, price mid-range chops on it, later taps and expands up.  
Daily bias stays bullish while bodies hold above that rail.  
Entry is not “because M5 looked nice in the chop”. Entry is the tap of the Daily rail + H4 close away. M5 only times it.

**B. Gold London long**  
Daily bull, H4 continuation, M5 prints a Fresh V during a shallow pullback.  
Intraday = long. Buy M5 Fresh V. SL under M5 V + M5 ATR. TP1 next M5 Fresh A, TP2 H4/Daily wall.

**C. Do not short gold M5 against Daily**  
Daily bull, H4 continuation, M5 Unfresh A failing and M5 looks bearish.  
Intraday = buy dips, not short. Wait M5 Fresh V. Shorting that M5 story is the error v4 exists to block.

**D. M5 bias without a Daily destination wall**  
No Daily Fresh above, but Daily Unfresh rail underneath is holding.  
Daily still bullish (rail). Confidence mid. M5 longs allowed only into/holding that rail, not as a naked M5 scalp in the vacuum.

**E. News**  
CPI in 20 minutes. M5 bias off. Daily map stays. After the H4 that contains the release closes, rebuild M5.

---

## 6. Codex I/O

### Input

```text
MSNR v4 XAU
Price:
Session: Asia / London / NY
News in next 60m: yes/no
Layer B: OFF

Daily tags (Fresh AND Unfresh rails):
H4 tags:
M5 tags (current swing only):
Last Daily body event (reject / close-through / unknown):
```

### Output

```text
### 1. Verdict
- Daily bias + acting rail/wall + destination
- H4 label
- M5 bias (or none)
- Intraday: long / buy-dips / short / sell-rallies / flat
- Confidence: high / mid / low
- Valid until

### 2. Daily map
walls / rails / invalidation

### 3. H4 map
vs Daily / walls / roads

### 4. M5 map
M5 Fresh walls only for bias
Unfresh M5 listed as do-not-enter

### 5. Plan (omit if flat)
IF [print] AND [confirm TF close-away] THEN [direction]
SL / TP1 / TP2
```

Hard bans:
- Do not let M5 unfresh Daily or H4.
- Do not set gold intraday from M5 against H4 Conflict.
- Do not skip Daily Unfresh on the map.
- Do not enter M5 Unfresh.
- Do not invent prices.
- News window: no M5 plan.

---

## 7. Mistakes

- Treating Daily Unfresh as dead (v3 error).
- Treating M5 Unfresh as a Daily-style rail (opposite error).
- Painting 20 M5 lines and calling it bias.
- Shorting gold M5 because the micro story flipped while Daily rail is holding.
- Using Asia M5 chop to override London H4.
- Re-entering the same Daily rail twice in one Daily bar without a new H4 confirm.

---

## 8. One-liner

```text
Gold: Daily Unfresh is still a rail. H4 is the parent. M5 is the child bias.
Entries: Daily rail (H4 confirm) or Fresh H4/M5 in nested direction.
M5 Unfresh is noise. M5 never outvotes Daily.
```
