# I. Introduction to Jupiter

## What is Jupiter?

Jupiter is a trading platform built on the Solana blockchain, designed to provide a seamless swap experience from wallet connection to executing the best-priced trade. It features a powerful aggregation engine that sources liquidity from across the Solana ecosystem. For advanced traders, **Jupiter Pro** offers advanced analytics, industry-leading MEV protection, and significantly lower fees to help discover, analyze, and trade tokens at high speed.

## Connecting Your Wallet

To begin trading on any of Jupiter's products (**Swap**, **Recurring**, or **Trigger**), you must first connect a wallet.

1.  Navigate to the relevant page on `jup.ag` (e.g., `jup.ag/swap`, `jup.ag/recurring`).
2.  Click the **“Connect Wallet”** button.
3.  Choose your connection method:
    *   **Quick Account:** An embedded wallet that allows for instant, signless transactions. You can create this wallet via social and web3 logins. It is non-custodial and secured by Privy.
    *   **Classic Mode:** Connect your preferred Solana ecosystem wallet, such as Jupiter Mobile, Phantom, Solflare, or any other supported wallet.
4.  Approve the connection request from within your wallet application.

# II. Instant Swaps: Executing Immediate Trades

## Overview of Swapping on Jupiter

Trading tokens on Jupiter is designed to be simple and efficient. The platform provides a seamless experience, abstracting away technical complexities while ensuring users get the best possible price for their trades.

## Step-by-Step: Swapping Tokens

### Choosing Your Swap Mode: Ultra vs. Manual

Jupiter offers two primary modes for executing swaps:

*   **Ultra Mode:** This mode is designed to "just work." It abstracts away all technical configurations by using Real-Time Slippage Estimation (RTSE) and dynamic priority fees for optimized transaction landing. It is the recommended mode for most users. A 0.1% swap fee is applied on volatile pairs, though swaps between SOL/Stables, JUP/Stables/SOL, JLP/Stables/SOL, and jupSOL/Stables/SOL are free.
*   **Manual Mode:** For users who desire granular control, this mode allows you to pick your own settings for slippage, fees, and more. It charges 0% fee to Jupiter.

### Understanding Swap Details (Price Impact, Route, Network Fee)

When preparing a swap, Jupiter displays several key details:

*   **Price Impact:** This is influenced by the available liquidity and the size of your trade. A large trade in a pool with low liquidity will result in a higher price impact.
*   **Route Details:** This acts as Jupiter's GPS for your tokens. By clicking on the market or route information (e.g., "1 Market" or "2 Routes + 2 Markets"), you can see the exact path your swap will take through various liquidity pools to achieve the quoted price.
*   **Network Fee:** This is the amount paid to the Solana network to process your transaction. If you are using **Manual Mode** with certain settings, this may also include **Jito Tips**, which is a cost for interacting with Solana via Jito validators.

## Advanced Swap Settings (Manual Mode)

By switching to **Manual Mode**, you can customize the following settings:

### Slippage Configuration

*   **Dynamic Slippage:** Automatically adapts to the token and current market conditions.
*   **Fixed Slippage:** Set a specific percentage threshold (e.g., 0.5%, 1%) to control the maximum price deviation you are willing to accept.

### Transaction Broadcasting Options (Priority Fee, Jito Only, Both)

*   **Priority Fee:** Uses a typical RPC and adds a priority fee to help your transaction get included in a block faster.
*   **Jito Only:** Sends your transaction directly to a Jito RPC with a Jito tip for MEV-protected transactions.
*   **Both:** Sends your transaction via both methods simultaneously. If the transaction is processed through a typical RPC, only the Priority Fee is paid. If it is processed through Jito validators, both the Priority Fee and Jito tip are paid.

### Transaction Speed Settings (Fast, Turbo, Ultra)

Adjust the priority fee based on urgency:
*   **Fast:** Lower fees for standard priority.
*   **Turbo:** Moderate fees for higher priority.
*   **Ultra:** Higher fees for maximum speed and priority.

### Fee Modes (Max Cap, Exact Fee)

*   **Max Cap:** A dynamically estimated maximum fee based on your trade size.
*   **Exact Fee:** Specify an exact fee amount for your transaction.

### AMM Exclusion

This setting allows you to turn specific Automated Market Makers (AMMs) on or off, excluding them from being used in your swap route.

## Understanding Wrapped SOL (wSOL)

### What is wSOL and Why is it Needed?

**Wrapped SOL (wSOL)** is native SOL that is wrapped using the Solana Token Program, which allows it to be treated like any other SPL token. Jupiter typically has to wrap SOL before a trade and unwrap it after, because native SOL is not an SPL token. This wrapping/unwrapping process adds an extra instruction to the transaction. For power traders who transact frequently with SOL, using wSOL directly makes trading faster and more convenient.

### How to Enable wSOL Settings

You can enable direct trading with wSOL in **Manual settings** by turning on the **Use wSOL** toggle. Once enabled, a wSOL toolbar will appear on the swap form, helping you track the amount of SOL vs. wSOL in your wallet. When this setting is on, all transactions will use wSOL. Note that any wSOL you receive will not be automatically unwrapped into native SOL.

### Wrapping and Unwrapping SOL

On the wSOL toolbar, click the **Manage** button. A modal will appear that allows you to wrap your native SOL into wSOL or unwrap your wSOL back into native SOL.

# III. Automated Trading: Recurring Orders (DCA)

## Understanding How Recurring Orders Work (In-Depth)

**Recurring** orders, also known as Dollar-Cost Averaging (DCA), allow you to set up a machine that processes your trades at specified intervals.

### Creation Process of Recurring Orders

1.  When you initiate a **Recurring** order, the input tokens (e.g., USDC) are transferred from your wallet into a special vault (a program-owned associated token account).
2.  Your first order is processed immediately upon creating the position. For example, if you set up a plan to swap $1,000 USDC into SOL over 10 days, the full $1,000 USDC is moved to the vault, and the first trade of $100 USDC for SOL happens right away.
3.  The remaining trades execute at regular intervals according to your chosen schedule. To make your strategy less predictable and more secure, each trade executes within a randomized window of ±30 seconds from the scheduled time.

### The Order Execution Mechanism

Your total order is split into smaller, individual trades. For example, a plan to swap $900 USDC into SOL over 3 days will be split into three trades of $300 USDC each.

*   **Day 1:** The first $300 USDC -> SOL trade happens immediately upon confirmation.
*   **Day 2:** Roughly 24 hours later, the second trade executes.
*   **Day 3:** The final trade completes the plan.

The amount of the output token you receive depends on its market price at the time of each trade.

### Token Transfers After Each Order

Each time a recurring order executes, the purchased tokens are automatically sent to your wallet within the same transaction. This automatic transfer requires that you have the correct **Associated Token Account (ATA)** for the purchased token. Jupiter **Recurring** creates this ATA for you when you set up your plan.

**Warning:** If you manually close the ATA for the purchased token (e.g., through your wallet or a third-party tool), future auto-transfers will fail. Instead, the purchased tokens will remain in your **Recurring** vault and will only be transferred as a lump sum at the end of the entire plan. You can also manually withdraw tokens from the vault at any time via the UI.

### Auto-Closing Your Recurring Orders

At the end of your recurring cycle, the program automatically handles the closure.
*   If your wallet's ATA for the output token remains open, tokens are transferred with each order.
*   If you closed the ATA mid-cycle, the program uses a portion of the rent from your order account to re-open the token account and transfer the accumulated tokens to you.
*   By default, 2/3 of the rent from your **Recurring** account is sent back to you. The remaining 1/3 is recoverable if you later close the ATA that holds your tokens.

## Setting Up Your First Recurring Order

### Choosing Tokens and Setting Plan Details

1.  Connect your wallet on the **Jupiter Recurring page**.
2.  Select the token you want to sell and the token you want to buy.
3.  Enter the amount you want to invest per transaction (e.g., $10 worth of SOL).
4.  Choose the frequency of your purchases (minute, hour, day, week, or month).
5.  Enter the total number of orders for the plan.
6.  Optionally, you can set a minimum and maximum price range, and orders will only execute if the market price is within that range.

### Reviewing Your Plan and Approving Transactions

1.  Before confirming, carefully review the summary, which details the tokens, amounts, frequency, and total investment.
2.  If everything is correct, click **“Place Recurring Order”**.
3.  Your wallet will prompt you to approve the initial transaction to set up the plan. Once approved, Jupiter will handle all future transactions automatically.

## Managing Your Recurring Plans

### Viewing Active and Past Orders

On the **Recurring** dashboard, you can view all your plans.
*   The **“Active Orders”** tab lists all ongoing plans.
*   The **“Past Orders”** tab lists all completed or canceled plans, showing the total amount invested, average purchase price, and duration.

### Understanding Plan Details (Overview, Balance, Summary)

By expanding an active order, you can view detailed information:
*   **Balance Summary:** Shows the remaining balance of your input token and the total amount of the output token that has been successfully purchased and withdrawn.
*   **Order Summary:** Displays key details like total deposited, total spent, order size, current average price, purchase interval, number of orders left, and the next order's scheduled time.

### Closing and Withdrawing Funds

At the bottom of an active order's details, there is a **“Close and Withdraw”** button. Clicking this will cancel the recurring order, halt any future trades, and withdraw all remaining funds from the vault back to your wallet.

### Pro Tips for Managing Multiple Orders

1.  **Diversify:** Use **Recurring** to build positions in tokens with different risk profiles.
2.  **Monitor Funds:** Ensure your wallet has sufficient funds for all active plans.
3.  **Analyze Trends:** Use the dashboard insights to adjust your strategies.
4.  **Stay Organized:** Regularly check the "My Plans" section to stay on top of your investments.

## Recurring Order FAQs

### Can I Modify or Pause My Recurring Plan?

No. Once a **Recurring** plan is set up, it cannot be modified or paused. To make changes, you must cancel the existing plan and create a new one with your updated preferences.

### Do Recurring Plans Automatically Retry on Failure?

Yes. If a recurring purchase fails due to network issues, insufficient slippage, or other reasons, the program will automatically retry the order. This ensures minimal disruption to your strategy.

### Does Jupiter Recurring Charge Fees?

Yes, Jupiter **Recurring** charges a platform fee of **0.1%** on order completion.

### How is the Price Calculated for Each Recurring Purchase?

The price for each purchase is calculated based on the current market price at the moment the transaction is executed. Jupiter includes slippage protection to ensure you receive a minimum amount of tokens. If the price moves beyond the acceptable slippage threshold, the transaction will fail to protect your funds.

# IV. Advanced Trading: Trigger Orders (Limit Orders)

## Understanding How Trigger Orders Work

Jupiter **Trigger** orders execute your trade based on a price you set, matching it with available on-chain liquidity across Solana. It is not a traditional order book system.

### Order Creation and Keeper Mechanism

When you place a **Trigger** order, the tokens you are selling are transferred to keeper bots. These keepers continuously monitor token prices on-chain via the Jupiter Price API. When the market price reaches your specified trigger price, the keepers are programmed to execute the order. You can set up multiple trigger orders using the same token, provided you have a sufficient balance.

### Token Acquisition and Order Closure

When the market price hits your trigger price, your order becomes eligible for execution. The keeper bot will execute the order, and the purchased tokens are automatically transferred to your wallet. If the order is large and liquidity is insufficient, the keeper may execute it in smaller chunks to avoid significant price impact, ensuring partial fulfillment at the best possible price until the order is completely filled.

### Trigger Orders vs. Central Limit Order Book (CLOB)

Unlike a CLOB, which matches buy and sell orders in a central book, Jupiter **Trigger** orders execute based on aggregated on-chain liquidity from over 20 DEXs and AMMs. This means a **Trigger** order may not always get filled even if the target price is briefly touched (a "wick"), as there might not be sufficient available liquidity for the keeper to execute against at that moment.

### Using Slippage with Trigger Orders (Ultra Mode)

By default, **Trigger** orders aim for **zero** slippage, meaning keepers will only try to execute when the price is met precisely. This can lead to aggressive retries in volatile markets. To improve the success rate, especially for volatile tokens, you can use **Ultra Mode** for **Trigger** orders, which allows you to opt-in to a fixed slippage amount to increase the likelihood of execution.

## Creating Trigger Orders

### Setting Your Order Mode: Ultra vs. Exact

*   **Ultra Mode:** Increases the chance of execution during volatile periods by intelligently applying minimal slippage. This is great for volatile tokens or when you prioritize a successful fill over receiving the exact amount.
*   **Exact Mode:** Executes with 0% slippage. You get exactly the amount you set (minus fees).

### Defining Your Rate and Expiry

1.  Enter the amount of the token you are selling.
2.  Enter the rate at which you want to buy your selected token. This is the trigger price.
3.  Optionally, set an expiry for your order (e.g., 10 minutes, 1 day, or never). If the order is not executed by the expiry time, it will be automatically canceled.

### Reviewing and Placing Your Trigger Order

Before placing the order, double-check all details in the summary. Click **“Place Trigger Order”** and approve the transaction in your wallet.

## Managing Your Trigger Orders

### Viewing Open and Past Orders

On the **Trigger** dashboard, you can see your **“Open Orders”** (submitted but not yet executed) and **“Past Orders”** (completed or canceled).

### Understanding Order Details (Target Price, Total Filled)

For each open order, you can see:
*   The tokens being sold and bought.
*   **Target Price:** The price at which the order will execute.
*   **Total Filled:** The percentage of the order that has been executed. This is useful for large orders that may be filled partially over time.
*   **Expiry:** The time remaining until the order expires.

### Cancelling Individual or All Orders

You have the option to cancel a single order using the **“Cancel Order”** button or cancel all of your open orders at once with the **“Cancel All”** button. When an order is canceled, the funds are refunded to your wallet.

## Trigger Order FAQs

### Why Didn't My Trigger Order Work? (Keeper Updates, Liquidity)

Jupiter **Trigger** is not an order-book system. It relies on a keeper to monitor on-chain prices and available liquidity. During highly volatile periods, the price and liquidity can change between the moment the price is hit and when the keeper attempts execution. The challenge is less about the keeper's refresh speed and more about the availability of on-chain liquidity at the exact trigger price.

### Why is My Trigger Order Not Being Filled?

An order may not be filled for several reasons:
*   **Insufficient Liquidity:** If your order size is too large for the available on-chain liquidity, the keeper will attempt to fill it in smaller chunks.
*   **Price Wicks:** The price may have touched your target for only a very short period, and the liquidity at that price was taken up before your order could be executed.

### Failed Transactions and Fees in Wallet History

You may see failed transactions in your wallet history associated with a **Trigger** order. The transaction fees for these failed attempts, which are initiated and signed by the keeper bot, are **paid by the keeper**, not by you.

# V. Trading Safely on Jupiter

## Navigating Risks in DeFi Trading

DeFi trading can be complex due to the variety of tokens and their different risk profiles. Jupiter is committed to balancing convenience with user protection by providing safety notifications, warnings, and key token information.

## Safety Notifications and Warnings

### Price Impact and Rate Discrepancy Alerts

*   **Price Impact Alert:** This warning is triggered based on available liquidity and your trade size. A large trade in an illiquid pool can cause huge price impact. You can mitigate this by breaking your trade into smaller sizes, for instance with **Recurring** orders.
*   **Rate Discrepancy Alert:** Jupiter shows the quoted rate versus a market rate. A large difference can be due to factors like price impact, token taxes, or a stale market price. Trade with caution if you see a large discrepancy.

## Key Token Information

### Token Verification Status

Jupiter displays a token's verification status as either **"Verified ✅"** or **"Not Verified ⚠️"**. This is based on a new class of metrics that gauges how organic a token is by measuring activity among real people, including buying, holding, and selling volume. This is not a guarantee against scams but an indicator of organic interest.

### Understanding Token2022 Extensions (Permanent Delegate, Transfer Tax, Freeze Authority)

Jupiter displays information pills for Token2022 extensions, which can carry risks:

| Extension            | Definition                                                                                             | Valid Use                                                        | Misuse                                                                                          |
| -------------------- | ------------------------------------------------------------------------------------------------------ | ---------------------------------------------------------------- | ----------------------------------------------------------------------------------------------- |
| **Permanent Delegate** | Allows creators to grant unlimited delegation privileges, including burning or transferring tokens.   | Enables automatic payments, wallet recovery, or processing refunds. | Scam projects could drain tokens from user wallets.                                             |
| **Transfer Tax**     | Enables fees to be withheld on each transfer, redeemable by a withdraw authority.                      | Allows projects to generate revenue or collect royalties.        | Scam projects might arbitrarily increase taxes and withdraw funds.                              |
| **Freeze Authority** | Allows issuers to halt token transfers or trading, temporarily or permanently.                           | Used for regulated tokens (e.g., stablecoins) for compliance.    | Scammers may use this to prevent trading, a red flag for market manipulation or potential fraud. |

## MEV Protection (Maximal Extractable Value)

### How Jupiter Protects Your Orders (Randomized Timing, Price Checks, Auto-Cancel)

Jupiter has built-in protections to reduce the risk of MEV and front-running:
*   **MEV Protect Mode:** When enabled, this sends your transactions directly to Jito block engines, minimizing the risk of sandwich attacks.
*   **Randomized Timing:** For **Recurring** orders, execution has a randomized window (±30 seconds), making it difficult for bots to predict and exploit your trade.
*   **Price Check Before Execution:** Jupiter checks real-time prices before executing a swap.
*   **Auto-Cancel for Bad Prices:** If a trade would result in you receiving less than your minimum expected amount (based on your slippage setting), it automatically fails to protect you.

### Considerations for Jito Only Modes

If you are using **MEV Protect** or **Jito Only** modes, be aware that your transactions may at times fail or be slow to process, as not all Solana validators use the Jito block engine.

## Understanding the Swap Summary

The swap summary shows crucial information related to your trade, including the minimum amount you will receive, the transaction fees, and any price difference when compared to the market rate.

## ExactOut Functionality

**ExactOut** is a feature that allows you to specify the exact amount of tokens you wish to receive. Jupiter will calculate the required input amount. An alert is shown when using this feature because liquidity venues and routes are more limited than for standard (ExactIn) swaps, which may result in a worse price. It is best practice to compare the rates between **ExactOut** and ExactIn before you trade.

# VI. Jupiter PRO: Tools for Advanced Traders

## What is Jupiter Pro? (Benefits and Features)

**Jupiter Pro** is designed for advanced traders to discover, analyze, and trade tokens at high speed. It brings together advanced analytics, industry-leading MEV protection, and fees that are 10x lower than other platforms.

Key features include:
*   Adding tokens to a personal watchlist.
*   Using **Quick Buy** to instantly trade with a default currency (USDC/SOL).
*   Discovering tokens with filters and real-time analytics.
*   Utilizing metrics like Net Buy Volume, Net Buyers, and Smart Likes.
*   Protection from sandwich attacks via **Ultra Mode's** MEV protection.
*   Paying significantly lower trading fees.

## Using AlphaScan on Jupiter Pro

### Discovering New, Soon, and Bonded Tokens

The **AlphaScan** feed helps you discover and trade new token launches. It is a pro terminal that fetches launchpad token data beyond the typical 24-hour window. You'll see tokens categorized as:
1.  **New**: Newly created tokens with at least one liquidity pool.
2.  **Soon**: Tokens with bonding curve progress above 50%.
3.  **Bonded**: Tokens that have successfully migrated their liquidity to an AMM.

### Filtering and Quick Buy Options

You can filter the **AlphaScan** feed by launchpads, market cap, net buy volume, and more. You can trade any token directly from the feed using the **Quick Buy ⚡️** button.

## Checking Token Safety on Jupiter Pro

**Jupiter Pro** provides several indicators to help you gauge token safety. However, always conduct your own research.

### Developer Controls (Mint/Freeze Authority)

*   **Mint Authority:** Indicates if the developer can create new tokens.
*   **Freeze Authority:** Shows if the developer can freeze token accounts.

### Token Distribution Analysis

*   Total number of holders.
*   Bonding Curve Percentage.
*   **Dev Bought and Sold (DB/DS):** Indicated on the token chart.
*   **Top 10 Holders:** A checkmark ✅ appears if the top 10 holders own less than 15% of the supply.

### Community Metrics (Organic Score, CT Likes)

*   **🌱 Organic Score:** Measures the organic interest in a token.
*   **👍 CT Likes:** Brings community validation from X (formerly Twitter), allowing users to "like" tokens or flag potential honeypots or scams.

# VII. Common Issues and Troubleshooting

## Why Do My Swaps Keep Failing?

When buying a new token with high volatility, it can be difficult to land a trade. This is often due to rapid price movement causing the trade to fail because of built-in slippage protection. It should typically take no more than 3 tries to buy a highly volatile token. You will pay the gas fee for failed trades.

## Why Did I receive Fewer Tokens Than Expected?

Possible reasons include:
1.  **Slippage:** The price of the token moved between the time you submitted the swap and when it was executed.
2.  **Routing Optimization:** Your swap may have been split across multiple liquidity pools.
3.  **Liquidity Depth:** A token with low liquidity will experience significant price impact from a trade.

Always check the preview screen before confirming a swap to see the estimated final amount.

## Why is My Transaction Taking Longer Than Expected?

Several factors can delay transaction execution:
*   **Network Congestion:** The Solana network may be experiencing high activity.
*   **Low Liquidity:** If your trade needs to be routed through multiple pools, it may take longer.
*   **Slippage Settings Too Low:** If the price moves beyond your set slippage tolerance, the trade may get stuck or fail.

## Accidentally Bought the Wrong Tokens - What Can I Do?

Tokens on Solana are permissionless, meaning anyone can create a token. Always verify you are buying the right token by checking the contract address. While Jupiter attempts to filter out fake tokens and ranks search results by activity, the platform is likely unable to provide support or compensation for buying the wrong token.

## I Got Rekt, I Want a Refund! (Ultra Mode Policy)

If you believe you suffered a loss due to a platform issue while using **Ultra Mode**, you may be eligible for compensation under specific circumstances:
*   A bad quote caused by bad routing, especially when a better route was available.
*   More than 3 consecutive trades failed to execute, leading to a loss in gas fees.
*   Unreasonably high slippage (>20%) when unnecessary, leading to a sandwich attack.

**Please note:**
*   Compensation is only for **Ultra Mode** users.
*   Potential profit and loss (P&L) is never compensated.
*   Jupiter cannot guarantee zero MEV; **Ultra Mode** minimizes the risk but does not eliminate it.
*   Allow up to 3 days for refund requests to be processed.

## Will My Order Get Frontrun?

Jupiter has built-in protections to make front-running risky and unprofitable for bots. These include randomized execution timing for **Recurring** orders, real-time price checks before execution, and auto-cancellation if the price moves against you beyond your slippage tolerance. Using **MEV Protect** mode further reduces risk by sending transactions directly to Jito block engines.

## Exporting Transaction History for Tax or Other Purposes

You can export your **Recurring** and **Trigger** order history by using a block explorer like Solscan. Navigate to your wallet address on Solscan, go to the "Transfers" tab, and use the export CSV option to download your transaction data.

## Understanding Failed Transactions in Your Wallet History

Third-party wallets like Phantom and Solflare may not accurately display what is happening with Jupiter transactions. For a clearer view, use Jupiter's built-in history/activity page or a block explorer like Solscan for a deeper dive. For **Trigger** orders specifically, failed transactions signed by the keeper are paid for by the keeper, not you.

# VIII. Understanding Jupiter's Core Technology & Fees

## All About Jupiter's Aggregation Engine: Juno

### Juno: A Self-Learning, Self-Improving Aggregator

**Juno** is Jupiter's next-generation, self-learning aggregator, the most powerful routing system designed in DeFi. It builds upon previous versions and, for the first time, includes third-party aggregators in its routing mix. Using real-time analysis, **Juno** learns and improves with every transaction. When it detects inaccuracies or other issues that degrade the user experience, it learns and proactively prevents them from reoccurring.

### Components of Juno (Metis, Metis V1.5, JupiterZ, Third-Party Aggregators)

**Juno** combines several powerful components to find the best price:
1.  **Metis:** Jupiter's original v1 on-chain engine that powers swaps in the API and for partners like Phantom and Solflare.
2.  **Metis V1.5:** A new router that solves complex swaps with new hops and splits, improving the quoted-to-executed price by an average of 4.6x.
3.  **JupiterZ:** An RFQ (Request for Quote) product that works with market makers to tap into new liquidity sources.
4.  **Third-Party Aggregators:** Integration of external aggregators like Hashflow and Dflow.

### History of Jupiter's Routing Engine

Jupiter's routing engine has evolved with the Solana ecosystem. It automatically lists new tokens from major AMMs like Raydium, Meteora, and Pump.fun for 15 days. After that, markets are checked every 30 minutes for liquidity requirements to remain part of the router.

### Metis: The OG On-Chain Engine

**Metis** is Jupiter's original routing algorithm, a heavily modified variant of the Bellman-Ford algorithm. It was designed to execute trades quickly, discover the best routes for all pairs to reduce slippage, and offer high-quality price routing at scale. **Metis** streams input tokens to iteratively build routes that can split and merge at any stage, allowing it to find better prices for complex trades. On average, **Metis** quotes prices 5.22% better than the previous version (V2).

## What Do I Pay to Trade on Jupiter? (Fee Breakdown)

When you trade on Jupiter, there are several potential fees:
*   **Solana Network Fees:** The cost of interacting with the Solana network. This fee is paid to Solana validators regardless of whether your trade succeeds or fails. **Ultra Mode** dynamically adjusts this fee to maximize your chances of success.
*   **Jito Tip:** A fee paid to Jito if you are using MEV-protect or **Jito Only** mode for your transaction.
*   **Jupiter Commission:** A fee paid to Jupiter. This is typically charged on **Jupiter Perps**, when using **Ultra Mode** for swaps (0.1% on volatile pairs), or when **Recurring** and **Trigger** orders are filled. Manual Swaps are free of Jupiter commission.

## How Secure is Ultra Swap?

**Ultra Swap** is built on the robust Solana blockchain, and its smart contracts undergo regular audits and security checks. While no system is completely risk-free, Jupiter follows best practices to ensure secure transactions. Users should always be vigilant, look out for safety notifications and warnings provided in the UI, verify transaction details, and use secure wallets.
