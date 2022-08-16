# Building a Coupon Recommender System with Multi-Armed Bandits
Final project for Udacity Data Science Nanodegree. Implementation of a coupon recommender system for Starbucks Mobile Rewards App. Applied methods are FunkSVD, k-Means Clustering and Multi-Armed Bandits. 

## **Table of Contents:**
1. [Project Overview](README.md#project-Overview)
2. [Description of Data Sets](README.md#description-of-data-sets)
3. [Libraries used](README.md#libraries-used)
4. [Licensing, Acknowledgements](README.md#licensing-acknowledgements)

## **Project Overview**<br/>
In this project I will analyze the purchases that Starbucks' customers make with and without offers sent by the Mobile Rewards App. Customers will be clustered based on their purchase behavior and a recommender system will be built, which sends offers to the customers that should optimize their performance. The following steps are included in the code:

#### I. Data Wrangling: <br/>
The challenge was that the transcript data set creates a new row for each interaction. Consequently, the first row would show the time a customer received an offer. The next row would show the time when a customer viewed this or any other offer and the next row would show either the completion of an offer or a regular transaction. Furthermore, each customer could receive each offer multiple times or comlete an offer before he has seen it.<br/> 
The goal of the data wrangling part was to create a master dataframe (named 'df_mab'), which shows the time and amount of each purchase as well as the time of contact, view, and completion of the offer that influenced the purchase in a single row. Also, the master dataframe contains purchases without offer infuence and offers that were not completed.<br/> 
Additionally, a customer dataframe was created (named 'df_member'), containg each unique customer with aggregated values regarding his purchase behavior, such as the number of purchases and the amount spent overall, the number of purchases that are influenced by an offer, the number of viewed, completed and not completet offers etc. Most members made five to ten purchases and completed one or two offers. Members with a higher number of purchases often have more completed offers. BOGO offers are more likely to not be completed, whereas discount offers are the type that is completed most often and is less likely not to be completed than BOGO offers.

#### II. Exploratory Analysis: <br/>
The data holds a total of 17.000 customers and 10 offers. Transactions without an offer influence get the offer name “No_offer” and will be modeled together with the official offers. This way, customers that perform just as good without an offer can be identified, preventing to send out offers where they are not necessary.

#### III. FunkSVD:<br/>
Creating a user-by-item matrix with a row for each user, a column for each offer and the mean performance of each user when makeing a transaction with the offer. Transactions without an offer influence have the offer name "No_offer". Performing matrix factorization using a basic form of FunkSVD with no regularization and calculating the dot-product of user matrix and offer matrix, to predict missing values. The dot-products of each user-offer combination will be used to build customer clusters in the following step.

#### IV. K-means Clustering:<br/>
After merging variables like the number of purchases, median amount, number of completed offers and the days since registration to the dot-products of each user-offer combination, the number of clusters is selected with silhouette analysis (https://scikit-learn.org/stable/auto_examples/cluster/plot_kmeans_silhouette_analysis.html). For 5-7 clusters the silhouette score is similarly high. The K-mean clustering is performed with 5 clusters and the mean reward for each cluster is calculated.

#### V. Multi-Armed Bandits: <br/>
Each cluster is considered as an agent and each offer is the arm of a slot machine. For this project, the the e-greedy-decay algorithm is used, which explores more in the begining and then slowly starts exploiting the most rewarding arm. However, exploration never reaches zero to assure a minimum of variance in the offers made to the customer clusters.

## **Description of Data Sets**<br/>

**Introduction:**<br/>
The data set contains simulated data that mimics customer behavior on the Starbucks rewards mobile app. Once every few days, Starbucks sends out an offer to users of the mobile app. An offer can be merely an advertisement for a drink or an actual offer such as a discount or BOGO (buy one get one free). Some users might not receive any offer during certain weeks.<br/>

Not all users receive the same offer. In this project I will combine transaction, demographic and offer data to determine which customer groups respond best to which offer type. The data set is a simplified version of the real Starbucks app because the underlying simulator only has one product whereas Starbucks actually sells dozens of products.<br/>

Every offer has a validity period before the offer expires. As an example, a BOGO offer might be valid for only 5 days. Informational offers have a validity period as well even though these ads are merely providing information about a product; for example, if an informational offer has 7 days of validity, one can assume the customer is feeling the influence of the offer for 7 days after receiving the advertisement.<br/>

The transactional data is showing user purchases made on the app including the timestamp of purchase and the amount of money spent on a purchase. This transactional data also has a record for each offer that a user receives as well as a record for when a user actually views the offer. There are also records for when a user completes an offer. Someone using the app might make a purchase through the app without having received an offer or seen an offer.<br/>

**Example:**<br/>
To give an example, a user could receive a discount offer buy 10 dollars get 2 off on Monday. The offer is valid for 10 days from receipt. If the customer accumulates at least 10 dollars in purchases during the validity period, the customer completes the offer.<br/>

However, there are a few things to watch out for in the data set. Customers do not opt into the offers that they receive; in other words, a user can receive an offer, never actually view the offer, and still complete the offer. For example, a user might receive the "buy 10 dollars get 2 dollars off offer", but the user never opens the offer during the 10 day validity period. The customer spends 15 dollars during those ten days. There will be an offer completion record in the data set; however, the customer was not influenced by the offer because the customer never viewed the offer. This makes data cleaning especially important and tricky.<br/>

Some customer groups will make purchases even if they don't receive an offer. From a business perspective, if a customer is going to make a 10 dollar purchase without an offer anyway, we wouldn't want to send a buy 10 dollars get 2 dollars off offer. We'll want to try to assess what a certain customer group will buy when not receiving any offers.<br/>

**Three files were provided:**<br/>
portfolio.json - containing offer ids and meta data about each offer (duration, type, etc.)<br/>
profile.json - demographic data for each customer<br/>
transcript.json - records for transactions, offers received, offers viewed, and offers completed<br/>
<br/>
**Two files werer created in Part I: Data Wrangling:**<br/>
df_mab.csv - contains the purchases of each member with a flag if an offer was used for the purchase or not, as well as offers that were not completed<br/>
df_member.csv - contains unique members with aggregated KPI's regarding his purchase behavior<br/>
<br/>
Here is the schema and explanation of each variable in the files:<br/>
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
<br/>
**df_mab.csv**<br/>
member_id (string) - customer id <br/>
time (int) - hour of the transaction. The data begins at time t=0<br/>
amount (float) - amount of the purchase. Is 0 when an offer was not completed.<br/>
offer_name (string) - 'No_offer' for transactions without offer influence.<br/>
viewed (float) - 1 if the offer was viewed else 0.<br/>
completed (float) - 1 if the offer was completed after beeing viewed else 0.<br/>
performance (float) - <br/>
<br/>
**df_member.csv**<br/>
member_id (string) - unique customer id <br/>
nr_purch (float) - number of purchases made by the customer<br/>
nr_purch_offer (float) - number of purchases that where influenced by an offer<br/>
compl_offers (float) - number of completed offers (BOGO-offers could influence more than one purchase but are only completed once)<br/>
amount_sum (float) - total spent amount during the comlete testing period<br/>
amount_median (float) - median spent amount during the comlete testing period<br/>
amount_std (float) - standard deviation of spent amount during the comlete testing period<br/>
mean_hrs_compl (flaot) - mean hours between view and completion of an offer (if the offer was completed after beeing viewed)<br/>
nr_NUO (float) - number of not-used-offers<br/>
days_reg (int) - days since the registration (with today beeing the maximum registration date +1)<br/>

## **Libraries Used**<br/>
Following libraries were used:<br/>
- Pandas<br/>
- Progressbar<br/>
- Seaborn<br/>
- scikit-learn<br/>

## **Licensing, Acknowledgements**<br/>
Thanks to Starbucks for providing the data.<br/>
Thanks to Udacity for providing knowledge on Data Science and a platform to work on this project.<br/>

