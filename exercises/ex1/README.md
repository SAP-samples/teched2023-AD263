<img src="https://user-images.githubusercontent.com/11653294/64466233-7cd17480-d119-11e9-8965-e036c1e23c9a.png" alt="CF logo" height="100" align="left"/>

## Introduction to Alert Notification service! 
[![Alert Notification Internal Pages](https://img.shields.io/badge/Instant%20Problem%20Notifications-Alert%20Notification-fabc02.svg)](https://github.wdf.sap.corp/pages/sl-hybrid/ans/)
[![Automation Pilot Internal Pages](https://img.shields.io/badge/Automatic%20Reactions-Automation%20Pilot-03cdff.svg)](https://github.wdf.sap.corp/pages/sl-actions/landing-page/) 

---

### **Overview**
Have you ever faced  the challenge to keep the information about your cloud resources in a single place? Have you ever thought _"Do I actually need to
keep watching so many dashboards"_?  

In the next 15 minutes, you will learn how to stay always notified about what is happening with your SAP BTP environment. In this example,
we will set up notifications for availability and lifecycle changes of Cloud Foundry application. Additionally, we will build a custom notification that 
will inform you for an occurred situation within the application itself.

Let's do it...     

---

### **1. Create Your Trial Account**
1. Visit https://www.sap.com and click the Log On icon in the upper-right corner
2. You can either enter your Global SAP credentials or create a brand new SAP account following the steps in Get a Free Trial Account on SAP BTP
3. Once you have entered SAP trial account on SAP BTP, click on **Enter Your Trial Account**

### **2. Instantiate Alert Notification and Initial Setup**
1. Navigate to the subaccount named **trial**
2. Navigate to **Members** tab (located on the left) and add the following user _sap_cp_eu10_ans@sap.com_ with role **Organization Auditor**. It's needed to pull lifecycle events for this subaccount.
3. Now, navigate to the already created space with name **dev**
4. In the **Service Marketplace**, search for __**Alert Notification**__ and click on the tile
5. In the **Instances** page, click on **New Instance**, choose plan **Standard**, proceed without parameters and application assigned, enter some **Instance Name**
6. Navigate to the newly created instance of Alert Notification
7. From the **Service Keys** page, click on **Create Service Key**, enter some name and provide the following **Configuration Parameters**:
```json
{
  "type": "BASIC"
}
```
_Note: Leave the browser open, as you will need it in the next steps_

### **3. Configure your application**
1. Navigate to _Documents_ and clone a clean instance of the current repository using Git Bash and the following command:
    ```bash
    mkdir $(date | md5sum | cut -d ' ' -f1) && cd "$_" && git config --global http.sslVerify false && git clone [repository_clone_url]
    ```
    _Note: Leave the terminal open, as you might need it in the next steps_
2. Open the **cloud-app** in your preferred Java editor – IntelliJ Idea or Eclipse  
_**NOTE: Make sure that you open your clone of the repository**_
3. Navigate to **application.properties** and populate:
    ```properties
       cloud_app.alert.notification.client.id={client_id}
       cloud_app.alert.notification.client.secret={client_secret}
    ```
    **Hint:** Replace _{client_id}_ and _{client_secret}_ with the corresponding values from the service key you have created in section 2) step 7) above
4. Navigate to **manifest.yaml** and replace _{ans-instance-name}_ with the name of your Alert Notification instance you have created in section 1) step 5) above
5. Navigate to **AnsUtils.java** and populate the methods:
```java
   public static IAlertNotificationClient buildAnsClient(String clientId, String clientSeret);
   public static CustomerResourceEvent convertToAnsEvent(CustomerEventDto eventDto);
   private static AffectedCustomerResource buildCustomerResource();
```
**Hint:** Use the the appropriate builders provided from [Alert Notification Client library](https://github.com/SAP/clm-sl-alert-notification-client). Some default values are left in order to give you some hints on how to build some of the objects. 
Furthermore, some instructions are provided in the source code itself.

### **4. Set up Alert Notification configuration**
1. Navigate to **Export or Import** page of your Alert Notification instance.
2. In the **Import** input, paste the JSON provided below & replace the placeholders as follows: 
    * _**{your_personal_email}**_ - replace it with your personal email, of course you can use your SAP email if you have easy access to it.
    * _**{your_event_type}**_ - replace it with the one you have provided in **AnsUtils.convertToAnsEvent** from section 3) step 5)

_Note: It is a predefined configuration that describes what happens when a particular event for you is received in Alert Notification._
```json
{
   "conditions": [
     {
       "name": "cloud_app-custom-event",
       "propertyKey": "eventType",
       "predicate": "EQUALS",
       "propertyValue": "{your_event_type}",
       "labels": [],
       "description": "Will match custom produced events, generated in your app web UI"
     },
     {
       "name": "is-app-crash",
       "propertyKey": "eventType",
       "predicate": "EQUALS",
       "propertyValue": "app.crash",
       "labels": [],
       "description": "Will match app.crash events, received from CF Cloud Controller"
     },
     {
       "name": "is-audit-app-process-crash",
       "propertyKey": "eventType",
       "predicate": "EQUALS",
       "propertyValue": "audit.app.process.crash",
       "labels": [],
       "description": "Will match audit.app.process.crash events, received from CF Cloud Controller"
     },
     {
       "name": "is-audit-app-update",
       "propertyKey": "eventType",
       "predicate": "EQUALS",
       "propertyValue": "audit.app.update",
       "labels": [],
       "description": "Will match audit.app.update events, received from CF Cloud Controller"
     },
     {
       "name": "is-audit-app-create",
       "propertyKey": "eventType",
       "predicate": "EQUALS",
       "propertyValue": "audit.app.create",
       "labels": [],
       "description": "Will match audit.app.create events, received from CF Cloud Controller"
     },
     {
       "name": "is-audit-app-droplet-create",
       "propertyKey": "eventType",
       "predicate": "EQUALS",
       "propertyValue": "audit.app.droplet.create",
       "labels": [],
       "description": "Will match audit.app.droplet.create events, received from CF Cloud Controller"
     },
     {
       "name": "is-audit-app-delete-request",
       "propertyKey": "eventType",
       "predicate": "EQUALS",
       "propertyValue": "audit.app.delete-request",
       "labels": [],
       "description": "Will match audit.app.delete.request events, received from CF Cloud Controller"
     }
   ],
   "actions": [
     {
       "name": "email-action",
       "state": "ENABLED",
       "labels": [],
       "description": "",
       "destination": "{your_personal_email}",
       "type": "EMAIL"
     },
     {
       "name": "store-action",
       "state": "ENABLED",
       "labels": [],
       "description": "",
       "type": "STORE"
     }
   ],
   "subscriptions": [
     {
       "name": "store-app-crash-events",
       "conditions": [
         "is-audit-app-process-crash",
         "is-app-crash"
       ],
       "actions": [
         "store-action"
       ],
       "labels": [],
       "state": "ENABLED",
       "description": "Any event that matches at least one of the conditions \"is-audit-app-process-crash\" or \"is-app-crash\" will be stored in your event storage (see your app web UI)."
     },
     {
       "name": "mail-me-app-lifecycle-events",
       "conditions": [
         "is-audit-app-update",
         "is-audit-app-create",
         "is-audit-app-droplet-create",
         "is-audit-app-delete-request"
       ],
       "actions": [
         "email-action"
       ],
       "labels": [],
       "state": "ENABLED",
       "description": "Any event that matches at least one of the included conditions will be sent to the provided e-mail address."
     },
     {
       "name": "mail-me-custom-event",
       "conditions": [
         "cloud_app-custom-event"
       ],
       "actions": [
         "email-action"
       ],
       "labels": [],
       "state": "ENABLED",
       "description": "Any event that matches the \"cloud_app-custom-event\" condition will be sent to the provided e-mail address."
     }
   ]
}
```
_**Note: Shortly after importing the configuration you will receive an email about confirming the Email action. From the email navigate to the given URL and click on Confirm.**_

### **5. Build and deploy your application**
1. In terminal, navigate to the home directory of your application
2. Build it with the command:
    ```bash
    mvn clean install
    ```
3. Push the application to Cloud Foundry. Here you have two options – either via the Cockpit UI, or using the command line:

* a. Push from  BTP Cockpit
1. In space **dev**, navigate to the **Applications** page   
2. Click on **Deploy Application**
   * 2.a. **File location**: browse for _{demo-app-home-dir}_/target/ans-demo-application-1.0.0.jar  
   * 2.b. Make sure the checkbox for using of Manifest.yaml is checked
   * 2.c. **Manifest location**: browse for _{demo-app-home-dir}_/manifest.yaml  

_**NOTE: Make sure that you use jar and manifest from your clone of the repository**_  
_**Hint: A simple `pwd` command in the opened terminal will give you the absolute path to the current directory**_

b. Push from command line  
1. Navigate to the application's home directory  
2. Login to Cloud Foundry  
    * 2.a. Navigate to your newly created/existing trial subaccount. Copy the _API Endpoint_ URL address, located under the _CloudFoundry_ information. The default url should be: _https://api.cf.eu10.hana.ondemand.com_
    * 2.b. Connect to Cloud Foundry using _cf_ command:
    ```bash
     cf login -a {copied-api-endpoint-url} -u {your-email-address-provided-upon-account-registration}
    ```
    _Note: When prompt, enter your password. Also if prompt, navigate to your **trial** account, space **dev**_
3. Push the application  
    ```bash
    cf push
    ```

### **4. Play around**
It's high time to take a look what is the actual benefit from what we have done so far:

#### Application lifecycle events
Within the configuration we imported in section 4) step 2), we declared we want to receive all associated notifications via e-mail every time
an application within the current space is started or stopped.   

Once the application is deployed & started, verify you have just received a notification that informs you for this event ✈️

#### Custom event
In section 3) above, we have just configured the application to react on some custom situation with producing a custom event that is sent to Alert Notification.  

Let's now induce that situation following the steps: 
1. Within the Cloud Cockpit, navigate to your application **Overview** page, click on the available application route
2. Click on **Produce Custom Event** tile
    * (_**Optional**_) 2.a. If you have configured to use the received object in the source code, fulfill the properties with some demo values
3. Click on **Send**

Verify you have just received an e-mail that contains the exact values you have fulfilled in the form from step 2.a.) above or have configured in the source code ✈️

#### Application availability test
In the demo application, we have defined a custom endpoint to be used by the Cloud Controller for health check. Let's now simulate that our application is not 
_"healthy"_ using the **Availability Check** tile within the application's UI. Turn the availability status off, so that our custom health endpoint will return a bad status code.
In no time the Cloud Controller will find out that our application is unhealthy and very soon you'll have an event in your storage that informs for this situation. Preview
the associated with application crash event by clicking on the **Stored events** tile in the main page of your application's UI.

### Congrats - you've just completed the session!
---
 
### **Want to automate the reactions to your routine problems?**
For the time of the hands-on session, we were only able to set up the notifications from your cloud environment. However, do you think it will be great to have some automatic
reactions upon the routine problems that occur in the platform, e.g. application stopped or crashed for no reason?

For these and more complicated DevOps scenarios that occur everyday to each of us, check out [Automation Pilot](https://help.sap.com/viewer/DRAFT/de3900c419f5492a8802274c17e07049/Cloud/en-US/4536e41c57aa442095ccbac977965f26.html).  
An easy integration to Alert Notification in a few steps is [available](https://help.sap.com/viewer/DRAFT/de3900c419f5492a8802274c17e07049/Cloud/en-US/684d09a964d14d07ac3d6b3a2754765a.html).


2.	Click here.
<br>![](/exercises/ex1/images/01_02_0010.png)


## Summary

You've now ...

Continue to - [Exercise 2 - Exercise 2 Description](../ex2/README.md)

