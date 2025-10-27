# Lab 7

**25/10/2025**

### Question 


### Commands Cheatsheet

```

```

### Step 1: Load Data

Command:

```
use https://www.stata-press.com/data/r19/nlswork.dta
```

### Step 2: Describe data

Command:

```
desc
```

Output:

```
Contains data from https://www.stata-press.com/data/r19/nlswork.dta
 Observations:        28,534                  National Longitudinal Survey of Young Women, 14-24 years old in 1968
    Variables:            21                  27 Nov 2024 08:14
                                              (_dta has notes)
--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
Variable      Storage   Display    Value
    name         type    format    label      Variable label
--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
idcode          int     %8.0g                 NLS ID
year            byte    %8.0g                 Interview year
birth_yr        byte    %8.0g                 Birth year
age             byte    %8.0g                 Age in current year
race            byte    %8.0g      racelbl    Race
msp             byte    %8.0g                 1 if married, spouse present
nev_mar         byte    %8.0g                 1 if never married
grade           byte    %8.0g                 Current grade completed
collgrad        byte    %8.0g                 1 if college graduate
not_smsa        byte    %8.0g                 1 if not SMSA
c_city          byte    %8.0g                 1 if central city
south           byte    %8.0g                 1 if south
ind_code        byte    %8.0g                 Industry of employment
occ_code        byte    %8.0g                 Occupation
union           byte    %8.0g                 1 if union
wks_ue          byte    %8.0g                 Weeks unemployed last year
ttl_exp         float   %9.0g                 Total work experience
tenure          float   %9.0g                 Job tenure, in years
hours           int     %8.0g                 Usual hours worked
wks_work        int     %8.0g                 Weeks worked last year
ln_wage         float   %9.0g                 ln(wage/GNP deflator)
--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
Sorted by: idcode  year
```

### Step 3: Declare dataset as panel data

Command:

```
xtset idcode year
```

Output:

```
Panel variable: idcode (unbalanced)
 Time variable: year, 68 to 88, but with gaps
         Delta: 1 unit
```

### Step 4: Describe Pattern of Data

Command:

```
xtdescribe
```

Output:

```
 idcode:  1, 2, ..., 5159                                   n =       4711
    year:  68, 69, ..., 88                                   T =         15
           Delta(year) = 1 unit
           Span(year)  = 21 periods
           (idcode*year uniquely identifies each observation)

Distribution of T_i:   min      5%     25%       50%       75%     95%     max
                         1       1       3         5         9      13      15

     Freq.  Percent    Cum. |  Pattern
 ---------------------------+-----------------------
      136      2.89    2.89 |  1....................
      114      2.42    5.31 |  ....................1
       89      1.89    7.20 |  .................1.11
       87      1.85    9.04 |  ...................11
       86      1.83   10.87 |  111111.1.11.1.11.1.11
       61      1.29   12.16 |  ..............11.1.11
       56      1.19   13.35 |  11...................
       54      1.15   14.50 |  ...............1.1.11
       54      1.15   15.64 |  .......1.11.1.11.1.11
     3974     84.36  100.00 | (other patterns)
 ---------------------------+-----------------------
     4711    100.00         |  XXXXXX.X.XX.X.XX.X.XX
```

### Step 5: Hypothesis

H<sub>01</sub>: There is no statistically significant difference between age and hours worked <br/>
H<sub>11</sub>: There is a statistically significant difference between age and hours worked

H<sub>02</sub>: There is no statistically significant difference between age and work experience <br/>
H<sub>12</sub>: There is a statistically significant difference between age and work experience

**Overall Model Hypothesis**

H<sub>0</sub>: The overall model is not a good fit <br/>
H<sub>1</sub>: The overall model is a good fit

### Step 6: Fit linear regression models (FE)


Command:

```
xtreg hours age ttl_exp, fe
```

Output:

```
Fixed-effects (within) regression               Number of obs     =     28,443
Group variable: idcode                          Number of groups  =      4,709

R-squared:                                      Obs per group:
     Within  = 0.0182                                         min =          1
     Between = 0.0387                                         avg =        6.0
     Overall = 0.0315                                         max =         15

                                                F(2, 23732)       =     220.09
corr(u_i, Xb) = -0.0784                         Prob > F          =     0.0000

------------------------------------------------------------------------------
       hours | Coefficient  Std. err.      t    P>|t|     [95% conf. interval]
-------------+----------------------------------------------------------------
         age |   -.466718   .0229773   -20.31   0.000    -.5117549   -.4216811
     ttl_exp |   .7055425   .0340991    20.69   0.000     .6387062    .7723789
       _cons |   45.72587   .4839007    94.49   0.000     44.77739    46.67435
-------------+----------------------------------------------------------------
     sigma_u |  7.7275136
     sigma_e |  8.1581434
         rho |  .47291182   (fraction of variance due to u_i)
------------------------------------------------------------------------------
F test that all u_i=0: F(4708, 23732) = 3.51                 Prob > F = 0.0000
```

**Store estimates**

Command:

```
estimates store fe_model
```

### Step 7: Fit linear regression models (RE)

Command:

```
xtreg hours age ttl_exp, re
```

Output:

```
Random-effects GLS regression                   Number of obs     =     28,443
Group variable: idcode                          Number of groups  =      4,709

R-squared:                                      Obs per group:
     Within  = 0.0178                                         min =          1
     Between = 0.0416                                         avg =        6.0
     Overall = 0.0324                                         max =         15

                                                Wald chi2(2)      =     645.89
corr(u_i, X) = 0 (assumed)                      Prob > chi2       =     0.0000

------------------------------------------------------------------------------
       hours | Coefficient  Std. err.      z    P>|z|     [95% conf. interval]
-------------+----------------------------------------------------------------
         age |   -.389292   .0167834   -23.20   0.000    -.4221868   -.3563971
     ttl_exp |   .6262849   .0247944    25.26   0.000     .5776888    .6748811
       _cons |   43.90606   .3834806   114.49   0.000     43.15445    44.65767
-------------+----------------------------------------------------------------
     sigma_u |  6.1911257
     sigma_e |  8.1581434
         rho |  .36544704   (fraction of variance due to u_i)
------------------------------------------------------------------------------
```

**Store estimates**

Command:

```
estimates store re_model
```

### Step 8: Hausman Test

Command:

```
hausman fe_model re_model
```

Output:

```
                 ---- Coefficients ----
             |      (b)          (B)            (b-B)     sqrt(diag(V_b-V_B))
             |    fe_model     re_model      Difference       Std. err.
-------------+----------------------------------------------------------------
         age |    -.466718     -.389292        -.077426         .015693
     ttl_exp |    .7055425     .6262849        .0792576        .0234091
------------------------------------------------------------------------------
                          b = Consistent under H0 and Ha; obtained from xtreg.
           B = Inconsistent under Ha, efficient under H0; obtained from xtreg.

Test of H0: Difference in coefficients not systematic

    chi2(2) = (b-B)'[(V_b-V_B)^(-1)](b-B)
            =  59.72
Prob > chi2 = 0.0000
```

---

## ...to be continued...
