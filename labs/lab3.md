---
title: Lab 3 - Customize and Deploy the Inventory Application
toc: true
sidebar: labs_sidebar
permalink: /lab3.html
summary: In this lab, you will add capabilities into the LoopBack application which was created in Lab 2. You will add custom javascript code which will alter the default behavior of the application. Once your edits are complete, you will package the application and publish it to Bluemix as a Cloud Foundry Application where it will be managed and enforced by the API Connect solution.
---

## Objective

In the following lab, you will learn:

+ About LoopBack remote hooks
+ How to create a remote hook
+ How to publish a LoopBack application to Bluemix

## Case Study Used in this Tutorial

At this point, you have: created a basic application template, added an `item` data model backed by a datasource, added a `review` data model backed by another data source, added a relationship between the `item` and `review` models.

In this tutorial you will extend the `inventory` application by adding a remote hook. Remote hooks allow you to provide pre- and post-processing to an API call, such as adding additional header information to a remote service or calculating a value, which is what you will do in this lab.

Then, you will publish your LoopBack Inventory application to Bluemix where it will run

## Step by Step Lab Instructions

### 3.1 - Edit the Application Configuration

Before publishing the API for our application, the configuration file that was generated for you needs to be edited. By default, the generated application uses a base path of `/api`. In the next few steps, you will modify the base path to listen on `/inventory`.

1.  Using the text editor of choice, then navigate to the `ThinkIBM / inventory` folder. 

    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab3/list-folder.png)

    {% include note.html content="LoopBack applications use a series of configuration files which drive the application runtime. For more information about these files, review the table in [Lab 1](lab1.html#loopback-files).
    " %}

1.  From the folder tree menu, expand the `server` folder and click on the `config.json` file to view the source.

    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab3/open-folder.png)

1.  Edit line 2 of the `config.json` file. Change `/api` to `/inventory`.

    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab3/atom-edit-config.png)

1.  Save the changes.

### 3.2 - Create a Remote Hook

Remote hooks are custom javascript code that execute before or after calling an operation on a LoopBack application.

For more information on Remote Hooks please see:

[https://docs.strongloop.com/display/public/LB/Remote+hooks](https://docs.strongloop.com/display/public/LB/Remote+hooks)

1.  In the your text editor, expand the local directory structure for the `common/models` location and open up the `item.js` file for editing.

    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab3/atom-item-file1.png)

1.  You are going to update this file to include a new remote hook function which will run *after* a new review is submitted for an item. The function will take an average of all reviews for that item, then update the item rating in the data source.

1.  Open up a new tab on your browser, and open the following file via github:

    [https://raw.githubusercontent.com/ibm-apiconnect/pot/gh-pages/assets/lab3/item.js](https://raw.githubusercontent.com/ibm-apiconnect/pot/gh-pages/assets/lab3/item.js)
	
1.  You have two choices in how to implement this change. You can either copy and paste in the code from github by simply copying the contents from the github to the clipboard and pasting it to your local `item.js` file.

1.  Alternatively, you can copy and paste the contents of the text box below.  Be sure to **Remove** everything in the `item.js` file. Then paste the contents of your clipboard to update the file.

    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab3/2a.png)

    ```javascript
    module.exports = function (Item) {
    
        /* Inject DATE into new REVIEW */
        
        Item.beforeRemote('prototype.__updateById__review', function (ctx, review, next) {
          var req = ctx.req;
          req.body.date = Date.now();
          ctx.args.data = req.body;
          next();
        });
        
        /* Inject DATE into new REVIEW */
        
        Item.beforeRemote('prototype.__create__reviews', function (ctx, review, next) {
          var req = ctx.req;
          req.body.date = Date.now();
          ctx.args.data = req.body;
          next();
        });
        
        /* Update ITEM rating after new REVIEW is submitted */
        
        Item.afterRemote('prototype.__create__reviews', function (ctx, remoteMethodOutput, next) {
          var itemId = remoteMethodOutput.itemId;
        
          console.log("calculating new rating for item: " + itemId);
        
          var searchQuery = {include: {relation: 'reviews'}};
        
          Item.findById(itemId, searchQuery, function findItemReviewRatings(err, findResult) {
            var reviewArray = findResult.reviews();
            var reviewCount = reviewArray.length;
            var ratingSum = 0;
        
            for (var i = 0; i < reviewCount; i++) {
              ratingSum += reviewArray[i].rating;
            }
        
            var updatedRating = Math.round((ratingSum / reviewCount) * 100) / 100;
        
            console.log("new calculated rating: " + updatedRating);
        
            findResult.updateAttribute("rating", updatedRating, function (err) {
              if (!err) {
                console.log("item rating successfully updated");
              } else {
                console.log("error updating rating for item: " + err);
              }
            });
        
            next();
          });
        
        });
        
    };
    ```

1.  Save the changes to the `item.js`.

### 3.3 - Publish App to Bluemix

In this section, you will publish the `inventory` application to Bluemix

#### 3.3.1 - Configure the Developer Toolkit to Communicate with API Connect

1.  Return to your `Terminal Emulator` session, or open a new one if you had closed it previously.

1.  Ensure you are in the `~/ThinkIBM/inventory` project folder by typing the following command:

    ```bash
    cd ~/ThinkIBM/inventory
    ```

1.  Launch the API Designer:

    ```bash
    apic edit
    ```

1.  Click the `Publish` icon.

    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab3/publishButton.png)

1.  Select `Add and Manage Targets` from the menu.

1.  Select `Add IBM Bluemix target`.

    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab3/15.png)

1.  Provide connection information to sign into the IBM API Connect Bluemix service then click the `Sign in` button: 

1.  On the "Select an organization and catalog" screen, choose the `Sandbox` catalog and click the `Next` button.

    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab3/10.png)
	
1.  Provide the name of your application. The name you provide here will be the name of the app inside of Bluemix once it's published.

    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab3/select-your-inventory-app.png)

1.  Make sure your app name is selected and click the `Save` button.

    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab3/select-your-inventory-app.png)

#### 3.3.2 - Publish the Application

1.  Click `Publish` button once more and select our target catalog, indicated by the grey highlighting.

    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab3/select-publish-target.png)

1.  Click the check box to select `Publish application`, then click the `Publish` button.

    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab3/publish-application.png)
	
	{% include note.html content="
	    The Developer Toolkit will package up our Node.js StrongLoop application and deploy it to the runtime collective.
	    <br/><br/>
	    We used the web UI in the previous few steps to publish the application. However, we could have also used the API Designer CLI to accomplish the same task.
    " %}

1.  Wait for the application publish process to complete:

    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab3/publish-app-success.png)

1.  Click the `x` button on your browser tab to close the API Designer.

1.  Return to the terminal editor. Stop the API Designer process:

    ```bash
    control+c
    ```

1.  Notice that logs were generated during the application publishing process:

    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab3/2.png)

3. **Note** the contents of the `API target urls:` this value will be automatically propagated into a property called `runtime-url`  and placed into your `invoke` policy within the API Assembly.  This is what will tell the runtime which back-end service to call.  No further action on your part is required to configure this step.

    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab3/3.png)
	

    {% include note.html content="
        Note that your API Target URL will be different for your application and is unique to you.
        <br/><br/>
        You will not be able to test your application until you actually expose it as an API as it creates a specific TLS-Profile that only your specific API Connect instance can use.  You can confirm these changes by going into the API Assembly and clicking on the Assemble button.  Open up the Invoke step and you will be able to see what was created.
        <br/><br/>
    " %}

    
## Conclusion

In this lab you learned:

+ How to create a remote hook
+ How to test the remote hook
+ Publish App to Bluemix

Proceed to [Lab 4 - Configure and Secure an API](lab4.html)

[important]: https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/common/important.png "Important!"
[info]: https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/common/info.png "Information"
[troubleshooting]: https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/common/troubleshooting.png "Troubleshooting"

