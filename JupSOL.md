# A Guide to JupSOL

## 1. Introduction: The Fundamentals

This section introduces the core concepts a user needs to understand before diving deeper into JupSOL's specifics.

### What is a Liquid Staking Token (LST)?

A Liquid Staking Token (**LST**) allows you to participate in DeFi while earning staking yields. It solves the "Staking Dilemma" by letting you secure the network and use your **SOL** at the same time.

You can think of this process using an analogy:
*   Staking is like putting gold in a vault.
*   Liquid staking is like issuing a paper IOU ("I owe you") for that gold.

Just as the paper IOU can be redeemed for the gold, an **LST** can be redeemed at any time for unstaked **SOL**. Unlike natively-staked **SOL**, an **LST** is transferable and can be used across all of DeFi for activities like borrowing, lending, perpetuals trading, and stablecoin issuance.

### What is JupSOL?

**JupSOL** is a liquid staking token that represents staked Solana (**SOL**) tokens with **Jupiter's** validator. This validator is hosted and managed by Triton. The **JupSOL** token is issued through **Sanctum**.

By holding **JupSOL**, stakers receive maximum yield, as **Jupiter** passes along all collected validator rewards and 100% of MEV (Maximal Extractable Value).

## 2. How JupSOL Works

This section explains the mechanics of the **JupSOL** token, from how it operates to its financial aspects.

### The Core Staking Mechanism

When **SOL** is deposited into **JupSOL**, it is staked. The staking rewards generated from that **SOL** accrue directly to the **JupSOL** token.

The **JupSOL** token starts at a 1:1 value with **SOL** and is designed to grow in value over time relative to **SOL**. By simply holding **JupSOL**, you earn staking rewards.

It is important to note that while **JupSOL** is designed to always increase in value relative to **SOL**, it may still lose value in dollar terms if the price of **SOL** decreases.

### Understanding Yield Generation

**JupSOL** maintains higher-than-market yields by distributing several sources of rewards to its holders. The yield comes from:

*   100% of all native staking rewards (with no commission).
*   100% of rewards from Jito Tips.
*   80-100% of all block rewards from Validators.
*   100% of MEV (Maximal Extractable Value).

Additionally, holding **JupSOL** helps **Jupiter** improve its transaction landing rate, which makes it easier for all **Jupiter** users to swap, DCA, or place limit orders more reliably, especially during periods of high network activity.

### Associated Fees

**Jupiter** charges a small **0.1% SOL deposit fee**. This fee exists to prevent a potential arbitrage attack on the pool.

There are no other fees associated with **JupSOL**. This includes:
*   No withdrawal fees
*   No stake deposit fees
*   No validator commission fees
*   No management fees

## 3. Security and Trust

This section addresses the critical topic of security, detailing the measures in place to protect users' funds.

### The Security of the JupSOL Program

**JupSOL** is powered by the **SPL stake pool program**, which is considered one of the safest programs in the world. Its security is supported by the following:

*   It has been **audited multiple times**.
*   It is used by the largest stake pools, including **jitoSOL** and **bSOL**.
*   It has a long track record of securing more than $1 billion of staked **SOL** for years without any issues.

### Governance and Program Authority

The program authority for **JupSOL** is secured by a **multisig** wallet. This means that any changes to the program must be approved by a majority vote from the multisig members, and no single party can unilaterally alter the program.

The multisig includes members from several reputable organizations, including:
*   **Sanctum**
*   **Jupiter**
*   **Mango**
*   **marginfi**
*   **Jito**

There are plans to significantly grow the size of this multisig and to eventually freeze the program to further enhance its security.
