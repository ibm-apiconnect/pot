---
title: Lab 6 - Working with API Products
toc: true
sidebar: labs_sidebar
permalink: /lab6.html
summary: API's published by API Connect are bundled into an object called a **Product**. The Product combines one or more API's with one or more Plans. A **Plan** is effectively a contract between the API Provider and API Consumer which specifies the allowed rate of API calls over a given period of time.
---

## Objective

In the following lab, you will learn:

+ How to create a Product
+ How to attach APIs to a Product
+ How to create a Plan
+ How to publish a Product

## Case Study Used in this Tutorial

Your work as an Application Developer / API Designer is now complete. It's time to switch roles and become an API Product Manager. The role of the API Product Manager is to take the developed assets and bundle them together using a go-to-market strategy.

In the case of **ThinkIBM**, you will publish all of our API's together as a single product offering to API Consumers. Additionally, you will create two plans which have different levels of access to your APIs.

## Step by Step Lab Instructions

### 6.1 - Creating an API Product

1.  If the API Designer screen has not already been launched, open a terminal and start the designer by issuing the following commands:

    ```bash
    cd ~/ThinkIBM/inventory
    apic edit
    ```

1.  Switch to the `Products` tab

    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab6/products.png)

1.  Click the link for the `inventory` product.
	
	{% include note.html content="This product was automatically created during creation of the Inventory LoopBack application.
    " %}

1.  Edit the Product Info & Contact details:

    > Title: `think`
    >
    > Name: `think`
    >
    > Description: `The **think** product will provide really awesome APIs to your application.`
    >
    > Contact Name: `Thomas Watson`
    >
    > Contact Email: `thomas@think.ibm`
    >
    > Contact URL: `http://www.ibm.com`  
	
    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab6/think-infocontact.png)

1.  Specify a License and Terms of Service:

    > License Name: `The MIT License (MIT)`
    >
    > License URL: `https://opensource.org/licenses/MIT`
    >
    > Terms of Service: _paste the contents of the file linked below:_
    > 
    > * [http://github.com/ibm-apiconnect/pot/raw/gh-pages/assets/lab6/license.txt](http://github.com/ibm-apiconnect/pot/raw/gh-pages/assets/lab6/license.txt)

    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab6/think-licensetos.png)
	
1.  Modify the Visibility so that the `think` product is only visible to `Authenticated users`:
  
    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab6/think-visibility.png)
	
1.  Navigate to the APIs section. Click the `+` button to add all of our new APIs to this product.

1.  Ensure the checkboxes next to `financing`, `logistics` `oauth` and `inventory` are selected.
	 
    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab6/think-apis.png)

1.  Click the `Apply` button.

1.  Navigate to the Plans section. Click the `+` button to add a new plan and specifying the following properties:

    > Title: `Silver`
    >
    > Name: `silver`
    >
    > Description: `Limited access to these APIs`
    >
    > Add rate limit of 100 with an interval of 1 hour
	
    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab6/think-silverplan.png)

1.  Click the `+` button to create a new plan. Give it the following properties:

    > Title: `Gold`
    >
    > Name: `gold`
    >
    > Description: `Unlimited access to these APIs for approved users`
    >
    > Rate limit: `Unlimited`
    >
    > Approval: check `Require subscription approval`  
	
    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab6/think-goldplan.png)

1.  Save your changes.

    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab6/save-icon.png)

### 6.2 - Publishing the API Product

1.  Click the `Publish` icon.

    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab6/think-publish.png)

1.  Select our target:

    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab6/publish-target.png)

1.  Check the box to `Stage or Publish products`:

    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab6/publish-product.png)

    {% include note.html content="
        Here we have the opportunity to select what gets published. If we were working on multiple API products as part of this project, we could chose specific ones to publish.
        <br/><br/>
        Also, the option exists to only Stage the product. A Stage-only action implies that we'll push the configuration to the Management server, but not actually make it available for consumption yet. The reason for doing this may be because your user permissions only allow staging, or that a different group is in charge of publishing Products.
    " %}

1.  Click the `Publish` button to make the API available in the Developer Portal and enforced on our API Gateway.

1.  Wait a moment while the Product is published, a `Success` message will appear letting you know the step is complete:

    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab6/publish-success.png)

1.  Close the Firefox web browser.

1.  In the `Terminal Emulator`, use the `control+c` keyboard command to quit the API Designer process.

## Conclusion

**Congratulations!** You have completed Lab 6.

In this lab, you learned:

+ About API Products
+ How to publish an API Product

Proceed to [Lab 7 - Consumer Experience](lab7.html)