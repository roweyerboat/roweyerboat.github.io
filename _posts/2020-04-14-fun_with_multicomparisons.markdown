---
layout: post
title:      "Fun with Multicomparisons"
date:       2020-04-14 06:24:00 +0000
permalink:  fun_with_multicomparisons
---


Z, T, P, F, chi-square... when it comes to hypothesis testing, we have many different test statistics from which to choose.  If you've taken a high school level statistics course, you most likely have been exposed to Z and T tests.  These are heavily covered in the AP Statistics curriculum, but in case you forgot - here's a brief rundown of how these work.  Z-tests are used when we are testing hypotheses about the population mean when we know the population standard deviation.  We can also use Z-tests when we are testing population proportions.  Most of the time, we don't have access to the populatino standard deviation, so we use a t-test.  Now there are actually two t-tests that we can use.  The first and the common one taught is the student's t-test.  There's a great story about why it's called student's, you can watch a video about it [here](https://www.youtube.com/watch?v=32CuxWdOlow).   The other t-test that can be used when the comparing two means that have different variances (student's t-test assumes that they have the same variance) is called the Welch's t-test.  

However, these tests are not why we are here today.  While these are very important and can help use determine difference between two means, what about if we have more than two means that we want to compare?  For example, say you want to figure out which level of discount influences how much customers purchase in an order at your store, if it does influence at all.  

Here's an example of looking at the average order size and discount levels from the Northwind Company:
![](https://i.imgur.com/fbha0cfl.png?1)

Your first instinct might be to run multiple t-tests comparing each level of discount to compare it with the non-discounted orders to see which ones are statistically significant in their difference from the non-discounted order.  This would be a **BIG** mistake.  What you've stumbled onto unknowingly is the multicomparison problem.  You see, when we run a z or t test, we are looking for a p-value to tell us what is the probability of getting this result given if in our assumption in the null hypothesis is true.  The more times we compare the more likely that we will stumble upon one with a smaller p-value.  Does this mean that we can reject the null?  Not quite.  You see, probability is an interesting thing.  We are measuring the likelihood of an event occurring.  Just because we get a small probability does not mean that alone can lead us to conclude that our alternative hypothesis is true.  
So what can we do you ask?
There are two tests that help work with comparing multiple numerical data at once, the first is the ANOVA test.  This stands for Analysis of Variance.  With this test, we start with a linear model and then compare the variances of each coefficient to see if the probability that each came from the same population or if the differ.
To see an example of this we can look at the ANOVA test I ran to compare Northwinds discounts and if each level of discount influenced the size of the order.
First here's the code and the result of creating a linear model:
![](https://i.imgur.com/OPoygfJl.png)
![](https://i.imgur.com/eUBSde9l.png)

Second we run the ANOVA test using statsmodel library (sm)
![](https://i.imgur.com/dmb6BU5l.png)

We can see from the output that our p-value is large and so even if there's statistical significance between discounted and nondiscounted orders.  There isn't a statistical significance of how much is ordered and  the level of discount.  If we want to investigate further we can run a Tukey test.  This output shows us comparison of each observation with each of the others.
![](https://i.imgur.com/tIXzg9kl.png)

Once again, each comparison fails to show a significant difference in the discount level and the order size. 
So if you find yourself needing to compare multiple observations at once, ANOVA and Tukey are your best friends.  The great news is, the lines of code to run them are very small.  Before performing either of these you will need to have your data be normally distributed, as well as your samples are derived from random sampling that is independent and the variances are the same and you're good to go!

