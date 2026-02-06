# CHAMBERLINE

> Your portfolio drifts. Chamberline fixes it. Automatically, across every chain.

An autonomous cross-chain portfolio rebalancer that monitors drift and executes optimal rebalancing across multiple chains — built for HackMoney 2026.

---

## Overview

Chamberline is an autonomous agent that:

- **Aggregates balances** — Reads positions across Arbitrum, Base, Ethereum, and Sui
- **Detects drift** — Compares current allocations against user-defined targets
- **Proposes rebalances** — Calculates optimal swaps and bridges to restore balance
- **Executes cross-chain** — Uses LI.FI for routing and Uniswap v4 for on-chain swaps

---

## Tech Stack

| Component | Technology |
|-----------|-----------|
| **Cross-chain Routing** | LI.FI |
| **EVM Swaps** | Uniswap v4 |
| **Sui Trading** | DeepBook v2 |
| **Frontend** | React + TypeScript |
| **Agent Core** | Node.js |

---

## How It Works

1. **Set Targets** — Define allocation targets (e.g., 50% ETH, 30% USDC, 20% BTC)
2. **Monitor** — Agent polls balances across all connected chains
3. **Detect Drift** — When allocation drifts beyond threshold (e.g., 5%), trigger rebalance
4. **Plan Execution** — Calculate optimal swap/bridge path with minimal slippage
5. **Execute** — Submit transactions via LI.FI with user approval
6. **Verify** — Show before/after allocations with execution proof

---

## Project Structure

```
chamberline/
├── IDEA.md                  # Detailed concept and architecture
└── PROMPT.md                # AI development prompts
```

---

## Documentation

- [IDEA.md](./IDEA.md) — Full concept, architecture, and integration details

---

## License

MIT
