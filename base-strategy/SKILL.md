---
name: base-strategy
description: Binance K线技术指标分析引擎 - 提供 RSI、MACD、布林带等技术指标计算与交易信号生成，可被其他 agent 调用。
user-invocable: true
requires:
  bins:
    - node
  env:
    - CDP_API_KEY_ID
    - CDP_API_KEY_SECRET
---

# Base Strategy Skill

基于 Binance API 的技术指标分析引擎，为 F0x 交易策略提供技术面信号支持。

## 功能特性

- **K线数据获取**: 直接从 Binance API 获取历史 K 线数据
- **技术指标计算**: RSI、MACD、布林带、ATR 等
- **交易信号生成**: 基于多指标组合判断买入/卖出/持有
- **批量分析**: 支持多代币批量扫描

## 支持指标

| 指标 | 说明 |
|------|------|
| RSI | 相对强弱指数，判断超买超卖 |
| MACD | 移动平均收敛发散，判断趋势 |
| Bollinger Bands | 布林带，判断支撑阻力 |

## Natural Language Triggers

- "Analyze RSI for DEGEN on 15m"
- "Generate MACD signal for BRETT"
- "Run technical analysis on FAI, JUNO"
- "Batch scan top tokens"

## CLI 命令

```bash
# 单币种分析
node packages/skills/base-strategy/main.ts analyze --symbol=DEGEN --interval=1h

# 批量分析
node packages/skills/base-strategy/main.ts batch --tokens=DEGEN,FAI,JUNO --interval=1h

# 参数说明
# --symbol=<TOKEN>     代币符号 (default: BTC)
# --interval=<TF>       时间周期: 1m, 5m, 15m, 1h, 4h, 1d (default: 1h)
# --limit=<N>           K线数量 (default: 100)
# --tokens=<LIST>       批量分析的代币列表
```

## 输出示例

```
🔬 Base Strategy - Technical Analysis Engine
===========================================

📊 Analyzing DEGEN on 1h timeframe...

💰 Current Price: $0.007261
📈 RSI (14): 42.50
📉 MACD: -0.000012 (signal: -0.000008)
📊 Bollinger Bands: 0.007350 / 0.007200 / 0.007050

🎯 Signal: BUY
   Reason: RSI 偏低, MACD 金叉
   Confidence: 60%
```

## 交易信号逻辑

### 买入信号 (BUY)
- RSI < 30 (超卖) +3分
- RSI < 40 (偏低) +1分
- MACD 金叉 +2分
- 价格触及布林下轨 +2分
- **总分 >= 3 分**

### 卖出信号 (SELL)
- RSI > 70 (超买) +3分
- RSI > 60 (偏高) +1分
- MACD 死叉 +2分
- 价格触及布林上轨 +2分
- **总分 >= 3 分**

### 持有信号 (HOLD)
- 不满足买入或卖出条件

## 依赖 Skills

- 可被 `trading-core` 买入/卖出前调用
- 可被 `orchestrator` 作为策略模块调用
- 输出 JSON 格式方便其他 agent 解析

## 注意事项

- 需要网络访问 Binance API
- 频率限制: 1200 次/分钟 (足够使用)
- 数据来源: Binance Spot API
