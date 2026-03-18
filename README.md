# 🚀 Base-Chain-Quant: 工业级量化交易框架

基于 Monorepo 架构重塑，旨在为 Base 链提供高可靠、零依赖的自动化收益优化与风险监控方案。

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
![Platform: Base](https://img.shields.io/badge/Platform-Base-blueviolet)
![TypeScript](https://img.shields.io/badge/Language-TypeScript-blue)

---

## 🌟 核心亮点

| 特性 | 说明 |
|------|------|
| **零依赖运行** | 所有 Skill 通过 `esbuild` 编译为单体 `main.cjs`，无需安装 `tsx` 或复杂依赖 |
| **安全保险杠** | 原生支持 `DRY_RUN` 模拟模式，在执行任何链上交易前进行逻辑验证 |
| **逻辑解耦** | 核心逻辑集于 `shared-core`，通过 `Orchestrator` 实现多模块毫秒级串联 |
| **Monorepo 管理** | 统一的代码组织结构，便于维护和扩展 |

---

## 📦 模块说明

| Skill | 功能描述 | 体积 |
|-------|----------|------|
| **trading-core** | 基础交易底座，负责 CDP 账户交互、余额查询、Swap、Transfer | ~18KB |
| **risk-monitor** | 实时风险预警，监控 P&L、异常授权检测、持仓同步 | ~21KB |
| **yield-optimizer** | 收益优化大师，支持 MORPHO/AAVE/Moonwell 协议自动扫描与迁移 | ~3.7MB |
| **orchestrator** | 全自动指挥官，一键完成「风险检查 → APY 扫描 → 自动优化」闭环 | ~3.7MB |

---

## 🛠️ 快速开始

### 1. 构建

```bash
npm install
npm run build:skills
```

### 2. 配置

在 `.env.cdp` 中配置：

```bash
CDP_API_KEY_ID=your_api_key_id
CDP_API_KEY_SECRET=your_api_key_secret
CDP_WALLET_SECRET=your_wallet_secret
CDP_SMART_ACCOUNT_ADDRESS=0x...
DRY_RUN=true  # 开启模拟模式
```

### 3. 测试

```bash
# 查询余额
node dist/trading-core/scripts/main.cjs query

# 扫描收益
node dist/yield-optimizer/scripts/main.cjs scan USDC

# 运行全自动优化
node dist/orchestrator/scripts/main.cjs full-cycle
```

---

## 📖 命令参考

### Trading Core

```bash
node dist/trading-core/scripts/main.cjs query                    # 查询余额
node dist/trading-core/scripts/main.cjs swap DEGEN USDC 100    # 兑换
node dist/trading-core/scripts/main.cjs sell DEGEN 50           # 卖出
node dist/trading-core/scripts/main.cjs transfer USDC <to> 10  # 转账
```

### Risk Monitor

```bash
node dist/risk-monitor/scripts/main.cjs query                   # 查询余额
node dist/risk-monitor/scripts/main.cjs positions               # 查看持仓
node dist/risk-monitor/scripts/main.cjs security-check          # 安全检查
```

### Yield Optimizer

```bash
node dist/yield-optimizer/scripts/main.cjs scan USDC            # 扫描 APY
node dist/yield-optimizer/scripts/main.cjs optimize 10 2        # 自动优化
node dist/yield-optimizer/scripts/main.cjs il ETH USDC 1 3000   # 计算无常损失
```

### Orchestrator

```bash
node dist/orchestrator/scripts/main.cjs status                  # 系统快照
node dist/orchestrator/scripts/main.cjs daily                   # 每日报告
node dist/orchestrator/scripts/main.cjs full-cycle              # 完整交易周期
```

---

## 🔒 安全机制

### DRY_RUN 模拟模式

在 `.env.cdp` 中设置：

```bash
DRY_RUN=true
```

开启后，所有交易类操作（swap、migrate、optimize）只会打印日志，不会实际执行链上交易。

---

## 📂 目录结构

```
base-trading-framework/
├── packages/
│   ├── shared-core/src/          # 核心逻辑
│   │   ├── config.ts             # 凭证管理
│   │   ├── logger.ts             # 日志服务
│   │   └── services/             # 业务服务
│   │       ├── trade.service.ts
│   │       ├── risk.service.ts
│   │       ├── yield.service.ts
│   │       └── orchestrator.service.ts
│   └── skills/                   # Skill 入口
│       ├── trading-core/
│       ├── risk-monitor/
│       ├── yield-optimizer/
│       └── orchestrator/
├── dist/                         # 构建产物（发布用）
│   ├── trading-core/scripts/main.cjs
│   ├── risk-monitor/scripts/main.cjs
│   ├── yield-optimizer/scripts/main.cjs
│   └── orchestrator/scripts/main.cjs
├── build.ts                      # esbuild 打包脚本
└── package.json
```

---

## 🔄 发布流程 (重要)

### 仓库关系

| 仓库 | 类型 | 用途 |
|------|------|------|
| `base-trading-framework` | 私有 | 源码开发 |
| `base-skills-public` | 公开 | ClawMart 上架产物 |

### 更新流程

**步骤：**
1. 修改 `packages/skills/*/SKILL.md` 或源码
2. 运行 `npx ts-node build.ts` 构建到 `dist/`
3. 推送源码到私有仓库 `base-trading-framework`
4. 将 `dist/` 内容推送到公开仓库 `base-skills-public`

**一键发布脚本：**
```bash
# 在 base-trading-framework 目录执行
./publish.sh
```

### 注意事项

- **先改源码**：所有修改必须先在 `packages/` 目录进行
- **从源码构建**：`SKILL.md` 修改后必须运行 build.ts 同步到 dist/
- **保持一致**：`packages/skills/`、`dist/`、`base-skills-public` 三处必须一致

---

## 🌐 支持的协议

| 协议 | 类型 | 状态 |
|------|------|------|
| MORPHO | 借贷 | ✅ |
| AAVE V3 | 借贷 | ✅ |
| Moonwell | 借贷 | ✅ |

---

## 📄 License

MIT License - see [LICENSE](LICENSE) for details.

---

## 🤝 贡献

欢迎提交 Issue 和 Pull Request！

---

*Built with ❤️ for Base Chain*
