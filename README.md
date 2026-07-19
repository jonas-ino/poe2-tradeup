# PoE 2 Trade-Up
*Personal Project (in progress)*

Arbitrage tool for finding profitable currency exchange sequences in Path of Exile 2's economy. Item rates are set by players and are very sensitive to community trends, rising and falling based on primarily crafting and build trends.

## Stack
Python, Requests, Jupyter

## Problem
PoE 2 has no fixed exchange rates, and item value is based on what other players are willing to accept at any time. By trading items for different currencies, sequences of trades have the possibility to return more than the initial investment.

Finding these patterns requires checking hundreds of pairs and recording current rates, which are very sensitive to change at any moment. A tool which can do this quickly and automatically would allow for another profitable strategy for currency generation in a league, alongside item crafting and mapping.

The tool targets trade rates as given by the in-game Currency Exchange, as it allows for the fastest (and thus highest volume) trading, with no loading screens or player interaction.

## Data Sources
Several sources were considered:
- Official Trade Site (prices don't reflect in-game currency exchange prices)
- Official API currency-exchange history (requires API access - not possible currently)
- poe.ninja (only tracks the three most popular currencies: exalted, chaos, divine)
- mirrormarket.org (tracks historical prices for every currency item exchange pair - CHOSEN)

MirrorMarket returns OHLC (open, high, low, close) candlestick histories for item exchange pairs across provided intervals. MirrorMarket endpoints use the same ids as defined by the official documentation, making searches straightforward.
Trade volume is also provided, allowing noise (low-volume trades which are unlikely to still be up) to be filtered out.

## Current State
- Parses official item catalogue into 14 tradeable categories
- Creates a requests.Session client for connection reuse across multiple item pair queries (only one TLS handshake instead of a new one for each pair). Requests timeout and notify on bad requests (4xx/5xx status codes)
- price_history(have, want, interval, league) returns a full history for an item pair
- current_rate(have, want) returns the latest close price
- get_price(have, want) returns the current rate as a ratio

# Plans
- Planning multi-trade sequence search (A -> B -> C -> A) and compare final value fom the start to determine profitability.

## Running Instructions
- pip install requests
- run tradeup.ipynb jupyter notebook

## Limitations
- Rates are taken from recorded trades. If the close price is an outlier (eg. a low volume pair), prices may be inaccurate.
- No rate limiting. 