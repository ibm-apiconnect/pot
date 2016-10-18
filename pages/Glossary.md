---
title: Glossary
toc: false
keywords:
tags:
sidebar: gs_sidebar
permalink: glossary.html
summary:
---

See also:

- [Full API Connect glossary](http://www.ibm.com/support/knowledgecenter/SSMNED_5.0.0/com.ibm.apic.overview.doc/overview_apimgmt_glossary.html)
- [LoopBack glossary](http://loopback.io/doc/en/lb2/Glossary.html)

apic
: The IBM API Connect command-line tool.  It provides commands to ...

assembly
: Application programming interface that provides rich functionality for interacting with an application: makes side calls to external services and then transforms and aggregates the response before a response is relayed to the calling application.

API definition
: Specification of a REST API encapsulated by an OpenAPI (Swagger) file, in YAML format.
Includes enumeration of paths (endpoints), parameters, responses, security configuration, and other API settings.

API Designer
: Graphical tool in the API Connect developer toolkit for creating, editing, and publishing APIs and LoopBack projects.

API Proxy
: Application programming interface that forwards requests to a user-defined back-end resource and relays responses back to the calling application.

assembly
: Application programming interface that provides rich functionality for interacting with an application: makes side calls to external services and then transforms and aggregates the response before a response is relayed to the calling application.

Catalog
: A staging target that behaves as a logical partition of the gateway and the Developer Portal. The URLs for API calls and for the Developer Portal are specific to a particular catalog.

gateway
:  Software component that processes and manages security protocols and stores relevant user and appliance authentication data and provides assembly functions that enable APIs to integrate with endpoints such as databases or HTTP-based endpoints.  You can use two gateways with API Connect:

- Micro Gateway is built on Node.js, and provides enforcement for the authentication, authorization, and flow requirements of an API.
- DataPower Gateway is deployed on a virtual or physical DataPower appliance and has more policies available to it than the Micro Gateway and can handle enterprise level complex integration.

microservice
: Approach for service-oriented architectures (SOA) used to build flexible, fine-grained,  independently-deployable software systems that communicate with each other over a network.

OpenAPI
:  Specification (also known as the Swagger specification) for machine-readable interface files for describing, producing, consuming, and visualizing RESTful web services.

LoopBack
: Highly-extensible, open-source Node.js framework based on Express that enables you to create dynamic end-to-end REST APIs with little or no coding.  LoopBack applications consist of models, and application logic, and are connected to back-end data stores through data source connectors.

Plan
: Packaging construct by which APIs are made available to developers. A Plan makes available a collection of operations from one or more APIs, and is published to communities of application developers. Application developers gain access to APIs by registering applications to access Plans.
A Plan carries with it a collection of policy settings. In the simplest form, a Plan defines a single quota policy that applies to all the API operations that are accessed through the Plan. In more advanced cases, additional policies can be associated with a Plan.

Product
: A grouping of APIs intended for a particular use. Products contain Plans, which can be used to differentiate between different offerings. You can create Plans only within Products, and these Products are then published in a Catalog.

Project
: TBD

Swagger
: See _[OpenAPI](#openapi)_.

YAML
: Human-readable data serialization language that API Connect uses to encapsulate _[API definitions](#api-definition)_.
