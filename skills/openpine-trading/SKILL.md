---
name: openpine-trading
description: Operate OpenPine Pine Script v6 strategy compilation, backtests, signal monitoring, and trade approvals.
---

# OpenPine Trading

Use this skill when the user wants to compile, backtest, schedule, monitor, approve, resize, or reject Pine Script v6 strategy trades through OpenPine.

## Core Identity

OpenPine is a ready Pine Script v6 interpreter and trading automation gateway. This plugin provides the operator skill on top of that gateway: it turns compiled Pine strategy signals into clear Telegram approval cards and controlled execution actions.

## Capabilities

- Import or reference Pine Script v6 strategy source in OpenPine.
- Compile Pine source through the OpenPine interpreter pipeline.
- Run backtests before scheduling a strategy.
- Watch strategy-generated order intents.
- Send compact trade approval messages to Telegram.
- Approve the default amount with a button.
- Override amount from chat before approval.
- Reject unsafe or unwanted signals.
- Keep runtime paths and internal scripts out of user-facing messages.

## Interpreter Pipeline

Use this model when explaining or operating the system:

1. Pine source is stored in OpenPine.
2. `pine2ast` parses Pine v6-style code into a normalized AST.
3. `ast2python` generates executable Python from the AST.
4. `pinelib` supplies Pine-compatible runtime helpers.
5. `backtest_engine` executes strategy logic on candles.
6. OpenPine persists artifacts, runs, orders, positions, and events.
7. This skill watches order intents and exposes approval controls.

## Trade Message Format

Trade messages should be short and operator-friendly:

```text
OpenPine trade signal
Strategy: <name>
Pair: <symbol>
Market: <spot|futures>
Side: <BUY|SELL>
Type: <MARKET|LIMIT|...>
Amount: <amount> <asset>
Exchange: <exchange>
Mode: <paper|testnet|live>
ID: <approval_id>
Change amount: /OpenPine approve <approval_id> <amount>
```

Do not include local filesystem paths, Python command paths, or implementation details in user-facing trade cards.

## Safety Rules

- Prefer paper mode until the user explicitly wants live trading.
- Never approve live orders without clear user intent.
- Show strategy, symbol, side, amount, exchange, and mode before approval.
- If the amount is unclear, ask or require an explicit override.
- If compile/backtest fails, explain the exact incompatible Pine feature or runtime issue.
- If old watcher processes are sending stale messages, restart the watcher and verify only one current process remains.

## Useful Operator Commands

Approve default amount:

```text
/OpenPine approve <approval_id>
```

Approve custom amount:

```text
/OpenPine approve <approval_id> <amount>
```

Reject:

```text
/OpenPine reject <approval_id>
```

## Expected Tone

Be concise and operational. The user cares about whether the strategy compiled, whether the backtest ran, whether the watcher is current, and whether orders are safe to approve.
