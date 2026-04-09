# SMC SuperFib Indicator

SMC SuperFib is a **TradingView Pine Script v6** indicator designed around Smart Money Concepts (SMC) and ICT-style dealing ranges. It combines multi-session Fibonacci anchors, confluence-derived SuperFib zones, structure confirmation, and execution filters to help identify cleaner premium/discount setups.

The current script in this repository is:

- **Name:** `SMC SuperFib v10.12.1 – SF Engine Redesign`
- **Source file:** `SMC-SuperFib-Indicator-v-10.10.txt` (contains the v10.12.1 script)

---

## What the indicator does

SMC SuperFib overlays several market-structure and confluence components on the chart:

- **Session Fib Engine (F1/F2/F3):** Tracks multiple session ranges (daily/weekly/monthly/yearly depending on chart context).
- **SuperFib Zones:** Builds weighted confluence anchors and renders a full ladder of SF levels.
- **BOS/MSS Structure Logic:** Marks regime and structure transitions.
- **Compression Guard:** Filters out low-quality fib construction during chop/compression.
- **Entry Fib (EF) Layer:** Provides proximity-aware entry context.
- **ICT Hybrid Engine:** Adds additional directional/context scoring and table output.
- **Signal Lifecycle Controls:** Includes validity windows, RR checks, and optional alert payload support.

---

## Core architecture (v10.12+)

The SuperFib engine is organized into three stages:

1. **Anchor Engine**
   Uses confluence across F1/F2/F3 to produce one weighted SF high/low anchor (requires at least 2 valid fibs).

2. **Ladder Engine**
   Iterates all configured fib ratios unconditionally to render the full canonical SF ladder whenever SF is valid.

3. **Star Engine**
   Assigns star tiers by distance from equilibrium (50%), then enforces monotonic progression and clamps output.

In v10.12.1, drawing is render-only and consumes final star values directly (no recalculation inside draw functions).

---

## Inputs and controls

The script exposes grouped settings (hidden in TradingView input display metadata) covering:

- **Display** (labels, zones, arrows, kill zones, period lines, density)
- **Fib visibility** (F1/F2/F3, SuperFib, ICT fib, EF)
- **Engine tuning** (tolerance, swing lookback, BOS thresholds, min SF range)
- **Compression guard** (minimum swing range and displacement body%)
- **Manual fib overrides**
- **EF settings** (colors, proximity, rolling anchors, pre-trigger behavior)
- **SL/TP display**
- **ICT hybrid settings**
- **Execution filter and signal lifecycle**

---

## Installation (TradingView)

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
- Backtest and forward-test before live deployment.

---

## Repository contents

- `SMC-SuperFib-Indicator-v-10.10.txt` — full Pine Script source (currently v10.12.1).
- `README.md` — project documentation.

---

## License

No license file is currently provided in this repository. If you plan to share or reuse this code, add an explicit license first.
