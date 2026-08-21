<!-- pre-align:aligned sig=43b0b1dcf371 -->

<a id="game-gamebase-console-guide-monitoring"></a>
## Game > Gamebase > Console Guide > Monitoring { #game-gamebase-console-guide-monitoring }

Status of app users is shown by indicator or graph.
The menu is composed of Monitoring, Concurrent Group User, User Statistics, Install URL Statistics, and Sales Status.


<a id="monitoring"></a>
## Monitoring { #monitoring }
![operation-indicator_01_201812_en](https://static.toastoven.net/prod_gamebase/operation-indicator_01_201812_en.png)
Shows statistics of current app users, reserved push status at the moment, and reserved maintenance.
The screen is automatically refreshed every five minutes, to display changed indicators in real time.

* Basic Indicators
    * Concurrent Connected User (CCU): Number of concurrently connected users in real time
    * Maximum Concurrent User (MCU): Maximum number of concurrently connected users of a day (can retrieve in real time and by date)
    * Daily Active User (DAU): Net number of users who use game of a day (can retrieve in real time and by date)
    * New Registered User (NRU): Number of new users of a day (can retrieve in real time and by date)
* Market Share Chart: Pie Chart of Games User's Market Share
    * By Operating System: Android, iOS, WebGL, and etc.
    * By Country: By USIM country collected by SDK
    * By Version: By client version registered at console
* Change Graph of Concurrent Connection: Graph of changes of concurrent connection from 00:00 today up to now
	* Show maintenance and push details on graph to easily follow changes in concurrent connection out of maintenance and push.
    * You can download the graph in .xlsx or .csv format by clicking on the button at the top right corner of the panel.

<a id="user-statistics"></a>
## User Statistics { #user-statistics }
![operation-indicator_02_201812_en](https://static.toastoven.net/prod_gamebase/operation-indicator_02_201812_en.png)
Check DAU, MCU,NRU, and CCU AVG on graphs.
Easy to identify trend changes of the current games users.
You can specify a date by using the selection bar at the top right to check data.
Each item is described as below:

* Description
    * Daily Active User (DAU): Net number of users who use game of a day (can retrieve in real time and by date)
    * Maximum Concurrent User (MCU): Maximum number of concurrently connected users of a day (can retrieve in real time and by date)
    * New Registered User (NRU): Number of new users of a day (can retrieve in real time and by date)
    * Concurrent Connected User Average(CCU AVG): Average of concurrently connected users in real time

<a id="concurrent-group-user"></a>
## Concurrent Group User { #concurrent-group-user }
![operation-indicator_03_201812_en](https://static.toastoven.net/prod_gamebase/operation-indicator_03_201812_en.png)
Check statistics of concurrent connection of groups where your project belongs to. The real-time number of concurrent connectors can be seen at a glance per operating system of many projects at your authority.

<a id="installed-url-statistics"></a>
## Installed URL Statistics { #installed-url-statistics }
![operation-indicator_04_201812_en](https://static.toastoven.net/prod_gamebase/operation-indicator_04_201812_en.png)
Shows statistical data of install URL calls.

* Change graph of installed URL calls by date
* Market share by browser: Internet Explorer, Chrome, and etc.
* Market share by platform: Android, iOS, and etc.


<a id="statistics"></a>
## Statistics { #statistics }
![image alt](http://static.toastoven.net/prod_gamebase/Operators_Guide/Console_Monitoring_Statistics1_1.2.png)
Shows real-time sales statistics of a game in Sales Status.
Select a currency to check sales volume of the currency.

<a id="statistics-1-statistical-graph-of-daily-sales-status"></a>
#### (1) Statistical Graph of Daily Sales Status
Easy to identify daily sales status and trends on a line graph.

<a id="statistics-2-monthly-sales-status"></a>
#### (2) Monthly Sales Status
Displays monthly or accumulated sales of the month in statistics by store and aggregated data.

<a id="statistics-3-daily-sales-status"></a>
#### (3) Daily Sales Status
Retrieves sales status of each store registered in an app, by date.
You can find data up to today for the month.
Daily sales status is more precise than monthly status. You can compare and analyze sales status, during an event or when an issue occurs.