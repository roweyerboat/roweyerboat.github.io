---
layout: post
title:      "Reality Behind the Data"
date:       2020-05-26 01:50:10 -0400
permalink:  reality_behind_the_data
---


I've officially entered my first data contest!  After spending countless hours cleaning processing the data and then running a wide variety of models, I was still very nervous about actually posting my work so publicly.  I know it's silly because there are thousands of entries and I knew I wouldn't land at the top.  After all, this was my first machine learning project that I have undertaken.  So often I want to fast forward in my data science process to be an expert in the field, but I am learning to embrace where I am in the process.  We all know data science is the continual learning process.  

![](https://i.imgur.com/isJxjrt.jpg)

The contest I entered is Pump It Up: Mining the Water Table
[](https://www.drivendata.org/competitions/7/pump-it-up-data-mining-the-water-table/page/23/).  
It is run by Driven Data.  The task is to create a model that will be able identify whether a well in Tanzania is functional, functional but needs repair, or not functional.  There are a number of features that could be used to create the model.

Initially, I began the task of creating a model that would identify using those three categories: functional, functional needs repair, or not functional.  I was able to create a model using the Random Forest algorithm that resulted in an accuracy of .79. After running several different models and using GridSearchCV to identify the best parameters, this was my best score.  So I took it and posted it to the competition just to see where I would land.

![](https://i.imgur.com/u5PEaBc.png?1)

So, not number 1, but not last either. 

I began to think more deeply about what the data represented: people without access to water.  As I thought through that idea, I looked again at my training data set and that while there were functional wells that needed repair, there were even more wells that weren't functioning. 

![](https://i.imgur.com/jSyZQFp.png)

When considering the need to be able to identify which wells needed attention, I decided to switch gears and change the labeling system from ternary to binary: functioning and needs repair.  Again my best model came from a Random Forest Algorithm.  This model was able to achieve an accuracy rating of .81 after tuning using GridSearchCV. While that .02 percentage of accuracy is not that large, if it means a village that is desparate for a working well gets attention, then every gain makes a difference.  Below is the heatmap of my final model.  

![](https://i.imgur.com/TZBCoJv.png)

It's good to take the time to put myself out there in and see where I land and my opportunity to grow and improve.  Through it all, learning how to improve can have a calcuable impact on the decisions made using data to improve the lives of people.  Keeping that in mind encourages me to continue to learn.



