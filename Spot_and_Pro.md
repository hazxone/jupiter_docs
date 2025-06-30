# Spot & Pro

## 1. Swap FAQs

### 1.1. What is Wrapped SOL (wSOL)?

Wrapped SOL is native SOL that is wrapped using the Solana Token Program, which allows it to be treated like any other Token program token type.

**Tip ℹ️**: You can use wSOL (wrapped SOL) to trade directly with any SPL token on Jupiter! This makes Jupiter even faster and more convenient for traders who trade frequently with SOL, since it avoids having to wrap/unwrap SOL - which is an additional instruction in the transaction.

#### Why the need to wrap SOL

Currently, Jupiter has to wrap and unwrap SOL when trading in SOL due to the fact that native SOL itself is not an SPL token.

*Read more about [wrapping SOL](https://spl.solana.com/token#wrapping-sol) or [transferring tokens](https://spl.solana.com/token#transferring-tokens).*

Because of Solana's transaction size limits, sometimes we need to break up the transaction into multiple ones when wrapping is involved. This can be a source of friction for power traders who transact a lot.

We expect this problem to be resolved with the upcoming transaction size increase in the next Solana upgrade. In the meantime, we have a new feature that allows users to trade directly in wSOL.

#### How to enable wSOL settings

You can do so in **Manual settings**, and turn on the **Use wSOL** setting. Thereafter, a wSOL tool bar will appear in your Swap Form, which helps you keep track of the amount of SOL vs wSOL you have in your wallet.

With wSOL settings turned on,  every transaction will use wSOL instead of SOL. If you have insufficient wSOL but have enough SOL, it will not work.

Do note that, the wSOL you receive will not be unwrapped into native SOL.

![The Swap Settings window, with the 'Use wSOL' toggle enabled.](https://support.jup.ag/hc/article_attachments/19209165117724)

![The Swap Settings window, with the 'Use wSOL' toggle enabled.](https://support.jup.ag/hc/article_attachments/19209210080028)

#### How to wrap/unwrap SOL

Clicking on the **Manage** button on the tool bar, a modal will pop up, allowing you to wrap SOL and unwrap wSOL.

![SThe SOL/wSOL management tool showing balances and wrapping options.](https://support.jup.ag/hc/article_attachments/19209210080284)

---

### 1.2. Manual Settings Overview

For users who want granular control, Manual Mode lets you customize key settings:

1. **Slippage Settings:**
   * **Dynamic Slippage:** Automatically adapts to the token and market conditions.
   * **Fixed Slippage:** Set a specific percentage threshold for tighter control.
2. **Transaction Broadcasting Options:**
   * **Priority Fee:** Use a typical RPC with a Priority Fee to speed up inclusion.
   * **Jito Only:** Use a Jito RPC with a Jito tip for MEV-protected transactions.
   * **Both:** Send your transaction via both methods for the best of both worlds.
   * **Important:** If processed through a typical RPC, only the Priority Fee is paid. If processed through Jito validators, both the Priority Fee and Jito tip are paid.
3. **Speed:** Adjust the urgency of your transaction with these options:
   * **Fast** (lower fees)
   * **Turbo** (moderate fees)
   * **Ultra** (higher fees for maximum speed)
4. **Fee Modes:** Choose how you want to manage fees:
   * **Max Cap:** A dynamically estimated maximum fee based on your trade size.
   * **Exact Fee:** Specify an exact fee amount for your transaction.

---

### 1.3. How do I verify a token?

Jupiter offers a brand new way to verify how organic a token is. You'll find this score next to the token when you choose a token.

![JUP token listing showing its 100 organic score for verification.](https://support.jup.ag/hc/article_attachments/18599579477916)

This is a new class of metrics, so if a token is legit it should have a good amount of activity among real people, from buying volume, holding volume, selling volume, etc.

If you have feedback about a certain token, reach out to us by submitting a ticket.

Note, this is not a metric to know whether this token could be cause of a potential scam.

---

### 1.4. Why is my transaction taking longer than expected?

There are several factors that can delay execution:

* **Network congestion:** Solana may be experiencing high activity.
* **Low liquidity:** If your trade needs to route through multiple pools, it may take longer.
* **Slippage settings too low:** If price movement exceeds your slippage tolerance, the trade may get stuck or revert.

If your transaction hasn't been processed in a long time, please reach out to the team.

---

### 1.5. Why did I receive fewer tokens than expected?

Possible reasons include:

1. **Slippage:** The token price moved before execution.
2. **Routing optimization:** The swap was split across multiple pools.
3. **Liquidity depth:** If a token has **low liquidity**, the trade impact can be significant.

Always check the **preview screen** before confirming a swap to see the estimated **final amount**.

If the difference is significant, please let the team know by submitting a ticket.

---

### 1.6. How secure is Ultra Swap?

Ultra Swap is built on the robust Solana blockchain, and its smart contracts undergo regular audits and security checks. While no system is completely risk-free, Jupiter follows best practices to ensure secure transactions.

Look out for safety notifications, non-intrusive warnings, key token information and the swap summary to reduce confusion and potential mistakes.

As always, make sure to verify transaction details and use secure wallets when engaging in any crypto trade.

---

### 1.7. Why do my swaps keep failing?

If you are trying to buy a new token with a lot of volatility, it might be tough to land your trades. Usually, it should take no more than 3 tries to buy a super volatile/hot/hyped token.

This is due to the volatility of the price moving, leading to your trades to fail due to the built in slippage protection (so you dont accidentally buy tokens at insane prices)

If you struggle for more than 3 times, please let our team know.

Note: You will pay the gas for the trades that failed. We are happy to compensate you if you tried more than 3 times and lost gas in the process.

If you think you got rekt, read more [here.](https://support.jup.ag/hc/en-us/articles/18474701911708-I-got-rekt-I-want-a-refund)

---

### 1.8. I got rekt, I want a refund!

Swapping on Solana is a complex problem, and we are actively driving this forward with Ultra Mode.

If you swap on Ultra Mode, you enjoy world-class support, real-time slippage estimation, and intelligent heuristics to handle all the swap details for you.

If you got rekt while using Ultra Mode, for the following reasons, you might be compensated for your loss:

- In case of **a bad quote that's been caused due to bad routing**; especially when there's a better route or market at time of swap.
- More than 3 consecutive trades that failed to execute, leading to **a loss in gas**.
- Unreasonably high slippage (>20%) when unnecessary, leading to sandwiching attacks.

Take note:

- We never compensate for potential P&L, and we only compensate ultra-mode users.
- We cannot guarantee zero MEV. Ultra mode minimizes the risk of getting sandwiched, does not prevent it.
- Allow up to 3 days for refund requests to be processed. Open a [support ticket and contact our team](https://support.jup.ag/hc/en-us/requests/new).

---

### 1.9. Accidentally bought the wrong tokens, what can I do?

Tokens on Jupiter are 100% permissionless on Solana, which means anyone can create a token. Always verify that you are buying the right token by checking the contract address.

Jupiter constantly tries to improve by filtering out the right tokens for you:

- When searching, we rank and display tokens based on activity
- We have a verification system for community-verified/approved tokens and not approved
- When buying tokens, we attempt to filter out fake tokens given to you

We are likely unable to support or compensate for buying the wrong tokens, but please continue to share with us how it happened so we can continue to improve.

This applies to all users on all platforms, such as Jupiter Mobile, Jup.ag, and more.

---

### 1.10. How do I modify my settings?

Looking to control your slippage, gas fee, etc?

Switch to Manual Mode with one click on Jup.ag. Manual Mode allows you to manually control each individual setting, and also does not charge any additional fees.

- Slippage: set the maximum deviation you are willing to accept when trading
- Priority Fees: set the gas fees you are willing to pay
- Transaction Broadcasting: set the channels your trade will go through to reach the Solana blockchain
- AMMs Exclusion: turn on and off certain AMMs when interacting with Jupiter.

If you are using Jupiter Mobile, go to Jup.ag in the web browser to access Manual mode.

Note: Ultra Mode is designed to enhance your trading experience by improving success rates automatically.

---

### 1.11. Why is this token not tradable on Jupiter?

We automatically list all new tokens on Solana (if you use the large AMMs like Raydium, Meteora, Pump.fun, etc.) for **25** days. After 25 days, we implement liquidity requirements to ensure your market/pool continues to be routed on Jupiter. The markets/pools are checked every **30** minutes.

Your market/pool must fit **one of the following criteria**:

#### 1. Less than 30% price difference on $500

Using a benchmark position size of **$500**, a user should encounter less than **30%** price difference after buying $500 worth and then selling back on the same market/pool.

```
Price Difference = ($500 - Final USD value) / $500
```

If the price difference is more than **30%**, it means that there is insufficient liquidity in the pool for the benchmark position size of **$500**.

#### 2. Less than 20% price impact on market/pool

If the above (sell back $500 worth) fails, we will compare the price per token received from buying $1000 worth vs the price per token received from buying $500 worth to calculate **price impact**.

If the price impact is more than 20%, it means that the market/pool is illiquid.

---

### 1.12. What do I pay to trade on Jupiter?

When trading on Jupiter, you pay a fee to the Solana Blockchain, and sometimes a fee to Jupiter.

Trading on Jupiter:

- Solana Fees: The cost of interacting with the Solana network. Jupiter dynamically decides the fee for you in Ultra mode.
- Jito Tip: A fee paid to Jito for interacting with Solana, if you are using MEV-protect or Jito only mode.
- A commission to Jupiter, usually on Jupiter Perps, Jupiter Ultra or when Recurring and Trigger Orders are filled. Manual Swaps are free.

Note that Solana Fees are paid to Solana, regardless of your trade going through. Jupiter dynamically adjusts your fees to maximize your chances of landing your trade on Ultra Mode.

---

## 2. Instant

### 2.1. Swapping on Jupiter (with Gasless Option!)

Trading tokens on Jupiter is as easy as it gets! Whether you're dipping your toes into Solana for the first time or optimising your trades, we’ve got you covered. A seamless swap experience starts from wallet connection to best priced trade landed.

#### Step 1: Connect Wallet via Quick or Classic Mode

* **Quick Account:** Embedded wallet for instant signless transactions
  1. Quickest way to swap without requiring wallet signature
  2. Create your embedded wallet via social and web3 logins
  3. Your wallet, your keys. Non-custodial, secured by Privy
* **Classic Mode**: Connect your favourite ecosystem wallets like Jupiter Mobile, Phantom, Solfare

#### Step 2: Swap using Ultra or Manual Mode

* **Ultra Mode:** It just works. All technical configurations optimised and abstracted for you.
  1. Real-Time Slippage Estimation (RTSE)
  2. Dynamic priority fees and optimized transaction landing
  3. Ultra Mode swap fees range from 0-0.1% depending on pair volatility. Select stable swaps are free.
* **Manual Mode**: Pick your settings and pay 0% fee to Jupiter

You’ll see some handy details, like:

* **Price Impact:** Price Impact is influenced by the available liquidity to settle the trade. The larger the trade the larger the price impact on the selected assets.
* **Route Details:** Jupiter's GPS for your tokens. By clicking the "1 Market" or "2 Routes + 2 Markets", you can discover the routing path for the quote
* **Network Fee**: Amount to send the transaction to the network. There's also Jito Tips: cost of interacting with Solana via Jito if you're in Manual mode.

#### Troubleshooting? We’ve Got Your Back!

* **Transaction Failed?** Check your wallet balance or try increasing slippage tolerance
* **Missing Token?** Make sure it’s supported and spelled correctly
* **Stuck Trade?** Reconnect your wallet or clear your browser cache and try again

---

### 2.2. How to Trade Safely on Jupiter

Trading in DeFi can get complex with tokens of various risk profiles and functionalities, leading to an overload of information. Jupiter is committed to balance convenience and protection for you.

We highlight safety notifications, non-intrusive warnings, key token info, and swap summary to reduce info asymmetry yet not overload you.

#### Warnings

![A warning message indicating an excessively large trade size and its associated price impact.](https://station.jup.ag/assets/images/warnings-456887f4d6fe09ad576f23f3b9a5a667.png)

Price impact alert is influenced by the available liquidity and your trade size. A large trade size in an illiquid pool often results in huge price impact, hence you can break up your trade size with [Recurring](https://jup.ag/recurring).

We also show the quoted rate (from Jupiter) against the market rate. The price difference can be due to various external factors such as price impact, token tax, stale market price (usually derived using last traded price), etc.

If your trade shows a large price impact and difference, please trade with caution and feel free to seek clarity in our [Discord](https://discord.gg/jup).

#### Token Information

![Token information UI showing which token verified and not verified](https://station.jup.ag/assets/images/token-info-11e64562f3aaca351d9d4e1194a21a1f.png)

Jupiter shows relevant token information to reduce information asymmetry you may face when trading. Token Verification shows as "Verified ✅" or "Not Verified ⚠️" and Token2022 extensions appears as information pills.

More on Token Verification criteria [here](https://support.jup.ag/hc/en-us/articles/18599586767644-How-do-I-verify-a-token).

More on Token2022 extensions below:

|  | **Definition** | **Valid Use** | **Misuse** |
| --- | --- | --- | --- |
| **Permanent Delegate** | Allows creators to grant unlimited delegation privileges over any account for that mint, including burning or transferring any tokens from any account. | Enables automatic payments, wallet recovery, and processing refunds. | Scam projects could drain tokens from users' wallets. |
| **Transfer Tax** | Enables fees to be withheld on each transfer, redeemable by those with withdraw authority. | Allows projects to generate revenue through service charges, or to collect royalties or taxes on transfers. | Scam projects might arbitrarily increase transaction taxes and withdraw funds with full authority. |
| **Freeze Authority** | Allows issuers to halt token transfers or trading, temporarily or permanently. | Commonly used for regulated tokens (e.g., stablecoins) to meet legal standards; issuers can freeze tokens for compliance with legal or regulatory concerns. | Scammers may use this to prevent trading or transferring scam tokens, a red flag for market manipulation or potential fraud. |

#### MEV Protect

![Swap settings UI showing MEV protect toggle enable/disable](https://station.jup.ag/assets/images/mev-protect-d45241e28742279ad05128813f61e4e8.png)

Jupiter introduces [MEV Protect](https://www.jupresear.ch/t/continuing-to-deliver-on-jupiters-best-ux-promise/22230) mode, which will only send your transactions directly to Jito block engines, minimising the risk of sandwiches for you.

In a sandwich attack, a bot spots your transaction, places a buy order before yours to push the price up, and places a sell order right after, pocketing the difference and leaving you with a higher cost per token. Turning on MEV Protect will hide your swaps and thus reducing the chances of MEV or sandwich attacks.

```
If you are using MEV Protect or Jito only modes:   
Do note that your transactions may at times fail or be slow to process as not all validators   
are using Jito block engine.
```

#### Swap Summary

![swap-summary](https://station.jup.ag/assets/images/swap-summary-857e8fb0561902c1fbd94ea75d125136.png)

The summary shows you the information relating to the trade, such as the minimum received, the transaction fees, and price difference when compared to the market rate.

#### ExactOut

![exactout warning message](https://station.jup.ag/assets/images/exactout-bd1481da45f2ae0997fb283713bb3df8.png)

ExactOut gets the exact amount of tokens that you need. We share this alert because liquidity venues and routes are lesser than ExactIn. Transparently, you should know that you might get a worse price. Best practice is to compare the rates between ExactOut and ExactIn before you trade.

---

### 2.3. All about Jupiter's Aggregation Engine: Juno

Our flagship aggregation router, Metis, powers Jupiter Swap and enables billions of dollars of trades each month. A high-level overview of how Jupiter Swap works:

1. Our indexer picks up whenever a new market and token is created on the AMMs that we support
2. Regardless of the liquidity requirements, these markets are tradable for 14 days
3. After 14 days, the market will be checked for liquidity requirements and subsequently checked every 30 minutes. If they pass the requirements, they will continue to be part of the router. If they don't, the market will be dropped
4. Our routing engine will perform math and the algorithms to ensure you achieve high quality quoted-to-executed price

Today, we've reached v4. Introducing **Juno**, our next-gen aggregator.

#### Juno: A Self-Learning, Self-Improving Aggregator

Juno is the most powerful routing system ever designed in DeFi. It is a new approach that builds and improves upon Metis, JupiterZ (our RFQ product), and for the first time also includes third-party aggregators in the mix.

1. **Metis**: Our v1 engine that powers swaps in API or binary partners like Phantom and Solflare
2. **Metis V1.5**: A new router that solves complex swaps with new hops and splits. This algorithm improves quoted-to-executed price by **4.6x** on average
3. **JupiterZ**: We work with market makers to tap onto new liquidity avenues to provide quotes
4. Inclusion of **third-party aggregators** like Hashflow and Dflow

Using real-time analysis, Juno learns and improves itself with every transaction.

When Juno detects inaccuracies in quoted-to-executed prices or other problems that degrade the user experience, it learns, and proactively prevents these from occurring in the future. Similarly, when quote providers provide high-quality experiences, Juno learns.

#### History of Jupiter's Routing Engine

We have iterated Jupiter's Routing Engine each time as Solana grew, from finding the best price across AMMs with the least friction, to accommodating the explosion of Pumpfun tokens.

##### Metis: The OG On-Chain Engine

[Metis](https://www.jupresear.ch/t/archived-jupiter-v3-the-metis-routing-algo/21712) is one of the big parts of how Jupiter Swap works, it is a heavily modified variant of the [Bellman-Ford algorithm](https://en.wikipedia.org/wiki/Bellman%E2%80%93Ford_algorithm) catered for the key requirements:

1. Trades being executed quickly and efficiently even under high demand
2. Better route discovery for all trading pairs, reducing slippage for large trades
3. Offering high quality price routing at scale in a very dynamic operational space

To find a quality price, Metis streams the input tokens to incrementally build a route to split and merge at any stage. By generating the routes for each split iteratively one after another, we can also use the same DEX in different splits - allowing us to find routes with better prices with more complex trades.

To improve the efficiency of the algo, we combine route generation and quoting into a single step, allowing us to avoid generating and using bad routes, which besides improving the efficiency, also allows us to use a larger set of tokens as intermediaries.

Metis (v3) is also equipped to scale to include more DEXs in a route when account lock limits (64) are increased, and can support more DEXs with only a modest increase in running time. Being able to refresh quotes in parallel and in real-time, Metis on average quotes prices 5.22% better than V2.

---

## 3. Recurring

### 3.1. How Recurring works (in depth)?

Creating a Recurring order is like setting up a reliable machine that processes your trades in the intervals you set them at, here’s how it works in a nutshell, let’s break it down into:

* How Recurring orders are created
* How their order mechanism works
* How tokens are transferred after each order
* How the Recurring order is (auto) closed

#### How Recurring orders are created

1. When you start a order, the tokens you’re using—say $USDC—will be transferred from your wallet into a special vault, otherwise known as a program-owned associated token account.
2. Once you create your first position, your first order is processed right away! For example, if you're setting up an order where; $USDC into $SOL, the full $USDC amount you chose will be stored in your vault, and the first trade (say 100 $USDC for $SOL) will happen immediately.
3. The remaining trades follow at regular intervals, based on the schedule you pick. For instance, if you’re splitting $1,000 USDC over 10 days, you’ll see the first $100 -> $SOL trade right away, and then 9 more trades of $100 each happening daily like clockwork.

⚠️ **Heads-Up! Keep It Up with Randomness**

To keep your Recurring strategy less predictable, each trade will execute within a randomized window of ±30 seconds from your chosen time. Think of it as a little sprinkle of unpredictability to keep things fresh and secure.

#### How their order mechanism works

When you create a Recurring Order, your big order is split into smaller, bite-sized trades. The number of trades depends on the options you pick. Let’s break it down:

Say you’re recurring $900 USDC into $SOL over 3 days. Your order will be split into 3 trades of $300 USDC each, one for every day. Let’s see this in action:

**Day 1:** Once your order is confirmed, the first $300 USDC -> $SOL trade happens immediately.

**Day 2:** About 24 hours later, the second trade of $300 USDC -> $SOL gets executed.

**Day 3:** Another 24 hours after that, the third and final trade wraps up your plan.

The amount of $SOL you receive depends on its price at the time of each trade. Following the same example:

**Day 1:** If $SOL costs $20, your $300 buys you 15 SOL.

**Day 2:** If $SOL jumps to $25, your $300 gets you 12 SOL.

**Day 3:** If $SOL dips to $15, your $300 scores you 20 SOL.

Note: Jupiter Recurring charges a tiny 0.1% fee for each trade. So, let’s adjust the math:

**Day 1:** For 15 SOL, after the fee, you’ll receive **14.985 SOL**.

**Day 2:** For 12 SOL, after the fee, you’ll get **11.988 SOL**.

**Day 3:** For 20 SOL, after the fee, you’ll end up with **19.98 SOL**.

#### How tokens are transferred after each order

Every time an order executes, the purchased tokens magically show up in your wallet within the same transaction. No extra steps required! Let’s break it down:

For example, if you’re recurring $900 USDC into $SOL over 3 days:

**Day 1:** Your first $300 USDC order executes, and voila! You receive **9.97 SOL** (net of fees) in your wallet if the price is $30 USDC per SOL.

**Day 2:** Your second $300 USDC order processes, delivering **11.988 SOL** (net of fees) if the price is $25 USDC per SOL.

**Day 3:** Your final $300 USDC order wraps things up with another **11.988 SOL** (net of fees) at the same $25 USDC per SOL price.

⚠️ **Caveat to Auto-Withdrawal: Keep Your ATAs Open**

If your purchased token isn’t SOL, automatic transfers only work if you have the correct **Associated Token Account (ATA)** set up. But don’t worry—Jupiter Recurring creates the necessary ATA when you set up your account.

**Heads-Up:** If you manually close your purchased token’s ATA (via your wallet or a 3rd-party tool), auto-transfers won’t work. Instead, tokens from future trades will stay in your Recurring vault and only transfer as a lump sum at the end of your Recurring cycle.

Example of Closing ATA:

Let’s say you have an order for $300 USDC daily into $BONK over 3 days:

**Day 1:** Everything goes smoothly; you receive your BONK in your wallet.

**Day 2:** Uh-oh! You’ve closed your BONK ATA. This day’s $100 USDC worth of BONK stays in your Recurring vault instead of transferring to your wallet.

**Day 3:** On the last trade, Jupiter will transfer all your BONK—including what’s left from Day 2—directly to your wallet. Read more about Auto Close below!

💡 **Pro Tip:** If you don’t want to wait, you can manually withdraw your tokens from the Recurring vault anytime through our user-friendly UI.

#### How the Recurring order is (auto) closed

At the end of your Recurring cycle, Jupiter takes care of everything for you. Here’s how it works:

* If your wallet’s Associated Token Account (ATA) stays open, your purchased tokens are transferred to your wallet with each order. (No one else can ever receive your tokens)
* If you’ve closed your token account mid-cycle (as mentioned above), don’t worry. The program recovers the rent from your order account’s in-token PDA and uses it to re-open your token account, so your tokens can still reach you safely.

**How Rent Works:**

By default, **2/3 of the rent** from your Recurring account is sent back to you. The remaining **1/3 of the rent** isn’t taken by Jupiter or anyone else—it’s recoverable by you if you decide to close your ATA that holds your tokens later.

🚨 **Danger** Do NOT close your ATA before withdrawing or swapping your tokens! If you do, it could result in token loss. This isn’t just a Jupiter thing—it’s how Solana wallets and accounts operate.

---

### 3.2. How to manage multiple Recurring plans?

Jupiter makes it easy to manage multiple Recurring plans seamlessly, whether you’re diversifying your portfolio or optimizing investments for different tokens. This guide will walk you through the process step-by-step.

#### Head to “Active Orders” on Dashboard

Right on the dashboard you can view all your active orders.

1. Navigate to the “Active Orders” section.
2. Here, you’ll see a list of all your active and past orders.

#### View Plan Details

Each plan in the “Active Orders” section will display key details to help you track progress.

1. **Active Orders:** This tab lists out all of your active orders.
2. **Individual Orders:** Ongoing order with a percentage to indicate how much of the order has been executed.
3. **Order Overview:** To see the order details, expand one of your ongoing orders and review the Overview Details.
4. **Balance Summary:** This shows the order balance progress. First is the remaining balance of the token you are spending/ allocating. Second is the amount of tokens that have been successfully purchased. Third is the amount of tokens purchased that have been withdrawn.
5. **Order Summary:** This will show the current ongoing order, with information like:
   * Total Deposited - The input amount and token that you are selling or swapping from.
   * Total Spent - The progress of the order, or the amount spent to swap from.
   * Each Order Size - The average order size for the order.
   * Current Average Price - The average price for the transactions being completed over the order.
   * Buying - The output token you are purchasing with the order.
   * Interval - The time interval specified in the Frequency fields.
   * '#' of Orders Left - The number of orders remaining with this request.
   * Next Order - The date and time of the next order to be executed.
   * Created - The date and time when the order was submitted.
6. **Close and Withdraw:** Use this to cancel and close the order. This will halt the progress on the Recurring order and withdraw all funds in the order to your wallet.

#### Close and Withdraw

Use the button at the bottom of your active order to cancel and close the order. This will halt the progress on the order and withdraw all funds in the order to your wallet.

#### View Past Orders

This tab lists all the orders that have been completed or canceled. Each past plan will display:

* The total amount invested.
* The average purchase price.
* Duration of the plan.
* Status (completed or canceled).
* Click on a specific past plan from the drop down to view detailed historical data.

Once you select a Past Orders, you can view all the transactions for your order.

#### Pro Tips for Managing Multiple Orders

1. **Diversify Strategically:**
   * Use Recurring to build positions in tokens with different risk profiles. For example, combine stablecoins with volatile tokens.
2. **Monitor Fund Availability:**
   * Ensure your wallet has sufficient funds to cover all active plans to avoid failed transactions.
3. **Analyze Market Trends:**
   * Use data insights from Jupiter’s dashboard to adjust strategies. Pause or cancel plans when market conditions shift significantly.
4. **Keep It Organized:**
   * Regularly check the "My Plans" section to stay on top of your investments.

---

### 3.3. How to set up your first Recurring order (DCA) on Jupiter?

#### Step 1: Connect Your Wallet

Before anything else, you’ll need to connect your wallet to Jupiter.

1. Visit the **Jupiter [Recurring](https://jup.ag/recurring) page**.
2. Click on the **“Connect Wallet”** button at the top right or through multiple “Connect Wallet”s on your dashboard: Select your preferred wallet from the list (e.g., Phantom, Solflare, or any other supported wallet).
3. Approve the connection request in your wallet.

#### Step 2: Choose Your Tokens

Now it’s time to decide which tokens you want to invest in.

1. On the Recurring page, upon clicking on the token you want to allocate, a list will open up.
2. Choose your preferred token.
3. If needed, select the token you want to buy in a similar way as well.

**Pro Tip:** Choose tokens that align with your investment goals. If you’re unsure, start with popular ones like SOL or WBTC.

#### Step 3: Set Your Recurring Plan Details

This is where the magic happens!

1. **Enter the amount** you want to invest per transaction (e.g., $10 worth of SOL).
2. Choose the **frequency** of your purchases; minute, hour, day, week or month.
3. Enter the number of orders you want your Recurring plan to be processed in.
4. [Optional] Enter the min. and max. price range under which only the order would be executed.

#### Step 4: Review the Summary and Start

Before confirming, double-check the details:

* The token you’re purchasing.
* The source token and the amount being used.
* The frequency and total investment amount.

Have a look at the Summary for a thorough overview:

If everything looks good, hit “Place Recurring Order".

#### Step 5: Approve the Transaction in Your Wallet

Your wallet will prompt you to approve the first transaction to set up the plan.

1. Check the transaction details in your wallet.
2. Approve the transaction.

Once approved, Jupiter will handle all future transactions for you—automatically.

---

## 4. Trigger

### 4.1. Creating Trigger Orders (Limit Order)

Setting up a Trigger Order plan on Jupiter is quick, simple, and designed to help you efficiently set up orders for your desired price levels to execute your strategy.

Set it, forget it, crush it. Set the price you want, forget about monitoring it, and let Jupiter execute that trade for you asap.

#### Step 1: Connect Your Wallet

Before anything else, you’ll need to connect your wallet to Jupiter.

1. Visit the [Jupiter Trigger page](https://jup.ag/trigger).
2. Double-check that the URL is correct at <https://jup.ag/trigger> and "Trigger” Tab is selected.
3. Click on the **“Connect Wallet”** button at the top right or use one of the other "Connect Wallet" buttons on the dashboard: Select your preferred wallet from the list (e.g., Phantom, Solflare, or any other supported wallet).
4. Approve the connection request in your wallet.

![Jupiter trigger page showing buttons position in the UI to connect wallet](https://jupiter-space-station-pcz9x10rq.preview.jup.ag/assets/images/limit-order-connect-7f65e21d4588cb4d049becd5b5d744f5.png)

#### Step 2: Set Trigger Order using Ultra or Exact Mode

![UI showing where the toggle of trigger order mode (Ultra or Exact) is located](https://support.jup.ag/hc/article_attachments/20115282836508)

* **Ultra Mode:** Your chances to get the order executed increases during volatile periods by intelligently applying some minimal slippage. Less output amount up to the slippage threshold may be received for the better transaction success rate.
  + Great for trading tokens that are volatile
  + Great for volatile periods
  + Great for folks who prioritise success rate > exact amount
* **Exact Mode**: 0% slippage, you get exactly what you set - sans the fees.

#### Step 3: Set your Rate using Ultra or Exact Mode

1. Enter the amount of the token you are selling
2. Enter the rate at which you want to buy your selected token. This is the price at which the order will get executed at. Jupiter will calculate how many tokens you will get based on the rate entered and show you in the “You’re Buying” selector

Optional parameters:

* Choose the Expiry of your order. Select the maximum period for which your order remains active, It can be 10 minutes or never The expiry option ensures that if the order is not executed in your specific time, it auto-cancels.

#### Step 4: Review and place your Trigger

Before placing the order, double-check the details:

1. Review the entered amounts
2. Place order!

Have a look at the Trigger Order Summary thoroughly before clicking the “Place Trigger Order” button. A success notification toast will appear in the lower left corner that will notify you once the order has been made.

---

### 4.2. Learn how Trigger Order works

Jupiter Trigger executes your order based on the price you set by matching it with onchain liquidity across Solana. The system utilizes a keeper to monitor token prices on-chain and trigger the fulfilment of orders if liquidity is available. It is not an order book system.

#### How your orders are created

When you place a trigger order, the tokens you are selling (e.g. USDC), will be transferred to keepers bots to execute at the price you specified.

At this point your tokens are locked but the order is not yet complete.

We also supports multiple trigger orders using the same token, as long as there are sufficient token balance and fees for transactions.

#### How your tokens are bought

The keepers will continuously scan the network for liquidity across the Solana ecosystem. They are programmed to execute the trigger order when the market price reaches your specified trigger price.

The difference between Jupiter Trigger with CLOB (orderbook) is that Jupiter Trigger is designed to prioritize UX, simplicity, flexibility, and access to the deepest liquidity across Solana. Jupiter aggregates liquidity from >20 DEXs and AMMs, ensuring the deepest liquidity gets tap on.

When the market price reaches your trigger, your order becomes **eligible for execution**. Keepers attempt to fill it immediately. However, if the price briefly touches your trigger but lacks sufficient liquidity, the order may not execute fully. In such cases, you might notice failed transaction attempts in your wallet as the keepers repeatedly try to fill your order.

#### How the Trigger Order is closed

Orders are executed by the keeper bot. When the token price hits your set trigger price (including fees), the keeper will execute the order. After the order is executed, you will receive the tokens you quoted, minus platform fees charged by Jupiter.

For example, you place a trigger order to buy 1 SOL for 100 USDC at a rate of 100 USDC/SOL:

* The keeper bot monitors the price on-chain via the Jupiter Price API.
* If the price of SOL drops to 100 USDC, the keeper executes your order.
* If the order size is large and liquidity is insufficient, the keeper executes the order in smaller chunks to avoid significant price impact. It aims for partial fulfilment to ensure the best price with minimal price impact, continuing until the order is completely filled.
* Whether fully or partially fulfilled, the acquired tokens are automatically transferred to your wallet.

#### How orders with slippage work?

Think of Trigger Orders as Jupiter Swaps at a specific price, with a condition of being executed with **zero** slippage. Keepers will only attempt to execute your order when the predetermined price is met precisely. However, market conditions/price are always changing, hence, when orders are attempted to execute with zero slippage, it will have to retry aggressively to successfully execute.

In some cases, you might be trading with highly volatile tokens that is critical to trade them immediately based on the price trigger, and you are willing to take the tradeoff of absorbing some slippage in order to increase the success rate of your order.

Hence, **Ultra Mode** for Trigger Order is introduced for you to opt in to a fixed slippage amount to improve execution rate.

#### How Jupiter Differs from CLOB (Central Limit Order Book)

Central Limit Order Book (CLOB) is a mechanism used by TradFi to facilitate trading between buyers and sellers. It acts as a hub where both buyers and sellers can submit their buy/sell orders, which are matched based on specific rules and executed accordingly.

This mechanism requires a certain level of market-making for it to operate efficiently. For a particular market or token to be available for trading on the CLOB, it requires a market maker to be present and accessible.

In contrast, Jupiter Trigger executes orders based on on-chain liquidity aggregated by Jupiter. This liquidity consists of >20 DEXs and AMMs with Solana-wide coverage of SPL tokens.

Trigger on Jupiter may not always get filled even when the target price is reached due to the liquidity-based nature of the system and not using an order book. This can result in keeper bots not being able to trigger the order during quick price wicks.

---

### 4.3. How to manage Trigger orders?

#### View Active Trigger Orders

Right on the dashboard you can view all your active Trigger Orders

1. Navigate to the “Open Orders” section.
2. Here you will see all your active Trigger Orders

This section shows the orders which are submitted to the system but they are yet to be executed as the target price is not yet reached.

1. Shows the amount of tokens you are selling and the amount of tokens you are buying. (In this example, you are selling 10 USDC to get 0.047619048 SOL)
2. This shows the target price of your selected token for the order to execute. Once the token reaches this price your order will execute (if liquidity is available)
3. Total Filled is helpful when you place a large order and there is insufficient liquidity. So Jupiter tries to execute the order partially whenever liquidity is available. If the order is fully executed, Jupiter will send the tokens directly to the user's wallet. There is no claiming process required once a Trigger Order is entirely filled.
4. Expiry shows the selected period for which your order remains active. In this example it's Never, therefore the order will remain active on-chain until the price hits the specified amount. Once it reaches the specified amount the order will be executed.
5. Cancel Order: It closes this order/position
6. Expanded View: This gives you the expanded view of your open order with the additional details
7. Cancel All: It closes all your open orders/positions.

#### View Trigger Order History

Orders that are Completed or cancelled are shown here.

1. Shows the amount of tokens sold and the amount of tokens you have bought. (In this example, you sold 10 USDC to get 0.047619048 SOL)
2. This shows the target price of your selected token at which the order was executed.
3. Status: Indicates the order state - Completed or Cancelled. (For cancelled orders the users' funds are refunded back to their wallet.)
4. Expanded View: This gives you the expanded view of your order with the additional details
5. Total Filled shows how much of the total order size has been fulfilled.
6. Expiry shows the selected time period for which your order remains active. In this example it's Never, therefore the order remained active on-chain until the price hit the specified amount are your order was executed.
7. Transactions: This shows the creation of the order, where the user specified the trigger order details and deposits the funds for the order to be created. The actual trade transaction is executed when the on-chain price hits the specified price submitted by the user. These trade actions can occur several times for each created order, depending on the available liquidity in the market

---

## 5. Trigger & Recurring FAQs

### 5.1. Will my order get frontrun?

Jupiter has built-in protections to reduce the risk of MEV (Maximal Extractable Value) frontrunning, so your swaps are as fair as possible. Here’s how we keep your trades safe:

#### How We Protect Your Orders:

- Randomized Timing – Your order isn’t placed at an exact second; instead, there’s a 2 to 30-second randomness in execution. This makes it hard for bots to predict and exploit your trade.
- Price Check Before Execution – Before your swap goes through, Jupiter checks real-time prices to make sure you’re getting a fair deal. If the amount you’d receive is too low, the transaction won’t happen.
- Auto-Cancel for Bad Prices – If your trade would result in less than your minimum expected amount, it automatically fails—so you never get a worse deal than expected.
- Dollar-Cost Averaging Helps Too – If you’re using Recurring, your order is split into smaller swaps over time. This naturally makes it harder for bots to profit from price manipulation.

We can’t fully stop MEV (no one can), but we make front-running risky and unprofitable for bots, ensuring you get fairer, more predictable trades.

---

### 5.2. Does Jupiter Recurring charge any fees?

Yes, charge a platform fees of **0.1%** on order completion.

---

### 5.3. Why didn't my trigger order work? How fast does the “keeper” update the token price data?

Jupiter Trigger Order is not an order-book system, where orders from buyers and sellers are submitted then matched and executed accordingly.

Jupiter utilized a keeper to monitor the token prices on-chain and trigger the fullfilment of orders, then execute your order based on your set price by matching with the available on-chain liquidity across Solana.

During highly volatile periods, the price (and liquidity) of the asset may change after order trigger and before order execution. The challenge is not about the keepers refresh, but more about available liquidity once your price is hit. You can find more information [here](https://support.jup.ag/hc/en-us/articles/18734458673180-Learn-how-Trigger-Order-works).

Feel free to reach out to us by [creating a support ticket](https://support.jup.ag/hc/en-us/requests/new), if you have any other concerns.

---

### 5.4. Can I pause my Recurring plan temporarily and resume it later?

No. Jupiter does not support pausing and resuming recurring plans. You must cancel the plan and create a new one when you're ready to resume.

---

### 5.5. How can I export my transactions for tax or other purposes?

Yes, you can export your Recurring and Trigger transaction history by viewing your wallet's transaction records through block explorers like Solana. These tools will allow you to download transaction data, which can be used for tax reporting and other purposes.

You can download all your transactions done by your wallet by using [Solscan](https://solscan.io/) when you click on the export CSV option in the transfer tab of your wallet.

![UI showing where to find the export CSV option in the transfer tab of your wallet](https://jupiverse.zendesk.com/attachments/token/4iQtOw9GlR3lWidd8K8GKTueY/?name=image.png)

---

### 5.6. Can I make changes to my Recurring plan after setting it up?

Unfortunately not, once a recurring plan is set up, it cannot be modified. To make changes, you would need to cancel the existing plan and create a new one with your updates preferences.

---

### 5.7. How is the price calculated with each recurring purchase?

The price for each purchase is calculated at the time the transaction is executed. Juputer checks the current market price and includes parameters like slippage settings to ensure you receive a minimum amount of tokens. If the price price exceeds the acceptable slippage, the transaction will fail to protect your funds.

Make sure to have a look at the recurring summary to make sure everything looks right:

![Recurring summary screenshot](https://support.jup.ag/hc/article_attachments/18600397922844)

---

### 5.8. Does my recurring plan automatically retry if it fails?

Yes, if a DCA purchase fails due to network issues, insufficient slippage, or other reasons. The program will automatically retry the order. This ensures minimal disruption to your strategy.

---

### 5.9. I see many failed transactions in my wallet, what's going on?

Third-party wallets like Phantom, Solflare do not do a good job of showing what is actually happening on Jupiter.

Use Jupiter's built in history or activity page, or Solscan for deep-diving various transactions.

We will improve guides and documentation to make this process easier for all users.

---

### 5.10. In the Wallet transaction history, of using the Trigger order, I see many failed transactions, did I pay for that transaction fees?

* By initiating an order on the trigger order, a keeper will try to fill your orders with swaps as soon as the specified price is reached. Sometimes, mostly due to slippage, transactions can fail.
* The transaction fees for transactions initiated *(and signed)* by the keeper are paid by the keeper.

---

### 5.11. Why is my Trigger Order not being filled?

Trigger Orders execute your order based on the price that you have set by matching with the available liquidity on-chain across Solana. A few scenarios or cases where the order is not being fulfill

If the order size is too large *(and there is insufficient liquidity on-chain)* - in this case, Jupiter keeper will attempt to execute your order in a smaller chunk to partially fill your orders and will continue to do so until order is fully executed

The price wick happen for a very short period of time, and the liquidity have all been taken up at that price.

We are actively developing improvements to Trigger Orders.

---

## 6. Pro

### 6.1. Using Jupiter Pro

Jupiter Pro is designed for advanced traders who discover, analyse, and trade tokens blazing fast. We bring the most advanced analytics, industry-leading MEV protection and 10x lower fees than others

#### As a Pro, you can

✅ Add Tokens to your Watchlist

✅ Trade a default currency (USDC/SOL) instantly with Quick Buy (tip: trade even faster by combining Quick Accounts' signless transactions)

✅ Discover your token of the day/hour/sec with filters and realtime analytics

✅ Use metrics: Net Buy volume, Net Buyers, Smart Likes

✅ Be protected by Ultra's MEV protection. Say no to sandwiches!

✅ Pay 10x lesser in trading fees compared to others

![Screenshot of Jupiter Pro](https://pbs.twimg.com/media/Gogh71NW4AUKEz3?format=jpg&name=4096x4096)

---

### 6.2. Using AlphaScan on Jupiter Pro

![Screenshot of AlphaScan feed](https://support.jup.ag/hc/article_attachments/19782396930972)

The AlphaScan feed helps you discover and trade new token launches like a pro. This is the only pro terminal that fetches launchpad tokens beyond 24 hours. You'll see:

1. **New**: Newly created tokens with at least one liquidity pool.
2. **Soon**: Tokens with bonding curve progress above 50%
3. **Bonded**: Tokens that have migrated liquidity to an AMM

You can filter by launchpads, marketcap, net buy volume, and more. You can trade any token directly from the feed using the Quick Buy ⚡️ button.

---

### 6.3. How do I check if a token is safe to trade on Jupiter Pro?

On Jupiter Pro, we added several indicators that can help you gauge whether or not a token is safe for you to trade. However, they are not catch-all and always do your own research!

**Developer Controls**

* Mint Authority tells you if the dev can create new tokens
* Freeze Authority shows if the dev can freeze token accounts

**Token Distribution**

* Total number of holders
* Bonding Curve Percentage
* Dev Bought and Sold are indicated as DB and DS on the chart
* Top 10 Holders: % owned by top 10 holders. ✅ if top 10 holders owns less than 15%

**Community Metrics**

* 🌱 Organic Score measures the organic interest of a token
* 👍 CT Likes brings the community's validation with liking tokens via X or flagging when a token is a potential honeypot or scam; all through the wisdom of the masses

---