---
title: Securing the Inventory API
toc: false
sidebar: labs_sidebar
folder: labs/lab3
permalink: /lab3_api_security.html
summary: Modify the security policy for your new API version to tell it to use your OAuth 2.0 provider.
applies_to: [developer]
---

## Apply an OAuth Security Policy

1.  Navigate to the `Security Definitions` section.

    Click the `+` icon in the **Security Definitions** section and select `OAuth` from the menu.
	
    ![](./images/labs/lab3/api-new-security-definition.png)
	
    A new security definition is created for you, called `oauth-1 (OAuth)`.

1.  Scroll down to edit the newly created security definition.

    Set it to have the following properties:
	
    > Name: `oauth`
    > 
    > Description: `Resource Owner Password Grant Type`
    > 
    > Flow: `Password`
    > 
    > Token URL: `<Catalog API Endpoint Base URL>/oauth2/token`

    **IMPORTANT**:

	{% include important.html content="
        The Token URL will be based upon the location of your Org and Space running on Bluemix public.
        <br/><br/>
        You can find your base URL by logging into the API Manager on Bluemix. Log into Bluemix and launch the API Manager, then navigate into your catalog (the default catalog created is `Sandbox`).
        <br/><br/>
        From there go into `Settings`. If you scroll down a bit you will see what your `API Endpoint` is called, simply copy and paste the contents into the token URL, then append `/oauth2/token`.
    " %}

    ![](./images/labs/lab3/bmx-api-endpoint.png)

	{% include tip.html content="We are going to need the Token URL later. Go ahead and save the Token URL value to a text editor for easy access.
    " %}

1.  Click the `+` icon in the **Scopes** section to create a new scope. Set the following properties. Note the organization portion of the token URL will be different for each student.

    > Scope Name: `inventory`
    > 
    > Description: `Access to all inventory resources`
	
    ![](./images/labs/lab3/api-oauth-settings.png)

1.  Navigate to the `Security` section and check the `oauth (OAuth)` checkbox.  

    ![](./images/labs/lab3/api-security.png)
	
1.  Save your changes.

    ![](./images/common/save.png)

1.  Click on the `<- All APis` link to return to the draft API list.

## Continue

Now you have a new version of the Inventory API that is secured using an OAuth provider. In the next lab, you will use the IBM API Connect Management Server's lifecycle controls to replace the running version 1.0.0 with the new version 2.0.0.

Proceed to [Lab 4 - Use Lifecycle Controls to Version your API](lab4_overview.html).