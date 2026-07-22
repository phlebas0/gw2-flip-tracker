# gw2-flip-tracker

a simple Guild Wars 2 (GW2) Trading Post (TP) flip tracker that uses the official GW2 API to track completed buy and sell transactions, match flips, and calculate profit after Trading Post fees.

## features

- tracks completed TP buys and sells
- matches buy orders with sell orders using first in, first out (FIFO)
- calculates profit after the 15% Trading Post cut
- handles partial quantities automatically
- stores transaction history locally with SQLite
- caches item names to avoid unnecessary API requests
- exports flip history to CSV
- filters and reviews completed flips

## profit calculation

profit is calculated as:

```text
net profit = (sell price × quantity × 0.85, rounded down) - (buy price × quantity)
```

the 0.85 multiplier accounts for the TP fees:

- 5% listing fee
- 10% sales tax

## requirements

- Python 3.10+
- requests

install dependencies:

```bash
pip install requests
```

## setup

1. run the application:

```bash
python gw2_flip_tracker.py
```

2. click `API Key` and enter a GW2 API key with the `tradingpost` permission. create an API key from:

```text
account.arena.net → My Account → Applications
```

4. press `Sync Now` to import your transaction history.

## usage

### `Sync Now`

downloads your latest completed Trading Post transactions from the GW2 API.

item names are cached locally, so only new item IDs need to be looked up.

### `Export CSV`

exports the current filtered flip list to a CSV file.

included columns:

- item
- quantity
- buy price
- sell price
- net profit
- buy date
- sell date

profitable flips are shown in green and losing flips are shown in red.

## how matching works

the GW2 API does not show which buy order was used for a specific sale, so flips are matched using FIFO.

example:

```text
buy 100 items at 1g
buy 100 items at 2g
sell 150 items

matched as:
100 from the first buy
50 from the second buy
```

this gives a reasonable estimate of profit but may not always reflect reality.

## limitations

- only completed transactions are tracked
- open buy and sell orders are ignored
- cancelled orders are not available through the GW2 API
- expired listings are not available through the GW2 API
- listing fees lost to cancellations cannot be calculated
- the GW2 API only keeps around 90 days of transaction history

## files

```text
gw2_flip_tracker.py  - main application
gw2_flips.db         - local SQLite database
gw2_config.json      - saved API key configuration
```

## license

MIT License
