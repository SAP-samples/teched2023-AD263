# Exercise 2 - Prepare SAP Cloud ALM

Logon to the AD263 Cloud ALM Tenant:

<https://ad263-ptnlz9xc.eu10.alm.cloud.sap/launchpad>

Select the **tdct3ched1.accounts.ondemand.com** Identity Provider. Use the credentials as provided in Excercise 0.

![](001.png)

After the logon the Launchpad of Cloud ALM is displayed

![](002.png)

## Check Landscape Management Service

Navigate to Landscape Management from Cloud ALM Launchpad

![](003.png)

Open and define a scope in the Scope Selector for Landscape Management Service

![](004.png)

![](005.png)

1. Click: Toggle Filter Bar to show search criteria
1. Select Managed Object Type: Service, System
1. Click: Go to load the list of service types
1. Click: Select all in the result list to show all available service types
1. Click Apply to close the scope selection and show the services and systems

In the dashboard loaded you see an overview of all systems and services available in the SAP Cloud ALM tenant. The dashboard entry groups those by Managed Object Type. In each tile you see how many objects of this type are already configured and collecting data, how many are paused for monitoring and how many are new and have not yet been configured. Clicking on any type you can navigate to the list of individual Managed Objects of this type.

Continue the exercise by selecting the tile SAP BTP Cloud Foundry environment

![](006.png)

Select the SAP BTP CF AD263-XXX ***(replace XXX with your group number)*** service where your Demo Application is deployed

![](007.png)

Please review the details of the service

![](008.png)

## Check Health Monitoring for SAP BTP CF Demo Application

Navigate to Health Monitoring from SAP Cloud ALM Launchpad

![](009.png)

As part of the preparation of your managed SAP BTP CF service the Java application has been instrumented with the OpenTelemetry Java agent and the SAP OpenTelemetry Agent Extensions. As such a set of technical KPI’s for this application is automatically collected for the application to monitor the status of the application.

Open the scope selection and define a scope to show only relevant managed objects for this excercise:

![](010.png)

![](011.png)

1. Click: Toggle Filter Bar
1. Select Service Type: SAP BTP, Cloud Foundry environment
1. Click Go to load the list of services
1. Filter the list by searching for AD263-XXX ***(replace XXX with your group number)***
1. Check the line with AD263-XXX ***(replace XXX with your group number)***
1. Click on Apply to active this scope

As result you should see the entry dashboard for Health Monitoring only with one tile for SAP BTP, Cloud Foundry environment and one service.

![](012.png)

Navigate to SAP BTP Cloud Foundry environment and select the service AD263-XXX ***(replace XXX with your group number)*** where your Demo Application is deployed

![](013.png)

In the following screen you can see all metrics collected for the health status of the application. For details each metric can be evaluated further and history data for the metric can be shown

![](014.png)

If multiple applications or multiple instances are deployed then for each application/instance a separate measurement is visible. For the application Teched2023Incident (this is the application name as provided in the application instrumentation) please review the history

![](015.png)

History Chart can reviewed for the metric

![](016.png)

Please check other metrics for this application. All metrics shown here are available by default and are provided as part of the SAP Otel extension package as automatic instrumentation. There is no development needed to enable those metrics.

## Configure Integration & Exception Monitoring

Navigate to Integration & Exception from Cloud ALM Launchpad

![](017.png)

The application opens the first time with a scope selection. The scope selected will be stored for your user and next time you open the application the same scope is automatically used

![](018.png)

1. Opens the scope selector (at first application start it is opened automatically)
1. Managed Component Selection: Select Business Services, Services and Systems
1. Service Status: Select All (by default only services configured in Integration and Exception Monitoring are selected, as you configure a new service you need to select All)
1. Click on Go to load the list of managed components according to the given filter criteria
1. Select Managed Component AD263-XXX ***(replace XXX with your group number)***
1. Click on Apply to Load the application with the selected scope

The application starts with an overview page showing the status of all managed components in your scope

![](019.png)

As AD263-XXX ***(replace XXX with your group number)*** was not yet used or configured for Integration and Exception Monitoring the configuration for this service needs to be started

![](020.png)

1. Click on the configuration button
1. Select your BTP CF service AD263-XXX ***(replace XXX with your group number)***<br>
Navigate to the Monitoring configuration of your service
![](021.png)<br>
![](022.png)

Toggle the Active Switch for Category Java Application Logs and click save. This enables the data collection of Java Application logs for all customer created Java application in this BTP CF subaccount. The configuration is transferred to the subaccount and data collection and transfer of data starts within the next minute.

Without this toggle switch set to active, even though the application is instrumented and might create errors, the errors are not transferred to SAP Cloud ALM. This mechanism is in place to ensure that only data for metrics which are used in SAP Cloud ALM are collected and transferred, especially as such transfer happens also across data centers as one Cloud ALM tenant manages all customer services independent of the service location.

Switch view to Events settings

![](023.png)

Add new Event for `Erroneous Java Application`

![](024.png)

Activate `Create Alert` as Event Action and Save

With this setting each time new exceptions are detected in your custom application a new alert is created in the alert inbox for Cloud ALM.

## Trigger Exception

![](025.png)

In your BTP space a sample java application has been deployed, which can create exceptions on user requests. This is a mock application to simulate exception creations from customer developed PaaS applications.

- Please click the button Create Exception to raise some example exceptions from this Application

## Review Exception in Cloud ALM

Navigate to the Home of Integration & Exception Monitoring

![](026.png)

1. Click on the Refresh Indicator
1. Click Refresh to manually refresh the current page (by default the current view is refreshed every 5 minutes automatically). The data transfer from your application to Cloud ALM can take up to two minutes
1. Click on the Exceptions to show the exception details<br>
![](027.png)
1. Click on one line to review details of a single exception<br>
![](028.png)
1. Review the Collection Context
1. Review the Correlation Context
1. Review the Process Arguments
1. Navigate to Cloud Logging Service to see the details in local dashboard

## Review Alert for the Exception in Cloud ALM

![](029.png)

1. Click on alerting to open the alert page
1. If the list is still empty refresh the view, select one alert
1. Review the alert details

## Summary
- You have now access to the Cloud ALM tenant as central observability solution for this Hands-on session.
- Your managed customer created Java application deployed in BTP CF is available and configured for monitoring in Heralth Monitoring ansd Integration & Exception Monitoring in SAP Cloud ALM
- Exceptions from the Demo Application are creating events and alerts in SAP Cloud ALM

