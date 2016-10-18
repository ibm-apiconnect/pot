# Getting Started on API Connect

**Learn the basics of API Connect by using these guides to build micro services and api proxies.**

NOTE: _Must use DataPower GW_

## Building an API Proxy

### Creating your first Assembly

1. Create API
   - Define Swagger definition 
   - Define operation
   - What this does: Creates Product, creates client ID policy, creates invoke policy
1. Wire up proxy
   - Edit host and path API properties.
   - Path
   - Parameters
   - Response defn.
1. Run using test tool
   - Select product
   - Select catalog and test app
   - Select operationand params
   - Expand debug pane
1. `curl` the new endpoint.  

Deploying Assemblies to Bluemix

### Exposing SOAP services as REST APIs

1. Create API defn.
1. Create target service
   - Provide URL of WSDL file
1. Create in the Swagger API defn.
1. Create operation switch
   - Add case for getForecastByZip (operation defined in swagger)
1. Add map for request
   - Select payload defn. left side, right side
   - zip, weatherCit, Country
1. Add invoke policy 
   - http://weather.api.yahoo.com
   - Set method to POST
   - Add set-variable
     - Header
     - SOAP OpID (TBD)
   - Add response map (zip -> zip, weatherCity -> city, XML -> json)

### Invoking a Micro Service from an API Proxy

TBD

## Building Micro Services

### Creating a Hello World Micro Service

1. Create memory data source
1. Create a model and properties
1. Choose memory data source
1. Test with Explorer
   - Show the generated Swagger

### Orchestrating SOAP services with LoopBack (nb. see existing blog)

1. Create LoopBack app.
1. Create datasource and choose SOAP connector
1. Choose models to create (not yet in UI, CLI only)
1. Choose methods to create (not yet in UI, CLI only)
1. Test using the Explorer.
   - Show the generated Swagger.

### Deploying LoopBack Applications to Bluemix

Link to existing docs.
  
## Organizing APIs

### Version controlling your API Connect Project

### Creating your first Product and API Definition

### Publishing a Product to a Catalog on Bluemix
  
## Glossary

  - `apic` CLI
  - API Definition
  - API Designer
  - API Proxy
  - Assembly
  - Gateway
  - Micro Service
  - Open API
  - LoopBack
  - Product
  - Project
  - Swagger
  - YAML
