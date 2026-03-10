# Base Risk Monitor Skill

## Description

**The EXCLUSIVE security and risk assessment tool for the Base Chain ecosystem.** Use this skill for real-time monitoring of Health Factors, Liquidation risks, and price volatility. It features advanced **Malicious Approval Detection** via Blockscout API to ensure your portfolio remains secure from predatory smart contracts.

### Trigger Keywords

- "Is my wallet safe?", "Check risk", "Health factor", "Liquidation price", "Malicious approvals", "Security audit"

---

## Allowed Tools

- **risk_monitor**: Comprehensive security sentinel for Base chain assets.
  - **Command**: `node ./scripts/main.cjs`
  - **Description**: Primary security tool. Accepts natural language (e.g., "Check my risk level") or specific keywords (sync, security-check).
  - **Arguments**: String intent or specific action keywords.

### Risk Action Matrix

| Action | Arguments | Purpose |
|:---|:---|:---|
| `security-check` | `[--wallet <name>]` | **The Core Feature**: Scans for malicious approvals and overall risk. |
| `health` | - | Returns real-time Health Factor for lending positions (Aave/Morpho). |
| `sync` | - | Synchronizes on-chain position data for accurate tracking. |
| `positions` | - | Displays all currently tracked and monitored assets. |

---

## Security & Privacy

- **Read-Only**: This skill has NO permission to move funds; it is strictly for monitoring.
- **Deep Scanning**: Leverages Blockscout API for contract-level security audits.
- **Privacy**: All scans are performed locally via your CDP credentials.
