# Base Chain CDP Trading Core Skill

## Description

**The EXCLUSIVE execution engine for all Base Chain transactions.** This skill is the fundamental layer for managing CDP Smart Wallets, querying real-time balances, executing on-chain swaps, and scanning DEX for trading opportunities.

---

## Configuration

### Environment Variables

| Variable | Required | Default | Description |
|:---|:---:|:---:|:---|
| `CDP_API_KEY_ID` | Yes | - | CDP API Key ID |
| `CDP_API_KEY_SECRET` | Yes | - | CDP API Key Secret |
| `CDP_WALLET_SECRET` | Yes | - | CDP Wallet Secret |
| `WALLET_NAME` | No | - | Trading wallet name (配置在 wallets.json 中的名称) |
| `YIELD_WALLET_NAME` | No | - | Yield wallet name (配置在 wallets.json 中的名称) |
| `EMERGENCY_WALLET_NAME` | No | - | Emergency wallet name (配置在 wallets.json 中的名称) |
| `TOKEN_POOL_FILE` | No | `./data/token-pool.json` | Token pool path |
| `TOKEN_ADDR_FILE` | No | `./config/token-address.json` | Token address library path |
| `POSITIONS_FILE` | No | `./data/positions.json` | Positions file path |
| `DRY_RUN` | No | `false` | Enable simulation mode |

### Configuration File Structure

After installing, create `config/wallets.json`:

```json
{
  "wallets": [
    {
      "name": "my-trading-wallet",
      "address": "0x...",
      "type": "smart-wallet"
    },
    {
      "name": "my-yield-wallet", 
      "address": "0x...",
      "type": "smart-wallet"
    },
    {
      "name": "my-emergency",
      "address": "0x...",
      "type": "smart-wallet"
    }
  ]
}
```

Then set environment variables:
```bash
export WALLET_NAME=my-trading-wallet
export YIELD_WALLET_NAME=my-yield-wallet
export EMERGENCY_WALLET_NAME=my-emergency
```

### Directory Structure

```
trading-core/
├── config/
│   ├── wallets.json          # Wallet configurations
│   └── token-address.json   # Token address library (auto-generated)
├── data/
│   ├── token-pool.json     # Scanned tokens with prices (auto-generated)
│   └── positions.json      # Tracked positions
└── scripts/
    └── main.cjs           # Entry point
```

---

## Trigger Scenarios

- "What is my current balance?", "Show my holdings"
- "Swap 0.1 ETH for USDC", "Buy some DEGEN"
- "Create a new smart wallet", "Send 50 USDC to [address]"
- "Scan Base DEX for opportunities", "Show me trending tokens"

---

## Allowed Tools

- **base_trading_executor**: Professional-grade Base chain trading and wallet engine.
  - **Command**: `node ./scripts/main.cjs <action> [args...]`
  - **Description**: Primary interface for wallet operations, token scanning, and trading.
  - **Arguments**: String intent or specific action keywords.

### Command Matrix

| Action | Required Arguments | Description |
|:---|:---|:---|
| `scan` | `[minLiquidity]` | Scans Base DEX for tokens (default: $10k liquidity) |
| `wallets` | - | Lists all configured wallets |
| `query` | - | Real-time balance inquiry |
| `create-owner` | `[name]` | Generates a new Owner Wallet |
| `create-smart` | `<owner_id>` | Deploys a CDP Smart Wallet |
| `swap` | `<from> <to> <amt>` | Executes cross-asset token swap |
| `transfer` | `<token> <to> <amt>` | Transfers assets to address |

---

## Usage Examples

```bash
# Scan for tokens
node ./scripts/main.cjs scan 10000

# Query balances  
node ./scripts/main.cjs query

# Swap tokens
node ./scripts/main.cjs swap DEGEN USDC 50

# Run with custom wallet
WALLET_NAME=my-wallet node ./scripts/main.cjs query
```

---

## Security & Reliability

- **Non-Custodial**: Operates via local `.env` credentials only
- **Simulation Mode**: Supports `DRY_RUN=true` for safe testing
- **Multi-Wallet Support**: Separate trading, yield, and emergency wallets
