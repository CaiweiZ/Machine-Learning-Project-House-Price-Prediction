# Machine-Learning-Project-House-Price-Prediction
## <span style="color:#1D665D;">Problem Statement</span>
Our project aims to predict house prices in Ames, Iowa, using machine learning methods.

In finance industry, analysts usually use existing asset pricing models, such as capital asset pricing model (CAPM) and Fama-French 3-factor model to evaluate asset prices. However, many literatures, for example, Li and Yang (2011), have provided empirical evidence against such models’ ability in predicting different asset prices and suggested finance industry should be cautious when applying these models. Therefore, we hope to build an application with machine learning techniques to help better predict future house prices for home buyers and investors. 

In this repository, You can find the code book, the knited R markdown file and this executive summary.

## Data
We use data from Kaggle’s “House Prices- Advanced Regression Techniques” competition project. The original Ames Housing data set was compiled by Dean De Coock (2011). The training data and test data are readily available for use on Kaggle. To differentiate from the Kaggle project, we use the training data only. The dataset includes 1460 data points with 79 independent variables that could potentially predict house prices. The independent variables consider a wide range of housing features, from number of bedrooms and bathrooms to fence quality. A comprehensive variable description is attached in Appendix. 

We took 70% of the housing data points as our training data, and the remaining 30% as test data. However, we still need to deal with missing values. There are 6965 missing values in our housing data. By visualizing the pattern of missing data and calculating the percentage of missing values in every variable, we find there are four variables with more than 80% missing values. Since those variables are too incomplete to offer valuable information, we delete these columns. We also found that some NAs have the meaning of zero, so we reassigned them as "none" for factors and 0 for numeric variables. For the variables with missing values of less than 5%, we also used Multiple Imputation method to generate replacement values for missing data.

## Methods
We apply six learning methods in our project: regression tree, lasso regression, ridge regression, elastic net, random forest and boosting. Since this is a regression question, we use root mean squared error (RMSE) to estimate the square root of the mean error between the predicted values and the true values. A smaller RMSE suggests a more precise model. In such dataset with 78 possible explanatory variables, it is hard to make predictions based on every variable and make clear interpretations for each variable, so simple regression models would perform poorly in this project and we mainly focus on shrinkage methods to avoid overfitting, and tree-based methods for better accuracy. To compare model performance, we obtain RMSEs from both 10-fold cross validations and predictions using test data for each model.

In regression tree method, we start with growing a fully grown tree by setting the complexity parameter (cp) to 0 to generate no penalty results. We test different number of nodes and prune the tree based on the performance to find minimum node that has similar performance with large enough nodes. The tree within one standard deviation has 8 terminals and we obtain a tree with 8 terminal nodes by setting maximum depth to 3. Finally, we perform 10-fold cross validation and make predictions on the test data with the model to obtain RMSEs. Since it is common to use the smallest tree within 1 standard error (SE) of the minimum CV error (1 SE rule), we also included RMSEs for 1SE regression tree.

Similarly, for lasso regression, ridge regression and elastic net, we obtain RMSEs from both cross validations and predictions on the test data. The 1 SE rule is also applied in each method. Different from regression tree, in these shrinkage methods, we focus on different alpha values to tune the models. We try to find out the best shrinkage penalty for each lambda and use the best lambda to train the model and make predictions. 

In random forest, we initialize our hyperparameter grid by setting mtry from 5 to 15 (increase by 1 each time), nodesize from 2 to 8 (increase by 2 each time) and sample size 0.6, 0.7, 0.8 and 0.9. we loop through each hyperparameter combination and apply 300 trees to find the best random forest model. Finally, we obtain RMSEs from cross validation and prediction.

In boosting, we also begin with performing a grid search, which includes different values for four hyperparameters- shrinkage, interaction.depth, n.minobsinnode, and bag.fraction. We again search across various models to find the best combination. Like all other methods, the RMSEs from cross validation and prediction are obtained for comparison

## Results
We estimate test error using k-fold cross validation approach. Specifically, we divide training data into 10 equal-sized folds, trained models on one-fold and validate them on other held-out folds and obtain test error by averaging the folds' errors.  Before we really get to the test data, we can pre-estimate the models’ performance by comparing their cross validation (CV) error. Then, we test models in the test data we set apart, and get a RMSE, which suggests the mean distance of our predictions to the true value.

| Left | Center | Default |
| ---- | ------ | --------|
| tree.nocp | 29454 | 35154 |
| tree.1se | 46445 | 42842 |
| lasso.best | 36210 | 30598 |
| lasso.1se | 40855 | 36753 |
| forest | 31548 | 26261 |
| boost | 28370 | 24188 |


We find that the boosting model has the smallest CV error as well as the final RMSE. The smallest RMSE we had was 24188. Comparing the average sales price in our test data – 180189, this error is acceptable, suggesting our prediction will be off by about 24188/180189 = 13.4% off by the actual value.

Now that we have trained the best models for predicting house prices, we analyze which factors have the biggest impact on house prices. We can see from the importance plot of the boosting model that the most three importance variables affecting house prices are the overall material and finish of the house (OverallQual), above ground living area square feet (GrLivArea), and the community the house is in within Ames city limits (Neighborhood).  

![fig.1.Important variables for predicting house prices](https://user-images.githubusercontent.com/99046089/152629680-a6852c0f-621a-41e3-98e6-0f5f01f783ec.png)

The area of the basement (TotalBsmtSF), lot (LotArea) , first floor (FirstFlrSF) and the size of garage (GarageCars) are also important in predicting house prices. The quality of kitchen (KitchenQual) and bathroom (FullBath) can affect house prices, too. 

Neighborhood is the only factor variable important in our prediction. To understand house price patterns in different neighborhoods, we can check the following plot.
![fig.2.House price patterns in different neighborhoods](https://user-images.githubusercontent.com/99046089/152629706-f389789e-acb7-40f6-b938-62628d622b16.png)


## Conclusions
From the models and RMSEs above, it is clear that different methods help us understand the data in different aspects. By the tree model, we analyze how different factors will contribute to the prediction. By shrinkage models and other tree-based models, we achieve better prediction results despite the models being difficult to interpret or visualize. Overall, we achieved the expected result and the project helped us gain better understanding of what has been taught in the machine learning course. 

From a managerial perspective, machine learning methods build models tailored for each asset, which can be a general asset class or a specific asset, like housing prices in Ames in our class. Our models apply specific factors that potentially influence the housing price and are possibly able to provide better prediction than those “one size fits all” asset pricing models. Moreover, depending on the needs of finance professionals, our models with different learning methods could provide more insights to help analyze investment opportunities.

## Works Cited
Li, Y., & Yang, L. (2011). Testing conditional factor models: A nonparametric approach. *Journal of Empirical Finance*, 18(5), 972-992. 

Cock, D. D. (2011). Ames, Iowa: Alternative to the Boston Housing Data as an End of Semester Regression Project. *Journal of Statistics Education*, Volume 19, Number 3. Retrieved November 22, 2021, from http://jse.amstat.org/v19n3/decock.pdf

