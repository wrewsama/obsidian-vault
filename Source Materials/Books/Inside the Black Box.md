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

## Risk Models
- purpose: measure and thus limiting the exposure to risk
- limiting
    - by constraint: cannot cross the limit
    - by penalty: can only cross the limit if expected returns exceed a threshold
- measuring
    - volatility (standard deviation of returns of an instrument over time)
    - dispersion (standard deviation between different instruments)

## Transaction Cost Models
- purpose: measure the extra overhead costs when making trades to ensure that it doesn't outweigh the expected increase in gains (alpha model) or the expected decrease in loss (risk)
- sources
    - commissions / fees
    - slippage (price changes between decision and execution)
    - how the trade itself affects the price
## Portfolio Construction Models
- purpose: process the inputs from the alpha, risk, and transaction cost models, then come up with the best possible portfolio (given some set constraints) 
- rule based models
    - equal position weighting: either own the position or don't, each position has the same size
    - equal risk weighting: size is inversely proportional to the risk
    - alpha driven weighting: size is proportional to the attractiveness of the position (based on the alpha model)
- portfolio optimisers
    - purpose: optimise some objective function
    - inputs: expected return, volatility, correlation (between instruments)
## Execution Models
- purpose: take in the required position changes from the portfolio construction model, then execute those changes as completely and as cheaply as possible
- model chooses
    - type of order (e.g. market, limit, fill-or-kill, etc)
    - how to send the order (i.e. how much to send at once, aka _iceberging_)
    - where to send the order (which liquidity pool)

## Data
- price data: info from exchanges, e.g.
    - all order book information
    - things that can be derived from indices
- fundamental data: everything else, e.g.
    - financial health / performance indicators
    - sentiment
- common data sources
    - exchanges
    - regulators
    - governments
    - corporations
    - news agencies
    - proprietary data vendors

## Research
- testing types
    - training / in-sample testing
    - out of sample testing
- model evaluation
    - graph of cumulative profits
    - average rate of return
    - variability of returns
    - max drawdown
    - predictive power (measured using $R^2$)
    - % winning trades / time periods
    - risk/return ratios
    - relationship with other strategies (ensure it's still useful together with other models)
    - time decay: how much does the strategy depend on getting information on time
    - sensitivity to specific parameters: if small changes to some parameters result in massive changes in performance, the parameters should be reconsidered
---
Source: https://www.goodreads.com/book/show/6800644-inside-the-black-box
