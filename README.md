# National-Nationwide-Inpatient-Sample-NIS-

A statistical analysis on the NIS dataset https://www.hcup-us.ahrq.gov/db/nation/nis/nisdbdocumentation.jsp 


- Is (Anterior STEMI) associated with Complete Heart Block (CHB)?

The study population was derived from hospital discharges (18,013,878) Patients.

The table below summaries the attributes description and data types that have been used in this study. We have four categorical attributes: 
Female, race, AnteriorSTEMI, chb.
Also, we have three numerical attributes: AGE, LOS and TOTCHG.

<img width="402" alt="Screen Shot 2023-01-26 at 7 37 00 PM" src="https://user-images.githubusercontent.com/121846325/214981258-12557b0c-c4df-481a-a4f2-f6f30801ec69.png">

Part 1 - EDA:

STATA code:
. use "/Desktop/NIS_STATA copy/NIS_2014_Core_above18.dta" 
. summarize AGE if AnteriorSTEMI == 1
. graph box AGE, over(AnteriorSTEMI)
. summarize AGE if chb == 1
. graph box AGE, over(chb)

Output plots:

<img width="573" alt="Screen Shot 2023-01-26 at 7 39 45 PM" src="https://user-images.githubusercontent.com/121846325/214981557-fd406f38-eba9-4d56-8a8b-ed0b17f08bc8.png">

<img width="571" alt="Screen Shot 2023-01-26 at 7 40 04 PM" src="https://user-images.githubusercontent.com/121846325/214981598-e4f76570-f61b-4fa4-98e9-4d32ba106060.png">

Part 2 - Statistical research questions:

 Q1) To find the association between the CHB and Anterior STEMI using the NIS dataset for year 2014?

Test: Chi-square analysis between (CHB and AnteriorSTEMI)
STATA CODE: . tabulate AnteriorSTEMI, chb,chi2

<img width="606" alt="Screen Shot 2023-01-26 at 7 53 51 PM" src="https://user-images.githubusercontent.com/121846325/214983141-8a72e0e1-5ccf-4e08-be98-73de36b27237.png">

p-value = 0.000. Since this is less than 0.05, we conclude that we have sufficient evidence to conclude that there is a statistically significant association between the development of Anterior STEMI and CHB.

Q2) It is reported that women usually have a lower incidence of cardiac disease than men. Are women more likely to develop CHB than men?

Test: Two-sample t-test with equal variance 
STATA code: .ttest FEMALE if chb == 1

<img width="541" alt="Screen Shot 2023-01-26 at 8 03 45 PM" src="https://user-images.githubusercontent.com/121846325/214984263-5b73996b-3c8d-4fdb-abe1-24f9c3f38619.png">

The output shows that Number of CHB patients in women is different than in men with p- value = 0.000.

Q3) It is now well documented that there are profound race-associated among those who are affected by and die from cardiovascular disease (CVD) [2]. Is patient race associated with CHB?

Test: One-way ANOVA
STATA code: . oneway chb RACE, tabulate

<img width="612" alt="Screen Shot 2023-01-26 at 8 05 30 PM" src="https://user-images.githubusercontent.com/121846325/214984439-8c271add-e04b-4c00-bb08-7d4441882286.png">

there is a statistically significant difference in the mean chb patients between the six different groups of the independent variable (i.e., “White", "Black “, "Hispanic", “Non-Hispanic”, “Asian” and “other”). To know which groups exactly different we use the Pairwise comparisons of means with equal variances output that contains the results of our post hoc tests.

Test: Pairwise comparisons result for the Tukey post hoc test
STATA code: . pwmean chb, over(RACE) mcompare(tukey) 

<img width="418" alt="Screen Shot 2023-01-26 at 8 06 25 PM" src="https://user-images.githubusercontent.com/121846325/214984548-caacaf92-7804-45dc-9736-96a55d0b6d76.png">


Q4) Predict the length of stay in the hospital for patients diagnosed with Anterior STEMI according to their age?

Test: Linear Regression
STATA code: . regress LOS age_chb



<img width="497" alt="Screen Shot 2023-01-26 at 8 13 20 PM" src="https://user-images.githubusercontent.com/121846325/214986011-c1b6ccc5-f368-403a-a717-105eb56772c7.png">

From this test, we can write the linear regression equation as:
 LengthOfStay = 13.05683 - .0866031*age
 at age = 60, Y = 13.05683 - .0866031* 60 = 7.86 days
 
 ----- End of Study ----
