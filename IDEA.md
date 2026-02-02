# Chamberline 🦎 — Autonomous Cross-Chain Portfolio Rebalancer

> Your portfolio drifts. Chamberline fixes it. Automatically, across every chain.

---

## Problem

Multi-chain portfolios are impossible to manage:

- **Drift is constant** — ETH pumps 40%, now you're overweight and exposed
- **Assets are scattered** — positions across 5 chains with no unified view
- **Rebalancing is painful** — manual bridging, swapping, gas on every chain
- **You always forget** — by the time you notice, the damage is done
- **Tools don't exist** — portfolio trackers show drift but can't fix it

Everyone with >$10k across chains has this problem. Nobody has a solution.

---

## Solution

An autonomous agent that:

1. **Aggregates** — connects to your wallets across all chains, unified view
2. **Monitors** — you set target allocations (50% ETH, 30% stables, 20% BTC)
3. **Detects** — calculates drift continuously, alerts when threshold exceeded
4. **Proposes** — generates optimal rebalance path across chains
5. **Executes** — one click, atomic execution via LI.FI + Uniswap v4 + Sui

**Chamberline** — like a financial chamberlain managing your treasury across kingdoms.

---

## Protocol Integrations

| Protocol | Role | How We Use It |
|----------|------|---------------|
| **LI.FI** | Cross-chain routing | Bridge + swap in single flow when rebalancing across chains |
| **Uniswap v4** | EVM execution | Swaps on EVM chains with hooks for MEV protection |
| **Sui** | Alternative venue | DeepBook for Sui-side portfolio positions |

---

## How It Works

### The Drift Problem

```
YOUR PORTFOLIO (January):
┌─────────────────────────────────────────────────────────────────┐
│  Target: 50% ETH | 30% USDC | 20% BTC                          │
│  Actual: 50% ETH | 30% USDC | 20% BTC                          │
│  Drift: 0% ✅                                                   │
└─────────────────────────────────────────────────────────────────┘

YOUR PORTFOLIO (March, after ETH +60%):
┌─────────────────────────────────────────────────────────────────┐
│  Target: 50% ETH | 30% USDC | 20% BTC                          │
│  Actual: 62% ETH | 22% USDC | 16% BTC                          │
│  Drift: +12% ETH ⚠️ | -8% USDC ⚠️ | -4% BTC ⚠️                │
│                                                                 │
│  You're overexposed to ETH. If it dumps, you lose more.        │
└─────────────────────────────────────────────────────────────────┘
```

### Chamberline Solution

```
┌─────────────────────────────────────────────────────────────────┐
│  1. CONNECT                                                     │
│                                                                 │
│  ├── Connect wallets across chains (read-only)                 │
│  ├── Chamberline scans: Ethereum, Arbitrum, Base, Sui          │
│  └── Aggregates all token balances                             │
│                                                                 │
