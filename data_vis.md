data\_vis
================
Jerri Chen
11/12/19

## Data import

``` r
nsqipspineCP_1617 = read_csv("./data/nsqipspineCP_1617.csv")
```

note: Where variable type and length disagree, we get error message.
Coercion occurs and outcome is all numeric/all character for any given
column with longest length.

## Data simplification and coding

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
      dischdest == "NULL" ~ "NA"
    )) %>%
  select(pufyear_x:ped_spn_post_neurodeftype, age_years, sex, height, weight, bmi, ethnicity_hispanic, race, asa_status, transt, ventilat, asthma, hxcld, oxygen_sup, crf, impcogstat, seizure, nutr_support, hemodisorder, optime, tothlos, d_opto_dis, death30yn, supinfec, wndinfd, orgspcssi, dehis, oupneumo, pulembol, renainsf, urninfec, cszre, neurodef, cdarrest, othbleed, bleed_ml_tot, othcdiff, othsysep, unplannedreadmission1, reoperation, dischdest, home_discharge)
```

## Patient Demographics

### For Visualization

``` r
cp_spine_tidy %>%
  group_by(reoperation) %>%
  summarize(n = n())
```

    ## # A tibble: 2 x 2
    ##   reoperation     n
    ##   <chr>       <int>
    ## 1 No            774
    ## 2 Yes            56

``` r
cp_spine_tidy %>%
  group_by(reoperation) %>% 
  count(sex)
```

    ## # A tibble: 4 x 3
    ## # Groups:   reoperation [2]
    ##   reoperation sex        n
    ##   <chr>       <chr>  <int>
    ## 1 No          Female   401
    ## 2 No          Male     373
    ## 3 Yes         Female    27
    ## 4 Yes         Male      29

``` r
cp_spine_tidy %>%
  group_by(sex, reoperation) %>% 
  summarize(mean_age = mean(age_years), sd = sd(age_years))
```

    ## # A tibble: 4 x 4
    ## # Groups:   sex [2]
    ##   sex    reoperation mean_age    sd
    ##   <chr>  <chr>          <dbl> <dbl>
    ## 1 Female No              12.8  2.71
    ## 2 Female Yes             13.5  2.37
    ## 3 Male   No              13.5  2.84
    ## 4 Male   Yes             13.7  2.63

``` r
cp_spine_tidy %>%
  group_by(sex, reoperation) %>% 
  summarize(mean_weight = mean(weight, na.rm = TRUE), sd = sd(weight, na.rm = TRUE))
```

    ## # A tibble: 4 x 4
    ## # Groups:   sex [2]
    ##   sex    reoperation mean_weight    sd
    ##   <chr>  <chr>             <dbl> <dbl>
    ## 1 Female No                 32.6 11.7 
    ## 2 Female Yes                32.2  8.35
    ## 3 Male   No                 34.7 10.3 
    ## 4 Male   Yes                33.2  7.88

``` r
cp_spine_tidy %>%
  group_by(reoperation) %>% 
  count(race) %>% 
  replace(is.na(.), 0) %>% 
  janitor::clean_names()
```

    ## # A tibble: 10 x 3
    ## # Groups:   reoperation [2]
    ##    reoperation race                                          n
    ##    <chr>       <chr>                                     <int>
    ##  1 No          American Indian or Alaska Native              3
    ##  2 No          Asian                                        23
    ##  3 No          Black or African American                   193
    ##  4 No          Native Hawaiian or Other Pacific Islander     4
    ##  5 No          Unknown/Not Reported                         81
    ##  6 No          White                                       470
    ##  7 Yes         Asian                                         1
    ##  8 Yes         Black or African American                    12
    ##  9 Yes         Unknown/Not Reported                          8
    ## 10 Yes         White                                        35

``` r
cp_spine_tidy %>%
  group_by(reoperation) %>% 
  count(ethnicity_hispanic) %>% 
  janitor::clean_names() %>% 
  rename(ethnicity = ethnicity_hispanic) %>% 
  mutate(ethnicity = case_when(
    ethnicity == "No" ~ "Non-Hispanic",
    ethnicity == "Yes" ~ "Hispanic",
    ethnicity == "NULL" ~ "Other/Did Not Answer"
  ))
```

    ## # A tibble: 6 x 3
    ## # Groups:   reoperation [2]
    ##   reoperation ethnicity                n
    ##   <chr>       <chr>                <int>
    ## 1 No          Non-Hispanic           619
    ## 2 No          Other/Did Not Answer    38
    ## 3 No          Hispanic               117
    ## 4 Yes         Non-Hispanic            45
    ## 5 Yes         Other/Did Not Answer     4
    ## 6 Yes         Hispanic                 7

``` r
cp_spine_tidy %>%
  group_by(reoperation) %>% 
  count(ped_spn_class) %>% 
  replace(is.na(.), 0) %>% 
  janitor::clean_names()
```

    ## # A tibble: 9 x 3
    ## # Groups:   reoperation [2]
    ##   reoperation ped_spn_class                                     n
    ##   <chr>       <chr>                                         <int>
    ## 1 No          Congenital/Structural                            22
    ## 2 No          Idiopathic                                       26
    ## 3 No          Insufficient clinical information to classify    10
    ## 4 No          Kyphosis                                          9
    ## 5 No          Neuromuscular                                   696
    ## 6 No          Syndromic                                        11
    ## 7 Yes         Congenital/Structural                             2
    ## 8 Yes         Idiopathic                                        1
    ## 9 Yes         Neuromuscular                                    53

### For Tables (Pivot Wider)

``` r
data_reop = cp_spine_tidy %>%
  group_by(reoperation) %>%
  summarize(n = n()) %>% 
  pivot_wider(
    names_from = "reoperation",
    values_from = "n"
  ) %>% 
  janitor::clean_names()%>% 
  mutate(percent = (yes/(no + yes))*100)
```

``` r
data_sex = cp_spine_tidy %>%
  group_by(reoperation) %>% 
  count(sex) %>% 
  pivot_wider(
    names_from = "reoperation",
    values_from = "n"
  ) %>% 
  janitor::clean_names() %>% 
  mutate(percent = (yes/(no + yes))*100)
```

``` r
data_age = cp_spine_tidy %>%
  group_by(sex, reoperation) %>% 
  summarize(mean_age = mean(age_years), sd = sd(age_years))
```

``` r
data_weight = cp_spine_tidy %>%
  group_by(sex, reoperation) %>% 
  summarize(mean_weight = mean(weight, na.rm = TRUE), sd = sd(weight, na.rm = TRUE))
```

``` r
data_race = cp_spine_tidy %>%
  group_by(reoperation) %>% 
  count(race) %>% 
  pivot_wider(
    names_from = "reoperation",
    values_from = "n"
  ) %>% 
  replace(is.na(.), 0) %>% 
  janitor::clean_names()
```

``` r
data_ethnicity = cp_spine_tidy %>%
  group_by(reoperation) %>% 
  count(ethnicity_hispanic) %>% 
  pivot_wider(
    names_from = "reoperation",
    values_from = "n"
  ) %>% 
  janitor::clean_names() %>% 
  rename(ethnicity = ethnicity_hispanic) %>% 
  mutate(ethnicity = case_when(
    ethnicity == "No" ~ "Non-Hispanic",
    ethnicity == "Yes" ~ "Hispanic",
    ethnicity == "NULL" ~ "Other/Did Not Answer"
  ))
```

``` r
data_diagnosis = cp_spine_tidy %>%
  group_by(reoperation) %>% 
  count(ped_spn_class) %>% 
  pivot_wider(
    names_from = "reoperation",
    values_from = "n"
  ) %>% 
  replace(is.na(.), 0) %>% 
  janitor::clean_names()
```

## Code for Home Discharge

``` r
cp_spine_tidy %>% 
  select(home_discharge)
```

    ## # A tibble: 830 x 1
    ##    home_discharge
    ##    <chr>         
    ##  1 TRUE          
    ##  2 TRUE          
    ##  3 FALSE         
    ##  4 TRUE          
    ##  5 TRUE          
    ##  6 TRUE          
    ##  7 TRUE          
    ##  8 TRUE          
    ##  9 TRUE          
    ## 10 TRUE          
    ## # … with 820 more rows

``` r
data_discharge = cp_spine_tidy %>%
  group_by(home_discharge) %>%
  summarize(n = n())
```
