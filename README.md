# Options Pricer — Black-Scholes, Greeks, and Newton-Raphson Implied Volatility Solver

## What this project does

Implements the full pricing engine underneath any options desk, from first principles:
- Black-Scholes call and put pricing
- All four core Greeks (Delta, Gamma, Vega, Theta), implemented analytically
- A finite-difference cross-check proving the analytical Greeks are actually correct
- A Newton-Raphson solver that recovers implied volatility from a price
- Sensitivity analysis across spot, volatility, and time
- Real pricing examples using the actual current NIFTY 50 spot and India VIX

## What's in this repo

| File | Description |
|---|---|
| `options_pricer_greeks_iv_solver.ipynb` | The full notebook |
| `chart_01_price_vs_spot.png` | Option price vs spot price (payoff curvature / Gamma) |
| `chart_02_price_vs_vol.png` | Call price vs volatility (Vega in action) |
| `chart_03_price_vs_time.png` | Call price vs time to expiry (Theta decay) |
| `README.md` | This file |

## Data — real only, no synthetic fallback

This notebook uses the real, current NIFTY 50 spot price and real, current India VIX, pulled live via `yfinance`. **There is no synthetic data anywhere in this notebook.** If the fetch fails (no internet), the notebook raises a clear error and stops.

**Honest limitation, stated up front:** NSE does not publish free historical per-strike option chain data. This notebook does not claim to reproduce actual traded option prices — it prices options using Black-Scholes with real spot and real VIX as the volatility input, which is a disclosed, standard substitute, consistent with the other two projects on this CV.

## Method

1. **Black-Scholes pricing**, implemented from the formula — no pricing library used.
2. **Analytical Greeks** (Delta, Gamma, Vega, Theta), also from the formula.
3. **Finite-difference cross-check** — each Greek is independently re-derived numerically (by nudging an input slightly and measuring the price change) and compared against the analytical value. This is the step that proves the formulas were implemented correctly, rather than just trusting a textbook derivation was typed in right.
4. **Newton-Raphson implied volatility solver** — tested by first pricing an option at a *known* volatility, then solving backward and confirming the solver recovers that same volatility. This validates the solver before it's trusted on anything else.
5. **Sensitivity analysis** — price and Greeks plotted against spot, volatility, and time, so the Section 3 numbers are shown in context rather than as an isolated snapshot.
6. **Real pricing examples** — a small grid of ITM/ATM/OTM strikes across weekly and monthly expiries, priced using today's actual spot and VIX.

## Honest conclusion

This notebook demonstrates correct, independently-verified Black-Scholes pricing and Greeks, and a working, tested implied volatility solver — the real mechanics underneath an options pricing engine.

**What it doesn't claim:** that these are tradeable market prices. Real NSE option prices deviate from theoretical Black-Scholes values due to the volatility smile/skew, bid-ask spread, and liquidity effects — none of which are modeled here, since that would require real historical option chain data that isn't freely available. This notebook is the pricing engine itself, built and verified from first principles; a real desk would layer a volatility surface on top of it, not replace it.

## How to run

Fully Colab-compatible. No local setup, no paid data, no GPU required.

```bash
pip install yfinance --quiet
```

Then run all cells top to bottom. Since there is no synthetic fallback, an internet connection is required — Colab has one by default.

## Requirements

```
numpy
pandas
matplotlib
scipy
yfinance
```
