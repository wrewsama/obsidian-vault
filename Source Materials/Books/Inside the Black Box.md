Tags:
- [[Quant Finance]]
---
## Introduction to Quant Trading
- quants research and build strategies to generate alpha
- there is a spectrum between fully discretionary and fully systematic strategies
- typical trading system structure
    - modules: alpha, risk, and transaction cost models
    - the 3 modules feed into a portfolio construction model
    - the portfolio construction model interacts with the execution model

## Alpha Models
- theory-driven: hypothesise why something is the way it is
    - price-related
        - trend following
        - mean reversion
        - technical sentiment (e.g. from put/call volume, order book shape)
    - fundamental data related
        - yield
        - growth
        - quality (e.g. leverage, diversity of revenue sources, management quality, fraud risk, sentiment)
- data-driven: look for patterns in the data that can't be intuitively explained
- strategy configuration
    - forecast target: what to predict
    - time horizon: when to predict it (e.g. 1 day ahead, 1 year ahead)
    - bet structure: absolute or relative
    - investment universe: which instruments to care about
    - model definition: what parameters does it take in
    - conditioning variables: extra variables / rules that affect the forecast (e.g. stop-loss)
    - run frequency
- alpha model forecasts can be weighted and combined to produce a better forecast
---
Source: https://www.goodreads.com/book/show/6800644-inside-the-black-box
