# SMC SuperFib Indicator

## Current version

**v10.14.0 Final Base — ICT Stabilization Patch**

The source file in this repository is `SMC-SuperFib-Indicator-v-10.10.txt`.
This is a legacy filename — it contains the **v10.14.0 Final Base** script.

---

## What the indicator does

SMC SuperFib is a TradingView Pine Script v6 overlay indicator built around Smart Money Concepts (SMC) and ICT-style dealing ranges. It combines multi-session Fibonacci anchors, confluence-derived SuperFib zones, structure confirmation, and execution filters to identify cleaner premium/discount setups.

Key components:

- **Session Fib Engine (F1/F2/F3):** Tracks multiple session ranges (daily/weekly/monthly/yearly depending on chart timeframe).
- **SuperFib Zones:** Builds weighted confluence anchors from F1/F2/F3 and renders a full ladder of SF levels with star ratings.
- **Staged Liquidity Sweep Engine:** Two-stage candidate → confirm pipeline. Price must raid a ranked liquidity pool and reclaim within a configurable window. Expiry on acceptance (2+ closes beyond raided level).
- **Post-Sweep MSS Confirmation:** MSS only fires after a confirmed sweep, within a configurable bar window, with a multi-factor displacement gate (body ratio, ATR range, FVG, extension past breached swing).
- **Entry Fib (EF) Layer:** Narrative-anchored execution fib with OTE zone, proximity filtering, and pre-trigger labels. Distinguishes narrative (sweep+MSS) anchoring from HTF year-fallback.
- **BOS/MSS Structure Logic:** Marks regime and structure transitions with configurable labels.
- **Compression Guard:** Filters out low-quality fib construction during chop/compression.
- **ICT Dashboard:** Real-time table showing structure, HTF PDA, bias, pressure, OTE zone, score, sweep state, displacement quality, EF source, sequence status, and signal lifecycle.
- **Signal Lifecycle Controls:** Validity windows, RR checks, SL/TP references, and optional alert/webhook payload support.
- **Kill Zones and Period Lines:** Optional session shading and vertical period markers.

---

## Architecture (v10.14.0)

The indicator is organized around four core engines:

### 1. Session Fib Engine (F1/F2/F3)
Computes three session-anchored Fibonacci ranges from daily, weekly, monthly, or yearly sessions (auto-detected or manually overridden). Each fib tracks high/low/direction independently.

### 2. SuperFib Confluence + Ladder + Star Engine (3-stage)
- **Anchor Engine:** Confluence across F1/F2/F3 builds one weighted-average SF high/low anchor (requires >= 2 valid fibs).
- **Ladder Engine:** Full canonical SF ladder iterates every level in fib_ratios unconditionally when SF is valid.
- **Star Engine:** Stars assigned by distance from equilibrium (50%) via EDE tier. Monotonic enforcement via running-max per side. Color tier capped by anchor quality.

### 3. Staged Sweep + MSS Engine (v10.14.0)
- **Stage A (Candidate):** Fires when price raids a ranked liquidity pool (EQH/EQL > PDH/PDL > PWH/PWL > swing).
- **Stage B (Confirm):** Confirmed when price reclaims within `sweep_confirm_bars`. Expired if 2+ closes accept beyond the raided level.
- **MSS Gate:** After confirmed sweep, MSS must fire within `mss_confirm_bars`. Displacement gate scores body ratio, ATR range, FVG presence, and extension past the breached swing. Requires score >= 2 when `require_disp_mss` is enabled.

### 4. Entry Fib (EF) Engine
Anchors execution fibs from sweep+MSS narrative legs. Falls back to current-year high/low when no valid leg is available (disabled on 1H and below by default via `ef_ltf_no_fallback`). Dashboard exposes whether EF is narrative-anchored or HTF-fallback.

---

## Inputs and controls

The script exposes grouped settings covering:

- **Display** — labels, zones, arrows, kill zones, period lines, chart density
- **Fib Visibility** — F1/F2/F3, SuperFib, EF toggles
- **Engine** — SF tolerance, swing lookback, BOS thresholds, min SF range, plus v10.14.0 additions:
  - `sweep_confirm_bars` — reclaim window for staged sweep confirmation (default 2, max 5)
  - `mss_confirm_bars` — MSS window after confirmed sweep (default 5, max 12)
  - `require_disp_mss` — require displacement score >= 2 for MSS (default true)
  - `ef_ltf_no_fallback` — disable EF year-fallback on 1H and below (default true)
- **Compression Guard** — minimum swing range and displacement body%
- **Manual Fib Overrides** — override F1/F2/F3 high/low manually
- **EF Settings** — colors, proximity, freshness, pre-trigger behavior, fallback toggle
- **SL/TP Display** — SL/TP lines on signals, buffer, style
- **ICT Hybrid Engine** — structure lookback, proximity filter, bias gate, ATR multiplier
- **Signal Lifecycle** — validity window, RR threshold, alerts, webhooks

---

## Installation

1. Open **TradingView**.
2. Go to **Pine Editor**.
3. Copy the full script from `SMC-SuperFib-Indicator-v-10.10.txt`.
4. Paste into the editor and click **Add to chart**.
5. Save as your own script version.

---

## Notes and caveats

- This is a **technical-analysis tool**, not financial advice.
- Signals can repaint intrabar depending on your alert/confirmation settings; prefer close-confirmed workflows when needed.
- Tune pip and threshold values per instrument class (e.g., USD vs JPY pairs).
- On 1H and below, EF year-fallback is disabled by default. Enable via setting "Disable year-fallback EF on 1H and below" to false if needed.
- Sweep state and MSS state are visible in the ICT dashboard table (rows: SWEEP STATE, DISPLACEMENT, EF SOURCE).
- Backtest and forward-test before live deployment.

---

## Repository contents

- `SMC-SuperFib-Indicator-v-10.10.txt` — full Pine Script v6 source (v10.14.0 Final Base).
- `README.md` — project documentation.

---

## License

No license file is currently provided in this repository. If you plan to share or reuse this code, add an explicit license first.
