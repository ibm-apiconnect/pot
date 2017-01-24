---
title: Lab 5 - Advanced API Assembly
toc: true
sidebar: labs_sidebar
permalink: /lab5.html
summary: In this lab, you will learn how to create advanced API assemblies. Using the graphical design tools, you will create a new API called `financing` which will expose an existing SOAP service as a RESTful API. Additionally, you will create another API called `logistics` which connects to existing public services and uses the assembly tools to map responses into a desired format.
---

## Objective

In the following lab, you will learn:

+ How to create a new API, including object definitions and paths
+ How to configure an API to access an existing SOAP service
+ How to import an existing API from Open API definition document (a.k.a Swagger)
+ How to map data retrieved from multiple API calls into an aggregate response
+ How to use gatewayscript directly within an API assembly

## Case Study Used in this Tutorial

In this tutorial, you will expand the product offerings for **ThinkIBM**. In addition to the Inventory API, **ThinkIBM** wishes to provide API's that offer financing and shipping logistics to consumer applications. Your goal is to utilize existing enterprise and public assets to create these API offerings.

## Step by Step Lab Instructions

### 5.1 - Create the Financing API (REST to SOAP)

1.  If the API Designer screen has not already been launched, open a terminal and start the designer by issuing the following commands:

    ```bash
    cd ~/ThinkIBM/inventory
    apic edit
    ```

1.  Sign in with your registered Bluemix account.

1.  Otherwise, click the `All APIs` link to return to the main Designer screen.

#### 5.1.1 - Create the API Definition

1.  Click on the `+ Add` button and select `New OpenAPI from scratch`.

    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab5/fin_add_new.png)

1.  Fill in the form values for the API, then click the `Create API` button to continue.

    > Title: `financing`
    >
    > Name: `financing`
    >
    > Version: `1.0.0`
    
    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab5/fin_api_info.png)

1.  API Connect will generate a new OpenAPI definition file for the `financing` API and automatically load the API editor screen. Notice that the API does not contain any paths or data definitions. We will be adding these in the following steps.

{% comment %}
1.  Add the new financing API to an existing product.  Click on the "more" elipse (...) at the right of the screen.  Choose `Add to existing products` from the menu.

    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab5/addtoproduct.png)
 
1.  Select the `inventory` app checkbox and then select `add`

    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab5/addtoproduct2.png)

1.  Click on the `Schemes` menu option, or select it from the API editor menu. Enable the `https` scheme.

    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab5/fin_schemes.png)

1.  Click on `Host` from the API editor menu. Remove `$(catalog.host)` from the Host field. We will keep this blank.

    {% include troubleshooting.html content="The host field will show a red line indicating that the field is required. You may ignore this message.
    " %}
    
    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab5/fin_no_host.png)
    
1.  Click on `Base Path` menu option and ensure the base path is properly set to `/financing`.

    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab5/fin_api_basepath.png)

1.  Click on the the `Consumes` menu option. Notice that `application/json` is pre-selected for "Consumes and Produces".  No changes are necessary.

    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab5/fin_consumes_produces.png)
{% endcomment %}

1.  Next, we need to create the model definition for our new API. These definitions are used in a few places. Their primary role is to serve as documentation in the developer portal on expected input and output parameters; however, they can also be used for data mapping actions. Click on `Definitions` from the API Designer menu.

1.  Click the `+` icon in the **Definitions** section to create a new definition. Then, click on `new-definitions-1` to edit the new definition.

1.  Edit the `Name` of the definition, set it to `paymentAmount`.

1.  The new definition already adds in a sample property called `new-property-1`. Edit the property values:

    > Property Name: `paymentAmount`
    >
    > Description: `Monthly payment amount`
    >
    > Type: `float`
    >
    > Example: `199.99`
	
    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab5/fin_definition_complete.png)

1.  Now that we have a definition, we'll create a path. Click on `Paths` from the API Designer menu.

1.  Click on the `+` button to create a new path. The template will generate the path and a GET resource under the path. This is sufficient for our needs, but we could also add other resources and REST verbs to our path if needed.

1.  Edit the default path location to be `/calculate`.

	+ Recall that our Base Path for this API is `/financing`. This new path will be appended to the base, creating a final path of `/financing/calculate`.

1.  Click on the `GET` operation to expand the configuration options for the resource.

1.  Next, we have the option of adding request parameters to the operation. This defines the input to the API request. Since this is a GET request, we'll add the required request parameters to the query component of the URI.

	Click on the `Add Parameter` link to create a new query parameter. Then, select `Add new parameter` from the sub-menu.

1.  We're actually going to need three total parameters for this operation, so go ahead and click on the `Add Parameter` link two more times to add the parameter templates.

    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab5/fin_parameter_template.png)

1.  Edit the parameters to set the values:

    |Name    |Located In|Description             |Required|Type   |
    |--------|----------|------------------------|--------|-------|
    |amount  |Query     |amount to finance       |yes     |float  |
    |duration|Query     |length of term in months|yes     |integer|
    |rate    |Query     |interest rate           |yes     |float  |
	
    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab5/fin_parameter_complete.png)

1.  Next we'll set the schema for the response. Since we already defined the `paymentAmount` definition, we will select it from the drop down list. You will find the `paymentAmount` definition at the top of the list.

    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab5/fin_resp_schema.png)

1.  Next, click on `Services` from API Designer menu to load a service as policy that will be used in the assembly view later on.

1.  Click the `+` icon in the **Services** section to import web service from WSDL.

    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab5/fin_service.png)

1.  Click the `Load from URL` icon and enter the WSDL URL below and then click `Next`:

    [https://thinkibm-services.mybluemix.net/financing/calculate?wsdl](https://thinkibm-services.mybluemix.net/financing/calculate?wsdl)

    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab5/fin_service_wsdl.png)

1.  Click the `Show operations` to see the available operations in the WSDL end point. Select `financingService` then click `Done`.

    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab5/fin_service_wsdl_operation.png)

1.  Save the API definition.

    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab5/save-icon.png)

1.  You have completed the creation of the new API definition. The path and model data will be presented to our consumers on the developer portal once it's published.

#### 5.1.2 - Build the Financing API Assembly

1.  Click on the `Assemble` tab to access the assembly editor.

    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab5/fin_assemble_tab.png)

1.  Select the `DataPower Gateway policies` filter.

    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab5/fin_gateway_policies.png)

1.  In the processing pipeline, mouse over the `invoke` policy and click the trash icon to delete it.

    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab5/fin_assembly_delete_invoke.png)

1.  Scroll down to the bottom of the policy menu, drag and drop the `financing` web service operations to processing pipeline.

    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab5/add-financing-assembly.gif)

1.  Now we are going to modify the input and output `map` policy for mapping our REST API into SOAP.

1.  In order to consume a SOAP-based service from our REST-based API, we need to map the query parameter inputs that we defined as part of the `GET /calculate` operation to a SOAP payload. To do so, click on the `financing: input` map policy on our pipeline to open the map editor.

1.  Click on the `+` icon to make the editor window fill the screen.

    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab5/fin_map_fullscreen.png)

1.  On the **Input** column, click on the `pencil` icon to bring up the input editor.

    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab5/input-pencil-icon.png)

1.  Recall that our GET operation has three required query parameters: `amount`, `duration` and `rate`. Click on the `+ input` button three times to add the entries to the input table.

1.  Fill in the values for each of the input parameters:

    |Context variable           |Name    |Content type|Definition|
    |---------------------------|--------|------------|----------|
    |request.parameters.amount  |amount  |none        |float     |
    |request.parameters.duration|duration|none        |integer   | 
    |request.parameters.rate    |rate    |none        |float     |
	
    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab5/fin_assembly_map_input.png)

1.  Click on the `Done` button to return to the map editor.

1.  For each of the `Input` query parameters, map them to their respective SOAP `Output` elements.

	To map from an input field to an output field, click the circle next to the *source* element once, then click the circle next to the *target* element. A line will be drawn between the two, indicating a mapping from the source to the target.

    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab5/fin_assembly_map_input_to_output.png)

1.  Click the `X` button in the map editor to return to the policy pipeline.

1.  Click the `invoke` policy to open its editor.

1.  The SOAP service `URL` has already been set for you during the service import when we create the API.

    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab5/fin_assembly_invoke.png)

1.  Click the `X` button to return to the policy pipeline.

1.  After we invoke the SOAP service, we need to map the SOAP response into a custom object `paymentAmount`, which is created previously in API definition, as API response.  To do so, click on the `financing: output` map policy on our pipeline to open the map editor.

1.  Click on the pencil icon to create a new output object.

1.  Click on the `+ output` button.

1.  Fill in the values for the output object:

    > Context variable: `message.body`
    >
    > Name: `paymentAmount`
    >
    > Content type: `none`
    >
    > Definition: `#/definitions/paymentAmount`
		
    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab5/fin_assembly_map_output.png)

1.  Click on the `Done` button to return to the map editor.

1.  To map SOAP response to custom object `paymentAmount`, click the circle next to the *source* element once, then click the circle next to the *target* element. A line will be drawn between the two, indicating a mapping from the source to the target.

    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab5/fin_assembly_map_output_link.png)

1.  Click the `X` button in the map editor to return to the policy pipeline.

1.  You are now finished with the assembly for the `financing` API. The assembly takes the following actions:

    + Maps the REST query parameters into a SOAP body.
    + Invokes the SOAP service.
    + Transforms the SOAP service's response into JSON.

1.  Save the API definition.

    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab5/save-icon.png)

### 5.2 - Add Logistics API (Advanced Assembly)

In this lab section, we will be adding a new API called `logistics` which will provide helper services around calculating shipping rates and locating nearby stores.
 
Rather than require you to build the entire API from scratch again, you will see how you can use the source editor to paste in an API definition that has already been started for you.  

#### 5.2.1 - Import Predefined API Definition

1.  Click on the `All APIs` link to return to the main API Designer screen.

1.  Click on the `+ Add` button to `Import an existing OpenAPI`.

    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab5/import-logistics-api.png)

1.  Click on the `Or import from URL...` link, enter the `logistics` API definition template URL below and click the `Import` button:

    [https://raw.githubusercontent.com/ibm-apiconnect/pot/gh-pages/assets/lab5/logistics_1.0.0.yaml](https://raw.githubusercontent.com/ibm-apiconnect/pot/gh-pages/assets/lab5/logistics_1.0.0.yaml)

    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab5/logistics_api_import.png)

#### 5.2.2 - Create the Logistics API Assembly

1.  Switch to the `Assemble` tab and click the `Create assembly` button.

1.  Add an `activity-log` policy to the assembly pipeline.

1.  Click the `activity-log` policy to open editor, configure it to log the `payload` for the **Content** field.

1.  Add an `operation-switch` policy to the right of the activity-log step.

1.  Click on the `operation-switch` policy to launch the editor. A single case `case 0` is created by default.

1.  Click `search operations...` to bring up the drop-down list of available operations.

1.  Select the `shipping.calc` operation.

1.  Click the `+ Case` button to add a second case for the `get.stores` operation.

1.  Click the `X` to close the operation switch configuration editor.

    You should see two new processing pipelines created on your `operation-switch` step - one for each case:  
    
    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab5/logistics-twopipelines.png)

##### 5.2.2.1 - Configure the `shipping.calc` Case:

This operation will end up invoking two separate back-end services to acquire shipping rates for the respective companies, then utilize a map action to combine the two separate responses back into a single, consolidated message for our consumers.

1.  Add an invoke policy to the `shipping.calc` case with the following properties:

    > Title: `invoke_xyz`
    >
    > URL: `$(shipping_svc_url)?company=xyz&from_zip=90210&to_zip={zip}`
    >
    > Stop on error: `unchecked`
    >
    > Response object variable (scroll to the bottom): `xyz_response`
    
    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab5/logistics_invokexyz1.png)
    
    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab5/uncheck-stop-on-error.png)
    
    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab5/logistics_invokexyz2.png)
    
    {% include note.html content="The `{zip}` parameter provided here is a reference to the `zip` parameter defined as input to the operation. The `{zip}` portion of the URL will get replaced by the actual zip code provided by then API consumers.
    " %}

1.  Hover over the `invoke_xyz` policy and click on the `clone` button to add another invoke action:

    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab5/clone-invoke-action.png)

1.  Edit the new invoke policy with the following properties:

    > Title: `invoke_cek`
    >
    > URL: `$(shipping_svc_url)?company=cek&from_zip=90210&to_zip={zip}`
    >
    > Response object variable: `cek_response`
	
    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab5/logistics_invokecek1.png)
	
    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab5/logistics_invokecek2.png)

1.  Add a `map` policy after the last invoke, then click it to open the editor.

1.  Click the `gear` icon next to Map and specify the following map properties.  Click `done` when finished:
  
    > Title: `map_responses`
    >
    > Description: `Map responses from invoke_xyz and invoke_cek to output`
	
    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab5/logistics-map-properties.png)

    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab5/logistics-map-propertiesa.png)

1.  Click the pencil button to add an input. Specify the following input configuration:
  
    > Context variable: `xyz_response.body`
    >
    > Name: `xyz`
    >
    > Content type: `application/json`
    >
    > Definition: `Inline schema`

1.  After you select `Inline schema`, you will be prompted to "Provide a schema".
	
    Copy the following predefined schema from the link below into the schema editor window.
    
    [https://raw.githubusercontent.com/ibm-apiconnect/pot/gh-pages/assets/lab5/schema_shippingSvc.yaml](https://raw.githubusercontent.com/ibm-apiconnect/pot/gh-pages/assets/lab5/schema_shippingSvc.yaml)
    
    ```yaml
    $schema: 'http://json-schema.org/draft-04/schema#'
    id: 'http://jsonschema.net'
    type: object
    properties:
      company:
        id: 'http://jsonschema.net/company'
        type: string
        name: company
      rates:
        id: 'http://jsonschema.net/rates'
        type: object
        properties:
          next_day:
            id: 'http://jsonschema.net/rates/next_day'
            type: string
            name: next_day
          two_day:
            id: 'http://jsonschema.net/rates/two_day'
            type: string
            name: two_day
          ground:
            id: 'http://jsonschema.net/rates/ground'
            type: string
            name: ground
        name: rates
    required:
      - company
      - rates
    ```

1.  Click `Done` to close the Schema window  
	
    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab5/logistics-map-xyz-schema.png)

1.  Click the pencil button again to add another input. Specify the following input configuration:
  
    > Context variable: `cek_response.body`
    >
    > Name: `cek`
    >
    > Content type: `application/json`
    >
    > Definition: `Inline schema`

1.  Paste the same schema definition that was used for the previous input (for our lab purposes, the responses from the shipping service are in the same format, thus using the same schema)

1.  You now have two inputs assigned to the `map` policy:

    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab5/logistics-map-responses-inputs.png)

1.  Click the `Done` button to return to the editor.

1.  Click the `pencil` icon next to `Output`, then click the `+ output` button to add an output field with the following properties:

    > Context variable: `message.body`
    >
    > Name: `output`
    >
    > Content type: `application/json`
    >
    > Definition: `#/definitions/shipping`  
	
    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab5/logistics-map-responses-output.png)

1.  Click the `Done` button to return to the editor.

1.  Complete the mapping. To map from an input field to an output field, click the circle next to the *source* element once, then click the circle next to the *target* element. A line will be drawn between the two, indicating a mapping from the source to the target.  

    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab5/logistics-map-complete.png)

1.  Click the `X` to close the map editor.

    Your assembly policy for the `shipping.calc` operation is now complete.
      
    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab5/logistics_assembly_shipping_calc_complete.png)
    
    {% include troubleshooting.html content="If there is an `exclamation mark` badge in `invoke_xyz`,  you may ignore this message.
    " %}
	
1.  Save your changes.

    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab5/save-icon.png)

##### 5.2.2.2 - Configure the `get.stores` Case:

This operation will call out to the Google Geocode API to obtain location information about the provided zip code, then utilize a simple gatewayscript to modify the response and provide a formatted Google Maps link.

1.  Add an invoke policy to the `get.stores` case with the following properties:

    > Title: `invoke_google_geolocate`
    >
    > URL: `https://maps.googleapis.com/maps/api/geocode/json?&address={zip}`
    >
    > Stop on error: `unchecked`
    >
    > Response object variable (scroll to the bottom): `google_geocode_response`  
	
    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab5/logistics_invokegeolocate1.png)
	
	![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab5/uncheck-stop-on-error.png)
	
    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab5/logistics_invokegeolocate2.png)

1.  Add a `gatewayscript` policy with the following properties:  
  
    > Title: `gws-format-maps-link`
    >
    > Paste the following predefined script from the link below into the code section of the policy.  
	
	[https://raw.githubusercontent.com/ibm-apiconnect/pot/gh-pages/assets/lab5/gws_formatMapsLink.js](https://raw.githubusercontent.com/ibm-apiconnect/pot/gh-pages/assets/lab5/gws_formatMapsLink.js)
	
    ```javascript
    // Require API Connect Functions
    var apic = require('local:///isp/policy/apim.custom.js');
    
    // Save the Google Geocode response body to variable
    var mapsApiRsp = apic.getvariable('google_geocode_response.body');
    
    // Get location attributes from geocode response body 
    var location = mapsApiRsp.results[0].geometry.location;
    
    // Set up the response data object, concat the latitude and longitude  
    var rspObj = {
      "google_maps_link": "https://www.google.com/maps?q=" + location.lat + "," + location.lng  
    };
    
    // Save the output  	
    apic.setvariable('message.body', rspObj);
    ```
	
    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab5/logistics-gws.png)
	
	{% include note.html content="Take a quick look at line 5. Notice how our gateway script file is reading the body portion of the `google_geocode_response` variable which was assigned to the output of the `invoke` action.
    " %}

1.  Click the `X` to close the gatewayscript editor.

1.  Your assembly for the `logistics` API will now include two separate operation policies:

    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab5/logistics_assembly_complete.png)

    {% include troubleshooting.html content="There may be an `exclamation mark` badge in `invoke_xyz` or `invoke_google`.  You may ignore this message.
    " %}

1.  Save your changes.

    ![](https://github.com/ibm-apiconnect/pot/raw/gh-pages/images/lab5/save-icon.png)

#### 5.2.3 - Move the Logistics Definition File

In this version of the APIC toolkit, our new Logistics API file is placed in a location apart from our other API definitions. We'll need to move it to ensure things are consistent.

1.  Return to the terminal editor. Stop the API Designer process:

    ```bash
    control+c
    ```

1.  Navigate to the `~/ThinkIBM/inventory` directory.

    {% include note.html content="In the following steps, the example commands are shown from a Linux terminal. You are free to move the file using the method of your choice.
    " %}

    ```bash
    student@ubuntu:~/ThinkIBM/inventory$ ls -l
    total 32
    drwxrwxr-x  3 student student 4096 Jan 24 13:09 common
    drwxrwxr-x  2 student student 4096 Jan 24 15:02 definitions
    -rw-rw-r--  1 student student 8873 Jan 24 14:51 logistics_1.0.0.yaml
    drwxrwxr-x 15 student student 4096 Jan 24 13:08 node_modules
    -rw-rw-r--  1 student student  729 Jan 24 13:12 package.json
    drwxrwxr-x  3 student student 4096 Jan 24 13:13 server
    ```

1.  Locate the `logistics_1.0.0.yaml` file and move it into the `~/ThinkIBM/inventory/definitions` directory.

    ```bash
    student@ubuntu:~/ThinkIBM/inventory$ mv logistics_1.0.0.yaml definitions/
    ```

1.  The `~/ThinkIBM/inventory/definitions` directory will now contain the configuration files for all our APIs:
    
    ```bash
    student@ubuntu:~/ThinkIBM/inventory$ ls -l definitions/
    total 60
    -rw-rw-r-- 1 student student  9475 Jan 24 14:35 financing_1.0.0.yaml
    -rw-rw-r-- 1 student student  1871 Jan 24 14:55 inventory-product.yaml
    -rw-rw-r-- 1 student student 17182 Jan 24 13:21 inventory.yaml
    -rw-rw-r-- 1 student student  8873 Jan 24 14:51 logistics_1.0.0.yaml
    -rw-rw-r-- 1 student student  8868 Jan 24 13:20 oauth_1.0.0.yaml
    ```

## Conclusion

**Congratulations!** You have successfully configured two new API's with advanced assemblies. In the next lab, you will bundle the API's into a Product and publish it to the consumer portal.

Proceed to [Lab 6 - Working with API Products](lab6.html)