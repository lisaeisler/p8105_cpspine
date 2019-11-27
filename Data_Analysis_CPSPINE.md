Data\_Analysis\_CPSPINE
================

``` r
knitr::opts_chunk$set(echo = TRUE)
library(tidyverse)
```

    ## Warning in as.POSIXlt.POSIXct(Sys.time()): unknown timezone 'zone/tz/2019c.
    ## 1.0/zoneinfo/America/New_York'

    ## ── Attaching packages ──────────────────────────────────────────────────────── tidyverse 1.2.1 ──

    ## ✔ ggplot2 3.2.1     ✔ purrr   0.3.3
    ## ✔ tibble  2.1.3     ✔ dplyr   0.8.3
    ## ✔ tidyr   1.0.0     ✔ stringr 1.4.0
    ## ✔ readr   1.1.1     ✔ forcats 0.4.0

    ## ── Conflicts ─────────────────────────────────────────────────────────── tidyverse_conflicts() ──
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

  - From the series of variables weight, height, and BMI (highly
    related) we selected based on literature review weight to be
    included in the model.
  - From our initial data visualization, ethnicity has large number of
    patients who self-identified as “Other”, it is unclear how to use
    other to create preditions thus the ethnicity variable was
    eliminated.
  - We examined the var “Transt” which indicates where the patient was
    before coming to surgery. The variable was turned in to binary
    “transt1”, as the sample size does not allow for multiple
    categorical variables. NEED TO TRANSFORM TO BINARY???
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
home_discharge1=as.factor(home_discharge1))%>% 
  select(age_years, weight, sex1, race1, asa_level, ventilat1, asthma1, hxcld1, oxygen_sup1, seizure1, nutr_support1, hemodisorder1, level_13, home_discharge1) 
```

1.  Overall quick descriptives for the data entered in the
    model

<!-- end list -->

``` r
summary(model_data)
```

    ##    age_years         weight       sex1     race1     asa_level ventilat1
    ##  Min.   : 3.34   Min.   :  2.00   0:398   2   : 24   0: 77     0:739    
    ##  1st Qu.:11.28   1st Qu.: 26.80   1:424   3   :203   1:745     1: 83    
    ##  Median :13.28   Median : 32.00           4   :  4                      
    ##  Mean   :13.13   Mean   : 33.58           5   :501                      
    ##  3rd Qu.:15.18   3rd Qu.: 38.90           NA's: 90                      
    ##  Max.   :17.99   Max.   :107.00                                         
    ##                  NA's   :5                                              
    ##  asthma1 hxcld1  oxygen_sup1 seizure1 nutr_support1 hemodisorder1
    ##  0:631   0:670   0:761       0:302    0:406         0:770        
    ##  1:191   1:152   1: 61       1:520    1:416         1: 52        
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
xtabs(~ home_discharge1+ level_13, data = model_data)
```

    ##                level_13
    ## home_discharge1 FALSE TRUE
    ##               0    10   43
    ##               1   186  583

Logistic regression (Full Model with
)

``` r
mylogit <- glm(home_discharge1 ~ age_years+ weight+ sex1+ asa_level + ventilat1 + asthma1 + hxcld1 + oxygen_sup1 + seizure1 + nutr_support1 + hemodisorder1+ level_13, data = model_data, family = "binomial")
summary(mylogit)
```

    ## 
    ## Call:
    ## glm(formula = home_discharge1 ~ age_years + weight + sex1 + asa_level + 
    ##     ventilat1 + asthma1 + hxcld1 + oxygen_sup1 + seizure1 + nutr_support1 + 
    ##     hemodisorder1 + level_13, family = "binomial", data = model_data)
    ## 
    ## Deviance Residuals: 
    ##     Min       1Q   Median       3Q      Max  
    ## -2.9596   0.2399   0.3179   0.4057   0.9273  
    ## 
    ## Coefficients:
    ##                 Estimate Std. Error z value Pr(>|z|)    
    ## (Intercept)     6.979271   1.290679   5.407 6.39e-08 ***
    ## age_years      -0.201586   0.066163  -3.047  0.00231 ** 
    ## weight         -0.002831   0.014993  -0.189  0.85024    
    ## sex11          -0.192483   0.297001  -0.648  0.51693    
    ## asa_level1     -1.124188   0.759970  -1.479  0.13907    
    ## ventilat11      0.010239   0.546168   0.019  0.98504    
    ## asthma11       -0.381148   0.360418  -1.058  0.29028    
    ## hxcld11         0.307587   0.421640   0.730  0.46569    
    ## oxygen_sup11    0.629524   0.690686   0.911  0.36206    
    ## seizure11       0.094874   0.321577   0.295  0.76797    
    ## nutr_support11 -0.280662   0.322784  -0.870  0.38457    
    ## hemodisorder11 -1.371087   0.423334  -3.239  0.00120 ** 
    ## level_13TRUE   -0.088250   0.379154  -0.233  0.81595    
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## (Dispersion parameter for binomial family taken to be 1)
    ## 
    ##     Null deviance: 392.43  on 816  degrees of freedom
    ## Residual deviance: 367.36  on 804  degrees of freedom
    ##   (5 observations deleted due to missingness)
    ## AIC: 393.36
    ## 
    ## Number of Fisher Scoring iterations: 6

## Reduced model based on the cross tabs above (cells with counts \<=5)

``` r
mylogit <- glm(home_discharge1 ~ age_years+ weight+ sex1+ asthma1 + hxcld1 + seizure1 + nutr_support1 + hemodisorder1+ level_13, data = model_data, family = "binomial")
summary(mylogit)
```

    ## 
    ## Call:
    ## glm(formula = home_discharge1 ~ age_years + weight + sex1 + asthma1 + 
    ##     hxcld1 + seizure1 + nutr_support1 + hemodisorder1 + level_13, 
    ##     family = "binomial", data = model_data)
    ## 
    ## Deviance Residuals: 
    ##     Min       1Q   Median       3Q      Max  
    ## -2.9906   0.2590   0.3271   0.3999   1.0449  
    ## 
    ## Coefficients:
    ##                 Estimate Std. Error z value Pr(>|z|)    
    ## (Intercept)     5.858628   1.046478   5.598 2.16e-08 ***
    ## age_years      -0.202908   0.066089  -3.070  0.00214 ** 
    ## weight          0.001892   0.015124   0.125  0.90043    
    ## sex11          -0.182685   0.295263  -0.619  0.53610    
    ## asthma11       -0.315003   0.354610  -0.888  0.37437    
    ## hxcld11         0.325254   0.412888   0.788  0.43084    
    ## seizure11       0.065148   0.322877   0.202  0.84009    
    ## nutr_support11 -0.324272   0.321275  -1.009  0.31282    
    ## hemodisorder11 -1.294916   0.417304  -3.103  0.00192 ** 
    ## level_13TRUE   -0.124372   0.378657  -0.328  0.74257    
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## (Dispersion parameter for binomial family taken to be 1)
    ## 
    ##     Null deviance: 392.43  on 816  degrees of freedom
    ## Residual deviance: 371.17  on 807  degrees of freedom
    ##   (5 observations deleted due to missingness)
    ## AIC: 391.17
    ## 
    ## Number of Fisher Scoring iterations: 6

## Reduced based on literature and visualization

``` r
mylogit <- glm(home_discharge1 ~ age_years+ weight+ hemodisorder1+ level_13, data = model_data, family = "binomial")
summary(mylogit)
```

    ## 
    ## Call:
    ## glm(formula = home_discharge1 ~ age_years + weight + hemodisorder1 + 
    ##     level_13, family = "binomial", data = model_data)
    ## 
    ## Deviance Residuals: 
    ##     Min       1Q   Median       3Q      Max  
    ## -2.8073   0.2676   0.3271   0.3970   0.8858  
    ## 
    ## Coefficients:
    ##                 Estimate Std. Error z value Pr(>|z|)    
    ## (Intercept)     5.329727   0.907011   5.876  4.2e-09 ***
    ## age_years      -0.186300   0.064062  -2.908  0.00364 ** 
    ## weight          0.004736   0.015080   0.314  0.75347    
    ## hemodisorder11 -1.288066   0.405631  -3.175  0.00150 ** 
    ## level_13TRUE   -0.177042   0.370621  -0.478  0.63287    
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## (Dispersion parameter for binomial family taken to be 1)
    ## 
    ##     Null deviance: 392.43  on 816  degrees of freedom
    ## Residual deviance: 373.82  on 812  degrees of freedom
    ##   (5 observations deleted due to missingness)
    ## AIC: 383.82
    ## 
    ## Number of Fisher Scoring iterations: 6
