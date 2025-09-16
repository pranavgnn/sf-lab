# Lab 4

**23/08/2025**

### Question 
It is often debated that there is a significant difference in the market capitalization of automobile companies listed on these two stock exchanges. Perform a hypothesis test to investigate this claim.

### Step 1: Hypothesis
H<sub>0</sub>: There is no statistically significant difference in the market capitalization of automobile companies listed on these two stock exchanges

H<sub>1</sub>: There is a statistically significant difference in the market capitalization of automobile companies listed on these two stock exchanges

### Step 2: Normality Test
Use **Shapiro-Wilk's test**.
Command:
```r
swilk MarketCapitalisation
```

Output:
```

                   Shapiro–Wilk W test for normal data

    Variable |        Obs       W           V         z       Prob>z
-------------+------------------------------------------------------
MarketCapi~n |         35    0.80796      6.854     4.018    0.00003

```

H<sub>0</sub>: Data is normal

H<sub>1</sub>: Data is not normal

Since `p < 0.05`, we **reject H<sub>0</sub>**.
Therefore, **data is not normal**.

Transform the data by taking `log`.
Command:
```r
gen Market_Cap = log(MarketCapitalisation)
```

Output:
```
. 
```

Perform **Shapiro-Wilk's test** again on the new column.
Command:
```r
swilk Market_Cap
```

Output:
```

                   Shapiro–Wilk W test for normal data

    Variable |        Obs       W           V         z       Prob>z
-------------+------------------------------------------------------
  Market_Cap |         35    0.96060      1.406     0.712    0.23833

```

Since `p > 0.05`, we **fail to reject H<sub>0</sub>**.
Therefore, **data is normal**.

We successfully managed to transform our data.

### Step 3: Select Test
We are choosing **Independent sample t test**.
Command:
```r
ttest Market_Cap, by(Index)
```

Output:
```
Two-sample t test with equal variances
------------------------------------------------------------------------------
   Group |     Obs        Mean    Std. err.   Std. dev.   [95% conf. interval]
---------+--------------------------------------------------------------------
BSE Auto |      20    6.667114    .1908554    .8535314    6.267649    7.066579
Nifty Au |      15    6.894473    .1984138    .7684532    6.468918    7.320028
---------+--------------------------------------------------------------------
Combined |      35    6.764553    .1376641    .8144316    6.484786     7.04432
---------+--------------------------------------------------------------------
    diff |           -.2273593    .2795772               -.7961635    .3414449
------------------------------------------------------------------------------
    diff = mean(BSE Auto) - mean(Nifty Au)                        t =  -0.8132
H0: diff = 0                                     Degrees of freedom =       33

    Ha: diff < 0                 Ha: diff != 0                 Ha: diff > 0
 Pr(T < t) = 0.2110         Pr(|T| > |t|) = 0.4219          Pr(T > t) = 0.7890
```

### Step 4 : Interpretation
The p value is `0.4219`

As the p value is more than the significance value of 0.05, we **fail to reject the null hypothesis**.


Therefore, there is no statistically significant difference in the market capitalization of automobile companies listed on these two stock exchanges.


