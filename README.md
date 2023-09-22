[![REUSE status](https://api.reuse.software/badge/github.com/SAP-samples/teched2023-AD263)](https://api.reuse.software/info/github.com/SAP-samples/teched2023-AD263)
# AD263 - Configure Alerting for SAP BTP Using SAP Cloud ALM!


## Description

This repository contains the material for the SAP TechEd 2022 session called AD263 - Configure Alerting for SAP BTP Using SAP Cloud ALM.

Get an overview regarding the coverage of the SAP Cloud ALM tool for use with the different components of SAP Business Technology Platform (SAP BTP). Configure some of the use cases, from configuration of data collection down to the integration of third-party consumers such as ticket systems. Learn how you can replace existing scenarios of SAP Alert Notification service for SAP BTP with SAP Cloud ALM. See how to enhance, customize, and troubleshoot integration to third-party event consumers.

## Overview

This session introduces attendees to the usage of SAP Cloud ALM as central observability solution for your whole SAP Solution Landscape. Escpecially in this session we will focus on the capabilities for customer developed applications in SAP BTP Cloud Foundry. You will learn how connect a PaaS application to SAP Cloud ALM, how to navigate Health Monitoring and Integration a& Exception Monitoring and how to set up alerts and notifications. To react to alerts, you will learn how to forward the event situations from SAP Cloud ALM to external 3rd party incident management tools.

## Requirements

The requirements to follow the exercises in this repository

Very basic knowledge in BTP Cockpit
Please use the Chrome browser on your Teched laptop

## Exercises

Provide the exercise content here directly in README.md using [markdown](https://guides.github.com/features/mastering-markdown/) and linking to the specific exercise pages, below is an example.

- [Getting Started](exercises/ex0/)
- [Exercise 1 - Prepare SAP BTP CF Subaccount](exercises/ex1/)
    - [Exercise 1.1 - Connect to SAP BTP CF Subaccount](exercises/ex1#exercise-11-sub-exercise-1-description)
    - [Exercise 1.2 - Check Demo Application](exercises/ex1#exercise-11-sub-exercise-1-description)
    - [Exercise 1.3 - Check Connectivity from SAP BTP CF to SAP Cloud ALM (optional)](exercises/ex1#exercise-12-sub-exercise-2-description)
    - [Exercise 1.4 - Check Cloud ALM Intrumentation for Demo Application (optional)](exercises/ex1#exercise-12-sub-exercise-2-description)
    - [Exercise 1.5 - Trigger Exception from Demo Application](exercises/ex1#exercise-12-sub-exercise-2-description)
    - [Exercise 1.6 - Review Exception from Demo Application in Cloud Logging Service](exercises/ex1#exercise-12-sub-exercise-2-description)
- [Exercise 2 - Prepare SAP Cloud ALM](exercises/ex2/)
    - [Exercise 2.1 - Check Landscape Management Service](exercises/ex2#exercise-21-sub-exercise-1-description)
    - [Exercise 2.2 - Check Health Monitoring for BTP CF Demo Application in your BTP subaccount](exercises/ex2#exercise-22-sub-exercise-2-description)
    - [Exercise 2.3 - Configure Integration and Exception Monitoring and Alerting for your BTP CF Demo application](exercises/ex2#exercise-21-sub-exercise-1-description)
    - [Exercise 2.4 - Trigger Exception from Demo Application](exercises/ex2#exercise-21-sub-exercise-1-description)
    - [Exercise 2.5 - View Exception in Cloud ALM Exception Monitoring](exercises/ex2#exercise-21-sub-exercise-1-description)
    - [Exercise 2.6 - View Alert in Cloud ALM Exception Monitoring](exercises/ex2#exercise-21-sub-exercise-1-description)
- [Exercise 3 - Configure Integration with External Ticket Management System from Cloud ALM](exercises/ex2/)
    - [Exercise 3.1 - Create Destination in SAP Cloud ALM BTP Cockpit](exercises/ex2#exercise-21-sub-exercise-1-description)
    - [Exercise 3.2 - Create Mapping](exercises/ex2#exercise-22-sub-exercise-2-description)
    - [Exercise 3.3 - Create Subscription](exercises/ex2#exercise-22-sub-exercise-2-description)
    - [Exercise 3.4 - Test Ticket creation from Alert](exercises/ex2#exercise-22-sub-exercise-2-description)
    - [Exercise 3.5 - Enable automatic Ticket creation for Exception Management Events](exercises/ex2#exercise-22-sub-exercise-2-description)
- [Exercise 4 - End-to-End Execution](exercises/ex2/)
    - [Exercise 4.1 - Trigger Exception from Demo Application](exercises/ex2#exercise-21-sub-exercise-1-description)
    - [Exercise 4.2 - View Alert in Cloud ALM Exception Monitoring](exercises/ex2#exercise-21-sub-exercise-1-description)
    - [Exercise 4.3 - Jump from Alert to Ticket](exercises/ex2#exercise-22-sub-exercise-2-description)
    - [Exercise 4.4 - Review created Ticket in Demo Ticket Application](exercises/ex2#exercise-22-sub-exercise-2-description)
    - [Exercise 4.5 - Start Processing of Ticket in Demo Ticket Application](exercises/ex2#exercise-22-sub-exercise-2-description)
    - [Exercise 4.6 - Review Alert Status after Ticket was set to in Progress](exercises/ex2#exercise-22-sub-exercise-2-description)
- [Exercise 5 - Troubleshooting](exercises/ex2/)
    - [Exercise 5.1 - Intelligent Event Processing Execution Log](exercises/ex2#exercise-21-sub-exercise-1-description)
    - [Exercise 5.2 - Intelligent Event Processing Event Paload Trace (optional)](exercises/ex2#exercise-21-sub-exercise-1-description)
    - [Exercise 5.3 - External API Management Console](exercises/ex2#exercise-22-sub-exercise-2-description)


Start the exercises [here] (https://ad263-ptnlz9xc.eu10.alm.cloud.sap/launchpad#Shell-home)).

**IMPORTANT**

Your repo must contain the .reuse and LICENSES folder and the License section below. DO NOT REMOVE the section or folders/files. Also, remove all unused template assets(images, folders, etc) from the exercises folder. 

## Contributing
Please read the [CONTRIBUTING.md](./CONTRIBUTING.md) to understand the contribution guidelines.

## Code of Conduct
Please read the [SAP Open Source Code of Conduct](https://github.com/SAP-samples/.github/blob/main/CODE_OF_CONDUCT.md).

## How to obtain support

Support for the content in this repository is available during the actual time of the online session for which this content has been designed. Otherwise, you may request support via email to [cloudalm@sap.com](mailto:cloudalm@sap.com).

## License
Copyright (c) 2022 SAP SE or an SAP affiliate company. All rights reserved. This project is licensed under the Apache Software License, version 2.0 except as noted otherwise in the [LICENSE](LICENSES/Apache-2.0.txt) file.
