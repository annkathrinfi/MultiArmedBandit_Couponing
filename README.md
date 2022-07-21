# MultiArmedBandit_Couponing
Final project for Udacity Data Science Nanodegree. Implementation of a coupon recommender system for Starbucks Mobile Rewards App. Applied methods are FunkSVD, k-Means Clustering and Multi-Armed Bandits. 

## **Table of Contents:**
1. [Project Overview](README.md#project-Overview)
2. [Description of Data Sets](README.md#description-of-data-sets)
3. [Libraries used](README.md#libraries-used)
4. [Licensing, Acknowledgements](README.md#licensing-acknowledgements)

##VORLAGE
## **Project Overview**<br/>
In this project I will analyze the interactions that users have with articles on the IBM Watson Studio platform, and make recommendations to them about new articles  they will like. The following steps are included in the code:<br/>

#### I. Data Wangling: <br/>
Data preparation, distribution of user-article-interactions and most viewed articles

#### II. Rank Based Recommendations: <br/>
Find the most popular articles simply based on the most interactions. Since there are no ratings for any of the articles, it is easy to assume the articles with the most interactions are the most popular. These are then the articles could be recommend to new users.

#### III. User-User Based Collaborative Filtering:<br/>
In order to build more personal recommendations for the users of IBM's platform, I match users that are similar in terms of the items they have interacted with. These items are then recommended to the similar users. 

#### IV. Matrix Factorization:<br/>
Finally, I will complete a machine learning approach to building recommendations. Using the user-item interactions, a matrix decomposition is built to get an idea of how well new articles are predicted/recommended for an individual.

## **Description of Data Sets**<br/>

**Introduction:**<br/>
The data set contains simulated data that mimics customer behavior on the Starbucks rewards mobile app. Once every few days, Starbucks sends out an offer to users of the mobile app. An offer can be merely an advertisement for a drink or an actual offer such as a discount or BOGO (buy one get one free). Some users might not receive any offer during certain weeks.<br/>

Not all users receive the same offer. In this project I will combine transaction, demographic and offer data to determine which customer groups respond best to which offer type. The data set is a simplified version of the real Starbucks app because the underlying simulator only has one product whereas Starbucks actually sells dozens of products.<br/>

Every offer has a validity period before the offer expires. As an example, a BOGO offer might be valid for only 5 days. Informational offers have a validity period as well even though these ads are merely providing information about a product; for example, if an informational offer has 7 days of validity, one can assume the customer is feeling the influence of the offer for 7 days after receiving the advertisement.<br/>

The transactional data is showing user purchases made on the app including the timestamp of purchase and the amount of money spent on a purchase. This transactional data also has a record for each offer that a user receives as well as a record for when a user actually views the offer. There are also records for when a user completes an offer. Someone using the app might make a purchase through the app without having received an offer or seen an offer.<br/>

**Example**<br/>
To give an example, a user could receive a discount offer buy 10 dollars get 2 off on Monday. The offer is valid for 10 days from receipt. If the customer accumulates at least 10 dollars in purchases during the validity period, the customer completes the offer.<br/>

However, there are a few things to watch out for in the data set. Customers do not opt into the offers that they receive; in other words, a user can receive an offer, never actually view the offer, and still complete the offer. For example, a user might receive the "buy 10 dollars get 2 dollars off offer", but the user never opens the offer during the 10 day validity period. The customer spends 15 dollars during those ten days. There will be an offer completion record in the data set; however, the customer was not influenced by the offer because the customer never viewed the offer. This makes data cleaning especially important and tricky.<br/>

Some customer groups will make purchases even if they don't receive an offer. From a business perspective, if a customer is going to make a 10 dollar purchase without an offer anyway, we wouldn't want to send a buy 10 dollars get 2 dollars off offer. We'll want to try to assess what a certain customer group will buy when not receiving any offers.<br/>

**The data is contained in three files:**<br/>
<br/>
portfolio.json - containing offer ids and meta data about each offer (duration, type, etc.)<br/>
profile.json - demographic data for each customer<br/>
transcript.json - records for transactions, offers received, offers viewed, and offers completed<br/>
<br/>
Here is the schema and explanation of each variable in the files:<br/>
<br/>
**portfolio.json**<br/>
id (string) - offer id<br/>
offer_type (string) - type of offer ie BOGO, discount, informational<br/>
difficulty (int) - minimum required spend to complete an offer<br/>
reward (int) - reward given for completing an offer<br/>
duration (int) - time for offer to be open, in days<br/>
channels (list of strings)<br/>
<br/>
**profile.json**<br/>
age (int) - age of the customer<br/>
became_member_on (int) - date when customer created an app account<br/>
gender (str) - gender of the customer (note some entries contain 'O' for other rather than M or F)<br/>
id (str) - customer id<br/>
income (float) - customer's income<br/>
<br/>
**transcript.json**<br/>
event (str) - record description (ie transaction, offer received, offer viewed, etc.)<br/>
person (str) - customer id<br/>
time (int) - time in hours since start of test. The data begins at time t=0<br/>
value - (dict of strings) - either an offer id or transaction amount depending on the record<br/>

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

