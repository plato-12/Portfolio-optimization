# Portfolio-optimization

Following project uses various optimization methods to create a portfolio for the stocks for Apple Inc, General Electrics and Microsoft taking data from yfinance api over the duration of one year; with the start date being the real time date.

## Returns

Calculate the returns from prices, and then calculate Expected Return, Risk and Covariance between assets.

We are investing in three stocks in the US Market: Apple Inc. (AAPL), General Electric Company (GE), Microsoft Corporation (MSFT). From the preparation above, we have daily adjusted closing prices from Jan 03, 2022 to Dec 31, 2022 of the three stocks downloaded from finance.yahoo.com

Compute the returns using the following formula:
$$R_t = \frac{AdjP_t-AdjP_{t-1}}{AdjP_{t-1}}$$

Adjusted closing price is the price of the stock taking into account dividends, stock splits and new stock offerings.

## Expected Returns, Standard Deviation, and Covariances

We are measuring risk(volatility) by standard deviation.
1. Expected return of each stock is estimated as the sample mean (average) of stock returns: $$\bar R =\sum_{t=1}^{T}\frac{R_{t}}{T}$$
2. Variance of each stock is estimated as: $$\sigma^{2} = \sum_{t=1}^{T}\frac{R_{i}-\bar R}{T-1}$$
3. Standard deviation of each stock is estimated by taking the square root of the variances. $$\sigma = \sqrt{\sigma^{2}}$$
4. Make the variance-covariance matrix of the thre stocks. Variance-covariance matrix contains variances and covariances, basically what we need to compute portfolio-variance.

\begin{array}{cccc}
\hline\hline
\textbf{} & \textbf{AAPLE} & \textbf{GELECT} & \textbf{MSOFT} \\ 
\hline
\textbf{AAPLE} & \sigma^2_{APPLE} & \sigma_{APPLE, GELECT} & \sigma_{APPLE, MSOFT}  \\ 
\textbf{GELECT} & \sigma_{APPLE, GELECT} & \sigma^2_{GELECT} & \sigma_{GELECT, MSOFT}  \\ 
\textbf{MSOFT} & \sigma_{APPLE, MSOFT} & \sigma_{GELECT, MSOFT} & \sigma^2_{MSOFT}  \\ 
\hline \hline
\end{array}

We have already estimated the variances and we use the following formula to estimate the covariances:
$$ cov(R^{1}, R^{2}) = \sum_{t=1}^{T}\frac{(R^{1}_{t} - \bar R_{1})(R^{2}_{t} - \bar R^{2})}{T-1} $$

## Risk and Return of a Portfolio

We calculate the risks and return of a portfolio, the weights of AAPL, GE and MSFT are 0.2, 0.5 and 0.3 respectively.

1. Portfolio consistes of three stocks. The portfolio return is computed as follows: $$ E(R_{P}) = w_{1}E(R_{1}) + w_{2}E(R_{2}) + w_{3}E(R_{3}) $$
Lets define vector $ \mathbf{w} = [w_{1}, w_{2}, w_{3}] $ and the expected return   vector $ E(\mathbf(R)) = [E(R_{1}), E(R_{2}), E(R_{3})] $, the portfolio return is: $$ E(R_{P}) = \mathbf{w}E(\mathbf{R})^{'} $$

2. Let's define $\Sigma$ as the covariance matrix of the three stocks and $\mathbf{w}$ is the vector of weights (the portion of wealth invested in each stock). The portfolio variance is: $$ \sigma^{2}_{P} =\mathbf{w}\Sigma \mathbf{w}^{'} $$ It looks like: $$ \sigma^{2}_{P} = \begin{bmatrix} w_{1} & w_{2} & w_{3} \end{bmatrix} \begin{bmatrix} \sigma^{2}_{1} & \sigma_{12} & \sigma_{13}\\ \sigma_{21} & \sigma^{2}_{2} & \sigma_{23}\\ \sigma_{31} & \sigma_{32} & \sigma^{2}_{3}\\ \end{bmatrix} \begin{bmatrix} w_{1} \\ w_{2} \\ w_{3} \end{bmatrix} $$ $$ = w_(1)^{2}\sigma_{1}^{2} + w_{2}^{2}\sigma_{2}^{2} + w_{3}^{2}\sigma_{3}^{2} + 2w_{1}w_{2}\sigma_{2} + 2w_{2}w_{3}\sigma_{23} +  2w_{1}w_{3}\sigma_{13} $$

3. We consider a portfolio with this vector of weights: $w_{1} = 0.2$, $w_{2}= 0.5$, and $w_{1} = 0.3$. Compute the expected return on this portfolio using the vector of expected returns for each stock and the vector of weights invested in each stock.

## Risk-Return Space
Let's start making a large set of portfolios and see how the risk-return combination looks like in risk-return space. To implement this step, we are going to build a large set of portfolios and therefore, we need to generate a large set of weights. Weights invested in the portfolio can be anything basically. We should only make sure that we don't invest more than we have. Therefore, we should make sure $\mathbf{1w^{'}} = 1$. Weights can also be constrained to be positive ($ w_{i}>0$) when we want to apply no short-selling constraint.

1. We already have the expected returns and risk for the stocks. Let's generate a large set of stocks' weights with constraint $ \mathbf{1w^{'}=1}$
2. Let's keep the weight values, for simplicity, between -1 and 1. As we already know the negative values represent the short-selling cases and the positive values represent the buying cases.
3. In order to generate a random value between 0 and 1, we use the function $\textbf{np.random.rand().}$ Use this function to get $ w_{1}$ and $w_{2}$. For $w_{3}$ use $ 1-w_{1}-w_{2}$. This will make sure that the summation of portfolio weights is 1 and thereby, the constraint $ \mathbf{1w^{'}}=1$ holds. Generate a sample of 2000 rows of protfoio weights.
4. Compute portfolio expected return and variance for each row of weights (or vector of weights).
5. Compute the standard deviation using the function $\textbf{np.std().}$
6. Now, we have different possible portfolios, and we have the portfolio expected returns and risk (in our case, it is measured by standard deviation).
7. Use plot function to create a scatter chart of risk and return. Consider the standard deviation of the portfolios as the X-axis and the Expected Return of the portfolios as the Y-axis.

## Section 2

In this section we'll create:
1. Minimum variance portfolio
2. Efficient frontier with short-selling constraint and no risk-free lending and borrowing
3. Efficient frontier without short-selling constraint and no risk-free lending and borrowing
4. Efficient frontier without short-selling constraint and also with risk-free lending and borrowing

## Optimization Problem

$$ \min_{x,y,z} f(x,y,z) = \frac{1}{xyz} $$  $$ s.t. \mathbf{x+y+z} = 1$$ $$ \mathbf{x,y,z} > 0$$ 

## The Minimum-Variance portfolio with no short-selling capability

Start with investing the minimum-variance portfolio with no short selling.

1. We have prepared the variance-covariance matrix, that shows the covariances between  three stocks, AAPL, GE, and MSFT, in the variable ret_cov. Moreover, we also have the Expected return and Standard deviation of these individual stocks in the variable sum_stats.
2. Now, we want to find the minimum variance portfolio, so we want to solve the following problem:
     $$ \min_{\mathbf{w}} \sigma^{2}_{P} = \mathbf{w\Sigma w'} $$ $$s.t. \mathbf{w1^{'}} = 1$$ $$\mathbf{w} >0 $$
3. We need to optimize (minimization in this case). There are many ways to solve the optimization problems with constraints.
4. Intuitively, the more random portfolio weights we generate, the more precise minimum variance weights we get. Here we can just generate 10,000 random portfolio weights.
5. Draw the scatter plot of all the weights simulation and indicate the weights yields to the minimum variance portfolio. 

## Efficient frontier with no short-selling capability and no riskless lending and borrowing opportunity

1. Goal is to find the efficient frontier. The first step is to find the minimum-variance portfolio, which we already found in the previous section.
2. The next step is to solve the portfolio optimization problem for specified values of targeted portfolio return: $$ \min \sigma^{2}_{P} = \mathbf{w\sigma w'}$$ $$E(R_{P}) = R_{P}^{0} $$ $$ \mathbf{w1^{'}} = 1 $$ $$\mathbf{w}>0$$
3. We can solve this optimization problem using the 'minimize' function for every given portfolio return. We need to specify values for $R_{i}$, which is the targeted portfolio return. But what values can we give? The values should be between the portfolio return of minimum-variance of portfolio (that you already computed) and also the maximum portfolio return from simulation. We can artifically create 100 risk levels in this range as $R_{i}, i\in (1, 100)$

4. Alternatively, we can use the Monte Carlo method again here to find the efficient frontier. The problem is equivalent to a maximization problem:
$$  \max E(R_{P}) = \mathbf{wE(R)} $$ $$\sigma^{2}_{i}$$ $$ \mathbf{w1^{'}}=1$$ $$\mathbf{w}>0)$$
5. We need to specify values for $\sigma^{2}_{i}$, which is the targeted portfolio risk. But what values can we give? The values should be between the portfolio risk of minimum-variance of portfolio (that we already computed) and also the maximum portfolio risk from simulation. We can artifically create 100 risk levels in this range as $\sigma^{2}_{i}, i\in (1,100)$.
6. For each value of $\sigma^{2}_{i}$, you need to find what is the highest return for the simulated portfolio with the risk level not higher than $\sigma^{2}_{i}$. After this we will have a combination of different risk levels and under which the best return we can get. Plot these combinations on the previous grapgh to the efficient frontier.

## Short-selling is allowed 

Now we construct the efficient frontier with short-selling and no risk-free lending and borrowing.

1. We want to find the efficient frontier with short-selling. First, let's look at the problem:
   $$ \min \sigma^{2}_{P}=\mathbf{w\sigma w'}$$ $$E(R_{P})= R_{P}^{0}$$ $$\mathbf{w1^{'}}=1$$
2. So, now, there is no limit on the weights. They can be anything but we cant spend more than what we have, and that's why we still have $\mathbf{w1^{'}}=1$.
3. Our goal is the same as before. Use either optimization problem sover, or Monte-Carlo Simulation, with non-negative constraints. Draw the efficient frontier with scatter plot.

## Short-selling with riskless lending and borrowing

Let's construct the efficient frontier with short-selling and also with the risk-lending and borrowing.

1. In this case, we would first find the tangency portfolio and then draw the line from the risk-free asset to the tangency portfolio. That's it. In this case, suppose the risk free rate is -0.00001(daily return, or about $-0.252 \%$ annually).
2. Let's first find the tangency portfolio, which has the largest slope $\theta$. We need to solve the following:
   $$ \max \theta = \frac{E(R_{P})-R_{F}} {\sigma_{P}}$$ $$\mathbf{w1^{'}}=1$$
3. Use solver or simply find the $\theta$ that offers the best sharpe ratio.
4. Next step, is to draw the line. Consider the following formula: $$ R_{P} = R_{F} + \frac{E_R{T}-R_{F}}{\sigma_{T}}\sigma_{P} = R_{F} + \theta\sigma_{P}$$. We already found the tangency portfolio and we have the $\textit{theta}$. The only step that we need to take is to generate different levels of portfolio standard deviation (starting from 0 to some level that we find appropiate) and compute the portfolio returns using the above formula.
5. Draw the efficient frontier with a scatter plot.
