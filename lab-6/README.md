# Lab 6

**25/10/2025**

### Question 


### Commands Cheatsheet

```
tsset
tsline depvar
dfuller devar
generate diff_depvar = D.depvar
dfuller diff_depvar
ac diff_depvar
pac diff_depvar
arima diff_depvar, arima (AR,I,MA)
predict res, residuals
wntestq res
```

---

### Step 1: Load Data

Command:

```
use https://www.stata-press.com/data/r19/wpi1.dta
```

---

### Step 2: Describe data

Command:

```
describe using https://www.stata-press.com/data/r19/wpi1.dta
```

Output:

```
Contains data                                 
 Observations:           124                  28 Nov 2024 10:31
    Variables:             3                  
-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
Variable      Storage   Display    Value
    name         type    format    label      Variable label
-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
wpi             float   %9.0g                 US Wholesale Price Index
t               int     %tq                   Quarterly date
ln_wpi          float   %9.0g                 
-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
Sorted by: t  
```

---

### Step 3: Declare dataset as time-series data

Command:

```
tsset t
```

Output:

```
Time variable: t, 1960q1 to 1990q4
        Delta: 1 quarter

```

---

### Step 4: Visualize the trend

Command:
```
tsline wpi
```

Output:

<img width="766" height="506" alt="Screenshot-2025-10-25-143702" src="https://github.com/user-attachments/assets/9834a4b6-2c3a-4cf3-99d7-c29cbb693113" />

---

### Step 5: Augmented Dickey-Fuller Test

H<sub>0</sub>: Series is not stationary<br>
H<sub>1</sub>: Series is stationary

ac

![img](https://i.ibb.co/9Hb8Pn2k/ac.png)

pac

![img](https://i.ibb.co/CKYZfYxT/pac.png)

Command:

```
dfuller wpi
```

Output:

```
Dickey–Fuller test for unit root          Number of obs  = 123
Variable: wpi                             Number of lags =   0

H0: Random walk without drift, d = 0

                                       Dickey–Fuller
                   Test      -------- critical value ---------
              statistic           1%           5%          10%
--------------------------------------------------------------
 Z(t)             2.725       -3.502       -2.888       -2.578
--------------------------------------------------------------
MacKinnon approximate p-value for Z(t) = 0.9991.
```

at 95% of significance, as `p > 0.05`, we fail to reject the null hypothesis.
Therefore, the data is not stationary.

---

### Step 6: First-order differencing

Command:

```
generate d_wpi = D.wpi
```

Output:

```
(1 missing value generated)
```

**Explanation for the missing value:** There will be one row which doesn't have another row to take differencing.

---

### Step 7: Augmented Dickey-Fuller Test on d_diff

Command:

```
dfuller d_diff
```

Output:

```
Dickey–Fuller test for unit root          Number of obs  = 122
Variable: d_wpi                           Number of lags =   0

H0: Random walk without drift, d = 0

                                       Dickey–Fuller
                   Test      -------- critical value ---------
              statistic           1%           5%          10%
--------------------------------------------------------------
 Z(t)            -4.661       -3.503       -2.889       -2.579
--------------------------------------------------------------
MacKinnon approximate p-value for Z(t) = 0.0001.
```

at 95% of significance, as `p > 0.05`, we reject the null hypothesis.
Therefore, the data is stationary.

---

### Step 8: ARIMA

Command:

```
arima d_wpi, arima (1, 1, 4)
```

Output:

```
(setting optimization to BHHH)
Iteration 0:  Log likelihood = -138.37392  
Iteration 1:  Log likelihood =  -136.6923  
Iteration 2:  Log likelihood = -135.16751  
Iteration 3:  Log likelihood = -134.86102  
Iteration 4:  Log likelihood = -134.78292  
(switching optimization to BFGS)
Iteration 5:  Log likelihood = -134.66002  
Iteration 6:  Log likelihood = -134.59466  
Iteration 7:  Log likelihood = -134.54565  
Iteration 8:  Log likelihood = -134.54115  
Iteration 9:  Log likelihood = -134.54056  
Iteration 10: Log likelihood = -134.54053  
Iteration 11: Log likelihood = -134.54053  

ARIMA regression

Sample: 1960q3 thru 1990q4                      Number of obs     =        122
                                                Wald chi2(5)      =      85.36
Log likelihood = -134.5405                      Prob > chi2       =     0.0000

------------------------------------------------------------------------------
             |                 OPG
     D.d_wpi | Coefficient  std. err.      z    P>|z|     [95% conf. interval]
-------------+----------------------------------------------------------------
d_wpi        |
       _cons |   .0185917   .0383504     0.48   0.628    -.0565737    .0937572
-------------+----------------------------------------------------------------
ARMA         |
          ar |
         L1. |   -.769201   .2848722    -2.70   0.007     -1.32754   -.2108617
             |
          ma |
         L1. |    .353302   .2955743     1.20   0.232    -.2260131     .932617
         L2. |  -.5030612   .1541581    -3.26   0.001    -.8052055   -.2009168
         L3. |  -.0851194     .10328    -0.82   0.410    -.2875446    .1173057
         L4. |   .1811093   .0843144     2.15   0.032     .0158561    .3463624
-------------+----------------------------------------------------------------
      /sigma |   .7275661   .0378414    19.23   0.000     .6533983    .8017339
------------------------------------------------------------------------------
Note: The test of the variance against zero is one sided, and the two-sided
      confidence interval is truncated at zero.
```

---

### Step 9: Interpretation of ARIMA

#### AR - L1

H<sub>00</sub>: The coefficient of AR - L1 is 0. (OR) There is no statistically significant difference between AR and L1.<br>
H<sub>10</sub>: There is a statistically significant difference between AR and L1.

Since `p < 0.05`, **We reject null hypothesis**.

#### MA - L1

H<sub>01</sub>: There is no statistically significant difference between MA and L1.<br>
H<sub>11</sub>: There is a statistically significant difference between MA and L1.

Since `p > 0.05`, **We fail to reject null hypothesis**.

#### MA - L2

H<sub>02</sub>: There is no statistically significant difference between MA and L2.<br>
H<sub>12</sub>: There is a statistically significant difference between MA and L2.

Since `p < 0.05`, **We reject null hypothesis**.

#### MA - L3

H<sub>03</sub>: There is no statistically significant difference between MA and L3.<br>
H<sub>13</sub>: There is a statistically significant difference between MA and L3.

Since `p > 0.05`, **We fail to reject null hypothesis**.

#### MA - L4

H<sub>04</sub>: There is no statistically significant difference between MA and L4.<br>
H<sub>14</sub>: There is a statistically significant difference between MA and L4.

Since `p < 0.05`, **We reject null hypothesis**.

#### Overall Model

H<sub>0</sub>: The overall model is not statistically significant.<br>
H<sub>1</sub>: The overall model is statistically significant.

Since `p < 0.05`, **We reject null hypothesis**.

Therefore, the overall model is statistically significant.

---

### Step 10: Generate residuals

Command:

```
predict res, residuals
```

Output:

```
(2 missing values generated)
```

### Step 11: Ljung-Box (Portmanteu) Test

Command:

```
wntestq res
```

Output:

```
Portmanteau test for white noise
---------------------------------------
 Portmanteau (Q) statistic =    39.2523
 Prob > chi2(40)           =     0.5037
```

H<sub>0</sub>: The residuals are white noise.
H<sub>1</sub>: The residuals are not white noise.

Since `p > 0.05`, we **fail to reject null hypothesis**.

The residuals are white noise. Therefore, the ARIMA(1,1,4) model is adequately specified.
