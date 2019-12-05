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

## Initial Data Cleaning & Selection of a subset of data to examine the purpose of the study

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

## Analysis and models

Given the sample size (small) our analysis goal is to develop a model of
possible assosiations between pre-surgical indicators and the outcome.
We use crosstabs and chisquare tests to examine associations between
each of the indicators and the outcome (Preliminary analysis) and
logistic regression to build our models.

Data processing, elimination of missing data, and data selection for the
analysis.

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
impcogstat1=case_when(
    impcogstat=="No"~"0",
    impcogstat=="Yes"~"1"),
  impcogstat1=as.factor(impcogstat),
 level_13=as.factor(level_13),
 home_discharge1=case_when(
   home_discharge=="TRUE"~"1",
   home_discharge=="FALSE"~"0"),
home_discharge1=as.factor(home_discharge1), 
  crf1 = case_when(
      crf == "Major cardiac risk factors" ~ "TRUE",
      crf == "Severe cardiac risk factors" ~"TRUE",
      crf == "Minor cardiac risk factors" ~ "FALSE",
      crf == "No cardiac risk factors" ~ "FALSE"), 
  crf1=as.factor(crf1),
  transt1 = case_when(
      transt == "Chronic care Rehab/Intermediate Care/Spinal Cord" ~ "FALSE",
      transt == "Transferred from outside hospital (NICU, PICU, Inpatient on General floor, Adult" ~"FALSE",
      transt == "other" ~ "FALSE",
      transt == "Admitted from home/clinic/doctor's office" ~ "TRUE",
      transt == "Admitted through ER, including outside ER with direct hospital admission" ~ "TRUE"),
  transt1=as.factor(transt1))%>%
  select(age_years, bmi, weight, height, sex1, transt1, crf1, race1, asa_level, ventilat1, asthma1,impcogstat1, hxcld1, oxygen_sup1, seizure1, nutr_support1, hemodisorder1, level_13, home_discharge1) 
```

Checking the data with quick
    descriptives

``` r
summary(model_data)
```

    ##    age_years          bmi              weight           height      
    ##  Min.   : 3.34   Min.   :  1.577   Min.   :  2.00   Min.   : 40.42  
    ##  1st Qu.:11.28   1st Qu.: 15.321   1st Qu.: 26.80   1st Qu.:124.82  
    ##  Median :13.28   Median : 17.540   Median : 32.00   Median :135.22  
    ##  Mean   :13.13   Mean   : 18.907   Mean   : 33.58   Mean   :134.84  
    ##  3rd Qu.:15.18   3rd Qu.: 20.114   3rd Qu.: 38.90   3rd Qu.:146.90  
    ##  Max.   :17.99   Max.   :238.140   Max.   :107.00   Max.   :193.17  
    ##                  NA's   :125       NA's   :5        NA's   :123     
    ##  sex1    transt1       crf1      race1     asa_level ventilat1 asthma1
    ##  0:398   TRUE:798   FALSE:769   2   : 24   0: 77     0:739     0:631  
    ##  1:424   NA's: 24   TRUE : 53   3   :203   1:745     1: 83     1:191  
    ##                                 4   :  4                              
    ##                                 5   :501                              
    ##                                 NA's: 90                              
    ##                                                                       
    ##                                                                       
    ##  impcogstat1 hxcld1  oxygen_sup1 seizure1 nutr_support1 hemodisorder1
    ##  No : 73     0:670   0:761       0:302    0:406         0:770        
    ##  Yes:749     1:152   1: 61       1:520    1:416         1: 52        
    ##                                                                      
    ##                                                                      
    ##                                                                      
    ##                                                                      
    ##                                                                      
    ##   level_13   home_discharge1
    ##  FALSE:196   0: 53          
    ##  TRUE :626   1:769          
    ##                             
    ##                             
    ##                             
    ##                             
    ## 

\#\#Preliminary Analysis Crosstabs & Chisqaure tests of association of
each of the indicators with the outcome. Two-way contingency tables of
categorical outcome and predictors identified. Ensure that there no
cells with counts \<=5 and compute Chi-square tests to examine the
association of each of the indicators to the outcome.

\#\#Sex

``` r
xtabs(~ home_discharge1+ sex1, data = model_data)
```

    ##                sex1
    ## home_discharge1   0   1
    ##               0  25  28
    ##               1 373 396

no cells with counts\<=5

``` r
tbl<-xtabs(~ home_discharge1+ sex1, data = model_data)
chisq.test(tbl) 
```

    ## 
    ##  Pearson's Chi-squared test with Yates' continuity correction
    ## 
    ## data:  tbl
    ## X-squared = 0.0021141, df = 1, p-value = 0.9633

Chi-square test reveals no association between Sex and Home\_discharge
(p\>.05).

## Race

``` r
xtabs(~ home_discharge1+ race1, data = model_data)
```

    ##                race1
    ## home_discharge1   2   3   4   5
    ##               0   3  15   0  25
    ##               1  21 188   4 476

Race cross tab with home\_dicharge1 has 1 cell with 0 data point and two
more cells with 15 and bellow data points. Thus race will be excluded
from the model.

## ASA Classification (ASA\_level)

``` r
xtabs(~ home_discharge1+ asa_level, data = model_data)
```

    ##                asa_level
    ## home_discharge1   0   1
    ##               0   2  51
    ##               1  75 694

Asa\_level has a cell with 2 data points we need to take out from the
model (unstable model.

## Ventilator Dependence (Ventilat1)

``` r
xtabs(~ home_discharge1+ ventilat1, data = model_data)
```

    ##                ventilat1
    ## home_discharge1   0   1
    ##               0  48   5
    ##               1 691  78

Cell with less than 5 data points may consider to take out (unstable
model)

## Asthma1

``` r
xtabs(~ home_discharge1+ asthma1, data = model_data)
```

    ##                asthma1
    ## home_discharge1   0   1
    ##               0  39  14
    ##               1 592 177

no cells with counts\<=5

``` r
tbl<-xtabs(~ home_discharge1+ asthma1, data = model_data)
chisq.test(tbl) 
```

    ## 
    ##  Pearson's Chi-squared test with Yates' continuity correction
    ## 
    ## data:  tbl
    ## X-squared = 0.15875, df = 1, p-value = 0.6903

Chi-square test reveals no association between Asthma1 and
Home\_discharge (p\>.05).

## Chronic lung disease (hxcld1)

``` r
xtabs(~ home_discharge1+ hxcld1, data = model_data)
```

    ##                hxcld1
    ## home_discharge1   0   1
    ##               0  44   9
    ##               1 626 143

no cells with counts\<=5

``` r
tbl<-xtabs(~ home_discharge1+ hxcld1, data = model_data)
chisq.test(tbl) 
```

    ## 
    ##  Pearson's Chi-squared test with Yates' continuity correction
    ## 
    ## data:  tbl
    ## X-squared = 0.012082, df = 1, p-value = 0.9125

Chi-square test reveals no association between hxcld1 and
Home\_discharge (p\>.05).

\#\#Oxygen support (oxygen\_sup1)

``` r
xtabs(~ home_discharge1+ oxygen_sup1, data = model_data)
```

    ##                oxygen_sup1
    ## home_discharge1   0   1
    ##               0  50   3
    ##               1 711  58

we have a cell with only 3 counts, will not include.

## Seizures

``` r
xtabs(~ home_discharge1+ seizure1, data = model_data)
```

    ##                seizure1
    ## home_discharge1   0   1
    ##               0  20  33
    ##               1 282 487

no cells with counts\<=5

``` r
tbl<-xtabs(~ home_discharge1+ seizure1, data = model_data)
chisq.test(tbl) 
```

    ## 
    ##  Pearson's Chi-squared test with Yates' continuity correction
    ## 
    ## data:  tbl
    ## X-squared = 6.7938e-05, df = 1, p-value = 0.9934

Chi-square test reveals no association between seizure1 and
Home\_discharge (p\>.05).

\#\#Nutrition support (nutr\_support1)

``` r
xtabs(~ home_discharge1+ nutr_support1, data = model_data)
```

    ##                nutr_support1
    ## home_discharge1   0   1
    ##               0  24  29
    ##               1 382 387

no cells with counts\<=5

``` r
tbl<-xtabs(~ home_discharge1+ nutr_support1, data = model_data)
chisq.test(tbl) 
```

    ## 
    ##  Pearson's Chi-squared test with Yates' continuity correction
    ## 
    ## data:  tbl
    ## X-squared = 0.22708, df = 1, p-value = 0.6337

Chi-square test reveals no association between nutr\_support1 and
Home\_discharge (p\>.05).

\#Pre-existing hemotological disorder (hemodisorder1)

``` r
xtabs(~ home_discharge1+ hemodisorder1, data = model_data)
```

    ##                hemodisorder1
    ## home_discharge1   0   1
    ##               0  44   9
    ##               1 726  43

no cells with counts\<=5

``` r
tbl<-xtabs(~ home_discharge1+ hemodisorder1, data = model_data)
chisq.test(tbl) 
```

    ## 
    ##  Pearson's Chi-squared test with Yates' continuity correction
    ## 
    ## data:  tbl
    ## X-squared = 9.017, df = 1, p-value = 0.002675

Chi-square test reveals a significant association between hemodisorder1
and Home\_discharge (p\<.05).

## Cardiac Risk Factor (crf1)

``` r
xtabs(~ home_discharge1+ crf1, data = model_data)
```

    ##                crf1
    ## home_discharge1 FALSE TRUE
    ##               0    47    6
    ##               1   722   47

no cells with counts\<=5

``` r
tbl<-xtabs(~ home_discharge1+ crf1, data = model_data)
chisq.test(tbl) 
```

    ## 
    ##  Pearson's Chi-squared test with Yates' continuity correction
    ## 
    ## data:  tbl
    ## X-squared = 1.4504, df = 1, p-value = 0.2285

Chi-square test reveals no significant association between crf1 and
Home\_discharge (p\>.05).

## Spine fusion level\_13

``` r
xtabs(~ home_discharge1+ level_13, data = model_data)
```

    ##                level_13
    ## home_discharge1 FALSE TRUE
    ##               0    10   43
    ##               1   186  583

no cells with counts\<=5

``` r
tbl<-xtabs(~ home_discharge1+ level_13, data = model_data)
chisq.test(tbl) 
```

    ## 
    ##  Pearson's Chi-squared test with Yates' continuity correction
    ## 
    ## data:  tbl
    ## X-squared = 0.50744, df = 1, p-value = 0.4763

Chi-square test reveals no significant association between crf1 and
Home\_discharge (p\>.05).

Selection of variables for the Full Model Initially there were 18
pre-operative indicators to be considered for the main outcome
`home_discharge` To select the variables for our full model we used the
following criteria:

1)  From groups of variables that were highly correlated with each
    other, we selected one variable that was supported from the
    literature. For example, we used weight instead of height and bmi,
    as height is a non reliable measure for individuals with CP and has
    a lot of missing values in the dataset.

2)  From the preliminary analysis (crosstabs) we identified and
    eliminated variables that presented with cell counts \<=5. Thus,
    `race`, `asa_level1`, `ventilat1` and `oxygen_sup1` were not
    included in the model

3)  We finally eliminated variables such as `ethnicity` because there
    was a large number of patients who self-identified as “Other” and it
    is unclear how to use “Other” to create preditions.

Therefore, our full regression model consisted of 10 variables.
\#\#Logistic Regression Models Full
Model

``` r
mylogit <- glm(home_discharge1 ~ age_years+ weight+ sex1 + crf1+asthma1 +impcogstat1+ hxcld1+ seizure1 + nutr_support1 + hemodisorder1+ level_13, data = model_data, family = "binomial")
summary(mylogit)
```

    ## 
    ## Call:
    ## glm(formula = home_discharge1 ~ age_years + weight + sex1 + crf1 + 
    ##     asthma1 + impcogstat1 + hxcld1 + seizure1 + nutr_support1 + 
    ##     hemodisorder1 + level_13, family = "binomial", data = model_data)
    ## 
    ## Deviance Residuals: 
    ##     Min       1Q   Median       3Q      Max  
    ## -3.0009   0.2542   0.3215   0.3979   1.0252  
    ## 
    ## Coefficients:
    ##                  Estimate Std. Error z value Pr(>|z|)    
    ## (Intercept)     6.2997088  1.1840824   5.320 1.04e-07 ***
    ## age_years      -0.1995969  0.0666931  -2.993  0.00276 ** 
    ## weight         -0.0003436  0.0153175  -0.022  0.98210    
    ## sex11          -0.1728926  0.2977881  -0.581  0.56152    
    ## crf1TRUE       -0.6762799  0.4816781  -1.404  0.16032    
    ## asthma11       -0.2876535  0.3560888  -0.808  0.41920    
    ## impcogstat1Yes -0.3890762  0.6396231  -0.608  0.54300    
    ## hxcld11         0.3343316  0.4139580   0.808  0.41929    
    ## seizure11       0.0472503  0.3313916   0.143  0.88662    
    ## nutr_support11 -0.2844485  0.3246403  -0.876  0.38092    
    ## hemodisorder11 -1.2965448  0.4180003  -3.102  0.00192 ** 
    ## level_13TRUE   -0.1417618  0.3786539  -0.374  0.70812    
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## (Dispersion parameter for binomial family taken to be 1)
    ## 
    ##     Null deviance: 392.43  on 816  degrees of freedom
    ## Residual deviance: 368.91  on 805  degrees of freedom
    ##   (5 observations deleted due to missingness)
    ## AIC: 392.91
    ## 
    ## Number of Fisher Scoring iterations: 6

Age and pre-existing hematological disorder were significant at a=0.05.
We further examined ways to improve our model fit. Given the small
sample size some variables known to the literature as predictors for
complications in patients (not patients with CP) who undergo spinal
fusion may have not been able to be identified. These are `asthma1`,
`hxcrl`, `nutr_support1`, `level_13`. We run a reduced model with these
variables as well as `age_years` and `hemodisorder1` .

Reduced model has 6
indicators

``` r
mylogit <- glm(home_discharge1 ~ age_years+ asthma1+ hxcld1 + `nutr_support1`+ hemodisorder1+ level_13, data = model_data, family = "binomial")
summary(mylogit)
```

    ## 
    ## Call:
    ## glm(formula = home_discharge1 ~ age_years + asthma1 + hxcld1 + 
    ##     nutr_support1 + hemodisorder1 + level_13, family = "binomial", 
    ##     data = model_data)
    ## 
    ## Deviance Residuals: 
    ##     Min       1Q   Median       3Q      Max  
    ## -3.0065   0.2601   0.3242   0.3993   1.0844  
    ## 
    ## Coefficients:
    ##                Estimate Std. Error z value Pr(>|z|)    
    ## (Intercept)     5.77430    0.94973   6.080  1.2e-09 ***
    ## age_years      -0.19568    0.06135  -3.190  0.00142 ** 
    ## asthma11       -0.32563    0.35073  -0.928  0.35319    
    ## hxcld11         0.32260    0.41281   0.781  0.43451    
    ## nutr_support11 -0.31423    0.30916  -1.016  0.30945    
    ## hemodisorder11 -1.29254    0.41650  -3.103  0.00191 ** 
    ## level_13TRUE   -0.12510    0.37431  -0.334  0.73821    
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## (Dispersion parameter for binomial family taken to be 1)
    ## 
    ##     Null deviance: 393.10  on 821  degrees of freedom
    ## Residual deviance: 372.22  on 815  degrees of freedom
    ## AIC: 386.22
    ## 
    ## Number of Fisher Scoring iterations: 6

When comparing the Full Model to the Reduced Model, we observe the
distribution of the deviance residuals for individual cases used in the
model. Compared to the full model the reduced model appears to fits
better. AIC is also better (lower). Age and the pre existence of
hematologic disorder remain significant at a=.05. Given that asthma is
an important indicator for prediction of post surgical outcomes in the
general population who udergo spinal fusion surgery, we run a reduced
model with 3 indicators: age\_years, asthma1, hemodisorder1

Another Reduced model has 3
indicators

``` r
mylogit <- glm(home_discharge1 ~ age_years+ hemodisorder1+ asthma1, data = model_data, family = "binomial")
summary(mylogit)
```

    ## 
    ## Call:
    ## glm(formula = home_discharge1 ~ age_years + hemodisorder1 + asthma1, 
    ##     family = "binomial", data = model_data)
    ## 
    ## Deviance Residuals: 
    ##     Min       1Q   Median       3Q      Max  
    ## -2.8150   0.2633   0.3271   0.4002   0.9657  
    ## 
    ## Coefficients:
    ##                Estimate Std. Error z value Pr(>|z|)    
    ## (Intercept)      5.4861     0.8833   6.211 5.26e-10 ***
    ## age_years       -0.1901     0.0598  -3.179  0.00148 ** 
    ## hemodisorder11  -1.2461     0.4059  -3.070  0.00214 ** 
    ## asthma11        -0.3250     0.3351  -0.970  0.33215    
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## (Dispersion parameter for binomial family taken to be 1)
    ## 
    ##     Null deviance: 393.10  on 821  degrees of freedom
    ## Residual deviance: 373.95  on 818  degrees of freedom
    ## AIC: 381.95
    ## 
    ## Number of Fisher Scoring iterations: 6

We use the null and deviance residuals

``` r
with(mylogit, null.deviance - deviance)
```

    ## [1] 19.15319

with Df

``` r
with(mylogit, df.null - df.residual)
```

    ## [1] 3

We compute the Chi-Square
test

``` r
with(mylogit, pchisq(null.deviance - deviance, df.null - df.residual, lower.tail = FALSE))
```

    ## [1] 0.0002541657

The chi-square provides a p-value=0.0028 (p\< 0.05), thus, our reduced
model as a whole fits significantly better than the null model.

We can use the likelihood ratio test (-2\*log likelihood) to see the
model’s log likelihood:

``` r
logLik(mylogit)
```

    ## 'log Lik.' -186.9736 (df=4)

\#\#Preliminary intrepretation for the betas Two predictors in the model
came up significant at a=.05. 1. For every 1 year increase in age the
log odds of homedischarge (versus discharge to other healthcare
facilities) decreases by 0.19. 2. Having a pre-existing hematologic
disorder decreases the log odds of being discharged to home by 1.24. 3.
Asthma does not appear to have a direct impact on the outcome.

Confidence intervals for the log odds coeficients (log-likelihood
function)

``` r
confint(mylogit)
```

    ##                     2.5 %     97.5 %
    ## (Intercept)     3.8360067  7.3056789
    ## age_years      -0.3111816 -0.0762553
    ## hemodisorder11 -2.0058350 -0.3974614
    ## asthma11       -0.9612710  0.3617379

Confidence intervals using the standard errors

``` r
confint.default(mylogit)
```

    ##                     2.5 %      97.5 %
    ## (Intercept)     3.7549209  7.21730652
    ## age_years      -0.3073067 -0.07287942
    ## hemodisorder11 -2.0416601 -0.45049989
    ## asthma11       -0.9817798  0.33181630

Exponantiate to odds ratios and Confidence intervals

``` r
exp(cbind(OR = coef(mylogit), confint(mylogit)))
```

    ##                         OR      2.5 %       97.5 %
    ## (Intercept)    241.3175558 46.3400568 1488.7303116
    ## age_years        0.8268822  0.7325808    0.9265796
    ## hemodisorder11   0.2876301  0.1345479    0.6720239
    ## asthma11         0.7225406  0.3824065    1.4358226

\#\#Data interpretation For every year increase in age the odds of being
released to a health care facility (not home) increase by (1/.82=1.22)
by 22% (all other variables remain constant). The odds of being
discharged at home if you have a hemodisorder are 0.28.This means that
if the patient with CP has a pre-existing hematologic disorder the odds
of being discharged to a health care facility are (1/.27=3.7) 3.7 the
odds of being discharged to home (all other variables remain constant).
Important pre-operative indicators that have been identified for other
patients who undergo spinal fusion surgery such as asthma do not appear
to have a direct impact on the outcome for individuals with CP. When we
exponantiate the beta for Asthma, we observe that the odds for patients
with CP who have asthma to develop complications and to be released to a
health care facility (not home) increase by (1/.82=1.44) by 44%.
However, this association seems influential because it is probably
correlated with another variable that we don’t have in the data. \#\#
Discussion Further studies need to expand on the results of our work for
this population. Future studies can incorporate a larger number of
patients with CP from future yearly releases of the NSQIP data.Studies
should aim to create a prediction model for non-home discharges
validated for this population within and outside of the NSQIP database.
They should further utilize the prediction model to create a risk score
that aids clinical decision making.
