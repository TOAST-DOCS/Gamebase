## Game > Gamebase > Console Guide > Analytics

Status of app users and sales indicators are available on tables or graphs. 
Analytics is composed of the following: 

* Real-Time Monitoring: Real-time concurrent access and purchase indicators of app users 
* User Indicators: Basic indicators (e.g. DAU, MCU, and NRU) for app users and other indicators, including environment, inflow/outflow, and retention 
* Sales Indicators: Indicators related to sales of app 
* Group Concurrence: Concurrent group access to projects of a Gamebase user, as well as basic indicators of each group  
* Service Environment: Statistical indicators for the call of installation URL 

## Real-time Monitoring 
### Real-time Concurrence 

Real-time concurrence indicators, as well as maintenance and push information of current app users are available.  

![gamebase_analytics_01_201901_2](https://static.toastoven.net/prod_gamebase/gamebase_analytics_01_201901_2.png)

#### 1. Change Graphs of Real-Time Concurrent Users 
Data is updated every minute to check changed indicators in real time. 

* Concurrent Users (CCU): Number of real-time concurrent users measured every minute  (number of logged-in users).
* Purchase Amount: Sum of amount paid by Gamebase users around the clock on the day, which includes no refund or cancellation. 
* Newly Registered Users (NRU): Users who are newly registered and whose initial login logs are collected around the clock of the day (by member number).  

#### 2. Maintenance Information
Maintenance information registered at Gamebase around the clock of the day, to check rise or all of concurrent users after maintenance. 

#### 3. Push Information 
Push information delivered to Gamebase around the clock of the day, to check rise or fall of concurrent users after delivery. 

### Dashboard

A variety of user indicators can be easily noticed in real time. 

![gamebase_analytics_02_201901_2](https://static.toastoven.net/prod_gamebase/gamebase_analytics_02_201901_2.png)

#### 1. Dashboard for Real-Time User Status 
App users and payment indicators are available. 
Data is updated every 10 minutes for today, or a selected date's indicators are available.   

* Concurrent Users (CCU): Number of real-time concurrent users measured every 10 minutes (number of logged-in users) 
* Maximum Concurrent Users (MCU): Number of maximum concurrent users around the clock (the largest CCU value of every minute) 
* Newly Registered Users (NRU): New subscribers whose first login logs are collected around the clock of the day (by member number) 
* Daily Active Users (DAU): Number of daily active users, by member number, logged in more than once 
* Average Play Time: Average total play time on each day (sum of play time of DAU/DAU)
* Accumulated Users: Number of all accumulated subscribed users (by member number) since Gamebase is installed 
* Purchase Amount: Total amount of user purchases around the clock 
* Purchase Count: Total number of user purchases around the clock 
* Paying Users (PU): Users (game users) who pay for products (=renewed PU +new PU) 
* New PU (New Paying Users, NPU): First-time paying users
* Average Revenue Per Paid User (ARPPU): Average amount paid by a paying user (=purchase amount/PU) 
* Average Revenue Per User (ARPU): Average amount paid by a daily active user (=purchase amount/DAU) 

※ In the case of MCU, ACU, and ARPU, the filter must be all. 

#### 2. Real-Time Graphs of Change for Major Indicators 
Changes in Concurrent Users, Newly Registered Users, Purchase Amount, and PU Indicators are available every 10 minutes. 

#### 3. Status of Real-Time Share
Share by each OS, app version, store or country, is available on a graph: shows by CCU for today, or DAU for the previous day. 

* OS Share: Share of DAU at each OS (by CCU for the day) 
* App Version Share: Share of DAU at each app version (by CCU for the day)
* Store Share: Share of DAU at each store (by CCU for the day)
* Country Share: Share of DAU at each country (by CCU for the day)

## User Indicators
### Users 

Basic user indicators are available. 

![gamebase_analytics_03_201901_2](https://static.toastoven.net/prod_gamebase/gamebase_analytics_03_202212_1.png)

#### 1. User Status 
Basic user indicators during a selected period are available. 

* Cumulative DAU: Sum of daily active users, by member number, who are logged in more than once a day
* Cumulative WAU: Sum of weekly active users. With weekly indicators, WAU items are substituted for DAU items.  
* Cumulative MAU: Sum of monthly active users. With monthly indicators, MAU items are substituted for DAU items. 
* Maximum Concurrent Users (MCU): The maximum number of concurrent users around the clock. The largest CCU value of each minute is collected on a daily basis. 
* Newly Registered Users (NRU): New subscribers whose first login logs are collected around the clock of the day (by member number)
* Withdrawer: Number of users whose member numbers are deleted around the clock.  
* Average CCU: Average of CCU during a selected period. 
* Avg.Playtime(/DAU) : Average play time during query period (sum of play time of DAU/DAU) 

#### 2. Daily Indicators
Shows basic indicators for daily users during selected period on graphs or tables. 

※ In the case of MCU and ACU, the filter must be all. 

### Service Environment 
![gamebase_analytics_04_201901_2](https://static.toastoven.net/prod_gamebase/gamebase_analytics_04_201901_2.png)

User indicators are available on each environment. 

* Query Conditions 
    * OS: Mobile operating software, such as Android or iOS 
    * Country: Country set on a user's mobile device
    * Store: IdP: User's IdP login/authentication information, like Facebook or Google. 
    * App Version: Version information of an executing app  
    * Device: Type of a user device. With a device selected, PU and purchase amount are not provided (as well as weekly nor monthly data)
* Query Values
    * Daily Active Users (DAU): Number of daily active users, by member number, who are logged in more than once a day 
    * Newly Registered Users (NRU): New subscribers whose first login logs are collected around the clock of the day (by member number)
    * Paying Users (PU): Users (game users) who pay for products (=renewed PU +New PU)
    * Purchase Amount: Total amount paid by a user  

### User Inflow and Outflow  

Inflow and outflow of app users are available on a daily basis. 

![gamebase_analytics_05_201901_2](https://static.toastoven.net/prod_gamebase/gamebase_analytics_05_201901_2.png)

* User Inflow (new + returned): User inflow is the combination of new and returned users (=new users + returned users)
* Newly Registered Users: Newly registered users whose first login logs are collected around the clock of the day (by member number)
* Returned Users: Users whose logs are not collected during the previous 8 days 
* User Outflow (withdrawn + inactive): User outflow is the combination of withdrawn and inactive users (=withdrawn users + inactive users)
* Withdrawn Users: Users who have withdrawn, whose member numbers have been deleted around the clock of the day
* Inactive Users: Users whose logs are not collected during the previous 7 days 

### Retention
![gamebase_analytics_06_202107_2](https://static.toastoven.net/prod_gamebase/gamebase_analytics_06_202107_2.png)

Retention refers to how many subscribed users remain in service for the next 180 days after subscribed. 

You can check the retention data either by including or excluding users who subscribe and withdraw on the same day.

* Exclusion of withdrawer on the same day of subscription: Calculate by excluding those users who subscribed and withdrew on the same day 
    * New Users = Subscriber - Withdrawer on the same day of subscription
      e.g.) Out of 100 new users on January 1st, 20 withdrew on January 1st: then, the number of actual new users is calculated at 80 (100-20).
      
### LTV
![gamebase_analytics_06_201912_1_ltv](https://static.toastoven.net/prod_gamebase/gamebase_analytics_06_201912_1_ltv.png)

LTV is an index representing the expected annual revenue from a single user in the selected user group.

LTV chart is provided per country/OS/date, and in the table below, you can see the details such as LTV, accumulated NRU, accumulated PU, accumulated amount of payment, etc.

#### Estimation method
As LTV estimation method, Gamebase uses the ARPU accumulated over 365 days after the registration. 

#### User group conditions
The user group conditions are as follows:

* Signed-up date
* Country
* OS

#### Restriction conditions
The following restrictions are applied for accurate estimation of LTV:

* The number of user group members must be 1,000 or more.
* The number of PU (Payment Users) of the user group must be 30 or more.
* The most recent signed-up date must be more than 7 days.

### Life Cycle
![gamebase_analytics_06_202002_1_lifeCycle](https://static.toastoven.net/prod_gamebase/gamebase_analytics_06_202002_1_lifeCycle.png)

Life Cycle is an index used to check the trend of daily active users since the first inflow of users. Data is retained for up to 3 years.

* Daily Active Users (DAU): The number of active users who logged in at least once on a daily basis based on memberno
* Maximum Concurrent Users (MCU): The number of concurrent users during the period between 0:00 and 24:00. The highest value among the CCU values every minute is aggregated on a daily basis
* Newly Registered Users (NRU): Newly registered users. The user whose login log is collected for the first time between 0:00 and 24:00 (based on memberno)
* Withdrawn Users: Users who withdrew their account. Users whose memberno is deleted during the period between 00:00 and 24:00
* Users who registered and withdrew on the same day: Users who deleted their account on the same day they signed up for the service
* Average CCU: The average CCU during the selected duration
* Average playtime - Avg.Playtime(/DAU): The average playtime during the retrieved duration (Sum of playtime of DAU/DAU)      

### Frequency7

![gamebase_analytics_06_202003_1_frequency](https://static.toastoven.net/prod_gamebase/gamebase_analytics_06_202003_1_frequency.png)

The Frequency7 index provides information about weekly visitor and ratio of DAU. It can be used to see immersion, loyalty, and other information at a glance.

Frequency7 is divided into the following three categories:

* Number of visits: The number of visits in 7 days
* Number of consecutive visits: The number of consecutive visits in 7 days (including the date)
* Number of maximum consecutive visits: The number of maximum consecutive visits in 7 days

Let's see an example of the three calculation models mentioned above: 
If there is a user who visited the site on March 1, 2, 3, 6, and 7, as of March 7, the number of visits is as follows:

* Total number of visits: 5 days (March 1, 2, 3, 6, and 7)
* Number of consecutive visits: 2 days (March 6-7)
* Number of maximum consecutive visits: 3 days (March 1, 2, and 3)

## Sales Indicators 
### Purchase Amount 

Shows indicators of purchase amount.   

![gamebase_analytics_07_201901_2](https://static.toastoven.net/prod_gamebase/gamebase_analytics_07_202212_1.png)

#### 1.Status Table for Purchase Amount 
Purchase amount during a specific period can be found. 
Total amount, as well as purchase amount by country of major stores are available.  

#### 2. Sales Trend  
New sales, renewed sales, and trends of paying users are displayed on graphs.
Sales by store, country, and Idp are also available on the below table. 
Monthly cumulative purchase amount can only be checked on daily view

### Paying Users 
![gamebase_analytics_08_201901_2](https://static.toastoven.net/prod_gamebase/gamebase_analytics_08_202212_1.png)

Indicators of paying users are displayed. 
Refer to the following glossaries:

* Purchase Amount: Amount of purchase paid by a user 
* New Purchase Amount: Amount of purchase paid by a newly paying user 
* Daily Active Users (DAU): Number of daily active users, logged in more than once 
* Paying Users (PU): Users who pay for products (= renewed PU + new PU)
* New PU (New Paying Users, NPU): First-time paying users
* Renewed PU: Accumulated  PU - New PU (calculated on a daily basis, as of the previous day)
* PUR: Rate of paying users (PU/DAU * 100)
* ARPU: Average purchase amount paid by the number of game users for a day (purchase amount/DAU) 
* ARPPU: Average purchase amount paid by the number of paying users (purchase amount/PU)
* ARPNPU: Average purchase amount paid by new paying users (purchase amount/NPU)
* Cumulative PU(M): Number of paying users on a monthly basis (duplicates excluded)

### Item Sales Indicators 

Sales indicators of Gamebase items are available. 

![gamebase_analytics_09_201901_2](https://static.toastoven.net/prod_gamebase/gamebase_analytics_09_202212_1.png)

* Item: List of items registered at Gamebase 
* 10 Best-Selling Items: List of 10 most selling items by the price or number of sales 
* Store: Stores, such as AppStore or Google Play
* Purchase Amount: Amount paid by users for each item  
* Purchase Count: Number of purchases for each item 
* PU: Number of paying users for each item
* Payment Ratio: Rate of purchase for each item 

### First Purchase
![gamebase_analytics_10_201901_2](https://static.toastoven.net/prod_gamebase/gamebase_analytics_10_202212_1.png)

First-purchase information of newly paying users can be found. 

Shows how long it takes for new paying users to make the first purchase from day 0 to day 90.

All purchased items are displayed in the order of purchase amount. 

* Item: List of items purchased by new paying users 
* Store: Stores, such as AppStore or Google Play 
* New PU (New Paying Users, NPU):  First-time paying users
* New Purchase Amount: Amount paid by new paying users 

## Group Concurrence   
### Concurrent Group Users 

 Indicators of concurrent users for all Gamebase projects are available.  

![gamebase_analytics_11_201901_2](https://static.toastoven.net/prod_gamebase/gamebase_analytics_11_201901_2.png)

* Real-Time Group Concurrence : Shows real-time concurrent users (CCU) of a Gamebase project. 
* Project Group Concurrence : Shows app users by selected period or filter.

### Group Comparison Indicators 

Projects of Gamebase users can be filtered and compared by group. ![gamebase_analytics_12_201901_2](https://static.toastoven.net/prod_gamebase/gamebase_analytics_12_201901_2.png)



* DAU: Daily active users, by member number, logged in more than once 
* NRU: Newly Registered Users of the day 
* PU: Paying Users (=Renewed PU + New PU)
* Purchase Amount: Amount paid by user

※ Group is named such as **{appId} _ {OS} _ {Country}** on a graph.

## Environment 
### Installation URL  

Statistical indicators for an installation URL call are available.  

![gamebase_analytics_13_201901_2](https://static.toastoven.net/prod_gamebase/gamebase_analytics_13_201901_2.png)

* Download URL Call Counts of the Recent Week: Number of clients who call API for game installation via [App > Installation URL]. You can find customer responses, as part of marketing for app installation, by measuring the number of users who call short URL. 
* Share by Browser (all accumulated): Shows the rate of URL call counts on each browser. 
* Share by Platform (all accumulated): Shows the rate of installation URL call counts on each platform 

## Transmission

The **Transmission Indicator** tab is available when indicators are sent via API on a game. 
There are three types of transfer indicators as below: 

* User Level: Access and sales data are available by user level. 
* World/Server/Channel: Access by world/server/channel and sales data are available with user ID. 
* Class/Occupation: Access and sales data are available by class/occupation. 

> [Note] 
>
> World/server/channel, class/profession are processed only pre-registered information.
> See the following document to learn how to register.
>
> - [App > Analytics Indicator](./oper-app/#analytics-indicator)

### Concurrent Status

You can find each type of selected transfer indicators, as well as access and sales information on a particular date.  
Concurrent access is available via CCU for the day, or DAU on each date. 
Information is updated at every 10 minutes for the day.  

![gamebase_analytics_14_201906_1](https://static.toastoven.net/prod_gamebase/gamebase_analytics_14_201906_1.png)

* CCU (Concurrent User): Concurrent real-time users measured at every minute (number of login users) 
* DAU (Daily Active User): Active users, who log in more than once
* NRU (Newly Registered User) :Users who are newly registered on the day
* Purchase Count : Number of paid product purchases
* Purchase Amount: Purchase amount of paid products 

### Status by Level

Access and sales status are available at each level.

![gamebase_analytics_15_201906_1](https://static.toastoven.net/prod_gamebase/gamebase_analytics_15_201906_1.png)

* DAU (Daily Active User): Daily active users, by user ID, who log in more than once 
* Avg.Playtime: Average total play time on each day of the level (sum of playtime of DAU/DAU)
* Purchase Amount: Purchase amount of paid products  
* New Purchase: Purchase amount by new paying users (NPU)
* Repeat Purchase: Purchase amount by repeat paying users
* PU (Paying User): Users who purchase paid products (= repeat PU + new PU)
* New PU: First-time paying user
* Repeat PU: Total PU - New PU (calculated upon the previous day on a daily basis)

### Status by Channel

Access and sales status are available by world/server/channel. 

![gamebase_analytics_16_201906_1](https://static.toastoven.net/prod_gamebase/gamebase_analytics_16_201906_1.png)


* DAU (Daily Active User): Daily active users, by user ID, who log in more than once 
* Avg.Playtime: Average total play time on each day of the level (sum of playtime of DAU/DAU)
* Purchase Amount: Purchase amount of paid products  
* New Purchase: Purchase amount by new paying users (NPU)
* Repeat Purchase: Purchase amount by repeat paying users
* PU (Paying User): Users who purchase paid products (= repeat PU + new PU)
* New PU: First-time paying user
* Repeat PU: Total PU - New PU (calculated upon the previous day on a daily basis)

### Status by Class

Access and sales status are available by class/occupation. 

![gamebase_analytics_17_201906_1](https://static.toastoven.net/prod_gamebase/gamebase_analytics_17_201906_1.png)

* DAU (Daily Active User): Daily active users, by user ID, who log in more than once 
* Avg.Playtime: Average total play time on each day of the level (sum of playtime of DAU/DAU)
* Purchase Amount: Purchase amount of paid products  
* New Purchase: Purchase amount by new paying users (NPU)
* Repeat Purchase: Purchase amount by repeat paying users
* PU (Paying User): Users who purchase paid products (= repeat PU + new PU)
* New PU: First-time paying user
* Repeat PU: Total PU - New PU (calculated upon the previous day on a daily basis)

### Level-ups

Find the level-up information of each user.  

* Achieved Level: The level that is achieved 
* Successful Level-up User: Users who achieved the level 
* Average Level-up Achievement Time (minutes): Average achievement time (minutes) of level-up users 

![gamebase_analytics_18_201906_1](https://static.toastoven.net/prod_gamebase/gamebase_analytics_18_201906_1.png)

### Item Sales Status 

Check item sales status for each type of selected transfer indicators. 
Click **Conditions** and select query values as below:

* Purchase Amount
* Number of Purchases 
* PU (Paying User) 
* New PU 
![gamebase_analytics_19_201906_1](https://static.toastoven.net/prod_gamebase/gamebase_analytics_19_201906_1.png)



### Top 50 Sales Items 

Find the 50 most selling items for each type and value of selected transfer indicators. 

![gamebase_analytics_20_201906_1](https://static.toastoven.net/prod_gamebase/gamebase_analytics_20_201906_1.png)