#### What user can choose
- Choose **trading mode**: Conservative / Balanced / Aggressive
- Provide preferences: industries, risk tolerance, etc.

#### Mathematical Model (Rule-based, Statistical)
For assets that behave more predictably or where traditional finance still dominates

- Stocks / ETFs / Bonds
	- use **Modern Portfolio Theory** (**MPT**) for risk-return optimization.
	- **Apply Sharpe Ratio** / **CAPM** / **Efficient Frontier** to balance assets.
	- For price prediction: **ARIMA** / **Exponential Smoothing** for time-series analysis
	- Incorporate technical indicators (RSI, MACD, moving average)

#### Machine Learning Models
For assets where data is noisy, nonlinear or sentiment-driven
- Cryptocurrency / volatile stocks
	- Use **LSTM / GRU (deep learning)** or **==XGBoost== / Random Forest** for price trend prediction.
	- Use **Sentiment analysis on news/Twitter** for direction prediction.
	- Combine model output with mathematical thresholds (e.g., only act if confidence > threshold).

#### Decision Explanation Engine
- After computing suggestions (Buy / Sell / Hold):
    - Show **reason for decision** — e.g.,
==“RSI < 30 → Oversold. LSTM model predicts +7% growth next week.”==  
==“Recent positive sentiment from news. Predicted upward trend.”==

#### User Feedback Learning
- Track user actions: which suggestions they followed, their outcomes.
- Create a **reinforcement or feedback model**:
    - If user prefers riskier assets, adapt to suggest slightly riskier options next time.
    - Use a simple **weight update system** (not full RL, but adaptive scoring).

#### News & Sentiment Integration
- Use APIs like **NewsAPI**, **Twitter API (or alternatives)**.
- Apply **BERT / FinBERT** for sentiment extraction.
- Integrate this as an additional signal for ML models or risk adjustment.
