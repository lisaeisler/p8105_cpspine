Data\_Analysis\_CPSPINE
================

``` r
knitr::opts_chunk$set(echo = TRUE)
library(tidyverse)
```

    ## Warning in as.POSIXlt.POSIXct(Sys.time()): unknown timezone 'zone/tz/2019c.
    ## 1.0/zoneinfo/America/New_York'

    ## ── Attaching packages ────────────────────────────────────────────────────────── tidyverse 1.2.1 ──

    ## ✔ ggplot2 3.2.1     ✔ purrr   0.3.3
    ## ✔ tibble  2.1.3     ✔ dplyr   0.8.3
    ## ✔ tidyr   1.0.0     ✔ stringr 1.4.0
    ## ✔ readr   1.1.1     ✔ forcats 0.4.0

    ## ── Conflicts ───────────────────────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()

``` r
library(haven)
library(viridis)
```

    ## Loading required package: viridisLite

## Data import

``` r
nsqipspineCP_1617 = read_csv("./nsqipspineCP_1617.csv")
```

## Initial Data Cleaning & Selection of a subset of data to examine the purpose of the study (From Jerri’s file we can ommit in the report once we double check that it is included on Jerri’s side)

``` r
cp_spine_tidy = nsqipspineCP_1617 %>%
  mutate_if(is.numeric, ~replace(., . == -99, NA)) %>%
  mutate(
    age_years = age_days/365.25,
    height = height*2.54,
    weight = weight/2.2) %>%
  mutate(
    bmi = weight/((height/100)*(height/100)),
    asa_status = case_when(
      asaclas == "ASA 1 - No Disturb" ~ "1",
      asaclas == "ASA 2 - Mild Disturb" ~ "2",
      asaclas == "ASA 3 - Severe Disturb" ~ "3",
      asaclas == "ASA 4 - Life Threat" ~ "4",
      asaclas == "None assigned" ~ "NA"),
    home_discharge = case_when(
      dischdest == "Expired" ~ "FALSE",
      dischdest == "Facility Which was Home" ~ "TRUE",
      dischdest == "Home" ~ "TRUE",
      dischdest == "Rehab" ~ "FALSE",
      dischdest == "Separate Acute Care" ~ "FALSE",
      dischdest == "Skilled Care, Not Home" ~ "FALSE",
      dischdest == "Unknown" ~ "NA",
      dischdest == "Unskilled Facility Not Home" ~ "FALSE",
      dischdest == "NULL" ~ "NA"),
    level_13 = case_when(
      prncptx == "ARTHRODESIS, ANTERIOR, FOR SPINAL DEFORMITY, WITH OR WITHOUT CAST; 2 TO 3 VERTEBRAL SEGMENTS" ~ "FALSE",
      prncptx == "ARTHRODESIS, ANTERIOR, FOR SPINAL DEFORMITY, WITH OR WITHOUT CAST; 4 TO 7 VERTEBRAL SEGMENTS" ~ "FALSE",
      prncptx == "ARTHRODESIS, ANTERIOR, FOR SPINAL DEFORMITY, WITH OR WITHOUT CAST; 8 OR MORE VERTEBRAL SEGMENTS" ~ "FALSE",
      prncptx == "ARTHRODESIS, POSTERIOR, FOR SPINAL DEFORMITY, WITH OR WITHOUT CAST; UP TO 6 VERTEBRAL SEGMENTS" ~ "FALSE",
      prncptx == "ARTHRODESIS, POSTERIOR, FOR SPINAL DEFORMITY, WITH OR WITHOUT CAST; 7 TO 12 VERTEBRAL SEGMENTS" ~ "FALSE",
      prncptx == "ARTHRODESIS, POSTERIOR, FOR SPINAL DEFORMITY, WITH OR WITHOUT CAST; 13 OR MORE VERTEBRAL SEGMENTS" ~ "TRUE"))  %>% 
  filter(home_discharge != "NA") %>% 
  select(pufyear_x:ped_spn_post_neurodeftype, age_years, sex, height, weight, bmi, ethnicity_hispanic, race, asa_status, transt, ventilat, asthma, hxcld, oxygen_sup, crf, impcogstat, seizure, nutr_support, hemodisorder, level_13, optime, tothlos, d_opto_dis, death30yn, supinfec, wndinfd, orgspcssi, dehis, oupneumo, pulembol, renainsf, urninfec, cszre, neurodef, cdarrest, othbleed, bleed_ml_tot, othcdiff, othsysep, unplannedreadmission1, reoperation, dischdest, home_discharge)
```

NOTE: THE ABOVE SECTION IS THE SAME AS IN DATA VISUALIZATION NOT NEEDED
IN THE FINAL REPORT \#\# Analysis and models Our goal is to develop a
model,in which we can use important pre-operative factors to predict the
patient’s discharge at home. \#\# Selection of predictors to include in
the initial model Data processing, elimination of missing data, and data
selection for the model. We observe that all cp patients in the dataset
were patient who came from home.

``` r
model_data = cp_spine_tidy %>% 
  mutate(
    sex1=case_when(
    sex=="Male"~"0",
    sex=="Female"~"1"),
    sex1=as.factor(sex1),
    race = case_when(
    race == "American Indian,Alaskan Native" ~ "1",
    race == "Asian" ~ "2",
    race == "Black or African American" ~ "3",
    race == "Native Hawaiian or Other Pacific Islander" ~ "4",
    race == "White" ~ "5",
    race == "Unknown Not Reported" ~ "6"),
    race1=as.factor(race),
    asa_level = ifelse(asa_status >2, 1, 0),
    asa_level=as.factor(asa_level),
    ventilat1=case_when(
    ventilat=="No"~"0",
    ventilat=="Yes"~"1"),
    ventilat1=as.factor(ventilat1), 
  asthma1=case_when(
    asthma=="No"~"0",
    asthma=="Yes"~"1"),
  asthma1=as.factor(asthma1),
  hxcld1=case_when(
    hxcld=="No"~"0",
    hxcld=="Yes"~"1"),
  hxcld1=as.factor(hxcld1),
  oxygen_sup1=case_when(
    oxygen_sup=="No"~"0",
    oxygen_sup=="Yes"~"1"),
  oxygen_sup1=as.factor(oxygen_sup1),
  seizure1=case_when(
    seizure=="No"~"0",
    seizure=="Yes"~"1"),
  seizure1=as.factor(seizure1),
  nutr_support1=case_when(
    nutr_support=="No"~"0",
    nutr_support=="Yes"~"1"),
  nutr_support1=as.factor(nutr_support1),
hemodisorder1=case_when(
    hemodisorder=="No"~"0",
    hemodisorder=="Yes"~"1"),
  hemodisorder1=as.factor(hemodisorder1),
 level_13=as.factor(level_13),
 home_discharge1=case_when(
   home_discharge=="TRUE"~"1",
   home_discharge=="FALSE"~"0"),
home_discharge1=as.factor(home_discharge1), 
  crf1 = case_when(
      crf == "Major cardiac risk factors" ~ "FALSE",
      crf == "Severe cardiac risk factors" ~"FALSE",
      crf == "Minor cardiac risk factors" ~ "TRUE",
      crf == "No cardiac risk factors" ~ "TRUE"), 
  crf1=as.factor(crf1),
  transt1 = case_when(
      transt == "Chronic care Rehab/Intermediate Care/Spinal Cord" ~ "FALSE",
      transt == "Transferred from outside hospital (NICU, PICU, Inpatient on General floor, Adult" ~"FALSE",
      transt == "other" ~ "FALSE",
      transt == "Admitted from home/clinic/doctor's office" ~ "TRUE",
      transt == "Admitted through ER, including outside ER with direct hospital admission" ~ "TRUE"),
  transt1=as.factor(transt1))%>%
  select(age_years, bmi, weight, sex1, transt1, crf1, race1, asa_level, ventilat1, asthma1, hxcld1, oxygen_sup1, seizure1, nutr_support1, hemodisorder1, level_13, home_discharge1) 
```

Overall quick descriptives for the data entered in the model suggest
that all cp patients came from
    home.

``` r
summary(model_data)
```

    ##    age_years          bmi              weight       sex1    transt1   
    ##  Min.   : 3.34   Min.   :  1.577   Min.   :  2.00   0:398   TRUE:798  
    ##  1st Qu.:11.28   1st Qu.: 15.321   1st Qu.: 26.80   1:424   NA's: 24  
    ##  Median :13.28   Median : 17.540   Median : 32.00                     
    ##  Mean   :13.13   Mean   : 18.907   Mean   : 33.58                     
    ##  3rd Qu.:15.18   3rd Qu.: 20.114   3rd Qu.: 38.90                     
    ##  Max.   :17.99   Max.   :238.140   Max.   :107.00                     
    ##                  NA's   :125       NA's   :5                          
    ##     crf1      race1     asa_level ventilat1 asthma1 hxcld1  oxygen_sup1
    ##  FALSE: 53   2   : 24   0: 77     0:739     0:631   0:670   0:761      
    ##  TRUE :769   3   :203   1:745     1: 83     1:191   1:152   1: 61      
    ##              4   :  4                                                  
    ##              5   :501                                                  
    ##              NA's: 90                                                  
    ##                                                                        
    ##                                                                        
    ##  seizure1 nutr_support1 hemodisorder1  level_13   home_discharge1
    ##  0:302    0:406         0:770         FALSE:196   0: 53          
    ##  1:520    1:416         1: 52         TRUE :626   1:769          
    ##                                                                  
    ##                                                                  
    ##                                                                  
    ##                                                                  
    ## 

``` r
xtabs(~ home_discharge1+ transt1, data = model_data)
```

    ##                transt1
    ## home_discharge1 TRUE
    ##               0   51
    ##               1  747

2.  Two-way contingency tables of categorical outcome and predictors we
    want \#\# to make sure there are no cells with n=0

<!-- end list -->

``` r
xtabs(~ home_discharge1+ sex1, data = model_data)
```

    ##                sex1
    ## home_discharge1   0   1
    ##               0  25  28
    ##               1 373 396

``` r
xtabs(~ home_discharge1+ race1, data = model_data)
```

    ##                race1
    ## home_discharge1   2   3   4   5
    ##               0   3  15   0  25
    ##               1  21 188   4 476

\#Race cross tab with home\_dicharge1 has 1 cell with 0 data point and
two more cells with 15 and bellow data points. Thus race will be
excluded from the model.

``` r
xtabs(~ home_discharge1+ asa_level, data = model_data)
```

    ##                asa_level
    ## home_discharge1   0   1
    ##               0   2  51
    ##               1  75 694

  - Asa\_level has a cell with 2 data points we need to take out from
    the model (unstable model.

<!-- end list -->

``` r
xtabs(~ home_discharge1+ ventilat1, data = model_data)
```

    ##                ventilat1
    ## home_discharge1   0   1
    ##               0  48   5
    ##               1 691  78

  - cell with less than 5 data points may consider to take out (unstable
    model)

<!-- end list -->

``` r
xtabs(~ home_discharge1+ asthma1, data = model_data)
```

    ##                asthma1
    ## home_discharge1   0   1
    ##               0  39  14
    ##               1 592 177

``` r
xtabs(~ home_discharge1+ hxcld1, data = model_data)
```

    ##                hxcld1
    ## home_discharge1   0   1
    ##               0  44   9
    ##               1 626 143

``` r
xtabs(~ home_discharge1+ oxygen_sup1, data = model_data)
```

    ##                oxygen_sup1
    ## home_discharge1   0   1
    ##               0  50   3
    ##               1 711  58

  - we have a cell with only 3 counts may consider to take out.

<!-- end list -->

``` r
xtabs(~ home_discharge1+ seizure1, data = model_data)
```

    ##                seizure1
    ## home_discharge1   0   1
    ##               0  20  33
    ##               1 282 487

``` r
xtabs(~ home_discharge1+ nutr_support1, data = model_data)
```

    ##                nutr_support1
    ## home_discharge1   0   1
    ##               0  24  29
    ##               1 382 387

``` r
xtabs(~ home_discharge1+ hemodisorder1, data = model_data)
```

    ##                hemodisorder1
    ## home_discharge1   0   1
    ##               0  44   9
    ##               1 726  43

``` r
xtabs(~ home_discharge1+ crf1, data = model_data)
```

    ##                crf1
    ## home_discharge1 FALSE TRUE
    ##               0     6   47
    ##               1    47  722

``` r
xtabs(~ home_discharge1+ level_13, data = model_data)
```

    ##                level_13
    ## home_discharge1 FALSE TRUE
    ##               0    10   43
    ##               1   186  583

Initially there were 19 pre-operative variables that could predict the
main outcome `home_discharge` To select the variables for our model we
used findings from our literature review and results from preliminary
analysis (crosstabs). From groups of variables that were highly
correlated with each other, we selected one variable that was supported
from the literature. For example, we used weight instead of height and
bmi, as height is a non reliable measure for individuals with CP and has
a lot of missing values in the dataset. After the cross tabs exploratory
analysis (see CPspine extensive), we eliminated variables that presented
with 0 data in at least one of the cells. We combined multilevel
variables to binary variables (i.e. crf) due to small sample size. We
finally eliminated variables such as `ethnicity` because there was a
large number of patients who self-identified as “Other” and it is
unclear how to use “Other” to create preditions. Therefore, our full
regression model consisted of 12 variable. \#\# Logistic Regression
Models Full
Model

``` r
mylogit <- glm(home_discharge1 ~ age_years+ weight+ sex1 + crf1+ventilat1 + asthma1 + hxcld1 + oxygen_sup1 + seizure1 + nutr_support1 + hemodisorder1+ level_13, data = model_data, family = "binomial")
summary(mylogit)
```

    ## 
    ## Call:
    ## glm(formula = home_discharge1 ~ age_years + weight + sex1 + crf1 + 
    ##     ventilat1 + asthma1 + hxcld1 + oxygen_sup1 + seizure1 + nutr_support1 + 
    ##     hemodisorder1 + level_13, family = "binomial", data = model_data)
    ## 
    ## Deviance Residuals: 
    ##     Min       1Q   Median       3Q      Max  
    ## -3.0162   0.2509   0.3235   0.4005   0.8834  
    ## 
    ## Coefficients:
    ##                  Estimate Std. Error z value Pr(>|z|)    
    ## (Intercept)     5.3700954  1.1200573   4.794 1.63e-06 ***
    ## age_years      -0.2035355  0.0666779  -3.053  0.00227 ** 
    ## weight         -0.0003505  0.0152235  -0.023  0.98163    
    ## sex11          -0.1489654  0.2966454  -0.502  0.61555    
    ## crf1TRUE        0.6684369  0.4816342   1.388  0.16518    
    ## ventilat11     -0.0316349  0.5458241  -0.058  0.95378    
    ## asthma11       -0.3531750  0.3620956  -0.975  0.32938    
    ## hxcld11         0.3057868  0.4211842   0.726  0.46783    
    ## oxygen_sup11    0.5607343  0.6906871   0.812  0.41688    
    ## seizure11       0.0025725  0.3257585   0.008  0.99370    
    ## nutr_support11 -0.3505018  0.3234973  -1.083  0.27860    
    ## hemodisorder11 -1.3329745  0.4203592  -3.171  0.00152 ** 
    ## level_13TRUE   -0.1450524  0.3789902  -0.383  0.70192    
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## (Dispersion parameter for binomial family taken to be 1)
    ## 
    ##     Null deviance: 392.43  on 816  degrees of freedom
    ## Residual deviance: 368.55  on 804  degrees of freedom
    ##   (5 observations deleted due to missingness)
    ## AIC: 394.55
    ## 
    ## Number of Fisher Scoring iterations: 6

## Reduced model based on the cross tabs above (cells with counts \<=5)

``` r
mylogit <- glm(home_discharge1 ~ age_years+ weight+ sex1+ asthma1 +crf1+ hxcld1 + seizure1 + nutr_support1 + hemodisorder1+ level_13, data = model_data, family = "binomial")
summary(mylogit)
```

    ## 
    ## Call:
    ## glm(formula = home_discharge1 ~ age_years + weight + sex1 + asthma1 + 
    ##     crf1 + hxcld1 + seizure1 + nutr_support1 + hemodisorder1 + 
    ##     level_13, family = "binomial", data = model_data)
    ## 
    ## Deviance Residuals: 
    ##     Min       1Q   Median       3Q      Max  
    ## -3.0199   0.2533   0.3226   0.3983   1.0316  
    ## 
    ## Coefficients:
    ##                  Estimate Std. Error z value Pr(>|z|)    
    ## (Intercept)     5.2937661  1.1123973   4.759 1.95e-06 ***
    ## age_years      -0.2026850  0.0664456  -3.050  0.00229 ** 
    ## weight          0.0003142  0.0152806   0.021  0.98359    
    ## sex11          -0.1566058  0.2965740  -0.528  0.59746    
    ## asthma11       -0.2849236  0.3556209  -0.801  0.42302    
    ## crf1TRUE        0.6969559  0.4799218   1.452  0.14644    
    ## hxcld11         0.3457859  0.4130185   0.837  0.40247    
    ## seizure11       0.0078499  0.3263220   0.024  0.98081    
    ## nutr_support11 -0.3174756  0.3209992  -0.989  0.32265    
    ## hemodisorder11 -1.2958398  0.4174579  -3.104  0.00191 ** 
    ## level_13TRUE   -0.1409626  0.3785483  -0.372  0.70961    
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## (Dispersion parameter for binomial family taken to be 1)
    ## 
    ##     Null deviance: 392.43  on 816  degrees of freedom
    ## Residual deviance: 369.31  on 806  degrees of freedom
    ##   (5 observations deleted due to missingness)
    ## AIC: 391.31
    ## 
    ## Number of Fisher Scoring iterations: 6

## Reduced based on literature and visualization

``` r
mylogit <- glm(home_discharge1 ~ age_years+ weight+hemodisorder1+crf1, data = model_data, family = "binomial")
summary(mylogit)
```

    ## 
    ## Call:
    ## glm(formula = home_discharge1 ~ age_years + weight + hemodisorder1 + 
    ##     crf1, family = "binomial", data = model_data)
    ## 
    ## Deviance Residuals: 
    ##     Min       1Q   Median       3Q      Max  
    ## -2.7900   0.2577   0.3239   0.3917   0.8581  
    ## 
    ## Coefficients:
    ##                 Estimate Std. Error z value Pr(>|z|)    
    ## (Intercept)     4.636257   0.946919   4.896 9.77e-07 ***
    ## age_years      -0.189979   0.063500  -2.992  0.00277 ** 
    ## weight          0.003662   0.015121   0.242  0.80865    
    ## hemodisorder11 -1.266583   0.405782  -3.121  0.00180 ** 
    ## crf1TRUE        0.697471   0.471261   1.480  0.13887    
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## (Dispersion parameter for binomial family taken to be 1)
    ## 
    ##     Null deviance: 392.43  on 816  degrees of freedom
    ## Residual deviance: 372.13  on 812  degrees of freedom
    ##   (5 observations deleted due to missingness)
    ## AIC: 382.13
    ## 
    ## Number of Fisher Scoring iterations: 6

Futhermore, we tested which of the two models (Full or Reduced) fits
better. We see the deviance residuals, which are a measure of model fit.
This part of output shows the distribution of the deviance residuals for
individual cases used in the model. Compared to the other two models
this fits better. AIC is also better (lower). Age variable is
significant at a=.01, the pre-existance of a hematologic disorder
variable is significant at a=.01. In addition, we wanted to see if our
model fits the data significantly better than a model with just the
intercept (a null model).

We use the null and deviance residuals

``` r
with(mylogit, null.deviance - deviance)
```

    ## [1] 20.29957

with Df

``` r
with(mylogit, df.null - df.residual)
```

    ## [1] 4

We compute the Chi-Square
test

``` r
with(mylogit, pchisq(null.deviance - deviance, df.null - df.residual, lower.tail = FALSE))
```

    ## [1] 0.0004357827

The chi-square provides a p-value=0.000367 (p\< 0.001), thus, our model
as a whole fits significantly better than the null model.

We can use the likelihood ratio test (-2\*log likelihood) to see the
model’s log likelihood:

``` r
logLik(mylogit)
```

    ## 'log Lik.' -186.0661 (df=5)

Preliminary intrepretation 1. For every 1 year increase in age the log
odds of homedischarge (versus discharge to other healthcare facilities)
decreases by 0.19. 2. Having a pre-existing hematologic disorder
decreases the log odds of being discharged to home by 1.275. \#\#
Confidence intervals for the log odds coeficients (log-likelihood
function)

``` r
confint(mylogit)
```

    ##                      2.5 %      97.5 %
    ## (Intercept)     2.86263258  6.58371920
    ## age_years      -0.31735438 -0.06811205
    ## weight         -0.02406641  0.03490964
    ## hemodisorder11 -2.02655603 -0.41853815
    ## crf1TRUE       -0.32129219  1.55642219

## Confidence intervals using the standard errors

``` r
confint.default(mylogit)
```

    ##                      2.5 %      97.5 %
    ## (Intercept)     2.78032984  6.49218350
    ## age_years      -0.31443545 -0.06552164
    ## weight         -0.02597433  0.03329778
    ## hemodisorder11 -2.06190169 -0.47126364
    ## crf1TRUE       -0.22618344  1.62112628

### Exponantiate to odds ratios and Confidence intervals

``` r
exp(cbind(OR = coef(mylogit), confint(mylogit)))
```

    ##                         OR      2.5 %      97.5 %
    ## (Intercept)    103.1574716 17.5075564 723.2241522
    ## age_years        0.8269769  0.7280727   0.9341558
    ## weight           1.0036684  0.9762209   1.0355261
    ## hemodisorder11   0.2817930  0.1317886   0.6580080
    ## crf1TRUE         2.0086672  0.7252113   4.7418255

\#\#Data interpretation For every one year increase in age, the odds of
being discharged home (versus being discharged to another health care
facility) increase by 0.83. This means that for every year increase in
age the odds of being release to a health care facility (not home)
increase by (1/.83=1.2) by 20%. The odds of being discharged at home if
you have a hemodisorder are 0.279.This means that if the patient has a a
pre-existing hematologic disorder the odds of being discharged to a
health care facility are (1/.279=3.58) 3.58 the odds of being discharged
to home.
