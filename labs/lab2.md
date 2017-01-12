---
title: Lab 2 - Create a Loopback Application
toc: true
sidebar: labs_sidebar
permalink: /lab2.html
summary: In this lab, you’ll gain a high level understanding of the architecture, features and development concepts related to the IBM API Connect (APIC) solution. Throughout the lab, you’ll get a chance to use the APIC command line interface for creating LoopBack applications, the intuitive web-based user interface, and explore the various aspects associated with the solution’s configuration of RESTful based services as well as their operation. At the end of this lab you will have created an new application which provides access to product inventory via a set of API resource operations.
---

## Objective

In the following lab, you will learn:

+ How to create a multi-model Loopback application
+ How to create a Representational State Transfer (REST) API definition using IBM Connect API Designer
+ How to create a Representational State Transfer (REST) API definition using IBM Connect Command Line
+ How to use the Loopback Cloudant Connector
+ How to test a REST API
+ How to create relationships between models

## Case Study Used in this Tutorial

**ThinkIBM** is a company which sells historical and rare IBM machinery. **ThinkIBM** wants to create easier acess to their inventory database through a collection of APIs. Additionally, the application should also support the ability for buyers to leave reviews. As an application developer, you will create the application that provides access to product inventory.

## Step by Step Lab Instructions

### 2.1 - Create a Working Directory

1.  Open up a new window for your Host Operating systems' command line interface (e.g. `cmd` for Windows or `terminal` for Linux).

1.  Create a project directory in the on your filesystem called ThinkIBM. 

    ```shell
    mkdir ThinkIBM
    ```

1.  Change to the new `ThinkIBM` directory by typing:

    ```shell
    cd ThinkIBM
    ```

### 2.2 - Create the Inventory App

To create your Inventory Application you will use LoopBack technology that comes with the API Connect Developer Toolkit. LoopBack enables you to quickly compose scalable APIs, runs on top of the Express web framework and conforms to the Swagger 2.0 specification. LoopBack is a highly-extensible, open-source Node.js framework that enables you to:

* Create dynamic end-to-end REST APIs with little or no coding.
* Access data from Relational and NoSQL Databases, SOAP and other REST APIs.
* Incorporates model relationships and access controls for complex APIs.

LoopBack consists of:

* A library of Node.js modules.
* Yeoman generators for scaffolding applications.

1.  From the command line terminal, type the following command to create the `inventory` application:

    ```shell
    apic loopback
    ```

1.  You will be asked to name your application. Call it `inventory` and press the `Enter` or `Return` key.

    ```
         _-----_
        |       |    .--------------------------.
        |--(o)--|    |  Let's create a LoopBack |
       `---------´   |       application!       |
        ( _´U`_ )    '--------------------------'
        /___A___\    
         |  ~  |     
       __'.___.'__   
     ´   `  |° ´ Y `
     
    ? What's the name of your application? (ThinkIBM) inventory
    ```

1.  Next you will be asked to supply the name of the directory where the application will be created.

    LoopBack will default the project directory name to the name of the application.

    Press the `Enter` or `Return` key to accept the default value of `inventory`.

1.  Next you will be asked to select the type of application.

    Use the arrow keys to select the `empty-server` option and press the `Enter` or `Return` key. 

    ```shell
    ❯ empty-server (An empty LoopBack API, without any configured models or datasources) 
    ```

1.  At this point, the project builder will install the core dependencies for our Node.js application.

    Please wait until you see the `Next steps:` section.

1.  Change to the newly created `inventory` directory:

    ```shell
    cd inventory
    ```

### 2.3 - Create a Data Source Connector to Cloudant

The datasource is what allows the API to communicate with the backend data repository. In this case we will be using Cloudant to store the inventory item information.

There are two parts to this. First is the definition of how to connect to the backend system. The second is downloading the actual loopback connector for Cloudant. The connector is akin to an ODBC or JDBC connector.

1.  In your terminal ensure that you are in the `ThinkIBM/inventory` directory. 

1.  In your terminal, type: 

    ```shell
    apic create --type datasource
    ```

    The terminal will bring up the configuration wizard for our new datasource for the item database. The configuration wizard will prompt you with a series of questions. Some questions require text input, others offer a selectable menu of pre-defined choices.
	
    Answer the questions with the following data:

    ```text
    ? Enter the data-source name: item-db-cloudant
    ? Select the connector for item-db-cloudant: IBM Cloudant DB (supported by StrongLoop)
    Connector-specific configuration:
    ? Connection String url to override other settings (eg: https://username:password@host): https://820923e0-be08-46f5-a34a-003f91f00f5c-bluemix:10d585c237c8d7b599b79cfcca39cb63356f2cea7d79abf27f284801b3c149d9@820923e0-be08-46f5-a34a-003f91f00f5c-bluemix.cloudant.com
    ? database: item
    ? username: (leave blank)
    ? password: (leave blank)
    ? modelIndex: (leave blank)
    ```
	
	{% include note.html content="
        By typing Y (Yes) to the question `Install loopback-connector-cloudant`, the Cloudant Connector will be downloaded and saved to your project automatically. 
        <br/><br/>
        This will create a connection profile in the `~/ThinkIBM/inventory/server/datasources.json` file. It is effectively the same as running the following to install the connector:
        <br/><br/> 
        `npm install loopback-connector-cloudant --save`
        <br/><br/>
        For more information on the LoopBack Connector for Cloudant, see:
        <br/><br/>
        [https://www.npmjs.com/package/loopback-connector-cloudant](https://www.npmjs.com/package/loopback-connector-cloudant)
    " %}

### 2.4 - Launch the API Connect Designer

1.  Ensure you're in the `~/ThinkIBM/inventory` directory, then type the following command:

    ```shell
    apic edit
    ```

    The Firefox web browser will launch and automatically load the designer screen.

1.  Now that the API Designer is running, you should see the start page with your `inventory` API.

    {% include note.html content="This API was created as a result of the generation of our LoopBack application.
    " %}

### 2.5 - Create a Model for the Inventory Items

In this section, you will define the `item` data model for our `inventory` API and attach it to the Cloudant data source. LoopBack is a data model driven framework. The properties of the data model will become the JSON elements of the API request and response payloads.

1.  Click the `Models` tab.

    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab2/api-designer-model-design-page.png)
	
1.  Click the `+ Add` button.

    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab2/api-designer-model-design-page-add-button.png)
	
1.  In the New LoopBack Model dialog, enter `item` as the model name, then click the `New` button.

    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab2/api-designer-model-design-page-new-model.png)

1.  When the Model edit page for the item model displays, select the `item-db-cloudant` Data Source:

    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab2/1.png)

### 2.6 - Create Properties for the `item` Model

The item table in the database has 6 columns that will need to mapped as well. To start creating properties for the item model: 

1.  Click the `+` button in the **Properties** section.

	![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab2/properties26.png)
	
1.  The `item` data model consists of six properties. Use the data below to add each of the properties:

    |Required|Property Name|Is Array|Type  |ID |Index|Description           |
    |--------|-------------|--------|------|---|-----|----------------------|
    |yes     |name         |no      |string|no |no   |item name             |
    |yes     |description  |no      |string|no |no   |item description      |
    |yes     |img          |no      |string|no |no   |location of item image|
    |yes     |img_alt      |no      |string|no |no   |item image title      |
    |yes     |price        |no      |number|no |no   |item price            |
    |no      |rating       |no      |number|no |no   |item rating           |

1.  Scroll to the top of the page and click the `Save` button to save the data model.

    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab2/api-designer-model-design-page-model-properties-save.png)

1.  Click the `All Models` link to return to the main API Designer page.

### 2.7 - Verify API

To confirm that the API has been correctly mapped and can interface with the datasource, you will run the server and test the API.

1.  Click the `Run` button to start the `inventory` LoopBack application

    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab2/run.png)

1.  Wait a moment while the servers are started. Proceed to the next step when you see the following:

    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab2/app-running.png)

1.  Click the `Explore` button to review your APIs. 

    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab2/explore.png)

1.  On the left side of the page, notice the list of paths for the `inventory` API. These are the paths and operations that were automatically created for you by the LoopBack framework simply by adding the `item` data model. The operations allow users the ability to create, update, delete and query the data model from the connected data source.

1.  Click the `GET /items` operation.

    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab2/api-designer-explore-page-get-items-api.png)

1.  By clicking the `GET /items` operation, your screen will auto-focus to the correct location in the window. In the center pane you will see a summary of the operation, as well as optional parameters and responses.

    On the right side you will see sample code for executing the API in various programming languages and tools such as cURL.

    In addition to the sample code, if you look further down the page you will see an example response, URL, API identification information, and API parameters.

1.  Scroll down slowly to locate the `Call operation` button.

    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab2/api-designer-explore-page-call-operation.png)

1. Click the `Call operation` button to invoke the API.

    {% include troubleshooting.html content="The first time you invoke the API, you may receive an error. The error occurs becuase the browser does not trust the self-signed certificate from the MicroGateway. To resolve the error, click on the link in the response window and accept the certificate warning.
    " %}

    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab2/cert-error.png)

1.  Once complete, return to the API explorer and click on the `Call operation` button again.

1.  Scroll down to see the `Request` and `Response` headers. 

    ```text
    Request
    GET https://localhost:4002/inventory/items
    APIm-Debug: true
    Content-Type: application/json
    Accept: application/json
    X-IBM-Client-Id: default
    X-IBM-Client-Secret: SECRET
    ```

    ```text
    Response
    Code: 200 OK
    Headers:
    content-type: application/json; charset=utf-8
    x-ratelimit-limit: 100
    x-ratelimit-remaining: 99
    x-ratelimit-reset: 3599999
    ```

1.  Scroll further and the payload returned from the GET request is displayed.

    ```json
    [
      {
        "name": "Dayton Meat Chopper",
        "description": "Punched-card tabulating machines and time clocks...",
        "img": "images/items/meat-chopper.jpg",
        "img_alt": "Dayton Meat Chopper",
        "price": 4599.99,
        "rating": 0,
        "id": 5
      },
      ...
    ]
    ```

1.  Test the `GET /items/count` operation by following the same process above. You should receive a count of inventory items.

    ```json
    {
      "count": 12
    }
    ```

1.  Click on the `Stop` button to shut down the running application.

    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab2/stop-application.png)

1.  Click on the `X` in the top-right portion of the screen to leave the API Explorer view.

    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab2/leave-explorer.png)

### 2.8 - Create the second Cloudant Data Source for Item Reviews

So far, we have created a LoopBack application which provides APIs around our inventory items stored in a Cloudant database in Bluemix.

In the next section, you will create the data model for item reviews which will use Cloudant to store the review data.

First you must create a data source entry for the Cloudant Reviews DB.

In the earlier steps, you used the command line to create a data source connection. This time you will use the API Designer.

1.  From the top navigation menu, select the `Data Sources` link to switch views.

    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab2/data-sources.png)

1.  Click on the `Add +` button.

    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab2/add-new-datasource.png)

1.  Name the new datasource `review-db-cloudant` and click the `New` button.

    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab2/add-new-datasource-name.png)

1.  Complete the new datasource configuration using the values in the table below.

    |Field      |Value        |
    |-----------|-------------|
    |URL        |https://820923e0-be08-46f5-a34a-003f91f00f5c-bluemix:10d585c237c8d7b599b79cfcca39cb63356f2cea7d79abf27f284801b3c149d9@820923e0-be08-46f5-a34a-003f91f00f5c-bluemix.cloudant.com|
    |Database   |review       |
    |Username   |<leave blank>|
    |Password   |<leave blank>|
    |Model Index|<leave blank>|

1.  Click on the `Save` icon to save the new data source connection. The toolkit will test the connection and report back. 

    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab2/add-new-datasource-success.png)

### 2.9 - Create Model for Reviews

The `review` data model will be used to store item reviews left by buyers. The reviews will be stored in a Cloudant.

In the earlier steps, you used the API Designer User Experience to create a data model. This time you will use the command line to create the `review` model.

1.  Click the `x` button on your browser tab to close the API Designer.

1.  Select the `Terminal Emulator` from the taskbar to open the command line.

1.  Even though we closed the browser, the API Designer application itself is still running.

    Hold the `control` key and press the `c` key to end the API Designer session:
	
    ```shell
    control+c
    ```
	
    This will return you to the command line prompt.

1.  Type the following command to create the `review` data model:

    ```shell
    apic create --type model
    ```

1.  Enter the properties for the `review` model:

    {% include important.html content="You will **not** expose the review mode as a REST API. This is because you create a relationship between item and review later that will create the REST APIs you will use.
    " %}

    ```text
    ? Enter the model name:  review
    ? Select the data-source to attach review to:
    	> review-db-cloudant (cloudant)
    ? Select models base class:
    	> PersistedModel
    ? Expose review via the REST API? (Y/n):  N
    ? Common model or server only?
    	> common
    ```
	
1.  Continue using the wizard to add properties for the `review` model:

1.  The first property is the `date` property:

    ```text
    Enter an empty property name when done.
    ? Property name: date
    ? Property type:
    	> date
    ?Required? Y
    ?Default value [leave blank for none]: <leave blank>"
    ```

1.  Next add the `reviewer_name` property:

    ```text
    Let's add another review property.
    Enter an empty property name when done.
    ? Property name: reviewer_name
    ? Property type:
    	> string
    ? Required? N
    ? Default value [leave blank for none]: <leave blank>
    ```

1.  Next add the `reviewer_email` property:

    ```text
    Let's add another review property.
    Enter an empty property name when done.
    ? Property name: reviewer_email
    ? Property type:
    	> string
    ? Required? N
    ? Default value [leave blank for none]: <leave blank>
    ```

1.  Next add the `comment` property:

    ```text
    Let's add another review property.
    Enter an empty property name when done.
    ? Property name: comment
    ? Property type:
    	> string
    ? Required? N
    ? Default value [leave blank for none]: <leave blank>
    ```

1.  Finally add a property for the item `rating`:

    ```text
    Let's add another review property.
    Enter an empty property name when done.
    ? Property name: rating
    ? Property type:
    	> number
    ? Required? Y
    ? Default value [leave blank for none]: <leave blank>
    ```

1.  To close the wizard, the next time it asks you to add another review proporty, just press `Enter` or `Return` to exit.

### 2.10 - Create a Relationship Between the `item` and `review` Data Models

The next step in this lab is to create a relationship between the `item` model and the `review` model. Even though the models reference entities in entirely different databases, API Connect provides a way to create a logical relationship between them. This logical relationship is then exposed as additional operations for the item model.

1.  In the terminal session, type the following command:

    ```shell
    apic loopback:relation
    ```

1.  Enter the details for the relationship as follows:

    ```text
    ? Select the model to create the relationship from:
    	> item
    ? Relation type:
    	> has many
    ? Choose a model to create a relationship with:
    	> review
    ? Enter the property name for the relation:  reviews
    ? Optionally enter a custom foreign key: <leave blank>
    ? Require a through model? No
    ```
	
### 2.11 - Verify the Relationship

To verify that the relationship has been created, you will open the API Connect Designer and view the operations on the Explore page.

1.  In the terminal session, type the following command to launch the API Connect Designer window:

    ```shell
    apic edit
    ```

1.  Click on the `inventory` link from the APIs tab.

    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab2/inventory-link.png)

1.  Scroll down to the `Paths` section of the API definition.

    Notice how three new API paths have been created which allow access to item revew data:

    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab2/new-paths.png)

1.  Click the `x` button on your browser tab to close the API Designer.

1.  Select the `Terminal Emulator` from the taskbar to open the command line.

1.  Even though we closed the browser, the API Designer application itself is still running.

    Hold the `control` key and press the `c` key to end the API Designer session:
	
    ```shell
    control+c
    ```
	
    This will return you to the command line prompt.

## Conclusion

**Congratulations!** In this lab you learned:

+ How to create a multi-model LoopBack application
+ How to create a Representational State Transfer (REST) API definition using IBM Connect API Designer
+ How to create a Representational State Transfer (REST) API definition using IBM Connect Command Line
+ How to use the LoopBack Cloudant Connector
+ How to test a REST API
+ How to create relationships between models

Lab 3 will build on what you have already created to enable processing hooks and publish the APIs to the API Manager.

Proceed to [Lab 3 - Customize and Deploy an Application](lab3.html)

