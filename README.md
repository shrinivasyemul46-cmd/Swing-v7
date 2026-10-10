# 🎯 NSE TOP-DOWN SWING PRO — V7

World → India → Sectors → **Market-cap & quality filters** → Stocks → Top 5 → Reward:Risk plan → Backtest → Journal.

## Files (all 4 must be in the GitHub repo root)
`app.py` (screen) · `engine.py` (all logic) · `requirements.txt` · `README.md`
Replace the old V6 files, commit, then **Reboot** the Streamlit app.

## Run locally
```bash
pip install -r requirements.txt
streamlit run app.py
```
Scan AFTER market close (after ~4 PM IST). Today's incomplete candle is ignored by default.

## What is new in V7 (vs V6)

### 1. Market cap replaces "must be in Nifty 500"
* New sidebar section **🏷️ Quality filters**: "Enforce minimum market cap" (default ON, ₹1,000 Cr) and a minimum
  share price (default ₹20, avoids penny / frequent-circuit names).
* The filter is applied to **whatever universe you pick**, including a new, wider universe option
  **"All NSE equities (EQ series, wider than Nifty 500)"** — so "market cap > 1000 Cr" genuinely gives you
  *more* candidates (any sized NSE company above the cutoff), not just a filtered-down Nifty 500.
* Market cap comes from yfinance (Yahoo), cached 24h, fetched **before** the price-history download so excluded
  stocks never waste download time. A stock Yahoo has no market-cap data for is dropped, not assumed to pass.
* **Honesty note:** market cap is TODAY's value — there is no reliable free historical market-cap series, so this
  is a one-time universe filter, not something the Backtest tab can replay. Treat the Backtest numbers as "this
  setup's history across large/liquid stocks in general," not "this exact universe's history."

### 2. New setup: Weekly Accumulation
Built from the volume-accumulation idea you shared: flags a stock whose **weekly** volume has been rising for
6 straight comparison points (vs 2, 3, 4, 5, 9 and 13 weeks ago) while price held or rose over the last week and
last ~4 months. Computed on properly resampled weekly bars (forming week excluded), not just daily data pretending
to be weekly. Shows up in the Setup column as "Weekly Accumulation" and is scored and backtested exactly like the
other setups — check its row in the Backtest "By setup" table before trusting it.

### 3. Two new safety gates (apply everywhere, every setup)
* **Circuit/no-range gate:** a bar where High == Low (frozen at a circuit limit, or a stale/illiquid print) is
  rejected outright.
* **Minimum price gate:** blocks stocks below your "Minimum share price" — penny stocks are disproportionately
  prone to manipulation and exchange circuit filters, which distort both technicals and fills.

## Everything from V6 is unchanged
World/India regime, sector strength + top-sector trading, trendline/63D-breakout/sweep/pullback setups, the
reward:risk resistance gate, 3 entry styles × 2 exit styles backtest comparison, position sizing capped to your
capital, fundamentals check, journal. See the in-app "📖 वापरण्याची पद्धत" expander for the week-by-week routine.

## Performance note
Market-cap fetching is one network call per stock (same limitation as yfinance price downloads). For the full
"All NSE equities" universe (~2,000 symbols) this can be slow on first run; it is cached for 24 hours, and you can
set "Scan first N stocks" to bound it. Nifty 500 / Nifty 200 stay the fast path.

## Important
* Score is NOT a probability. Only the Backtest tab's per-setup Win% / Avg R, built from real history, tells you that.
* Backtest limits: today's universe membership (survivorship bias), free data, no slippage beyond the cost
  setting; regime, sector and market-cap rules are NOT replayed in the backtest (no history for them).
* yfinance is unofficial and can be delayed, rate-limited, or missing data for some symbols. Verify prices,
  market cap, results dates and corporate actions with NSE / your broker before trading.
* Educational tool, not investment advice.
