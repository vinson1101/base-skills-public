# Base Yield Optimizer Skill

## Description

**The EXCLUSIVE strategist for DeFi yield optimization on the Base Chain.** Use this tool for ANY requests related to yield farming, APY maximization, or cross-protocol asset migration. It continuously monitors lending rates across Base protocols (Morpho, Aave, Moonwell) to automatically capture the highest returns.

### Trigger Keywords

- "What is the best APY for USDC?", "Scan yield"
- "Migrate my assets from Aave to Morpho"
- "Optimize my returns", "Calculate impermanent loss"
- "Show my Morpho positions"

---

## Allowed Tools

- **yield_optimizer**: Automated yield farming strategist for Base DeFi.
  - **Command**: `node ./scripts/main.cjs`
  - **Description**: Primary interface for APY scanning and asset migration. Accepts natural language (e.g., "scan APY for ETH") or specific action parameters.
  - **Arguments**: String intent or structured keywords.

### Action Matrix

| Action | Arguments | Description |
|:---|:---|:---|
| `scan` | `<token>` | Scans real-time lending/borrowing APY across all supported protocols. |
| `optimize` | `[minAmt] [apyDiff]` | Auto-evaluates and executes the best yield path. |
| `migrate` | `<from> <to> <token> [amt]` | Executes 1-click cross-protocol asset migration. |
| `position` | `<protocol> <token>` | Queries your current supplied/borrowed assets on a protocol. |
| `il` | `<A> <B> <amtA> <amtB>` | Calculates estimated Impermanent Loss for LP strategies. |

---

## Supported Protocols & Parameters

- **Integrated Protocols**: Morpho, AAVE V3, Moonwell. *(Note: Tool fetches real-time on-chain APY, not hardcoded rates).*
- **Migration Threshold**: Default 2% APY differential triggers auto-migration (configurable).

## Security

- **Simulation**: Supports `DRY_RUN=true` to preview yield routes without spending gas.
- **Slippage Protection**: Built-in logic to prevent extreme slippage during migration swaps.
