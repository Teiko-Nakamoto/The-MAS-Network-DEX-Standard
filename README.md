MAS SATS – Token & DEX (sBTC Pair)
Clarity smart contracts for a sustainable, fee-driven project funding model on the Stacks blockchain.

Overview
This repository contains two smart contracts that work together to create a sustainable funding mechanism for crypto projects:

MAS SATS Token Contract – A SIP-010 fungible token representing project “units” with locking and majority-holder mechanics. The majority token locker can also update the token’s metadata URI.

MAS SATS DEX/Treasury Contract – An automated market maker paired with sBTC that allows users to buy and sell MAS tokens. Every trade charges a fee, which is stored on-chain and can be claimed by the majority token locker under specific rules.

The goal is to align incentives between traders and long-term supporters while creating transparent, on-chain treasuries for projects.

Key Features
Token Contract
SIP-010 Compliant with 8 decimal places.

Max supply: 21,000,000 MAS (21M × 10⁸ base units).

One-time mint of the full supply to the DEX contract.

Token locking system to determine the majority holder.

Majority locker can update the token’s metadata URI.

DEX/Treasury Contract
Trading pair: sBTC.

Automated market maker with virtual liquidity of 1,500,000 sats.

Trade fee: 2.1% total on every buy/sell:

0.6% → Swap fee wallet.

1.5% → Fee pool for majority locker.

Fee claim rules:

Only the current majority locker can withdraw from the fee pool.

Initial unlock threshold = 1,500 sats.

After each withdrawal, the unlock threshold becomes the last withdrawn amount (max 1,000,000 sats).

If you withdraw fees, you cannot unlock tokens until the fee pool is replenished to at least the threshold.

Contract Addresses & IDs (Testnet)
sBTC token: ST1F7QA2MDF17S807EPA36TSS8AMEFY4KA9TVGWXT.sbtc-token

MAS SATS token: ST37918Q7NBZ52AMV133VTY5C864KVK0S2HZ3CGA4.mas-sats

MAS SATS DEX: ST37918Q7NBZ52AMV133VTY5C864KVK0S2HZ3CGA4.mas-sats-dex

How It Works
Buy/Sell
Users can trade MAS for sBTC using the DEX contract. All trades use bonding-curve math with a virtual sBTC starting balance.

Fee Accumulation
2.1% fee is split between a swap fee wallet (0.6%) and the DEX’s fee pool (1.5%).

Majority Locker

Determined by locking tokens in the contract.

Must hold >50% of all locked tokens to claim status.

Fee Claim & Unlock Rules

Majority locker can withdraw from the fee pool.

After withdrawal, they must wait until trading replenishes the pool to the last withdrawn amount before unlocking tokens.

Functions
Token Contract
mint-entire-supply-to-dex – One-time mint to DEX (owner only).

lock-tokens / unlock-tokens – Lock MAS in the token contract to compete for majority.

claim-majority-holder-status – Set yourself as majority locker.

set-token-uri – Update token metadata URI (majority only).

SIP-010 functions: get-balance, transfer, get-name, get-symbol, etc.

DEX/Treasury Contract
buy / sell – Trade MAS for sBTC or vice versa.

lock-tokens / unlock-tokens – Lock MAS in the DEX contract for fee claims.

claim-majority-holder-status – Set yourself as majority locker (DEX side).

withdraw-fees – Claim accumulated sBTC fees (majority only).

Read-only helpers: get-buyable-token-details, get-sellable-sbtc, get-sbtc-fee-pool, get-threshold, etc.

Setup
Prerequisites
Clarinet for local testing.

Stacks wallet (testnet) for deployment.

Run Locally
bash
Copy
Edit
# Clone the repo
git clone <this-repo-url>
cd <repo-name>

# Test with Clarinet
clarinet test
Deployment
Deploy MAS SATS token contract.

Deploy MAS SATS DEX contract.

From the token contract, call mint-entire-supply-to-dex to fund the DEX.

Security Notes
Always use post-conditions when sending sBTC in buy, sell, or withdraw-fees.

Majority calculations are based on locked balances; lock amounts can change.

Fee pool thresholds are enforced to prevent draining without subsequent trading.

