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

![gamebase_analytics_03_201901_2](https://static.toastoven.net/prod_gamebase/gamebase_analytics_03_201901_2.png)

#### 1. User Status 
Basic user indicators during a selected period are available. 

* Total Daily Active Users (DAU+): Sum of daily active users, by member number, who are logged in more than once a day
* Total Weekly Active Users (WAU+): Sum of weekly active users. With weekly indicators, WAU items are substituted for DAU items.  
* Total Monthly Active Users (MAU+): Sum of monthly active users. With monthly indicators, MAU items are substituted for DAU items. 
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

Retention refers to how many subscribed users remain in service for the next 90 days after subscribed. 

![gamebase_analytics_06_201901_2](https://static.toastoven.net/prod_gamebase/gamebase_analytics_06_201901_2.png)

By opting for the exclusion of withdrawer on the same day of subscription, you can check by excluding those users who subscribed and withdrew on the same day.  

* Exclusion of withdrawer on the same day of subscription: Calculate by excluding those users who subscribed and withdrew on the same day 
    * New Users = Subscriber - Withdrawer on the same day of subscription
      e.g.) Out of 100 new users on January 1st, 20 withdrew on January 1st: then, the number of actual new users is calculated at 80 (100-20).

## Sales Indicators 
### Purchase Amount 

Shows indicators of purchase amount.   

![gamebase_analytics_07_201901_2](https://static.toastoven.net/prod_gamebase/gamebase_analytics_07_201901_2.png)

#### 1.Status Table for Purchase Amount 
Purchase amount during a specific period can be found. 
Total amount, as well as purchase amount by country of major stores are available.  

#### 2. Sales Trend  
New sales, renewed sales, and trends of paying users are displayed on graphs.
Sales by each country are also available on the below table. 

### Paying Users 

Indicators of paying users are displayed. 

![gamebase_analytics_08_201901_2](https://static.toastoven.net/prod_gamebase/gamebase_analytics_08_201901_2.png)

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

### Item Sales Indicators 

Sales indicators of Gamebase items are available. 

![gamebase_analytics_09_201901_2](https://static.toastoven.net/prod_gamebase/gamebase_analytics_09_201901_2.png)

* Item: List of items registered at Gamebase 
* 10 Best-Selling Items: List of 10 most selling items by the price or number of sales 
* Store: Stores, such as AppStore or Google Play
* Purchase Amount: Amount paid by users for each item  
* Purchase Count: Number of purchases for each item 
* Payment Ratio: Rate of purchase for each item 

### First Purchase
![gamebase_analytics_10_201901_2](https://static.toastoven.net/prod_gamebase/gamebase_analytics_10_201901_2.png)

First-purchase information of newly paying users can be found. 
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