# Is It Possible to Predict a Restaurant Inspection Grade?



## Project Overview


For food service establishment owners and staff, chances are, you are well acquainted with restaurant inspections. In New York City, this is probably also the case for restaurant-goers since all restaurants are required to post their restaurant inspection grade for the public since 2010. Inspectors from the Department of Health and Mental Hygiene conduct surprise restaurant inspections throughout each year, which usually leads to panic in owners and last minute scrambles to prepare for their arrival. All owners seek to receive a restaurant inspection grade of A, which is the best. Grades below an A (ie. B, C, and so on) can cause a decrease in the customer base.

For this project, I was interested in finding the potential factors that can be used to determine if a restaurant would receive an A during their inspection. Based on research, the factors that would contribute to receiving a particular grade are the type of violations (ie. public hazard violation, critical violation, and general violation) and the points calculated from these violations leading to a score. I thought it would also be fascinating to analyze if there is are relationships between: restaurant location vs. grade, cuisine vs. grade, restaurant size vs. grade, when inspection is conducted vs. grade.

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


Restaurants have 2 chances within an inspection cycle to receive an A. If a restaurant does not receive an A during the first inspection, it is revisited 1 month afterwards for a re-inspection. Restaurants are labeled as "Not Yet Graded" if a grade has not been provided yet.

Points are given depending on the violation commited and the final score is calculated by totaling all of the points. Each violation has a different weight depending on the severity of the violation.

In general, the following are the grade and points breakdown:

* A: 0-13 points
* B: 14-27 points
* C: 28 or more points

There are situations in which only a score is given without a grade. As mentioned in the next section, this is perhaps one of the reasons for the null values found in the dataset. More details on how these were handled in the next paragraph. More information on the NYC Department of Health and Mental Hygiene's process for evaluating restaurants can be found online at: <a href=https://www1.nyc.gov/assets/doh/downloads/pdf/rii/how-we-score-grade.pdf>https://www1.nyc.gov/assets/doh/downloads/pdf/rii/how-we-score-grade.pdf</a>. This is also noted in the Data Sources section at the end of the page.


## Data Collection and Analysis


The NYC Restaurant Inspections Dataset was the primary data source for this project. Additional information from the dataset on Sidewalk Cafes Licenses and Applications was also used. Both were obtained from NYC Open Data's website. 


## Feature Engineering and Selection


## Machine Learning Models

## Conclusion & Next Steps

## Data Sources

- <b> Brief Information Sheet On How the NYC Health Department Scores and Grades Restaurants</b>:
    <a href=https://www1.nyc.gov/assets/doh/downloads/pdf/rii/how-we-score-grade.pdf>https://www1.nyc.gov/assets/doh/downloads/pdf/rii/how-we-score-grade.pdf</a>
