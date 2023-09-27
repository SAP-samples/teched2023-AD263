# Exercise 3 - Configure Integration with External Ticket Management System from Cloud ALM

## Create Destination to your Incident Management Ticket System

### Logon to BTP Cockpit subaccount where your SAP Cloud ALM is deployed

Navigate to Connectivity->Destinations: (direct link) <https://emea.cockpit.btp.cloud.sap/cockpit#/globalaccount/e2a835b0-3011-4c79-818a-d7767c4627cd/subaccount/731a2c70-457e-466c-a810-d32cf775c958/destinations>

### Create a new Destination as follows

Destination Configuration: Blank Template

Name: CALM\_INCIDENT\_AD263-XXX ***(replace XXX with your group number)***

Type: HTTP

Description: Incident for AD263-XXX ***(replace XXX with your group number)***

Proxy Type: Internet

Authentication: NoAuthentication

Use default JDK truststore: yes

![](./images/001.png)

To find the correct URL for the Incident Application deployed in your subaccount, please logon to your BTP Subaccount AD263-XXX and navigate to Cloud Foundry -> Spaces -> AD263-XXX -> Applications -> TechEd2023Incident

![](./images/002.png)

The URL mentioned in Application Routes is the target URL needed to create the destination

Save your Destination

### Test Conectivity

Click ![](./images/003.png)

![](./images/004.png)

The result code “404: Not Found” is the expected result and means that the connection is technical reachable, but there is no default page on the main URL. This is for the TechEd Incident Demo application ok.

## Create Webhook

Logon to the AD263 Cloud ALM Tenant: <https://ad263-ptnlz9xc.eu10.alm.cloud.sap/launchpad#Shell-home>

Navigate to “External API Management”

![](./images/005.png)

Create a new Webhook ![](./images/006.png) AD263-XXX\_Webhook ***(replace XXX with your group number)***

Name: AD263-XXX\_Webhook ***(replace XXX with your group number)***

Path: /api/v1/tech-ed

External Resource Type: Incident

Destination Source: BTP Destination

Destination Id: CALM\_INCIDENT\_AD263-XXX (replace XXX with your group number, the destination you have created in the previous step)

Destination Type: Other

![](./images/007.png)

Save

## Create Subscription

Create a new Subscription ![](008.png) AD263-XXX\_Ticket ***(replace XXX with your group number)****

Name: AD263-XXX\_Ticket ***(replace XXX with your group number)****

Description: Ticket Application for AD263-XXX ***(replace XXX with your group number)***

Type: Built-in

Resource Type: CALM Event Situation

Webhook: AD263-XXX\_Webhook ***(replace XXX with your group number)***

Mapping: AD263 Teched Demo Ticket System (all groups can use the same mapping)

![](./images/009.png)

## Create ticket manually from Alert Inbox

Navigate to Integration and Exception Monitoring in Cloud ALM (<https://ad263-ptnlz9xc.eu10.alm.cloud.sap/shell/run?sap-ui-app-id=com.sap.crun.imapp.ui#/Home>)

![](./images/010.png)

Select Open Alerts ![](./images/011.png)

![](./images/012.png)

Select one of the open Alerts for your BTP CF Subaccount AD263-XXX ***(replace XXX with your group number)***

![](./images/013.png)

Create Ticket

![](./images/014.png)

Select your Subscription AD263-XXX\_Ticket ***(replace XXX with your group number)***. The other input fields shall be kept as is. Send the ticket with OK.

If a success message Incident INC<XXXXX> created appears, then your ticket integration was successfully configured.

You can also see the incident in the ticket section of the alert details.

## Review Ticket in Ticket Demo Application

Your newly created ticket shall appear in the list of tickets in the TechEd2023Incident App in your BTP CF Space AD263-XXX ***(replace XXX with your group number)***.

![](./images/015.png)

## Summary

- The connectivity between Cloud ALM the your customer ticket system (the Demo Application for this Hands-on Session will act as mock application for this purpose) has been established
- Tickets can be manually created from Cloud ALM in the Ticket system

