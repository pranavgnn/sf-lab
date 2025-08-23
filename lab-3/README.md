# Lab 3
**18/08/2025**

### Stata Guide
File > Import > Excel spreadsheet
Browse for data
`Import first row as variable names`
Statistics > Summaries, tables, and tests > Summary and descriptive statistics > Summary statistics

### Step 1: Hypothesis
H<sub>0</sub>: Current Ratio for all large companies is 2
H<sub>1</sub>: Current Ratio for all large companies is not 2

### Step 2: Normality Test
Use Shapiro-Wilk's test.
Command:
```r
swilk CurrentRatio 
```

Output:
```

                   Shapiro–Wilk W test for normal data

    Variable |        Obs       W           V         z       Prob>z
-------------+------------------------------------------------------
CurrentRatio |         99    0.34224     53.854     8.838    0.00000

```

H<sub>0</sub>: Data is normal
H<sub>1</sub>: Data is not normal

Since `p < 0.05`, we fail to reject H<sub>0</sub>.
Therefore, data is not normal.

Transform the data by taking `log`.
Command:
```r
gen Current_Ratio = log(CurrentRatio)
```

Output:
```
(12 missing values generated)
```

Perform Shapiro-Wilk's test again on the new column.
Command:
```r
swilk Current_Ratio
```

Output:
```
                   Shapiro–Wilk W test for normal data

    Variable |        Obs       W           V         z       Prob>z
-------------+------------------------------------------------------
Current_Ra~o |         88    0.82920     12.681     5.595    0.00000

```

Since `p < 0.05`, we fail to reject H<sub>0</sub>.
Therefore, data is not normal.

We could not transform our data.

### Step 3: Select Test
We are choosing Wilcoxon Signed Rank Test.
Command:
```r
signrank CurrentRatio = 2
```

Output:
```

Wilcoxon signed-rank test

        Sign |      Obs   Sum ranks    Expected
-------------+---------------------------------
    Positive |       19      1087.5      2474.5
    Negative |       79      3861.5      2474.5
        Zero |        1           1           1
-------------+---------------------------------
         All |       99        4950        4950

Unadjusted variance    82087.50
Adjustment for ties      -28.88
Adjustment for zeros      -0.25
                     ----------
Adjusted variance      82058.38

H0: CurrentRatio = 2
         z = -4.842
Prob > |z| = 0.0000
Exact prob = 0.0000
```

### Step 4 : Interpretation
The p value is 0.0000

As the p value is less than the significance value of 0.05, we reject the null hypothesis.
Therefore, all companies do not maintain a current ratio of 2.