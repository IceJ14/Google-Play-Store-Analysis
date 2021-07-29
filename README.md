# Google-Play-Store-Analysis
Using a data set sourced from Kaggle.com, we asked if we could model the set of Google Play app installation behavior to analyze what combination of features makes a user more likely to install an app.

## Project description

Thousands of new apps are released onto the Google Play Store every week. An app developer approaches us with plans of developing a new app and releasing it onto the Google Play Store. The developer does not know exactly what the app will look like in terms of size, functionality, category, etc. 


### Goal
The developer does have a goal: to have the app installed on as many devices as possible. 
1. Create a dependent variable which best describes an app’s success.
1. Model previous 2020 Google Play app installation behavior to analyze what combination of parameters makes a user more likely to install an app.
1. Narrow down the data set to an accurate representation of the 2020 Google Play Store. 

### Analytics Objectives / Questions
1. Can we model the previous Google Play app installation behavior to analyze what combination of parameters makes a user more likely to install an app?
1. Is there a correlation between the App’s rating and the number of installs?
1. Does an app with a higher rating tend to have more installs? Are there outliers?
1. Do free apps tend to be downloaded more than non-free apps?
1. What is the correlation between app price and the number of installs?
1. Is there a correlation between app category and the number of installs?
1. Does the app size have an effect on the number of downloads?
1. Are apps released earlier in 2020 installed more on average than later releases?
1. Is there app seasonality?
1. Is there a correlation between the content rating and the average number of downloads?
1. Do apps that receive an ‘Editors Choice’ rating get more installs?
1. Do ad support apps fare better than non-ad support apps?
1. Are apps with in-app purchases more download than apps without in-app purchases?

### Links
[Google play Store Apps | Kaggle](https://www.kaggle.com/gauthamp10/google-playstore-apps)

### Collaborators
Work completed by [Bradford Murphy]() and [Jayke Sudana](https://github.com/IceJ14)
### Software
The XLMiner add-on in Excel was used to perform regressions and decision trees.

Tableau for data preprocessing and visualization.

## 0.0 MetaData
![image](https://user-images.githubusercontent.com/84804115/127534443-7da19423-59bf-4da2-b1c5-e8955eaabbcf.png)


## 1.0 Data PreProcessing
PreProcessing of the data involved filtering and sorting the csv file, deleting columns and rows, visualizing the remaining data to find trends and outliers, and summarizing the data.

#### 1.1 Handling missing data
Because our dataset was so large( over a million records), we dealt with all missing data by deleting the records which contained cells with missing values. We also delete columns which we consider to be useless. This is because the size of the dataset was causing excel to freeze and restart every time we performed a task. The only way to solve this problem was to “cut the fat” from the dataset.


#### 1.2 Random sampling
After deleting missing values and narrowing down our search, our data set contained about 107,000 records (cut down from about 1.2 million ). We did this by narrowing down the data in our dataset. The first thing we did was to extract just the 2020 data instead of the entire time frame (apps released in 2020). The next thing we did filter the data down to only top 5 values (in terms of frequency) in our category variable. This left us with the app categories Education, Business, Lifestyle, Music, and Entertainment. To sample, we assigned a random number to each of these records using the RAND() function and then sorted the “Random” column from largest to smallest. We then took the first 10,000 records of this sorted list as our randomly generated sample. 

#### 1.3 Summary characteristics
In the Standard Deviation column, we see some extremely large values. This indicates that the data is very spread out in columns like “Installs per day” and “rating count”. Upon further inspection, we found outliers in both variables. We did view this as a problem but one which we solved using decision tree analysis. 

![image](https://user-images.githubusercontent.com/84804115/127533497-bb88f171-79d3-4c77-8bf2-884473c777d4.png)

#### 1.4 Correlation table
The first thing I notice is the correlation of all the variables with our dependent variable: installs per day. This means our numerical variables are likely to have an effect on our results. There is also some correlation between the amount of time the app has been out and the rating count. This inter-independent variable correlation did end up causing some issues and the variable “rating count” had to be left out of our decision tree. 

![image](https://user-images.githubusercontent.com/84804115/127533690-6b8e2750-62b8-4de7-a231-fad04d48142e.png)

#### 1.5 Histograms
For rating, we see normal distribution with the exception of the 0-star rating. This may be due to the apps with no ratings. The size histogram shows normal distribution. The installs per day histogram (our dependent variable) has an issue. The bins are too large because of some outlier apps which have a massive number of installs per day. The decision tree dealt with this problem effectively and is explained in the conclusion.

![image](https://user-images.githubusercontent.com/84804115/127533749-dfaeb005-78cd-4a22-9052-2c910952b7e8.png)


#### 1.6 Scatterplots 
Apps with higher ratings tend to be downloaded more times per day. Apps that are rated “Everyone” are downloaded the most times per day. There is no obvious relationship between app size and the number of installs. 

![image](https://user-images.githubusercontent.com/84804115/127533801-ad416753-db0f-451e-8207-5e73b1fbf849.png)

#### 1.7 Box Plots
The boxplots which involve installation count tend to skew towards the lower values. For example, when looking at installs per day, we see that almost all the values are below 10K except for some possible outliers. From the size boxplot, we see that the majority of our apps are on the smaller side. From the rating boxplot, we see a very evenly distributed boxplot. 

![image](https://user-images.githubusercontent.com/84804115/127533905-730eff7c-a9f7-429a-a7fb-4bca754564d8.png)

## 2.0 First Model: Linear Regression
A linear regression is a linear model which assumes a linear relationship between our features (independent variables) and our output variable (installs per day)


#### 2.1 Variable Selection
The independent variables selected were category, rating, rating count, size, days, minimum android, content rating, ad supported, free, in app, and editors choice. These variables were selected because they were involved in our business questions as well as were considered to be the parameters to have an effect on our dependent variable installs per day.

We used a stepwise feature selection to select the best subset of features for the regression.

#### 2.2 Equations and Model Interpretation
Installs per day = 64.60 + 0.6644RatingCount + 1.4979Size - 0.5018Days
1. Installs per day increases by 0.6644 when Rating Count increases by 1, holding other variables constant.
1. Installs per day increases by 1.4979 when Size increases by 1, holding other variables constant.
1. Installs per day decreases by 0.5018 when Days increases by 1, holding other variables constant.
![image](https://user-images.githubusercontent.com/84804115/127535104-bdc06699-c926-4a0a-8a66-2f03cf9ab092.png)

#### 2.3 Training & Validation Summary Reports
Validation: Prediction Summary

![image](https://user-images.githubusercontent.com/84804115/127535222-62f18c16-da76-4d7e-b481-886b6cfb33f9.png)

Training: Prediction Summary

![image](https://user-images.githubusercontent.com/84804115/127535271-894459c4-87ea-47d4-a814-39fdaab4297a.png)

Lift Charts. (Validation on the left and training on the right)

![image](https://user-images.githubusercontent.com/84804115/127535414-e15eb29f-4987-444a-b0b1-fe80eb65ee06.png)

When comparing training and validation data and lift charts, we saw that the AUCs for both charts were quite similar. The RMSE for validation (1197.9) was a bit lower than the training data’s RMSE (1665.6), while the R^2 statistic for Validation (0.30) was higher than that of Training (-0.26). 

#### 2.4 Conclusions
First of all, the results tell us to ignore many of the variables we thought may have an effect on the number of installations. Also, the results tell us that in order to increase the installs per day for an app, an app developer must increase the number of ratings on the app, advertise heavily soon after the app's release, and increase the size of the app. A larger app may perform better than smaller sized apps.

##### 2.4.1 Advice to app creators
We defined a successful application as one which receives many installations per day. The characteristics of an app which contributes to its success were somewhat unexpected. First of all, an app will be more successful if it has more ratings. This means that developers should encourage users to write reviews for their app. Maybe, alert app users with a notification saying something like “If you enjoy using our app, leave a review on the Google Play store!” and include a link to the review area. Secondly, an app will receive less downloads per day as the app's time on the Google Play platform increases. This makes sense because many apps have periods of high growth soon after release. We recommend app developers to market heavily soon after the app is released to capitalize on the app’s early “buzz”. Lastly, larger sized apps tend to receive more downloads. This may seem counterintuitive but we believe that it has to do more with the apps performance. We believe that larger sized apps (generally speaking) perform better than smaller sized apps. Users would rather download high-performance apps. Because of this, we recommend making the app as high-performance as possible, even if they have to make the app larger to achieve this goal.

## 3.0 Second Model: Regression Tree
We used a regression tree to model the parameters in an easily interpretable manner and automatically perform variable selection. A regression tree gives us a simplified insight into which variables are most important for determining a high number of installs per day.

#### 3.1 Variable Selection
The independent variables selected were category, rating, size, days, minimum android, content rating, ad supported, free, in app, and editors choice. These variables were selected because they were involved in our business questions as well as were considered to be the parameters to have an effect on our dependent variable installs per day. *We should note that rating count was excluded from the input since it was too highly correlated with installs per day.*

#### 3.2 Model Output and Interpretation

##### 3.2.1 Fully Grown Tree

![image](https://user-images.githubusercontent.com/84804115/127535831-2a380410-398f-4f1a-bffa-abea5fecb312.png)

##### 3.2.2 Best-Pruned Tree

![image](https://user-images.githubusercontent.com/84804115/127535908-018c4a01-fc1c-4479-8fcd-b9e832064e7e.png)

Rules:
1. Go left if Size < 91.5 and right if Size > 91.5
1. Go left if Rating < 2.25 and right if Rating > 2.25
1. Go left if Days < 335.5 and right if Days > 335.5
1. Go left if In App Purchases is True and right if In App Purchases is False.

#### 3.3 Training, Validation, & Test Summary Reports

Training Summary:

![image](https://user-images.githubusercontent.com/84804115/127536148-d4decbed-5df6-47b3-9f18-24eefa3a00d8.png)

Validation Summary:

![image](https://user-images.githubusercontent.com/84804115/127536243-f1d4b01c-75b4-43bf-96ee-91fc67fa285d.png)

Test Summary:

![image](https://user-images.githubusercontent.com/84804115/127536316-8630cdf8-298e-4635-bfa4-3d052ef6566c.png)

Lift Charts (training to the left, validation to the right):

![image](https://user-images.githubusercontent.com/84804115/127536414-c6e22d3b-b602-4d1f-a665-6de0bdfbaa65.png)

Test lift chart:

![image](https://user-images.githubusercontent.com/84804115/127536452-54666e0e-6e31-48cf-9fd9-1f9816066f15.png)

Based on the training, validation, and test lift charts, this model performed quite well. The AUC for each lift chart was rather large: validation being the largest, then training, then test. While comparing the summary statistics, we saw that validation had the highest RMSE (1894) while the test summary had the lowest (483.6) with training in between (1387.7). The R^2 for training and validation were extremely close (0.015 and 0.014 respectively), while for the test the R^2 was -0.099. Validation lift chart performed better than training and test. One observation from the test was that the first few cases fell below the average curve, then the slope became nearly vertical and the ROC was far above the average curve.

#### 3.4 Conclusions

The results from the regression tree indicated that the variables most influential to our dependent variable were Size, Rating, Days, and In App Purchases. From these results, a developer would want to increase their app size, make an app that is highly rated, be patient with their app’s success, and have in-app purchases available.

##### 3.4.1 Example:
App with size 50MB, has a rating of 4.2, has been on the Google Play Store for 125 days, and does not have in-app purchases.

Size < 91.5, go left. Rating > 2.25, go right. Days < 335.5, go left. In App purchases in false, go right. 

The predicted average installs per day would be 102.05.

##### 3.4.2 Advice to app creators:
We defined a successful application as one which receives many installations per day. As found in both the linear regression model and the regression tree, larger sized apps tend to receive more downloads. This may seem counterintuitive but we believe that it has to do more with the apps performance. We believe that larger sized apps (generally speaking) perform better than smaller sized apps. Users would rather download high-performance apps. Because of this, we recommend making the app as high-performance as possible, even if they have to make the app larger to achieve this goal. An app that takes into account use of latest technology, security, business solutions, connectivity, offline functionality, personalization, and more constitutes a more successful app and would generally be larger due to the number of incorporated factors (“What factors contribute to the success of a mobile app?”).
	Apps that receive higher ratings tend to receive higher downloads per day. This makes sense, since an app that is rated highly would be more attractive to the user and more likely to install it. Furthermore, apps that are installed more are also rated more, therefore the likelihood of earning higher ratings increases. We recommend using surveys, app testers, app reviewers, and targeting an audience that generally gives higher ratings in order to boost the rating of an app and increase the likelihood of app success. 
	Another important variable to consider is the number of days the app has been on the Google Play Store. Our findings show that apps that have been out longer tend to have more installs per day. This may be due to the fact that apps can grow in popularity over time. We recommend that after an app is released that it is important to still market the app and update the app to maximize growth over time. Lastly, in-app purchases may have a factor on app success. Apps that have in-app purchases tend to be installed more frequently than apps that do not have in-app purchases. “Monetization is directly linked to engagement,” so we recommend that the developer has a strategy for driving user engagement via in-app purchases to increase repeat purchases and overall popularity (“Driving Buyer Behavior with In-App Purchases). 
  
## 4.0 Lessons, Conclusions, and Potential Issues with this Project
#### 4.1 Comparison Between Models
In this case, we would use the decision tree model. Both models performed very well in terms of ROC but for our goals, it makes sense to use the decision tree. One very important reason for doing this is that a tree model does not assume the data is in normal distribution. This means, the model can handle a dataset with outliers (as it will simply locate the outliers in a separate section of tree). This is exactly what our tree ended up doing for our Google Play dataset. This made it possible for us to feed the model an imperfect dataset and receive useful insights. The decision tree also ended up giving very accurate results (shown in the lift charts above). The linear regression model, although somewhat successful, did not deal with the complexity of our dataset in the way we wanted it to. 

#### 4.2 Lessons Learned!
1. We learned how to create research questions related to a data set that had real-life applications.
1. We learned how to clean, process, and sample a data set of over 1 million values. Our conclusion? It was hard!
1. We learned how to visualize our data and draw conclusions from histograms, scatter plots, box plots, correlation tables, and summary statistics.
1. We learned how to implement different types of models for our data set and performed variable selection and interpretation.
1. We learned how to analyse training, validation, and test summary tables and lift charts to gauge and compare model performance.
1. We learned how to apply the models and statistics to our questions, and relay those conclusions in business-applicable insights and recommendations.

#### 4.3 Potential Issues with our Analysis
There were several potential issues that arose as we worked through this data set. First of all, we had to drill down our data set by choosing apps that were released in 2020 and fell under 5 categories: Music, Education, Lifestyle, Entertainment, Business. We drilled this down further by taking a random sample of 10,000 values from the remaining data set. Such a small sample size (10,000 / 1.1million, or 0.9%) does not capture an accurate representation of all the Google Play Store apps. To deal with this issue, we would need more computational power to successfully create predictive models from 1.1 million+ rows of data in Excel. Another way to deal with this issue is to test out different kinds of sampling. 
	Another issue with our data set was the presence of outliers in our dependent variable. From our Tableau analysis and summary characteristics, we found that the standard deviation for installs per day was very high (1,453). The histograms indicated that installs per day had extreme outliers since the bins were so large (such as an app that saw 90,000+ installations per day). To deal with this issue, we could have set a finite range to limit outliers. Also, we could have used a log transformation on installs per day to reduce outliers. 
	The variable, Rating Count, presented problems for our predictive models. We had to remove Rating Count from our regression tree analysis since the correlation between Rating Count and our dependent variable was high (0.5). A possible solution to this issue would be to remove the Rating Count column from the data set.
	We had an issue in determining the dependent variable. The dependent variable was derived from Maximum Installs divided by the number of days the app was on the Play Store in 2020. A possible solution to this issue would be to have this variable included in the data set beforehand.
