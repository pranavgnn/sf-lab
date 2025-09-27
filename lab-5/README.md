
# Lab 5

**27/09/2025**

### Question 
It is often debated whether the percentage spent on CSR activities has a significant impact on the profitability of FMCG companies. Perform a hypothesis test to investigate this claim.

### Variables
**CSR**: Corporate Social Responsibility **(Independent Variable)**
**ROCE**: Return on Capital Employed **(Dependent Variable)**

### Ratios (Control Variables)
1. Total Asset Turnover Ratio
2. Interest Coverage Ratio
3. Inventory Turnover Ratio

### Step 1: Hypothesis
H<sub>0</sub>: There is no statistically significant difference in the market capitalization of automobile companies listed on these two stock exchanges

H<sub>1</sub>: There is a statistically significant difference in the market capitalization of automobile companies listed on these two stock exchanges

### Step 2: Regression Analysis
Use **Linear Regression**.
Command:
```r
regress ROCE PercentageSpent TotalAssetTurnoverRatio InventoryTurnoverRatio InterestCoverRatio
```

Output:
```

      Source |       SS           df       MS      Number of obs   =        76
-------------+----------------------------------   F(4, 71)        =      4.26
       Model |  5037.93377         4  1259.48344   Prob > F        =    0.0038
    Residual |  21005.1264        71  295.846851   R-squared       =    0.1934
-------------+----------------------------------   Adj R-squared   =    0.1480
       Total |  26043.0602        75  347.240803   Root MSE        =      17.2

-----------------------------------------------------------------------------------------
                   ROCE | Coefficient  Std. err.      t    P>|t|     [95% conf. interval]
------------------------+----------------------------------------------------------------
        PercentageSpent |  -1.455325   3.829543    -0.38   0.705    -9.091217    6.180568
TotalAssetTurnoverRatio |   3.547468   1.148524     3.09   0.003     1.257378    5.837559
 InventoryTurnoverRatio |   .1247081   .2049705     0.61   0.545    -.2839914    .5334077
     InterestCoverRatio |   .0320972   .0152231     2.11   0.039     .0017431    .0624513
                  _cons |   11.09876   5.316966     2.09   0.040      .497032    21.70049
-----------------------------------------------------------------------------------------

```

### Step 3: Interpretation
The p value is `0.4219`

As the p value is more than the significance value of 0.05, we **fail to reject the null hypothesis**.


Therefore, there is no statistically significant difference in the market capitalization of automobile companies listed on these two stock exchanges.
