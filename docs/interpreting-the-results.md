# Interpreting the Results — in Plain English

*What the AIFEL toolkit's live-data output actually means, for someone with no finance or technical background — and what you'd responsibly do with it.*

> ⚠️ **Read this first.** These tools **describe and analyze** — they never tell you what to buy or sell. Nothing here is financial advice; it's an educational engineering project. Real money decisions belong with a licensed advisor who knows your situation.

**Data:** live Yahoo Finance prices · **Window:** Jan 2023 – Jul 2026 (~3.5 years) · **Looked at:** Apple, Microsoft, Google, and an S&P 500 fund (SPY).

> 🧪 **Live data vs. synthetic data — read this before trusting any number.** The AIFEL engines ship with two data sources. Their **default is *synthetic*** — a seeded, reproducible simulation used for offline demos, tests, and continuous integration, so a run always gives the same output. Those synthetic numbers demonstrate the **machinery**; they carry **no market meaning** and must never be read as investment signal. **Everything in *this* guide comes from the *live* source instead** — real Yahoo Finance prices (`--source yfinance`) — which is what makes it safe to interpret. Anywhere else in the portfolio (sample files, notebook charts, CI reports), assume results are **synthetic** unless a data-source line explicitly says "live".

> There is also an interactive, visual version of this guide. *(Ask the portfolio owner for the shared link.)*

---

## The one-minute version

Over the last three-and-a-half years, the broad **S&P 500 fund (SPY)** gave the **smoothest ride for its return** — fewer stomach-drops than any single company. **Google** grew the most but swung the wildest. **Microsoft** quietly lagged. And spreading money across all four was **less risky than betting on any one of them**.

The catch, which the rest of this page keeps repeating: this was a **mostly-good stretch** for big tech. These numbers explain what *happened*. They do not predict what happens next.

---

## The four investments, side by side

The toolkit measured each one four ways. Plain-English headers first; the technical name is in *italics* underneath.

| Investment | Growth *(return, per year)* | Bumpiness *(volatility)* | Reward for risk *(Sharpe)* | Worst fall *(max drawdown)* |
|---|--:|--:|--:|--:|
| **SPY** — the whole-market fund | 21.7% | **15.2%** 🟢 | 1.30 | **−18.8%** 🟢 |
| **Google** (GOOG) | **44.5%** 🟢 | 30.2% 🟡 | **1.41** 🟢 | −29.4% |
| **Apple** (AAPL) | 29.8% | 25.8% | 1.08 | −33.4% 🔴 |
| **Microsoft** (MSFT) | 18.0% 🔴 | 25.1% | 0.64 🔴 | −34.5% 🔴 |

🟢 calmest / strongest here · 🟡 bumpiest · 🔴 weakest / deepest fall

**How to read a row.** Take Google: it grew about **45% a year**, but with the biggest swings of the four (that's the "bumpiness"), and at its worst point it was down ~29% from its high. Its "reward for risk" of 1.41 was still the best — you were paid well for that bumpy ride. Microsoft is the cautionary tale: similar bumpiness to the rest, but the **lowest reward for it**, and the deepest fall.

### What each measure means

| Term | Plain name | What it tells you |
|---|---|---|
| **Sharpe ratio** | Reward for risk | How much return you got **for each unit of nerve-wracking** you endured. Above **1.0** is generally considered good; higher is better. Fairer than raw growth, because it counts the stress. |
| **Volatility** | Bumpiness | How much the price **jumps around** day to day. The fund (15%) was half as jumpy as the single stocks (~25–30%). More bumpiness = more sleepless nights and a wider range of outcomes. |
| **Max drawdown** | Worst fall | The **biggest drop from a high to a low** during the window — the "could you have stomached this without panic-selling?" number. Every single stock here fell **30%+**; the fund fell ~19%. |
| **Alpha / Beta** | Beating the market | **Beta** ≈ how much it moves when the whole market moves (all four sat near 1). **Alpha** = whether it beat what its risk level "deserved" — Google was strongly positive (+19%), Microsoft negative (−3.5%). |

---

## On a rough day, how much could you lose?

This is the question the risk tool answers — and the one that actually keeps people up at night. For the four held together as one pot of money:

> **In plain terms:** on **roughly 19 days out of 20**, a daily loss should stay under about **1.8%** of the pot. Roughly **1 day in 20** it could be worse — and when it is one of those bad days, the average loss was about **2.5%**. The single worst day in the whole window was about **−5%**.

**The quiet lesson.** That 1.8% bad-day figure for the *combined* four is **lower than any single stock on its own** (Apple, Microsoft and Google each land around 2.4–2.6%). That's diversification, made concrete: spreading your money out genuinely softened the bad days, without giving up much. Boring is a feature.

---

## The optimizer's answer — and its biggest trap

One tool searches for the "best" mixes of the four. It found three worth knowing:

| Mix | Plain name | What it chose |
|---|---|---|
| **Min-variance** | The calmest mix | Almost entirely the **market fund (SPY, ~99%)** — it was the least bumpy thing available. |
| **Max-Sharpe** | The best-scoring mix | Heavy on **Google and the fund**, some Apple, and **zero Microsoft**. Best reward-for-risk score (1.54). |
| **Equal-weight** | The simple split | A plain **25% in each**. Scored 1.39 — remarkably close to the "optimal" mix, for a fraction of the cleverness. |

> 🚫 **The trap — read this twice.** The optimizer is **grading the past, not forecasting the future**. It "loves" Google and shuns Microsoft only because Google already won and Microsoft already lagged *in this window*. That is hindsight dressed up as a recommendation. Run the same tool over a different stretch — say the 2022 downturn — and it would crown different winners. **"Optimal" here means "optimal looking backward."** Never read it as "buy Google."

---

## Beyond stocks: loans, bets, and "will I get paid back?"

| Tool | Plain name | What it showed |
|---|---|---|
| **Bond** (duration 8.1) | A loan that pays interest | If interest rates rise about **1%**, this 10-year bond's price falls roughly **8%**. Bonds move mainly on interest rates — often the opposite way to the excitement in stocks. That's why people hold both. |
| **Option** (3 methods agree) | A bet on a future price | A contract betting on where a stock goes by a future date. Three different pricing methods landed on the same value (~$29.55) — a sign the price is trustworthy. Options are **advanced and can lose everything**; shown for completeness. |
| **Credit** (default ≈ 0%) | Will they pay their debts? | For Apple: essentially **0%** — one of the safest borrowers on earth. This is the lens a lender uses before handing over money, and why safe borrowers pay lower interest. |

---

## What you could actually take from this

Not instructions — **questions these results help you think through**, and the principles behind them.

1. **Match the risk to your stomach and your timeline.** Every single stock here fell 30%+ at some point. If a temporary 30% drop would make you panic-sell, that's your answer about how much belongs in single stocks versus a steadier fund — regardless of how good the returns look.
2. **Don't chase the biggest number.** Raw growth ignores the stress you paid for it. "Reward for risk" is the fairer lens — and even that flips with the calendar. The highest-returning thing in a good year is often the hardest to hold in a bad one.
3. **Spreading out is close to a free lunch.** The combined pot had smaller bad days than any single stock in it, with barely any cost to returns. Diversification is the one thing in investing that reliably lowers risk without a matching sacrifice.
4. **Know your bad-day number in advance.** Deciding how a ~2% (or −5%) down day would feel *before* you invest is what stops fear-driven decisions later.
5. **Different money, different tools.** Stocks, bonds, and options behave differently and answer different needs — growth, steady income, hedging. A whole plan usually blends them.

---

## Five reasons to hold these results loosely

- **It's one short, flattering window.** Early 2023 to mid-2026 was mostly kind to big tech. The exact same tools over 2022 would paint a far grimmer — and equally "real" — picture.
- **The past doesn't forecast the future.** Every number here is history. The optimizer especially *describes* what already happened; it cannot see tomorrow.
- **Four names isn't a portfolio.** Three of these are giant tech companies that tend to rise and fall together. A genuinely diversified plan spans many more industries, sizes, and countries.
- **Real-world frictions are missing.** Nothing here accounts for trading fees, taxes, inflation eating returns, or your personal income, debts, and goals.
- **This is an educational tool, not advice.** It was built to analyze and explain transparently — not to recommend. For decisions with real money, talk to a licensed financial professional.

---

*Every figure above came from running the seven-project [AIFEL toolkit](../README.md) on live Yahoo Finance data. Each tool ships a plain-language explanation of its own output — which is what made this translation possible. **Educational project · not financial advice.***
