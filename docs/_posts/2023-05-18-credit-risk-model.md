---
layout: single
title:  "Credit Risk Models"
classes: wide
date:   2023-05-18
last_modified_at: 2023-05-18
categories:
    - credit risk
    - model
    - econometrics
---

## What is Credit Risk
Credit risk refers to the potential risk of loss that arises from a borrower's failure to repay a debt obligation as agreed. It is the risk that a borrower or counterparty may default on their financial obligations, leading to a loss for the lender or creditor.

Credit risk is commonly encountered in various financial transactions, such as loans, bonds, mortgages, credit cards, and trade credit. When extending credit, lenders assess the creditworthiness of borrowers by considering factors such as their credit history, income, assets, and financial stability. Based on this assessment, lenders assign a credit rating or score to determine the level of risk associated with lending to a particular individual or entity.

## What is Credit Risk Model
A credit risk model is a quantitative tool or framework used by financial institutions and lenders to assess and quantify the credit risk associated with lending to borrowers or counterparties. These models help institutions make informed decisions regarding credit extension, pricing, and risk management. 

Credit risk models aim to estimate the likelihood of default by borrowers and the potential loss severity in the event of default. They typically incorporate a variety of factors and data points to evaluate the creditworthiness of borrowers. Some common components of credit risk models include:

- Credit scoring: Credit scoring models assign numerical scores to borrowers based on their credit history, payment patterns, outstanding debt, income, and other relevant factors. These scores help lenders categorize borrowers into different risk tiers and determine their likelihood of default.

- Financial ratios and indicators: Credit risk models often consider financial ratios and indicators, such as debt-to-income ratio, leverage ratio, profitability measures, and liquidity measures. These indicators provide insights into the financial health and stability of borrowers.

- Collateral valuation: In cases where loans are secured by collateral, credit risk models may incorporate the value and quality of the collateral to assess potential loss severity in the event of default. This helps lenders determine the loan-to-value ratio and mitigate risk.

- Macroeconomic factors: Some credit risk models consider macroeconomic indicators and trends, such as GDP growth, interest rates, unemployment rates, and industry-specific factors. These factors provide a broader context for evaluating credit risk, as economic conditions can impact the ability of borrowers to repay their obligations.

- Behavioral scoring: In addition to credit history, credit risk models may also incorporate behavioral scoring, which analyzes patterns of borrower behavior, such as payment behavior, credit utilization, and past delinquencies. This provides insight into the borrower's credit management practices and potential future behavior.

Credit risk models are commonly used in two areas: underwriting and future loss projection. In the underwriting case, the model is used to determine borrowers' creditworthiness which in turn helps lenders to decide whether or not to issue a loan or credit card to borrowers. It usually generates the probability of default within a certain period (i.e. 6 months) in the future. In the loss projection case, the model is used to predict the the probability of default every month in the future. In this article we will discuss the model used in the loss projection case. We will have a separate article for the underwriting model.

## Models
The occurrence of a default event can be regarded as a termination event, signifying the conclusion of the lending relationship between borrowers and lenders. Thus, predicting the timing of default essentially involves forecasting the duration of the relationship's survival. In econometrics, this field is referred to as duration analysis or survival analysis. The fundamental concept behind such models is to define the functional form of either the cumulative distribution function or the hazard function for the duration. The factors that influence credit risks, as discussed earlier, are incorporated into these functions in various ways. The estimation process typically involves maximum likelihood estimation (MLE). 
### Data and Notations
Before delving into the general form of the likelihood function, it is important to discuss the structure of the data and available information. The sample consists of $n$ loans, with monthly status (default or non-default) observed between $t_{1}$ and $t_{2}$. In addition to loan status, we also observe some covariates, $x$, including loan characteristics, denoted as $x_i$, and time specific characteristics, denoted as $x_t$. The loan status at a specific month $t$ for loan $i$ is denoted as $y_{it}$, where $y_{it}$ equal to 1 if the loan defaults in that month and 0 otherwise. It's worth noting that since loan or credit card payments are typically made on a monthly basis, the duration discussed in this article is discrete. 

In survival analysis, the primary challenge lies in handling censored data. For loans that have not defaulted within the observation window, we lack information regarding their eventual default and the exact timing of it. We can only infer that these loans have survived up to $t_2$. This type of data is referred to as right-censored. It is incorrect to simply assign the duration as $t_2$ for these loans. Instead, the appropriate approach is to acknowledge that the durations of these loans extend beyond $t_2$ and incorporate this understanding into the likelihood function used for maximum likelihood estimation (MLE).

### General Form of Likelihood Function
The likelihood function for a non-censored loan represents the probability that the loan survives until time $t_{i-1}$ and defaults at time $t_i$, denoted as $Pr(T=t_i|x_i)$. On the other hand, the likelihood for a censored loan captures the probability that the loan survives until time $t_2$, denoted as $Pr(T>t_2|x_i)$. To distinguish between censored and non-censored loans, we use $c_i=\mathbb{1}(t_i=t_2)$, where $c_i$ equals 1 if loan $i$ is censored and 0 otherwise. Then the general form of the likelihood function for loan $i$ can be written as 
\$$
L_i=Pr(T=t_i|x_i)^{(1-c_i)}Pr(T>t_2|x_i)^{c_i}
\$$
and the likelihood function for the whole sample is then 
\\[
L = \prod_{i=1}^{n}L_i,
\\]
assuming loans are independent from each other.
### Method 1: Specify Distribution Function of Duration
In this approach, we specify the cumulative distribution function of duration, denoted as $F(t|x; \beta)$, parameterized with $\beta$, along with the corresponding probability density function $f(t|x; \beta)$. Since our focus is no discrete duration, $f(t|x; \beta)$ represents the probability of the duration being equal to $t$.
The likelihood function for loan $i$ can now be expressed as:
\\[
L_i=f(t_i|x_i; \beta)^{(1-c_i)} (1-F(t_2|x_i;\beta))^{c_i},
\\]
It is important to note that this estimation method relies on a single observation from each loan, making it challenging to incorporate time-varying covariates into the analysis.

#### Accelerated Failure Time Model
When the distribution function $F(t|x;\beta)$ is assumed to follow the form $F_0(e^{-x\beta}t)$, where $F_0$ represents the distribution function of a baseline survival time $T_0$ that is independent of covariates, the specific model is referred to as an accelerated failure time model. The assumption essentially implies that actual survival time $T$ can be expressed as $T=e^{x\beta}T_0$, as one can see from the equation: 
\\[F(t|x;\beta) = Pr(T<t|x;\beta)=F_0(e^{-x\beta}t)=Pr(T_0<e^{-x\beta}t)=Pr(e^{x\beta}T_0<t).\\]
Hence, it is evident that the model can be represented as a log-linear model, given by $\log T=x\beta + \epsilon$, where $\epsilon$ corresponds to $\log T_0$. The distribution of $\log T_0$ determines the distribution of $\log T$. Estimating this model is relatively straightforward.

### Method 2: Specify Hazard Function
This approach addresses the problem by examining the conditional survival probability on a monthly basis, specifically focusing on the probability of default in month $t$ given that the loan has survived until month $t_1$. The discrete version of the hazard function is defined as $h(t|x) = \frac{Pr(T= t|x)}{Pr(T\ge t-1|x)}$.

By using the properties of conditional probabilities, we can express the probability of the loan defaulting at time $t_i$ as:
\\[Pr(T=t_i|x)=\prod_{j=1}^{t_{i-1}}Pr(T>j|T>j-1)h(t_i|x)=\prod_{j=1}^{t_{i-1}}(1-h(j|x))h(t_i|x).\\]
Similarly, the probability of the loan surviving beyond $t_2$ can be represented as:
\\[
Pr(T>t_2|x)=\prod_{j=1}^{t_{2}}(1-h(j|x))
\\]
For loans that defaulted during the observation window, the sub-sample for that loan includes all observations until time $t_i$. The likelihood function for this loan can then be written as:
\\[
L_{i}=\prod_{t=1}^{t_i-1}(1-h(t|x_i, x_{t}))h(t_i|x_i,x_{t_i}).
\\]
For loans that were censored, the likelihood function is simply 
\\[
L_i=\prod_{t=1}^{t_i}(1-h(t)|x_i,x_{t})
\\]
and the likelihood function of the whole sample is therefore 
\\[
L =\prod_{i=1}^{N}(\prod_{t=1}^{t_i-1}(1-h(t)|x_i, x_{t})h(t_i|x_i, x_{t_i}))^{(1-c_i)}(\prod_{t=1}^{t_i}(1-h(t)|x_i, x_{t}))^{c_i}.
\\]
It is evident that this method utilizes a total of $\sum_{i=1}^{n}t_i$ samples.
#### Relationship with Logistic Regression
Recall that we use $y_{it}$ to denote whether loan $i$ is terminated at time $t$. Therefore, For censored loans, $y_{it}=0$ for all $t$; while for loans that were not censored, $y_{it}=0$ for $t<t_i$ and 1 when $t=t_i$. The likelihood function of both types of loans can be simplified as 
\\[
L_{i}=\Pi_{t=1}^{t_i}(1-h(t|x_i,x_{t}))^{1-y_{it}}h(t_i|x_i, x_{t_i})^{y_{it}},
\\]
and the likelihood of the whole sample then becomes
\\[
L =\Pi_{i=1}^{N}\Pi_{t=1}^{t_i}(1-h(t|x_i, x_{t}))^{1-y_{it}}h(t_i|x_i, x_{t_i})^{y_{it}},
\\]
which can be further expressed as the log-likelihood function:
\\[
l = \sum_{i=1}^N\sum_{t=1}^{t_i}(1-y_{it})\log(1-h(t|x_i, x_{t})) + y_{it}\log(h(t_i|x_i, x_{t_i}))
\\]
Assuming the hazard function follows the cumulative distribution function of the logistic distribution, $h(t|x)=\frac{1}{1+\exp(-x_t\alpha-x_i\beta)}$, we arrive at a familiar logistic regression framework.

From this perspective, we observe that the widely used logistic credit risk model handles the issue of right censoring.

## Summary

In this article we discussed several credit risk models. We started from the general form of likelihood function to two specific methods, accelerated failure time model and logistic regression model. We also showed that the logistic model has handled the issue of right censoring.

