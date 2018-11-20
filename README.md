<h1>West Nile Virus in Chicago</h1>
Goal: Predict when and where various species of mosquitos will test positive for West Nile Virus within the city of Chicago.
Background:
West Nile virus is a disease that is predominately spread to humans through infected mosquitos. About 20% of people who are infected develop symptoms which can range from a fever to a serious neurological illness and even death.

The Chicago Department of Public Health has set up Traps around the City of Chicago to capture mosquitos. Those traps are checked weekly and a count is made of the number of mosquitos found and whether or not any of them tested positive for West Nile (a simple yes/no boolean flag)

Mosquito Lifecycle
Mosquitos start to emerge from hiberation in May, peak in roughly Mid-August but continue to breed all the way through until September. The two most common species of mosquito found in the Chicago area are Culex Pipiens and Culex Restuans, although it is mainly pipiens that feeds on humans. Depending on weather conditions, it can take between 4 days and a week for eggs to hatch. Generally, these mosquitos do not venture far from where they hatched -- their flight range is about 1/2 mile (data taken from Harrison Count Health Department (www.harrisoncountyhealth.com/wnv_and_mosquitoes.htm) and http://vectorbio.rutgers.edu/outreach/species/pip2.htm

It is believed that hot and dry conditions are more favorable for West Nile virus than cold and wet. We provide you with the dataset from NOAA of the weather conditions of 2007 to 2014, during the months of the tests.

Data Sources
the data for this project was taken from the Kaggle competition:https://www.kaggle.com/c/predict-west-nile-virus/.
The Chicago Department of Public Health provided the following csv files:
train.csv: for each of the traps, contains a weekly count of the number of mosquitos found and a boolean flag about whether or not West Nile virus was found in each. This file contains data for 2007, 2009, 2011 and 2013
test.csv: contains trap information for 2008, 2010, 2012 and 2014, but without counts as this is the dataset for which we need to generate prections
weather.csv: contains daily weather observations from two weather stations in Chicago
spray.csv: contains GIS coordinates for where the city conducted spraying for mosquitos in Chicago
Approach
within our trainig data, we'll treat 2013 as our prediction year for train/test split testing
WnvPresent is what we are trying to predict (Y)
Steps taken

Basic Cleanup and EDA
train test split (use 2013 as our test dataset)
use data frame mapper to imput missing values and select only the columns of interest
calculate key weather data (cumulative precipitation for year and running average precip and temp for last 1 and 2 weeks)
append weather data to trap dataset
add week of year and month cols to trap dataset
use SMOTE to generate new smaples of WNV presents
cluster traps using HTBSCAN
add cluster to to trap dataset
run Classification models
EDA Findings
Culex Pipiens appears to be the main carrier of West Nile Virus as it is most commonly associated with the presence of west nile virus (note: we do not have a count of WNV infected mosquitos -- just a yes/no flag about whether it was present
only 8 of 134 traps have more than 10 positive results Mosquito Numbers:
Culex Pipiens numbers dropped off rapidly in 2009 and 2011, although increased again slightly in 2013
culex restuans is showing a gradual increase over time
most counts are up in 2013 over 2011 West Nile Counts:
numbers increased in 2013
2013, many observed in Culex Pipiens, Culex Pipiens/Restuans, Culex Restuans Mosquito Counts Per Year
in 2007, mosquito counts peaked in August, while in 2009, they peaked much earlier in June and July
2011 also showed a peak in July
Results
Best classification models were LogisticREgression and a Support Vector Machine Classifier. The best score achieved on the validation set was with Logistic Regression
top predictors for presence of West Nile virus:

week_num (18457.99)
two_week_avg_temp (15469.16)
distance_top_trap (-8835.48)
month (3448.50)
Species_CULEX PIPIENS (2508.58)
Species_CULEX RESTUANS -(2338.69)
cum_precip_yr (945.53)
year (-889.31)
Trap_T115 (865.19)
week_avg_temp (-743.56)
