from xbbg import blp
import pandas as pd

# Pick a single day for now, or loop through multiple days
date = '2025-07-26'
tickers = ['RELIANCE IS Equity', 'TCS IS Equity', 'INFY IS Equity', ...]

# Pull 1-min OHLCV data
intraday_data = {}
for ticker in tickers:
    df = blp.bdh(ticker, f"{date} 09:15:00", f"{date} 15:00:00", Bar='1Min')
    intraday_data[ticker] = df



opening_range = {}
for ticker, df in intraday_data.items():
    open_range = df.between_time('09:15:00', '09:30:00')
    high_15min = open_range['PX_HIGH'].max()
    low_15min = open_range['PX_LOW'].min()
    opening_range[ticker] = (high_15min, low_15min)


vwap_data = {}
for ticker, df in intraday_data.items():
    df = df.between_time('09:15:00', '09:30:00')
    vwap = (df['PX_LAST'] * df['VOLUME']).sum() / df['VOLUME'].sum()
    vwap_data[ticker] = vwap


signal_table = []

for ticker, df in intraday_data.items():
    post_open = df.between_time('09:30:00', '15:00:00')
    high_15, low_15 = opening_range[ticker]
    vwap = vwap_data[ticker]

    for time, row in post_open.iterrows():
        price = row['PX_LAST']
        if price > high_15 and price > vwap:
            signal_table.append((ticker, time, 'LONG', price))
            break  # First breakout
        elif price < low_15 and price < vwap:
            signal_table.append((ticker, time, 'SHORT', price))
            break


positions = pd.DataFrame(signal_table, columns=['Ticker', 'EntryTime', 'Side', 'EntryPrice'])

# Pull 3 PM prices
exit_prices = {}
for ticker, df in intraday_data.items():
    price_3pm = df.between_time('15:00:00', '15:01:00')['PX_LAST'].mean()
    exit_prices[ticker] = price_3pm

# Calculate P&L
def calculate_pnl(row):
    entry = row['EntryPrice']
    exit = exit_prices.get(row['Ticker'], None)
    if exit is None:
        return 0
    if row['Side'] == 'LONG':
        return (exit - entry) / entry
    else:
        return (entry - exit) / entry

positions['PnL'] = positions.apply(calculate_pnl, axis=1)


# Equal weight per day
avg_return = positions['PnL'].mean()
print(f"Day's Return: {avg_return:.2%}")

