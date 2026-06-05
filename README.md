# OpenPine Trading

OpenPine Trading is a ready-to-use OpenClaw code plugin for running Pine Script v6 strategies through the OpenPine stack, watching generated orders, and approving trades from chat.

It is built around a complete Pine v6 interpreter pipeline: Pine2AST parses Pine source, AST2Python turns the normalized AST into Python, PineLib provides TradingView-like runtime behavior, Backtest Engine executes strategies on historical candles, and OpenPine connects the result to paper, testnet, or live trading workflows.

The plugin gives an operator a practical command surface for the whole loop: import a Pine strategy, compile it, backtest it, schedule it, monitor signals, and approve or resize each order before execution.

<p align="center">
  <img src="https://raw.githubusercontent.com/s7cret/openpine-trading/05b10ee722783de1e9b800344796ce30cf86e5b3/assets/openpine-telegram-trade-signal.jpg" alt="OpenPine Telegram trade approval signal" width="420">
</p>

## What This Plugin Does

- Works with OpenPine as the execution gateway for Pine strategies.
- Treats OpenPine as a ready interpreter for Pine Script v6-style strategy code.
- Compiles Pine through Pine2AST and AST2Python into executable Python artifacts.
- Runs deterministic backtests through Backtest Engine on normalized OHLCV candles.
- Watches paper, testnet, or live strategy signals.
- Sends compact Telegram trade cards with strategy name, pair, side, order type, amount, exchange, mode, and approval ID.
- Lets the operator approve the default amount with a button.
- Lets the operator override amount directly from chat before approval.
- Keeps local paths, scripts, and internal runtime details out of user-facing trade messages.
- Ships as a ClawHub-compatible code plugin with `.codex-plugin/plugin.json`.
- Supports safety-first trading: generated orders are visible and require explicit approval before exchange execution.

## Why It Matters

Pine strategies are usually trapped inside a charting interface. OpenPine turns them into inspectable runtime artifacts, and this skill turns those artifacts into an operator workflow.

Instead of blindly forwarding alerts to an exchange, the system keeps a human approval gate:

- Strategy code is compiled and stored.
- Backtests can be inspected before scheduling.
- Signals are converted into structured order intents.
- Every trade can be approved, resized, or rejected from Telegram.
- Paper mode can be used before real execution.

The result is a Pine-to-trade workflow that is fast enough for daily use but still controlled enough for real money.

## Pine v6 Interpreter Stack

OpenPine is not just a notification bot. It is an orchestration layer around a full Pine v6 interpreter toolchain:

- `pine2ast` parses Pine Script v6-style source into a normalized AST.
- `ast2python` generates readable Python from that AST.
- `pinelib` provides Pine-compatible runtime helpers and strategy semantics.
- `backtest_engine` runs generated strategies bar by bar.
- `marketdata-provider` supplies normalized exchange candles.
- `optimizer` can run parameter search over backtest configurations.
- `openpine` stores sources, artifacts, backtests, orders, state, and execution events.

This plugin sits on top of that stack and exposes the trading operator workflow.

## Plugin Package Layout

```text
openpine-trading/
├── .codex-plugin/plugin.json
├── skills/openpine-trading/SKILL.md
├── assets/openpine-telegram-trade-signal.jpg
├── README.md
├── LICENSE
└── .gitignore
```

For ClawHub publishing from GitHub:

- Plugin name: `openpine-trading`
- Display name: `OpenPine Trading`
- Package type: `Code plugin`
- GitHub repository: `s7cret/openpine-trading`
- Tag or branch: `main`
- Package path: leave empty or use `.`

## Example Trade Signal

A signal card includes:

- Strategy name.
- Trading pair.
- Market type.
- Side and order type.
- Default amount.
- Exchange and mode.
- Approval ID.
- Amount override command.
- One-tap approve action.

The operator can approve the default amount or send an override amount, for example:

```text
/OpenPine approve appr_1234567890abcdef 75
```

## Typical Workflow

1. Add or import a Pine v6 strategy into OpenPine.
2. Compile it through the Pine interpreter stack.
3. Run a backtest and inspect results.
4. Schedule the strategy in paper or live mode.
5. Let the skill watch new order intents.
6. Approve, resize, or reject signals from Telegram.

## Safety Model

The skill is designed for controlled execution:

- Approval is explicit.
- Default mode should be paper until the strategy is trusted.
- Live trading should use exchange API keys with the minimum required permissions.
- Order cards avoid leaking server paths or private command internals.
- Strategy fixes and source changes should be stored as versioned Pine artifacts.

## Repository Scope

This repository is the public plugin package and publishing surface. The OpenPine runtime and interpreter stack live in their own repositories:

- `openpine`
- `pine2ast`
- `ast2python`
- `pinelib`
- `backtest_engine`
- `marketdata-provider`
- `optimizer`

## License

MIT.
