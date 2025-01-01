## misc
market risk
credit risk
yield curve
## Chapter 1
money market account: amount you get at time t, with interest rate r, for $1 invested. Equal to $e^{rt}$

Zero Coupon Bond (ZCB)
- asset that pays 1 at time T
- `Z(t, T)` = price of ZCB with maturity T at time t, aka discount factor / present value
- `Z(T, T) = 1` 
- $Z(t, T) = e^{-r(T - t)}$

Annuities:
- series of fixed cashflows at specified times T0, T1, ... Ti
- value V at time t = C * sum of the discount factors from t to Ti

Other definitions
- stock
	- asset giving ownership in a fraction of a company
	- may pay a dividend
	- spot price: known current price
- fixed rate bond
	- has a coupon `c` and a notional `N`
	- pays `cN` after some interval and `N` at maturity
- security: traded, negotiable instruments with financial value (e.g. stocks and bonds)
- asset: securities and instruments that may not be readily tradable (e.g. real estate)
- counterparties: entities that enter into a derivative contract

## Chapter 2
derivative: financial contract between 2 counterparties whose value is a function of the value of another variable (e.g. a stock or interest rates)

Over-the-counter (OTC) derivatives: between 2 counterparties directly
exchange-traded derivatives: exchange matches buyers and sellers

Forward Contract / Forward: agreement to trade a specific asset at a certain time `T`(maturity) and a certain price `K` (delivery price)

$V_K(t, T)$: value of being long a forward contract
Forward price $F(t, T)$: delivery price $K$ such that $V_K(t, T) = 0$

Replication proof: take 2 portfolios and show they always have the same value at time $T$ => must also have the same value at time $t$

no-arbitrage: prove by contradiction by start with an empty portfolio, do some stuff, and end up with a portfolio of positive value at time $T$

Forward on asset paying no income:
$F(t, T) = S_te^{r(T-t)}$

Forward on asset paying known income $I$:
$F(t, T) = (S_t - I)e^{r(T-t)}$

Value of forward contract satisfies:
$V_K(t, T) = (F(t, T) - K)e^{-r(T-t)}$

Forward on stock paying dividends (dividend yield = $q$):
$F(t, T) = S_te^{(r-q)(T-t)}$

physical settlement: at time $T$, actually pay $K$ and receive the asset
cash settlement: at time $T$, receive $S_T - K$

## Chapter 3
Price of forward contract with maturity $T_1$ on ZCB with maturity $T_2$:
$F(t,T_1,T_2) = \frac{Z(t,T_2)}{Z(t,T_1)}$

**Forward rate** (at time $t$ for period $T_1$ to $T_2$), aka $f_{12}$: rate agreed at $t$ at which one can borrow/lend money from $T_1$ to $T_2$
$F(t,T_1,T_2) = \frac{1}{(1 + f_{12})^{T_2 - T_1}}$

libor: rate at which banks borrow or lend to each other, denoted by $L_t[t,t+\alpha]$ (libor rate for period $t$ to $t+\alpha$)

Forward rate agreement(FRA): forward contract to exchange 2 cashflows.
With maturity $T$ and fixed rate $K$, at time $T + \alpha$, buyer agrees to:
- pay $\alpha K$
- receive $\alpha L_t[T,T+\alpha]$

Forward libor rate: value of K such that the FRA has 0 value at time $t$
$L_t[T,T+\alpha] = \frac{Z(t,T) - Z(t,T+\alpha)}{\alpha Z(t, T + \alpha)}$

## Chapter 4
swap: agreement to exchange cashflows at agreed dates. Has a floating and a fixed leg.

vanilla swap
- $T_{i+1} = T_i + \alpha$, intuitively, each time interval is $\alpha$
- pv01 $KP_t[T_0,T_n]$ = present value of receiving 1 * $\alpha$ at each payment date = $\sum_{i=1}^{n}\alpha Z(t,T_i)$
- floating leg
	- at time $T_i + \alpha$, pay $\alpha L_{T_i}[T_i, T_i + \alpha]$
	- intuitively, we pay the libor rate for the time period
	- value = $V^{FL} = \sum_{i=1}^{n}L_T[T_{i-1}, T_i]\alpha Z(t,T_i) = \sum_{i=1}^{n}(Z(t,T_{i-1}) - Z(t, T_i)) = Z(t, T_0) - Z(t, T_n)$ 
- fixed leg
	- at time $T_i + \alpha$, pay $\alpha K$
	- paying a fixed rate $K$
	- value = $V^{FXD}_{K} = K \sum_{i=1}^{n}\alpha Z(t,T_i) = KP_t[T_0,T_n]$
 - forward swap rate
	 - value $y_t[T_0, T_n]$ of the fixed rate K such that the value of the swap at t is 0
	 - $y_t[T_0, T_n] = \frac{Z(t, T_0) - Z(t, T_n)}{KP_t[T_0,T_n]}$
- value of the swap: $V^{SW}_K(t) = (y_t[T_0, T_n] - K)P_t[T_0,T_n]$

## Chapter 5
Futures contract: like a forward contract except that the holder receives changes in the futures price over the entire lifetime of the contract, not just at maturity
- futures price: $\Phi(t,T)$
- by defn, $\Phi(T,T) = S_T$  
- mark-to-market change(variation margin) = $\Phi(i,T) - \Phi(i-1,T)$
- total payout = $S_T - \Phi(t,T)$ 

futures vs forward prices
- if interest rates are constant, $\Phi(t,T) = F(t, T)$
- if interest rates are not constant, $\Phi(t,T) - F(t, T)$ is proportional to the covariance between $S_T$ and the money market account $M_T$

eurodollar futures contract:
defined by price at maturity $\Phi(T,T) = 100(1-L_T[T,T+\alpha])$

## Chapter 6
$V^A(t)$: value of portfolio $A$ at time $t$
$V^A(T,\omega_i)$: value of portfolio $A$ at future time $T$ for sample outcome $\omega_i$

arbitrage portfolio 
- $V^A(t) \leq 0$
- $V^A(T, \omega_i) \geq 0$ for all states of the world $\omega_i$
- $V^A(T) > 0$ with nonzero probability

monotonicity theorem
- if $V^A(T,\omega_i) \geq V^B(T, \omega_i)$ for all $i$, then $V^A(t) \geq V^B(t)$
- if, in addition, $V^A(T,\omega_i) > V^B(T, \omega_i)$ for some $j$ with $P{w_j} > 0$, then $V^A(t) > V^B(t)$

corollary: if $V^A(T,\omega_i) = V^B(T, \omega_i)$ for all $i$, then $V^A(t) = V^B(t)$

## Chapter 7

Call option with strike/exercise price $K$ and maturity/exercise date $T$: the right to buy the asset for $K$ at time $T$
Put option with strike/exercise price $K$ and maturity/exercise date $T$: the right to sell the asset for $K$ at time $T$
straddle: call and put of the same strike and maturity, has payout = $|S_T - K|$

types:
- European: can only exercise at $T$
- American: can exercise at any time $t \leq T$
- Bermudan: can exercise at a finite set of times $T_0, ... T_n \leq T$

for a call option at time $t$:
- at the money: $S_t = K$
- in the money: $S_t > K$
- out of the money: $S_t < K$
- at the money forward (ATMF): $F(t,T) = K$
- in the money forward (ATMF): $F(t,T) > K$
- out of the money forward (ATMF): $F(t,T) < K$

put call parity:
- price of European call = $C_K(t,T)$
- price of European put = $P_K(t,T)$
- $P_K(t,T) \geq 0$ and $C_K(t,T) \geq 0$
- $C_K(t,T) - P_K(t,T) = V_K(t,T)$ (recall that $V_K(t,T)$ is the value of a forward contract with delivery price $K$)

European call price on non dividend paying stock:
$\max\{0, S_t-KZ(t, T)\} \leq C_K(t,T) \leq S_t$

American vs European calls
- equal price for non dividend paying stocks
- American worth more when there are dividends

call spread: 
- portfolio with:
	- long call option with strike $K_1$ and maturity $T$
	- short call option with strike $K_2$ and maturity $T$
- value: $C_{K_1}(t,T) - C_{K_2}(t,T)$
- if $K_1 < K_2$ then 
	- $C_{K_1}(t,T) \geq C_{K_2}(t,T)$ and $P_{K_1}(t,T) \leq P_{K_2}(t,T)$
	- $(C_{K_1}(t,T) - C_{K_2}(t,T)) + (P_{K_2}(t,T) - P_{K_1}(t,T)) = Z(t,T)(K_2-K_1)$
	
call butterfly:
- portfolio with $\lambda$ calls with strike $K_1$, $(1-\lambda)$ calls with strike $K_2$ and $-1$ calls with strike $K^*$
- $C_{K^*}(t,T) \leq \lambda C_{K_1}(t,T) + (1-\lambda)C_{K_2}(t,T)$

digital options
- call: pays 1 if $S_T \geq K$ and 0 otherwise
- put: pays 1 if $S_T \leq K$ and 0 otherwise

## Chapter 8
To find a replicating portfolio for a derivative $D(\omega_A)$ in state A and $D(\omega_B)$ in state B, solve the linear equations
- $\lambda S_1(\omega_A) + \mu = D(\omega_A)$
- $\lambda S_1(\omega_B) + \mu = D(\omega_B)$
by solving it, you get a portfolio with $\lambda$ stocks and $\mu$ ZCBs (long if positive, short if negative)
can then use replication argument to find the present, **_arbitrage-free_** value of the derivative

risk-neutral probability $p^*$:
- given state A has probability $p$ and state B has probability $(1-p)$, $p^*$ is the unique value of $p$ for which the present value of the expected value of the option payout (i.e. $Z(0,1)E(S_1-K)^+$) is equal to the present arbitrage-free value of the derivative
- using $p^*$, the expected stock price is equal to its forward price, i.e. $E_*(S_1) = S_0(1+r)$

multiple time steps:
- use the replicating portfolio and backpropagate from the leaves
- $E_*(S_n) = S_0(1+r)^n$

general no-arbitrage condition: given a binomial tree where at each node, the stock can move from $S_{n}$ to $S_n(1+u)$ or $S_n(1+d)$ where $d < u$ arbitrage free iff $d < r < u$ 

## Chapter 9
Martingale: 
- series of random variables $X_0, X_1, ... X_n$ such that $\forall i$, $E(|X_i|) < \infty$ and $E(X_{n+1} | X_0, ... X_n) = X_n$
- probabilistic definition of a "fair game", where expected value tomorrow = today's value

Markov: when the series satisfies 
$P(X_{n+1} | X_0, ... X_n) = P(X_{n+1} | X_n)$ (i.e. $X_n$ captures all the info about the conditional distribution of $X_{n+1}$)
this makes the martingale condition become $E(X_{n+1} | X_n) = X_n$

martingale with respect to $Y_0, ... Y_n$:
when $E(|X_i|) < \infty$ and $E(X_{n+1} | Y_n) = Y_n$

numeraire: unit by which we discount / rebase prices

fundamental theorem of asset pricing: there are no arbitrage portfolios iff
- there exists risk-neutral probability distribution $P^*$ such that the ratios of asset prices to the money market account are martingales under $P^*$
- (general form) for given positive asset, there exists probability $Q*$, defined over the same set of possible outcomes, such that the ratios of asset prices to the numeraire $N_t$ are martingales under $Q^*$
	- $\frac{D(t,T)}{N_t} = E_*(\frac{D(T,T)}{N_T} | S_t)$
	- using ZCB as the numeraire (since $Z(T,T)$ is a constant 1), $D(t,T) = Z(t,T)E_*(D(T,T) | S_t)$

given derivative with payout $D(T,T) = g(S_t)$ and risk-neutral density $f(S_T)$,
$D(t,T) = Z(t,T) \int g(x)f(x) dx$

## Chapter 10
logarithmic return $\lambda_n = log(\frac{S_n}{S_{n-1}})$ 
multiplicative return $I_n = \frac{S_n - S_{n-1}}{S_{n-1}}$ 

**Black-Scholes formula**
$C_K(t,T) = S_t\Phi(d_1) - KZ(t,T)\Phi(d_2)$
where
$d_1 = \frac{log(\frac{S_t}{K}) + (r + 0.5\sigma^2)(T-t)}{\sigma \sqrt{T-t}}$
$d_2 = d_1 - \sigma \sqrt{T-t}$
$\sigma$ is the volatility

alternate form:
$C_K(t,T) = Z(t,T)(F(t,T)\Phi(d_1) - K\Phi(d_2))$

properties:
- as $S_t \to \infty$, $C_K(t,T) \to S_t - Ke^{-r(T-t)}$
- as $\sigma \to 0$, 
	- $C_K(t,T) \to S_t - Ke^{-r(T-t)}$ if $S_t > Ke^{-r(T-t)}$
	- $0$ otherwise
- as $\sigma \to \infty$, $\Phi(d_1) \to 1$, and $\Phi(d_2) \to 0$: $C_K(t,T) \to S_t$

delta: how much the option price changes for a change in stock price
$\frac{\delta C_K(t,T,S_t)}{\delta S_t} = \Phi(d_1)$

vega: how much the option price changes for a change in volatility $\sigma$
$\frac{\delta C_K(t,T,S_t)}{\delta \sigma} = S_t\sqrt{T-t}\phi(d_1)$

## Chapter 11
Price of digital call option struck at $K$:
$\lim_{\lambda\to\infty}\lambda(C_K(t,T) - C_{K+\frac{1}{\lambda}}(t,T)) = -\frac{\delta C_K(t,T)}{\delta K}$
$-\frac{\delta C_K(t,T)}{\delta K} = Z(t,T)\Phi(d_2)$
