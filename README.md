# NFL Game Spread Betting Model

## Overview and History

Upon learning basic machine learning tactics around the same time I was first exposed to sports gambling, I set out to accomplish what I have wanted to do my entire life: make money off of my sports knowledge and my natural affinity for numbers. Currently, this goal has not yet been realized as my model is far from perfect. However, the aim has and will always stay the same. I will continue to scour the internet for more data, dive deeper into new advanced algorithms, and tweak my model until it is perfect. Vegas will not best me in the long run.


## Data Acquisition and Preprocessing
This is the section where the brunt of my work has taken place thus far. And it has come a long way from when I started.

#### Iteration One
My first batch of data came in csv titled spreadspoke_scores and contained data on every NFL game dating back to 1966, and this was quickly supplemented by a file named nfl_elo which contained the ELO of NFL teams dating back to 1920. ELO is a common measurement used across many sports to determine the general strength of an entity in that sport. After cleaning, training and testing this data, I found little success in creating a model that was useful for monetary prediction.

#### Iteration Two
After initial models showed disappointing results, I believed the solution lie in gathering more data. I couldn't find any additional existing datasets that gave me more data on a game by game basis. After hitting this brick wall, I thought I was stuck. That's when I stumbled upon https://www.teamrankings.com/nfl/, a treasure trove of NFL week by week data. I got to work understanding how to scrape this data and the rest is history. Using this approach, I was able to add variables relating to the site's predictive rating, home field rating, away field rating, margin of victory, redzone percentage, points per game, and yards per play.

## Model Making
With such a large set of data, I believed the model making process would come with ease. I first used MinMaxScaler to standardize the data. Then, to make the number of variables more reasonable, I used recursive feature elimination to find the most indicative variables. I then used a loop to fit the data to four different types of regression models:  k-Nearest Neighbor, Gaussian Naive Bayes, Decision Tree, Support Vector Machine, and Logistic Regression. For each of these models, I trained and tested the data and looked at metrics including precision, recall, f1-score and support. To date, no model has met my expectations.

## Inspirations
My initial dataset and variables for my model are inspired by Ty Walters' project (https://github.com/TyWalters/NFL-Prediction-Model). He did a great job cleaning the data and prepping it for use as well. Check out his project on github.

## Design Choices and Challenges
The current design of the model is naive approach. The model is designed to predicts whether the favorite or underdog will cover the spread. However, in the future, I would like to explore other approaches:
- Model predicts margin of victory to reasonable accuracy. If the margin of victory is bigger than spread, then favorites will cover. If the margin of victory is significantly greater than spread, confidence in the favorite is greater. Can be accomplished by model predicting the score of each team.
- Model predicts the percent chance the favorite will cover. 
- Model predicts the percent chance the favorite/underdog will win. If there is a significant gap between the predicted odds and implied ML odds, bet the ML.

One decision I made was to remove all week 1s and week 17s from data because week 1 can be difficult to predict because there is a lack of relevant information regarding how good a team is and week 17 can be tricky to predict because teams can rest players for the playoffs. On top of this, I chose to only look at games from 2003 onwards because the website I scrape data from only had that data for the 2003 season onwards. Finally, if a game is missing any data, I removed it from the dataset because I want to make sure all the rows can be trained the same way.

The largest challenge I've run into thus far was with the time needed to scrape data from my source. This makes sense because the original script was sending a request to a url for every single game in my dataset and creating a BeautifulSoup object for each game and then parsing the object and finding relevant information. This process took hours on hours to run. After some thought, I came up with an elegant solution. I realized that many of the BeautifulSoup objects that were being created were the same because  at least 4 games are played every single Sunday, meaning for each one of those games the same object could be parsed. My idea was creating dictionary of BeautifulSoup objects for each unique date in dataset, so when I looped through the games, I would not have to make a new BeautifulSoup object for each game, instead finding the object in the dictionary using the date as a key.

