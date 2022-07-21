# MultiArmedBandit_Couponing
Final project for Udacity Data Science Nanodegree. Implementation of a coupon recommender system for Starbucks Mobile Rewards App. Applied methods are FunkSVD, k-Means Clustering and Multi-Armed Bandits. 

## **Table of Contents:**
1. [Project Overview](README.md#project-Overview)
2. [File Description](README.md#file-description)
3. [Libraries used](README.md#libraries-used)
4. [Licensing, Acknowledgements](README.md#licensing-acknowledgements)

##VORLAGE
## **Project Overview**<br/>
In this project I will analyze the interactions that users have with articles on the IBM Watson Studio platform, and make recommendations to them about new articles  they will like. The following steps are included in the code:<br/>

#### I. Exploratory Data Analysis: <br/>
Data preparation, distribution of user-article-interactions and most viewed articles

#### II. Rank Based Recommendations: <br/>
Find the most popular articles simply based on the most interactions. Since there are no ratings for any of the articles, it is easy to assume the articles with the most interactions are the most popular. These are then the articles could be recommend to new users.

#### III. User-User Based Collaborative Filtering:<br/>
In order to build more personal recommendations for the users of IBM's platform, I match users that are similar in terms of the items they have interacted with. These items are then recommended to the similar users. 

#### IV. Matrix Factorization:<br/>
Finally, I will complete a machine learning approach to building recommendations. Using the user-item interactions, a matrix decomposition is built to get an idea of how well new articles are predicted/recommended for an individual.

## **File Description**<br/>
The following files store the data used in this project:<br/>
1) user-item-interactions.csv: file contains user interaction. <br/>
2) articles_community.csv: file contains articles description. <br/>

## **Libraries Used**<br/>
Following libraries were used:<br/>
- Nltk<br/>
- Pandas<br/>
- Progressbar<br/>
- Seaborn<br/>
- scikit-learn<br/>

## **Licensing, Acknowledgements**<br/>
Thanks to IBM for providing the data.<br/>
Thanks to Udacity for providing knowledge on Recommendation Engines and a platform to work on this project.<br/>

