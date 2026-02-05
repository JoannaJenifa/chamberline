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
├─────────────────────────────────────────────────────────────────┤
│  2. CONFIGURE                                                   │
│                                                                 │
│  ├── Set target allocation:                                    │
│  │   └── ETH: 50%                                              │
│  │   └── USDC: 30%                                             │
│  │   └── BTC: 20%                                              │
│  ├── Set drift threshold: 5%                                   │
│  └── Set rebalance constraints (max slippage, preferred chains)│
│                                                                 │
├─────────────────────────────────────────────────────────────────┤
│  3. MONITOR                                                     │
│                                                                 │
│  ├── Chamberline tracks prices continuously                    │
│  ├── Calculates current allocation vs target                   │
│  ├── When drift > threshold:                                   │
│  │   └── Alert user                                            │
│  │   └── Generate rebalance proposal                           │
│  └── Shows drift dashboard in real-time                        │
│                                                                 │
├─────────────────────────────────────────────────────────────────┤
│  4. REBALANCE                                                   │
│                                                                 │
│  ├── User reviews proposed trades                              │
│  ├── One-click execution:                                      │
│  │   └── Sell 2.1 ETH on Arbitrum (Uniswap v4)                │
│  │   └── Bridge USDC to Base (LI.FI)                          │
│  │   └── Buy WBTC on Base                                      │
│  ├── All trades execute atomically where possible              │
│  └── Dashboard updates with new allocations                    │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

## User Experience

### Dashboard

```
┌─────────────────────────────────────────────────────────────────┐
│  🦎 Chamberline                              0xabc...123 🟢     │
│                                                                 │
│  PORTFOLIO OVERVIEW                                             │
│  Total Value: $127,450 across 4 chains                         │
│                                                                 │
│  ┌───────────────────────────────────────────────────────────┐  │
│  │  Asset   │ Target │ Current │ Drift   │ Value    │ Chain  │  │
│  │──────────┼────────┼─────────┼─────────┼──────────┼────────│  │
│  │  ETH     │  50%   │  58%    │ +8%  ⚠️ │ $73,921  │ Multi  │  │
│  │  USDC    │  30%   │  24%    │ -6%  ⚠️ │ $30,588  │ Multi  │  │
│  │  BTC     │  20%   │  18%    │ -2%     │ $22,941  │ Arb    │  │
│  └───────────────────────────────────────────────────────────┘  │
│                                                                 │
│  ⚠️ DRIFT ALERT: Portfolio exceeds 5% threshold                │
│                                                                 │
│  REBALANCE PROPOSAL                                             │
│  ┌───────────────────────────────────────────────────────────┐  │
│  │  To restore target allocation:                            │  │
│  │                                                           │  │
│  │  Step 1: SELL 2.1 ETH ($3,948)                           │  │
│  │          └── Arbitrum → Uniswap v4 → USDC                │  │
│  │                                                           │  │
│  │  Step 2: BRIDGE $3,948 USDC                              │  │
│  │          └── Arbitrum → Base (via LI.FI)                 │  │
│  │                                                           │  │
│  │  Step 3: BUY 0.04 WBTC ($2,549)                          │  │
│  │          └── Base → Uniswap v4                           │  │
│  │                                                           │  │
│  │  Remaining $1,399 USDC stays on Base (increases stables) │  │
│  │                                                           │  │
│  │  Est. Total Cost: $4.20 │ Slippage: <0.1%                │  │
│  └───────────────────────────────────────────────────────────┘  │
│                                                                 │
│  [🦎 Execute Rebalance]  [Simulate Only]  [Edit Targets]       │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### Target Configuration

```
┌─────────────────────────────────────────────────────────────────┐
│  CONFIGURE TARGETS                                              │
│                                                                 │
│  Allocation Strategy:                                           │
│  ┌───────────────────────────────────────────────────────────┐  │
│  │  [x] Custom                                               │  │
│  │  [ ] Conservative (60% stables, 30% ETH, 10% BTC)        │  │
│  │  [ ] Balanced (40% stables, 40% ETH, 20% BTC)            │  │
│  │  [ ] Aggressive (20% stables, 50% ETH, 30% alts)         │  │
│  └───────────────────────────────────────────────────────────┘  │
│                                                                 │
│  Custom Allocation:                                             │
│  ┌───────────────────────────────────────────────────────────┐  │
│  │  ETH      [============================] 50%              │  │
│  │  USDC     [================] 30%                          │  │
│  │  BTC      [==========] 20%                                │  │
│  │                                        Total: 100% ✅     │  │
│  └───────────────────────────────────────────────────────────┘  │
│                                                                 │
│  Rebalance Settings:                                            │
│  ├── Drift Threshold: [5%] (alert when exceeded)               │
│  ├── Max Slippage: [0.5%]                                      │
│  ├── Preferred DEX: [Uniswap v4 ▼]                             │
│  └── Auto-rebalance: [Off ▼] (Manual / Daily / Weekly)         │
│                                                                 │
│  [Save Configuration]                                           │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

## Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│  Chamberline Dashboard (React)                                  │
│  ├── Portfolio view                                            │
│  ├── Target configuration                                      │
│  ├── Drift visualization                                       │
│  └── Rebalance execution                                       │
└───────────────────────────┬─────────────────────────────────────┘
                            │
                            ▼
┌─────────────────────────────────────────────────────────────────┐
│  Chamberline Agent Core                                         │
│                                                                 │
│  ├── Portfolio Aggregator                                      │
│  │   └── Multi-chain balance fetching (RPC + indexers)        │
│  │                                                             │
│  ├── Drift Calculator                                          │
│  │   └── Current allocation vs target, price feeds            │
│  │                                                             │
│  ├── Strategy Engine                                           │
│  │   └── Optimal rebalance path (minimize cost + slippage)    │
│  │                                                             │
│  └── Execution Engine                                          │
│      └── Route and execute trades                             │
│                                                                 │
└───────────────────────────┬─────────────────────────────────────┘
                            │
          ┌─────────────────┼─────────────────┐
          ▼                 ▼                 ▼
    ┌──────────┐      ┌──────────┐      ┌──────────┐
    │  LI.FI   │      │ Uniswap  │      │   Sui    │
    │ (bridge  │      │   v4     │      │ DeepBook │
    │  + swap) │      │ (swaps)  │      │          │
    └──────────┘      └──────────┘      └──────────┘
```

---

## Agent Decision Logic

### Drift Detection

```typescript
interface PortfolioState {
  assets: {
    symbol: string;
    targetPercent: number;
    currentPercent: number;
    valueUSD: number;
    chains: { chain: string; balance: number }[];
  }[];
  totalValueUSD: number;
}

function calculateDrift(state: PortfolioState): DriftReport {
  return state.assets.map(asset => ({
    symbol: asset.symbol,
    drift: asset.currentPercent - asset.targetPercent,
    driftUSD: (asset.drift / 100) * state.totalValueUSD,
    action: asset.drift > 0 ? 'SELL' : 'BUY',
    amount: Math.abs(asset.driftUSD)
  }));
}

function shouldRebalance(drift: DriftReport, threshold: number): boolean {
  return drift.some(d => Math.abs(d.drift) > threshold);
}
```

### Rebalance Strategy Generation

```typescript
interface RebalanceStep {
  action: 'SWAP' | 'BRIDGE' | 'BRIDGE_AND_SWAP';
  fromAsset: string;
  toAsset: string;
  fromChain: string;
  toChain: string;
  amount: number;
  estimatedOutput: number;
  estimatedCost: number;
  route: LiFiRoute | UniswapRoute;
}

async function generateRebalancePlan(
  drift: DriftReport,
  constraints: RebalanceConstraints
): Promise<RebalanceStep[]> {
  
  // 1. Identify sells (assets over target)
  const sells = drift.filter(d => d.drift > 0);
  
  // 2. Identify buys (assets under target)
  const buys = drift.filter(d => d.drift < 0);
  
  // 3. For each sell, find optimal route
  const steps: RebalanceStep[] = [];
  
  for (const sell of sells) {
    // Get LI.FI quote for cross-chain if needed
    const lifiQuote = await getLiFiQuote({
      fromChain: sell.bestChainToSell,
      toChain: buys[0].bestChainToBuy,
      fromToken: sell.symbol,
      toToken: 'USDC', // intermediate
      amount: sell.amount
    });
    
    steps.push({
      action: 'BRIDGE_AND_SWAP',
      ...lifiQuote
    });
  }
  
  // 4. Optimize for gas (batch same-chain operations)
  return optimizeSteps(steps, constraints);
}
```

---

## Cross-Chain Execution

### Same-Chain Rebalance

```
ETH overweight on Arbitrum, need more USDC on Arbitrum
         │
         ▼
Simple Uniswap v4 swap
         │
         ▼
ETH → USDC on Arbitrum
         │
         ▼
Done (single tx, ~$0.50)
```

### Cross-Chain Rebalance

```
ETH overweight on Arbitrum, need USDC on Base
         │
         ▼
LI.FI Composer route:
├── Swap ETH → USDC on Arbitrum
├── Bridge USDC Arbitrum → Base
└── All in one flow
         │
         ▼
Done (appears as single operation to user)
```

### Multi-Asset Rebalance

```
ETH overweight, USDC and BTC underweight
         │
         ▼
Chamberline optimizes:
├── Step 1: Sell 2.1 ETH → USDC (Arbitrum, Uniswap v4)
├── Step 2: Bridge 60% USDC to Base (LI.FI)
├── Step 3: Swap USDC → WBTC on Base (Uniswap v4)
├── Step 4: Keep 40% USDC on Arbitrum
         │
         ▼
Three txs batched, total cost ~$5
```

---

## Sui Integration

### DeepBook for Sui Positions

```
User has SUI tokens on Sui network
         │
         ▼
Chamberline reads balance via Sui RPC
         │
         ▼
If rebalance needed on Sui:
├── Use DeepBook for limit orders (better prices)
├── Or market orders for immediate execution
└── PTB for atomic multi-step on Sui
```

### Cross-Chain to/from Sui

```
Need to move value from EVM to Sui (or vice versa)
         │
         ▼
LI.FI handles Sui bridging
├── Wormhole for most assets
└── Native bridges where available
```

---

## Execution Proofs

Every rebalance generates proof:

```
┌─────────────────────────────────────────────────────────────────┐
│  REBALANCE COMPLETE ✅                                          │
│                                                                 │
│  Execution Summary:                                             │
│  ├── Total Value Moved: $7,640                                 │
│  ├── Total Cost: $4.18                                         │
│  ├── Slippage: 0.08%                                           │
│  └── Time: 47 seconds                                          │
│                                                                 │
│  Transactions:                                                  │
│  ├── Arbitrum: 0xabc...123 (Sell 2.1 ETH)                     │
│  ├── LI.FI: 0xdef...456 (Bridge USDC)                         │
│  └── Base: 0x789...xyz (Buy WBTC)                             │
│                                                                 │
│  Before → After:                                                │
│  ├── ETH: 58% → 50.2%                                         │
│  ├── USDC: 24% → 29.8%                                        │
│  └── BTC: 18% → 20.0%                                         │
│                                                                 │
│  [View on Explorer] [Download Report]                          │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

## Database Schema

```sql
portfolios:
  id                TEXT PRIMARY KEY
  wallet_address    ADDRESS
  name              TEXT
  created_at        TIMESTAMP

portfolio_targets:
  portfolio_id      TEXT REFERENCES portfolios
  asset_symbol      TEXT
  target_percent    DECIMAL
  
portfolio_settings:
  portfolio_id      TEXT REFERENCES portfolios
  drift_threshold   DECIMAL DEFAULT 5.0
  max_slippage      DECIMAL DEFAULT 0.5
  auto_rebalance    ENUM (off, daily, weekly)
  preferred_chains  TEXT[]

rebalance_history:
  id                TEXT PRIMARY KEY
  portfolio_id      TEXT REFERENCES portfolios
  executed_at       TIMESTAMP
  total_value_moved DECIMAL
  total_cost        DECIMAL
  steps             JSON
  before_allocation JSON
  after_allocation  JSON
  tx_hashes         TEXT[]
```

---

## Why This Wins

| Angle | Why It's Compelling |
|-------|---------------------|
| **Universal pain** | Everyone with multi-chain assets has this problem |
| **Clear agent logic** | Monitor → detect drift → propose → execute |
| **Novel execution** | Cross-chain rebalancing in one flow doesn't exist |
| **Great demo** | Show drift, click rebalance, watch execution across chains |
| **LI.FI showcase** | This is exactly what Composer is built for |

---

## Prize Track Alignment

| Prize | How We Qualify |
|-------|----------------|
| **LI.FI - Best AI x LI.FI** ($2k) | Agent using LI.FI as cross-chain execution layer |
| **Uniswap v4 - Agentic Finance** ($5k) | Agent programmatically managing positions via v4 |
| **Sui - Notable Project** ($1k) | DeepBook integration for Sui-side execution |

**Total potential: $8k**

---

## Hackathon Scope

### Must Have
- [ ] Multi-chain balance aggregation (Arbitrum, Base, Ethereum)
- [ ] Target allocation configuration UI
- [ ] Drift calculation and visualization
- [ ] Rebalance proposal generation
- [ ] LI.FI integration for cross-chain swaps
- [ ] Uniswap v4 integration for EVM swaps
- [ ] Basic execution flow (manual trigger)
- [ ] Execution proof display

### Nice to Have
- [ ] Sui DeepBook integration
- [ ] Automated triggers (rebalance when drift > X%)
- [ ] Historical performance tracking
- [ ] Gas optimization (batch where possible)
- [ ] Multiple portfolio support
- [ ] Preset allocation strategies

---

## The Pitch

> "Your portfolio drifts every day. ETH pumps, you're overexposed. Stables drop, you're under-hedged. Chamberline watches your allocation across every chain, and when drift exceeds your threshold, rebalances everything in one click. Cross-chain. Atomic. Done."
