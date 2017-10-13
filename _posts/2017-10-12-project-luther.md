---
layout: post
title: Predicting Foreign Box Office for Movies
---

## Metis Weeks 2-3

For project #2 at Metis we were supposed to scrape data from the internet and use it to predict something with a linear regression model. I decided to try to predict a movie's total foreign box office, as well as what genres are more popular in certain other countries relative to others.

### Step 1: Adventures with Scrapy

We learned several different methods for web scraping, including `BeautifulSoup`, `Selenium`, and `Scrapy`. I decided to go with `Scrapy`, as it seemed to be more powerful than `BeautifulSoup`, and I wasn't scraping any dynamically generated content that would require `Selenium`. I wrote a script that pulled the following features from [boxofficemojo](http://www.boxofficemojo.com/yearly/) for each movie in the top 200 grossing movies for the years 2010-2016 (1400 movies total):
* Title
* Domestic Total Gross
* Distributor
* Release Date
* Genre
* Runtime
* Budget
* Total Foreign Gross
* Opening Weekend
* Widest Release
* Days In Theaters
* Foreign Countries with Reported Gross
* Gross for each Country

Writing a web scraper was pretty cool! It was a bit tedious troubleshooting all of the XPaths, but once it worked I felt like this:

<iframe src="https://giphy.com/embed/o0vwzuFwCGAFO" width="480" height="480" frameBorder="0" class="giphy-embed" allowFullScreen></iframe><p><a href="https://giphy.com/gifs/cat-hacker-webs-o0vwzuFwCGAFO">via GIPHY</a></p>

### Step 2: Clean Up Clean Up Everybody Everywhere

Sadly boxofficemojo does not include budget for a lot of movies, which mean dropping around half my movies. I could have scraped farther back, but since I was trying to predict foreign gross I didn't want any sort of geopolitical changes to have too much of an effect on my findings. I thought going back to 2010 was fair -- it was post financial crisis, which meant little inflation.

I then wrote a few helper functions to clean up my data, such as functions that converted strings to money (ints and floats), converted strings to datetime objects, and organized genres from around 100 different values into 9 broader categories.

### Step 3: Explore!

This is a pairplot of all of the numerical variables:

![pairplot]({{ site.baseurl }}/images/pairplotluther.png)

A few takeaways / issues:

1. The money variables are not normally distributed
2. Domestic gross and opening weekend are highly collinear
3. Runtime has almost no relationship with anything
4. The number of theaters and any of the gross variables have a sort of exponential relationship

How I decided to deal with these issues:

1. I did a whole separate analysis after log transforming these variables, and while this was probably more "academically" correct (fewer linear regresssion assumptions were violated), I was more focused on the interpretation of my model, so keeping everything as-is was easier to interpret. Plus, I only had money variables in the end, so at least they still varied together
2. I decided not to use domestic gross in my model. If the below is the trajectory of a movie, my cutoff point for prediction was the red line. Basically I didn't think it was fair to use total domestic gross and other things to predict total foreign gross when you couldn't know total domestic gross before the movie was released
![movie-diagram]({{ site.baseurl }}/images/movie-diagram.png)

3. I decided not to use runtime as a feature
4. The number of theaters got eliminated from my feature set because of #2 above, but I thought this relationship was interesting nonetheless. It suggests that movies that are released to fewer than around 3,000 theaters make roughly the same amount of money, but once you hit that 3,000 theater threshold movies start to make more money. I also found it interesting that there didn't seem to be any diminishing returns on the number of theaters, I guess because movie producers already know the optimal number of theaters in which to release their films!

### Step 4: Feature Selection

My initial set of features were budget, opening weekend, genre (8 categories), distributor (9 categories, i.e., Disney/Buena Vista, Universal), MPAA rating (G, PG, PG-13, R), and month of release. I ran an OLS model on all of the features and got a cross-validated \\( R^2 )\\  
