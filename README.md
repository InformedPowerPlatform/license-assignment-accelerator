# Microsoft Dataverse Power Platform License Assignment Accelerator

Microsoft Dataverse Power Platform License Assignment Accelerator allows you to track Contracted License Assignments to People and Cost Centers/Agencies/Departments. These licenses can then be assigned to individual employees and track which cost center the charges apply to.

Example: You contract for 1,000 Microsoft 365 E5/G5 licenses for 3 years at $xx per license, per month. You enter data into the **Contract** table first (e.g., Microsoft EA), including the date the contract will expire. Next, enter data for a related **Contract Line** (e.g., Microsoft 365 E5/G5), including details like the total number of licenses purchased, cost per license, and cost to charge out to the cost center. Once this is set up, you can then assign any of those licenses to a specific person and cost center. After that you can track who has what licenses assigned and set up an optional monthly chargeback invoice to that cost center.

## Contents

* [Requirements](#requirements)
* [Personas](#personas)
* [Installation and Setup](#installation-and-setup)
* [Configure Cloud Flows](#configure-cloud-flows)
* [Configure Data](#configure-data)
  * [Cost Centers](#cost-centers)
  * [Assignees](#assignees)
* [Configure Contracts and Line Items](#contracts)
  * [Contract Lines](#contract-lines)
* [Create Assignments](#assignments)
* [Process Requests](#requests)
* [ERD](#simplified-entity-relationship-diagram)
* [Release Notes](#release-notes)

## Features Include

* An Unmanaged solution, so you can easily modify it for your own needs.
* Model Driven App for overall data entry and license assignments
* Customizable Dataverse tables
* Power Automate Flows for monitoring and notifying when contract expiration dates are approaching, and usage parameters are exceeded
* The app is NOT specific to Microsoft licenses, it can be used to monitor contracts and license counts for any number of vendors you work with (e.g., Amazon, Google, SAP, Adobe) that have monthly or annual per user license fees.
* Ability to add optional cost markup to each monthly license for internal support or other associated overhead costs.
* Cascading updates when Contracts are terminated, costs change, or employees are off boarded.
* Tablet and mobile friendly based Canvas Apps that can be deployed to your employees for them to see what has been assigned to them, and they can request available licenses from the pool.

## Requirements

* Power Platform Environment with Dataverse database deployed
* Each Persona will need a Power Platform per user (or per app) license assigned to them in the Microsoft 365 admin center

## Personas

* ***Contract Owner***: Responsible for top level contract, sets Line Items, Costs, etc. Will receive notifications as contracts reach expiry dates, or contract lines approach or exceed thresholds.
* ***IT/Operations Staff***: Assigns/Deactivates licenses. This can also be done with automation from external systems through the Power Platform API.
* ***Cost Center Owner***: Monitors Costs and Assignments.
* ***End User (Optional)***: Can view existing licenses assigned, and request new licenses from the available pool.

## Installation and Setup

* ***Best Practice:*** While not required, it is a best practice to install these solutions into their own Power Platform environment. This will allow you to restrict access through Azure AD security roles and teams.
  * **NOTE:** It is NOT recommended that you install these solutions into your (Default) environment.

* Download the latest solution files from the [Releases](https://github.com/InformedPowerPlatform/license-assignment-accelerator/releases) page. For Initial install, there are 2 solution files
  * Install 1st: LicenseAssignment_x_x_x_x.zip
  * Install 2nd: LicenseAssignmentPAFlowsOnly_x_x_x_x.zip
  * **Tip:** To ensure smooth operation, apply a ***Publish All Customizations*** to the solution after import is complete.

## Configure Cloud Flows

* Open the ***LicenseAssignmentPAFlowsOnly*** Solution in the Power Apps Maker Site
  * Commercial M365 Tenant: https://make.powerapps.com/
  * US Govt GCC Tenant: https://make.gov.powerapps.us/ 
* Navigate to the ***Cloud Flows*** section and manually run the flow titled ***MSFTPP.PA.Settings.InitialLoad***
  * This will set up 4 levels of Reminders for contract expiration notifications. These can be later modified to your requirements by selecting the ***License Admin Settings*** table in the Model-driven app.
* Open the Cloud Flow titled ***MSFTPP.PA.ChildFlow.GetTeamMemberEmails*** for editing. This child flow gathers the members of a team into a list usable by the Outlook action in the notification flows. You will need to open this flow for editing and save it so that it will be in a ***Published*** state.
* There is an Environment Variable called ***NotificationFromAddress*** that will need to be updated
  * Set this value to an email box that can send internal notifications for your organiztion. 
  * During the initial installation of the solution, a connection reference to the Outlook connector was created. The Azure AD user selected when this connection reference was created will need **Send As** access from this mailbox.
    * Example 1: If you used service_account@youremail.com as the Azure AD login for the Outlook connection reference, and then set notifications@youremail.com as the FROM address in the Environment Variable, the service_account@youremail.com login will need **Send As** rights to the notifications@youremail.com mailbox in Microsoft 365 Exchange.
    * Example 2: If you used service_account@youremail.com as the login for the Outlook connection reference, you can also set the environment variable to service_account@youremail.com. 
* There are 2 Cloud flows that are turned OFF by default that will need to be turned ON if you wish to use the notification features related to contract expiry and license counts. You will need to edit each one to turn the Cloud Flow ON. Double check the ***FROM*** email address is set to the environment variable above.
  * ***MSFTPP.PA.ContractLines.Scheduled.CheckLicenseCount***
  * ***MSFTPP.PA.Contract.Schedule.CheckForReminder***

## Configure Data

* From the ***Apps*** navigation in the Power Apps maker site, Open the **License Contracts** Model-Driven app

### Cost Centers

* Navigate to the Admin group and select the **License Cost Centers** table
  * Enter your Cost Centers that you wish to track as part of the individual assignments. These could be your internal Agencies, Departments, Teams, etc.

### Assignees

* Navigate to the Admin group and Select **License Assignees**
  * Enter or Import all of your possible assignees to this table. If available, set the assignee's default cost center to simplify the assignments.
  * ***Note:*** You may want to use teams, departments, divisions, etc. as your assignees instead of individual people. If so, you can re-label the first/last name fields to use those names instead. Use the Power Apps Maker tool and edit the form in the **License Assignees** table to change the labels. When creating an ***assignment*** row, you can set the quanity of licenses being assigned to a number larger than the default of 1.
  * ***Optional:*** If you plan to allow users to track their own license assignments, and to request new assignments, those assignee's will also need to be ***users*** in the Dataverse environment. Once they are added into the *systemuser* table, you would select their Related User on the License Assignee row.

### Contracts

* Navigate to **License Contracts** table and enter any high-level contracts you want to track.
  * Expiry Date is used to calculate the notifications sent out as contracts are about to expire.
  * The Owner can be an individual user, or a Team. When the notifications are sent, it will use the owner field to determine who to send the notification to.

### Contract Lines

* Next we will populate the **License Contract Lines** table. These are the individual descriptive types of licenses that will be assigned to people.
  * e.g. For a Microsoft Enterprise Agreement, you might have licenses for Microsoft 365, Dynamics 365 Customer Service, Power Platform Per User, etc. These would be **License Contract Lines**.
  * To simplify the entry of contract lines, while you are editing the individual **License Contract** click the ***+ New License Contract Line*** button in the Contract Lines subgrid.
    * Enter a Name, Owner, Total License Count, Charge Rate (cost to be charged to the cost center), and the Contract Rate.
    * Enter a **Notify Threshold Pct** as a decimal value. This is the percentage at which notifications will be sent when the total count of assigned licenses exceeds the threshold.
      * E.g. If you have 100 licenses and you want to be notified when your assigned count exceeds 85, enter 0.85 as a Notify Threshold Pct.

### Assignments

To set up a **License assignment**, the easiest place to do this is on the **License Contract Line** form.

* Navigate to and edit the **License Contract Line** row that you wish to add a new assignment for.
* Click the **+ New License Assignment** button in the Assignments sub-grid on this form. This should open a quick create form.
* Select the Assignee name from the list of available assignees. **Cost Center** should be selected for that assignee if they have a default set.
  * Note: You can always override the Cost Center assignment if needed.
* Optionally select the Date of this assignment for reference.
* If you are assigning a larger quantity of licenses (e.g. to a whole department/team instead of a person), enter the quantity assigned.
* Click **Save and Close** to save the assignment, or **Save and New** to create another assignment for the same contract line.

## Requests

If you are utilizing the Power Platform Canvas App(s) where users can request a license, you will have rows added to the **License Request** table.

* To review the requests, select **License Requests** from the left navigation in the Model-driven app
  * To approve a request, edit the row. In the Status Reason field, change the value to **Approved** then Save the row. This will copy the data from the request over to the License Assignment table.

## Simplified Entity Relationship Diagram

![ERD](ERDArchitecture.png?raw=true "ERD")


# Release Notes
## v3.0.0.3 
* Added a Quantity field to the Assignment table, updated the rollups to use qty * cost.

## v3.0.0.1 
* Separated the Main Solution from the Power Automate Cloud Flows and Connection References. Added a new Environment Variable to set the FROM address on the Cloud Flow notifications.


