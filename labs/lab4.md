---
title: Lab 4 - Configure and Secure an API
toc: true
sidebar: labs_sidebar
permalink: /lab4.html
summary: In this lab, you will learn how to configure and secure the `inventory` API created during loopback application generation. Using the graphical design tools in API Designer, you will create an OAuth 2.0 provider API called `oauth` and then update the `inventory` API to use this provider. You will use the API Editor assembly view to specify the API's runtime behavior.
---

## Objective

In the following lab, you will learn:

+ How to create an OAuth 2.0 Provider, specifically using the Resource Owner Password grant type.
+ How to secure an existing API using the newly created OAuth 2.0 Provider.
+ How to add catalog-specific properties to an API.
+ How to assemble an API implementation using the activity-log, set-variable and proxy policies

## Case Study Used in this Tutorial

In this tutorial, you will secure the Inventory API to protect the resources exposed by **ThinkIBM**. Consumers of your API will be required to obtain & provide a valid OAuth token before they can invoke the Inventory API.

## Step by Step Lab Instructions

### 4.1 - Working with the Inventory API in API Designer

1.  First, launch API Designer by typing the following commands from your project directory:

    ```bash
    cd ~/ThinkIBM/inventory/
    apic edit
    ```
    
    API Designer will open in your browser. You may see an informational message about Draft APIs. (This message appears the very first time you launch the API Designer.) If so, click the `Got it!` button, when you are ready to proceed to creating an API.
    
    You should see the APIs view, and a single API listed. The `inventory` API was automatically created during loopback app generation. We will edit this API at a later step.
    
    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab4/startapidesigner.png)

### 4.2 - Adding a New OAuth 2.0 Provider API

1.  Click the `+ Add` button and select `OAuth 2.0 Provider API` from the menu.

1.  Specify the following properties and click the `Add` button to continue.

    > Title: `oauth`
    > 
    > Name: `oauth`
    > 
    > Base Path: `/`
    > 
    > Version: `1.0.0`
	
    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab4/newoauthprops.png)

1.  The API Editor will launch. If this is your first time using the API Editor, you will see an informational message. When you are ready to proceed, click the `Got it!` button to dismiss the message.  
	
    The API Editor opens to the newly created `oauth` API. The left hand side of the view provides shortcuts to various elements within the API definition: Info, Host, Base Path, etc. By default, the API Editor opens to the `Design` view, which provides a user-friendly way to view and edit your APIs. You may notice additional tabs labeled `Source` and `Assemble`. We will work with these views as well.
	
    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab4/newoauth-start.png)

1.  Navigate to the `OAuth 2` section.

    Over the next several steps, we will set up OAuth-specific options, such as client type (public vs confidential), valid access token scopes, supported authorization grant types, etc. The [OAuth 2.0 Specification](http://tools.ietf.org/html/rfc6749) has detailed descriptions of each of the properties we are configuring here.

1.  For the `Client type` field, click the drop down menu and select `Confidential`.

1.  Three scopes were generated for you when the OAuth API Provider was generated: `scope1`, `scope2`, `scope3`.

1.  Modify the values for `scope1`, set the following fields:

    > Name: `inventory`
    > 
    > Description: `Access to Inventory API`

1.  Delete `scope2` and `scope3` by clicking the trashcan icons to the right of the scope definitions.

1.  We want to configure this provider to *only* support the Resource Owner Password Credentials grant type. Deselect the `Implicit`, `Application` and `Access Code` Grants, but leave `Password` checked.

    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab4/oauthgranttypes.png)

1.  Set the remaining OAuth 2 settings as follows:

    > Collect credentials using: `basic`
    > 
    > Authenticate application users using: `Authentication URL`
    > 
    > Authentication URL: `https://thinkibm-services.mybluemix.net/auth`
    > 
    > Turn off the `Enable revocation` option
    
    When complete, your settings should look like this:
    
    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab4/1.png)

	{% include important.html content="The scope defined here must be identical to the scope that we define later when telling the `inventory` API to use this OAuth config. A common mistake is around case sensitivity. To avoid running into an error later, make sure that your scope is set to all **lowercase**.
    " %}
    
{% comment %}
1.  Switch to the `Assemble` tab.

    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab4/assemble-tab.png)

1.  Select the radio button next to `DataPower Gateway policies`.

    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab4/dp-gwy-policies.png)

1.  Click on the `Create assembly` button.

    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab4/create-assembly-btn.png)
	
1.  In the list of policies on the left-hand scroll bar, find the policy named `gatewayscript`.

    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab4/select-gws-policy.png)

1.  Click and drag the `gatewayscript` policy to the assembly pipeline.

    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab4/gws-assembly.png)

1.  In the gatewayscript editor, type the following lines of code:

    ```javascript
    var hm = require('header-metadata');
    hm.response.set('Access-Control-Allow-Origin','*');
    ```

    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab4/gws-set-cors.png)
{% endcomment %}

1.  Click the `Save` icon in the top right corner of the editor to save your changes.

    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab4/save-icon.png)

### 4.3 - Configuring and Securing the Inventory API

1.  Click the `All APIs` link at the top left of the API Editor to return to list of APIs.

1.  Click the `inventory` link.

    The inventory API will open in the API Editor, where we can make the necessary configuration changes. Over the next several steps you will set this API up to use the OAuth provider you just created.

1.  Navigate to the `Base Path` section.

    Change the base path from `/api` to `/inventory`.

    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab4/base-path.png)

1.  Navigate to the `Security Definitions` section.

    Click the `+` icon in the **Security Definitions** section and select `OAuth` from the menu.  
	
    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab4/inv-addoauthsecuritydef.png)
	
    A new security definition is created for you, called `oauth-1 (OAuth)`.

1.  Scroll down slightly to edit the newly created security definition.

    Set it to have the following properties:
	
    > Name: `oauth`
    > 
    > Description: `Resource Owner Password Grant Type`
    > 
    > Flow: `Password`
    > 
    > Token URL: `https://<BluemixHost>/<BluemixOrgName>-<BluemixSpaceName>/sb/oauth2/token`

    **IMPORTANT**:

	{% include important.html content="
        The Token URL will be based upon the location of your Org and Space running on Bluemix public.
        <br/><br/>You can find your base URL by logging into the API Manager on Bluemix. Log into Bluemix and launch the API Manager, then navigate into your catalog (the default catalog created is `Sandbox`).
        <br/><br/>From there go into `Settings`. If you scroll down a bit you will see what your `API Endpoint` is called, simply copy and paste the contents into the token URL, then append `/oauth2/token`.
    " %}

    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab4/bmx-api-endpoint.png)

	{% include tip.html content="We are going to need the Token URL later. Go ahead and save the Token URL value to a text editor for easy access.
    " %}

1.  Click the `+` icon in the **Scopes** section to create a new scope. Set the following properties. Note the organization portion of the token URL will be different for each student.

    > Scope Name: `inventory`
    > 
    > Description: `Access to all inventory resources`
	
    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab4/3.png)

1.  Navigate to the `Security` section and check the `oauth (OAuth)` checkbox.  

    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab4/inv-security-addoauth.png)
	
1.  Save your changes.

    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab4/save-icon.png)
	
    Now that the API is secured using our OAuth provider, we can define how the API should behave when called. In the next two sections, we will configure the `inventory` API to call our inventory application which was published at the end of Lab 3.
	
### 4.4 - Defining API Processing Behavior
An API Assembly provides collection of policies which are enforced and executed on the API Gateway. Policies include actions like modifying the logging behavior and altering the message content or headers. Additionally, if the out of the box policies do not meet your specific needs, you may opt to create your own policy and have it available for API designers through the API Connect UI.

1.  Switch to the `Assemble` tab. A simple assembly has been created for you.  

    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab4/inv-assembly-start.png)

1.  Modify your assembly to use DataPower Gateway policies.
	
    Select the `DataPower Gateway policies` radio button.
	
    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab4/filter-datapowerpolicies.png)

1.  Add an `activity-log` policy to the assembly and configure it to log API payload.  

    Drag the `activity-log` policy from the list of available policies to the left of the `invoke` policy already created in your assembly.

1.  Select the newly added `activity-log` step. A properties menu will open on the right of your screen.

    Under `Content` select `payload` from the drop-down list.  
	
    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab4/inv-assembly-logpayload.png)

1.  Click on the `X` to close the activity-log editor menu


1.  Save your changes.

    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab4/save-icon.png)

### 4.5 - Validation

We will validate the inventory application by using an Oauth test app we have running in Bluemix.

1.  First, you need to attach the OAuth API to our `inventory` product.

    Click on the `<- All APIs` link, then click on the `Products` link to view the product list.

1.  Click on the `inventory 1.0.0` product to open it for editing.

    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab4/open-inv-product.png)

1.  Scroll down to the **APIs** section and click the `+` button.

1.  Select the `oauth` API to add it to the product and click the `Apply` button.

    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab4/add-oauth-api-to-product.png)

1.  Save the Product.

    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab4/save-product.png)

1.  Publish your API to Bluemix clicking on the `Publish` button.

    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab4/publish.png)

1.  Click on your Bluemix instance. **Note:** Each instance name is unique and will be different than the screenshot below.

    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab4/6.png)

1.  Select `Stage or Publish products` and then `Publish`.

    Make sure that the option to publish the Application is **NOT** selected.

    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab4/7.png)

1.  Open a new browser tab and navigate to your Bluemix environment and launch the API Manager.

1.  Click on the Sandbox catalog and navigate to the `Settings` tab.

    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab4/catalog-settings.png)
	
1.  Click on the `Show` button next to the Client ID and Client Secret fields in the **Automatic Subscription** section, we're going to need these values in the next few steps.

    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab4/show-client-creds.png)

1.  To test the API, we need to use a Web App to simulate the Oauth handshake. To do this, open a new browser tab and navigate to the following URL:

	[https://thinkibm-services.mybluemix.net/oauthtester](https://thinkibm-services.mybluemix.net/oauthtester)

1.  Enter in the following values into the Form
	
    > Token URL: `< your token url >`
    > 
    > Client Id: `< auto-subscribed client id >`
    > 
    > Client Secret: `< auto-subscribed client secret >`
    > 
    > Scope: `inventory`

    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab4/oauth-tester-form.png)

1.  Click the `Submit` button.

1.  The OAuth Tester will attempt to call your Token URL and obtain an OAuth token. Once it receives a token, the tool will also call the `/inventory/items` API with a query filter to return back the first two data sets. If everything worked properly, you will see the token and the API response, similar to this:
    
    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab4/oauth-test-success.png)

    {% include troubleshooting.html content="
        The OAuth Tester client is an Angular-based web client. Logs for the tool can be found in your browser's developer tool set.
        <br/><br/>
        You can use the Console, Network and Application Session storage features to trace the calls and logs.
    " %}
    
## Conclusion

**Congratulations!** You have successfully configured and secured the inventory API. You will consume this API in a later step.

Proceed to [Lab 5 - Advanced API Assembly](lab5.html)