## Marathon Data Analysis and Predicting Marathon Finish Times Project


## Overview

Welcome to the Marathon Data Analysis project! This project involves the analysis of race results from approximately 26,000 marathon runners. As there are many factors that go into the final result of a marathon such as training rigor and nutrition, but if we held these things constant, as a serious athlete would, could a race time be predicted based on prior year results? 
The primary goal is to utilize statistical and machine learning techniques to gain insights from the data and make predictions related to runner demographics and performance. Metrics of $R^2$ and $RMSE​$ (Root Mean Squared Error) are used to measure the results.  

## Objectives

The project is structured around several key objectives:

1. **Exploratory Analysis and Insights:**
   - Perform exploratory data analysis to uncover trends and patterns in the marathon data.
   - Identify relevant factors influencing marathon performance and demographics for future work.

2. **Gender Prediction using Kernel Density Estimation (KDE):**
   - Calculate 1-dimensional KDEs for male and female runners based on their finish times.
   - Utilize Bayes theorem to predict the gender of runners given their finish times.
   - Evaluate the accuracy of these gender predictions.

3. **Age and Finish Time Prediction using 2-D KDEs:**
   - Create 2-dimensional KDEs using finish times and ages of runners.
   - Apply the same Bayesian approach to predict gender and assess prediction accuracy.

4. **Gender Prediction using k-Nearest Neighbors (k-NN):**
   - Implement k-NN classification with input data of finish times and ages.
   - Compare the accuracy of gender predictions with KDE-based predictions.

5. **Finish Time Prediction using Linear Regression:**
   - Employ linear regression to predict finish times based solely on 5K times.
   - Evaluate the accuracy of finish time predictions.

6. **Multi-Feature Finish Time Prediction:**
   - Extend the linear regression model to include age and gender as additional features.
   - Evaluate whether this improves finish time predictions compared to the 5K time-based model.

## Tools and Libraries

- Python for data preprocessing, visualization, and analysis.
- SciPy library for Kernel Density Estimation.
- Scikit-learn library for k-Nearest Neighbors and Linear Regression.

### Data Preprocessing

With 26K runners, the following features were given in each set:

*  *Bib* - race number determined by the qualifying race time
* *Name* - the name of each runner
* *Age* - age of the runner on the day of the race
* *M/F* - gender of the runner
* *City* - city where runner is from
* *State* - state where runner is from (optional)
* *Country* - country where runner is from
* *Citizen* - country of citizenship (optional)
* *Unnamed* - special category for runners who are visually or mobility impaired (optional)
*  *5K, 10K, 15K, 20K, 25K, 30K, 35K, 40K* - the elapsed time at each 5K split of the race
* *Half* - elapsed time at the half way point
* *Pace* - overall average pace of the race
* *Official Time* - finishing time
* *Overall* - overall ranking of all runners
* *Gender* - ranking within the gender
* *Division* - ranking within the age/gender division (for example: Female ages 30-34, Male ages 45-50, etc.)

**Feature Enginering and additional data**

From the features above *City, State, Country, Citizen, Unnamed, Pace* were dropped because it was determined that there were either too many *NULL* values, too many unique categorical values, or highly correlated with other features (e.g. *Pace* and *Offical Time*).  

The  *5K, 10K, 15K, 20K, 25K, 30K, 35K, 40K, Half* elapsed times were transformed into differentials and the best fit slope (rate of change of pace) was calculated and added as a feature called *pace_rate* in place of them.  

The gender feature was split into 2 variables using dummy variables (0/1) for male and female. 

Lastly, historical weather data (*temperature, humidity, wind_speed*) from Weather Underground for each year was collected and added.  This was representative of the half way point (Wellesley) and the finish point (Boston) which can vary significantly since the runners are running from inland to the coast.     

The final dataset was brought down to only the legacy runners (runners who ran consecutive years) approximately 13k, and the current year finish time was replaced with the consecutive year finish time as the dependant variable and *'Bib', 'Age', 'overall_rank', 'gender_rank', 'division_rank', 'pace_rate', 'temp', 'humidity', 'wind', 'Gender_F', 'Gender_M'* as the independant variables.  

### Data Exploration and Visulization

The graph below displays the spread of finish times over the given bib numbers.  It makes sense that there is a linear relationship between the bib number and the finish time since the bib number is assigned based on the qualifying time from some other previous qualifying race.  The breaks in the bib numbers, which is obvious on the graphs are indicative of the waves that go out at different start times.  This is not unusual at large races for safty reasons.  

One other interesting observation is the variance in male versus female of finish times.  Men on average had a much larger variance of finish times compared to women.  Meaning women stay closer to their bib qualifying time than men do.  Something for exploration another time. 


![download-1](https://user-images.githubusercontent.com/20651507/51808778-1021fd80-224d-11e9-8cb5-4713e3cfc663.png)


The graph below displays the distribution of finish times of male versus female.  The distribution shape is the same for male/female and right skewed because mostly likely due to the charity runners or the older aged runners.  It is interesting to see large gap between the male and female for the faster runners, but as the time gets longer eventually the distribution evens out.  


![download](https://user-images.githubusercontent.com/20651507/51808779-14e6b180-224d-11e9-8893-289130b6fa92.png)


## Algorithms and Techniques

The algorithms that were most used were **Linear Regression** and **Stochastic Gradient Descent**.  

A pipeline was built that took in the features and data and split the data into X/y train/validation/test sets.  The train and validation sets were fit onto a model and predictions were made.  The predictions were then used to measure the results in terms of $R^2​$ and $RMSE​$. Learning curves and residual plots were made for each model in order to visualize the process.  

As a benchmark, I first ran a simple linear regression model using all of the independant variables and recorded the results.  The goal was to improve the results by reducing the $RMSE$ value and increasing the $R^2​$ value with subsequent models.  Those subsequent models inolved cross validation using KFolds and Stochastic Gradient Descent with various combinations of regularization (LASSO and RIDGE), tuning of hyper-parameters, and omitting various independant variables. 

## Results and Impact

This project showcases proficiency in quantitative analysis, statistical techniques, and machine learning. The outcomes include gender and finish time predictions and valuable insights for event planning and marketing.

## Results

In the process of viewing the results of each model a few things are worth pointing out in the graphs below.  The 2 graphs are representative of learning curves one which converges the other does not. Both graphs are for SGD and it is interesting to see the behavior when it behaves badly.  After scaling the data by using the ```StandardScaler()``` function in ```sklearn``` the learning curve behaved better and it convereged. 


![download-6](https://user-images.githubusercontent.com/20651507/51808747-c46f5400-224c-11e9-9375-a8e6818abea7.png)


![download-5](https://user-images.githubusercontent.com/20651507/51808753-cfc27f80-224c-11e9-8a85-4106ceb243ae.png)


Below is a table of the results of each model:

| **Model**                                        | $R^2$        | $RMSE$      | Minutes conversion |
| ------------------------------------------------ | ------------ | ----------- | ------------------ |
| Linear Regression                                | 0.66         | 1426.35     | 23.77              |
| KDE                                              | 0.63         | 1486.79     | 24.78              |
| **KDE w/ Naive Bayes theorem**                   | **0.69**     | **1353.81** | **22.56**          |



Below is the results of Linear Regression actual Vs. predicted values.  Notice how most of the points fall within one standard deviation (red dashed lines) of the regression line (in blue). 

![download-7](https://user-images.githubusercontent.com/20651507/51809371-25028f00-2255-11e9-9a80-e68fc7e96800.png)

## Usage

1. Download the `marathon_results.csv` dataset.
2. Run the project scripts to perform the analyses and predictions as outlined in the objectives.
3. Explore the project's visualizations and findings to draw insights from the data.

