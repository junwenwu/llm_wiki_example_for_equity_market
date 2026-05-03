---
title: Position Sizing & Risk Management
type: source
created: 2026-05-02
updated: 2026-05-02
sources: [position-sizing-risk-management.md]
tags: [risk-management, position-sizing, trading, stop-loss]
---

# Position Sizing & Risk Management

Source summary covering how to calculate appropriate position sizes and manage trading risk.

---

## The 2% Rule

- **Core principle**: Never risk more than 2% of account equity on any single trade
- **Purpose**: Survive losing streaks, preserve capital for recovery
- **Math**: 10 consecutive 2% losses = ~18% drawdown (recoverable)

---

## Position Size Formula

```
Position Size = (Account Risk) / (Trade Risk per Share)

Where:
- Account Risk = Account Value × 2% (or your chosen risk %)
- Trade Risk per Share = Entry Price - Stop-Loss Price
```

### Example Calculation
- Account: $50,000
- Risk tolerance: 2% = $1,000 max risk
- Entry: $100
- Stop-loss: $95
- Risk per share: $5
- **Position size**: $1,000 / $5 = **200 shares** ($20,000 position)

---

## Stop-Loss Strategies

| Type | Description | Use Case |
|------|-------------|----------|
| **Fixed %** | Set % below entry (e.g., 5-10%) | Simple, consistent |
| **ATR-based** | Multiple of Average True Range | Adapts to volatility |
| **Support-based** | Below key technical level | Respects price structure |
| **Trailing** | Moves up with price, locks gains | Trend following |

---

## Gap Risk Considerations

- Overnight gaps can breach stop-losses
- Earnings announcements = heightened gap risk
- Mitigation: Reduce position size before known events
- Consider options for defined-risk exposure

---

## Related Pages

- [[candlestick-charts-basics]]
- [[risk-management]] *(concept)*
- [[active-trading]] *(concept)*
