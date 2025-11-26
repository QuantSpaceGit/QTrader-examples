# QTrader Examples

Example trading strategies and experiments demonstrating the [QTrader](https://github.com/QuantSpaceGit/QTrader) backtesting framework.

## Quick Start

```bash
# 1. Setup environment
make setup

# 2. Run a backtest
uv run qtrader backtest experiments/buy_hold
```

## Project Structure

```text
├── config/                 # Global configuration
│   ├── qtrader.yaml       # Main QTrader settings
│   └── data_sources.yaml  # Data provider configuration
├── data/                   # Market data
│   └── sample-csv/        # Sample OHLCV data
├── experiments/            # Backtest experiments
│   ├── buy_hold/          # Buy and hold strategy
│   ├── sma_crossover/     # SMA crossover strategy
│   └── template/          # Experiment template
└── library/                # Custom components
    ├── adapters/          # Data adapters
    ├── indicators/        # Technical indicators
    ├── strategies/        # Trading strategies
    └── risk_policies/     # Risk management
```

## Setup

### 1. Install Dependencies

```bash
make setup
```

### 2. Configure Data Source

Update your universe at `data/us-equity-yahoo-csv/universe.json`, then download data:

```bash
uv run qtrader data yahoo-update
```

Or create a custom data adapter - see [QTrader documentation](https://github.com/QuantSpaceGit/QTrader).

### 3. Review Configuration

Edit files in `config/` as needed:

- `qtrader.yaml` - Main QTrader settings
- `data_sources.yaml` - Data source configuration

## Running Experiments

### Run a Backtest

```bash
# Directory-based (recommended)
uv run qtrader backtest experiments/buy_hold

# Direct file path
uv run qtrader backtest experiments/sma_crossover/sma_crossover.yaml

# With options
uv run qtrader backtest experiments/buy_hold --silent
uv run qtrader backtest experiments/buy_hold --start-date 2020-01-01 --end-date 2020-12-31
```

### Backtest Options

| Option | Description | |--------|-------------| | `--silent, -s` | Silent mode (no event display) | | `--replay-speed, -r` | Replay speed (-1=silent, 0=instant, >0=delay) | | `--start-date` | Override start date (YYYY-MM-DD) | | `--end-date` | Override end date (YYYY-MM-DD) | | `--log-level, -l` | Log level (DEBUG, INFO, WARNING, ERROR) |

### View Results

Each run creates an isolated directory with full provenance:

```text
experiments/{experiment}/runs/{timestamp}/
├── run_manifest.json      # Run metadata (git, environment, status)
├── config_snapshot.yaml   # Configuration snapshot
├── performance.json       # Summary metrics (Sharpe, returns, drawdown)
├── report.html            # Visual performance report
└── timeseries/            # Time series data
```

## Available Examples

| Experiment | Description | |------------|-------------| | `buy_hold` | Simple buy and hold strategy | | `sma_crossover` | Moving average crossover strategy | | `weekly_monday_friday` | Weekly trading pattern strategy |

## CLI Reference

### Data Commands

```bash
# Update Yahoo data
uv run qtrader data yahoo-update                    # Update all symbols
uv run qtrader data yahoo-update AAPL MSFT          # Specific symbols
uv run qtrader data yahoo-update --full-refresh     # Force re-download

# List datasets
uv run qtrader data list

# Browse raw data
uv run qtrader data raw --symbol AAPL --dataset yahoo-us-equity-1d-csv

# Cache management
uv run qtrader data cache-info --dataset yahoo-us-equity-1d-csv
uv run qtrader data cache-clear --dataset yahoo-us-equity-1d-csv
```

### Project Commands

```bash
# Initialize new project
uv run qtrader init-project my-trading-system

# Initialize library components
uv run qtrader init-library ./library --type strategy
uv run qtrader init-library ./library --type indicator
uv run qtrader init-library ./library --type adapter
```

### Help

```bash
uv run qtrader --help
uv run qtrader backtest --help
uv run qtrader data --help
```

## Development

See [QTrader](https://github.com/QuantSpaceGit/QTrader)

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
