# Statistics for Finance - Group Project

**Submitted by:**
1. Pranav G Nayak - 230958011
2. Roselin Maria T J - 230958032
3. Dhruva Deepak - 230958068

**Date:** 02/11/2025

---

## 1. Topic

To examine whether there is a statistically significant relationship between firm financial and governance variables and the Price to Earnings (P/E) Ratio of BSE 100 companies.

---

## 2. Hypothesis

**Null Hypothesis (H₀):**  
There is no statistically significant relationship between firm financial and governance variables and the P/E ratio of BSE 100 companies.

**Alternative Hypothesis (H₁):**  
There is a statistically significant relationship between firm financial and governance variables and the P/E ratio of BSE 100 companies.

---

## 3. Variables

### Dependent Variable:
1. **Price to Earnings (P/E) Ratio:** A common measure of market valuation.

### Independent Variables:
1. **Return on Equity (ROE):** Measures profitability relative to shareholder equity.
2. **Interest Coverage Ratio:** Measures a firm's ability to pay interest.
3. **CSR Compliance:** Proxy for corporate governance performance.
4. **Firm Size (Log of Total Assets):** Represents scale and market influence.

### Rationale:
These variables capture both financial performance (ROE, Interest Coverage Ratio) and governance aspects (CSR Compliance), along with a control for firm size, forming a multivariate regression model to explain variation in the P/E ratio.

---

## 4. Sample

1. **Population:** BSE 100 companies.
2. **Sample Size:** 178 firm-year observations.
3. **Data Source:** Capitaline Database (financial and CSR data).
4. **Sampling Type:** Secondary data using publicly available financial information.

---

## 5. Type of Analysis

**Method Used:** Cross-sectional multiple regression analysis.

**Justification:**  
The study examines the relationship between financial and governance variables and P/E ratio for a single time period across multiple firms (BSE 100). Therefore, a cross-sectional OLS regression is appropriate to test the hypothesis.

---

## 6. Results

### Regression Command:

```
regress PERatio ROE InterestCoverRatio CSRCompliance FirmSize
```

### Regression Output:

```
      Source |       SS           df       MS      Number of obs   =       178
-------------+----------------------------------   F(4, 173)       =      8.22
       Model |  194051.882         4  48512.9706   Prob > F        =    0.0000
    Residual |  1021057.75       173  5902.06793   R-squared       =    0.1597
-------------+----------------------------------   Adj R-squared   =    0.1403
       Total |  1215109.63       177  6865.02618   Root MSE        =    76.825

------------------------------------------------------------------------------------
           PERatio | Coefficient  Std. err.      t    P>|t|     [95% conf. interval]
-------------------+----------------------------------------------------------------
               ROE |  -1.220432   .3758098    -3.25   0.001    -1.962195   -.4786693
InterestCoverRatio |   .0010446   .0138755     0.08   0.940    -.0263425    .0284317
     CSRCompliance |   2.773793   2.059502     1.35   0.180    -1.291193    6.838778
          FirmSize |    -45.219   9.422477    -4.80   0.000    -63.81682   -26.62119
             _cons |   258.8904   38.32529     6.76   0.000      183.245    334.5358
------------------------------------------------------------------------------------
```

### Model Summary:

1. **Number of observations:** 178
2. **F(4,173):** 8.22
3. **Prob > F:** 0.0000
4. **R²:** 0.1597
5. **Adjusted R²:** 0.1403
6. **Root MSE:** 76.825

---

## 7. Interpretation

At the 95% confidence level:

1. **ROE (p=0.001):** Significant negative relationship.
   - For every 1 unit increase in ROE, P/E ratio decreases by 1.22.

2. **Firm Size (p=0.000):** Significant negative relationship.
   - For every 1 unit increase in Firm Size, P/E ratio decreases by 45.22.

3. **Interest Coverage Ratio (p=0.940)** and **CSR Compliance (p=0.180)** are not statistically significant.

4. **R² = 0.1597** and **Adjusted R² = 0.1403**, indicating that around 16% of the variation in P/E ratio is explained by the model.

---

## 8. Diagnostic Tests

### a. Linearity (Ramsey RESET Test)

**Command:**

```
estat ovtest
```

**Output:**

```
Ramsey RESET test for omitted variables
Omitted: Powers of fitted values of PERatio

H0: Model has no omitted variables

F(3, 170) =   5.90
 Prob > F = 0.0007
```

**Hypotheses:**  
H₀: Data is linear  
H₁: Data is not linear

Since `p < 0.05`, the data shows non-linearity, indicating possible omitted variable bias. **We reject the null hypothesis.**

---

### b. Multicollinearity (Variance Inflation Factor)

**Command:**

```
estat vif
```

**Output:**

```
    Variable |       VIF       1/VIF  
-------------+----------------------
         ROE |      1.19    0.836852
InterestCo~o |      1.19    0.839440
    FirmSize |      1.02    0.978450
CSRComplia~e |      1.02    0.980885
-------------+----------------------
    Mean VIF |      1.11
```

As all VIFs are between 1 and 5, **there is no multicollinearity.**

---

### c. Homoscedasticity (Breusch–Pagan Test)

**Command:**

```stata
estat hettest
```

**Output:**

```
Breusch–Pagan/Cook–Weisberg test for heteroskedasticity 
Assumption: Normal error terms
Variable: Fitted values of PERatio

H0: Constant variance

    chi2(1) =  73.56
Prob > chi2 = 0.0000
```

**Hypotheses:**  
H₀: Data is homoscedastic (constant variance)  
H₁: Data is not homoscedastic

Since `p < 0.05`, we reject H₀ → **Data is heteroscedastic (variance not constant).**

---

### d. Normality

As n = 178, by the Central Limit Theorem, the residuals are assumed to be approximately normal.

---

## 9. Conclusion

The regression analysis indicates that ROE and Firm Size significantly affect the P/E ratio, both showing negative relationships. The other variables—Interest Coverage Ratio and CSR Compliance—are not significant.

However, diagnostic tests highlight issues of **heteroscedasticity** and **non-linearity**, which may affect model reliability. Future studies can employ robust standard errors or variable transformations to improve accuracy.

### Final Decision:

**Reject the null hypothesis (H₀).**

There exists a statistically significant relationship between firm financial and governance variables and the P/E ratio of BSE 100 companies.

---

## 10. Personal Learning

Through this analysis, we learned how to:

1. Perform and interpret multiple regression analysis in STATA.
2. Diagnose model assumptions (linearity, multicollinearity, heteroscedasticity).
3. Interpret coefficients, p-values, and R² effectively.
4. Understand that statistical significance does not always imply strong explanatory power.
5. Recognize the importance of model validation for reliable inferences.

---

## 11. References

1. Capitaline Database (2025). Financial data for BSE 100 companies.
2. STATA Documentation (2024). Regression and Postestimation Commands.
