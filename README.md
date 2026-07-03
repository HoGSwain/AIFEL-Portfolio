# AIFEL — AI-Augmented Financial Engineering Laboratory

A 7-project portfolio building a coherent, **reproducible** quantitative-finance
stack in Python. Each project is an independent, tested library + CLI, and later
projects build directly on earlier ones instead of re-implementing shared logic —
starting from a rigorous data foundation and working up through portfolio
construction, risk, fixed income, options, and credit.

> **Design philosophy:** one standardized, tested pipeline per concern; every
> significant design decision documented and justified; deterministic, offline
> reproducibility wherever possible; and real automated tests, not just notebooks.

## Projects

| # | Project | Scope | Status |
|---|---------|-------|--------|
| 1 | [**Financial Market Data Engine**](https://github.com/HoGSwain/financial-market-data-engine) | Acquire → validate → clean → engineer features → store → report for market data. The data foundation every later project imports. | ✅ **Live** |
| 2 | Portfolio Analytics | Returns, risk/return metrics, correlation and performance analytics on multi-asset portfolios, built on the FMDE data layer. | 🔜 Planned |
| 3 | Portfolio Optimization | Mean-variance / efficient-frontier allocation and related optimization methods. | 🔜 Planned |
| 4 | Risk Analytics | Value-at-Risk, Expected Shortfall, and scenario/stress risk measures. | 🔜 Planned |
| 5 | Bond Analytics | Pricing, yield, duration, and convexity for fixed-income instruments. | 🔜 Planned |
| 6 | Option Pricing | Black–Scholes and numerical (binomial / Monte-Carlo) pricing and Greeks. | 🔜 Planned |
| 7 | Credit Risk | Default probability, credit scoring, and credit-portfolio risk modeling. | 🔜 Planned |

## Foundation: Project 1

Project 1 — the **Financial Market Data Engine (`fmde`)** — is the shared data
layer. Every later project imports `fmde.pipeline.run_pipeline` rather than
re-downloading and re-cleaning market data, so methodology stays consistent and
results stay reproducible across the whole portfolio.

- Six-stage pipeline: acquisition → validation → cleaning → feature engineering → storage → reporting
- Pluggable providers (live Yahoo Finance + an offline, deterministic synthetic source)
- CSV / Parquet / SQLite outputs with JSON metadata (checksum, row count, versions)
- Python library **and** Typer CLI; 27 passing `pytest` tests

## Tech stack

Python 3.10+ · pandas · numpy · pyarrow · Typer · loguru · pytest · ruff · black

## License

MIT
