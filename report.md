Predicting Non-Routine Discharge in Cerebral Palsy Patients Undergoing
Spinal Fusion
================

![Wheelchair](./images/wheelchair.jpg)

# Table of Contents

  - <a href="#intro">Introduction</a>
  - <a href="#characterize">Data Characterization</a>
  - <a href="#tidying">Tidying The Data</a>
  - <a href="#lit">Literature Review</a>
  - <a href="#sub">Subanalyses</a>
  - <a href="#regress">Regression Analysis</a>
  - <a href="#conclusion">Discussion</a>
  - <a href="#changes">Changes Mid-Report</a>
  - <a href="#refs">References</a>

-----

### Authors

  - Katherine Dimitripoulou PhD (UNI: kd2524)
  - Jerri Chen MD, PhD (UNI: jc4166)
  - Lisa Eisler MD (UNI: ldl2113)

<h1 id="intro">

Introduction

</h1>

## Project Motivation

Children with cerebral palsy (CP) often present with a curvature of the
spine, or scoliosis, as a result of abnormal muscle tone and postural
weakness.<sup>[1](https://www.flintrehab.com/2019/cerebral-palsy-scoliosis/)</sup>
The clinical characteristics of this scoliosis in CP patients involve a
deformity of the lumbar and thoracic spine often accompanied by pelvic
torque and postural problems, which leads to loss of function and pain.
Severe spinal curves (\>50 degrees) are difficult to control with
braces, especially if they are rapidly growing. Surgical treatment is
often the recommended intervention to prevent further deterioration of
functional mobility, and improve quality of life.

The surgical treatment of individuals with CP who present with severe
neuromuscular scoliosis has been associated with peri- and postoperative
complication rates as high as 75%. Mohamad et al. (2007) carried out a
retrospective record review of 175 patients with neuromuscular
scoliosis, with 73.7 % of them being patients with
CP.<sup>[2](https://www.ncbi.nlm.nih.gov/pubmed/17513958/)</sup> The
peri- and post-operative complications rate was 33.1 % (58/175).
Patients experienced a combination of pulmonary complications (~20%),
infections (8%), neurological (4%), and cardiovascular problems (4%). In
another retrospective study, Tsirikos et al. (2008) reported data in 287
patients with CP who underwent spinal
fusion.<sup>[3](https://www.ncbi.nlm.nih.gov/pubmed/18449049/)</sup>
They reported major complications including 3 perioperative deaths, an
intraoperative complication rate of 10.8 % (38/287), an early
postoperative (within 6 weeks) complication rate of 9.4 % (27/287), and
a late postoperative complication rate of 10.1 % (29/ 287). Lastly, a
prospective cohort study with 127 patients with CP (Samdani, et.al.
2016) reported an overall 39.4% of major perioperative complications
that resulted in increased hospitalization length, use of intensive care
unit, and readmission. The authors reported perioperative risk variables
such as increase of blood loss, staged procedures and the lack of
antifibrinolytic
use.<sup>[4](https://www.ncbi.nlm.nih.gov/pubmed/26148567/)</sup> They
also reported some pre-operative indicators such as the larger
preoperative kyphosis, lower body mass index (BMI) as risk factors for
complications.

In all studies, there appear to be pre-operative risk factors associated
with increased complications, but no study has comprehensively examined
these factors. In addition, no study has examined non-routine discharge,
with discharge to home (or home facility) assumed to be the routine as
well as preferred outcome from a rehabilitation standpoint. We believe
that patients and their families benefit from reasonable expectations
about the chance of a decline in function, higher-level care needs or
even death in the initial 30-day postoperative period. A model
predicting non-routine discharge (defined as discharge to a higher level
of care including postoperative death).

Our team comprises anesthesiologists and rehabilitation therapists who
can combine expertise to develop behavioral and biological intervention
protocols, and to make decisions regarding the proper timing and
optimization for surgery.

## Project Roadmap

<h1 id="characterize">

Data Characterization

</h1>

``` r
library(tidyverse)
library(haven)
library(plotly)
library(viridis)
library(data.table)
library(formattable)
library(readxl)

# Formatting plot output
knitr::opts_chunk$set(
  out.width = "90%"
  )
# Set the plot design
theme_set(theme_classic() + 
            theme(legend.position = "bottom", 
                  legend.key.size = unit(1.5, "line"),
                  plot.title = element_text(hjust = 0.5)))
# Raw data
nsqipspineCP_1617 = read_csv("./data/nsqipspineCP_1617.csv")
```

In its raw form, the data has 830 rows and 384 columns. Each row
corresponds to a single spinal fusion and 384 demographic, medical,
surgical, and pre/intra/postoperative characteristics and events, which
required a massive amount of chart review by a clinical reviewer.

A copy of the data could not be shared due to licensing agreements.

This is a focused analysis, interested only in a handful of important
postoperative outcomes as they relate to the recovery of CP patients
undergoing spinal fusion. These variables are defined here:

<table class="table table-condensed">

<thead>

<tr>

<th style="text-align:left;">

Data Category

</th>

<th style="text-align:left;">

Variable

</th>

<th style="text-align:left;">

Description

</th>

</tr>

</thead>

<tbody>

<tr>

<td style="text-align:left;">

<span style="color: grey; font-weight: bold"> </span>

</td>

<td style="text-align:left;">

</td>

<td style="text-align:left;">

</td>

</tr>

<tr>

<td style="text-align:left;">

<span style="color: grey; font-weight: bold">Case Data </span>

</td>

<td style="text-align:left;">

</td>

<td style="text-align:left;">

</td>

</tr>

<tr>

<td style="text-align:left;">

<span style="color: grey; font-weight: bold"> </span>

</td>

<td style="text-align:left;">

pufyear\_x

</td>

<td style="text-align:left;">

Year of Participant Use Data File (PUF)

</td>

</tr>

<tr>

<td style="text-align:left;">

<span style="color: grey; font-weight: bold"> </span>

</td>

<td style="text-align:left;">

case\_id

</td>

<td style="text-align:left;">

Case Identification Number

</td>

</tr>

<tr>

<td style="text-align:left;">

<span style="color: grey; font-weight: bold"> </span>

</td>

<td style="text-align:left;">

</td>

<td style="text-align:left;">

</td>

</tr>

<tr>

<td style="text-align:left;">

<span style="color: grey; font-weight: bold">Demographic Data </span>

</td>

<td style="text-align:left;">

</td>

<td style="text-align:left;">

</td>

</tr>

<tr>

<td style="text-align:left;">

<span style="color: grey; font-weight: bold"> </span>

</td>

<td style="text-align:left;">

sex

</td>

<td style="text-align:left;">

Gender (Male, Female)

</td>

</tr>

<tr>

<td style="text-align:left;">

<span style="color: grey; font-weight: bold"> </span>

</td>

<td style="text-align:left;">

height

</td>

<td style="text-align:left;">

Height (cm)

</td>

</tr>

<tr>

<td style="text-align:left;">

<span style="color: grey; font-weight: bold"> </span>

</td>

<td style="text-align:left;">

weight

</td>

<td style="text-align:left;">

Weight (kg)

</td>

</tr>

<tr>

<td style="text-align:left;">

<span style="color: grey; font-weight: bold"> </span>

</td>

<td style="text-align:left;">

bmi

</td>

<td style="text-align:left;">

Body Mass Index

</td>

</tr>

<tr>

<td style="text-align:left;">

<span style="color: grey; font-weight: bold"> </span>

</td>

<td style="text-align:left;">

ethnicity\_hispanic

</td>

<td style="text-align:left;">

Hispanic Ethnicity (Yes, No, Null)

</td>

</tr>

<tr>

<td style="text-align:left;">

<span style="color: grey; font-weight: bold"> </span>

</td>

<td style="text-align:left;">

race

</td>

<td style="text-align:left;">

Race (American Indian or Alaska Native, Asian, Black or African
American, Native Hawaiian or Other Pacific Islander, Unknown/Not
Reported, White)

</td>

</tr>

<tr>

<td style="text-align:left;">

<span style="color: grey; font-weight: bold"> </span>

</td>

<td style="text-align:left;">

age\_years

</td>

<td style="text-align:left;">

Age (years)

</td>

</tr>

<tr>

<td style="text-align:left;">

<span style="color: grey; font-weight: bold"> </span>

</td>

<td style="text-align:left;">

</td>

<td style="text-align:left;">

</td>

</tr>

<tr>

<td style="text-align:left;">

<span style="color: grey; font-weight: bold">Patient Medical History
</span>

</td>

<td style="text-align:left;">

</td>

<td style="text-align:left;">

</td>

</tr>

<tr>

<td style="text-align:left;">

<span style="color: grey; font-weight: bold"> </span>

</td>

<td style="text-align:left;">

asa\_status

</td>

<td style="text-align:left;">

American Society of Anesthesiology (ASA) Classification (ASA 1 - No
Disturb, ASA 2 - Mild Disturb, ASA 3 - Severe Disturb, ASA 4 - Life
Threat, ASA 5 - Moribund)

</td>

</tr>

<tr>

<td style="text-align:left;">

<span style="color: grey; font-weight: bold"> </span>

</td>

<td style="text-align:left;">

ventilat

</td>

<td style="text-align:left;">

Ventilator Dependence (Yes, No)

</td>

</tr>

<tr>

<td style="text-align:left;">

<span style="color: grey; font-weight: bold"> </span>

</td>

<td style="text-align:left;">

asthma

</td>

<td style="text-align:left;">

History of Asthma (Yes, No)

</td>

</tr>

<tr>

<td style="text-align:left;">

<span style="color: grey; font-weight: bold"> </span>

</td>

<td style="text-align:left;">

hxcld

</td>

<td style="text-align:left;">

Bronchopulmonary Dysplasia/Chronic Lung Disease (Yes, No)

</td>

</tr>

<tr>

<td style="text-align:left;">

<span style="color: grey; font-weight: bold"> </span>

</td>

<td style="text-align:left;">

oxygen\_sup

</td>

<td style="text-align:left;">

Oxygen Support (Yes, No)

</td>

</tr>

<tr>

<td style="text-align:left;">

<span style="color: grey; font-weight: bold"> </span>

</td>

<td style="text-align:left;">

crf

</td>

<td style="text-align:left;">

Cardiac Risk Factors (Major cardiac risk factors, Minor cardiac risk
factors, No cardiac risk factors, Severe cardiac risk factors)

</td>

</tr>

<tr>

<td style="text-align:left;">

<span style="color: grey; font-weight: bold"> </span>

</td>

<td style="text-align:left;">

impcogstat

</td>

<td style="text-align:left;">

Developmental Delay/Impaired Cognitive Status (Yes, No)

</td>

</tr>

<tr>

<td style="text-align:left;">

<span style="color: grey; font-weight: bold"> </span>

</td>

<td style="text-align:left;">

seizure

</td>

<td style="text-align:left;">

History of Seizure Disorder (Yes, No)

</td>

</tr>

<tr>

<td style="text-align:left;">

<span style="color: grey; font-weight: bold"> </span>

</td>

<td style="text-align:left;">

nutr\_support

</td>

<td style="text-align:left;">

Nutritional Support (Yes, No)

</td>

</tr>

<tr>

<td style="text-align:left;">

<span style="color: grey; font-weight: bold"> </span>

</td>

<td style="text-align:left;">

hemodisorder

</td>

<td style="text-align:left;">

Hematologic Disorder (Yes, No)

</td>

</tr>

<tr>

<td style="text-align:left;">

<span style="color: grey; font-weight: bold"> </span>

</td>

<td style="text-align:left;">

transt

</td>

<td style="text-align:left;">

Transfer Status (Admitted from home/clinic/doctor’s office, Admitted
through ER, including outside ER with direct hospital admission, Chronic
care/Rehab/Intermediate Care/Spinal Cord, Transferred from outside
hospital (NICU, PICU, Inpatient on General floor, Adult, Other)

</td>

</tr>

<tr>

<td style="text-align:left;">

<span style="color: grey; font-weight: bold"> </span>

</td>

<td style="text-align:left;">

ped\_spn\_class

</td>

<td style="text-align:left;">

Classification of Spinal Deformity (Congenital/Structural, Idiopathic,
Insufficient clinical information to classify, Kyphosis, Neuromuscular,
Syndromic)

</td>

</tr>

<tr>

<td style="text-align:left;">

<span style="color: grey; font-weight: bold"> </span>

</td>

<td style="text-align:left;">

ped\_spn\_priorop

</td>

<td style="text-align:left;">

Prior Operation for Spinal Deformity (Yes, No)

</td>

</tr>

<tr>

<td style="text-align:left;">

<span style="color: grey; font-weight: bold"> </span>

</td>

<td style="text-align:left;">

ped\_spn\_preopmri

</td>

<td style="text-align:left;">

Preoperative MRI (Yes, No)

</td>

</tr>

<tr>

<td style="text-align:left;">

<span style="color: grey; font-weight: bold"> </span>

</td>

<td style="text-align:left;">

</td>

<td style="text-align:left;">

</td>

</tr>

<tr>

<td style="text-align:left;">

<span style="color: grey; font-weight: bold">Intraoperative Variables
</span>

</td>

<td style="text-align:left;">

</td>

<td style="text-align:left;">

</td>

</tr>

<tr>

<td style="text-align:left;">

<span style="color: grey; font-weight: bold"> </span>

</td>

<td style="text-align:left;">

optime

</td>

<td style="text-align:left;">

Total Operation Time (minutes)

</td>

</tr>

<tr>

<td style="text-align:left;">

<span style="color: grey; font-weight: bold"> </span>

</td>

<td style="text-align:left;">

bleed\_ml\_tot

</td>

<td style="text-align:left;">

Total Blood Transfused (mL)

</td>

</tr>

<tr>

<td style="text-align:left;">

<span style="color: grey; font-weight: bold"> </span>

</td>

<td style="text-align:left;">

ped\_spn\_neuromon

</td>

<td style="text-align:left;">

Intraoperative Neuromonitoring (Yes, No)

</td>

</tr>

<tr>

<td style="text-align:left;">

<span style="color: grey; font-weight: bold"> </span>

</td>

<td style="text-align:left;">

ped\_spn\_antibio\_wnd

</td>

<td style="text-align:left;">

Intraoperative Antibiotics (Yes, No)

</td>

</tr>

<tr>

<td style="text-align:left;">

<span style="color: grey; font-weight: bold"> </span>

</td>

<td style="text-align:left;">

ped\_spn\_antifib

</td>

<td style="text-align:left;">

Intraoperative Antifibrinolytics (Yes, No)

</td>

</tr>

<tr>

<td style="text-align:left;">

<span style="color: grey; font-weight: bold"> </span>

</td>

<td style="text-align:left;">

ped\_spn\_trnsvol\_cell

</td>

<td style="text-align:left;">

Intraoperative Cell-Saver Transfusion (mL)

</td>

</tr>

<tr>

<td style="text-align:left;">

<span style="color: grey; font-weight: bold"> </span>

</td>

<td style="text-align:left;">

ped\_spn\_trnsvol\_allogen

</td>

<td style="text-align:left;">

Intraoperative Allogeneic Transfusion (mL)

</td>

</tr>

<tr>

<td style="text-align:left;">

<span style="color: grey; font-weight: bold"> </span>

</td>

<td style="text-align:left;">

</td>

<td style="text-align:left;">

</td>

</tr>

<tr>

<td style="text-align:left;">

<span style="color: grey; font-weight: bold">Outcomes/Complications
</span>

</td>

<td style="text-align:left;">

</td>

<td style="text-align:left;">

</td>

</tr>

<tr>

<td style="text-align:left;">

<span style="color: grey; font-weight: bold">Hospitalization/Discharge</span>

</td>

<td style="text-align:left;">

tothlos

</td>

<td style="text-align:left;">

Length of Total Hospital Stay (days)

</td>

</tr>

<tr>

<td style="text-align:left;">

<span style="color: grey; font-weight: bold"> </span>

</td>

<td style="text-align:left;">

ped\_spn\_icudays

</td>

<td style="text-align:left;">

ICU Length of Stay (0 - 30 days)

</td>

</tr>

<tr>

<td style="text-align:left;">

<span style="color: grey; font-weight: bold"> </span>

</td>

<td style="text-align:left;">

d\_opto\_dis

</td>

<td style="text-align:left;">

Days From Operation to Discharge

</td>

</tr>

<tr>

<td style="text-align:left;">

<span style="color: grey; font-weight: bold"> </span>

</td>

<td style="text-align:left;">

dischdest

</td>

<td style="text-align:left;">

Discharge Destination (Expired, Facility Which was Home, Home, Rehab,
Separate Acute Care, Skilled Care, Not Home, Unknown, Unskilled Facility
Not Home, NULL)

</td>

</tr>

<tr>

<td style="text-align:left;">

<span style="color: grey; font-weight: bold"> </span>

</td>

<td style="text-align:left;">

home\_discharge

</td>

<td style="text-align:left;">

Discharge Home (True, False)

</td>

</tr>

<tr>

<td style="text-align:left;">

<span style="color: grey; font-weight: bold"> </span>

</td>

<td style="text-align:left;">

unplannedreadmission1

</td>

<td style="text-align:left;">

Unplanned Readmission (Yes, No, Null)

</td>

</tr>

<tr>

<td style="text-align:left;">

<span style="color: grey; font-weight: bold"> </span>

</td>

<td style="text-align:left;">

reoperation

</td>

<td style="text-align:left;">

Unplannted Reoperation (Yes, No, Null)

</td>

</tr>

<tr>

<td style="text-align:left;">

<span style="color: grey; font-weight: bold"> </span>

</td>

<td style="text-align:left;">

death30yn

</td>

<td style="text-align:left;">

Death in 30 Days (Yes, No)

</td>

</tr>

<tr>

<td style="text-align:left;">

<span style="color: grey; font-weight: bold"> </span>

</td>

<td style="text-align:left;">

</td>

<td style="text-align:left;">

</td>

</tr>

<tr>

<td style="text-align:left;">

<span style="color: grey; font-weight: bold">Infection </span>

</td>

<td style="text-align:left;">

supinfec

</td>

<td style="text-align:left;">

Occurrences Superficial Incisional SSI (Superficial Incisional SSI, No
Complication)

</td>

</tr>

<tr>

<td style="text-align:left;">

<span style="color: grey; font-weight: bold"> </span>

</td>

<td style="text-align:left;">

wndinfd

</td>

<td style="text-align:left;">

Occurrences Deep Incisional SSI (Deep Incisional SSI, No Complication)

</td>

</tr>

<tr>

<td style="text-align:left;">

<span style="color: grey; font-weight: bold"> </span>

</td>

<td style="text-align:left;">

orgspcssi

</td>

<td style="text-align:left;">

Occurrences Organ/Space SSI (Organ/Space SSI, No Complication)

</td>

</tr>

<tr>

<td style="text-align:left;">

<span style="color: grey; font-weight: bold"> </span>

</td>

<td style="text-align:left;">

dehis

</td>

<td style="text-align:left;">

Occurrences Deep Wound Disruption/Dehiscence (Wound Disruption, No
complication)

</td>

</tr>

<tr>

<td style="text-align:left;">

<span style="color: grey; font-weight: bold"> </span>

</td>

<td style="text-align:left;">

oupneumo

</td>

<td style="text-align:left;">

Occurrences Pneumonia (Pneumonia, No Complication)

</td>

</tr>

<tr>

<td style="text-align:left;">

<span style="color: grey; font-weight: bold"> </span>

</td>

<td style="text-align:left;">

othcdiff

</td>

<td style="text-align:left;">

Occurrence of Postoperative Clostridium difficile (C.diff) Colitis (C.
Diff, No Complication)

</td>

</tr>

<tr>

<td style="text-align:left;">

<span style="color: grey; font-weight: bold"> </span>

</td>

<td style="text-align:left;">

othsysep

</td>

<td style="text-align:left;">

Occurrences Sepsis (Systemic Sepsis, No Complication)

</td>

</tr>

<tr>

<td style="text-align:left;">

<span style="color: grey; font-weight: bold"> </span>

</td>

<td style="text-align:left;">

urninfec

</td>

<td style="text-align:left;">

Occurrences Urinary Tract Infection (Urinary Tract Infection, No
Complication)

</td>

</tr>

<tr>

<td style="text-align:left;">

<span style="color: grey; font-weight: bold"> </span>

</td>

<td style="text-align:left;">

</td>

<td style="text-align:left;">

</td>

</tr>

<tr>

<td style="text-align:left;">

<span style="color: grey; font-weight: bold">Bleeding/Transfusion
</span>

</td>

<td style="text-align:left;">

ped\_spn\_post\_trnsvol\_cell

</td>

<td style="text-align:left;">

Postoperative Cell-Saver Transfusion (mL)

</td>

</tr>

<tr>

<td style="text-align:left;">

<span style="color: grey; font-weight: bold"> </span>

</td>

<td style="text-align:left;">

ped\_spn\_post\_trnsvol\_allogen

</td>

<td style="text-align:left;">

Postoperative Allogeneic Transfusion (mL)

</td>

</tr>

<tr>

<td style="text-align:left;">

<span style="color: grey; font-weight: bold"> </span>

</td>

<td style="text-align:left;">

othbleed

</td>

<td style="text-align:left;">

Occurrences Bleeding/Transfusion (Bleeding/Transfusion, No Complication)

</td>

</tr>

<tr>

<td style="text-align:left;">

<span style="color: grey; font-weight: bold"> </span>

</td>

<td style="text-align:left;">

</td>

<td style="text-align:left;">

</td>

</tr>

<tr>

<td style="text-align:left;">

<span style="color: grey; font-weight: bold">Other </span>

</td>

<td style="text-align:left;">

pulembol

</td>

<td style="text-align:left;">

Occurrences Pulmonary Embolism (Pulmonary Embolism, No Complication)

</td>

</tr>

<tr>

<td style="text-align:left;">

<span style="color: grey; font-weight: bold"> </span>

</td>

<td style="text-align:left;">

renainsf

</td>

<td style="text-align:left;">

Occurrences Progressive Renal Insufficiency (Progressive Renal
Insufficiency, No Complication)

</td>

</tr>

<tr>

<td style="text-align:left;">

<span style="color: grey; font-weight: bold"> </span>

</td>

<td style="text-align:left;">

cszre

</td>

<td style="text-align:left;">

Seizure Disorder (Seizure, No Complication)

</td>

</tr>

<tr>

<td style="text-align:left;">

<span style="color: grey; font-weight: bold"> </span>

</td>

<td style="text-align:left;">

cdarrest

</td>

<td style="text-align:left;">

Occurrences Cardiac Arrest Requiring CPR (Cardiac Arrest Requiring CPR,
No Complication)

</td>

</tr>

<tr>

<td style="text-align:left;">

<span style="color: grey; font-weight: bold"> </span>

</td>

<td style="text-align:left;">

neurodef

</td>

<td style="text-align:left;">

Nerve Injury (Nerve Injury, No Complication)

</td>

</tr>

<tr>

<td style="text-align:left;">

<span style="color: grey; font-weight: bold"> </span>

</td>

<td style="text-align:left;">

ped\_spn\_post\_neurodef

</td>

<td style="text-align:left;">

Postoperative Neurological Deficit

</td>

</tr>

<tr>

<td style="text-align:left;">

<span style="color: grey; font-weight: bold"> </span>

</td>

<td style="text-align:left;">

ped\_spn\_post\_neurodeftype

</td>

<td style="text-align:left;">

Postoperative Neurological Deficit Type (Cauda equina injury, Peripheral
nerve, plexus or nerve root injury, Spinal cord injury, NULL)

</td>

</tr>

</tbody>

</table>

Therefore we will tidy the data to that end.

<h1 id="tidying">

Tidying the Data

</h1>

## Data Reduction & Cleaning

To start the cleaning, we replaced the value “-99” with NA to indicate
non-zero missingness. We converted age in days to age in years and
height and weight to the metric system. We then created a BMI estimate
based on height and weight. American Society of Anesthesiologists
Physical Status Classification (ASA Class) was converted from a
character variable to a factor variable. Extent of surgical fusion
(inflection point 13 levels) was used to create a dichotomous variable,
level\_13. A dichotomous variable indicating whether the patient
returned home or was discharged to a facility was created and null
values deleted. A subset of the data was selected for analysis.

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
      prncptx == "ARTHRODESIS, POSTERIOR, FOR SPINAL DEFORMITY, WITH OR WITHOUT CAST; 13 OR MORE VERTEBRAL SEGMENTS" ~ "TRUE")) %>%
  filter(home_discharge != "NA") %>% 
  select(pufyear_x:ped_spn_post_neurodeftype, age_years, sex, height, weight, bmi, ethnicity_hispanic, race, asa_status, transt, ventilat, asthma, hxcld, oxygen_sup, crf, impcogstat, seizure, nutr_support, hemodisorder, level_13, optime, tothlos, d_opto_dis, death30yn, supinfec, wndinfd, orgspcssi, dehis, oupneumo, pulembol, renainsf, urninfec, cszre, neurodef, cdarrest, othbleed, bleed_ml_tot, othcdiff, othsysep, unplannedreadmission1, reoperation, dischdest, home_discharge)
```
