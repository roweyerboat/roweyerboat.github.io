---
layout: post
title:      "Cleaning Geographical Data"
date:       2020-02-26 06:03:56 +0000
permalink:  cleaning_geographical_data
---


While working on a project that involved looking at King County housing data and working on creating a model to predict pricing, I was intrigued during the data cleaning process.  Since I live in King County, I was interested in the data because I see the housing costs around me.  When examining the data, I found that the column indicating whether a property was waterfront property or not had many Nan entries. 
![](https://i.imgur.com/yKqOzZT.png)
I tried a couple different approaches to figure out how to deal with these unknown values.  Knowing more about King County, when I checked to see how many properties were listed as waterfront, it seemed a bit low given how much water exists in this county.
![](https://i.imgur.com/UUIxyX1.png)
The first was to look at the houses on a map and see if it was easy to designate waterfront properties based on their latitude and longitude.  I used geomapping to display both the waterfront and non-waterfront properites. 
I was able to plot the properties that had the waterfront column filled with either 1 or 0, but I couldn't figure out how to plot the properties that had Nan entered in the waterfront column.
![](https://i.imgur.com/peG6VvZ.png)
Even without seeing the missing data, it became very evident that there were many properties whose latitude and longitude were close together and could either be waterfront property or not.  Therefore it didn't seem very easy to be able to distinguish waterfront solely based on latitude and longitude.

However, I did enjoy the process of learning how to plot the data geographically.  I know I could extend it by adding in the actual King County map, but am still learning how to do that.  I enjoyed the fact that the data looks like King County as I know it and I could easily identify the various bodies of water that exist in the county.



