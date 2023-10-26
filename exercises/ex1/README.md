# Exercise 1 - Setting up events and alerts via SAP Alert Notification service 

## Exploring the ANS Sample App 

> [!NOTE]
> Before proceeding with configurations within SAP Alert Notification service it is important to understand how the ANS Sample App works as we will use it in this excercise to push events to Alert Notification service.

**Context**
![](./images/ans-007-producer-api.png)

The diagram above is explained underneath: 
- there is a cloud app already deployed and running on BTP CF in your subaccount
- this app is already instrumented with the Alert Notification service java library so that it is possible to be defined different custom events which can be triggered by the app automatically or manually. In our use cases the events can be triggered manually.
- once the events are triggered by the app, these will be ingested into the Alert Notification service Producer API so that further filtering and actions from the service can be performed. **NOTE**: creating filtering (conditions) and applying specific actions associated to the conditions are described in details

Following excercise 0 - you should be already loged into your SAP BTP sub-account. If not please make sure you follow these steps: 

1. From the BTP Global Account `SAP-TechEd-2023` --> select the subaccount associated to your user.
![](./images/ans-002.png)

2. Once that's done - you should access your SAP BTP Subaccount
![](./images/ans-003.png)

3. To access the ANS sample app:
Click on `Cloud Fioundry` -> `Spaces` and then select the space that is already provisoned for your SAP BTP subaccount. 
![](./images/ans-006.png)

4. There is an app named `ans-sample-app-{your_user_id}` where your `{user_id}` is the one associated to your subaccount, e.g. `ad263-002` , `ad263-003`, `ad263-n+1` , etc. Click on the name of your app.
![](./images/ans-009.png)

5. You will be redirected to the Application Overview screen. Click on the `Application Routes` URL. 
![](./images/ans-010.png)

6. You will land to the ANS Sample App home screen where you will also see 3 buttons as per the screenshot below. Clicking on each of the buttons will push a unique custom event to the Alert Notification service Producer API. 
![](./images/ans-012.png)

Please see below exact attributes for each of the 3 events pushed from the ANS Sample App to the Alert Notification service Producer API. 

**Push Info Notification**
It pushes a custom event `.withType("demo-application")` and `.withSeverity(EventSeverity.INFO)`
Event code is shared as well here: 
```
public static CustomerResourceEvent buildWarningEvent() {
        return new CustomerResourceEventBuilder()
                .withCategory(EventCategory.NOTIFICATION)
                .withSeverity(EventSeverity.WARNING)
                .withBody("This is the body of the event pushed by the ANS Sample Application")
                .withSubject("ANS Tech Ed Demo event")
                .withType("TechEdDemoEvent")
                .withAffectedResource(
                        new AffectedCustomerResourceBuilder()
                                .withType("demo-application")
                                .withName("ans-sample-app-ad263-000")
                                .build()
                ).build();
```

**Push Warning Notification**
It pushes a custom event `.withType("demo-application")` and `.withSeverity(EventSeverity.WARNING)`

Event code is shared as well here: 
```
public static CustomerResourceEvent buildWarningEvent() {
        return new CustomerResourceEventBuilder()
                .withCategory(EventCategory.NOTIFICATION)
                .withSeverity(EventSeverity.WARNING)
                .withBody("This is the body of the event pushed by the ANS Sample Application")
                .withSubject("ANS Tech Ed Demo event")
                .withType("TechEdDemoEvent")
                .withAffectedResource(
                        new AffectedCustomerResourceBuilder()
                                .withType("demo-application")
                                .withName("ans-sample-app-ad263-3")
                                .build()
                ).build();
```


**Push Error Notification**
It pushes a custom event `.withType("demo-application")` and `.withSeverity(EventSeverity.ERROR)`

Event code is shared as well here: 
```
public static CustomerResourceEvent buildErrorEvent() {
        return new CustomerResourceEventBuilder()
                .withCategory(EventCategory.NOTIFICATION)
                .withSeverity(EventSeverity.ERROR)
                .withBody("This is the body of the event pushed by the ANS Sample Application")
                .withSubject("ANS Tech Ed Demo event")
                .withType("TechEdDemoEvent")
                .withAffectedResource(
                        new AffectedCustomerResourceBuilder()
                                .withType("demo-application")
                                .withName("ans-sample-app-ad263-3")
                                .build()
                ).build();
```

> [!NOTE]
> As you see: main difference between these 3 events is the severity. That will be useful to determine specific actions based on events' severity once these get pushed into the Alert Notification service. 

8. Now, let's play with the app and see what the result it will be. Click on any of the available buttons (e.g. `Push Info Notification`, etc.) and you should see this this confirmation message. 
![](./images/ans-013.png)

The result is that the custom event was pushed successfully to ANS API. In order to get alerts and / or automated actions triggered by the Alert Notification service based on the event ingested we need to configure the Alert Notification service itself. That is explained in details in the next section. 


## Access SAP Alert Notification service 

To access SAP Alert Notification service, since you are already logged into the SAP BTP Cockpit, please follow these steps: 

1. From the left sidebar menu click on `Instances and Subscriptions` memu item to access the BTP services and applications already provisioned for your subacount
![](./images/ans-004.png)

2. Scroll to the section `Instances` where you will find all BTP services provisioned in your subaccount. Click on the `ans` instance to access the SAP Alert Notification service.
![](./images/ans-005.png)

3. You will land to your SAP Alert Notification service . 
![](./images/ans-006.png)

In the next section you will understand how to confiure the service. 


## Configuring SAP Alert Notification service 
Now it is time to configure the  Alert Notification service itself and see it in actions for 3 different use cases. To do so - please follow the next sections. 

> [!NOTE]
> In order to be able filter and act on the events ingested into the Alert Notification service it is needed to configure: `conditions` (needed to filter out the events which are important for you) , `actions` (specify  actions to be triggered by the Alert Notification service, e.g. send email, send IM notification, trigger HTTPS webhooks, trigger automation flow, etc), and set `subcription` (which is the entiry that binds one or more conditions to one or more actions that have been already set). IMPORTANT: without an active `subcription` no action will be triigered by Alert Notification service.

---

### Use Case: Alert Notification service - Create a Ticket in a Ticket System based on a custom event

#### Solution Diagram
![](./images/ans-014.png)

#### Explained
In this use case there will be a custom event (`Info Notification` - already explained in previous sections) pushed by the ANS Sample App to the Alert Notification service. Based on an active `Subcription` (which cosists of `Conditions` and `Actions`) in Alert Notification service, the event will be filtered out and an action (create a ticket in a ticketing system)  will be automatically triggered  by the Alert Notification service. Please follow the section below. 

#### Alert Notification service - Configuration

**Access the Alert Notification service** overview page (see _Access SAP Alert Notification service _)

1. **Create Conditions**

1.1. Create a Condition with "Severity" set to `WARNING`

1.1.1.From the left-sidebar menu: select `Conditions` menu item, followed by the button `Create`
![](./images/ans-015.png)

1.1.2. Fill in the  details to create the needed dondition. In this use case it we need to create a condition so that we can filter out the events based on a particular `severity`. 
* Name:  `severityWarning`
* Description: `EventSeverity set to WARNING`
* Label: `severity`
* Condition: use the drop downs so that you can setup the condition correctly = `severity` ->  `Is Equal To` -> `WARNING`
* Click on `Create` button to save the configuration
![](./images/ans-037.png)
* You will see a confirmation screen. Then click on the `Close` button. 

1.2. Create a Condition with "Event Type" set to `TechEdDemoEvent`
> [!NOTE]
> Since there might be events from different sources with severity set to `INFO` it is a good practice to filter out the events also on other parameters. In this use case, we will use also the "Event type" which for all events pushed by the ANS Sample app is set to `TechEdDemoEvent`. Therefore on a later stage it migth be used a combination of two conditions `severity` **AND** `eventType` to make a unique filtering for the events. However, considering that the landscape for this demo is not that complex and there won't be that many events pushed to the Alert Notification service, for setting up the subscriptions will be used this combination of subscriptions:   `severity` **OR** `eventType`

1.2.1.From the left-sidebar menu: select `Conditions` menu item, followed by the button `Create`
![](./images/ans-015.png)

1.2.2. Fill in the  details to create the needed condition. In this use case it we need to create a condition so that we can filter out the events based on a particular `eventType`. 
* Name:  `eventType_TechEdApp`
* Description: `eventType set to TechEdDemoEvent`
* Label: `eventType`
* Condition: use the drop downs so that you can setup the condition correctly = `eventType` ->  `Is Equal To` -> `TechEdDemoEvent`
* Click on `Create` button to save the configuration
![](./images/ans-018.png)
* You will see a confirmation screen. Then click on the `Close` button. 
![](./images/ans-019.png)



2. **Create Actions**
As a next step you need to create an action - as per the specific use case - it is needed to create an action for creating a ticket in a Ticketing system. To do so follow the steps defined below. 

2.1. From the left-sidebar menu: select `Actions` menu item, followed by the button `Create`
![](./images/ans-021.png)

2.2. There is "Create Action" wizzard displayed , please follow these steps:
2.2.1. Select `Webhook`  and click `Next`
![](./images/ans-038.png)

2.2.2. Fill in the  details to create the needed action: 
* Name: `creteTicket`
* Description: `create a ticket in a Ticketing system`
* Labels: `ticketingSystem`
* URL address: `{teched-incident-demo-ui-url}{createTicket_api_endpoint}` that has been already deployed in your BTP CF Space - where you can replace `{createTicket_api_endpoint}` with `/api/v1/tech-ed/createTicket`
* Payload Tempalte:
```
{
    "subject": "Ticket from ANS Sample App",
    "priority": 2
}
```
* Click on `Create` button
![](./images/ans-039.png)

2.2.3. You will see an confirmation screen that the action has been created. 

Now the action `creteTicket` is created and you should see this action within the "Actions" section. 
![](./images/ans-040.png)


3. **Create Subscription**
As a next step you need to create the subscription.

3.1. From the left-sidebar menu: select `Subscriptions` menu item, followed by the button `Create`
![](./images/ans-029.png)

3.2. There is "Create Subscription" wizzard displayed , please follow these steps:
3.2.1. Create Subsription: fill in the form with the needed details 
* Name: `sampleApp_WarningEvent`
* Description: `a subscription to be fired for a custom event with severity WARNING`
* Labels: `sampleApp`
* State: `On` (this is the default state)
* Click on the `Create` button
![](./images/ans-041.png)

3.2.2. Select Conditions: select the conditions you have created and click on the `Assign` button
![](./images/ans-042.png)

3.2.3. Select Actions: select the action you have created and click on the `Assign` button
![](./images/ans-043.png)

3.2.4. Confirmation screen: you will see a confirmation screen. Click on `Close` button. 
![](./images/ans-044.png)

Now you will see the active subscription you just have configured.
![](./images/ans-045.png)


#### Simulation and Outputs 

Now it is time to simulate the use case and inspect the outputs. To do so , please follow these steps: 

1. Access the ANS Sample App (as already described).
2. Click on the `Push Warning Notification`
3. The custom event will be pushed into the Alert Notification sevice Producer API, then Alert Notification service will filter out the event based on the conditions set for the active subcription and it will also trigger the action associated to this very subscription.
4. Access the UI of the ticketing system that has been already deployed in your BTP CF Space  - `{teched-incident-demo-ui-url}
5. You should see an incident created automatically by the Alert Notification service (see an example as per the screenshot below). 
![](./images/ans-046.png)

---

## Summary

You've now managed to see Alert Notification service in action. With its flexible events' filtering combined with a rich catalog of actions that can be triggered automatically, the Alert Notification service is a solid product, valuable for each (Dev)Ops team that can be used in a wide variaty in use cases.

Continue to - [Exercise 2 - Exercise 2 Description](../ex2/README.md)

