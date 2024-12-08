A housing appraisal is an unbiased professional opinion of the value of a home. This value can influence loan options and can be used in the determination of a home’s property taxes. As it stands, many homeowners claim that the appraisal process is flawed and subject to bias and manipulation (cite article). For example, appraisal values can differ based upon homeowner race. This was the experience of an interacial Florida couple whose home was valued 40% higher after removing images of their Black family members (cite article). Additionally appraisal values can be altered by tens of thousands of dollars if a homeowner can provide comparable properties (which differ from those used by the appraisal district to access the home's value) that support his or her opinion of what the property is worth.
Since appraisal values have immense consequences for homeowners, it is important that these values be fair and objective. Home sales could be cancelled if the appraisal value of a home is lower than expected (as the bank could deny the potential homebuyer a loan). In many areas, if the assessed property value increases significantly, property tax would increase accordingly, meaning that homeowners on limited budgets could potentially lose their homes to foreclosure.
We are interested in understanding the factors that most affect home appraisal values. More specifically, our project seeks to answer the following question:
What factors influence the appraisal value of single family dwellings in San Francisco, California?
By answering this question, we hope to provide San Francisco homeowners with an additional resource that can be used to determine the accuracy of their home appraisal value and contest this value if need be.

The dataset that was used in this project was the were interested in prop. The Office of the Assessor-Recorder provides this publicly available property tax data for the City and County of San Francisco. The data spans from 2009 to 2019 and can be obtained in .csv format from https://data.sfgov.org/Housing-and-Buildings/Assessor-Historical-Secured-Property-Tax-Rolls/wv5m-vpq2. The raw data file (over 400 MB in size) contains 2.03 million rows and 43 columns or fields, with each row representing a property. The fields that we are interested in for this study are:
Screen Shot 2021-02-09 at 10.28.34 AM.png
Data Cleaning
Because the original dataset was quite large and could not be uploaded in Jupyter to conduct analysis, we decided to focus our study on only single-unit single-family residential dwellings. We dropped fields that we were not interested in exploring in our analysis and cleaned this subset of data as described below. The final data set that we worked with was approximately 37 MB in size with about 291,000 rows of data.
Analysis Neighborhood
Since a propertiy's 'Analysis Neighborhood' is not likely to change within a short timespan, we corrected any NaNs in 'Analysis Neighborhood' for properties that had this field populated in previous years. After making corrections, we dropped any rows with NaNs in this field, as 'Analysis Neighborhood' is a main component of our analysis. Additionally, since we are interested in predicting a property's value using 'Analysis Neighborhood' as a factor, we removed data for Analysis Neighborhoods with less than 10 data points, as this information may not allow for accurate predictions. After doing this, we have a total of 30 analysis neighborhoods in our study.
Number of Bedrooms
Within the 'Number of Bedrooms' field, we corrected values for some large number of bedrooms (12, 13, 22, 44, 80, 2065) that appeared to be a result of human error. Corrected values were determined either by viewing the number of bedrooms for that property in other years, or by verifying with online real estate websites like Realtor.com or Zillow. Furthermore, we dropped rows with 'Number of Bedrooms' greater than 10 as the distribution of data showed that an almost negligible percentage of datapoints were outside of the 10 bedroom cut-off and may have been as a result of human error that we could not verify.
Property Area
For the 'Property Area' column we started by correcting property area that appeared to be a result of human error. Corrected values were determined either by viewing the property area from other years, or by verifying with online real estate websites like Realtor.com or Zillow. After viewing the distribution of 'Property Area', we dropped rows with 'Property Area' less than 500 sq. ft. and greater than 6000 sq. ft. as there wree very few properties outside of the 500-6,000 sq. ft. range.
Number of Bathrooms and Rooms
Since very few properties had NaN, zero, or more than 6 bathrooms in the dataset, we kept only rows with 1-6 bathrooms. A similar approach was used for the 'Number of Rooms' field and resulted in the retention of data points with 1-9 rooms. Rows with NaNs in both of these fields were dropped since 'Number of Bathrooms' and 'Number of Rooms' are critical to our analysis.
the_geom
'the_geom' field which contained the coordinates of each property was provided in (longiture, latitude) format. in order to use this data to create a map in out dashboard (described below), we converted the quantities in this field to (latitude, longitude) format.
Additional Data Fields
Two additional rows were added into the data set : 'Total Assesed Value' and 'Property Age'. The 'Total Assessed Value' field, which is the dependent variable of interest, was created by adding the "Assessed Fixtures Value', 'Assessed Improvement Value', and 'Assessed Land Value' fields. Since we were interested in how property age may impact assessed value, we created a 'Property Age' field by subtracting the "Year Property Built' field from the 'Closed Roll Year'.
Exploratory Data Analysis
Histograms were created to view the distribution of our data with respect to the Assessed Value (in Millions), Property Area, Property Age, and Number of Rooms, Bathrooms, and Bedrooms.
Screen Shot 2021-02-09 at 12.26.07 PM.png
The assessed value ranges from $300 - about $13 million. The mean assessed value is $725,678 and median value is $566,549. The histogram shows that the assessed value is skew right with most properties having an assessed value between $250,000 and $800,000.
The property area is slightly skew right. The values range from 500-6000 square feet, with most homes having 1,000-2,000 square feet. The mean and median property area are 1745 and 1658 square feet respectively.
The average property age is 78 years and the median property age is 81 years. The histogram shows two groups of properties, newer homes that are less than 50 years old and older homes that are between 75-100 years. Most of the property however have ages between 75-100 years.
The number of bedrooms ranges from 1-9. Most homes have 3 bedrooms, which is also the mean and median number of bedrooms. The number of rooms also ranges from 1-9 rooms with the mean number of rooms being 6 and median being 7. As shown in the histogram, most properties had 5-8 rooms. The number of bathrooms range from 1-5 with most homes having 2 bathrooms. Both the mean and median number of bathrooms are 2 respectively.
Box plots were also created to visually display the distribution of assesed property value (in millions) as it relates to the number of rooms, bathrooms and bedrooms. In all of the boxplots, we see that there are many outliers in the data. Additionaly, we notice that assessed property value increases with the number of rooms and bathrooms, as expected. However, we notice that the assessed value increases with the number of bedrooms until one reaches 5 bedrooms. After 5 bedrooms, it seems that the assessed value begins to slightly decreasand increases sharply at 9 rooms.
Screen Shot 2021-02-09 at 12.28.09 PM.png
When exploring analysis neighborhoods, we notice that about 75% of our data points are in Sunset/Parkside, West of Twin Peaks, Outer Richmond, Bernal Heights, Bayview Hunters Point, Excelsior, Oceanview/Merced/Ingleside, Inner Sunset, and Outer Mission, with the first four neighborhoods mentioned holding 50% of the data.
Screen Shot 2021-02-09 at 12.30.06 PM.png
Additionally, a box plot of Price vs. Analysis Neighborhood, we notice that some of the most expensive neighborhoods are Pacific Heights and Nob Hill while Visitacion Valley and McLaren Park appear to be among the least expensive neighborhoods.
Screen Shot 2021-02-09 at 3.24.17 PM.png
Correlation Analysis was done to identify the features that are most correlated with a home's Assessed Value. A correlation heatmap (below) shows that property area is most correlated with assessed value. In fact, it has a higher correlation with a home's price than the number of rooms or bedrooms. Additionally, we see that property age has a positive correlation with assessed value, which is surprising. Upon further investigation, we see that even tough the correlation between assessed value and property age across all of our data points is positive, most of the conditional correlations within analysis neighborhoods are negative.
Screen Shot 2021-02-09 at 12.28.52 PM.png
Screen Shot 2021-02-09 at 12.29.05 PM.png
Dashboard
As an extension to our EDA, we made a Dashboard on Google Data Studio. This dashboard is interactive and shows how changes in on variables could affect other variables.
Please click this link to view our dashboard: https://datastudio.google.com/reporting/e0b08254-0f15-4759-b618-94e6aca3301f
Statistical Analysis
After complete our EDA, we conducted statistical tests (T-Test / ANOVA / Pairwise Test) to determine which explanatory variables to use in our Predictive Models.
Linear Predictive Model
First, we started with a Linear Regression Model in order to understand the effects of the indepedent variables on the dependent variable.
Because the 'Analysis Neighborhood' variable was a string, we converted each 'Analysis Neighborhood' into a separate variable. This variable has a value of 0 when the specific property was not in that neighborhood and a value of 1 if the property is in that neighborhood.
We got the following results:
image.png
image.png
From these results, we can see the effects of each independent variable. For example, holding all else constant, increasing the 'Property Area' by 1 would increase the 'Total Assessed Value' by 354.
However, in this type of model, we expect an R-Squared value of around 0.9 whereas our model only has an R-Squared of 0.442. Although this indicates that our Linear Model is not highly predictive, we can see the relative impacts of the different independent variables. For example, we can see that 'Number of Bathrooms' has a greater impact on 'Total Assessed Value' than 'Number of Bedrooms' Does.
Machine Learning Predictive Model
We also decided to train and test a Machine Learning Model using a Random Forest with the same variables as we used in the Linear Regression Model. Due to RAM availability, we were only able to train the model for the year 2019.
The following how we trained and tested the Random Forest:
Train Size: 0.9 Test Size: 0.1
n_estimators: 25
max_depth: 10
Accuracy: 0.0012
From this, we see that our ML Model has extrermely low accuracy. We believe this is due to the low sample size (especially because RAM capability was limited) and due to the low number of variables used in the model.
Conclusions & Future Work
Although variables like 'Property Area', 'Number of Bedrooms', and 'Number of Bathrooms' are key factors in influencing the valuation of the home, there are many more variables not included in the dataset we used that could impact the appraisal values. Including variables like 'Crime Rate', 'Proximity to Schools', and 'Upgraded Appliances' could also influence the value.
If more features were included in the model, future work could give 'homes similar to yours' recommendations and their respecive appraisal values to help property owns challenge appraisal values.
We would also like to see the descrepancy between the Sale Price of a house and the Assessed Value for that property. A compiled dataset including both the 'Assessed Value' and 'Sale Price' would help with this future analysis
