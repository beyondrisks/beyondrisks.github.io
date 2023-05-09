---
layout: single
title:  "Stress Test -- DFAST"
last_modified_at: 2023-05-08
classes: wide
categories:
  - credit risk
  - statistical model
  - stress test
  - DFAST
---

## Overview

DFAST stands for Dodd-Frank Act Stress Test. It is an annual stress test conducted by the Federal Reserve as required by the Dodd-Frank Act of 2010. It requires that large banks predict their performance in the future 9 quarters under a range of stress scenarios to evaluate their capital adequacy.

## Supervisory Stress Test Scenarios

The stress test scenarios are designed by Federal Reserve. They include a baseline scenario and a severely adverse scenario. 
1. Baseline scenario: a profile similar to that of average projections from a survey of economic forecasters.
2. Severely adverse scenario is a hypothetical set of conditions in an adverse economic environment. You can visualize the economic environment through the charts in the last section of the article.

### Some Facts of Scenarios
- **Time period of scenarios**: 9 quarters.
- **Sets of Variables for Domestic Economic Condition**
    - real / nominal GDP growth
    - real / nominal disposable personal income growth
    - Consumer Price Index (CPI) inflation rate
    - unemployment rate
    - house prices indexes
    - commercial real estate prices
    - Dow Jones Total Stock Market Index
    - stock market volatility
    - 3-month Treasury rate
    - 5-year Treasury yield
    - 10-year Treasury yield
    - 10-year BBB corporate yield
    - mortgage rate
    - prime rate
- **Sets of Variables for International Economic Condition**: three variables in four countries or country blocs. 
    - real GDP growth in euro area, United Kingdom, developing Asia and Japan
    - CPI inflation rate in euro area, United Kingdom, developing Asia and Japan
    - bilateral dollar exchange rate in euro area, United Kingdom, developing Asia and Japan 

## What to submit

As is seen in [2022 Federal Reserve Stress Test Results](https://www.federalreserve.gov/publications/files/2022-dfast-results-20220623.pdf), the final package submitted by the banks include four parts: capital, pre-tax net income, losses, and pre-provision net revenue under severely adverse scenario.

1. **Capital** refers to the common equity tier 1 (CET1) capital ratio, which is calculated as the ratio of CET1 capital to the total risk weighted assets.
> **Knowledge Sharing**
> 
> - The risk-weighted assets are calculated based on standardized approach described in [12 C.F.R. part 217 Subpart D](https://www.ecfr.gov/current/title-12/chapter-II/subchapter-A/part-217/subpart-D).
> - The risk free asset such as government bond is not included in the risk-weighted assets since the risk weight is 0.
2. **Pre-tax net income** is the projections of pre-tax net income in the future under the severely adverse scenario. The result depends on two components included projections of losses and pre-provision net revenue.
3. **Losses** include loan losses, the losses from loans booked under the fair-value option (FVO), trading and counterparty losses, processing or custodial operations, and securities losses.
> **Knowledge Sharing**
> 
> - **Amortized cost**: initial amount of the loan - principal repayments + unamortized premium - unamortized discount
> - **FVO**: fair market value. It is adjusted to reflect changes in market conditions or other factors that may affect its value.

4. **Pre-provision net income** includes projections of post-stress income and expense captured in PPNR. The expenses are mainly the operational risk losses.

## Credit Risk Model

As one can see, the final submission involves lots of forecast and calculation. Let's check one by one. 
1. **Capital ratios**
   
   Since the calculation of risk-weighted assets is rule based as discussed above, there are no prediction models involved in this part.
2. **Loan losses**

   In essence, the projected losses can be computed as the product of the probability of default and the loss given default, which is determined by multiplying the exposure at default with the severity rate (1-recovery rate). These components necessitate the use of statistical models to generate accurate predictions. For instance, consider mortgage loans. The probability of default is influenced by various factors, such as borrower characteristics (credit history, income, etc.), loan features (type, interest rate, purpose, etc.), and macroeconomic factors (housing price, unemployment rate, interest rate, etc.). The exposure at default, on the other hand, depends on the timing of default, with a significant difference in exposure between early-stage and late-stage defaults. Finally, the recovery rate is influenced by factors such as the condition of the collateral, availability of mortgage insurance, and housing market conditions at the time of default.

3. **Pre-provision net income**
   
   Forecasting future income and expenses involves taking into account numerous factors. For instance, when predicting income from mortgage loans, there are two components: income from existing loans and income from future loans. The income from existing loans depends on factors such as prepayment speed and probability of default, while income from future loans depends on predictions of loan volumes as well as prepayment and credit risks. There are various methods for predicting future loan volumes, which are dependent on the type of asset. A common approach is to use a time series model that incorporates economic conditions.

## Summary

The Federal Reserve regulates the DFAST stress test, which is aimed at evaluating the performance of large banks in hypothetical severely adverse scenarios. To accurately assess sensitivities to economic conditions, a set of robust statistical models are required. These models encompass credit risk models for various loan types, prepayment risk models, and time series models for volume forecasting.

## Reference
1. [2023 Stress Test Scenarios](https://www.federalreserve.gov/supervisionreg/dfa-stress-tests-2023.htm), Federal Reserve.
2. [2022 Federal Reserve Stress Test Results](https://www.federalreserve.gov/publications/files/2022-dfast-results-20220623.pdf), Federal Reserve

## Visualization of Severely Adverse Scenario 

> Note: all the data are downloaded from [Federal Reserve's website](https://www.federalreserve.gov/supervisionreg/dfa-stress-tests-2023.htm).

{% include dfast_scenarios.html %}