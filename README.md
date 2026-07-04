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
| 1 | [**Financial Market Data Engine**](https://github.com/HoGSwain/financial-market-data-engine) | Acquire → validate → clean → engineer features → store → report for market data. The data foundation every later project imports. | ✅ **Live** · [![CI](https://github.com/HoGSwain/financial-market-data-engine/actions/workflows/ci.yml/badge.svg)](https://github.com/HoGSwain/financial-market-data-engine/actions/workflows/ci.yml) |
| 2 | [**Portfolio Analytics Engine**](https://github.com/HoGSwain/portfolio-analytics-engine) | Returns, volatility, Sharpe, Sortino, max drawdown, beta, and alpha for one or more tickers — built on Project 1 as a real dependency. | ✅ **Live** · [![CI](https://github.com/HoGSwain/portfolio-analytics-engine/actions/workflows/ci.yml/badge.svg)](https://github.com/HoGSwain/portfolio-analytics-engine/actions/workflows/ci.yml) |
| 3 | [**Portfolio Optimization Engine**](https://github.com/HoGSwain/portfolio-optimization-engine) | Minimum-variance and maximum-Sharpe (tangency) portfolios and the efficient frontier (mean-variance / Markowitz) — built on Projects 1–2. | ✅ **Live** · [![CI](https://github.com/HoGSwain/portfolio-optimization-engine/actions/workflows/ci.yml/badge.svg)](https://github.com/HoGSwain/portfolio-optimization-engine/actions/workflows/ci.yml) |
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
- Python library **and** Typer CLI; 27 passing `pytest` tests, run in CI on Linux + Windows × Python 3.10 & 3.12

## Project 2: Portfolio Analytics Engine

Built directly on Project 1, the **Portfolio Analytics Engine (`pae`)** turns
cleaned returns into performance and risk metrics. It imports
`fmde.pipeline.run_pipeline` and re-implements no acquisition, validation, or
cleaning of its own.

- Metrics: annualized return & volatility, Sharpe, Sortino, maximum drawdown, CAPM beta, Jensen's alpha
- Every formula documented (`docs/methodology.md`) and validated against textbook values / mathematical invariants
- Python library **and** Typer CLI; 39 passing `pytest` tests, run in CI on Linux + Windows × Python 3.10 & 3.12

## Project 3: Portfolio Optimization Engine

Built on Projects 1–2, the **Portfolio Optimization Engine (`poe`)** turns each
asset's return and risk into optimal allocations. It imports Project 2 for
aligned returns and metrics and adds only the optimization layer.

- Mean-variance (Markowitz): minimum-variance and maximum-Sharpe (tangency) portfolios and the efficient frontier
- Closed-form solutions (shorting) + scipy SLSQP (long-only); ex-ante figures cross-checked against Project 2's ex-post metrics
- Python library **and** Typer CLI; 35 passing `pytest` tests, run in CI on Linux + Windows × Python 3.10 & 3.12

## Tech stack

Python 3.10+ · pandas · numpy · scipy · pyarrow · Typer · loguru · pytest · ruff · black

## License

MIT
