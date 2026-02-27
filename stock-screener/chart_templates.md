# Chart Templates — Mẫu biểu đồ phân tích kỹ thuật

Tham khảo các mẫu code dưới đây khi tạo biểu đồ phân tích. Điều chỉnh theo dữ liệu thực tế.

## 1. Biểu đồ Candlestick + Volume + MA

```python
import matplotlib.pyplot as plt
import matplotlib.dates as mdates
from mplfinance.original_flavor import candlestick_ohlc
import pandas as pd
import numpy as np

def plot_candlestick_with_indicators(df, symbol, ma_periods=[20, 50, 200]):
    """
    df cần có columns: Date, Open, High, Low, Close, Volume
    """
    fig, (ax1, ax2, ax3, ax4) = plt.subplots(4, 1, figsize=(14, 12),
        gridspec_kw={'height_ratios': [4, 1, 1, 1]}, sharex=True)
    fig.suptitle(f'Phân tích kỹ thuật: {symbol}', fontsize=14, fontweight='bold')

    # --- Candlestick + MA ---
    df_ohlc = df[['Date','Open','High','Low','Close']].copy()
    df_ohlc['Date'] = df_ohlc['Date'].map(mdates.date2num)
    candlestick_ohlc(ax1, df_ohlc.values, width=0.6, colorup='green', colordown='red')

    for period in ma_periods:
        ma = df['Close'].rolling(period).mean()
        ax1.plot(df['Date'], ma, label=f'MA{period}', linewidth=1)
    ax1.legend(loc='upper left', fontsize=8)
    ax1.set_ylabel('Giá (VND)')
    ax1.grid(True, alpha=0.3)

    # --- Volume ---
    colors = ['green' if c >= o else 'red' for c, o in zip(df['Close'], df['Open'])]
    ax2.bar(df['Date'], df['Volume'], color=colors, alpha=0.7)
    ax2.set_ylabel('KL')

    # --- RSI ---
    rsi = compute_rsi(df['Close'], 14)
    ax3.plot(df['Date'], rsi, color='purple', linewidth=1)
    ax3.axhline(y=70, color='red', linestyle='--', alpha=0.5)
    ax3.axhline(y=30, color='green', linestyle='--', alpha=0.5)
    ax3.set_ylabel('RSI(14)')
    ax3.set_ylim(0, 100)

    # --- MACD ---
    macd_line, signal_line, histogram = compute_macd(df['Close'])
    ax4.plot(df['Date'], macd_line, label='MACD', color='blue', linewidth=1)
    ax4.plot(df['Date'], signal_line, label='Signal', color='orange', linewidth=1)
    colors_hist = ['green' if h >= 0 else 'red' for h in histogram]
    ax4.bar(df['Date'], histogram, color=colors_hist, alpha=0.5)
    ax4.set_ylabel('MACD')
    ax4.legend(loc='upper left', fontsize=8)

    ax4.xaxis.set_major_formatter(mdates.DateFormatter('%m/%Y'))
    plt.tight_layout()
    return fig


def compute_rsi(series, period=14):
    delta = series.diff()
    gain = (delta.where(delta > 0, 0)).rolling(window=period).mean()
    loss = (-delta.where(delta < 0, 0)).rolling(window=period).mean()
    rs = gain / loss
    return 100 - (100 / (1 + rs))


def compute_macd(series, fast=12, slow=26, signal=9):
    ema_fast = series.ewm(span=fast).mean()
    ema_slow = series.ewm(span=slow).mean()
    macd_line = ema_fast - ema_slow
    signal_line = macd_line.ewm(span=signal).mean()
    histogram = macd_line - signal_line
    return macd_line, signal_line, histogram
```

## 2. Radar Chart cho đánh giá (dùng với Stock Evaluation)

```python
import matplotlib.pyplot as plt
import numpy as np

def plot_radar_chart(scores, categories, symbol, max_scores=None):
    """
    scores: dict hoặc list điểm số
    categories: list tên trụ cột
    max_scores: điểm tối đa mỗi trụ cột (để normalize)
    """
    if max_scores:
        values = [s/m * 100 for s, m in zip(scores, max_scores)]
    else:
        values = list(scores)

    N = len(categories)
    angles = [n / float(N) * 2 * np.pi for n in range(N)]
    values += values[:1]
    angles += angles[:1]

    fig, ax = plt.subplots(figsize=(8, 8), subplot_kw=dict(polar=True))
    ax.plot(angles, values, 'o-', linewidth=2)
    ax.fill(angles, values, alpha=0.25)
    ax.set_thetagrids([a * 180/np.pi for a in angles[:-1]], categories)
    ax.set_ylim(0, 100)
    ax.set_title(f'Đánh giá tổng hợp: {symbol}', fontsize=14, fontweight='bold', y=1.08)
    ax.grid(True)
    return fig
```

## 3. Correlation Heatmap (dùng với Portfolio Allocation)

```python
import matplotlib.pyplot as plt
import seaborn as sns
import pandas as pd

def plot_correlation_heatmap(returns_df):
    """
    returns_df: DataFrame với cột là mã cổ phiếu, hàng là ngày, giá trị là daily return
    """
    corr = returns_df.corr()
    fig, ax = plt.subplots(figsize=(10, 8))
    sns.heatmap(corr, annot=True, fmt='.2f', cmap='RdYlGn_r',
                center=0, vmin=-1, vmax=1, ax=ax,
                square=True, linewidths=0.5)
    ax.set_title('Ma trận tương quan', fontsize=14, fontweight='bold')
    plt.tight_layout()
    return fig
```

## 4. Efficient Frontier (dùng với Portfolio Allocation)

```python
import numpy as np
import matplotlib.pyplot as plt
from scipy.optimize import minimize

def plot_efficient_frontier(returns_df, rf=0.04, n_portfolios=5000):
    """
    returns_df: DataFrame daily returns cho mỗi mã
    rf: risk-free rate (lãi suất TPCP VN)
    """
    mean_returns = returns_df.mean() * 252
    cov_matrix = returns_df.cov() * 252
    n_assets = len(mean_returns)

    # Monte Carlo simulation
    results = np.zeros((3, n_portfolios))
    weights_record = []
    for i in range(n_portfolios):
        weights = np.random.random(n_assets)
        weights /= np.sum(weights)
        weights_record.append(weights)
        port_return = np.sum(weights * mean_returns)
        port_vol = np.sqrt(np.dot(weights.T, np.dot(cov_matrix, weights)))
        results[0,i] = port_vol
        results[1,i] = port_return
        results[2,i] = (port_return - rf) / port_vol  # Sharpe

    fig, ax = plt.subplots(figsize=(10, 7))
    scatter = ax.scatter(results[0,:], results[1,:], c=results[2,:],
                         cmap='viridis', marker='o', s=10, alpha=0.3)
    plt.colorbar(scatter, label='Sharpe Ratio')

    # Max Sharpe portfolio
    max_sharpe_idx = np.argmax(results[2])
    ax.scatter(results[0,max_sharpe_idx], results[1,max_sharpe_idx],
               marker='*', color='red', s=300, label='Max Sharpe')

    # Min Variance portfolio
    min_vol_idx = np.argmin(results[0])
    ax.scatter(results[0,min_vol_idx], results[1,min_vol_idx],
               marker='*', color='blue', s=300, label='Min Variance')

    ax.set_xlabel('Volatility (Rủi ro)')
    ax.set_ylabel('Expected Return (Lợi suất kỳ vọng)')
    ax.set_title('Đường biên hiệu quả (Efficient Frontier)', fontsize=14, fontweight='bold')
    ax.legend()
    ax.grid(True, alpha=0.3)
    return fig, weights_record[max_sharpe_idx], weights_record[min_vol_idx]
```
