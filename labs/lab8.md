---
title: Lab 8 - Analytics in API Connect
toc: true
sidebar: labs_sidebar
permalink: /lab8.html
summary: In this lab, you will gain a high level understanding of how analytics are used to visualize the information captured by the gateway node. You can filter, sort and aggregate your API event data; then present the results within correlated charts, tables and maps to help you manage service levels, set quotas, establish controls, set up security policies, manage communities and analyze trends. API Analytics is built on the Kibana V4.3 open source analytics and visualization platform, which is designed to work with the Elastic Search real-time distributed search and analytics engine.
---

## Objective
 
In this lab, you will learn:

+ How to view the analytics for a catalog inside of API Manager
+ How to create a custom dashboard
+ How to add visualizations to a dashboard 
+ How to customize and arrange visualizations in the dashboard

## Case Study Used in this Tutorial

In this tutorial, you will simulate a good amount of traffic passing through the API Connect platform by executing a script that will invoke a series of `curl` commands that will hit a dummy API. Upon that script finishing, you will go into the API Manager and get some hands on experience with customizing a dashboard and viewing the analytics for traffic run on the system.

## Step by Step Lab Instructions

### 8.1 - Browse the Analytics Data in API Connect

1.  If not already open, launch the `Firefox Web Browser` from the favorites menu.

1.  Click on the `API Manager` bookmark.

1.  Log in to the `API Manager` server with the following credentials:

    > Username: `student@think.ibm`
    >
    > Password: `Passw0rd!`

1.  Click on the `Sandbox` catalog tile:

    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab8/api-mgr-dashboard-sandbox-tile.png)

1.  From the `Sandbox` catalog configuration screen, click on the `Analytics` tab:

    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab8/analytics-tab.png)

1.  The default dashboard gives some general information like the 5 most active Products and 5 most active APIs.  This information is interesting, but we can see much more information by customizing the dashboard. Add a new visualization by clicking on the `+ Add Visualization` icon:

    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab8/analytics-add-visualization.png)

1.  This will bring a list of some of the standard visualizations. You can then type in a string to filter through visualizations or use the arrows to page through the list.

1.  Add the `API Calls` visualization to the dashboard by simply clicking on it. The new visualization will be added to the bottom of our dashboard.

    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab8/image21.png)

1.  Scroll down to find the new visualization. You can adjust the size by clicking and dragging the border from the lower right. Additionally, you can adjust its position by clicking and dragging the box to where you want it.

    Move the `API Calls` box in between the 5 Most Active Products and 5 Most Active API windows.

    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab8/image22.png)
	
1.  Feel free to play around with the other visualizations by adding them to the Dashboard. You can also save the dashboard by clicking on the `Save Dashboard` button:

    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab8/analytics-save-dashboard.png)

1.  There are also several out of the box Dashboards that you can play with by clicking on the `Load Saved Dashboard` icon. Go ahead and open the `api default` Dashboard.

    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab8/analytics-load-dashboard.png)

1.  Here you will see some interesting visualizations that show graphs and charts with information about the API Traffic that was processed.

1.  Analytics data can be filtered over different time periods, and the widgets can even be automatically refreshed. Click on the calendar icon which specifies the default time period of `Last 7 days`.

    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab8/analytics-calendar.png)
	
1.  Click on `Auto Refresh`.

    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab8/analytics-auto-refresh.png)

1.  Set the refresh period to `5 seconds`.

    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab8/analytics-refresh-5sec.png)

1.  Return to the consumer application in the Chrome web browser. Navigate around the site and test out some of the features in order to generate some additional API calls.

1.  Return back to the analytics view and notice how the data is refreshed automatically.

## Completion

**Congratulations!** You have completed all of the labs!