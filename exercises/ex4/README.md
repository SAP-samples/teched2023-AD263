# <a name="_toc146285056"></a>**Exercise 4 - End-to-End Execution**
### ***Configure automatic ticket creation at event in Integration & Exception Monitoring***
Navigate to Integration & Exception from Cloud ALM Launchpad

![](001.png)

![](002.png)

1. Click on the configuration button
1. Select your BTP CF service AD263-XXX *(replace XXX with your group number)*

Navigate to the Events configuration of your service

![](003.png)

Activate the Create Ticket Event Action and Assign your Subscription AD263-XXX\_Ticket *(replace XXX with your group number)*

![](004.png)

Save the changes and close
### ***Trigger Exception***
![](005.png)

In your BTP space a sample java application has been deployed, which can create exceptions on user requests. This is a mock application to simulate exception creations from customer developed PaaS applications.

- Please click the button Create Exception to raise some example exceptions from this Application
### ***Review Exception in Cloud ALM***
Navigate to the Home of Integration & Exception Monitoring

![](006.png)

1. Click on the Refresh Indicator
1. Click Refresh to manually refresh the current page (by default the current view is refreshed every 5 minutes automatically). The data transfer from your application to Cloud ALM can take up to two minutes
1. Click on the Exceptions to show the exception details

![](007.png)
### ***Review Ticket in Ticket Demo Application*** 
Your newly created ticket shall appear in the list of tickets in the TechEd2023Incident App in your BTP CF Space AD263-XXX *(replace XXX with your group number)*. It could take up to 5 minutes until the ticket is created.

![](008.png)

### ***Summary***
- In case of new exceptions created by the customer application automatically new incident tickets are created in the ticket system

