# Fixed-Income-Volatility-Regime-Analysis-Python-


data sources (FRED + Yahoo Finance)

FRED (Federal Reserve Economic Data)

Series

DGS2 → 2-Year Treasury Constant Maturity Rate
DGS5 → 5-Year Treasury Constant Maturity Rate
DGS10 → 10-Year Treasury Constant Maturity Rate

Website: https://fred.stlouisfed.org

Maintained by: Federal Reserve Bank of St. Louis

https://www.newyorkfed.org/markets/reference-rates/effr
https://www.tradingview.com/symbols/FRED-FEDFUNDS/


Yahoo Finance via yfinance
https://finance.yahoo.com
https://ranaroussi.github.io/yfinance/
https://pypi.org/project/yfinance/

methodology (EWMA, rolling corr, regime definition)


1. Pick assets + date range

Rates: 2Y/5Y/10Y Treasury yields (FRED: DGS2, DGS5, DGS10). I used constant-maturity Treasury yields from FRED since they’re the standard macro reference for rates, monetary policy expectations, and curve dynamics. Used for its large database of economic indicators for macroeconomic signal generation. Pulled for Python via its API in this project.

FX: UUP Since official DXY isn’t freely distributed, I used UUP as a traded proxy for true dollar strength.


# Liquid proxies from Yahoo Finance
# Equities: SPY (S&P 500) I used SPY because it provides a clean, tradable proxy for U.S. equity risk and integrates naturally into cross-asset portfolios.
# Gold: GLD (risk-off/real rates proxy) I use GLD as a risk-off and inflation hedge and USO for cyclical commodity exposure tied to growth and inflation regimes
# Dollar: UUP (tradable USD index proxy) OR EURUSD=X (spot)
# Yahoo assets
YF_TICKERS = ["SPY", "QQQ", "HYG", "EEM", "TLT", "BTC-USD"]
I selected liquid, widely-used proxies across rates, FX, equities, and commodities so that regime shifts, correlation dynamics, and volatility clustering reflect real macro behavior rather than idiosyncratic asset noise.

Downloaded data using pandas_datareader for FRED (rates), yfinance for ETFs.

Processed data to align on business days.

Computed returns

For prices: log returns.

For yields: used daily changes in yield.

Volatility estimates

Rolling volatility: std(returns) * sqrt(252)

EWMA volatility: RiskMetrics recursive style

Computed annual volatility (rolling (20/60/120) day and EWMA) and drawdowns plotted for SPY.


Compute rolling correlation matrix for 60d and labeled "risk-on/off" regime proxy


Risk-off when equity–bond correlation becomes strongly negative and vol is high, or when cross-asset correlations converge (average off-diagonal correlation rises).


Regime-conditioned Monte Carlo

Split history by regime label.

Estimated matrix (μ, Σ) for each regime.
Simulated N paths over horizon H (20d/60d/120d) , and computed distribution of max drawdown and worst percentile P&L (tail risk / VaR / ES style summaries) of SPY

# Computed statistics such as,
top 5 worst drawdown dates

regime transition dates

MC VaR/ES for each regime

# Multi-asset Regime labeling
Created multi-asset risk basket of YF_TICKERS and plotted against REGIME + volatility targets

# Trading use: Regime + Vol Target Overlay on SPY only
Created signal --> risk control --> evaluation Regime + Volatility  Target Overlay on SPY only
risk-off = high risk-basket vol + (flight-to-quality or correlation stress or vol spike)


TODO: add backtest report

## results screenshots
<img width="1136" height="528" alt="image" src="https://github.com/user-attachments/assets/7efaafb3-7a0e-45f7-8879-402fcb8de13f" />
Figure 1: Volatility and Drawdowns (SPY only)

<img width="272" height="129" alt="Screenshot 2025-12-29 at 1 00 56 AM" src="https://github.com/user-attachments/assets/5ded56ef-2c42-4538-9a3d-bda043db1fa5" />
Figure 2: Top 5 Worst Drawdowns (SPY only)

<img width="547" height="435" alt="image" src="https://github.com/user-attachments/assets/43940b7b-652d-4eab-8318-6957189a8bae" />
<img width="845" height="210" alt="Screenshot 2025-12-29 at 1 13 10 AM" src="https://github.com/user-attachments/assets/9e5313c5-e3a3-4c5a-a2f6-8f8a29303533" />
<img width="556" height="435" alt="image" src="https://github.com/user-attachments/assets/90ffe30d-70f8-4927-bd1f-365fb262120e" />

Figure 3: Regime labeling
Figure 4: Regime counts and regime-conditioned Monte Carlo
Figure 5: Risk Overlay Equity Curve

<img width="1152" height="487" alt="Screenshot 2025-12-29 at 1 16 00 AM" src="https://github.com/user-attachments/assets/5a59d86e-737a-4074-9f20-5deadfa0bc87" />

Figure 6: Multi-asset Regime labeling

<img width="336" height="499" alt="Screenshot 2025-12-29 at 1 16 27 AM" src="https://github.com/user-attachments/assets/5e80b02c-f4b5-43db-91b7-298145bdfbfd" />
Figure 7: Regime Transition Dates

<img width="645" height="630" alt="Screenshot 2025-12-29 at 1 18 00 AM" src="https://github.com/user-attachments/assets/2ab3d8cd-53fe-4a41-970b-0a1107defdeb" />
<img width="1049" height="510" alt="Screenshot 2025-12-29 at 1 18 20 AM" src="https://github.com/user-attachments/assets/16073314-8665-4d51-a5c0-9499941312d3" />

Figure 8: Trading Use Regime-conditioned Monte Carlo + VaR / ES --> on SPY only and risk-basket
