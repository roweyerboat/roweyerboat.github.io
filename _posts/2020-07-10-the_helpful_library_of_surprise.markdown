---
layout: post
title:      "The Helpful Library of Surprise"
date:       2020-07-10 19:29:29 +0000
permalink:  the_helpful_library_of_surprise
---


When attempted to build a recommendation system, there are a lot of things to consider.  When I took on the project to build a system for the famous [MovieLens](https://grouplens.org/datasets/movielens/) I first began with sparse matrices and pivot tables.  I knew to create a recommendation system, I would need to utilize one of these algorithms to be able to make a system to predict which movies the user would enjoy watching.  
* SVD
* k-NN Baseline
* k-NN Basic

After using a sparse matrix and being able to fit the data, I decided to check out the library [surprise](http://surpriselib.com/) as it was built with the purpose of creating recommendation systems.  

With just a few lines of code I was able to bring in the data, fit various models and check the results.

The import of the library is very straightforward:
```
from surprise import Reader, Dataset
from surprise.model_selection import cross_validate
from surprise.prediction_algorithms import SVD, NormalPredictor
from surprise.prediction_algorithms import KNNWithMeans, KNNBasic, KNNBaseline
from surprise.model_selection import GridSearchCV
from surprise.model_selection import train_test_split
from surprise import accuracy
```

After bringing in the library you then can load your dataset.  In my case I'm using the csv's provided by MovieLens and relying on the ratings.csv and movies.csv to provide the data that I need for my model.  

To begin with the surprise library you first initiate two classes - Reader and Dataset:
```
reader = Reader()
data = Dataset.load_from_df(dataframe_you_wish_to_use, reader)
```

These two classes set your data up to be read as a sparse matrix with the movies that users haven't rated seen as null and you're still able to run algorithms even with large amounts of missing data.

Next you can create a train and test set using similar code to what you would use when training other sets.  In this case we aren't looking for a relationship between 2 variables so there's no need for an x_train, y_train, x_test, y_test variables.  The code needed just takes two input values.

```
trainset, testset = train_test_split(data, test_size=0.25)
```

Surprise also has the capability of running GridSearch which helps tune your parameters for the different algorithms you are choosing from.  It runs just as it does from the sklearn library where you input the algorithm, the parameters you're looking to tune, the measures or measures of success you would like to be reported, and how many cross validation folds you would like to be performed.

```
gs = GridSearchCV(algorithm, parameters, measures=['rmse'], cv=3)
gs.fit(data)
print(gs.best_score)
print(gs.best_params)
```

These lines will give you the output of what you should tune your parameters to and the best RMSE score (or other measure that you insert here).
To fit your data to your tuned algorithm it is simply:
```
svdtuned = SVD(n_factors=100, n_epochs=30,                          lr_all=0.01, reg_all=0.1)
svdtuned.fit(trainset)
svd_preds = svdtuned.test(testset)
accuracy.rmse(svd_preds)
```

Just with these simple lines that are slightly different from the sklearn library - fit instead of fit_transform and test instead of predict - you're able to fit your data to the algorithm and see how well it predicts what a user would prefer.

The [surprise library ](http://surpriselib.com/)is worth checking out as you're creating a recommendation system.  It is very intuitive and the documentation is written for easy implementation.  

