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
| 4 | [**Risk Analytics Engine**](https://github.com/HoGSwain/risk-analytics-engine) | Value-at-Risk (historical & parametric), Expected Shortfall (CVaR), and stress losses for assets and portfolios — built on Projects 1–2. | ✅ **Live** · [![CI](https://github.com/HoGSwain/risk-analytics-engine/actions/workflows/ci.yml/badge.svg)](https://github.com/HoGSwain/risk-analytics-engine/actions/workflows/ci.yml) |
| 5 | [**Bond Analytics Engine**](https://github.com/HoGSwain/bond-analytics-engine) | Bond pricing (price↔yield), duration, convexity, DV01, and a spot yield curve. **Standalone** — the fixed-income primitive. | ✅ **Live** · [![CI](https://github.com/HoGSwain/bond-analytics-engine/actions/workflows/ci.yml/badge.svg)](https://github.com/HoGSwain/bond-analytics-engine/actions/workflows/ci.yml) |
| 6 | [**Option Pricing Engine**](https://github.com/HoGSwain/option-pricing-engine) | Black–Scholes, binomial, and Monte-Carlo option pricing with the full Greeks, implied volatility, and deterministic explainability — composed with Projects 1–2 (spot from `fmde`, realized vol from `pae`). | ✅ **Live** · [![CI](https://github.com/HoGSwain/option-pricing-engine/actions/workflows/ci.yml/badge.svg)](https://github.com/HoGSwain/option-pricing-engine/actions/workflows/ci.yml) |
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
- Python library **and** Typer CLI; 38 passing `pytest` tests, run in CI on Linux + Windows × Python 3.10 & 3.12

## Project 4: Risk Analytics Engine

Built on Projects 1–2, the **Risk Analytics Engine (`rae`)** measures downside
risk. It imports Project 2 for aligned returns and adds only the
risk-measurement layer.

- Value-at-Risk (historical & parametric Gaussian), Expected Shortfall (CVaR), and stress (worst 1-day / k-day loss); per-asset and portfolio, with dollar figures
- Deterministic decision-level explanation ("at 95% confidence the one-day loss is not expected to exceed X% ($Y)") with explicit limitations
- Python library **and** Typer CLI; 29 passing `pytest` tests, run in CI on Linux + Windows × Python 3.10 & 3.12

## Project 5: Bond Analytics Engine

The portfolio's **fixed-income** primitive. Unlike Projects 2–4, a bond's price
and risk are analytic in its cash flows (not equity returns), so the **Bond
Analytics Engine (`bae`)** is deliberately **standalone** — no AIFEL git
dependency. Knowing when *not* to couple is itself an engineering decision.

- Pricing (price↔yield via Brent), Macaulay & modified duration, convexity, DV01, and a duration+convexity yield-shock scenario vs full reprice
- A synthetic spot yield curve; every measure validated against a finite-difference oracle (independent of the analytic formulas)
- Python library **and** Typer CLI; 36 passing `pytest` tests, run in CI on Linux + Windows × Python 3.10 & 3.12

## Project 6: Option Pricing Engine

The portfolio's **derivatives** primitive. An option is written *on an equity*, so
unlike the standalone bond engine the **Option Pricing Engine (`ope`)** genuinely
**composes** with Projects 1–2: it sources the spot from `fmde` and a realized-
volatility estimate by reusing `pae`'s metric, while the pure option math still
runs offline from explicit inputs.

- Three independent pricers — Black–Scholes (closed form), a Cox-Ross-Rubinstein binomial tree (European + American), and seeded Monte-Carlo with a standard error — cross-checked against each other
- The full Greeks (delta, gamma, vega, theta, rho) validated against a finite-difference oracle, plus implied volatility via Brent; deterministic explanation with the realized≠implied-vol caveat
- Python library **and** Typer CLI; 41 passing `pytest` tests, run in CI on Linux + Windows × Python 3.10 & 3.12

## Explainability & Governance

Every project answers a **sixth mandatory question** beyond the usual five —
*"Can you explain every financial conclusion the system reaches?"* —
and treats explainability (*understanding*) and governance (*accountability*) as
first-class, **per-project** deliverables. This index is the thin rollup, not a
second copy.

Each live project provides:

- A deterministic **`explain()`** that turns its numeric outputs into
  plain-language findings **and** an explicit limitations list, appended to every
  report and generated from the actual numbers — **never** by a language model
  (an LLM narrating a model's output would itself be an unauditable black box).
- A **`docs/explainability.md`** with the checklist below.
- **Pinned** cross-project git dependencies (commit SHAs) — reproducible *in
  fact*, not just in principle.

### Checklist template (each project's `docs/explainability.md`)

**Explainability** — problem defined · concepts in plain language · assumptions
documented · parameters justified · outputs interpreted for a human · limitations
disclosed · alternatives discussed.

**Governance** — data provenance recorded · versions tagged · dependencies
declared **and pinned** · tests pass in CI · validated against known values ·
reproduction steps complete · fairness/bias assessment (mandatory once a model
affects people; **N/A** for the current financial-engineering projects).

## Tech stack

Python 3.10+ · pandas · numpy · scipy · pyarrow · Typer · loguru · pytest · ruff · black

## License

MIT
