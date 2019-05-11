# Is It Possible to Predict a Restaurant Inspection Grade?



## Project Overview


For food service establishment owners and staff, chances are, you are well acquainted with restaurant inspections especially since all restaurants in NYC are required to post their restaurant inspection grade for the public since 2010. Inspectors from the Department of Health and Mental Hygiene conduct surprise restaurant inspections throughout each year, which usually causes panic in owners and last minute scrambles to prepare for their arrival. All owners seek to receive a restaurant inspection grade of A, which is the best. Grades below an A (ie. B, C, and so on) can cause a decrease in the customer base.

For this project, I was interested in finding the factors that can be used to determine if a restaurant would receive an A during their inspection. Based on research, the factors that would contribute to receiving a particular grade are the type of violations (ie. public hazard violation, critical violation, and general violation) and the points calculated from these violations leading to a score. I thought it would also be fascinating to analyze if there are relationships between: restaurant location vs. grade, cuisine vs. grade, restaurant size vs. grade, and when inspection is conducted vs. grade.

<b><u> Questions </u>:</b>
* Do the following factors affect the restaurant inspection grade?
    * Location (Borough, Zip Code, Community District)
    * Type of Violations
    * Number of Violations (Critical, Non-Critical)
    * Cuisine
    * Restaurant Size

<b><u> Target Variable </u>:</b>

The target variable for this project was restaurant inspection grade. A grade of A was expressed as a 1 and any grade other than A was expressed as a 0 in this project. Restaurant inspection score data was also available for this project and could be used as the target variable instead. However, in this case I decided to approach it as a binary classification problem instead of regression problem since the restaurant inspection grade information was available.


## Restaurant Inspection Information


Restaurants have two chances within an inspection cycle to receive an A. If a restaurant does not receive an A during the first inspection, it is revisited 1 month afterwards for a re-inspection. Restaurants are labeled as "Not Yet Graded" if a grade has not been provided yet.

Points are given depending on the violation commited and the final score is calculated by totaling all of the points. Each violation has a different weight depending on the severity of the violation.

In general, the following are the breakdown of grade and points:

   * A: 0-13 points
   * B: 14-27 points
   * C: 28 or more points

There are situations in which only a score is given without a grade. This is perhaps one of the reasons for the null values found in the dataset. More details on how these were handled in the next paragraph. More information on the NYC Department of Health and Mental Hygiene's process for evaluating restaurants can be found online at: <a href=https://www1.nyc.gov/assets/doh/downloads/pdf/rii/how-we-score-grade.pdf>here</a>. The link is also listed in the Data Sources section at the end of the page.


## Data Collection and Analysis


The NYC Restaurant Inspections Dataset was the primary data source for this project with 383,300 data points, last updated May 6th, 2019. Each row represents a citation. Additional information from the dataset on Sidewalk Cafes Licenses and Applications was later added, which had 1551 data points, last updated May 3rd, 2019. Both were obtained from NYC Open Data's website. 


### Null Value Handling

One of the challenging items of this project was handling the null values in the datasets. 

Null values in the NYC Restaurant Inspections dataset were as follows:

<p align="center">
    <img src='./images/inspections_missing1.png' title="Missing Values in NYC Restaurant Inspections Dataset">
</p>

   * Key issue: Grade, which is the target variable, contains a large number of null values.
   * The following steps were taken to clean up this situation:
       * Any rows that had null values in both the Scores and Grade columns were removed.
       * Remaining rows with null values in Grade columns were closely examined and the following can be noted:
           * Scores were given for each citation but ultimately a grade was not given. According to documentation from the Department of Health (DOHMH), there are times when scores may be given but not grade. One restaurant can have more than one citation (more than one row).
           * I grouped and aggregated together the data based on restaurant information, and inspection date to view the null values. Most were due to a high score (>14 points) during their initial inspection, which can lead to no grade being given at the time and for the DOHMH to return for a re-inspection.
           * For this reason, since it is unclear if the restaurants have been reinspected since this occurence, I followed the DOHMH's guideline to mark the null values as Not Yet Graded. A note is that this may cause a class imbalance but this slowly shifts along the rest of the process.
           * Some of the null values were due to listings of new restaurants that have not completed inspections yet. These were all removed as all other columns also did not contain any useful information.
   * A couple of the columns with null values were ultimately dropped: BUILDING, STREET, PHONE, ACTION, VIOLATION DESCRIPTION, GRADE DATE, RECORD DATE, INSPECTION TYPE. 
   * Because ZIPCODE is a feature I was interested in, citations that had missing zip codes were ultimately dropped. 
   * After reviewing the context around the null values in VIOLATION CODE, they were filled with None as the ACTION column stated that no violations were noted during those inspections leading to the null values. 
   * Most of the null values were able to be resolved by creating dummy variables for a couple of the columns (ie. Inspection Month, Inspection Year) and then grouping the rest of the dataframe restaurant information.

Null values in the Sidewalk Cafe Licenses and Applications dataset were as follows:

<p align="center">
    <img src='./images/cafe_missing1.png' title="Missing Values in Sidewalk Cafe Licenses and Applications Dataset">
</p>

   * All columns with null values except for ISSUANCE_DD (Date License was Issued) and SWC_SQ_FT (Cafe Size in Sq. Ft) were dropped. 
   * Data points with no information on when their licenses were issued were also dropped, as most of them were indicated to have Applications Pending. 
   * In addition, because I was interested in cafe size as a parameter, I removed any data points which were missing this information. 

### Data Wrangling and Analysis

The project was approached in ways. It was initially approached using only the NYC Restaurant Inspections dataset to see if:

   * Specific violations (each violation indicated by its code) are important factors for predicting the grade.
   * Numbers of critical and non-critical violations (indicated by Critical Flags) are important factors for predicting the grade.
   * Type of cuisine is a predictor. I thought this would be a fun point to check if there is a relationship and if yes, why is this the case.
   * Restaurant location by borough and zip code has an affect on the grade it obtains. This would be interesting because some inspectors evaluate restaurants in the same area and seeing if there are usually a cluster of restaurants with As in an area may encourage potential owners to locate their restaurants there.

Then I added the Sidewalk Cafe Licenses and Applications dataset and added the following features:

   * Duration the business has been open for (calculated by subtracting the license issuance date from 5/9/19)
   * Restaurant size (sq. ft)

All specific violation dummy variables were removed instead as it would be difficult to interpret how each violation affects the grade since the number of points for each violation is unknown (I was unable to find the rubric).

#### Outliers Handling

Further data cleaning was completed during this step during outlier evaluation. 

<u>NYC Restaurant Inspections Dataset</u>:

A boxplot of inspection scores showed positive skewness with outliers after 28 points. All data points with scores greater than 28 points were removed as any scores in that range are prone to shutdown. After removing these outliers, the distribution of the scores was more normal. A boxplot of inspection years showed outliers as any years below 2016. Since most recent years would be more relevant for prediction, data points from inspection years older than 2016 were also removed. 

<u>Sidewalk Cafe Licenses and Applications Dataset Merged with NYC Restaurant Inspections Dataset</u>:

A couple of outliers were also detected in scores, tables, chairs, and cafe size (sq ft) using boxplots. After reviewing the target value counts before/after the outliers were removed, some of these outliers were removed with the goal of also normalizing the class imbalance in the dataset. More caution was placed in handling these outliers as this subset of data was small - about 381 points. The tables and chairs columns were ultimately dropped as they were highly correlated with cafe size (sq ft) after reviewing the correlation matrix (0.797, 0.896 respectively).

#### Data Analysis

To begin, I took a look at the pairwise correlation matrix chart to see if there are any pair of features that are strongly correlated. 

<u>NYC Restaurant Inspections Dataset</u>:

Here is a heatmap showing the correlation matrix for a subset of features:

<p align="center">
    <img src='./images/corr_heatmap.png' title="Correlation Heatmap of Key Features">
</p>

The following was noted:

   * Strongly correlated:
       * Critical Violations x Score: 0.82
        
   * Moderately correlated:
       * Score x Grade: -0.601886
       * Violation__22E x Violation__06G: 0.664572
       * Violation__08A x Violation__04L: 0.6537
        
<u>Sidewalk Cafe Licenses and Applications Dataset Merged with NYC Restaurant Inspections Dataset</u>:

Here is a heatmap showing the correlation matrix for a subset of features:

<p align="center">
    <img src='./images/cafe_corr_heatmap.png' title="Correlation Heatmap of Key Features">
</p>

The following was noted:

   * Strongly correlated:
       * Manhattan x Community District: -0.96
       * Tables x Chairs: 0.94
       * Manhattan x City Council District: -0.93
       * Community District x City Council District: 0.88
       * Tables x Sq Ft: 0.88
       * Chairs x Sq Ft: 0.83
       * Brooklyn x City Council District: 0.85
       * Critical Violations x Non Critical Violations: 0.74
       * Queens x Community District: 0.73
       * Brooklyn x Manhattan: -0.72
        
   * Moderately correlated:
       * Score x Critical Violations: 0.59
       * Brooklyn x Community District: 0.58
       * Queens x Manhattan: -0.54




## Feature Engineering and Selection


## Machine Learning Models

## Conclusion & Next Steps

## Data Sources


- <b> Brief Information Sheet On How the NYC Health Department Scores and Grades Restaurants</b>:
    <a href=https://www1.nyc.gov/assets/doh/downloads/pdf/rii/how-we-score-grade.pdf>https://www1.nyc.gov/assets/doh/downloads/pdf/rii/how-we-score-grade.pdf</a>
    
- <b> NYC Restaurant Inspections Data </b>:
    <a href=https://data.cityofnewyork.us/Health/DOHMH-New-York-City-Restaurant-Inspection-Results/43nn-pn8j>https://data.cityofnewyork.us/Health/DOHMH-New-York-City-Restaurant-Inspection-Results/43nn-pn8j</a>

- <b> Sidewalk Cafe Licenses and Applications </b>:
    <a href=https://data.cityofnewyork.us/Business/Sidewalk-Caf-Licenses-and-Applications/qcdj-rwhu>https://data.cityofnewyork.us/Business/Sidewalk-Caf-Licenses-and-Applications/qcdj-rwhu</a>
