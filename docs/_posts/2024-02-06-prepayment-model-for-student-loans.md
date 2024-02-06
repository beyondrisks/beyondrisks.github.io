---
layout: single
title:  "Prepayment Model for Student Loans"
classes: wide
date:   2024-02-06
last_modified_at: 2024-02-06
categories:
    - prepayment risk
    - model
    - student loans
    - econometrics
---

We've recently developed a prepayment model for student loans as part of our client's CECL calculation. In this article, we aim to discuss two challenges encountered during our development process. 

## What is Prepayment
Prepayment involves borrowers paying off their loans before the scheduled maturity date, either partially or in full. This article focuses specifically on full prepayment. In the realm of student loans, there are generally two types: borrower payoff and consolidation. Borrower payoff occurs when individuals have surplus funds and opt to repay their loans ahead of schedule. This repayment rate typically remains consistent over time and isn't typically the primary focus of the model. Consolidation, on the other hand, involves repaying an existing loan by obtaining a new loan at a lower interest rate. This form of prepayment essentially functions as a call option embedded within the loan. When prevailing market interest rates fall below the loan's rate, borrowers find themselves "in the money" and are more likely to exercise this option by repaying the loan. This process is often referred to as refinancing within the mortgage industry.

## Two Challenges
During the model development, two primary challenges emerged. Firstly, unlike the mortgage industry, there's no universally recognized market rate for student loans. Consequently, we lacked a direct comparison between the existing loan rate and the market rate to determine whether borrowers were "in the money" or "out of the money." To address this, we opted to utilize the 10-year Treasury note rate as a proxy for the market rate. The incentive was then calculated as the variance between the loan rate and the 10-year Treasury note rate. However, it's important to note that the sign of the incentive alone doesn't indicate the borrower's position. This incentive works not only for the fixed-rate loans, but also for the variable-rate loans. We integrated both fixed-rate and variable-rate loans into one model. For the variable-rate loans, this incentive is essentially the difference between the short-term rate and long-term rate, which is one of the most drivers of prepayment for the variable-rate loans, as the existing loan rate typically comprises the index rate plus the margin, with the index rate typically representing one of the short-term interest rates. This approach enabled us to construct a coherent monotonic response curve for the incentive, which flattens as the incentive grows sufficiently large.

The second challenge revolves around the significant increase in observed prepayment rates between 2014 and 2021, while the incentive remained relatively stable during this period. The existing model attributed this phenomenon to house price appreciation, suggesting that borrowers tapped into their home equity to repay student loans. However, this reasoning poses an issue, considering that student loan borrowers typically belong to a demographic just starting their careers, making it improbable for them to have accumulated substantial home equity. Following discussions with clients, it was suggested that the expansion of the consolidation market, exemplified by companies like SoFi, which partnered with Fannie Mae to [allow homeowners to pay down their student debts with cash out refinance](https://www.sofi.com/press/sofi-fannie-mae-give-homeowners-smart-way-reduce-student-debt/), could be the driving force behind this trend. Viewing consolidation desires as demand-side factors, this expansion represents an external shift on the supply side. Lacking pertinent market data, we were unable to control for this within the model. Fortunately, internal data since 2015 provided insight into distinguishing between borrower payoff and consolidation. Leveraging this information, we constructed a consolidation index over time and incorporated it into the model by interacting it with the incentive, as expressed below
\\[
\beta \times consolidationIndex \times incentive,
\\]
where $\beta$ is the coefficient. This expression can be interpreted in two ways. Firstly, considering $consolidationIndex$ and $incentive$ as components of a new composite incentive. This suggests that the presence of the consolidation market enhances the overall incentive. Thus, having the same level of incentive within a larger consolidation market implies a greater real incentive. Alternatively, one can view this aspect as combining $\beta$ and $consolidationIndex$ into a new coefficient. This implies that with a larger consolidation market, the same incentive yields a larger marginal effect. In practice, it's assumed that the consolidation market reached maturity around 2019 and has remained stable since then. By incorporating this interaction, the model effectively captures the increasing trend over the observed period.

## Summary
In our model, we calculated the incentive as the difference between loan rate and 10-year Treasury note rate. In order to capture the impact of the expansion of the consolidation market, we constructed an index and interacted it with the incentive to estimate a more accurate marginal effect of the incentive.
