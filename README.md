# Explore Bias in the NYC Shooting Incidents Dataset by NYPD

Unless otherwise specified, any materials in this repository are released under the [MIT License](LICENSE).

Prepared by Toan Luong on December 7, 2018 as part of [UW DT512: Human Centered Data Science](https://wiki.communitydata.cc/Human_Centered_Data_Science_(Fall_2018)) Final Project.

Abstract
---

(motivations + research questions)
(results)
(recommendations - same as Conclusion)

Human-Centered Design Considerations
---
I would like to acknowledge several personal biases that might interfere with the formation of my research questions, treatment of the datasets, and subsequent data analyses.
- As a foreigner with certain privileges, I never had an interaction with the police (except parking enforcement officers) and does not belong to demographic groups that typically suffer from police brutality. As the result, my analyses might not account for the complexities of the relationship between NYPD and its residents. Any calculations or speculations about NYPD operations or behaviors might be stated with limited knowledge and without first-hand experience.
- I often find myself in constant news influx of wrongful police shootings and police brutality. As a liberal-leaning person, I am more likely to seek out negative evidences against NYPD operations in my statistical analyses.
- Any statistical results from the NYC Open Data datasets are only applied to New York City. Such results are not representative of the United States population and their relationships with law enforcement forces. As an ethical data scientist, I need to warn news outlets and political organizations not to extrapolate these results about police shootings, to their own states and cities.

Software Requirements
---
All packages are open-sourced and free for use.

Analysis
- Python 3.6
- Pandas/Numpy
- Jupyter Notebook

Visualization
- Matplotlib
- Folium

Usage Instructions
---
(reproducibility session)

Data
---
### Data Description

I will be using a combination of historic and year-to-date data that capture every shooting incident occured in New York City from 2013 to 2018. Their links are referenced below:
- [NYPD Shooting Incident Data *(Historic)*](https://data.cityofnewyork.us/Public-Safety/NYPD-Shooting-Incident-Data-Historic-/833y-fsy8). Created on June 5, 2018. Last updated on November 8, 2018.
- [NYPD Shooting Incident Data *(Year To Date)*](https://data.cityofnewyork.us/Public-Safety/NYPD-Shooting-Incident-Data-Year-To-Date-/5ucz-vwe8). Created on June 5, 2018. Last updated on November 8, 2018.
- These datasets share the same [data footnote](https://data.cityofnewyork.us/api/views/833y-fsy8/files/e4e3d86c-348f-4a16-a17f-19480c089429?download=true&filename=NYPD_Shootings_Incident_Level_Data_Footnotes.pdf) which details variables creation methods, exceptions, filtering guidelines, and addresses any data abnormalies and inconsistencies.

Reviewed by Office of Management Analysis and Planning and provided by New York Police Department, the data provide a comprehensive view about "the shooting event, the location and time of occurrence, and information related to suspect and victim demographics". For the *Historic* dataset, there are 6,407 rows and 18 columns. For the *Year-to-date* dataset, there are 441 rows of similar 18 columns. I plan to merge these 2 datasets together since they have similar columns. These datasets are viewed more than 460 times and downloaded 75 times.

Several limitations and potential biases are discussed in the following sections.

### License & Terms of Use

The datasets are hosted on [NYC Open Data](https://opendata.cityofnewyork.us/), collected, and maintained by the New York City government. The following excerpt is from their [About](http://www.nyc.gov/html/data/about.html) page, which demonstrates the agencies' commitment to "ensure transparency and foster civic innovation within the City". The NYC Open Data initiative aims to "improve the quality of life" for all its residents. 

> NYC Open Data makes the wealth of public data generated by various New York City agencies and other City organizations available for public use. As part of an initiative to improve the accessibility, transparency, and accountability of City government, this catalog offers access to a repository of government-produced, machine-readable data sets. 

> Anyone can use these data sets to participate in and improve government by conducting research and analysis or creating applications, thereby gaining a better understanding of the services provided by City agencies and improving the lives of citizens and the way in which government serves them.

The rights to use the data for public research are mentioned in the umbrella [Terms of Use](https://opendata.cityofnewyork.us/overview/#termsofuse) and quoted below. There are no disclaimers or copyright exceptions to these datasets.

> By accessing datasets and feeds available through NYC Open Data, the user agrees to all of the [Terms of Use](http://www1.nyc.gov/home/terms-of-use.page) of NYC.gov as well as the [Privacy Policy](http://www1.nyc.gov/home/privacy-policy.page)  for NYC.gov. The user also agrees to any additional terms of use defined by the agencies, bureaus, and offices providing data. Public data sets made available on NYC Open Data are provided for informational  purposes. The City does not warranty the completeness, accuracy,  content, or fitness for any particular purpose or use of any public data  set made available on NYC Open Data, nor are any such warranties to be implied or inferred with respect to the public data sets furnished  therein.
>
> The City is not liable for any deficiencies in the completeness,  accuracy, content, or fitness for any particular purpose or use of any  public data set, or application utilizing such data set, provided by any third party.
>
> Submitting City Agencies are the authoritative source of data  available on NYC Open Data. These entities are responsible for data  quality and retain version control of data sets and feeds accessed on the Site. Data may be updated, corrected, or refreshed at any time.

### Limitations & Biases

As stated in the dataset headers, the NYPD shooting incident data "is manually extracted every quarter and reviewed by the Office of Management Analysis and Planning before being posted on the NYPD website". Such scrutiny might result in excessive filtering of shooting incidents or metadata related to such incidents. Below is a quote from the data footnote that indicates the exclusion of non-injured police shootings. Omitting the non-injured shootings or gunpoint threats might under-estimate the impacts of NYPD historic patterns of excessive policing minorities. 

> Only valid shooting incidents resulting in an injured victim are included in this release. Shooting incidents not resulting in an injured victim are classified according to the appropriate offense according to NYS Penal Law. 

There is no description of data de-identification process to protect individuals' privacies. Below is a one-line instruction quoted from the data footnote, which should have been a standardized process given GDPR regulations.

> Any attempt to match the approximate location of the incident to an exact address or link to other datasets is not recommended.

The authoring agencies state that the *"data can be used by the public to explore the nature of shooting/criminal activity"*. Such description explicitly links police shootings with criminal activities. Because of such implicit biases, the datasets might not account for wrongful police shootings and other verbal and physical threats against minorities and communities at harm.

### Schema

| ﻿Column Name             | Column Description                                                                                                                                                                    | Type       |
|-------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------|
| INCIDENT_KEY            | Randomly generated persistent ID for each incident                                                                                                                                    | Plain Text |
| OCCUR_DATE              | Exact date of the shooting incident                                                                                                                                                   | Date Time  |
| OCCUR_TIME              | Exact time of the shooting incident                                                                                                                                                   | Plain Text |
| BORO                    | Borough where the shooting incident occurred                                                                                                                                          | Plain Text |
| PRECINCT                | Precinct where the shooting incident occurred                                                                                                                                         | Number     |
| JURISDICTION_CODE       | Jurisdiction where the shooting incident occurred. Jurisdiction codes 0 (Patrol), 1 (Transit) and 2 (Housing) represent NYPD whilst codes 3 and more represent non NYPD jurisdictions | Number     |
| LOCATION_DESC           | Location of the shooting incident                                                                                                                                                     | Plain Text |
| STATISTICAL_MURDER_FLAG | Shooting resulted in the victim’s death which would be counted as a murder                                                                                                            | Check Box  |
| PERP_AGE_GROUP          | Perpetrator’s age within a category                                                                                                                                                   | Plain Text |
| PERP_SEX                | Perpetrator’s sex description                                                                                                                                                         | Plain Text |
| PERP_RACE               | Perpetrator’s race description                                                                                                                                                        | Plain Text |
| VIC_AGE_GROUP           | Victim’s age within a category                                                                                                                                                        | Plain Text |
| VIC_SEX                 | Victim’s sex description                                                                                                                                                              | Plain Text |
| VIC_RACE                | Victim’s race description                                                                                                                                                             | Plain Text |
| X_COORD_CD              | Midblock X-coordinate for New York State Plane Coordinate System, Long Island Zone, NAD 83, units feet (FIPS 3104)                                                                    | Number     |
| Y_COORD_CD              | Midblock Y-coordinate for New York State Plane Coordinate System, Long Island Zone, NAD 83, units feet (FIPS 3104)                                                                    | Number     |


Sources
---
1. https://www1.nyc.gov/site/nypd/stats/crime-statistics/crime-statistics-landing.page
2. https://www.nytimes.com/2017/12/27/nyregion/new-york-city-crime-2017.html
3. https://www.nytimes.com/2015/12/30/nyregion/bratton-rebukes-kelly-for-questioning-new-york-crime-data-shame-on-him.html
4. https://wiki.communitydata.cc/upload/7/76/HCDS_2018_week_4_slides.pdf

4. https://opendata.cityofnewyork.us/
2. https://en.wikipedia.org/wiki/New_York_City_Police_Department_corruption_and_misconduct
3. https://www.nytimes.com/2008/05/08/nyregion/08nypd.html
4. https://www.newsweek.com/nypd-officers-shot-lowest-number-people-ever-2017-758205