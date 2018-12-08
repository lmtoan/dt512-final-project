# Analysis of NYPD Shooting Incidents 2013-2018

By Toan Luong. Last updated 11/21/2018.

As part of the final project plan assignment of [UW DT512 class](https://wiki.communitydata.cc/Human_Centered_Data_Science_(Fall_2018))

Motivations
---
As a foreign student living in the U.S. for less than 5 years, I have a limited understanding of the complex relationship between law enforcement officers and the public they are tasked to protect. Never had a serious interaction with the police, I am shocked by the news of Michael Brown's death that led to the unrest in Ferguson and fascinated by the formation of the Black Lives Matter movement. For the final project, I want to study the case of police brutality, how it varies among demographic groups, and how its extent changes over time. The project will equip me with more background knowledge about a powerful social change campaign and provide a human-centered application to the data science analyses.

With the data sourced from NYC Open Data initiative, the project focuses on police shooting incidents that occured in New York City from 2013 to 2018. NYPD has a history of misconduct and discrimination against minorities, where "over 12,000 such cases have resulted in lawsuit settlements totalling over $400 million during a five-year period ending in 2014"<sup>[2](https://en.wikipedia.org/wiki/New_York_City_Police_Department_corruption_and_misconduct)</sup>. In the contrary, several reports state that NYPD "is the most restrained in the country"<sup>[3](https://www.nytimes.com/2008/05/08/nyregion/08nypd.html)</sup> and its officers "have shot fewer people than ever in 2017", only 18 people, compared those of Los Angeles (at 42) and Chicago (at 65) in the same year<sup>[4](https://www.newsweek.com/nypd-officers-shot-lowest-number-people-ever-2017-758205)</sup>. These contradicting reports suggest additional cautions on my part when analyzing the datasets maintained and provided by the NYPD itself. I am curious to investigate the most recent police shooting incident statistics to find out which narrative holds true.

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


Research Questions
---

### Police shooting trends from 2013-2018

*Questions*
- Have there been more or less police shootings in NYC after Trump's election in November 2016?
- Are there any correlation between notable police protests and the number of police shootings?

*Hypotheses*
- After Trump's election, there have been controversial calls from the administration for more "law and order" and heavy policing state. My inkling is that there will be
more police shootings because the police might become more hostile with traditionally suppressed communities.
- Over the past 5 years, NYPD has been responsible for several wrongful deaths, most notably of Eric Garner (7/17/2014) and of Akai Gurley (11/20/2014). These deaths are often followed by series of police protests. I want to explore if the public resentment toward excessive policing might result in more hostility from the police forces? An increasing number of police shooting incidents before and after key protest dates might suggest some correlation. I plan to do additional research to retrieve the key protest dates.

*Deliverables & Use Cases*
- The main deliverables for this section will be a statistical analysis to compare the average number of police shootings, before and after a certain key date. Similar to A/B testing techniques, a simple average calculation and t-test might do the job.
- The analysis might be useful to journalists and human rights organizations to further their calls to protect at-risk communities against excessive policing. Any statistically significant result might motivate NYPD to revise their operations and training regimes to properly protect its residents despite the recent divisive political climate.


### Police shooting locations visualization

*Questions*
- Are police shooting incidents concentrated in any geographical area in NYC? 

*Hypotheses*
- I would like to verify the common perception that there are more police shootings in areas in NYC that have low median incomes and high crime rates.

*Deliverables & Use Cases*
- The main deliverable is a geographical map plot (given the longtitude and latitude data) to show the distribution of police shooting incidents in NYC. From that, I want to select the top 10 areas with the highest number of police shootings.
- For each area, I want to map the area's demographics and other metadata to gain a better understanding of why there is a high concentration of police shooting incidents.
- The visualization and additional demographics analysis might alert journalists, NYPD, and lawmakers for any potential discriminations.

### Correlations between demographic factors of perpetrators and victims

*Questions*
- Are there any trends in gender, race, age group of perpetrators and victims?
- Do those trends change over the span of 5 years and observe any significant shifts after significant events?
- How do the missing values affect the integrity of demographic analyses?

*Hypothesis*
- In recent news, minorities, especially those of Black and Hispanic groups, tend to be under threats of wrongful police shootings, even if they commit a crime or not. I am curious if there are variations in the number of police shootings among males/females and different age groups.
- The datasets provide demographic information of both perpetrators and victims. However, many rows have missing values of perpetrators or victims demographics. I would like to study how NYPD treats these missing values in their reports and identify any potential biases.

*Deliverables & Use Cases*
- The deliverable is a group-by count of the number of police shootings associated with each demographic group. I also want to perform a time-trend analysis on these statistics. Any signifcant shift can signal journalists, NYPD, and law-makers about their operations and identify any potential discrimination toward a demographic group.

Human-Centered Design Considerations
---

I would like to acknowledge several personal biases that might interfere with the formation of my research questions, treatment of the datasets, and subsequent data analyses.
- As a foreigner with certain privileges, I never had an interaction with the police (except parking enforcement officers) and does not belong to demographic groups that typically suffer from police brutality. As the result, my analyses might not account for the complexities of the relationship between NYPD and its residents. Any calculations or speculations about NYPD operations or behaviors might be stated with limited knowledge and without first-hand experience.
- I often find myself in constant news influx of wrongful police shootings and police brutality. As a liberal-leaning person, I am more likely to seek out negative evidences against NYPD operations in my statistical analyses.
- Any statistical results from the NYC Open Data datasets are only applied to New York City. Such results are not representative of the United States population and their relationships with law enforcement forces. As an ethical data scientist, I need to warn news outlets and political organizations not to extrapolate these results about police shootings, to their own states and cities.

Tools
---

I plan to use several Python packages to aid my data wrangling, statistical analyses, and geographical visualization. All packages are open-sourced and free for use.

Analysis
- Python 3.6
- Pandas/Numpy
- Jupyter Notebook

Visualization
- Matplotlib
- Folium

Sources
---
1. https://opendata.cityofnewyork.us/
2. https://en.wikipedia.org/wiki/New_York_City_Police_Department_corruption_and_misconduct
3. https://www.nytimes.com/2008/05/08/nyregion/08nypd.html
4. https://www.newsweek.com/nypd-officers-shot-lowest-number-people-ever-2017-758205