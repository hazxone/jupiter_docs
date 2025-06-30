# Perps & JLP

## 1. Jupiter Liquidity Provider (JLP)

### 1.1. JLP Economics

#### The JLP Token

The [JLP token](https://jup.ag/tokens/27G8MtK7VtTcCHkpASjSDdkWWYfoqT6ggEuKidVJidD4) is the token minted to users who provide liquidity to the JLP pool. JLP can be acquired either through [Jupiter Swap](https://jup.ag/swap/JLP-USDC) or through the [Earn page](https://jup.ag/perps-earn), which mints or burns JLP tokens directly from the pool.

More information on [how to get JLP](https://support.jup.ag/hc/en-us/articles/18735258178204-How-To-Get-JLP) here.

#### Custodies

The JLP pool manages several custodies (tokens) for liquidity providers:

*   SOL: [7xS2gz2bTp3fwCC7knJvUWTEU9Tycczu6VhJYKgi1wdz](https://solscan.io/account/7xS2gz2bTp3fwCC7knJvUWTEU9Tycczu6VhJYKgi1wdz)
*   ETH: [AQCGyheWPLeo6Qp9WpYS9m3Qj479t7R636N9ey1rEjEn](https://solscan.io/account/AQCGyheWPLeo6Qp9WpYS9m3Qj479t7R636N9ey1rEjEn)
*   BTC: [5Pv3gM9JrFFH883SWAhvJC9RPYmo8UNxuFtv5bMMALkm](https://solscan.io/account/5Pv3gM9JrFFH883SWAhvJC9RPYmo8UNxuFtv5bMMALkm)
*   USDC: [G18jKKXQwBbrHeiK3C9MRXhkHsLHf7XgCSisykV46EZa](https://solscan.io/account/G18jKKXQwBbrHeiK3C9MRXhkHsLHf7XgCSisykV46EZa)
*   USDT: [4vkNeXiYEUizLdrpdPS1eC2mccyM4NUPRtERrk6ZETkk](https://solscan.io/account/4vkNeXiYEUizLdrpdPS1eC2mccyM4NUPRtERrk6ZETkk)

#### Assets Under Management

The AUM for each custody account in the pool is calculated as follows:

##### Stablecoins (USDC, USDT)

```
aum = owned * current_price
```

##### Non-stablecoins (SOL, ETH, wBTC)

1.  **Calculate the global short position profit/loss (Unrealized PnL in USD):**

    ```
    unrealized_pnl = (global_short_sizes * (|global_short_average_prices - current_price|)) / global_short_average_prices)
    ```

2.  **Net Asset Value (NAV):**

    ```
    available_tokens = owned - locked  

    nav = available_tokens * current_token_price  

    guaranteed_usd = size of all trades (USD) - collateral of all trades (USD)  

    nav += guaranteed_usd
    ```

    The *`guaranteed_usd`* value in each custody account represents an estimate of the total size of all long positions. It is only an estimate as *`guaranteed_usd`* is **only updated when positions are updated** (i.e. opening / closing positions, updating collateral). It does not update in real-time when asset prices change.

    *`guaranteed_usd`* is used to calculate the pool's AUM as well as the overall PnL for all long positions efficiently.

3.  **Assets Under Management (AUM)**

    If traders are losing on short positions, the losses are added to the pool's AUM:

    ```
    aum = nav + unrealized_pnl
    ```

    Otherwise, traders' profits are deducted from the pool's AUM:

    ```
    aum = nav - unrealized_pnl
    ```

    The Total AUM is then calculated as the sum of all the custodies' AUM:

    ```
    total_aum = Σ(aum)
    ```

    **NOTE:** The pool sizes displayed on the [Earn page](https://jup.ag/perps-earn) do not add up to the pool's AUM. This discrepancy occurs because the Earn page shows each custody's *`owned`* value in USD without accounting for the traders' unrealized PnL. This simplified representation on the Earn page provides a quick overview of the pool's holdings but doesn't reflect the JLP pool's true AUM.

#### Virtual Price, Market Price and AUM Limit

```
Virtual Price = Sum of all JLP Assets (in USD) / Total Quantity of JLP in circulation
```

When the AUM limit is hit:

```
Market Price = Virtual Price + Market-assigned Premium
```

Usually, users can mint new JLP or redeem (burn) them at the Virtual Price. However, when the AUM limit is hit, new minting of JLP is disabled to cap the amount of TVL in the pool.

When this happens, the demand for JLP on the market usually leads to a premium for JLP compared to the virtual price.

You may sell your JLP for the Market Price at any time. If the Market Price is below the Virtual Price, your JLP tokens are redeemed (burned) at the virtual price instead of the market price.

You can view the current TVL and AUM Limit on the [JLP Earn page](https://jup.ag/perps-earn).

Every 7 days, the estimated APY figure is updated with the above calculation by using the previous week's fees.

#### Fetching AUM and Calculating JLP Virtual Price

The code snippets below show how to fetch the AUM from the JLP pool onchain account, as well as calculating the JLP virtual price in real time:

*   [Fetch pool AUM](https://github.com/julianfssen/jupiter-perps-anchor-idl-parsing/blob/main/src/examples/get-pool-aum.ts)
*   [Calculate JLP virtual price](https://github.com/julianfssen/jupiter-perps-anchor-idl-parsing/blob/main/src/examples/get-jlp-virtual-price.ts)

##### Calculate global unrealized PnL for longs

The most accurate way to calculate the unrealized PnL for all open long positions is to loop through all open positions (by fetching them from onchain accounts) and use the unrealized PnL calculation shown in [calculating unrealized PnL](https://support.jup.ag/hc/en-us/articles/18734778781468-How-is-PnL-on-Perps-calculated).

To get an estimate of the global unrealized PnL for longs, you can use the following calculation:

```
// 1) Get the custody account you're interested in calculating unrealized PnL for longs  
// https://station.jup.ag/guides/perpetual-exchange/onchain-accounts#custody-account  

// 2) Get the `assets.guaranteedUsd` field which stores the value of `position.sizeUsd - position.collateralUsd` for  
// all open long positions for the custody. Note that a position's `sizeUsd` value is only updated when a trade is made, which  
// is the same for `guaranteedUsd` as well. It does *not* update in real-time when the custody's price changes  

guaranteedUsd = custody.assets.guaranteedUsd  

// 3) Multiply `custody.assets.locked` by the custody's current price to get the USD value of the tokens locked   
// by the pool to pay off traders' profits  

lockedTokensUsd = custody.assets.locked * currentTokenPriceUsd  

// 4) Subtract `guaranteedUsd` from `lockedTokensUsd` to get the estimate of unrealized PnL for all open long positions. Note that  
// the final value is greater than the actual unrealized PNL as it includes traders' collateral  

globalUnrealizedLongPnl = lockedTokensUsd - guaranteedUsd
```

##### Calculate global unrealized PnL for shorts

The custody accounts store a *`global_short_sizes`* value that stores the USD value of all open short positions in the platform. The *`global_short_average_prices`* value stores the average price (USD) for all open short positions and is used together with *`global_short_sizes`* to get an estimate of the global unrealized PNL for shorts, as shown below:

```
globalUnrealizedShortPnl = (custody.assets.globalShortSizes * (|custody.assets.globalShortAveragePrices - currentTokenPriceUsd|)) / custody.assets.globalShortAveragePrices)
```

The most accurate way is again to loop through all open positions and sum the unrealized PNL, as the calculation above also includes traders' collateral.

#### Yield

The JLP token adopts a growth-focused approach, similar to accumulating ETFs like VWRA or ARKK. Rather than distributing yield through airdrops or additional token mints, the JLP token's value is designed to appreciate over time. This appreciation is driven by the growth of the JLP pool's AUM, which is used to derive the virtual price as shown above.

75% of all fees generated from Jupiter Perpetuals trading, token swaps, and JLP minting/burning are reinvested directly into the JLP pool.

This mechanism continuously enhances the pool's liquidity and value for token holders. The remaining 25% is allocated to Jupiter as protocol fees, supporting ongoing development and maintenance.

For example, if trading activities generate 10 SOL in fees, 7.5 SOL would be added to the JLP pool, increasing its SOL holdings and AUM.

Fees generated by the pool are reinvested directly back into the JLP pool, mirroring how accumulating ETFs reinvest dividends. This reinvestment strategy compounds the pool's growth, steadily increasing the JLP token's price and intrinsic value.

#### Exposure

The intrinsic value of the JLP token is linked to the price movements of the liquidity pool's underlying tokens (SOL, ETH, BTC, USDC, and USDT).

As a JLP holder, your portfolio is exposed to market movements, particularly to the performance of the non-stablecoin tokens: SOL, ETH, and BTC. If these tokens decrease in price, the value of your JLP position will likely decrease as well.

That said, JLP holders earn yield from fees generated by trading activities. When traders incur losses, these are reinvested into the JLP pool, benefiting JLP holders. Conversely, when traders profit, it comes at the expense of the JLP pool.

The JLP usually outperforms its underlying assets during sideways or bearish market conditions since traders often struggle to be profitable in bear markets. However, during strong bull markets, the situation can reverse. Traders may open more long positions which can lead to trader profitability at the expense of JLP holders.

#### Risks

JLP holders are exposed to risks that can impact their portfolio, such as:

1.  **Market volatility**

    Rapid price movements can negatively impact the JLP. Extreme market events or black swan scenarios may cause correlations between assets to break down, potentially amplifying losses instead of mitigating them.

2.  **Counterparty risk**

    The JLP pool poses a counterparty risk to JLP holders, as smart contract vulnerabilities or platform issues could potentially impact the ability to maintain or close hedge positions. That said, Jupiter is working with leading firms in the field to audit and maintain our contracts to protect the Jupiter Perpetuals exchange and JLP holders.

3.  **Opportunity cost**

    Capital allocated to acquiring JLP could potentially earn higher returns if allocated elsewhere. In bull markets for example, JLP may underperform compared to simply holding the underlying assets. JLP holders should thoroughly research and understand the benefits and risks of the JLP token before acquiring them.

---

### 1.2. How To Get JLP?

#### Method 1: Jupiter Swap

For a detailed breakdown of swaps, check out [How to Swap](https://support.jup.ag/hc/en-us/articles/18735703603740-Swapping-on-Jupiter-with-Gasless-Option).

1.  Go to the [USDC-JLP](https://jup.ag/swap/USDC-JLP) pair on Jupiter Swap
2.  Make sure your wallet is connected. There should be an icon on the top right showing your connection status.
3.  Choose the token you'd like to trade for JLP in the "You're Selling" section with the token selector.
4.  JLP should already be selected as the token in the "You're Buying" section.
5.  Decide the amount you'd like to swap. An estimate of the quantity of JLP you will receive will appear in the "You're Buying" section.
6.  Click on the `Swap` button found near the bottom of the page.
7.  A notification asking for confirmation of the transaction will appear. Confirm the transaction.
8.  You will receive a message on the success of the transaction.

#### Method 2: JLP Page

1.  Go to [JLP Page](https://jup.ag/perps-earn).
2.  There will be an interface on the right side with an option to Trade, Buy, and Sell. Select Buy.
3.  An input box will appear at the bottom of the interface with a token selector.
4.  Select the token you'd like to trade for JLP.
5.  Decide the amount you'd like to trade for JLP.
6.  A module will appear at the bottom showing the estimated quantity of JLP and the fees for buying into the pool.
7.  A notification asking for confirmation of the transaction will appear. Confirm the transaction.
8.  You will receive a message on the success of the transaction.

---

### 1.3. How to Become a Liquidity Provider (LP)

Anyone can become a Liquidity Provider by contributing assets or tokens to the Jupiter Liquidity Provider Pool (JLP Pool).

*   JLP tokens represent your share of the pool.
*   Any asset traded on Jupiter Exchange can be used to buy JLP.
*   There are fees associated with buying into the JLP pool.

The best way to purchase or exit JLP is always via [Jupiter Instant](https://jup.ag/swap/USDC-JLP).

---

### 1.4. What is JLP?

The **Jupiter Liquidity Provider (JLP)** Pool is a liquidity pool that acts as a counterparty to traders on [Jupiter Perps](https://jup.ag/perps). Traders borrow tokens from the pool to open leveraged positions on the Jupiter Perpetuals exchange.

The JLP token derives its value from:

*   An index fund of **SOL, ETH, WBTC, USDC, USDT**.
*   Trader's **profit and loss**.
*   **75%** of the generated fees from **opening** and **closing** fees, **price impact**, **borrowing fees**, and **trading fees** of the pool.

You can view the APY and Performance of JLP, [here.](https://jup.ag/perps-earn)

---

### 1.5. What are the risks associated with JLP?

*   Price of Assets held in JLP will go down
*   Smart contract risk of the Jupiter Perps Platform
*   Traders earning positive P&L (As JLP acts as a counter-party)
*   Low Utilization/Activity on Jupiter Perps.

View Dashboards by [Chaos Labs](https://community.chaoslabs.xyz/jupiter/risk/overview) and [Gauntlet](https://dashboards.gauntlet.xyz/protocols/jupiter) to understand JLP's activity more.

---

## 2. How Jupiter Perps Work

### 2.1. Price Oracles

Jupiter Perps aggregates token prices from three price oracles:

*   Edge by Chaos Labs
*   Chainlink
*   Pyth

**Edge by Chaos Labs** is the **primary oracle** for Jupiter Perps, with Chainlink and Pyth being used to verify the Edge oracle prices and acting as backup price oracles. The following describes how the multi-oracle system works:

1.  If the Edge oracle is not stale and its price difference is within a set threshold of both Chainlink and Pyth prices, the Edge oracle price is used.
2.  If the Edge oracle is stale or its price exceeds the threshold, the system compares Chainlink and Pyth prices. If Chainlink and Pyth prices are within the threshold, the latest price between the two oracles is selected.
3.  If two out of three oracles fail, no price update will happen.

*NOTE: Oracle price updates happen during trade execution and through regular updates by a dedicated keeper.*

This multi-oracle approach ensures Jupiter Perps uses accurate token prices even during market volatility or when a price oracle fails.

#### Oracle Price Accounts

The oracle price data is stored the following onchain accounts:

*   SOL: <https://solscan.io/account/FYq2BWQ1V5P1WFBqr3qB2Kb5yHVvSv7upzKodgQE5zXh>
*   ETH: <https://solscan.io/account/AFZnHPzy4mvVCffrVwhewHbFc93uTHvDSFrVH7GtfXF1>
*   BTC: <https://solscan.io/account/hUqAT1KQ7eW1i6Csp9CXYtpPfSAvi835V7wKi5fRfmC>
*   USDC: <https://solscan.io/account/6Jp2xZUTWdDD2ZyUPRzeMdc6AFQ5K3pFgZxk2EijfjnM>
*   USDT: <https://solscan.io/account/Fgc93D641F8N2d1xLjQ4jmShuD3GE3BsCXA56KBQbF5u>

#### Migration Guide

If you were using the old price oracle account (set in the custody account's *`dovesOracle`* field), you should migrate to the new price oracle accounts by using the *`dovesAgOracle`* field instead.

[You can also fetch the latest IDL for the oracle program here.](https://github.com/julianfssen/jupiter-perps-anchor-idl-parsing/blob/main/src/idl/doves-idl.ts)

---

### 2.2. How limit orders work on Jupiter Perps?

A typical market order to open a position will open at the current price of the market. Now with limit order, you can indicate a specific price for the position to be opened.

#### General Notes

*   Limit Orders operate independently from your existing positions.
*   They remain active until either triggered at your specified price or manually cancelled.
*   If triggered, they will either:
    *   Open a new position if you have no existing position.
    *   Increase and combine with your existing position in that market.
*   They stay active even if you close or get liquidated on an existing position.
*   The Perps V2 Beta supports up to 20 limit orders on the same pair and side.

#### Important Notes

**⚠️ Placing Limit Order Near Liquidation Price**

Jupiter Perps does not enforce First-in, First-out (FIFO), meaning execution order is not strictly based on price priority. Instead, it depends on which transaction - your Limit Order (LO) or the liquidation transaction - gets processed first.

If you create a Limit Order at a price near your liquidation level, and having the expectation of potentially saving your existing position, you must know that the outcome is uncertain:

*   If the Limit Order transaction executes first = the position may be saved from liquidation
*   If the Liquidation transaction executes first = the existing position will be liquidated, but the Limit Order will remain active, potentially opening a new position immediately.

**⚠️ Liquidation Price on Order Form**

The liquidation price on the order form for a Limit Order will be the **simulated liquidation price** based on the position requested at the time when you fill in the order form.

*   If you have an existing position = liquidation price includes existing position + current requested order
*   If you have no existing position = liquidation price based on current requested order

**However, the liquidation price on the order form does not represent the liquidation price when the limit order is triggered and the position is opened - as when the limit order is executed, conditions may have changed.**

**ℹ️ Limitation on Limit Order**

*   When the selected market's utilisation is above 80%, new limit orders cannot be created.

---

### 2.3. Collateral Management

Traders can open up to 6 positions at one time:

*   Long SOL
*   Long wETH
*   Long wBTC
*   Short SOL
*   Short wETH
*   Short wBTC

When a trader opens multiple positions for the same side (long / short) and token:

1.  Open positions: Price and fee calculations are based on the existing open position. This is essentially increasing the size of an open position.
2.  Deposits, withdrawals, and closing: Price, PNL, and fee calculations are also based on the existing open position.

Traders can open long or short positions or increase the size for existing positions for SOL with up to 100x, ETH and wBTC with up to 150x leverage based on the initial margin (collateral).

#### For Long Positions

##### Collateral Management for Long Positions

Traders can deposit or withdraw collateral from the position to manage the position's margin.

*   When traders deposit collateral, the liquidation price and leverage for the long position decreases as the maintenance margin increases.
*   When traders withdraw collateral, the liquidation price and leverage for the long position increases as the maintenance margin decreases.

##### Underlying Collateral

The underlying collateral for a long position is the token for the open position, as shown below:

| Position | Collateral |
| :------- | :--------- |
| Long SOL | SOL        |
| Long ETH | ETH        |
| Long wBTC | wBTC       |

Profits and collateral withdrawals are disbursed to traders in the token that is being longed.

For example, a trader with a profit long SOL position will receive the underlying token, SOL by default when they close the position. Trader has the option to choose to receive USDC as well.

#### For Short Positions

##### Collateral Management for Short Positions

Traders can deposit or withdraw collateral from the position to manage the position's margin.

*   When traders deposit collateral, the liquidation price increases and leverage for the short position decreases as the maintenance margin increases.
*   When traders withdraw collateral, the liquidation price decreases and leverage for the short position increases as the maintenance margin decreases.

##### Underlying Collateral

The underlying collateral for a long position is the token for the open position, as shown below:

| Position | Collateral |
| :------- | :--------- |
| Short SOL | USDC       |
| Short ETH | USDC       |
| Short wBTC | USDC       |

Profits and collateral withdrawals are paid out to traders in the stablecoin used as the underlying collateral for the position.

For example, a trader with a profitable short SOL position with USDC as the underlying collateral will receive USDC when they close the position or withdraw collateral. Trader has the option to get paid out in the underlying token (e.g. SOL for SOL-Short) as well.

---

### 2.4. How Jupiter creates and executes trade requests (Request Fulfillment Model)?

Jupiter Perpetuals uses an **onchain request fulfillment model** to create and execute trade requests, ensuring a seamless and automated trading experience.

![Image showing how Jupiter Perps creates and executes trade requests](https://support.jup.ag/hc/article_attachments/18735135237660)

#### Creating a Trade Request

Traders can perform a variety of actions on the Jupiter Perpetuals exchange, including:

*   Opening a position
*   Closing a position
*   Increasing position size
*   Decreasing position size (or closing a partial position)
*   Depositing collateral
*   Withdrawing collateral
*   Creating, editing, or closing take profit (TP) and stop loss (SL) orders

These actions can be performed through the **Jupiter Perpetuals platform** or via the **API**. Once the trader initiates an action, the **frontend** or **API server** verifies the action and submits a **transaction** to the **Solana blockchain**, containing all the necessary data (like trade size, collateral size, position side, etc.) to fulfill the request.

For example, when a trader creates an open position request, the submitted transaction will include all details required to open the position on the blockchain.

#### Fulfilling a Trade Request

Jupiter Perpetuals uses **keepers** to automatically process and execute trade requests.

*   **Keepers** are offchain services that listen to onchain events (like trade requests) and perform the corresponding actions without manual intervention.
*   Jupiter runs two keepers that **continuously poll** the Solana blockchain for trade requests.

Once a keeper detects a new trade request, it **verifies** the details and creates a new transaction to **execute** the trade. The trade is only executed when the transaction **succeeds** and is **confirmed** by the Solana blockchain, which then updates the position or the TP/SL request.

This system requires **two transactions** to complete a trade request: one to create the request and another to fulfill and execute it.

By utilizing this model, Jupiter ensures that every trade request is fulfilled efficiently and automatically, allowing traders to focus on their strategy without worrying about manual intervention. Keepers help ensure that the requests are processed promptly and correctly, making the entire process smooth and seamless!

---

### 2.5. Example of a Perp Trade

Now that we've covered all the concepts, let's make it a bit more exciting with a **real-life example** of a trade on Jupiter Perpetuals!

For a step by step guide on how to trade on the Perps dashboard, click [here](https://support.jup.ag/hc/en-us/articles/18734967564060-How-to-Open-Position).

Imagine a trader wants to open a **2x long position on SOL**. Here’s the setup:

*   **Position size**: $1000 USD
*   **Collateral**: $500 USD worth of SOL (this is what the trader deposits)
*   **Borrowed amount**: $500 USD worth of SOL from the liquidity pool (basically borrowing to leverage the position)

#### The Setup:

*   **Initial position value**: $1000 USD
*   **Initial deposit**: $500 USD
*   **Borrowed amount**: $500 USD
*   **Leverage**: 2x
*   **Initial SOL price**: $100
*   **Utilization rate**: 50% (just the percentage of tokens borrowed vs. available in the pool)
*   **Borrow rate**: 0.012% per hour
*   **Position opening fee**: \(0.06\% \cdot \$1000 = \textbf{\$0.6}\)

#### What Happens Next?

The trader holds the position for **2 days** (48 hours) while the price of **SOL** appreciates by **10%**! Let’s see how this impacts things:

*   **Final position value**: $1100 USD (because SOL price went up to $110)
*   **Final SOL price**: $110
*   **Position closing fee**: \(0.06\% \cdot \$1100 = \textbf{\$0.66}\)

#### Calculating the Borrow Fee:

Now, the trader has to account for the **borrow fees** they accumulate over the two days. Here’s how the borrow fee works:

*   **Hourly borrow fee**: (Tokens borrowed / Tokens in the pool) \* Borrow rate \* Position size This is calculated as: \(\$500 \text{ (borrowed)} / \$1000 \text{ (total in pool)} \cdot 0.012\% \cdot \$1000 = \textbf{\$0.6 per hour}\)
*   Over **48 hours**, the **total borrow fee** is: \(\$0.6 \cdot 48 = \textbf{\$2.88 USD}\)

#### Final Profit:

Let’s crunch the numbers! Here’s how the trader’s **final profit** works out:

*   **Final position value**: $1100
*   **Initial position value**: $1000
*   **Borrow fee**: $2.88
*   **Opening fee**: $0.6
*   **Closing fee**: $0.66

So, the trader’s **final profit** after the 2-day trade is:

\(\$1100 - \$1000 - \$2.88 - \$0.6 - \$0.66 = \textbf{\$95.86 USD}\)

**Ta-da!** The trader made a neat **$95.86 profit** from this trade! Simple as that!

---

### 2.6. Fees

#### Base Fees

A flat rate of **0.06%** of the position amount is charged when opening or closing a position. This base fee is also charged when a position is closed partially.

To calculate the base open or close for a trade:

```
BPS_POWER = 10^4      // 10_000

// 1. Get the base fee (BPS) from the custody account's `increasePositionBps` for open position requests
// or `decreasePositionBps` for close position requests
// <https://github.com/julianfssen/jupiter-perps-anchor-idl-parsing/blob/main/src/idl/jupiter-perpetuals-idl.ts#L2677>
   baseFeeBps = custody.increasePositionBps

// 2. Convert `baseFeeBps` to decimals
   baseFeeBpsDecimals = baseFeeBps / BPS_POWER

// 3. Calculate the final open / close fee in USD by multiplying `baseFeeBpsDecimals` against the trade size
   openCloseFeeUsd = tradeSizeUsd * baseFeeBpsDecimals
```

This [code snippet](https://github.com/julianfssen/jupiter-perps-anchor-idl-parsing/blob/main/src/examples/get-open-close-base-fee.ts) contains an example on calculating open and close base fees programmatically.

#### Price Impact Fees

When trading on Jupiter Perpetuals, large trades don't incur price impact due to the use of price oracles for token prices.

To address this, Jupiter Perpetuals implements a**price impact fee**, which simulates the price impact on traditional exchanges (price changes due to limited liquidity at each price level).

![Image of graph of Notional Size USD vs Trading Fee USD](https://support.jup.ag/hc/article_attachments/18735045234076)

##### How to Calculate the Price Impact Fee:

```
USDC_DECIMALS = 10^6  // 1_000_000
BPS_POWER = 10^4      // 10_000

Calculate Price Impact Fee:

// 1. Get the trade impact fee scalar from the custody account's `pricing.tradeImpactFeeScalar` constant
// <https://dev.jup.ag/docs/perp-api/custody-account>
   tradeImpactFeeScalar = custody.pricing.tradeImpactFeeScalar

// 2. Convert trade size to USDC decimal format
   tradeSizeUsd = tradeSizeUsd * USDC_DECIMALS

// 3. Scale to BPS format for fee calculation
   tradeSizeUsdBps = tradeSizeUsd * BPS_POWER

// 4. Calculate price impact fee percentage in BPS
   priceImpactFeeBps = tradeSizeUsdBps / tradeImpactFeeScalar

// 5. Calculate final price impact fee in USD
   priceImpactFeeUsd = (tradeSizeUsd * priceImpactFeeBps / BPS_POWER) / USDC_DECIMALS
```

This [code snippet](https://github.com/julianfssen/jupiter-perps-anchor-idl-parsing/blob/main/src/examples/get-price-impact-fee.ts) contains an example on calculating price impact fees programmatically.

##### How the Price Impact Fee Helps:

1.  **Orderbook simulation**
    The fee structure mimics traditional order book dynamics, helping to prevent price manipulation. The fee structure ensures that the fee is proportional to the impact a trade might have on the market, making the environment fairer for both traders and liquidity providers.

2.  **Fair compensation for JLP holders**

    The price impact fee compensates for the undesirable delta imposed on the JLP. Traders will not benefit from splitting positions across different trades or addresses, as positions must be opened quickly before their alpha decays. The liquidity pool receives reasonable trading fees regardless of whether traders open large trades or split them up.

Jupiter works with partners like [Chaos Labs](https://chaoslabs.xyz/) and [Gauntlet](https://www.gauntlet.xyz/) to set and optimize the price impact parameters above. The parameters may be adjusted over time as market conditions change. Consult [the proposal and analysis on the price impact fee here](https://www.jupresear.ch/t/jupiter-perpetuals-price-impact-fee-mechanism/17140) for additional information on calculating the price impact fee and other useful information.

#### Borrow Fees

On the Jupiter Perpetuals exchange, traders can open leveraged positions by borrowing assets from the liquidity pool. In return, they pay borrow fees, which serve two key purposes:

1.  **Compensating Liquidity Providers** – The fees ensure liquidity providers are rewarded for supplying assets to the pool.
2.  **Managing Risk** – The fees help balance the risks associated with leveraged trading.

Unlike other exchanges that charge funding rates for open positions, Jupiter Perpetuals uses a borrow fee system. These fees compound hourly based on the amount borrowed for the leveraged position.

The borrow fees are reinvested back into the **Jupiter Liquidity Pool (JLP)**, boosting its yield and liquidity. This also helps align the token mark price with its spot market price.

##### How the Borrow Fee Works:

The **hourly borrow fee** is calculated using the following formula:

**Hourly Borrow Fee** = Total Tokens Locked / Tokens in Pool (Utilization) × Hourly Borrow Rate × Position Size

*   **Utilization:** The proportion of tokens in the pool that are currently locked in open positions.
    *   **Total Tokens Locked:** Tokens locked in all open positions.
    *   **Total Tokens in Pool:** Tokens deposited into the liquidity pool for the position’s underlying token.
*   **Hourly Borrow Rate:** The base rate charged for borrowing, which varies by asset.
*   **Position Size:** The USD value of the leveraged position.

##### Finding Hourly Borrow Rates:

The **hourly borrow rate** for assets can be found in the **Borrow Rate** section of the trade form or accessed on-chain via the **funding\_rate\_state.hourly\_funding\_dbps** field in the custody account.

##### Calculating Utilization Rate:

To calculate the **utilization rate**, use the following:

```
// Calculate utilization percentage
if (custody.assets.owned > 0 AND custody.assets.locked > 0) then
    utilizationPct = custody.assets.locked / custody.assets.owned
else
    utilizationPct = 0

// Get hourly funding rate in basis points
hourlyFundingDbps = custody.fundingRateState.hourlyFundingDbps

// Convert basis points to percentage and apply utilization
hourlyBorrowRate = (hourlyFundingDbps / 1000) * utilizationPct
```

##### Worked Example:

Let’s say the price of **SOL** is $100. Here’s how the fee is calculated for a 100 SOL position:

*   **Total Tokens Locked** = 200 SOL (100 SOL from the position + 100 SOL in the pool)
*   **Total Tokens in Pool** = 1,010 SOL (1,000 SOL in custody + 10 SOL collateral)
*   **Utilization** = 200 SOL / 1,010 SOL = 19.8%
*   **Hourly Borrow Rate** = 0.012% (0.00012 in decimal)

Using the formula:

**Hourly Borrow Fee** = \((200 / 1,010) \cdot 0.00012 \cdot 10,000 = \textbf{0.238}\) USD/hour.

This means your position will accrue a borrow fee of **$0.238** every hour it remains open.

##### Important Considerations:

*   **Ongoing Borrow Fees:** Borrow fees are continuously deducted from your collateral, which impacts both your **leverage** and **liquidation price** over time.
*   **Monitoring Fees:** Since fees increase with position duration, it's important to regularly check your borrow fees and liquidation price to avoid unexpected liquidation, especially during volatile market conditions.

**Note:** Jupiter works with experts, like **Gauntlet**, to optimize borrow fees and ensure they remain fair and balanced for both traders and liquidity providers. You can check Gauntlet's analysis [here](https://station.jup.ag/guides/perpetual-exchange/how-it-works#borrow-fee).

#### Transaction and Priority Fees

Traders will have to pay SOL for submitting transactions onto the Solana chain. Traders also pay priority fees or Jito bundle tips (or both) depending on their settings.

At the same time, a minor SOL amount will be used for rent to create an escrow account ([PDA](https://solanacookbook.com/core-concepts/pdas.html#facts)). The SOL rent will be returned to you once you close your position.

---

### 2.7. How leverage works on Jupiter Perps?

#### What is Leverage?

Leverage allows you to trade larger positions than your initial capital by borrowing funds. On **Jupiter Perps**, you can use leverage to increase your position size, allowing you to amplify potential profits. However, leverage also increases risk, as both gains and losses are magnified.

#### How Leverage Works

When you open a position with leverage, you’re essentially borrowing capital from the platform to control a larger position than you could with your own collateral. For example, with **100x leverage**, you can control a position worth 100 times the amount of collateral you’ve deposited.

The leverage you choose impacts the size of your position and the potential for profit or loss. However, higher leverage also means higher risk. A small price movement against your position can lead to liquidation if your collateral isn’t enough to cover the loss.

#### Leverage Limits

Jupiter Perps offers leverage up to **100x** for **SOL**, **150x** for **ETH** and **wBTC**. You can adjust your leverage depending on the size of your position and the risk you’re willing to take. The more leverage you use, the higher the potential profit, but the greater the risk of liquidation.

#### Leverage Slider

The **Leverage Slider** on the Jupiter Perps interface allows you to easily adjust your leverage. Moving the slider lets you select leverage from **1.1x** to **100x**, depending on your risk appetite and strategy.

#### Margin and Maintenance Margin

When using leverage, your **initial margin** (collateral) is the amount you must deposit to open the position. The **maintenance margin** is the minimum collateral required to keep your position open. If your collateral falls below the maintenance margin due to losses, you’ll receive a **PnL call** to add more collateral. If not replenished, your position will be liquidated.

#### Risks of Using Leverage

While leverage can amplify profits, it also increases the risk of liquidation. If the market moves against you, your position can be closed automatically if your collateral isn’t enough to cover the loss. It’s crucial to manage leverage carefully, monitor your **PnL**, and use tools like **stop-loss orders** to mitigate risk.

#### Best Practices for Using Leverage

*   **Start small:** Begin with lower leverage and increase as you gain confidence and experience.
*   **Use stop-loss orders:** Always set stop-loss orders to limit potential losses.
*   **Monitor your position:** Keep an eye on your **PnL** and collateral to avoid liquidation.

---

### 2.8. How to Open Position

Welcome to the world of **Jupiter Perps**—where trading gets fast, exciting, and packed with potential! Whether you're looking to go **long** or **short** on top tokens like **SOL**, **ETH**, or **wBTC**, this guide will walk you through every step to master the platform.

#### Step 1: Connect Your Wallet

To begin trading, head to [Jupiter Perps](https://jup.ag/perps) and click on the **“Connect Wallet”** button in the top-right corner. From there, select your preferred Solana wallet, such as Phantom or Solflare, and approve the connection. Once connected, you’re ready to dive into the trading interface!

#### Step 2: Select the Market

The next step is choosing the market you want to trade in. Using the **Perp Market Selector**, pick from the supported blue-chip tokens: **SOL**, **ETH**, or **wBTC**. This defines the token for which you will open a leveraged long or short position.

#### Step 3: Choose Long or Short

Now it’s decision time—are you betting on the price going up or down? Use the **Long/Short Selector** to choose your position. Selecting **Long** means you believe the token’s price will rise, while **Short** indicates you expect the price to fall.

#### Step 4: Set Your Collateral

After deciding your position, it’s time to set your collateral. In the **Input Token Selector**, choose the SPL token you want to use as collateral for your trade and specify the amount you wish to deposit. This collateral will support your position and determine your potential trade size.

#### Step 5: Adjust Leverage

With your collateral in place, use the **Leverage Slider** to set your desired leverage. You can choose anywhere from a modest **1.1x** to a maximum of **100x or 150x** leverage. Remember, higher leverage increases both potential rewards and risks, so adjust thoughtfully!

#### Step 6: Review Price Stats

Before placing your order, check the **Price Stats** section for essential market data. Here, you’ll find real-time metrics like the current index price, 24-hour price movements, highs, and lows. These stats can help you make an informed trading decision.

#### Step 7: Review Order Summary

Take a moment to review the **Order Summary**, which provides all the key details of your trade, including position size, leverage, entry price, and estimated liquidation price. Ensure everything looks good before proceeding, as this is your last chance to tweak the settings.

#### Step 8: Submit Your Order

When you’re satisfied with the order details, click the **“Open Long”** or **"Open Short"** button to confirm. You’ll need to approve the transaction in your connected wallet. Once approved, your trade is live, and you’ve officially entered the Jupiter Perps market!

#### Step 9: Monitor and Manage Your Position

After your trade is live, head to the **Positions Tab** to monitor it. Here, you can track real-time profit and loss (PnL), add or withdraw collateral, or make adjustments to your position. This tab ensures you stay in control of your trades.

#### Step 10: Closing Your Position

When you’re ready to close your trade, go to the **Positions Tab** and select the position you wish to exit. You can choose to close it partially or fully based on your strategy. Confirm the closure and approve the transaction in your wallet to finalize the trade.

---

### 2.9. Perps Quickstart

Welcome to Jupiter Perps, a perpetual exchange where you can trade with leverage **up to 100x on SOL,** and **up to 150x** on **ETH and** **wBTC**.

The Jupiter Perps is a trader-to-LP exchange which means liquidity providers (LPs) provide liquidity to the pool, while the traders borrow tokens from the liquidity pool (the JLP pool) for leverage.

#### Jupiter Liquidity Providers

Liquidity for the Jupiter Perps is provided by the JLP Pool, which holds SOL, ETH, wBTC, and USDC as the underlying tokens. The pool provides ample liquidity for traders to open highly-leveraged positions, while earning an attractive return on a portion of the trading fees.

#### Traders

Traders can deposit collateral to enter positions and borrow the rest of the position from the pool. Subsequently, they can manage their positions by withdrawing collateral to close their positions fully or partially. The Jupiter Perpetuals Exchange features a simple interface and underlying trading mechanisms to provide a safe and seamless trading experience.

[Learn more about how trading works here](https://support.jup.ag/hc/en-us/articles/18735419447452-Collateral-Management).

#### Key Features for Liquidity Providers

*   **Rewards and Earnings**

    By contributing to the JLP Pool, liquidity providers earn a portion of the trading fees and traders' profits and losses, which is redistributed to pool.

*   **Composability**

    The JLP token is a SPL token, making it easily transferable and tradable like any other SPL tokens within the Solana ecosystem. This also allows for AMM pools to be established for trading JLP tokens.

*   **No Active Management**

    LPs do not need to actively "stake" tokens or "harvest" yields - APR earnings are embedded within each JLP token and accumulate automatically (reflected as an an increase in the JLP token price).

#### Key Features for Traders

*   **Liquidity and Leverage**

    The JLP Pool provides ample liquidity for traders to open highly-leveraged positions, up to 150x.

*   **Collateral**

    Traders can choose any token as collateral and Jupiter will swap the collateral token to the position underlying token, powered by Jupiter Instant.

*   **Price Oracles**

    The exchange uses price oracles for pricing, this benefits traders by allowing them to trade with leverage without worrying about price impact like traditional derivatives platforms (however, there is a [price impact fee](https://support.jup.ag/hc/en-us/articles/18735045234588-What-are-the-fees-associated-with-Jupiter-Perps#h_01JN4Q09D7CP5JRCH4Q7VGCHD6)).

*   **Risk and Economic Management**

    Jupiter Perps is working closely with different economic and risk teams to ensure a stable, fair yet competitive trading environment. Refer to the assessments at <https://www.jupresear.ch/tag/risk>.

*   **Limit Orders**

    Traders can place limit orders to enter their long or short positions at a specific price. This allows for more control over the entry price of their positions.

*   **Gasless Orders**

    Traders can place orders without paying for transaction fees. This is powered by a new keeper execution model where you submit a request to execute an order (direct to keepers instead of a transaction on-chain), and the keeper will execute the order for you.

---

### 2.10. How to Calculate PnL

#### What is PnL?

**PnL** stands for **Profit and Loss**, which reflects how much you’ve gained or lost on your open position. It’s a critical metric to track as it shows how well your trade is performing based on price movements.

#### How PnL Works

When you open a position on Jupiter Perps (long or short), your **PnL** updates in real-time as the market moves. The **PnL** value can either be **positive** (profit) or **negative** (loss), depending on whether the price is moving in your favor or against you.

For **long positions**, if the price of your asset goes up, your **PnL** increases. If the price drops, your **PnL** decreases. For example:

*   Position: 1,000 USD long on SOL
*   If SOL price increases by 10%: You profit 100 USD
*   If SOL price decreases by 10%: You lose 100 USD

For **short positions**, the opposite happens. If the price of the asset goes down, your **PnL** increases. If the price rises, your **PnL** decreases. For example:

*   Position: 1,000 USD short on SOL
*   If SOL price decreases by 10%: You profit 100 USD
*   If SOL price increases by 10%: You lose 100 USD

#### Understanding PnL Calls

A **PnL Call** occurs when the value of your open position reaches a certain threshold, triggering a call for additional collateral to maintain the position.

If your **PnL** turns negative and your margin falls below the maintenance level, you may be required to deposit more collateral to avoid liquidation.

#### Calculating realized and unrealized PnL

```
// 1) Get the current token price / exit price

exitPrice = currentTokenPrice

// 2) Determine if the position is profitable by checking if the exit price is greater than the position's
// average price for longs, or if the exit price is less than the position's average price for shorts

IF isLong THEN
    inProfit = exitPrice > positionAvgPrice
ELSE
    inProfit = exitPrice < positionAvgPrice

// 3) Calculate the absolute delta between the exit price and the position's average price

priceDelta = |exitPrice - positionAvgPrice|

// 4) Calculate the PnL delta for the closed portion of the position: multiply the size being closed (`tradeSizeUsd`) 
// by the price delta, then divide by the entry price to get the PnL delta

pnlDelta = (tradeSizeUsd * priceDelta) / positionAvgPrice

// 5) Calculate the final unrealized PnL depending on whether the position is profitable or not

IF inProfit THEN
    unrealizedPnl = pnlDelta
ELSE
    unrealizedPnl = -pnlDelta

// 6) Deduct the outstanding fees from the unrealized PnL to get the final realized PnL
// Read the `Fee` section below to understand how the fee calculations work
realizedPnl = unrealizedPnl - (closeBaseFee + priceImpactFee + borrowFee)
```

By understanding and monitoring your **PnL**, you can make informed decisions on whether to adjust your position, take profits, or cut losses, giving you more control over your trades.

---

### 2.11. Liquidation

The liquidation price for an open position represent the price at which the position will be automatically closed by the system when the trader's collateral is insufficient to maintain the position.

#### For long positions:
Liquidation occurs when the current token price falls below the liquidation price.
Example: If the liquidation price is $90, the long position will be closed if the price of the token falls below $90.

#### For short positions:
Liquidation occurs when the current token price rises above the liquidation price.
Example: If the liquidation price is $110, the short position will be closed if the token price rises above $110.

The liquidation price can be calculated with the following formulae:

$$
\text{Long Liquidation Price} = \text{price} \cdot \left(1 - \frac{\text{collateral\_size} - \text{close\_fee} - \text{borrow\_fee}}{\text{size}}\right)
$$

$$
\text{Short Liquidation Price} = \text{price} \cdot \left(1 + \frac{\text{collateral\_size} - \text{close\_fee} - \text{borrow\_fee}}{\text{size}}\right)
$$

Where:
1.  **price**: The average price (USD) of the position
2.  **collateral\_size**: The collateral size (USD) for the position
3.  **close\_fee**: The fee (USD) charged for closing the position
4.  **borrow\_fee**: The accumulated borrowing fees (USD) for maintaining a leveraged position
5.  **size**: The size (USD) of the position
6.  **max\_lev**: The maximum allowed leverage (500x is the maximum allowed leverage in the Jupiter Perpetuals exchange for now)

#### Note:

The liquidation price changes over time as it is affected by fees, particularly with leverage exceeding 10x and the accumulation of borrow fees over extended position durations. Regularly monitoring your liquidation price is essential.

To mitigate the risk of liquidation, collateral adjustments and leverage fine-tuning can be performed through the Edit button in the position row, offering an avenue to add collateral and enhance the liquidation price.

---

## 3. Frequently Asked Questions (FAQ)

### 3.1. How are token prices determined on Jupiter Perps?

Token prices for SOL, wETH, wBTC, USDC, and USDT are determined by onchain price oracles.

The prices sourced from the oracles are used as the mark price for:

*   Opening and closing positions
*   Increasing or reducing position sizes
*   Depositing or withdrawing collateral
*   Calculating PNL
*   Calculating liquidation prices
*   Triggering TP / SL requests
*   Price charts

Jupiter is working with [Chaos' Edge Pricing Data](https://x.com/omeragoldberg/status/1834231003071774778) that provide fast, accurate, and reliable price data for the supported tokens on the Jupiter Perpetuals exchange.

**Note:** Price data used in the Jupiter Perpetuals exchange may differ from other onchain and offchain price aggregators. Traders should use the Jupiter Perpetuals price chart and historical prices as the source of truth when making trade decisions.

### 3.2. Why do long and short positions use different collateral tokens?

Traders can deposit any [SPL token supported by Jupiter Swap](https://station.jup.ag/docs/old/token-list) as the initial margin to open a position or to deposit collateral for an existing open position.

The deposited tokens will then be converted to the collateral tokens for the position (SOL / wETH / wBTC for long positions, USDC / USDT stablecoin for short positions).

The platform will automatically swap the deposited tokens to the right collateral token so traders don't need to swap tokens manually before opening a position or increasing their collateral.

The underlying collateral for long positions are the tokens themselves (SOL, wBTC, wETH) and stablecoins (USDC, USDT) for short positions.

This is to protect the pool from scenarios that might put the pool at risk, for example a series of ongoing profitable trades that deplete the pool's reserves.

For more information on this, consult the [JLP pool documentation](https://station.jup.ag/guides/jlp/How-JLP-Works#risks-associated-with-holding-jlp) which describes the dynamics between traders and the liquidity pool for long and short scenarios.

### 3.3. Why are my collateral sizes fixed on Jupiter Perps?

When providing the initial margin or depositing collateral, the exchange records the position's collateral value in USD. The recorded USD value remains constant, even if the collateral token's price fluctuates.

For example, if a trader deposits $100 USD worth of SOL to open a position, their collateral will always be valued at $100 for this position. Even if the price of SOL changes by 50% in either direction, the trader's collateral size for this position remains fixed at $100.

The platform will automatically swap the deposited tokens to the right collateral token so traders don't need to swap tokens manually before opening a position or increasing their collateral.

### 3.4. I have an existing SOL-Long position. I added 1 SOL to open a new SOL-Long position but I don’t see my new position. Why?

You could only have 1 position for each market and side.

When you try to open a position by adding collateral, both positions will be combined into one position, where

*   Leverage of the combined position = Average of the leverage level of each position. e.g. \((1.2\text{x} + 1.4\text{x}) / 2 = 1.3\text{x}\)
*   Size = initial collateral \* leverage level.

The TP/SL you have set earlier will remain the same after the positions are combined.

### 3.5. I deposited 1 SOL (where 1 SOL = $100) for a 5x leveraged SOL-Long position and profited $50 (where 1 SOL = $110). Why did I get less than the full amount?

Assuming zero fees, with $50 profit, you will be getting SOL in return with value of $150. At the time of closing the position, 1 SOL = $110

$$
\$150 / \$110 = 1.3636
$$

You will be getting 1.3636 SOL where the value is equivalent to $150.

![Screenshot of the Deposit/Withdrawal tab](https://station.jup.ag/assets/images/faq1-abc2a8e9132fb1a3c29916bbebb298e4.png)

Here is another example, this is a SOL-Long position, the total amount the user should be getting is $3086.28 in SOL.

The value of SOL at the time is $189.28, hence \(\$3086.28 / \$189.28 = 16.30\). The user will be receiving 16.30 SOL in return.

Note: The **`PNL`** shown is the amount before fees. The exact amount is shown under **`Deposit/Withdraw`** tab.

### 3.6. Why is my liquidation price changing?

There is an hourly borrow fee on every position, the hourly borrow fee will change the liquidation price over time.

If you want to understand how the hourly borrow rate works, read [here](https://support.jup.ag/hc/en-us/articles/18735045234588-What-are-the-fees-associated-with-Jupiter-Perps).

### 3.7. I closed my position with profit but I have not received my funds.

You will receive the underlying asset for a LONG-position, i.e. SOL for SOL-Long, ETH for ETH-Long, wBTC for wBTC-Long.

You will receive either USDC or USDT for all SHORT-positions.

Perform these steps to check for funds from closed positions:

1.  Under `Transaction History` on Jupiter Perpetual Trading, click on the transaction ID of the closed position.

    ![Screenshot showing the transaction history page](https://station.jup.ag/assets/images/returned2-1-65f3d076c7dc8729fa2c32cd9c553e7e.png)

2.  You could find your returned fund here:

    For **SOL-Long position**, check **`SOL Balance Change`** tab

    ![Screenshot showing the SOL Balance Change tab](https://station.jup.ag/assets/images/returned2-2-93751145b3202af744365c6c6db85b61.png)

    For **ALL other positions**, check `Token Balance Change` tab

    ![Screenshot showing the Token Balance Change tab](https://station.jup.ag/assets/images/returned2-3-de333426515fc856acf4e5ea77240b21.png)

### 3.8. I tried opening a position/adding collateral and it was not successful. Where are my funds?

Under normal circumstances, funds will be returned to your wallet in about 1-2 minutes. During periods of congestion on the Solana network there might be a delay for your funds to be returned to your wallet. If this is the case, you can close your expired request under the `Expired Orders` tab.

Follow these steps on how to find your funds:

1.  Go to the Transaction ID

    ![Screenshot showing the Transaction Details tab](https://station.jup.ag/assets/images/returned1-cf8cbbf44c256ea6a6e35627fbd3a249.png)

2.  Scroll down to `CreateIncreasePositionMarketRequest`

    ![Screenshot showing where to find the CreateIncreasePositionMarketRequest](https://station.jup.ag/assets/images/returned2-32439cdeb7251770f90de05d9aa0e9a9.png)

3.  Click on `Position Request` address

    ![Screenshot showing the Position Request address](https://station.jup.ag/assets/images/returned3-3c7fbe698daacdac7b98068cc06de2a4.png)

4.  Click on the latest successful transaction

    ![Screenshot showing the latest successful transaction](https://station.jup.ag/assets/images/returned4-b93934a90adfc763a2180e335a9c03f9.png)

5.  You could find your returned fund here:

    For **SOL**, check **`SOL Balance Change`** tab

    ![Screenshot showing the SOL Balance Change tab](https://station.jup.ag/assets/images/returned5-93751145b3202af744365c6c6db85b61.png)

    For **ALL other token**, check `Token Balance Change` tab

    ![Screenshot showing the Token Balance Change tab](https://station.jup.ag/assets/images/returned6-de333426515fc856acf4e5ea77240b21.png)

Note: Wallet service providers might not be fully parsing the transactions, so, it is always a better idea to check on-chain. If you still couldn’t locate your fund although it was shown that it’s returned on the explorers, please contact your wallet service provider accordingly.

### 3.9. What happens when you exceed leverage limits? (Auto closing)

The maximum allowable leverage on Jupiter Perps is **500x**.

Positions are liquidated if the trader’s collateral, after accounting for fees, unrealized profits, and unrealized losses, falls below **0.2%** of the position size.

Note: **Reducing Position Size and Collateral**

When reducing a position’s size, the collateral is also adjusted accordingly to maintain the same leverage. For example, if you’re holding a position with **10x leverage** and reduce the size by **50%**, the collateral is decreased by the same proportion to ensure that the leverage remains at **10x**.

### 3.10. How many positions can be opened on Jupiter Perps at one time?

Traders can open up to 9 positions at one time:

*   Long SOL
*   Long wETH
*   Long wBTC
*   Short SOL (USDC)
*   Short SOL (USDT)
*   Short wETH (USDC)
*   Short wETH (USDT)
*   Short wBTC (USDC)
*   Short wBTC (USDT)

When a trader opens multiple positions for the same side (long / short) and token:

1.  **Open positions**: Price and fee calculations are based on the existing open position. This is essentially increasing or decreasing the size of an open position.
2.  **Deposits, withdrawals, and closing**: Price, PNL, and fee calculations are also based on the existing open position.
