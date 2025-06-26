# Overview

Welcome to my algorithmic trading project, inspired by the [QuantProgram YouTube tutorial](https://www.youtube.com/watch?v=GDMkkmkJigw&t=52s). This project explores algorithmic trading using Python, starting with a simple moving average strategy and evolving into a multi-asset backtest system. It served as a practical way to learn how trading strategies are built, tested, and evaluated.

Throughout the process, I completed a series of challenges that introduced concepts like risk-adjusted returns, drawdown analysis, portfolio diversification, and the full implementation of the Stan Weinstein strategy. The result is a clear, step-by-step progression from basic signal generation to advanced portfolio-level backtesting.

## About This Project & My Learning Journey

**Please Note:** This project was completed as an intensive self-learning endeavor to gain practical experience in Python and algorithmic trading. **I am currently a beginner in Python, with my formal intensive course starting soon.** Everything implemented here was learned independently, driven by a strong desire to understand quantitative finance and apply programming skills.

While the trading strategies demonstrate core concepts, they are not yet optimized for real-world trading and are a foundation upon which I plan to build as I deepen my knowledge. This project showcases my commitment to self-education and my rapid learning capabilities in a new domain.
# The Challenges
### Challenge 1 – Risk and Reward Analysis  
**What was the goal?**  
We were challenged to download 20 random stocks, calculate their **risk and reward potential**, compare **covariance and correlation**, and identify the **5 best long-term investment candidates**.

---

### Challenge 2 – Moving Averages Visualization  
**What was the goal?**  
Visualize the price action of the **top 5 selected stocks**, overlaying their **50-day and 200-day moving averages** to analyze trend strength and long-term momentum.

---

### Challenge 3 – Strategy Optimization & Portfolio Selection  
**What was the goal?**  
Backtest multiple **SMA windows (10 to 200)** to find the optimal one for each stock, implement a **bullish bias strategy** using **SMA crossovers (50/200)**, compare strategy **drawdowns and volatility**, and select the **top 3 stocks** based on **return-to-drawdown ratio**.

---

### Challenge 4 – Stan Weinstein Strategy  
**What was the goal?**  
Implement and backtest the full **Stan Weinstein strategy** using weekly data, applying logic based on:

- 30-week SMA  
- Volume spike confirmation  
- 52-week high breakout  
- Relative strength vs. SPY

# Tools I Used

To build and analyze my algorithmic trading strategies, I used the following tools:

### Programming & Libraries

- **Python** – Main language for analysis and strategy logic
  - **Pandas** – For data manipulation and time series handling
  - **NumPy** – For numerical calculations and performance metrics
  - **Matplotlib** – For visualizing strategies and performance
  - **Seaborn** – For enhanced statistical charts and heatmaps

### Data

- **yfinance** – To download historical weekly and daily stock data

### Development Environment

- **Jupyter Notebook** – For writing and running code interactively
- **Visual Studio Code** – My main IDE for project management and execution
- **Git & GitHub** – For version control, documentation, and sharing the project

# The Analysis

Each notebook in this project was focused on exploring a specific aspect of strategy development and evaluation. Here's how I approached each challenge:

## Challenge 1 – Risk and Reward Analysis

I started this challenge by downloading 20 large-cap stocks using the `yfinance` API. To compare them on equal footing, I normalized the stock prices so they all started at 100. This allowed me to visually track how each stock performed relative to the others over time.

View my notebook with detailed steps here: [challenge_1](challenge_1.ipynb).

###  Top 5 Performing Stocks (Normalized)

After normalizing the prices, I calculated each stock's overall performance and plotted the top 5 performers.

![Top 5 Performing Stocks](top_5_performing_stocks.png)


---

Next, I calculated the **mean return** and **standard deviation** (volatility) of daily returns for all stocks. These were then annualized to represent yearly performance. I visualized this relationship with a **risk-return scatter plot**, labeling each stock.

###  Risk vs. Reward (Annualized Mean vs. Volatility)

![Risk Return](risk_return.png)

---

To factor in both return and risk, I computed the **Sharpe Ratio** for each stock (assuming a 2% risk-free rate). Then I sorted the top 10 stocks based on their Sharpe Ratios and plotted them as a horizontal bar chart.

###  Top 10 Stocks by Sharpe Ratio

![Top 10 Stocks with best Sharpe Ratio](sharpe_ratio_top10.png)

---

Finally, I compared the **covariance** and **correlation** matrices of the top stocks' returns. This helped me understand how these stocks move together — both in magnitude (covariance) and direction (correlation).

###  Covariance and Correlation Matrices

![Covariance and Correlation](covariance_correlation.png)

---

Based on these metrics, I designed a **selection method** for building a long-term portfolio:  
- Start with the stock with the highest Sharpe Ratio  
- Iteratively add stocks only if their correlation with already-selected stocks is **below 0.5**  
- Repeat until 5 diversified and high-performing stocks are selected

This approach balanced **risk-adjusted performance** with **diversification potential**, resulting in a more resilient and efficient portfolio.

###  Final Selected Stocks:

- **AAPL** – Strong Sharpe ratio, growth stock in the Technology sector with consistent long-term performance and innovation-driven returns.

- **HD** – Retail giant in the Consumer Discretionary sector, benefiting from steady U.S. housing market demand and known for strong fundamentals and dividend growth.

- **V** – Global leader in digital payments (Financials sector), offering a scalable, high-margin business model with stable cash flow and low volatility.

- **LMT** – Major player in the Defense and Aerospace industry (Industrials), offering resilience in volatile markets due to government-backed contracts and defensive positioning.

- **NEE** – Renewable energy and utilities leader (Utilities sector), combining stable dividends with long-term sustainability-focused growth.


These 5 companies span multiple industries and exhibit **both strong individual performance** and **low interdependence**, making them solid candidates for long-term investing.

###  Key Insights

- **Performance isn't everything**: Some of the top-performing stocks had high volatility, which made their risk-adjusted return (Sharpe Ratio) less attractive.
- **Sharpe Ratio is a better filter**: By factoring in both return and volatility, Sharpe Ratio revealed more consistent performers that weren't necessarily the highest gainers.
- **Diversification matters**: Many high-performing stocks were highly correlated with each other, which increases portfolio risk. The correlation matrix helped identify which stocks offered independent behavior.
- **Covariance is harder to interpret**: While useful, raw covariance values depend on the scale of each stock, making the **correlation matrix** more intuitive for portfolio construction.
- **Systematic selection beats guesswork**: Using a correlation threshold (< 0.5) to filter from the top Sharpe stocks gave a reliable, data-driven method to build a diversified long-term portfolio.



## Challenge 2 – Moving Averages Visualization

Challenge 2 was a straightforward one: visualize the **50-day** and **200-day** simple moving averages (SMA) for the 5 selected stocks from Challenge 1. These moving averages are commonly used to identify short- and long-term trends in financial markets.

For each stock, I overlaid the SMAs on the price chart to observe whether the stock was in an uptrend (price above both SMAs) or showing signs of consolidation or weakness.

View my notebook with detailed steps here: [challenge_2](challenge_2.ipynb).

####  Price with 50-day and 200-day Moving Averages

### Visualize Data
```python
for ticker in tickers:
    plt.figure(figsize=(12, 5))
    plt.plot(close[ticker], label=f'{ticker} Close', color='black', linewidth=1)
    plt.plot(sma50[ticker], label='50-Day SMA', color='blue', linestyle='--')
    plt.plot(sma200[ticker], label='200-Day SMA', color='red', linestyle='--')
    
    plt.title(f'{ticker} Price with 50 & 200-Day Moving Averages')
    plt.xlabel('Date')
    plt.ylabel('Price')
    plt.legend()
    plt.grid(True)
    plt.tight_layout()
    plt.show()
```

![appl](AAPL_moving_average.png)
![hd](HD_moving_average.png)
![lmt](LMT_moving_average.png)
![nee](NEE_moving_average.png)
![v](v_moving_average.png)
---

###  Key Insights

- The 50- and 200-day SMAs provide a **clear visual of long-term trend shifts**.
- While most of the selected stocks showed **strong uptrends at times**, there were also periods where the price **dipped below one or both SMAs**, signaling corrections or consolidations.
- This step confirmed that although past performance was strong, **timing matters**, and **not all moments are ideal for entry** based solely on long-term performance.


## Challenge 3 – Strategy Optimization & Portfolio Selection

This challenge moved us from historical analysis into building a real trading strategy, optimizing it, evaluating risk, and finalizing a top-performing stock portfolio.

View my notebook with detailed steps here: [challenge_3](challenge_3.ipynb).

---

###  What I Did

####  Found the Best SMA Window
For each of the 5 stocks, I tested all SMA windows from 10 to 200. The strategy was simple: be in the market only when the stock is trading above its SMA. I selected the window that yielded the highest cumulative return for each stock.

| Ticker | Best SMA | Cumulative Return |
| ------ | -------- | ----------------- |
| AAPL   | 19       | 39.02             |
| HD     | 187      | 10.74             |
| LMT    | 91       | 3.75              |
| NEE    | 191      | 2.96              |
| V      | 200      | 6.47              |


###  Built a Bias Strategy with SMA 50/200
I introduced a trend bias filter using a 50-day vs 200-day SMA crossover. The strategy only traded when 50 SMA > 200 SMA, to avoid entering during bearish periods. This reduced noise and improved robustness.
| Ticker | Buy & Hold Return | Strategy Return | B\&H Volatility | Strategy Volatility |
| ------ | ----------------- | --------------- | --------------- | ------------------- |
| AAPL   | 21.61%            | 15.86%          | 0.309           | 0.237               |
| HD     | 10.37%            | 2.86%           | 0.264           | 0.201               |
| LMT    | 3.76%             | 2.00%           | 0.236           | 0.172               |
| NEE    | 3.56%             | 1.96%           | 0.247           | 0.183               |
| V      | 12.53%            | 8.67%           | 0.287           | 0.212               |


### Compared Max Drawdowns
After analyzing returns and volatility separately, I focused on **maximum drawdown** — the worst peak-to-trough loss each stock experienced. This gave a clearer picture of downside risk for both Buy & Hold and the SMA strategy.

| Ticker | Buy & Hold Max Drawdown | Strategy Max Drawdown |
|--------|--------------------------|------------------------|
| AAPL   | 88.75%                   | 60.90%                 |
| HD     | 50.55%                   | 47.79%                 |
| LMT    | 70.46%                   | 48.89%                 |
| NEE    | 60.01%                   | 44.06%                 |
| V      | 73.20%                   | 45.20%                 |



###  Selected Top 3 Stocks for Portfolio
Using a simple but effective scoring system — Strategy Return / Max Drawdown — I ranked the 5 stocks and picked the Top 3 with the best risk-adjusted performance.

| Ticker | Return-to-Drawdown Ratio |
| ------ | ------------------------ |
| AAPL   | 26.04                    |
| V      | 19.18                    |
| HD     | 5.98                     |


---

###  Key Insights

- **Exploring SMA optimization builds intuition**  
  Testing dozens of moving average windows helped me understand how sensitive strategies are to parameter tuning — and how different assets behave in trends.

- **Implementing strategy filters deepens market understanding**  
  Building a bias filter using SMA crossovers showed how we can avoid poor market conditions and make strategies more robust through simple logic.

- **Quantifying performance teaches real-world tradeoffs**  
  Comparing volatility, drawdowns, and cumulative returns reinforced how every trading strategy involves balancing risk and reward — not just chasing profits.

- **Drawdown analysis reveals psychological pressure points**  
  Measuring the worst-case scenarios was a valuable reminder that returns alone aren’t enough — investors must survive the tough periods.

- **Scoring systems improve decision-making**  
  Ranking stocks by return-to-drawdown ratio taught me how to prioritize based on both performance and resilience, mimicking real portfolio construction.

- **This challenge bridged theory with practice**  
  More than just coding exercises, this challenge simulated how real traders think through strategy design, filtering, and risk-adjusted evaluation.


##  Challenge 4 – Implement the Stan Weinstein Strategy

This challenge was focused on building a classic trend-following strategy from scratch: the **Stan Weinstein Strategy**. The goal was to fully code the logic behind Weinstein’s stage analysis approach and evaluate how it performs across both a single stock and a full portfolio.

View my notebook with detailed steps here: [challenge_4](challenge_4.ipynb).

---

###  Key Concepts from the Stan Weinstein Strategy

| Component              | Description |
|------------------------|-------------|
| ** 30-Week SMA**     | Trend filter — go long when price is above the 30-week SMA. |
| **🔄 Market Stages**   | Use price structure to identify market stages (1–4) and trade only during Stage 2 (breakout uptrend). |
| **📈 Relative Strength** | Compare each stock to SPY — only trade if outperforming. |
| **🔊 Volume Confirmation** | Breakouts must happen on higher-than-average volume to avoid false signals. |
| **📌 52-Week High Filter** | Additional confirmation: price must break out above its previous 52-week high. |

---

###  What I Did

####  Step-by-step Implementation:
1. Fetched **weekly data** from Yahoo Finance for both a single stock (e.g., MSFT) and 20 large-cap stocks.
2. Calculated indicators: 30-week SMA, relative strength vs SPY, 52-week high, and volume spike filter.
3. Defined entry and exit conditions using all components above.
4. Created a custom backtest logic to:
   - Track trades
   - Manage a portfolio with max 5 open positions at a time
   - Allocate 20% capital per trade
5. Compared strategy performance vs SPY benchmark using cumulative return, Sharpe ratio, and drawdown.

---

###  Single-Stock Strategy Backtest

We first tested the strategy on a single ticker (`MSFT`) and compared it to Buy & Hold:
![msft](MSFT_spy.png)

| Metric                      | Strategy       | Buy & Hold    |
|-----------------------------|----------------|---------------|
| Cumulative Return           | 177.61%        | 380.59%       |
| Annualized Return           | 10.65%         | 17.02%        |
| Annualized Volatility       | 16.64%         | 23.50%        |
| Sharpe Ratio                | 0.64           | 0.72          |
| Max Drawdown                | -33.04%        | -48.63%       |
| Total Trades                | 8              | -             |
| Win Rate                    | 75.00%         | -             |

---

###  Portfolio Backtest (20 Stocks)

Then we scaled the strategy to a **20-stock portfolio**. It only entered trades with strong breakouts, confirmed by volume and relative strength, while managing position sizing.
![msft](portfolio_spy.png)

| Metric                      | Strategy Portfolio |
|-----------------------------|--------------------|
| Initial Capital             | $100,000           |
| Final Portfolio Value       | $317,231           |
| Cumulative Return           | 217.23%            |
| Annualized Return           | 12.37%             |
| Annualized Volatility       | 13.21%             |
| Sharpe Ratio                | 0.94               |
| Max Drawdown                | -22.27%            |
| Total Trades                | 47                 |
| Win Rate                    | 63.83%             |

---

###  Key Insights

- This project taught the full end-to-end process of developing a professional-grade strategy using historical price, volume, and relative performance data.
- Implementing market stage logic helps filter out sideways or risky environments.
- Combining **trend**, **volume**, **momentum**, and **breakout filters** makes the strategy more robust.
- Risk-adjusted returns (Sharpe ratio) were strong, especially when applied across a diversified portfolio.
- Building a custom backtest engine improves flexibility and control over trading logic, compared to black-box solutions.

---

 **Takeaway**: This challenge wasn't just about coding a strategy — it was about learning how to blend different technical components, implement trade logic with discipline, and evaluate real-world performance.

##  What I Learned

Throughout this project, I significantly expanded my knowledge of algorithmic trading and strengthened my technical skills in Python. By tackling each challenge progressively, I was able to move from simple return analysis to full strategy backtesting across multiple assets.

- **Backtesting Fundamentals**: I learned how to simulate historical trading strategies using moving averages, volatility metrics, and drawdown analysis to evaluate performance.

- **Portfolio Construction**: Through strategy comparisons and scoring systems, I explored how to build a more risk-aware portfolio by combining performance and resilience.

- **Python for Financial Analysis**: I gained confidence using libraries like `pandas`, `numpy`, `matplotlib`, and `yfinance` to structure, visualize, and analyze financial data effectively.

- **Strategy Design & Optimization**: I developed the mindset of a systematic trader — testing ideas rigorously, avoiding lookahead bias, and iterating based on statistical outcomes.



##  Insights

This project provided a number of broader insights into trading strategy development:

- **Simplicity Scales**: Even simple strategies, like moving average crossovers, can provide valuable frameworks for understanding market behavior and risk dynamics.

- **Risk-Adjusted Thinking Is Crucial**: Maximizing returns isn't enough — accounting for volatility, drawdowns, and win rates is what creates robust strategies.

- **One Size Doesn’t Fit All**: Optimization results varied across different stocks, reinforcing the idea that strategies must be adaptive to asset-specific behavior.

- **Discipline Beats Prediction**: Systematic rules help avoid emotional decisions, creating more repeatable and measurable trading outcomes.

- **Trend + Confirmation = Power**: Combining breakout signals with volume and relative strength dramatically improved signal quality and trade reliability.



##  Challenges I Faced

- **Debugging Strategy Logic**: Writing custom backtest logic came with its own hurdles — ensuring signals were realistic, positions correctly tracked, and no lookahead bias existed.

- **Data Limitations**: Using weekly data simplified analysis but came with trade-offs in precision and trade frequency. Handling data gaps also required attention.

- **Choosing the Right Metrics**: Understanding which metrics mattered most — Sharpe, drawdown, CAGR — was a learning process in itself.

- **Scaling to Portfolios**: Moving from single-stock logic to a full portfolio required rethinking capital allocation, open trade limits, and trade logs.


##  Conclusion

This algorithmic trading project offered an invaluable, hands-on education in developing, testing, and evaluating quantitative strategies. **Undertaking this as a beginner in Python and a self-learner in algorithmic trading has been a profound experience.** It allowed me to bridge theory with practice — not just by coding strategies, but by deeply understanding how traders think about risk, bias, and market structure.

I'm incredibly proud of the comprehensive journey from simple return analysis to advanced multi-asset backtesting. More than a technical exercise, this project was a significant step toward building intuition in systematic trading and portfolio design. **It has solidified my passion for the field and prepared me with practical skills as I prepare to embark on my intensive Python course.** This project truly lays the groundwork for future explorations into more advanced topics such as intraday strategies, machine learning, and live trading infrastructure.







