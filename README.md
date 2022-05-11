# ds_consultant

**Background and Introduction** \n
Munchies is one of the fastest-growing cannabis brands in the world (made up project) and I have been hired as a consultant to conduct market research and effectively model and predict sales ($) a company may have based on data like the brand, total units, product count, inhalables offered, previous month sales, etc. I have decided to build a model that predicts sales because this will enable Munchies to leverage this information to maximize a feature that will likely increase their profits and ultimate success.

To begin, I will engage in data cleaning to create a data set that I am happy with and that can be used and implemented correctly into a machine learning model. As for the exact machine learning algorithms I intend to use, I will be building both a multiple linear regression and a random forest model. By using different models and approaches, I will be able to determine which one is better at accurately predicting and modeling the data. My ultimate goal is to provide Munchies with a model that is generalizable and can be used to feed in new data and accurately predict sales, helping them identify the most important features that maximize growth and grow as a company as a whole.
**Methodology**
I was provided with multiple data sets containing different features however all linked to each other through at least one feature. To start off, I built a single dataset that contained features from all of the data sets provided. On top of this, I developed a basic time-series feature extraction plan. This would enable me to use time-series in my model and accurately predict sales based on this as well as other features. For example, by enabling the feature of month, I can leverage month in my model to influence predicted sales output depending on what month it is. I converted month into a usable datetime format so that the model could interpret and use it. Ultimately, at the end of this process, the data frame that would be used to feed into my model had usable time-series information that could be used to predict sales.

Next, I engaged in some intensive data cleaning as well as augmentation and imputation. I decided to drop all columns/features that contained any NA’s because the features that did contain NA’s had thousands of them. In my mind, this was too many missing data pieces to use another method such as imputing the mean or mode. Instead, I decided to entirely drop these columns and not use them to predict sales. I also decided to create new features for the data set being if inhalables were sold or not by the brand. This was not a feature explicitly given in any of the provided data sets however I deemed it a potentially influential factor in sales so I added it to the data frame. I also analyzed multicollinearity within the data set and dropped columns that were highly correlated with one another. I did this because features may become repetitive and negatively impact the final model. As for augmentation, this is where I dealt with categorical variables and scaled numeric features. In order for numeric features to be implemented correctly in a machine learning model, they must be scaled equivalently or one may have much more of an impact than another. For example, if one column contains all values within the thousands however another is all below 100, these will have very different influences on the significance of these features and their impacts. To fix this, I scaled the features by their relative maximums and minimums. As for categorical variables, one hot encoding was used to where each categorical variable was given its own column where it would be a binary representation of if that categorical variable was present or not.

In order to tune hyperparameters in my random forest model, I put it through the gridSearch and cross-validation process. In this, grid search and cross-validation run the model through different folds of data and test different hyperparameters to test determine which model with which combination of hyperparameters outputs the best performing model. This is very beneficial to use because it allows me to find which hyperparameters produce the best model instead of manually testing different parameters which can be extremely tedious and time-consuming.

When experimenting with my own custom model, the random forest model, I decided to use bagging, also known as bootstrap aggregation, in order to see if I could optimize performance. Bagging allows training data to be randomly sampled with replacement which may reduce variance. Essentially, random forest is an extension of bagging which fits multiple models on different subsets and then combines predictions from all these models. Usually, random forest can be seen as an improvement over bagged decision trees, however giving bagging a try is a good way to test other ensemble methods, as in bagging, we are less concerned with the overfitting of the actual training data.

**Results**
In order to test for the significance of each of my numerical features, I used my non-pipelined data to find important features. I did this through statistical hypothesis testing in which I was able to get the p-value for each regression coefficient and determine which ones were significant in actually predicting sales. At a significance level of 0.1, I found that all but one feature were significant. Therefore, I kept all numeric features except ‘vs. Prior Period’ which output a p-value of 0.380. Because it is was not significant under hypothesis testing I would fail to reject the null hypothesis and conclude that the regression coefficient ‘vs. Prior Period’ does not accurately predict sales all other features held constant. Based on this statistical hypothesis testing as a whole, keys to product sales would be total units, product count, and average rolling price.

<img width="699" alt="Screen Shot 2022-05-11 at 1 36 52 PM" src="https://user-images.githubusercontent.com/48026800/167943284-63aed04c-d8f9-4985-a878-e1f4c20d5ad0.png">


This is also where I tested for correlation between variables so I removed one variable from a pair if they were highly correlated with each other, -1 or 1. The heatmap showing correlations between features is shown below. In this case, I removed the features ‘previous month’ and ‘Rolling Average’ because previous month has a correlation of 0.99 with rolling average and rolling average has a 0.97 correlation with ‘Total Units’.

<img width="641" alt="Screen Shot 2022-05-11 at 1 37 15 PM" src="https://user-images.githubusercontent.com/48026800/167943373-0bf7f577-76ca-4884-b391-72ea3d27ca51.png">

 
Principal component analysis is a technique for reducing the dimensionality of datasets while minimizing information loss. However, after using PCA, explained variance scores were very low (first figure) and all of my regression results seemed to decrease and the model performed worse (2nd and 3rd figure). The fit of the model also can be seen as much worse. The predicted values were far off from the actual values so I did not decide to use a PCA model.

<img width="589" alt="Screen Shot 2022-05-11 at 1 37 35 PM" src="https://user-images.githubusercontent.com/48026800/167943424-f846889e-30f8-4940-9f71-afa876c1e610.png">

For my ensemble method, I decided to use a random forest model. This model outperformed the multilinear regression model by leaps and bounds. As seen in the regression results, all of the outputs are significantly better than previously. Ultimately, this model shows a great fit as the R^2 value is very high showing that about 91% of the variance in the response variable can be attributed to the predictor variables.

<img width="496" alt="Screen Shot 2022-05-11 at 1 37 52 PM" src="https://user-images.githubusercontent.com/48026800/167943468-0b718130-82a4-425d-b12a-391eecdb38ca.png">
   
After using cross-validation on my multiple regression model, the cross-validation scores output negative values indicating poor performance and bad fit. I can conclude even after k-folds cross-validation, that the regression model is not a good model for this data.

<img width="749" alt="Screen Shot 2022-05-11 at 1 38 07 PM" src="https://user-images.githubusercontent.com/48026800/167943504-f15baa2f-be37-4ce1-85fd-f716e534795e.png">

After using cross-validation and grid search on my random forest model, I was able to extract my best model with an R^2 of about 0.96. The ideal hyperparameters and regression results are shown below. As seen in the Predicted vs. Actual Sales plot, the model very closely fits the predicted sales.

<img width="758" alt="Screen Shot 2022-05-11 at 1 39 22 PM" src="https://user-images.githubusercontent.com/48026800/167943694-821e258b-7bd8-4fdf-aeb6-2fc82ae13f7f.png">

<img width="555" alt="Screen Shot 2022-05-11 at 1 39 37 PM" src="https://user-images.githubusercontent.com/48026800/167943732-49cb5735-31d2-445e-8ae4-b45cd4abb9e6.png">

  
After using bagging, my performance, specifically my R^2 value went down slightly so I decided to stick with my original best model created after grid search.

<img width="372" alt="Screen Shot 2022-05-11 at 1 39 48 PM" src="https://user-images.githubusercontent.com/48026800/167943768-859ea76b-f396-4db8-b6cc-f46136be01bf.png">




**Discussion**
From the results I’ve gathered, it is clear that my model obtained after using grid search and cross-validation is the best model. It fit the data so that about 95% of the variance in the response variable can be attributed to the predictor variables as well had an extremely low mean squared error loss. As for recommendations for Munchies, I recommend them to use my model because it can accurately predict sales given a number of features including brands, total units, average rolling price, inhalables, product count, etc. By using this model, Munchies can maximize their revenue and profits. While my initial multiple linear regression model did not accurately predict sales, my ensemble method, random forest, is able to accurately predict this and Munchies should utilize it to enable their growth as a company. From my model, they may also gain insight into what features of a company may be the most important for maximizing sales, for example, the brand, or the total units. I am confident that by adopting the model I have provided, Munchies can take their business to the next level and not only predict future sales but improve upon them and grow as a whole.
