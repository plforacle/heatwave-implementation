# Build a real-time dashboard in Oracle Analytics Cloud with MySQL HeatWave

![mysql heatwave](./images/mysql-heatwave-logo.jpg "mysql heatwave")

## Introduction

MySQL HeatWave can easily be used for development tasks with existing Oracle services, such as Oracle Cloud Analytics. -> Oracle Analytics Cloud (OAC) provides the industry’s most comprehensive cloud analytics in a single unified platform, including self-service visualization and inline data preparation to enterprise reporting, advanced analytics, and self-learning analytics that deliver proactive insights.

Use MySQL HeatWave with OAC to explore and perform collaborative analytics with your MySQL data.

_Estimated Lab Time:_ 20 minutes

### Objectives

In this lab, you will be guided through the following tasks:

- Create Oracle Analytics Cloud and connect to MySQL HeatWave
- Create a dashboard on OAC for the airportdb

### Prerequisites

- An Oracle Trial or Paid Cloud Account
- Some Experience with MySQL Shell
- Completed Lab 4

## Task 1:  Create an Oracle Analytic Cloud Service

1. From the OCI console, navigate to Analytics & AI-> Analytics Clouds

    ![analytics menu](./images/analytics-menu.png "analytics menu")

2. Select the **root** Compartment and Click the **Create Instance** button

    ![create-instance-oac](./images/create-instance-oac.png "create-instance-oac ")

3. On the Create Analytics Instance enter the required information as shown below

    Name:

    ```bash
    <copy>hwoac</copy> 
    ```

    Description:

    ```bash
        <copy>Oracle Analytics Cloud HeatWave Test</copy>
    ```

    Capacity: select **OCPU** and select **1**

    License and Edition:
    - select **License Included**
    - select **Enterprise Edition**

4. Click the **Create** button

    ![configure oac](./images/config-oac.png "config-oac ")

5. It takes about 12-15 mins for OAC instance creation to complete.

    ![created oac](./images/created-oac.png " created-oac")

## Task 2: Configure Private Access Channel

1. Go down to the “Private Access Channel” resources page and click on the **Configure Private Access Channel**  button.

2. Click the create Private Access Channel button

3. On the create Private Access Channel page enter the following:

    Name:

    ```bash
         <copy>hwoacpac</copy>
    ```

    DNS Zones:
    **Check Virtual Cloud Network's domain name as DNS zone (hwvcn.oraclevcn.com)**

    Description:

    ```bash
        <copy>Testing</copy>
    ```

    **Remove second  DNS Zone entry**
    ![configure private access for oac](./images/config-pac-oac.png " config-pac-oac")

4. Click the **Config** button

5. Wait 15 minutes for the process to finish, then continue to Task 3
    ![oac private access created  ](./images/created-pac-oac.png " created-pac-oac")

## Task 3: Get HeatWave DB Hostname **

1. Before starting go to Menu Databases > DB Systems

2. Select HeatWave database: HeatWave-DB

3. Click on the "Connections" tab on the Endpoinst > Internal FQDN > Click on the Copy link.

    ![database endpoint](./images/hw-db-endpoint.png "hw-db-endpoint ")

4. Save the Hostname, in notepad, for use with OAC

    Example : **hwdb.sub09012.....hwvcn.oraclevcn.com**

## Task 4: - Use OAC to Analyze the Lakehouse passenger-survey data**

1. Navigate to hamburger->Analytics->Analytics Clouds

2. Select the OAC instance you provisioned to access the OAC console by clicking on Analytics Home Page. Click on the "Anlytics Home Page" button.

    ![analytics go home page](./images/analytics-go-home-page.png "analytics-go-home-page ")

3. Create a Connection to HeatWave to build a dashboard

    ![analytics home page](./images/analytics-home-page.png " analytics-home-page")
4. Click the "Create Connection" button
    ![analytics home page](./images/analytics-dataset-connections.png " analytics-home-page")

5. Search for Heatwave and select Heatwave as the database

      ![connection search for heatwave](./images/add-connection-mysql.png "aconnection search for heatwave")

6. Specify the connections details

    - Specify the hostname of HEATWAVE-DB
    - Use the FQDN information you save in Step 3
    - Port: 3306
    - Database Name: airportdb  
    - Be sure to use the Heatwave db user name and password

   Hit the "Save" button to fisnish creating the connection.

    ![config myql connection](./images/config-add-connection-mysql.png "config-add-connection-mysql ")

7. The completed connection will display  a "New Dataset" page. Click on the "Schemas" link and select the "airportdb" schema

    ![open schema](./images/open-schema.png "open schema ")
8. Double click the "passenger\_survey" table. In a few seconds it will display all of the table columns. 

    - Click the "Save" icon to display the "Save Dataset As" dialog box 
    - Set the name and description to "passenge\_survey"
    - click the "OK" button

    ![save schema dataset](./images/schema-dataset.png "save schema dataset ")

9. Click the "Create workbook" button
    ![create oac workbook](./images/create-workbook-schema.png "create-workbook-oac")

10. We are only interested in values that can be improved through the booking process. So we will create pie charts to visualize the Satifaction level the following  Services or Results
    - Ease\_of\_Online\_booking and satisfaction\_2
    - Departure\_Arrival\_time\_convenient and satisfaction\_2
    - Departure\_Delay\_in\_Minutes and satisfaction\_2
    - Arrival\_Delay\_in\_Minutes and satisfaction\_2

11. Ease_of\_Online\_booking and satisfaction\_2

    - Click on the satifaction\_v2 value and the Ease_of\_Online\_booking
    - Right click on the Ease_of\_Online\_booking selection anc click "Pick Visualization" option
    - Click on the Pie chart and you will display
    ![online bookingsatisfaction](./images/chart-online-booking-satisfaction.png "online bookingsatisfaction")

12. Do the activities in Step 11  for the remaining 3 Data points

    - Departure\_Arrival\_time\_convenient and satisfaction\_2
    - Departure\_Delay\_in\_Minutes and satisfaction\_2
    - Arrival\_Delay\_in\_Minutes and satisfaction\_2

    Your fianl dashboard should look like this:

    ![satisfaction dashboard](./images/satisfaction-dashboard.png "satisfaction dashboard")

13. Save the workbook as "passenger\_satisfaction".
14. Let's review our charts and determine how they can be used in the booking application to improve customer satisfaction
    - a. Ease of Online booking Satisfaction Level is greater 60% - they're happy let's leave it alone.
    - b. Departure Arrival time convenient - Satisfaction Level is greater 50% let's leave it alone.
    - c. The customer's are very disatisfied with  Departure and Arrival Delays. How can we fix this problem with the booking application.

15. What if we could get data on airline routes and their delays... and what if we cauld find a way to predict fligt delays before the customer made their reservation and provide this information so they could choose flights with less delays? You can skip to Lab 7 to help make it happen!

## Task 5: - Build OAC Dashboard with non-Lakehouse data**

1. Select the MySQL Connection created earlier "heatwave"

2. Double click "Manual Query" on the left panel and click ”Manual Query” tab on the bottom

    ![manual query](./images/manual-query-select.png "manual-query-select ")

3. Add the following SQL query (find per-company average age of passengers from Switzerland, Italy and France) in the statement text box, and select “Live” in Data Access on the right, then click OK on the top

    ```bash
    <copy>SELECT
        airline.airlinename,
        AVG(datediff(departure,birthdate)/365.25) as avg_age,
        count(*) as nbpeople
    FROM
        booking, flight, airline, passengerdetails
    WHERE
        booking.flight_id=flight.flight_id AND
        airline.airline_id=flight.airline_id AND
        booking.passenger_id=passengerdetails.passenger_id AND
        country IN ("SWITZERLAND", "FRANCE", "ITALY")
    GROUP BY
        airline.airlinename
    ORDER BY
        airline.airlinename, avg_age
    LIMIT 10;</copy>
    ```

4. Dataset screen
    ![set oac dataset](./images/new-data-set-oac.png "new-data-set-oac ")

5. Click the  save button and set the DataSet name to Passengers then click Create workbook button
    ![create oac workbook](./images/create-workbook-oac.png "create-workbook-oac")

6. On new display page  click on the Dataset icon, select airline and nbrpeople
    ![select columns](./images/passenger-column.png "passenger-column")
7. Right click and select "Create Best Visualization".
    ![auto visualization](./images/best-visualization-oac.png "best-visualization-oac")

9. Click on the bottom + sign to add Canvas 2 , select airline and avg_age

9. Right click and select "Pick Visualization" and select pie chart.
    ![manual visualization](./images/pick-visualization-oac.png "pick-visualization-oac ")

10. Save Workbook as "passenger Workbook" and close OAC application
You may now **proceed to the next lab**

## Acknowledgements

- **Author** - Perside Foster, MySQL Principal Solution Engineering
- **Contributors** - Mandy Pang, MySQL Principal Product Manager,  Nick Mader, MySQL Global Channel Enablement & Strategy Manager
- **Last Updated By/Date** - Perside Foster, MySQL Solution Engineering, August 2023