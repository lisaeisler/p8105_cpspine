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

note: Where variable type and length disagree, we get error message.
Coercion occurs and outcome is all numeric/all character for any given
column with longest
length.

## Initial Data Cleaning & Selection of a subset of data to examine the purpose of the study.

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

## Analysis

Our goal is to develop a model,in which we can use important
pre-operative factors to predict the patient’s discharge at home. The
initial set of pre-operative variables is:
pufyear\_x:ped\_spn\_post\_neurodeftype, age\_years= Age in Years
(continues var.) sex=Gender (binary var.) height= Height (inch)
(continues var.) weight= Weight(lbs) (continues var.) bmi= BMI
(continues var.) ethnicity\_hispanic=Ethnicity (Categorical var.) race=
Race (Categorical var.) asa\_status= ASA Classification (Categorical
var.) transt= Transfer Status (Admitted from Home or ER, or Rehab Care
facility, etc.) (Categorical var.) ventilat= Ventilator Dependence
(binary var.) asthma=Asthma (binary var.) hxcld= Bronchopulmonary
Dysplasia/Chronic Lung Disease (binary var.) oxygen\_sup= Oxygen Support
(binary var.) crf= Cardiac Risk Factors (Categorical var.) impcogstat=
Developmental Delay/Impaired Cognitive Status (binary var.) seizure=
Seizure Disorder (binary var.) nutr\_support=Nutritional Support (binary
var.) hemodisorder=Hematologic Disorder (binary var.) level\_13= Fusion
of 13 or more vertebral segments (binary var.)

## Selection of predictors to include in the model

  - From the series of variables weight, height, and BMI (all three
    highly related) we selected based on literature review bmi to be
    included in the model.
  - From our initial data visualization, ethnicity has large number of
    patients who self-identified as “Other”, it is unclear how to use
    other to create preditions thus the ethnicity variable was
    eliminated.
  - We examined the var “Transt” which indicates where the patient was
    before coming to surgery. The variable was turned in to binary
    “transt1”, as the sample size does not allow for multiple
    categorical variables. This below does not run. transt1 =
    case\_when( transt == “Chronic care;Rehab;Intermediate Care;Spinal
    Cord” ~ “FALSE”, transt == “Transferred from outside hospital (NICU,
    PICU, Inpatient on General floor, Adult” ~“FALSE”, transt == “other”
    ~ “FALSE”, transt == “Admitted from home;clinic;doctor’s office” ~
    “TRUE”, transt == “Admitted through ER, including outside ER with
    direct hospital admission” ~ “TRUE”), transt1=as.factor(transt1),
  - From the literature we know that cognitive ability can be a confound
    to the severity of Neuromusculoskeletal disorder and does not relate
    to medical complications. Thus, we eliminated from the model
    “impcogstat”.
  - Also recoded cardiac risk factor (crf) as a binary variable, again
    because of the small sample size.NEED TO TRANSFORM TO BINARY???
  - We will examine a total of 13 (will be 15 after we fix the two
    above) variables for the full model. Before entering the full model
    we will use cross tabs to see if there are any cells with 0 (n=0).

<!-- end list -->

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
  crf1=as.factor(crf1))%>%
  select(age_years, bmi, weight, sex1, crf1, race1, asa_level, ventilat1, asthma1, hxcld1, oxygen_sup1, seizure1, nutr_support1, hemodisorder1, level_13, home_discharge1) 
```

1.  Overall quick descriptives for the data entered in the
    model

<!-- end list -->

``` r
summary(model_data)
```

    ##    age_years          bmi              weight       sex1       crf1    
    ##  Min.   : 3.34   Min.   :  1.577   Min.   :  2.00   0:398   FALSE: 53  
    ##  1st Qu.:11.28   1st Qu.: 15.321   1st Qu.: 26.80   1:424   TRUE :769  
    ##  Median :13.28   Median : 17.540   Median : 32.00                      
    ##  Mean   :13.13   Mean   : 18.907   Mean   : 33.58                      
    ##  3rd Qu.:15.18   3rd Qu.: 20.114   3rd Qu.: 38.90                      
    ##  Max.   :17.99   Max.   :238.140   Max.   :107.00                      
    ##                  NA's   :125       NA's   :5                           
    ##   race1     asa_level ventilat1 asthma1 hxcld1  oxygen_sup1 seizure1
    ##  2   : 24   0: 77     0:739     0:631   0:670   0:761       0:302   
    ##  3   :203   1:745     1: 83     1:191   1:152   1: 61       1:520   
    ##  4   :  4                                                           
    ##  5   :501                                                           
    ##  NA's: 90                                                           
    ##                                                                     
    ##                                                                     
    ##  nutr_support1 hemodisorder1  level_13   home_discharge1
    ##  0:406         0:770         FALSE:196   0: 53          
    ##  1:416         1: 52         TRUE :626   1:769          
    ##                                                         
    ##                                                         
    ##                                                         
    ##                                                         
    ## 

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

  - Cell with only 2 data points we may need to take out from the model
    (unstable model.

<!-- end list -->

``` r
xtabs(~ home_discharge1+ ventilat1, data = model_data)
```

    ##                ventilat1
    ## home_discharge1   0   1
    ##               0  48   5
    ##               1 691  78

  - cell with only 5 data points may consider to take out (unstable
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

Logistic regression (Full Model with
)

``` r
mylogit <- glm(home_discharge1 ~ age_years+ bmi+ sex1+ asa_level + crf1+ventilat1 + asthma1 + hxcld1 + oxygen_sup1 + seizure1 + nutr_support1 + hemodisorder1+ level_13, data = model_data, family = "binomial")
summary(mylogit)
```

    ## 
    ## Call:
    ## glm(formula = home_discharge1 ~ age_years + bmi + sex1 + asa_level + 
    ##     crf1 + ventilat1 + asthma1 + hxcld1 + oxygen_sup1 + seizure1 + 
    ##     nutr_support1 + hemodisorder1 + level_13, family = "binomial", 
    ##     data = model_data)
    ## 
    ## Deviance Residuals: 
    ##     Min       1Q   Median       3Q      Max  
    ## -2.9830   0.2156   0.3038   0.3830   1.0877  
    ## 
    ## Coefficients:
    ##                 Estimate Std. Error z value Pr(>|z|)    
    ## (Intercept)     6.930979   1.584134   4.375 1.21e-05 ***
    ## age_years      -0.202325   0.071623  -2.825  0.00473 ** 
    ## bmi            -0.006945   0.008353  -0.831  0.40574    
    ## sex11           0.102504   0.335219   0.306  0.75977    
    ## asa_level1     -1.633812   1.036850  -1.576  0.11508    
    ## crf1TRUE        0.827978   0.534693   1.549  0.12150    
    ## ventilat11     -0.122534   0.571107  -0.215  0.83011    
    ## asthma11       -0.332963   0.395866  -0.841  0.40029    
    ## hxcld11         0.227989   0.459929   0.496  0.62010    
    ## oxygen_sup11    0.334522   0.714361   0.468  0.63958    
    ## seizure11      -0.246564   0.376836  -0.654  0.51292    
    ## nutr_support11 -0.181642   0.364448  -0.498  0.61820    
    ## hemodisorder11 -1.452341   0.470054  -3.090  0.00200 ** 
    ## level_13TRUE   -0.135322   0.431252  -0.314  0.75368    
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## (Dispersion parameter for binomial family taken to be 1)
    ## 
    ##     Null deviance: 317.38  on 696  degrees of freedom
    ## Residual deviance: 291.13  on 683  degrees of freedom
    ##   (125 observations deleted due to missingness)
    ## AIC: 319.13
    ## 
    ## Number of Fisher Scoring iterations: 7

## Reduced model based on the cross tabs above (cells with counts \<=5)

``` r
mylogit <- glm(home_discharge1 ~ age_years+ bmi+ sex1+ asthma1 +crf1+ hxcld1 + seizure1 + nutr_support1 + hemodisorder1+ level_13, data = model_data, family = "binomial")
summary(mylogit)
```

    ## 
    ## Call:
    ## glm(formula = home_discharge1 ~ age_years + bmi + sex1 + asthma1 + 
    ##     crf1 + hxcld1 + seizure1 + nutr_support1 + hemodisorder1 + 
    ##     level_13, family = "binomial", data = model_data)
    ## 
    ## Deviance Residuals: 
    ##     Min       1Q   Median       3Q      Max  
    ## -3.0267   0.2377   0.3054   0.3776   1.1328  
    ## 
    ## Coefficients:
    ##                 Estimate Std. Error z value Pr(>|z|)    
    ## (Intercept)     5.445680   1.232242   4.419  9.9e-06 ***
    ## age_years      -0.199569   0.071495  -2.791  0.00525 ** 
    ## bmi            -0.007553   0.008407  -0.898  0.36897    
    ## sex11           0.109507   0.333130   0.329  0.74237    
    ## asthma11       -0.320699   0.389312  -0.824  0.41008    
    ## crf1TRUE        0.874653   0.526767   1.660  0.09683 .  
    ## hxcld11         0.187830   0.450690   0.417  0.67685    
    ## seizure11      -0.286869   0.378315  -0.758  0.44828    
    ## nutr_support11 -0.287408   0.361923  -0.794  0.42713    
    ## hemodisorder11 -1.399894   0.456154  -3.069  0.00215 ** 
    ## level_13TRUE   -0.181044   0.428993  -0.422  0.67301    
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## (Dispersion parameter for binomial family taken to be 1)
    ## 
    ##     Null deviance: 317.38  on 696  degrees of freedom
    ## Residual deviance: 295.39  on 686  degrees of freedom
    ##   (125 observations deleted due to missingness)
    ## AIC: 317.39
    ## 
    ## Number of Fisher Scoring iterations: 6

## Reduced based on literature and visualization

``` r
mylogit <- glm(home_discharge1 ~ age_years+ bmi+hemodisorder1+crf1, data = model_data, family = "binomial")
summary(mylogit)
```

    ## 
    ## Call:
    ## glm(formula = home_discharge1 ~ age_years + bmi + hemodisorder1 + 
    ##     crf1, family = "binomial", data = model_data)
    ## 
    ## Deviance Residuals: 
    ##     Min       1Q   Median       3Q      Max  
    ## -2.8167   0.2518   0.3108   0.3746   0.8550  
    ## 
    ## Coefficients:
    ##                 Estimate Std. Error z value Pr(>|z|)    
    ## (Intercept)     4.705918   1.047790   4.491 7.08e-06 ***
    ## age_years      -0.176132   0.065762  -2.678  0.00740 ** 
    ## bmi            -0.006196   0.008439  -0.734  0.46284    
    ## hemodisorder11 -1.386472   0.438696  -3.160  0.00158 ** 
    ## crf1TRUE        0.787231   0.517140   1.522  0.12794    
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## (Dispersion parameter for binomial family taken to be 1)
    ## 
    ##     Null deviance: 317.38  on 696  degrees of freedom
    ## Residual deviance: 298.91  on 692  degrees of freedom
    ##   (125 observations deleted due to missingness)
    ## AIC: 308.91
    ## 
    ## Number of Fisher Scoring iterations: 6

## The latest reduced model fits better

1.  We see the deviance residuals, which are a measure of model fit.
    This part of output shows the distribution of the deviance residuals
    for individual cases used in the model. Compared to the other two
    models this fits better. AIC is also better (lower). Age and the
    pre-existance of a hemodisorder are significant. For every 1 year
    increase in age the log odds of homedischarge (versus discharge to
    other healthcare facilities) decreases by 0.176. Having a
    pre-existing hematologic disorder changes the log odds of
    homedischarge by -1.386. \#\# Confidence intervals for the log odds
    coeficients (log-likelihood function)

<!-- end list -->

``` r
confint(mylogit)
```

    ##                      2.5 %      97.5 %
    ## (Intercept)     2.75381487  6.87287993
    ## age_years      -0.30978435 -0.05135965
    ## bmi            -0.02097187  0.01706992
    ## hemodisorder11 -2.20942569 -0.46891732
    ## crf1TRUE       -0.34215027  1.72504503

## Confidence intervals using the standard errors

``` r
confint.default(mylogit)
```

    ##                      2.5 %      97.5 %
    ## (Intercept)     2.65228702  6.75954903
    ## age_years      -0.30502372 -0.04724066
    ## bmi            -0.02273708  0.01034501
    ## hemodisorder11 -2.24629965 -0.52664468
    ## crf1TRUE       -0.22634407  1.80080707

### Exponantiate to odds ratios and 95% CI

``` r
exp(cbind(OR = coef(mylogit), confint(mylogit)))
```

    ##                         OR      2.5 %      97.5 %
    ## (Intercept)    110.5997718 15.7024204 965.7257913
    ## age_years        0.8385071  0.7336051   0.9499370
    ## bmi              0.9938231  0.9792465   1.0172164
    ## hemodisorder11   0.2499556  0.1097637   0.6256793
    ## crf1TRUE         2.1973048  0.7102415   5.6127738

\#\#For one year increase in age, the odds of being discharged home
(versus being discharged to another health care facility) increase by
0.83. The odds of being discharged at home if you have a hemodisorder
are 0.24.
