Predicting Non-Routine Discharge in Cerebral Palsy Patients Undergoing
Spinal Fusion
================

![Wheelchair](./images/wheelchair.jpg)

# Table of Contents

  - <a href="#intro">Introduction</a>
  - <a href="#characterize">Data Characterization</a>
  - <a href="#tidying">Tidying The Data</a>
  - <a href="#explore">Data Exploration</a>
  - <a href="#model">Modeling Discharge Status</a>
  - <a href="#discuss">Discussion</a>
  - <a href="#refs">References</a>

-----

### Authors

  - Katherine Dimitripoulou PhD (UNI: kd2524)
  - Jerri Chen MD, PhD (UNI: jc4166)
  - Lisa Eisler MD (UNI: ldl2113)

<h1 id="intro">

Introduction

</h1>

## Project Motivation and Literature Review

Children with cerebral palsy (CP) often present with a curvature of the
spine, or scoliosis, as a result of abnormal muscle tone and postural
weakness.<sup>[1](https://www.flintrehab.com/2019/cerebral-palsy-scoliosis/)</sup>
The clinical characteristics of scoliosis in CP patients involve a
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

In all studies, there appear to be pre- and/or peri-operative risk
factors associated with increased complications, but no study has
comprehensively examined these factors. In addition, no study has
examined non-routine discharge, with discharge to home (or home
facility) assumed to be the routine as well as preferred outcome from a
rehabilitation standpoint. We believe that patients and their families
benefit from reasonable expectations about the chance of a decline in
function, higher-level care needs or even death in the initial 30-day
postoperative period. A model predicting non-routine discharge (defined
as discharge to a higher level of care including postoperative death)
may aid in patient selection and family decision making. We consider the
outcome of non-home discharge in particular to be an indicator that
spinal fusion surgery has failed to improve quality of life or
funcitonal status, at least in the short term and perhaps permanently.

Our team comprises anesthesiologists and rehabilitation therapists who
often care for patients with CP. We can combine expertise to develop
behavioral and biological intervention protocols, and to make decisions
regarding the proper timing and optimization for surgery.

<h1 id="characterize">

Data Characterization

</h1>

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

After data cleaning, we had a dataset with 822 observations and 57
variables total.

<h1 id="explore">

Data Exploration

</h1>

## Descriptive Analyses

To give a sense of the overall demographic and medical characteristics
of our cohort, continuous and categorical variables considered to be
predictors or covariates impacting adverse outcomes are included in the
following table, as mean(SD) or n(%).

``` r
cp_spine_table1 = cp_spine_tidy %>%
mutate(
    sex = factor(sex, ordered = TRUE, levels = c("Female", "Male")),
    race = factor(race, ordered = FALSE, levels = c("American Indian or Alaska Native", "Asian", "Black or African American", "Native Hawaiian or Other Pacific Islander", "Unknown/Not Reported", "White")),
    admit_from = factor(transt, ordered = TRUE, levels = c("Admitted from home/clinic/doctor's office", "Admitted through ER including outside ER with direct hospital admission", "Chronic care/Rehab/Intermediate Care/Spinal Cord", "Transferred from outside hospital (NICU, PICU, Inpatient on General floor, Adult)", "Other")),
    ASAstatus = factor(asa_status, ordered = TRUE, levels = c("1", "2", "3", "4", "5")),
    ventilator_dependence = factor(ventilat, ordered = TRUE, levels = c("No", "Yes")),
    asthma = factor(asthma, ordered = TRUE, levels = c("No", "Yes")),
    home_oxygen = factor(oxygen_sup, ordered = TRUE, levels = c("No", "Yes")),
    cognitive_impairment = factor(impcogstat, ordered = TRUE, levels = c("No", "Yes")),
    seizure_disorder = factor(seizure, ordered = TRUE, levels = c("No", "Yes")),
    nutritional_support = factor(nutr_support, ordered = TRUE, levels = c("No", "Yes")),
    hematologic_disorder = factor(hemodisorder, ordered = TRUE, levels = c("No", "Yes"))
)
myVars <- c("age_years", "sex", "height", "weight", "ASAstatus", "admit_from", "ventilator_dependence", "asthma", "home_oxygen", "cognitive_impairment", "seizure_disorder", "nutritional_support", "hematologic_disorder")
catVars <- c("sex", "ASAstatus", "admit_from", "ventilator_dependence", "asthma", "home_oxygen", "cognitive_impairment", "seizure_disorder", "nutritional_support", "hematologic_disorder")
tab3 <- CreateTableOne(vars = myVars, data = cp_spine_table1, factorVars = catVars)
```

``` r
tab3df = print(tab3)
```

    ##                                                      
    ##                                                       Overall       
    ##   n                                                      822        
    ##   age_years (mean (SD))                                13.13 (2.78) 
    ##   sex = Male (%)                                         398 (48.4) 
    ##   height (mean (SD))                                  134.84 (17.56)
    ##   weight (mean (SD))                                   33.58 (10.96)
    ##   ASAstatus (%)                                                     
    ##      1                                                     2 ( 0.2) 
    ##      2                                                    75 ( 9.1) 
    ##      3                                                   672 (81.9) 
    ##      4                                                    72 ( 8.8) 
    ##   admit_from (%)                                                    
    ##      Admitted from home/clinic/doctor's office           789 (97.0) 
    ##      Chronic care/Rehab/Intermediate Care/Spinal Cord      9 ( 1.1) 
    ##      Other                                                15 ( 1.8) 
    ##   ventilator_dependence = Yes (%)                         83 (10.1) 
    ##   asthma = Yes (%)                                       191 (23.2) 
    ##   home_oxygen = Yes (%)                                   61 ( 7.4) 
    ##   cognitive_impairment = Yes (%)                         749 (91.1) 
    ##   seizure_disorder = Yes (%)                             520 (63.3) 
    ##   nutritional_support = Yes (%)                          416 (50.6) 
    ##   hematologic_disorder = Yes (%)                          52 ( 6.3)

From this table, we can appreciate that this is not a healthy
population. The majority of our patients were American Society of
Anesthesiologists (ASA) Physical Status 3-4, indicating that their
anesthesiologist felt that they suffered severe systemic disease, with
substantive functional limitations. 97% of patients were being cared for
at home preoperatively, and 10% were ventilator dependent. 91% were
cognitively impaired, with ~50% requiring nutritional support.

Next, to give a sense of the frequency of adverse events in our overall
cohort, categorical outcome variables are included in the following
table, as n(%).

``` r
cp_spine_table2 = cp_spine_tidy %>%
mutate(
    urinary_tract_infection = factor(urninfec, ordered = TRUE, levels = c("No Complication","Urinary Tract Infection")),
    wound_infection = factor(wndinfd, ordered = FALSE, levels = c("No Complication", "Deep Incisional SSI")),
    home_discharge = factor(home_discharge, ordered = TRUE, levels = c("TRUE", "FALSE")),
    reoperation = factor(reoperation, levels = c("No", "Yes")),
    death_in_30_days = factor(death30yn, ordered = TRUE, levels = c("No", "Yes"))
)
myVars <- c("urinary_tract_infection", "wound_infection", "home_discharge", "reoperation", "death_in_30_days")
catVars <- c("urinary_tract_infection", "wound_infection", "home_discharge", "reoperation", "death_in_30_days")
tab4 <- CreateTableOne(vars = myVars, data = cp_spine_table2, factorVars = catVars)
```

``` r
tab4df = print(tab4)
```

    ##                                                        
    ##                                                         Overall   
    ##   n                                                     822       
    ##   urinary_tract_infection = Urinary Tract Infection (%)  19 (2.3) 
    ##   wound_infection = Deep Incisional SSI (%)              17 (2.1) 
    ##   home_discharge = FALSE (%)                             53 (6.4) 
    ##   reoperation = Yes (%)                                  50 (6.1) 
    ##   death_in_30_days = Yes (%)                              6 (0.7)

The most common major adverse outcome was non-home discharge, impacting
6.4% (n=53) of patients. Given the frequency of non-home discharge and
our clinical interests in preventing adverse peri-operative outcomes and
in rehabilitation of children with CP, we decided to focus our analysis
on this outcome.

The following plot shows similar numbers of male and female patients who
had home vs. non-home discharge.

``` r
cp_spine_tidy %>%
  group_by(home_discharge) %>% 
  count(sex) %>% 
  plot_ly(
    x = ~home_discharge, y = ~n, color = ~sex, type = "bar"
    ) %>% 
  layout(
    xaxis = list(title = "Home Discharge"),
    yaxis = list(title = "Number of Patients"))
```

<!--html_preserve-->

<div id="htmlwidget-ef391257415ac28049a0" class="plotly html-widget" style="width:90%;height:480px;">

</div>

<script type="application/json" data-for="htmlwidget-ef391257415ac28049a0">{"x":{"visdat":{"2c623a9312bc":["function () ","plotlyVisDat"]},"cur_data":"2c623a9312bc","attrs":{"2c623a9312bc":{"x":{},"y":{},"color":{},"alpha_stroke":1,"sizes":[10,100],"spans":[1,20],"type":"bar"}},"layout":{"margin":{"b":40,"l":60,"t":25,"r":10},"xaxis":{"domain":[0,1],"automargin":true,"title":"Home Discharge","type":"category","categoryorder":"array","categoryarray":["FALSE","TRUE"]},"yaxis":{"domain":[0,1],"automargin":true,"title":"Number of Patients"},"hovermode":"closest","showlegend":true},"source":"A","config":{"showSendToCloud":false},"data":[{"x":["FALSE","TRUE"],"y":[28,396],"type":"bar","name":"Female","marker":{"color":"rgba(102,194,165,1)","line":{"color":"rgba(102,194,165,1)"}},"textfont":{"color":"rgba(102,194,165,1)"},"error_y":{"color":"rgba(102,194,165,1)"},"error_x":{"color":"rgba(102,194,165,1)"},"xaxis":"x","yaxis":"y","frame":null},{"x":["FALSE","TRUE"],"y":[25,373],"type":"bar","name":"Male","marker":{"color":"rgba(141,160,203,1)","line":{"color":"rgba(141,160,203,1)"}},"textfont":{"color":"rgba(141,160,203,1)"},"error_y":{"color":"rgba(141,160,203,1)"},"error_x":{"color":"rgba(141,160,203,1)"},"xaxis":"x","yaxis":"y","frame":null}],"highlight":{"on":"plotly_click","persistent":false,"dynamic":false,"selectize":false,"opacityDim":0.2,"selected":{"opacity":1},"debounce":0},"shinyEvents":["plotly_hover","plotly_click","plotly_selected","plotly_relayout","plotly_brushed","plotly_brushing","plotly_clickannotation","plotly_doubleclick","plotly_deselect","plotly_afterplot"],"base_url":"https://plot.ly"},"evals":[],"jsHooks":[]}</script>

<!--/html_preserve-->

The distribution of ages for patients with non-home discharge trends
higher than patients with home discharge.

``` r
cp_spine_tidy %>%
  plot_ly(y = ~age_years, color = ~home_discharge, type = "box",
          colors = "Set2") %>% 
  layout(
    xaxis = list(title = "Home Discharge"),
    yaxis = list(title = "Age (Years)"))
```

<!--html_preserve-->

<div id="htmlwidget-022af04197621093dc7f" class="plotly html-widget" style="width:90%;height:480px;">

</div>

<script type="application/json" data-for="htmlwidget-022af04197621093dc7f">{"x":{"visdat":{"2c62644c9b3":["function () ","plotlyVisDat"]},"cur_data":"2c62644c9b3","attrs":{"2c62644c9b3":{"y":{},"color":{},"colors":"Set2","alpha_stroke":1,"sizes":[10,100],"spans":[1,20],"type":"box"}},"layout":{"margin":{"b":40,"l":60,"t":25,"r":10},"xaxis":{"domain":[0,1],"automargin":true,"title":"Home Discharge"},"yaxis":{"domain":[0,1],"automargin":true,"title":"Age (Years)"},"hovermode":"closest","showlegend":true},"source":"A","config":{"showSendToCloud":false},"data":[{"fillcolor":"rgba(102,194,165,0.5)","y":[14.4476386036961,17.990417522245,9.52772073921971,17.6043805612594,14.8117727583847,13.6728268309377,11.0499657768652,15.9342915811088,17.8973305954825,17.6563997262149,17.3141683778234,14.9267624914442,16.3860369609856,8.74469541409993,16.0520191649555,17.5195071868583,15.1238877481177,11.8220396988364,12.9226557152635,15.088295687885,13.8507871321013,11.8220396988364,12.6789869952088,14.1793292265572,13.1444216290212,16.8240930869268,12.6105407255305,17.9137577002053,12.145106091718,17.4702258726899,8.11772758384668,12.4818617385352,14.2696783025325,10.631074606434,16.6078028747433,15.9726214921287,16.2464065708419,16.5941136208077,10.8692676249144,10.8747433264887,15.8110882956879,11.5154004106776,13.3935660506502,8.31759069130732,16.8870636550308,15.8165639972621,13.1060917180014,16.3504449007529,16.3613963039014,11.4579055441478,17.1937029431896,13.3963039014374,14.5407255304586],"type":"box","name":"FALSE","marker":{"color":"rgba(102,194,165,1)","line":{"color":"rgba(102,194,165,1)"}},"line":{"color":"rgba(102,194,165,1)"},"xaxis":"x","yaxis":"y","frame":null},{"fillcolor":"rgba(179,179,179,0.5)","y":[17.1498973305955,15.378507871321,14.4969199178645,12.829568788501,14.0698151950719,9.19644079397673,9.12251882272416,13.223819301848,10.1738535249829,10.4695414099932,17.3634496919918,16.4517453798768,11.5208761122519,14.8856947296372,3.62217659137577,14.6447638603696,17.8261464750171,14.0396988364134,9.90280629705681,10.3107460643395,14.8939082819986,11.8001368925394,16.6954140999316,16.8240930869268,14.8062970568104,15.4688569472964,14.5051334702259,15.8110882956879,14.8145106091718,12.7802874743326,9.51676933607118,9.01300479123888,12.684462696783,10.0780287474333,17.305954825462,11.7618069815195,14.280629705681,8.68720054757016,14.7707049965777,15.8713210130048,6.13278576317591,13.1279945242984,9.47843942505133,11.2799452429843,17.9767282683094,8.68720054757016,10.1437371663244,17.7878165639973,14.5544147843943,11.9780971937029,12.4517453798768,13.8754277891855,11.0910335386721,12.829568788501,13.6098562628337,17.6892539356605,17.8891170431211,13.8535249828884,11.4387405886379,16.5886379192334,17.1526351813826,13.9192334017796,15.5181382614648,12.9774127310062,12.1478439425051,10.2696783025325,10.7953456536619,16.807665982204,6.88569472963724,7.56741957563313,15.3538672142368,10.798083504449,17.6454483230664,11.8028747433265,12.0082135523614,11.772758384668,15.9945242984257,16.9253935660507,9.96303901437372,10.6584531143053,7.23340177960301,10.7624914442163,12.5366187542779,13.6728268309377,14.1957563312799,10.8117727583847,11.772758384668,15.4661190965092,13.8507871321013,13.0075290896646,16.1505817932923,10.798083504449,13.678302532512,12.2546201232033,10.0123203285421,12.9034907597536,12.6461327857632,17.4373716632444,9.92470910335387,9.89185489390828,9.27310061601643,14.362765229295,11.047227926078,17.8699520876112,11.6851471594798,13.8918548939083,13.6974674880219,5.76317590691307,15.6358658453114,12.9281314168378,16.1916495550992,15.1841204654346,14.9075975359343,11.9260780287474,7.83299110198494,10.0205338809035,12.2108145106092,14.4832306639288,12.6872005475702,8.0766598220397,10.5681040383299,17.0869267624914,15.3100616016427,16.3915126625599,14.9267624914442,15.3839835728953,13.3661875427789,13.3305954825462,10.8774811772758,10.611909650924,13.7275838466804,17.4264202600958,12.5256673511294,17.0130047912389,14.715947980835,9.40999315537303,14.4887063655031,3.34017796030116,13.3579739904175,8.76933607118412,15.2580424366872,13.4757015742642,12.0766598220397,12.7145790554415,15.0280629705681,12.0574948665298,16.4791238877481,13.7768651608487,12.8925393566051,9.68651608487338,15.211498973306,16.7282683093771,5.50034223134839,12.0958247775496,9.76317590691307,14.6009582477755,10.7898699520876,11.8193018480493,9.9356605065024,14.3545516769336,17.2511978097194,11.564681724846,11.8056125941136,17.8507871321013,11.1567419575633,13.9794661190965,11.0034223134839,11.9342915811088,12.3969883641342,13.9466119096509,11.5783709787817,15.6577686516085,13.2785763175907,13.4346338124572,17.9931553730322,15.7043121149897,13.1444216290212,16.5831622176591,15.7481177275838,12.9281314168378,14.507871321013,10.0561259411362,12.0328542094456,17.1800136892539,10.4010951403149,10.4969199178645,11.9014373716632,11.4852840520192,14.7789185489391,8.88158795345654,8.84052019164955,17.7522245037645,16.8843258042437,16.4599589322382,10.7871321013005,13.9000684462697,14.2258726899384,10.0561259411362,15.0910335386721,14.6584531143053,14.3518138261465,17.1964407939767,16.9691991786448,15.337440109514,12.6242299794661,10.6858316221766,10.6830937713895,13.6043805612594,11.1813826146475,16.3093771389459,10.6502395619439,14.0917180013689,14.4941820670773,6.23408624229979,10.3463381245722,16.4407939767283,13.8973305954825,15.9863107460643,11.854893908282,16.8514715947981,11.1238877481177,13.8507871321013,17.2457221081451,13.0458590006845,9.73853524982888,11.0116358658453,12.1122518822724,15.8740588637919,12.3066392881588,8.64065708418891,15.0143737166324,12.0136892539357,15.5071868583162,14.715947980835,10.1464750171116,13.6481861738535,13.9520876112252,12.9500342231348,17.9110198494182,13.8179329226557,11.2470910335387,15.9096509240246,13.7221081451061,4.89253935660506,15.0691307323751,17.7932922655715,12.4243668720055,16.7939767282683,12.7008898015058,13.3114305270363,9.01300479123888,12.9637234770705,14.1327857631759,15.0828199863107,13.5359342915811,10.1273100616016,17.2539356605065,12.9555099247091,13.45106091718,8.55852156057495,8.02190280629706,12.0109514031485,13.3798767967146,15.0527036276523,9.98494182067077,16.3258042436687,4.27378507871321,17.1663244353183,10.1327857631759,16.3778234086242,13.347022587269,10.0123203285421,10.7542778918549,13.8891170431211,13.1252566735113,16.1615331964408,17.4674880219028,13.9383983572895,14.9486652977413,12.7802874743326,10.5270362765229,10.4421629021218,10.3271731690623,13.2867898699521,9.67830253251198,11.605749486653,14.631074606434,17.6509240246407,14.4558521560575,14.9075975359343,10.8583162217659,11.9561943874059,16.8514715947981,16.3477070499658,11.7864476386037,15.3401779603012,15.3648186173854,13.596167008898,13.5386721423682,12.4900752908966,14.9514031485284,13.8836413415469,13.2128678986995,17.4702258726899,14.7926078028747,15.9644079397673,12.5092402464066,6.02053388090349,15.3648186173854,14.2669404517454,13.0869267624914,13.1635865845311,16.2628336755647,7.5482546201232,14.1683778234086,12.4106776180698,17.7878165639973,14.0561259411362,15.0280629705681,12.1615331964408,15.1841204654346,10.2751540041068,10.6338124572211,10.3162217659138,13.5687885010267,9.03764544832307,14.7132101300479,13.6208076659822,16.3093771389459,10.17659137577,14.6338124572211,16.7227926078029,13.7768651608487,12.249144421629,15.3264887063655,11.025325119781,11.5208761122519,15.8083504449008,8.71457905544148,10.6995208761123,17.8562628336756,16.249144421629,16.7583846680356,17.1006160164271,17.1964407939767,9.48665297741273,13.5879534565366,13.4346338124572,12.0602327173169,16.6351813826146,11.3045859000684,11.8275154004107,13.8179329226557,10.299794661191,13.2758384668036,10.5160848733744,15.6851471594798,9.8590006844627,12.5092402464066,13.2046543463381,16.2245037645448,12.1779603011636,10.3764544832307,12.3394934976044,16.9500342231348,16.7200547570157,15.6988364134155,12.1314168377823,14.2340862422998,11.066392881588,14.7214236824093,10.3381245722108,11.4934976043806,12.8843258042437,14.9130732375086,10.3436002737851,14.6666666666667,16.3613963039014,14.7022587268994,12.5941136208077,13.0266940451745,17.4757015742642,8.21081451060917,13.07871321013,6.72142368240931,15.0581793292266,15.9972621492129,13.2950034223135,17.9794661190965,12.8925393566051,12.2135523613963,7.96167008898015,12.4435318275154,14.4777549623546,16.0383299110199,17.7275838466804,13.2895277207392,15.2826830937714,12.8405201916496,14.1108829568789,8.8186173853525,14.4476386036961,10.7515400410678,16.1834360027378,16.3832991101985,11.7152635181383,10.8528405201917,15.958932238193,12.4791238877481,16.6735112936345,10.4887063655031,14.7323750855578,13.6125941136208,10.9322381930185,9.77960301163587,10.4777549623546,15.3702943189596,12.9363449691992,11.27446954141,12.1286789869952,13.6016427104723,14.5927446954141,15.6714579055441,11.1594798083504,14.8774811772758,14.7296372347707,12.290212183436,11.937029431896,13.8672142368241,12.3367556468172,13.2511978097194,13.9219712525667,14.5954825462012,10.9568788501027,8.8186173853525,15.8329911019849,13.1143052703628,17.3853524982888,13.5030800821355,14.8720054757016,12.0191649555099,11.5920602327173,11.4496919917864,13.9082819986311,10.4339493497604,14.7488021902806,15.0088980150582,15.0116358658453,16.2135523613963,17.1526351813826,12.1998631074606,14.7515400410678,17.3333333333333,11.5975359342916,14.6584531143053,12.9089664613279,12.6735112936345,9.65639972621492,11.5154004106776,17.4866529774127,15.2525667351129,15.0609171800137,16.2436687200548,13.6509240246407,15.0362765229295,14.8090349075975,13.8672142368241,17.7029431895962,13.4045174537988,13.8754277891855,12.2847364818617,12.4599589322382,9.33333333333333,15.9397672826831,13.015742642026,16.1889117043121,14.570841889117,8.4435318275154,6.09445585215606,14.2751540041068,17.5578370978782,16.5530458590007,10.7104722792608,12.5585215605749,13.9110198494182,17.2676249144422,15.0308008213552,11.7754962354552,13.5359342915811,10.7679671457906,12.5530458590007,9.1937029431896,13.0184804928131,11.9151266255989,13.7932922655715,9.31416837782341,12.621492128679,13.4784394250513,8.33675564681725,9.99041752224504,8.65160848733744,15.5400410677618,10.7049965776865,16.6351813826146,7.26351813826146,9.6974674880219,15.8028747433265,17.4729637234771,12.3586584531143,11.9069130732375,12.7392197125257,12.952772073922,16.5420944558522,16.3285420944559,11.6002737850787,12.9199178644764,8.0082135523614,14.1656399726215,13.6947296372348,8.69815195071869,13.0403832991102,12.5311430527036,11.6084873374401,13.2703627652293,12.2819986310746,10.2368240930869,10.1957563312799,15.356605065024,13.82340862423,10.5462012320329,10.1492128678987,14.0424366872005,10.6255989048597,13.3497604380561,9.68377823408624,11.3073237508556,15.3483915126626,11.7973990417522,8.21629021218344,9.82888432580424,12.2628336755647,13.8343600273785,13.6919917864476,12.4709103353867,12.0903490759754,16.0273785078713,13.8836413415469,13.1718001368925,14.1957563312799,14.1026694045175,12.227241615332,15.3702943189596,16.0766598220397,12.6570841889117,15.791923340178,12.7282683093771,14.7378507871321,10.4093086926762,14.8199863107461,12.8377823408624,16.2026009582478,7.08008213552361,8.76933607118412,8.22724161533196,10.4284736481862,11.9753593429158,17.4647501711157,4.44900752908966,16.1286789869952,12.4134154688569,12.7912388774812,15.1868583162218,15.5071868583162,6.12731006160164,11.8439425051335,10.0971937029432,9.88364134154689,14.0260095824778,10.4476386036961,11.8083504449008,10.3682409308693,11.5865845311431,12.9171800136893,14.9103353867214,16.2026009582478,10.7816563997262,15.1786447638604,12.8158795345654,15.8986995208761,15.4223134839151,13.9876796714579,8.49555099247091,6.59000684462697,17.782340862423,8.74469541409993,13.2758384668036,14.0479123887748,13.0075290896646,7.20602327173169,15.709787816564,11.6194387405886,11.2005475701574,10.9760438056126,14.631074606434,12.0958247775496,13.0266940451745,11.5564681724846,16.7145790554415,15.1211498973306,9.637234770705,14.7898699520876,13.6399726214921,12.7474332648871,15.3730321697467,15.0444900752909,12.2819986310746,16.9664613278576,14.0917180013689,12.1149897330595,13.0951403148528,14.1245722108145,11.2607802874743,12.5338809034908,13.533196440794,11.8877481177276,12.7583846680356,11.6878850102669,16.766598220397,12.6543463381246,9.93839835728953,12.3832991101985,13.9383983572895,10.4558521560575,11.0362765229295,14.1629021218344,13.242984257358,9.68104038329911,16.8678986995209,11.6933607118412,15.9534565366188,15.1759069130732,12.1724845995893,11.2635181382615,11.7125256673511,17.4318959616701,14.0424366872005,8.69815195071869,11.2908966461328,16.7419575633128,15.668720054757,16.9281314168378,13.4455852156057,15.3264887063655,8.07392197125257,17.2292950034223,17.2484599589322,14.6502395619439,12.4298425735797,15.4798083504449,15.8302532511978,13.2895277207392,13.3716632443532,10.6475017111567,12.2354551676934,11.2689938398357,14.2313483915127,14.8281998631075,10.6557152635181,15.1649555099247,16.7638603696099,5.19917864476386,9.78507871321013,12.1286789869952,8.99931553730322,12.643394934976,15.6303901437372,8.42710472279261,15.9479808350445,14.4503764544832,14.694045174538,7.58110882956879,15.1321013004791,12.9500342231348,12.7529089664613,15.4414784394251,16.1560574948665,14.0999315537303,12.870636550308,15.8713210130048,10.3518138261465,14.444900752909,15.7262149212868,12.1724845995893,13.1882272416153,11.709787816564,14.7104722792608,14.4421629021218,8.56399726214921,11.8466803559206,12.0164271047228,5.31964407939767,13.6618754277892,11.8001368925394,12.5667351129363,10.9130732375086,13.0814510609172,14.5242984257358,11.9096509240246,14.362765229295,16.6899383983573,17.119780971937,11.9479808350445,11.6358658453114,14.6447638603696,13.2320328542094,11.6495550992471,10.1382614647502,14.2505133470226,10.9349760438056,11.5756331279945,13.4729637234771,10.4421629021218,7.57289527720739,14.8008213552361,16.9911019849418,15.0390143737166,4.79397672826831,9.78507871321013,17.3607118412047,17.3196440793977,10.2696783025325,11.0499657768652,13.1279945242984,10.4777549623546,15.7864476386037,14.5598904859685,13.1964407939767,11.9534565366188,10.5133470225873,16.5585215605749,15.0828199863107,10.5681040383299,11.211498973306,16.1615331964408,8.65982203969884,4.94729637234771,13.5578370978782,11.9479808350445,9.12525667351129,12.2847364818617,13.7467488021903,14.2751540041068,11.9835728952772,14.5297741273101,16.8843258042437,15.6194387405886,16.6516084873374,17.0677618069815,16.5667351129363,9.4839151266256,14.8117727583847,16.4845995893224,15.4223134839151,11.8822724161533,14.4531143052704,6.39561943874059,13.9383983572895,8.65982203969884,12.0793976728268],"type":"box","name":"TRUE","marker":{"color":"rgba(179,179,179,1)","line":{"color":"rgba(179,179,179,1)"}},"line":{"color":"rgba(179,179,179,1)"},"xaxis":"x","yaxis":"y","frame":null}],"highlight":{"on":"plotly_click","persistent":false,"dynamic":false,"selectize":false,"opacityDim":0.2,"selected":{"opacity":1},"debounce":0},"shinyEvents":["plotly_hover","plotly_click","plotly_selected","plotly_relayout","plotly_brushed","plotly_brushing","plotly_clickannotation","plotly_doubleclick","plotly_deselect","plotly_afterplot"],"base_url":"https://plot.ly"},"evals":[],"jsHooks":[]}</script>

<!--/html_preserve-->

The following plots show the percent of patients with non-home
discharge, stratified by race or ethnicity.

``` r
cp_spine_tidy %>%
  group_by(home_discharge) %>% 
  count(race) %>% 
  pivot_wider(
    names_from = "home_discharge",
    values_from = "n"
  ) %>% 
  janitor::clean_names() %>% 
  mutate(percent_nothome = (false/(true + false))*100) %>% 
  replace(is.na(.), 0) %>% 
  plot_ly(
    x = ~race, y = ~percent_nothome, type = "bar"
    ) %>% 
  layout(
    xaxis = list(title = "Race"),
    yaxis = list(title = "Percent Non-Home Discharge"))
```

<!--html_preserve-->

<div id="htmlwidget-adfcc1aabc563557df7e" class="plotly html-widget" style="width:90%;height:480px;">

</div>

<script type="application/json" data-for="htmlwidget-adfcc1aabc563557df7e">{"x":{"visdat":{"2c625b75c5f":["function () ","plotlyVisDat"]},"cur_data":"2c625b75c5f","attrs":{"2c625b75c5f":{"x":{},"y":{},"alpha_stroke":1,"sizes":[10,100],"spans":[1,20],"type":"bar"}},"layout":{"margin":{"b":40,"l":60,"t":25,"r":10},"xaxis":{"domain":[0,1],"automargin":true,"title":"Race","type":"category","categoryorder":"array","categoryarray":["American Indian or Alaska Native","Asian","Black or African American","Native Hawaiian or Other Pacific Islander","Unknown/Not Reported","White"]},"yaxis":{"domain":[0,1],"automargin":true,"title":"Percent Non-Home Discharge"},"hovermode":"closest","showlegend":false},"source":"A","config":{"showSendToCloud":false},"data":[{"x":["Asian","Black or African American","Unknown/Not Reported","White","American Indian or Alaska Native","Native Hawaiian or Other Pacific Islander"],"y":[12.5,7.38916256157635,11.4942528735632,4.99001996007984,0,0],"type":"bar","marker":{"color":"rgba(31,119,180,1)","line":{"color":"rgba(31,119,180,1)"}},"error_y":{"color":"rgba(31,119,180,1)"},"error_x":{"color":"rgba(31,119,180,1)"},"xaxis":"x","yaxis":"y","frame":null}],"highlight":{"on":"plotly_click","persistent":false,"dynamic":false,"selectize":false,"opacityDim":0.2,"selected":{"opacity":1},"debounce":0},"shinyEvents":["plotly_hover","plotly_click","plotly_selected","plotly_relayout","plotly_brushed","plotly_brushing","plotly_clickannotation","plotly_doubleclick","plotly_deselect","plotly_afterplot"],"base_url":"https://plot.ly"},"evals":[],"jsHooks":[]}</script>

<!--/html_preserve-->

``` r
cp_spine_tidy %>%
  group_by(home_discharge) %>% 
  count(ethnicity_hispanic) %>% 
  rename(ethnicity = ethnicity_hispanic) %>% 
  mutate(ethnicity = case_when(
    ethnicity == "No" ~ "Non-Hispanic",
    ethnicity == "Yes" ~ "Hispanic",
    ethnicity == "NULL" ~ "Other/Did Not Answer"
  )) %>% 
    pivot_wider(
    names_from = "home_discharge",
    values_from = "n"
  ) %>% 
  janitor::clean_names() %>% 
  mutate(percent_nothome = (false/(true + false))*100) %>% 
  replace(is.na(.), 0) %>% 
  plot_ly(
    x = ~ethnicity, y = ~percent_nothome, type = "bar"
    ) %>% 
  layout(
    xaxis = list(title = "Ethnicity"),
    yaxis = list(title = "Percent Non-Home Discharge"))
```

<!--html_preserve-->

<div id="htmlwidget-6729bfd3206e2ac83776" class="plotly html-widget" style="width:90%;height:480px;">

</div>

<script type="application/json" data-for="htmlwidget-6729bfd3206e2ac83776">{"x":{"visdat":{"2c62675f72b3":["function () ","plotlyVisDat"]},"cur_data":"2c62675f72b3","attrs":{"2c62675f72b3":{"x":{},"y":{},"alpha_stroke":1,"sizes":[10,100],"spans":[1,20],"type":"bar"}},"layout":{"margin":{"b":40,"l":60,"t":25,"r":10},"xaxis":{"domain":[0,1],"automargin":true,"title":"Ethnicity","type":"category","categoryorder":"array","categoryarray":["Hispanic","Non-Hispanic","Other/Did Not Answer"]},"yaxis":{"domain":[0,1],"automargin":true,"title":"Percent Non-Home Discharge"},"hovermode":"closest","showlegend":false},"source":"A","config":{"showSendToCloud":false},"data":[{"x":["Non-Hispanic","Other/Did Not Answer","Hispanic"],"y":[6.37329286798179,19.5121951219512,2.45901639344262],"type":"bar","marker":{"color":"rgba(31,119,180,1)","line":{"color":"rgba(31,119,180,1)"}},"error_y":{"color":"rgba(31,119,180,1)"},"error_x":{"color":"rgba(31,119,180,1)"},"xaxis":"x","yaxis":"y","frame":null}],"highlight":{"on":"plotly_click","persistent":false,"dynamic":false,"selectize":false,"opacityDim":0.2,"selected":{"opacity":1},"debounce":0},"shinyEvents":["plotly_hover","plotly_click","plotly_selected","plotly_relayout","plotly_brushed","plotly_brushing","plotly_clickannotation","plotly_doubleclick","plotly_deselect","plotly_afterplot"],"base_url":"https://plot.ly"},"evals":[],"jsHooks":[]}</script>

<!--/html_preserve-->

Because of the large “Other/Did Not Answer” group, it is difficult to
interpret the contribution of ethnicity as a risk factor for non-home
discharge.

While we focused on the home discharge outcome, during exploratory
analysis, we did attempt to see if there were any other interesting
relationships between potential risk factors and outcomes. For example:

### Age and Operation Time

``` r
cp_spine_tidy %>%
  plot_ly(
    x = ~age_years, y = ~optime, type = "scatter", mode = "markers", alpha = 0.5)  %>% 
  layout(
    xaxis = list(title = "Age (Years)"),
    yaxis = list(title = "Length of Operation (Minutes)"))
```

<!--html_preserve-->

<div id="htmlwidget-57db047386781ba3e44e" class="plotly html-widget" style="width:90%;height:480px;">

</div>

<script type="application/json" data-for="htmlwidget-57db047386781ba3e44e">{"x":{"visdat":{"2c6270cc0f92":["function () ","plotlyVisDat"]},"cur_data":"2c6270cc0f92","attrs":{"2c6270cc0f92":{"x":{},"y":{},"mode":"markers","alpha":0.5,"alpha_stroke":1,"sizes":[10,100],"spans":[1,20],"type":"scatter"}},"layout":{"margin":{"b":40,"l":60,"t":25,"r":10},"xaxis":{"domain":[0,1],"automargin":true,"title":"Age (Years)"},"yaxis":{"domain":[0,1],"automargin":true,"title":"Length of Operation (Minutes)"},"hovermode":"closest","showlegend":false},"source":"A","config":{"showSendToCloud":false},"data":[{"x":[17.1498973305955,15.378507871321,14.4476386036961,14.4969199178645,12.829568788501,14.0698151950719,9.19644079397673,9.12251882272416,13.223819301848,10.1738535249829,10.4695414099932,17.3634496919918,16.4517453798768,11.5208761122519,14.8856947296372,3.62217659137577,14.6447638603696,17.8261464750171,14.0396988364134,9.90280629705681,10.3107460643395,14.8939082819986,11.8001368925394,16.6954140999316,17.990417522245,16.8240930869268,14.8062970568104,15.4688569472964,9.52772073921971,14.5051334702259,15.8110882956879,14.8145106091718,12.7802874743326,9.51676933607118,9.01300479123888,12.684462696783,17.6043805612594,10.0780287474333,17.305954825462,14.8117727583847,11.7618069815195,14.280629705681,8.68720054757016,14.7707049965777,15.8713210130048,6.13278576317591,13.1279945242984,9.47843942505133,11.2799452429843,17.9767282683094,8.68720054757016,10.1437371663244,17.7878165639973,14.5544147843943,13.6728268309377,11.9780971937029,12.4517453798768,13.8754277891855,11.0910335386721,12.829568788501,13.6098562628337,17.6892539356605,17.8891170431211,13.8535249828884,11.4387405886379,16.5886379192334,17.1526351813826,13.9192334017796,15.5181382614648,11.0499657768652,12.9774127310062,12.1478439425051,10.2696783025325,10.7953456536619,16.807665982204,15.9342915811088,17.8973305954825,6.88569472963724,7.56741957563313,15.3538672142368,10.798083504449,17.6454483230664,11.8028747433265,12.0082135523614,11.772758384668,15.9945242984257,17.6563997262149,17.3141683778234,14.9267624914442,16.9253935660507,9.96303901437372,16.3860369609856,10.6584531143053,7.23340177960301,10.7624914442163,12.5366187542779,13.6728268309377,14.1957563312799,10.8117727583847,11.772758384668,15.4661190965092,13.8507871321013,13.0075290896646,16.1505817932923,10.798083504449,13.678302532512,12.2546201232033,10.0123203285421,12.9034907597536,12.6461327857632,17.4373716632444,8.74469541409993,9.92470910335387,16.0520191649555,9.89185489390828,9.27310061601643,14.362765229295,17.5195071868583,11.047227926078,17.8699520876112,11.6851471594798,13.8918548939083,13.6974674880219,5.76317590691307,15.6358658453114,12.9281314168378,16.1916495550992,15.1841204654346,14.9075975359343,11.9260780287474,7.83299110198494,10.0205338809035,12.2108145106092,14.4832306639288,12.6872005475702,8.0766598220397,10.5681040383299,17.0869267624914,15.3100616016427,16.3915126625599,14.9267624914442,15.3839835728953,15.1238877481177,13.3661875427789,13.3305954825462,10.8774811772758,10.611909650924,13.7275838466804,17.4264202600958,11.8220396988364,12.5256673511294,17.0130047912389,14.715947980835,9.40999315537303,14.4887063655031,3.34017796030116,13.3579739904175,8.76933607118412,15.2580424366872,13.4757015742642,12.0766598220397,12.7145790554415,15.0280629705681,12.0574948665298,16.4791238877481,13.7768651608487,12.8925393566051,9.68651608487338,15.211498973306,16.7282683093771,5.50034223134839,12.9226557152635,12.0958247775496,9.76317590691307,14.6009582477755,10.7898699520876,15.088295687885,11.8193018480493,9.9356605065024,14.3545516769336,17.2511978097194,11.564681724846,11.8056125941136,17.8507871321013,11.1567419575633,13.9794661190965,11.0034223134839,11.9342915811088,12.3969883641342,13.9466119096509,11.5783709787817,15.6577686516085,13.2785763175907,13.4346338124572,17.9931553730322,15.7043121149897,13.1444216290212,16.5831622176591,15.7481177275838,12.9281314168378,14.507871321013,10.0561259411362,12.0328542094456,17.1800136892539,10.4010951403149,10.4969199178645,11.9014373716632,11.4852840520192,14.7789185489391,13.8507871321013,8.88158795345654,8.84052019164955,17.7522245037645,16.8843258042437,16.4599589322382,11.8220396988364,10.7871321013005,13.9000684462697,12.6789869952088,14.2258726899384,10.0561259411362,15.0910335386721,14.1793292265572,14.6584531143053,14.3518138261465,17.1964407939767,16.9691991786448,15.337440109514,12.6242299794661,10.6858316221766,10.6830937713895,13.6043805612594,11.1813826146475,16.3093771389459,10.6502395619439,14.0917180013689,14.4941820670773,6.23408624229979,10.3463381245722,16.4407939767283,13.8973305954825,15.9863107460643,11.854893908282,16.8514715947981,11.1238877481177,13.8507871321013,17.2457221081451,13.0458590006845,9.73853524982888,11.0116358658453,12.1122518822724,15.8740588637919,12.3066392881588,8.64065708418891,15.0143737166324,12.0136892539357,15.5071868583162,14.715947980835,10.1464750171116,13.6481861738535,13.9520876112252,12.9500342231348,17.9110198494182,13.8179329226557,11.2470910335387,15.9096509240246,13.7221081451061,4.89253935660506,15.0691307323751,17.7932922655715,12.4243668720055,16.7939767282683,12.7008898015058,13.3114305270363,9.01300479123888,12.9637234770705,14.1327857631759,15.0828199863107,13.5359342915811,10.1273100616016,17.2539356605065,12.9555099247091,13.45106091718,8.55852156057495,8.02190280629706,12.0109514031485,13.3798767967146,15.0527036276523,9.98494182067077,16.3258042436687,4.27378507871321,17.1663244353183,10.1327857631759,16.3778234086242,13.347022587269,10.0123203285421,10.7542778918549,13.8891170431211,13.1252566735113,16.1615331964408,17.4674880219028,13.9383983572895,14.9486652977413,12.7802874743326,10.5270362765229,10.4421629021218,10.3271731690623,13.2867898699521,9.67830253251198,11.605749486653,14.631074606434,17.6509240246407,14.4558521560575,14.9075975359343,10.8583162217659,11.9561943874059,16.8514715947981,16.3477070499658,11.7864476386037,15.3401779603012,15.3648186173854,13.596167008898,13.1444216290212,13.5386721423682,12.4900752908966,14.9514031485284,13.8836413415469,13.2128678986995,17.4702258726899,14.7926078028747,15.9644079397673,12.5092402464066,6.02053388090349,15.3648186173854,14.2669404517454,13.0869267624914,13.1635865845311,16.2628336755647,7.5482546201232,14.1683778234086,12.4106776180698,17.7878165639973,14.0561259411362,15.0280629705681,12.1615331964408,15.1841204654346,10.2751540041068,10.6338124572211,10.3162217659138,13.5687885010267,9.03764544832307,14.7132101300479,13.6208076659822,16.8240930869268,16.3093771389459,10.17659137577,14.6338124572211,16.7227926078029,13.7768651608487,12.249144421629,15.3264887063655,11.025325119781,11.5208761122519,15.8083504449008,8.71457905544148,10.6995208761123,17.8562628336756,16.249144421629,16.7583846680356,17.1006160164271,12.6105407255305,17.1964407939767,9.48665297741273,13.5879534565366,13.4346338124572,12.0602327173169,16.6351813826146,11.3045859000684,11.8275154004107,13.8179329226557,10.299794661191,13.2758384668036,10.5160848733744,15.6851471594798,9.8590006844627,17.9137577002053,12.5092402464066,13.2046543463381,16.2245037645448,12.1779603011636,10.3764544832307,12.3394934976044,16.9500342231348,16.7200547570157,15.6988364134155,12.1314168377823,12.145106091718,14.2340862422998,11.066392881588,14.7214236824093,10.3381245722108,11.4934976043806,12.8843258042437,14.9130732375086,10.3436002737851,14.6666666666667,16.3613963039014,14.7022587268994,12.5941136208077,13.0266940451745,17.4757015742642,8.21081451060917,13.07871321013,6.72142368240931,15.0581793292266,15.9972621492129,13.2950034223135,17.9794661190965,12.8925393566051,12.2135523613963,7.96167008898015,12.4435318275154,14.4777549623546,16.0383299110199,17.7275838466804,17.4702258726899,13.2895277207392,15.2826830937714,12.8405201916496,14.1108829568789,8.8186173853525,14.4476386036961,10.7515400410678,16.1834360027378,16.3832991101985,11.7152635181383,8.11772758384668,10.8528405201917,15.958932238193,12.4791238877481,16.6735112936345,10.4887063655031,14.7323750855578,13.6125941136208,10.9322381930185,9.77960301163587,10.4777549623546,15.3702943189596,12.4818617385352,12.9363449691992,11.27446954141,12.1286789869952,13.6016427104723,14.5927446954141,15.6714579055441,11.1594798083504,14.8774811772758,14.7296372347707,12.290212183436,11.937029431896,13.8672142368241,12.3367556468172,13.2511978097194,13.9219712525667,14.5954825462012,10.9568788501027,8.8186173853525,15.8329911019849,13.1143052703628,17.3853524982888,13.5030800821355,14.8720054757016,12.0191649555099,11.5920602327173,11.4496919917864,13.9082819986311,10.4339493497604,14.7488021902806,15.0088980150582,15.0116358658453,16.2135523613963,17.1526351813826,12.1998631074606,14.7515400410678,17.3333333333333,11.5975359342916,14.6584531143053,12.9089664613279,12.6735112936345,9.65639972621492,14.2696783025325,11.5154004106776,17.4866529774127,15.2525667351129,15.0609171800137,16.2436687200548,13.6509240246407,15.0362765229295,14.8090349075975,13.8672142368241,17.7029431895962,13.4045174537988,13.8754277891855,12.2847364818617,12.4599589322382,9.33333333333333,15.9397672826831,13.015742642026,16.1889117043121,14.570841889117,8.4435318275154,6.09445585215606,10.631074606434,16.6078028747433,15.9726214921287,14.2751540041068,17.5578370978782,16.5530458590007,10.7104722792608,12.5585215605749,13.9110198494182,17.2676249144422,15.0308008213552,11.7754962354552,13.5359342915811,10.7679671457906,12.5530458590007,9.1937029431896,13.0184804928131,16.2464065708419,11.9151266255989,13.7932922655715,9.31416837782341,12.621492128679,13.4784394250513,8.33675564681725,9.99041752224504,8.65160848733744,15.5400410677618,10.7049965776865,16.6351813826146,7.26351813826146,9.6974674880219,15.8028747433265,17.4729637234771,12.3586584531143,11.9069130732375,12.7392197125257,12.952772073922,16.5420944558522,16.3285420944559,11.6002737850787,12.9199178644764,8.0082135523614,14.1656399726215,13.6947296372348,8.69815195071869,13.0403832991102,12.5311430527036,16.5941136208077,11.6084873374401,13.2703627652293,12.2819986310746,10.2368240930869,10.1957563312799,15.356605065024,13.82340862423,10.5462012320329,10.1492128678987,14.0424366872005,10.6255989048597,13.3497604380561,9.68377823408624,11.3073237508556,15.3483915126626,11.7973990417522,8.21629021218344,9.82888432580424,12.2628336755647,13.8343600273785,13.6919917864476,12.4709103353867,12.0903490759754,16.0273785078713,13.8836413415469,13.1718001368925,14.1957563312799,14.1026694045175,12.227241615332,15.3702943189596,16.0766598220397,12.6570841889117,15.791923340178,12.7282683093771,14.7378507871321,10.4093086926762,14.8199863107461,12.8377823408624,16.2026009582478,7.08008213552361,8.76933607118412,8.22724161533196,10.4284736481862,11.9753593429158,17.4647501711157,4.44900752908966,16.1286789869952,12.4134154688569,12.7912388774812,15.1868583162218,15.5071868583162,6.12731006160164,11.8439425051335,10.0971937029432,9.88364134154689,10.8692676249144,14.0260095824778,10.4476386036961,11.8083504449008,10.3682409308693,11.5865845311431,10.8747433264887,12.9171800136893,14.9103353867214,16.2026009582478,10.7816563997262,15.1786447638604,12.8158795345654,15.8986995208761,15.4223134839151,13.9876796714579,8.49555099247091,6.59000684462697,17.782340862423,8.74469541409993,13.2758384668036,14.0479123887748,13.0075290896646,7.20602327173169,15.709787816564,11.6194387405886,11.2005475701574,10.9760438056126,14.631074606434,12.0958247775496,13.0266940451745,11.5564681724846,16.7145790554415,15.1211498973306,9.637234770705,14.7898699520876,13.6399726214921,12.7474332648871,15.3730321697467,15.0444900752909,12.2819986310746,16.9664613278576,14.0917180013689,12.1149897330595,13.0951403148528,14.1245722108145,11.2607802874743,12.5338809034908,13.533196440794,11.8877481177276,12.7583846680356,11.6878850102669,16.766598220397,12.6543463381246,9.93839835728953,12.3832991101985,13.9383983572895,10.4558521560575,11.0362765229295,14.1629021218344,13.242984257358,9.68104038329911,16.8678986995209,11.6933607118412,15.9534565366188,15.1759069130732,12.1724845995893,11.2635181382615,11.7125256673511,17.4318959616701,14.0424366872005,8.69815195071869,11.2908966461328,16.7419575633128,15.668720054757,16.9281314168378,13.4455852156057,15.3264887063655,8.07392197125257,17.2292950034223,17.2484599589322,14.6502395619439,12.4298425735797,15.4798083504449,15.8302532511978,13.2895277207392,13.3716632443532,10.6475017111567,12.2354551676934,11.2689938398357,14.2313483915127,14.8281998631075,10.6557152635181,15.1649555099247,16.7638603696099,5.19917864476386,9.78507871321013,12.1286789869952,8.99931553730322,12.643394934976,15.6303901437372,8.42710472279261,15.9479808350445,14.4503764544832,14.694045174538,7.58110882956879,15.1321013004791,12.9500342231348,12.7529089664613,15.4414784394251,16.1560574948665,14.0999315537303,12.870636550308,15.8713210130048,10.3518138261465,14.444900752909,15.7262149212868,12.1724845995893,15.8110882956879,11.5154004106776,13.1882272416153,13.3935660506502,8.31759069130732,11.709787816564,14.7104722792608,14.4421629021218,8.56399726214921,11.8466803559206,12.0164271047228,5.31964407939767,13.6618754277892,11.8001368925394,12.5667351129363,16.8870636550308,10.9130732375086,15.8165639972621,13.0814510609172,14.5242984257358,11.9096509240246,14.362765229295,16.6899383983573,17.119780971937,11.9479808350445,11.6358658453114,14.6447638603696,13.2320328542094,11.6495550992471,10.1382614647502,14.2505133470226,13.1060917180014,10.9349760438056,16.3504449007529,11.5756331279945,13.4729637234771,10.4421629021218,7.57289527720739,14.8008213552361,16.9911019849418,16.3613963039014,15.0390143737166,4.79397672826831,9.78507871321013,17.3607118412047,17.3196440793977,10.2696783025325,11.0499657768652,13.1279945242984,10.4777549623546,15.7864476386037,11.4579055441478,14.5598904859685,13.1964407939767,11.9534565366188,10.5133470225873,16.5585215605749,15.0828199863107,10.5681040383299,11.211498973306,16.1615331964408,8.65982203969884,4.94729637234771,13.5578370978782,11.9479808350445,9.12525667351129,12.2847364818617,13.7467488021903,17.1937029431896,14.2751540041068,11.9835728952772,14.5297741273101,16.8843258042437,15.6194387405886,13.3963039014374,16.6516084873374,17.0677618069815,16.5667351129363,9.4839151266256,14.5407255304586,14.8117727583847,16.4845995893224,15.4223134839151,11.8822724161533,14.4531143052704,6.39561943874059,13.9383983572895,8.65982203969884,12.0793976728268],"y":[314,319,204,405,332,282,358,218,590,130,397,343,190,343,398,289,291,396,331,281,380,225,285,611,430,131,254,160,236,297,423,266,191,168,367,357,583,282,424,669,431,235,366,362,239,103,315,398,92,360,348,216,312,633,285,340,367,226,244,398,280,305,409,270,349,225,412,214,282,294,269,238,434,89,636,198,308,242,241,332,405,520,406,354,195,724,474,195,316,233,259,522,429,209,257,107,374,213,345,278,331,182,383,564,355,309,390,483,500,187,397,233,373,376,201,261,248,390,452,443,270,309,325,203,261,334,352,788,349,537,160,150,394,596,324,140,353,314,421,327,272,193,507,200,422,484,201,385,458,504,361,422,222,260,295,117,320,324,149,324,301,375,361,354,334,334,167,229,424,770,202,137,385,263,521,670,330,187,327,120,355,512,285,320,518,433,282,246,419,331,288,271,237,416,408,161,148,270,320,556,405,501,243,359,380,188,363,389,147,594,148,92,481,533,346,283,276,155,669,461,287,252,366,396,235,280,179,407,506,407,456,366,421,215,427,309,299,368,399,453,429,338,338,184,352,599,413,232,107,474,164,262,322,363,379,115,313,454,336,236,470,303,318,567,239,200,490,240,437,407,408,376,459,260,117,425,419,347,203,370,303,132,294,104,201,246,205,368,479,374,347,508,337,331,365,235,213,154,392,366,322,274,209,414,258,282,270,322,170,335,295,300,300,365,288,262,274,375,332,371,383,340,271,223,123,276,370,444,413,352,340,208,89,461,153,278,730,843,177,379,207,285,441,413,305,403,426,531,221,126,278,402,339,316,329,246,505,439,601,340,273,322,193,265,331,266,391,315,350,345,310,455,259,352,223,201,468,266,286,345,369,175,265,204,264,272,559,291,350,215,175,321,328,172,398,529,481,204,222,223,133,424,495,333,511,528,256,215,469,206,337,419,633,196,259,340,275,312,350,286,320,356,266,400,279,225,402,528,440,350,243,572,123,592,296,169,130,340,305,292,263,524,276,237,437,424,277,493,286,452,328,309,152,506,272,259,226,376,217,711,193,413,296,285,281,411,188,376,374,192,561,260,326,348,430,174,417,525,479,317,323,337,198,227,411,180,690,453,309,190,224,335,159,442,310,200,313,351,437,408,580,504,401,274,431,137,256,309,327,492,348,302,317,694,568,454,398,540,208,652,426,321,573,315,296,523,394,312,311,371,159,290,272,233,335,294,389,343,339,338,400,180,357,165,307,269,243,389,242,157,326,526,355,162,282,159,195,383,328,346,392,295,228,183,341,320,276,464,430,212,442,413,272,243,352,102,564,352,396,340,503,369,288,316,375,471,163,353,317,231,479,627,362,615,287,324,224,289,377,274,216,198,83,363,405,202,375,214,154,221,515,203,163,345,395,452,375,225,212,336,404,210,395,447,433,495,256,309,519,191,367,106,340,352,352,201,255,200,162,292,183,653,296,225,229,769,198,220,407,281,477,292,613,406,397,304,372,212,442,604,672,370,377,409,328,353,409,116,247,399,192,265,200,229,443,262,272,240,370,389,361,484,267,425,160,185,194,518,622,141,425,399,394,121,384,268,282,383,378,301,179,400,261,586,414,341,289,192,450,244,194,430,187,320,354,187,290,336,315,371,249,246,336,387,230,304,477,286,525,336,435,524,359,374,244,430,161,355,291,204,245,249,384,422,256,261,410,107,139,376,267,418,259,323,352,336,279,150,184,312,109,504,355,322,313,205,278,632,244,169,138,447,221,287,364,314,302,305,441,225,413,521,285,226,289,254,310,686,525,282,288,477,339,168,121,179,273,292,113,322,156,494,233,427,219,287,235,362,401,488,368,125,520,405,218,446,314,429,240,486,315,277],"mode":"markers","type":"scatter","marker":{"color":"rgba(31,119,180,0.5)","line":{"color":"rgba(31,119,180,1)"}},"error_y":{"color":"rgba(31,119,180,0.5)"},"error_x":{"color":"rgba(31,119,180,0.5)"},"line":{"color":"rgba(31,119,180,0.5)"},"xaxis":"x","yaxis":"y","frame":null}],"highlight":{"on":"plotly_click","persistent":false,"dynamic":false,"selectize":false,"opacityDim":0.2,"selected":{"opacity":1},"debounce":0},"shinyEvents":["plotly_hover","plotly_click","plotly_selected","plotly_relayout","plotly_brushed","plotly_brushing","plotly_clickannotation","plotly_doubleclick","plotly_deselect","plotly_afterplot"],"base_url":"https://plot.ly"},"evals":[],"jsHooks":[]}</script>

<!--/html_preserve-->

### Operation Time and Bleeding

``` r
cp_spine_tidy %>%
  plot_ly(
    x = ~optime, y = ~bleed_ml_tot, type = "scatter", mode = "markers", alpha = 0.5)  %>% 
  layout(
    xaxis = list(title = "Length of Operation (Minutes)"),
    yaxis = list(title = "Blood Loss (mL)"))
```

<!--html_preserve-->

<div id="htmlwidget-3d82cf228f73ce0e2256" class="plotly html-widget" style="width:90%;height:480px;">

</div>

<script type="application/json" data-for="htmlwidget-3d82cf228f73ce0e2256">{"x":{"visdat":{"2c6225cb7e3e":["function () ","plotlyVisDat"]},"cur_data":"2c6225cb7e3e","attrs":{"2c6225cb7e3e":{"x":{},"y":{},"mode":"markers","alpha":0.5,"alpha_stroke":1,"sizes":[10,100],"spans":[1,20],"type":"scatter"}},"layout":{"margin":{"b":40,"l":60,"t":25,"r":10},"xaxis":{"domain":[0,1],"automargin":true,"title":"Length of Operation (Minutes)"},"yaxis":{"domain":[0,1],"automargin":true,"title":"Blood Loss (mL)"},"hovermode":"closest","showlegend":false},"source":"A","config":{"showSendToCloud":false},"data":[{"x":[319,204,405,332,282,590,130,397,343,190,343,398,291,396,331,281,380,225,285,611,430,131,160,236,423,266,191,367,583,282,669,431,235,366,362,239,315,398,360,216,633,285,340,367,244,280,305,409,270,349,225,214,294,269,238,636,198,308,242,241,332,405,520,406,354,195,724,195,316,233,259,429,257,107,374,213,345,278,331,182,383,564,355,309,390,483,500,187,376,261,390,452,443,270,309,334,352,788,349,537,150,394,596,324,353,314,421,327,272,507,200,422,484,201,385,458,504,361,422,260,295,320,324,149,324,375,361,354,334,334,167,229,424,770,137,263,521,670,330,327,355,512,518,282,419,331,288,271,416,408,161,270,320,556,405,501,243,359,380,188,363,389,594,148,481,533,346,283,276,155,669,287,252,366,235,280,179,407,506,407,456,421,215,427,309,299,368,399,453,338,338,599,413,232,474,164,262,322,363,379,313,454,336,236,470,318,567,490,437,408,376,425,419,347,203,370,132,294,246,205,368,479,374,347,508,337,331,235,154,392,366,322,274,209,258,282,270,322,170,335,295,300,300,262,274,375,332,371,340,271,223,123,276,370,444,352,340,208,461,153,730,843,177,379,207,285,441,413,403,426,531,221,278,402,339,316,329,246,505,439,340,273,322,193,265,331,391,315,345,310,259,352,201,468,286,369,175,265,264,272,559,350,175,481,204,223,424,495,333,511,528,256,215,469,206,337,419,633,259,340,275,312,350,286,320,356,266,400,528,440,350,572,123,592,296,169,130,340,305,292,524,237,437,424,493,286,452,328,309,152,506,259,226,376,217,711,193,413,296,285,281,411,188,376,374,192,561,260,326,430,174,417,525,479,317,337,198,227,411,180,690,453,309,190,224,335,159,442,310,313,351,437,408,580,504,401,274,431,137,256,309,327,492,348,302,317,694,568,454,398,540,208,652,426,321,573,315,296,523,311,371,159,272,233,389,343,400,357,165,269,389,242,326,526,355,159,195,383,328,346,392,295,228,183,341,464,430,413,272,352,564,396,340,503,369,288,316,375,471,163,353,317,231,479,627,362,615,287,324,224,289,377,216,363,405,202,375,214,154,515,203,345,395,452,375,212,336,404,210,395,447,433,495,256,309,519,191,367,352,352,201,255,292,183,653,296,225,769,220,407,281,477,292,613,406,397,304,372,212,442,604,672,370,377,409,328,353,409,116,247,399,192,265,443,272,370,389,484,267,425,160,185,194,518,622,141,399,384,268,282,383,378,301,179,400,586,414,450,430,354,336,315,371,249,246,336,387,304,477,525,336,435,524,359,374,244,430,161,355,291,204,384,422,256,261,410,139,267,418,259,323,352,336,279,150,184,312,355,322,313,205,278,632,244,169,447,221,287,364,314,305,441,413,521,285,226,289,254,310,686,525,282,288,339,168,179,273,292,322,156,494,233,427,219,287,235,401,488,368,405,446,314,429,486,277],"y":[511,884,429,80,593,499,549,750,557,142,385,2778,997,500,992,655,799,610,1205,171,748,338,165,200,1595,500,110,1010,666,273,914,1635,135,525,375,552,100,1972,200,55,520,1137,525,1252,525,1475,102,1200,700,900,125,1564,3720,460,69,947,680,410,350,242,308,332,625,965,30,289,1151,542,159,182,350,743,220,88,335,110,408,110,837,700,287,620,1077,570,1209,469,837,370,345,310,5100,2579,1980,1000,97,776,570,300,1315,602,310,317,1160,828,580,260,375,675,750,1366,220,1500,345,1149,500,613,814,885,1759,850,273,421,200,510,300,900,379,420,120,844,200,562,2731,3862,2098,110,525,2057,920,490,2875,660,476,890,60,350,1100,239,690,564,120,60,325,1309,425,827,550,600,350,836,1031,1434,950,47,452,1316,361,1600,500,550,3198,700,250,915,275,150,849,700,250,1795,501,743,300,590,135,350,1080,871,743,250,1529,565,584,1150,896,559,850,650,430,880,122,200,165,220,720,165,796,326,557,160,440,650,429,150,270,1400,261,990,346,790,948,749,1158,378,125,667,825,300,300,674,125,1004,135,833,1000,125,470,785,350,796,278,632,557,143,232,465,579,1039,447,478,500,332,1044,985,793,245,88,400,466,540,2050,979,70,2200,398,340,310,1269,2034,625,948,304,125,60,913,640,674,350,83,130,1054,130,564,55,55,110,600,300,500,819,1284,108,190,313,275,686,392,400,650,600,787,1659,300,1350,386,519,1815,342,200,362,1315,322,440,635,666,110,694,1471,1931,1269,1350,241,2000,400,300,1234,250,288,1523,1240,705,2327,600,435,300,332,588,543,250,1005,865,462,125,667,171,259,435,869,350,656,875,805,220,900,1118,1667,200,550,502,1269,639,1037,117,562,430,259,1209,440,1046,429,59,750,1400,965,420,497,250,220,560,1031,3286,80,316,126,250,150,490,925,486,117,215,1362,250,516,815,165,380,1574,210,200,626,140,440,400,200,715,1202,487,403,250,750,30,1044,697,1012,345,105,75,980,600,450,45,50,887,1215,1030,101,165,150,426,350,325,197,1112,42,125,100,459,719,1815,516,125,459,300,575,485,4155,1121,635,220,2700,250,831,1331,430,964,3559,969,450,150,50,551,366,137,7215,200,1320,272,357,135,1798,1010,1108,250,612,50,650,1125,375,250,478,800,351,466,375,1011,725,125,550,1441,510,1100,795,600,1031,750,2080,525,450,450,440,55,2047,110,1391,493,458,154,2126,325,295,310,130,1050,140,1200,165,160,230,625,750,645,425,570,886,468,140,800,600,115,100,110,550,1234,1735,135,435,621,250,75,334,330,241,311,875,200,111,1083,1175,294,210,250,946,200,467,1141,320,220,531,491,625,900,1735,320,496,138,510,337,1951,1167,950,698,2530,241,1080,350,1050,185,675,530,73,967,135,40,873,1650,135,1023,93,92,591,495,250,250,108,500,331,676,706,1297,545,426,924,115,460,75,42,684,415,420,1620,260,1050,300,30,474,775,230,530,1036,1346,120,283,125,850,300,95,500,605,849,3025,600,650,83,503,631,300,3222,300,320,115,131,585,726,500],"mode":"markers","type":"scatter","marker":{"color":"rgba(31,119,180,0.5)","line":{"color":"rgba(31,119,180,1)"}},"error_y":{"color":"rgba(31,119,180,0.5)"},"error_x":{"color":"rgba(31,119,180,0.5)"},"line":{"color":"rgba(31,119,180,0.5)"},"xaxis":"x","yaxis":"y","frame":null}],"highlight":{"on":"plotly_click","persistent":false,"dynamic":false,"selectize":false,"opacityDim":0.2,"selected":{"opacity":1},"debounce":0},"shinyEvents":["plotly_hover","plotly_click","plotly_selected","plotly_relayout","plotly_brushed","plotly_brushing","plotly_clickannotation","plotly_doubleclick","plotly_deselect","plotly_afterplot"],"base_url":"https://plot.ly"},"evals":[],"jsHooks":[]}</script>

<!--/html_preserve-->

### Bleeding and Length of Stay

``` r
cp_spine_tidy %>%
  plot_ly(
    x = ~bleed_ml_tot, y = ~tothlos, type = "scatter", mode = "markers", alpha = 0.5)  %>% 
  layout(
    xaxis = list(title = "Blood Loss (mL)"),
    yaxis = list(title = "Length of Stay (Days)"))
```

<!--html_preserve-->

<div id="htmlwidget-034b886876b0ea58255c" class="plotly html-widget" style="width:90%;height:480px;">

</div>

<script type="application/json" data-for="htmlwidget-034b886876b0ea58255c">{"x":{"visdat":{"2c625e9536a2":["function () ","plotlyVisDat"]},"cur_data":"2c625e9536a2","attrs":{"2c625e9536a2":{"x":{},"y":{},"mode":"markers","alpha":0.5,"alpha_stroke":1,"sizes":[10,100],"spans":[1,20],"type":"scatter"}},"layout":{"margin":{"b":40,"l":60,"t":25,"r":10},"xaxis":{"domain":[0,1],"automargin":true,"title":"Blood Loss (mL)"},"yaxis":{"domain":[0,1],"automargin":true,"title":"Length of Stay (Days)"},"hovermode":"closest","showlegend":false},"source":"A","config":{"showSendToCloud":false},"data":[{"x":[511,884,429,80,593,499,549,750,557,142,385,2778,997,500,992,655,799,610,1205,171,748,338,165,200,1595,500,110,1010,666,273,914,1635,135,525,375,100,1972,200,55,520,1137,525,1252,525,1475,102,1200,700,900,125,1564,3720,460,69,947,680,410,350,242,308,332,625,965,30,289,1151,542,159,182,350,743,220,88,335,110,408,110,837,700,287,620,1077,570,1209,469,837,370,345,310,5100,2579,1980,1000,97,776,570,300,1315,602,310,317,1160,828,580,260,375,675,750,1366,220,1500,345,1149,500,613,814,885,1759,850,273,421,200,510,300,900,379,420,120,844,200,562,2731,3862,2098,110,525,2057,920,490,2875,660,476,890,60,350,1100,239,690,564,120,60,325,1309,425,827,550,600,350,836,1031,1434,950,47,452,1316,361,1600,500,550,3198,700,250,275,150,849,700,250,1795,501,743,300,590,135,350,1080,871,743,250,1529,565,584,1150,896,559,850,650,430,880,122,200,165,220,165,796,326,557,160,440,650,429,150,270,1400,261,990,346,790,948,749,1158,378,125,667,825,300,300,674,125,1004,135,833,1000,125,470,785,350,796,278,632,557,143,232,465,579,1039,447,478,500,332,1044,985,793,245,88,400,466,540,2050,979,70,2200,398,340,310,1269,2034,625,948,304,125,60,913,640,674,350,83,130,1054,130,564,55,55,110,600,300,500,819,1284,108,190,313,275,686,392,400,650,600,787,1659,300,1350,386,519,1815,342,200,362,1315,322,440,635,666,110,694,1471,1931,1269,1350,241,2000,400,300,1234,250,288,1523,1240,705,2327,600,435,300,332,588,543,250,1005,865,462,125,667,171,259,435,869,350,656,875,805,220,900,1118,1667,200,550,502,1269,639,1037,117,562,430,259,1209,440,1046,429,59,750,1400,965,420,497,250,220,560,1031,3286,80,316,126,250,150,490,925,486,117,215,1362,250,516,815,165,380,1574,210,200,626,140,440,400,200,715,1202,487,403,250,750,30,1044,697,1012,345,105,75,980,600,450,45,50,887,1215,1030,101,165,150,426,350,325,197,1112,42,125,100,459,719,1815,516,125,459,300,575,485,4155,1121,635,220,2700,250,831,1331,430,964,3559,969,450,150,50,551,366,137,7215,200,1320,272,357,135,1798,1010,1108,250,612,50,650,1125,375,250,478,800,351,466,375,1011,725,125,550,1441,510,1100,795,600,1031,750,2080,525,450,450,440,55,2047,110,1391,493,458,154,2126,325,295,310,130,1050,140,1200,165,160,230,625,750,645,425,570,886,468,140,800,600,115,100,110,550,1234,1735,135,435,621,250,75,334,330,241,311,875,200,111,1083,1175,294,210,250,946,200,467,1141,320,220,531,491,625,900,1735,320,496,138,510,337,1951,1167,950,698,2530,241,1080,350,1050,185,675,530,73,967,135,40,873,1650,135,1023,93,92,591,495,250,250,108,500,331,676,706,1297,545,426,924,115,460,75,42,684,415,420,1620,260,1050,300,30,474,775,230,530,1036,1346,120,283,125,850,300,95,500,605,849,3025,600,650,83,503,631,300,3222,300,320,115,131,585,726,500],"y":[3,12,7,7,6,7,5,12,8,15,4,13,12,4,6,6,7,15,35,8,15,8,5,6,17,3,3,10,12,5,8,6,6,4,6,6,14,30,6,7,6,4,4,4,4,3,16,5,13,6,9,9,6,7,10,7,22,5,5,7,15,0,4,5,5,17,13,4,6,6,4,5,3,7,4,6,3,7,6,7,20,8,15,6,7,6,4,8,7,29,25,7,14,3,20,6,9,4,12,9,4,8,18,12,10,4,5,3,8,6,5,11,3,5,33,10,4,4,5,7,6,8,3,5,12,4,4,8,18,5,8,8,19,0,3,11,8,13,14,6,6,6,12,6,5,7,13,12,44,5,5,7,8,5,6,3,4,5,10,3,8,6,24,3,8,6,20,4,6,12,4,4,6,5,7,28,4,4,8,7,7,5,11,25,5,10,8,5,3,4,13,8,4,4,26,5,5,47,12,7,2,2,11,8,11,5,5,43,3,11,5,8,50,8,12,6,4,4,5,5,3,5,9,4,5,6,11,94,4,5,4,4,16,3,5,4,10,7,4,9,9,23,4,14,6,55,8,3,5,5,5,7,5,6,4,7,13,14,46,49,4,6,6,6,7,8,9,6,6,9,3,9,13,8,3,4,25,3,7,5,4,4,3,7,4,3,2,4,4,3,10,4,7,5,6,3,6,4,8,7,27,3,4,4,17,4,17,11,5,4,8,4,5,6,7,5,26,12,5,9,9,3,5,5,7,8,9,6,8,6,8,5,5,3,8,6,6,4,4,18,6,8,16,4,6,4,6,7,19,5,7,9,6,4,4,4,3,6,5,3,7,5,3,5,4,8,43,8,9,10,6,9,5,8,3,9,6,13,5,4,8,7,3,3,7,7,7,5,6,5,22,6,6,16,7,15,4,4,22,4,6,4,8,7,20,4,5,6,3,9,8,4,7,4,4,3,5,7,5,4,10,4,117,8,5,4,9,3,5,11,9,4,6,3,8,6,10,4,5,11,4,4,4,10,5,8,4,9,5,4,8,7,10,11,15,2,4,3,9,3,4,10,7,4,8,5,6,6,5,11,4,5,4,8,3,2,3,8,4,3,30,3,12,5,6,9,4,6,6,4,5,5,8,6,4,7,7,4,40,9,3,8,4,5,5,12,4,5,5,7,18,6,9,6,6,2,6,10,5,4,3,7,6,4,11,11,4,8,3,6,7,12,6,4,6,1,6,5,4,6,4,7,5,11,6,5,4,18,17,11,3,8,8,8,3,4,13,7,5,4,4,5,0,9,4,6,9,5,10,12,9,7,8,7,6,4,4,4,9,6,4,4,7,6,6,7,6,7,9,8,9,4,6,10,8,5,4,5,32,16,6,4,5,7,7,4,13,5,11,4,5,3,5,8,8,12,37,13,12,4,3,7,3,5,3,5,8,8,17,3,4,5,4,3,10,8,6,5,7,3,0,6],"mode":"markers","type":"scatter","marker":{"color":"rgba(31,119,180,0.5)","line":{"color":"rgba(31,119,180,1)"}},"error_y":{"color":"rgba(31,119,180,0.5)"},"error_x":{"color":"rgba(31,119,180,0.5)"},"line":{"color":"rgba(31,119,180,0.5)"},"xaxis":"x","yaxis":"y","frame":null}],"highlight":{"on":"plotly_click","persistent":false,"dynamic":false,"selectize":false,"opacityDim":0.2,"selected":{"opacity":1},"debounce":0},"shinyEvents":["plotly_hover","plotly_click","plotly_selected","plotly_relayout","plotly_brushed","plotly_brushing","plotly_clickannotation","plotly_doubleclick","plotly_deselect","plotly_afterplot"],"base_url":"https://plot.ly"},"evals":[],"jsHooks":[]}</script>

<!--/html_preserve-->

There were no clear relationships between single predictors and
outcomes, which lead us to attempt to build a model that incorporates
multiple predictors for our outcome of interest.

<h1 id="model">

Modeling Discharge Status

</h1>

## Full Model

## Reduced Model

## Comparison of Residuals, Goodness of Fit

<h1 id="discuss">

Discussion

</h1>

## Major Findings

## Future Directions

<h1 id="refs">

Referenes

</h1>
