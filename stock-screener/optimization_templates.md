# Optimization Templates — Mẫu code tối ưu hóa danh mục

## 1. Mean-Variance Optimization (Markowitz)

```python
import numpy as np
import pandas as pd
from scipy.optimize import minimize

def optimize_portfolio(returns_df, rf=0.04, target='max_sharpe',
                       min_weight=0.0, max_weight=0.3, max_sector_weight=0.4,
                       sector_map=None):
    """
    Tối ưu danh mục theo Markowitz.

    Parameters:
    - returns_df: DataFrame daily returns (columns = ticker, rows = dates)
    - rf: risk-free rate (lãi suất TPCP VN, ~4% cho 10Y)
    - target: 'max_sharpe' | 'min_variance' | 'target_return'
    - min_weight, max_weight: ràng buộc tỷ trọng mỗi mã
    - max_sector_weight: ràng buộc tỷ trọng mỗi ngành
    - sector_map: dict {ticker: sector_name}
    """
    mean_returns = returns_df.mean() * 252  # annualize
    cov_matrix = returns_df.cov() * 252
    n_assets = len(mean_returns)

    def portfolio_return(weights):
        return np.sum(weights * mean_returns)

    def portfolio_volatility(weights):
        return np.sqrt(np.dot(weights.T, np.dot(cov_matrix, weights)))

    def neg_sharpe(weights):
        ret = portfolio_return(weights)
        vol = portfolio_volatility(weights)
        return -(ret - rf) / vol

    # Ràng buộc
    constraints = [{'type': 'eq', 'fun': lambda w: np.sum(w) - 1}]

    # Ràng buộc ngành
    if sector_map:
        sectors = list(set(sector_map.values()))
        tickers = returns_df.columns.tolist()
        for sector in sectors:
            sector_indices = [i for i, t in enumerate(tickers) if sector_map.get(t) == sector]
            constraints.append({
                'type': 'ineq',
                'fun': lambda w, idx=sector_indices: max_sector_weight - sum(w[i] for i in idx)
            })

    bounds = tuple((min_weight, max_weight) for _ in range(n_assets))
    init_weights = np.array([1/n_assets] * n_assets)

    if target == 'max_sharpe':
        result = minimize(neg_sharpe, init_weights, method='SLSQP',
                          bounds=bounds, constraints=constraints)
    elif target == 'min_variance':
        result = minimize(portfolio_volatility, init_weights, method='SLSQP',
                          bounds=bounds, constraints=constraints)

    optimal_weights = result.x
    opt_return = portfolio_return(optimal_weights)
    opt_vol = portfolio_volatility(optimal_weights)
    opt_sharpe = (opt_return - rf) / opt_vol

    return {
        'weights': dict(zip(returns_df.columns, np.round(optimal_weights, 4))),
        'expected_return': round(opt_return, 4),
        'volatility': round(opt_vol, 4),
        'sharpe_ratio': round(opt_sharpe, 4)
    }
```

## 2. Risk Parity

```python
from scipy.optimize import minimize
import numpy as np

def risk_parity_portfolio(returns_df):
    """
    Tối ưu danh mục theo Risk Parity — mỗi mã đóng góp rủi ro bằng nhau.
    """
    cov_matrix = returns_df.cov() * 252
    n_assets = len(returns_df.columns)
    target_risk = 1.0 / n_assets  # mỗi mã đóng góp 1/n rủi ro

    def risk_budget_objective(weights):
        port_vol = np.sqrt(np.dot(weights.T, np.dot(cov_matrix, weights)))
        marginal_contrib = np.dot(cov_matrix, weights)
        risk_contrib = weights * marginal_contrib / port_vol
        risk_contrib_pct = risk_contrib / port_vol
        # Minimize sai lệch risk contribution so với target
        return sum((rc - target_risk)**2 for rc in risk_contrib_pct)

    constraints = [{'type': 'eq', 'fun': lambda w: np.sum(w) - 1}]
    bounds = tuple((0.01, 0.5) for _ in range(n_assets))
    init_weights = np.array([1/n_assets] * n_assets)

    result = minimize(risk_budget_objective, init_weights, method='SLSQP',
                      bounds=bounds, constraints=constraints)

    return dict(zip(returns_df.columns, np.round(result.x, 4)))
```

## 3. Backtest Engine

```python
import pandas as pd
import numpy as np

def backtest_portfolio(prices_df, weights, initial_capital=1_000_000_000,
                       rebalance_freq='Q', transaction_cost=0.003):
    """
    Backtest danh mục với rebalance định kỳ.

    Parameters:
    - prices_df: DataFrame giá đóng cửa (columns = ticker, rows = dates)
    - weights: dict {ticker: weight}
    - initial_capital: vốn ban đầu (VND)
    - rebalance_freq: 'M' (monthly), 'Q' (quarterly), 'Y' (yearly)
    - transaction_cost: phí giao dịch 2 chiều (~0.3% cho VN)
    """
    returns_df = prices_df.pct_change().dropna()
    tickers = list(weights.keys())
    w = np.array([weights[t] for t in tickers])

    # Xác định ngày rebalance
    rebalance_dates = returns_df.resample(rebalance_freq).last().index

    portfolio_value = [initial_capital]
    current_weights = w.copy()
    total_turnover = 0

    for i in range(len(returns_df)):
        date = returns_df.index[i]
        daily_returns = returns_df.loc[date, tickers].values

        # Cập nhật giá trị
        new_value = portfolio_value[-1] * (1 + np.sum(current_weights * daily_returns))

        # Rebalance nếu đến ngày
        if date in rebalance_dates:
            # Tính drift weights (sau khi giá thay đổi)
            drifted = current_weights * (1 + daily_returns)
            drifted = drifted / drifted.sum()
            turnover = np.sum(np.abs(drifted - w)) / 2
            total_turnover += turnover
            # Trừ phí giao dịch
            new_value *= (1 - turnover * transaction_cost)
            current_weights = w.copy()
        else:
            # Drift tự nhiên
            drifted = current_weights * (1 + daily_returns)
            current_weights = drifted / drifted.sum()

        portfolio_value.append(new_value)

    portfolio_value = pd.Series(portfolio_value[1:], index=returns_df.index)

    # Tính metrics
    total_return = (portfolio_value.iloc[-1] / initial_capital) - 1
    n_years = len(returns_df) / 252
    ann_return = (1 + total_return) ** (1/n_years) - 1
    ann_vol = portfolio_value.pct_change().std() * np.sqrt(252)
    sharpe = (ann_return - 0.04) / ann_vol  # rf = 4%

    # Max drawdown
    rolling_max = portfolio_value.cummax()
    drawdown = (portfolio_value - rolling_max) / rolling_max
    max_dd = drawdown.min()

    return {
        'portfolio_value': portfolio_value,
        'total_return': round(total_return, 4),
        'annualized_return': round(ann_return, 4),
        'annualized_volatility': round(ann_vol, 4),
        'sharpe_ratio': round(sharpe, 4),
        'max_drawdown': round(max_dd, 4),
        'calmar_ratio': round(ann_return / abs(max_dd), 4) if max_dd != 0 else None,
        'total_turnover': round(total_turnover, 4),
    }
```

## 4. Rebalance Calculator

```python
def calculate_rebalance(current_holdings, target_weights, current_prices, lot_size=100):
    """
    Tính giao dịch rebalance cụ thể.

    Parameters:
    - current_holdings: dict {ticker: số_cổ_phiếu}
    - target_weights: dict {ticker: tỷ_trọng_mục_tiêu}
    - current_prices: dict {ticker: giá_hiện_tại}
    - lot_size: lô giao dịch (HOSE = 100)

    Returns: danh sách giao dịch cần thực hiện
    """
    # Tính tổng giá trị danh mục
    total_value = sum(qty * current_prices.get(t, 0) for t, qty in current_holdings.items())

    trades = []
    total_buy_cost = 0
    total_sell_proceeds = 0

    all_tickers = set(list(current_holdings.keys()) + list(target_weights.keys()))

    for ticker in sorted(all_tickers):
        current_qty = current_holdings.get(ticker, 0)
        target_pct = target_weights.get(ticker, 0)
        price = current_prices.get(ticker, 0)

        if price == 0:
            continue

        current_value = current_qty * price
        target_value = total_value * target_pct
        diff_value = target_value - current_value
        diff_qty = int(diff_value / price)

        # Làm tròn theo lô
        diff_qty = (diff_qty // lot_size) * lot_size

        if diff_qty > 0:
            action = 'MUA'
            cost = diff_qty * price * 1.0015  # + phí mua 0.15%
            total_buy_cost += cost
        elif diff_qty < 0:
            action = 'BÁN'
            proceeds = abs(diff_qty) * price * 0.9975  # - phí bán 0.15% - thuế 0.1%
            total_sell_proceeds += proceeds
        else:
            continue

        trades.append({
            'ticker': ticker,
            'action': action,
            'quantity': abs(diff_qty),
            'price': price,
            'value': abs(diff_qty * price),
            'current_weight': round(current_value / total_value * 100, 2),
            'target_weight': round(target_pct * 100, 2),
        })

    return {
        'trades': trades,
        'total_buy_cost': total_buy_cost,
        'total_sell_proceeds': total_sell_proceeds,
        'net_cash_needed': total_buy_cost - total_sell_proceeds,
        'estimated_fees': total_buy_cost * 0.0015 + total_sell_proceeds * 0.0025,
    }
```
