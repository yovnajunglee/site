---
layout: post
title: A movie recommender system
excerpt: "Implementing the matrix factorisation algorithm for recommender systems"
tags: [intro, R, matrix factorisation, recommender systems, clustering]
comments: true
category: blog
---

The explosive growth in technology has contributed to an overwhelming amount of online information and digitalized services through the rise of platforms such as *YouTube*, *Netflix* and *Amazon*.  While internet users have now access to a plethora of information, navigating through this data overload has become an increasingly tedious task. This has led to the need to build a technology that would facilitate navigation by generating customised recommendations to enhance customer experience. The *recommender system* is the major breakthrough that emerged and was deemed as a revolutionary technique to provide automated suggestions. Amongst the strategies developed by researchers, collaborative-based filtering and content-based filtering algorithms are the most commonly used today. Collaborative-based filtering algorithms work by matching a particular user against other users in the database who have similar history and tastes - eg. same movies liked. On the other hand, content-based filtering algorithms make use of the features of the movies that the user has liked in the past, eg. genres, to recommend new items. In the context of collaborative filtering, user-based and item-based nearest neighbour algorithms are very common, nevertheless, the  [matrix factorisation](https://www.inf.unibz.it/~ricci/ISR/papers/ieeecomputer.pdf) model is also a successful implementation of recommender systems.

In this project, I will implement the matrix factorisation (MF) algorithm, from first principles, using the **R** programming language. 
# Methods

Before fitting any model, it is important to transform the data into the right format. In the context of recommender systems, we need to transform the ratings data to a user-item matrix, where the rows represent each user, and columns each movie. An example is as follows

|   | *Me Before You* | *Spider-man: Homecoming* | *Once upon a time in Hollywood* | *Captain America: Civil War* |
| :--: | :--: | :--: | :--: | :--: | 
| *John* | | 4 | 2 | 5 |
| *Monica* | 4 | | 5 | 2 |
| *Marc* | 3 | 3 | 4 | 4 |


## Matrix Factorization
Below is the mathematical formulation of the MF algorithm and a brief description of the steps involved in training the  model. 
### Model Formulation
The underlying objective of the model is to map users and items to a joint latent factor of smaller dimension, such that the interactions between the users and the items are modelled as an inner product of that space. More simply, we aim to model a large user-item matrix (*U X N*) into two smaller matrices (*(U X f)x(f X N)*) of smaller dimensions (*f << N*). The columns (denoted by ![\boldsymbol{p_i}](https://render.githubusercontent.com/render/math?math=%5Cboldsymbol%7Bp_i%7D)) of the (*f X N*) matrix represent the weight associated to each item *i* onto the different factors. Similarly, each user *u* is allocated a certain weight onto the factors ( rows of the (*U X f*) , denoted by ![\boldsymbol{p_u}](https://render.githubusercontent.com/render/math?math=%5Cboldsymbol%7Bp_u%7D)) which could be seen as a measure of the extent of interest the user possesses for those factors. The predicted ratings obtained from the model are then calculated as follows:
![\boldsymbol{q_i}^T\boldsymbol{p}_u](https://render.githubusercontent.com/render/math?math=%5Cboldsymbol%7Bq_i%7D%5ET%5Cboldsymbol%7Bp%7D_u)

The above captures the interaction between user *u* and item *i* and is therefore an estimate of the ratings. The algorithm aims at modelling the observed ratings only and the entries of the vectors ![\boldsymbol{p_i}](https://render.githubusercontent.com/render/math?math=%5Cboldsymbol%7Bp_i%7D) and ![\boldsymbol{p_u}](https://render.githubusercontent.com/render/math?math=%5Cboldsymbol%7Bp_u%7D) are learnt by minimising the regularised sum of squares (cost function) as follows:

![\argmin_{\boldsymbol{p},\boldsymbol{q}}\frac{1}{2}\sum_{(u,i)\in k}(r_{ui}-\boldsymbol{q_i}^T\boldsymbol{p_u})^2+\lambda(\sum_u||\boldsymbol{p_u}||^2+\sum_i||\boldsymbol{q_i}||^2)](https://render.githubusercontent.com/render/math?math=%5Cargmin_%7B%5Cboldsymbol%7Bp%7D%2C%5Cboldsymbol%7Bq%7D%7D%5Cfrac%7B1%7D%7B2%7D%5Csum_%7B(u%2Ci)%5Cin%20k%7D(r_%7Bui%7D-%5Cboldsymbol%7Bq_i%7D%5ET%5Cboldsymbol%7Bp_u%7D)%5E2%2B%5Clambda(%5Csum_u%7C%7C%5Cboldsymbol%7Bp_u%7D%7C%7C%5E2%2B%5Csum_i%7C%7C%5Cboldsymbol%7Bq_i%7D%7C%7C%5E2))

where ![\lambda](https://render.githubusercontent.com/render/math?math=%5Clambda) controls the magnitude of regularization and can be optimally found using cross-validation. **Stochastic Gradient Descent** will be used to fit the model to the observed ratings. This is done by looping over all observed ratings in the training set, and updating the parameters by a proportion, known as the learning rate, of the magnitude of the gradient of the surface to be minimised i.e the cost function, but in the opposite direction. This is repeated until convergence is reached. Due to lengthy run-times, the convergence criterion is sometimes set as a fixed maximum number of iterations, but this is not recommended. 

We define the convergence criterion as the iterative difference between the cost function at iteration *j* and *j-1* as follows: 

![\left|f(\boldsymbol{p_u^j},\boldsymbol{q_i^j}) - f(\boldsymbol{p_u^{j-1}},\boldsymbol{q_i^{j-1}}) \right|\leq \epsilon](https://render.githubusercontent.com/render/math?math=%5Cleft%7Cf(%5Cboldsymbol%7Bp_u%5Ej%7D%2C%5Cboldsymbol%7Bq_i%5Ej%7D)%20-%20f(%5Cboldsymbol%7Bp_u%5E%7Bj-1%7D%7D%2C%5Cboldsymbol%7Bq_i%5E%7Bj-1%7D%7D)%20%5Cright%7C%5Cleq%20%5Cepsilon)

## Matrix Factorization with bias terms

A large proportion of the variance in the ratings is associated to the individual effects arising for each user and each item. This can be explained by the fact that some users tend to give higher ratings than others, while some items are more highly rated than others. Thus the introduction of these additional bias terms will try to identify the portion of the ratings arising from the user effect and item effect as well as the resulting underlying user-item interaction effect (![\boldsymbol{q_i}^T\boldsymbol{p}_u](https://render.githubusercontent.com/render/math?math=%5Cboldsymbol%7Bq_i%7D%5ET%5Cboldsymbol%7Bp%7D_u)). 

The predicted ratings are then modelled as:
 ![\hat{r}_{ui} = \mu+ b_i +b_u + \boldsymbol{q_i}^T\boldsymbol{p_u}](https://render.githubusercontent.com/render/math?math=%5Chat%7Br%7D_%7Bui%7D%20%3D%20%5Cmu%2B%20b_i%20%2Bb_u%20%2B%20%5Cboldsymbol%7Bq_i%7D%5ET%5Cboldsymbol%7Bp_u%7D)
 
 where ![\mu](https://render.githubusercontent.com/render/math?math=%5Cmu) represents the overall mean ratings, ![b_i](https://render.githubusercontent.com/render/math?math=b_i) and ![b_u](https://render.githubusercontent.com/render/math?math=b_u) is the effect arising from item *i* and user *u* respectively. 
 
 The new regularised cost function is then defined as:

![\argmin_{\boldsymbol{p},\boldsymbol{q}}\frac{1}{2}\sum_{(u,i)\in k}(r_{ui}-\hat{r}_{ui})^2+\lambda(\sum_u||\boldsymbol{p_u}||^2 + b_u^2+\sum_i||\boldsymbol{q_i}||^2+b_i^2)](https://render.githubusercontent.com/render/math?math=%5Cargmin_%7B%5Cboldsymbol%7Bp%7D%2C%5Cboldsymbol%7Bq%7D%7D%5Cfrac%7B1%7D%7B2%7D%5Csum_%7B(u%2Ci)%5Cin%20k%7D(r_%7Bui%7D-%5Chat%7Br%7D_%7Bui%7D)%5E2%2B%5Clambda(%5Csum_u%7C%7C%5Cboldsymbol%7Bp_u%7D%7C%7C%5E2%20%2B%20b_u%5E2%2B%5Csum_i%7C%7C%5Cboldsymbol%7Bq_i%7D%7C%7C%5E2%2Bb_i%5E2))

Similar optimising and convergence strategies as described for the simple MF model can be applied here. 

# Data

The MovieLens data set comprising of 100836 ratings for 9742 movies by 610 users will be used for this analysis. The data can be downloaded [here](https://grouplens.org/datasets/movielens/latest/). The following data set ([metadata](https://www.kaggle.com/rounakbanik/the-movies-dataset)) consisting of movie features was also used for the purpose of exploration analysis. 

## Data Exploration

Before atempting to fit the model, we take a look at the data to understand what kind of information we are looking at. 

The MovieLens users have rated the movies on a scale from 0.5 (lowest) to 5 (highest). The distribution of the ratings is as follows:

|<img src="/Plots/EDA/Distribution_ratings.png" width="70%" height="50%">|<img src="/Plots/EDA/Ranked_genres_rated.png" width="70%" height="50%">|
|:--:|:--:|
|*Distribution of the ratings*|*Number of movies rated grouped by genres*|

We note that there are more high ratings (>2.5) than low ratings given by the users, while 4 is the most commonly given movie rating. We further see that drama, comedy and action movies are the most watched and rated movies amongst the users. 

The summary statistics - mean and variance (a measure of how far each ratings will deviate from the mean) - of the ratings of each of the genres can be seen below:

<p align="center">
  <img width="30%" height="30%" src="/Plots/EDA/Stats_by_genres.png">
</p>


Despite the fact that Film-Noir, War and Documentary movies are not commonly watched, the latter have the highest mean ratings as well as relatively low variance, which suggests that the smaller number of users who choose to watch this genres of movies generally appreciate them. On the other hand, horror movies have lower ratings *but* a high standard deviation - which implies a lack of consensus amongst viewers. 


Out of curiosity, as movies happen to be a mix of genres and features, I was further interested into discovering how many different kinds of movies there are by taking the movie genres, year of release, movie budget and revenue into consideration. An attempt to cluster movies was thus made. The *Partition around Medoids* (PAM), an algorithm developed to implement *k*-medoids was used by taking into account the features mentioned earlier. The optimal value of *k* was found to be 77 and the different clusters yielded can be visually represented as follows:

<p align="center">
 <img src="/Plots/Clusters/mean_year.png" width="30%" height="30%"><img src="/Plots/Clusters/mean_budget.png" width="30%" height="30%"> <img src="/Plots/Clusters/mean_revenue.png" width="30%" height="30%">
</p>

Interestingly, we  identify clusters of old movies, clusters of newer high revenue generating and high budget movies. We further delve into the prominent genres found in the some of the cluster (left to right; cluster 1, 5, and 40):

<p align="center">
 <img src="/Plots/Clusters/clus1.jpg" width="20%" height="20%"><img src="/Plots/Clusters/clus5.jpg" width="20%" height="20%"><img src="/Plots/Clusters/clus40.jpg" width="20%" height="20%">
</p>

We find that cluster 1 is made up of animated movies mostly suited for children while a cluster of rom-com movies was also formed. Finally, high budget and high revenue action, adventure and Sci-Fi movies were clustered together. Notable movies in that cluster include *Star Wars*, *The Hunger Games*, *Avengers* and *Avatar*.

# Application

Now that we have a good understanding of what the data consists of, we can proceed with the modelling. The regularization parameter is found using cross-validation by fitting the matrix factorization model using stochastic gradient descent (MF-SGD) over a narrow range of ![\lambda](https://render.githubusercontent.com/render/math?math=%5Clambda) values. The results are displayed below

<p align="center">
 <img src="/Plots/MF_SGD/cv_error.png" width="40%" height="40%">
</p>

With the lowest CV error and low standard deviation, we find that ![\lambda=0.002](https://render.githubusercontent.com/render/math?math=%5Clambda%3D0.002) is the optimal regularization parameter value in this case. 

We then proceed by fitting the MF-SGD and MF-SGD including the biased terms, using 2 features and a tolerance level of 0.001. The gradual decrease in mean square error values as the algorithms iterate over the dataset can be seen below. There is strong evidence - flattened curve - that both algorithms have converged. At convergence, both algorithms have low mean square errors on the training set. 

|<img src="/Plots/MF_SGD/mse_trace.png">|<img src="/Plots/Biased_MF_SGD/mse_trace.PNG">|
|:--:|:--:|
|*MF-SGD*|*Biased MF-SGD*|

# Results

The results below display the loadings of each item onto each latent feature and the user weightings onto each feature, respectively. We note a negative linear relationship between the features i.e a high loading on F2 would result on a low loading on F2 for both items, and more strongly for users. As weightings can be seen as the extent of interest into the item, users who load high on F2 will subsequently enjoy movies which also load high on F2.

|<img src="/Plots/MF_SGD/item_feature.png" width="70%" height="50%">|<img src="/Plots/MF_SGD/user_feature.png" width="70%" height="50%">|
|:--:|:--:|
|*MF-SGD : Item features* | *MF-SGD : User features* |

We then decompose the ratings to further include the user and item effects (bias terms) and overall mean , found to be equal to 3.49. We find that the inclusion of the bias terms has removed the previously identified linear relationship *but*  the user and item feature loadings are strongly centered around 0. This suggests a low interaction effect between users and items. 

|<img src="/Plots/Biased_MF_SGD/item_feature.png" width="70%" height="50%"> |<img src="/Plots/Biased_MF_SGD/user_feature.png" width="70%" height="50%">|
|:--:|:--:|
|*Biased MF-SGD : Item features*|*Biased MF-SGD : User features*|

Moreover, we note that the item effect of several movies are low (close to 0), although some movies such as *Troll 2* are more highly rated than others while *Bernie* is not a particularly loved movie. Similar observations can be made for the user bias effect which is close to 0 for many users. Nevertheless we identify some outliers where some users were more generous than others. 

|<img src="/Plots/Biased_MF_SGD/item_effect.png" width="70%" height="50%">|<img src="/Plots/Biased_MF_SGD/user_effect.png" width="70%" height="50%">|
|:--:|:--:|
|*Biased MF-SGD : Item effect*|*Biased MF-SGD : User effect*|

Altogether, the low interaction effects, user and item bias effects for a large proportion of the observed ratings tend to be quite low. As a result, we find that a large proportion of the ratings could be modelled by the overall mean. 


Finally, we obtain the performance of the fitted models on the test data set: 

||Mean Absolute Error|Root mean square error|
| :--: | :--: | :--: |
|MF-SGD|0.737|1.010|
|Biased MF-SGD|0.767|1.120|



# Concluding Remarks

Finally,  both algorithms appear to be performing fairly well, although the biased model is slightly less accurate than the simple model. This might be a case of overfitting which can be further looked into. This project was mostly focussed on implementing the matrix factorization algorithms. More could be done to attempt to improve the performance of this implementation:
- Performing cross validation over a larger range of regularization parameters.
- Finding the optimal number of features, also using cross validation.
- Using a bigger dataset.

