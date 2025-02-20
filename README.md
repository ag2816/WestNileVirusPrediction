<h1>West Nile Virus in Chicago</h1>
<h2>Goal:</h2> 
Predict when and where various species of mosquitos will test positive for West Nile Virus within the city of Chicago.
<h2>Background:</h2>
West Nile virus is a disease that is predominately spread to humans through infected mosquitos. About 20% of people who are infected develop symptoms which can range from a fever to a serious neurological illness and even death.

The Chicago Department of Public Health has set up Traps around the City of Chicago to capture mosquitos. Those traps are checked weekly and a count is made of the number of mosquitos found and whether or not any of them tested positive for West Nile (a simple yes/no boolean flag)

<h2>Mosquito Lifecycle</h2>
Mosquitos start to emerge from hiberation in May, peak in roughly Mid-August but continue to breed all the way through until September. The two most common species of mosquito found in the Chicago area are Culex Pipiens and Culex Restuans, although it is mainly pipiens that feeds on humans. Depending on weather conditions, it can take between 4 days and a week for eggs to hatch. Generally, these mosquitos do not venture far from where they hatched -- their flight range is about 1/2 mile (data taken from Harrison Count Health Department (www.harrisoncountyhealth.com/wnv_and_mosquitoes.htm) and http://vectorbio.rutgers.edu/outreach/species/pip2.htm

It is believed that hot and dry conditions are more favorable for West Nile virus than cold and wet.

<h2>Tools Used</h2>
<ul>
  <li>Pandas</li>
  <li>Imbalanced Learn (SMOTE)</li>
  <li>DataFrameMapper, Pipelines</li>
  <li>Clustering: HDBSCAN</li>
  <li>Classifiers: Random Forest, CatBoost, XGBBoost, Logistic Regression, SGVClassifier</li>
  <li>Visualizations: Seaborn, Matplotlib, OpenStreetMap</li>
  <li>Geopy</li>
</ul>

<h2>Data Sources</h2>
The data for this project was taken from the Kaggle competition:https://www.kaggle.com/c/predict-west-nile-virus/.
The Chicago Department of Public Health provided the following csv files:
<ul>
  <li>train.csv: for each of the traps, contains a weekly count of the number of mosquitos found and a boolean flag about whether or not West Nile virus was found in each. This file contains data for 2007, 2009, 2011 and 2013</li>
<li>test.csv: contains trap information for 2008, 2010, 2012 and 2014, but without counts as this is the dataset for which we need to generate prections</li>
<li>weather.csv: contains daily weather observations from two weather stations in Chicago</li>
<li>spray.csv: contains GIS coordinates for where the city conducted spraying for mosquitos in Chicago</li>
</ul>

<h2>Approach</h2>
Within the training data, decided to treat 2013 as the prediction year for train/test split testing (i.e. model was trained on data from 2007, 2009 and 2011 and then tested against 2013)

We are trying to predict the labelled variable WnvPresent (Y/N field in dataset)
<h2>Steps taken</h2>
<ul>
<li>Basic Cleanup (handle missing data, consolidate satelite traps, find "top traps" for WNV presence)</li>
<li>EDA</li>
<li>train test split (use 2013 as our test dataset)</li>
<li>use data frame mapper to imput missing values and select only the columns of interest</li>
<li>calculate key weather data (cumulative precipitation for year and running average precip and temp for last 1 and 2 weeks)</li>
<li>append weather data to trap dataset</li>
<li>add week of year and month cols to trap dataset</li>
<li>use SMOTE to generate new samples of WNV present</li>
<li>cluster traps using HDBSCAN</li>
<li>add cluster to trap dataset</li>
<li>run Classification models</li>
</ul>

<h2>EDA Findings</h2>
<ul>
<li>The dataset is imbalanced --  there are many more cases of negative results than positive results for the presence of West Nile virus.  West nile is only found 3.85% of the time.   We will need to take action to address this imbalance prior to training a model</li
<li>Culex Pipiens appears to be the main carrier of West Nile Virus as it is most commonly associated with the presence of west nile virus (note: we do not have a count of WNV infected mosquitos -- just a yes/no flag about whether it was present)</li>
<li>only 8 of 134 traps have more than 10 positive results Mosquito Numbers</li>
<li>Culex Pipiens numbers dropped off rapidly in 2009 and 2011, although increased again slightly in 2013</li>
<li>Culex Restuans is showing a gradual increase over time</li>
<li>Most counts are up in 2013 over 2011 West Nile Counts</li>
<li>In 2007, mosquito counts peaked in August, while in 2009, they peaked much earlier in June and July.  2011 also showed a peak in July</li>
  <li>2007 had wet august, 2009 had cool fall and 2011 had a wet june</li>
</ul>


<h2>Results</h2>
Best classification models were LogisticRegression and a Support Vector Machine Classifier. The best score achieved on the validation set was with Logistic Regression
<h3>Top predictors for presence of West Nile virus:</h3>
<ul>
<li>week_num (18457.99)</li>
<li>two_week_avg_temp (15469.16)</li>
<li>distance_top_trap (-8835.48)</li>
<li>month (3448.50)</li>
<li>Species_CULEX PIPIENS (2508.58)</li>
<li>Species_CULEX RESTUANS -(2338.69)</li>
<li>cum_precip_yr (945.53)</li>
<li>year (-889.31)</li>
<li>Trap_T115 (865.19)</li>
<li>week_avg_temp (-743.56)</li>
</ul>
