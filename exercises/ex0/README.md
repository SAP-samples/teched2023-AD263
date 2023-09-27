Reliability plays a central role, especially in hybrid landscapes that contain software-as-a-service (SaaS) cloud services, managed cloud components, and on-premise components. The different components involved must be monitored to maintain reliable operations at the application level. In case of an alert, a swift and ideally automatic reaction is required. SAP Cloud ALM provides monitoring applications for various components running in the cloud as well as on-premise.
# **Exercise 0 – Getting Started**
## ***Connect to SAP BTP CF Subaccount***
To access all BTP accounts please logon with the credentials to the BTP Global Account for TechEd 2023 Hands-On Sessions:

<https://emea.cockpit.btp.cloud.sap/cockpit/?idp=tdct3ched1.accounts.ondemand.com#/globalaccount/e2a835b0-3011-4c79-818a-d7767c4627cd/>

Your trainer will provide:

- Username: <AD263-XXX@education.cloud.sap> *(replace XXX with your group number)*
- Password: <the password is handed out during the session by your trainer)

Please connect via browser to the provided URL and use your credentials to log in.

![](001.png)

After successful logon you see SAP BTP Cockpit global account overview two different subaccounts:

- AD263: this is the subaccount where SAP Cloud ALM and SAP BTP Alert Notification service is deployed
- AD263-XXX: this is the subaccount with the managed PaaS application used during the hands-on exercise

![ref1]

Select Subaccount: AD263-XXX *(replace XXX with your group number)*
## ***Check Demo Application (Optional)***
### *Check Connectivity from SAP BTP CF to SAP Cloud ALM (optional)*
Navigate to Connectivity -> Destinations

![](003.png)

Verify that the Destination CALM\_datacollector\_AD263-XXX *(replace XXX with your group number)* is created as shown below. Please do not change the destination!

![](004.png)

Execute Check Connection

![](005.png)

Please make sure connection check is successful. The response “404: Not found” is normal as the API endpoint does not provide a default URL.
### *Check Cloud ALM Intrumentation for Demo Application (optional)*
## ***Trigger Exception from Demo Application***
Navigate to your BTP subaccount AD263-XXX

![ref1]

Navigate to Cloud Foundry -> Spaces

![](006.png)

Open Space AD263-XXX

![](007.png)

Here you see 3 deployed applications

![](008.png)

- tech-ed-svc: java backing service
- wa-approuter: managed approuter
- waapp: UI5 frontend

Navigate to the UI5 frontend application

![](009.png)

Klick on the link to open the Incident Mock Demo application

![](010.png)

In your BTP space a sample java application has been deployed, which can create exceptions on user requests. This is a mock application to simulate exception creations from customer developed PaaS applications.

- Please click the button Create Exception to raise some example exceptions from this Application

The very same application is also used as mock ticketing system, which means tickets can be created in the application by a remote call and the ticket can be shown here. Please keep the application window open for the remainder of this hands-on session.

> [!NOTE]
> How exceptions are raised in the customer applications: 

Exceptions in this sample application are created with slf4j logging facility. The generic approach to raise an exception is done as shown in the code snippet below

```
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

Logger logger = LoggerFactory.getLogger(loggerName);
logger.error(message)
```

Note: Furthermore, this application can display incidents created by other tools. In customer environments incident management can be handled by a broad variety of Incident Management Tools provided by different vendors. In this hands-on session we will use this mock application to simulate an Incident Management tool with limited functionality. Please note that SAP does not offer or recommend any specific solution.

To learn more about how to develop and instrument your customer applications please see session “XP261 – Observability for Your SAP BTP Applications with SAP Cloud ALM”
## ***Review Exception from Demo Application in Cloud Logging Service***
The log messages created by the sample application are instrumented to send the generated exceptions to Cloud Logging Service. In Cloud Logging Service the local observability and detailed exception analysis can be performed. For the scenario of sending the exception information from BTP CF PaaS Applications to Cloud ALM the Cloud Logging Service is optional. The data forwarding to SAP Cloud ALM works independently.

<a name="_toc146285036"></a>To show the generated exceptions logon with the credentials to your subaccount AD263-XXX of the the BTP Global Account for TechEd 2023 Hands-On Sessions:

<https://emea.cockpit.btp.cloud.sap/cockpit#/globalaccount/e2a835b0-3011-4c79-818a-d7767c4627cd>

Navigate to your subaccount AD263-XXX *(replace XXX with your group number)*

![](011.png)

1. Click on Instances and Subscriptions
1. Navigate to the details of Instance CLS by clicking on the arrow on the right side of the instance
1. Click on Button view Credentials ![](012.png)
1. Copy the values for dashboard-username and dashboard-password into Notepad on your laptop. You need them in the next step to logon to the dashboard
1. Click on the instance name cls to open the Logging Service dashboard![](013.png)
1. ` `Logon with the credentials you retrieved in the previous step ![](014.png)
1. Select the tenant as ‘Global’ 

![](015.png)

1. Navigate in the Dashboard to discover ![](016.png)
1. Review the Exceptions ![](017.png)

### ***Summary***
- You have now access to the Demo Application for this Hands-on session.
- You are able to trigger exceptions on demand to simulate erroneous behavior of a customer developed BTP CF Java application
- You are able to view incidents created by SAP Cloud ALM or SAP BTP Alert Notification Service



[ref1]: 002.png
