# Microsoft Dataverse Power Platform License Assignment Accelerator

Microsoft Dataverse Power Platform License Assignment Accelerator allows you to track Contracted License Assignments to People and Cost Centers/Agencies/Departments. These licenses can then be assigned to individual employees and track which cost center the charges apply to.

Example: You contract for 1,000 Microsoft 365 E5/G5 licenses for 3 years at $xx per license, per month. You enter data into the **Contract** table first (e.g., Microsoft EA), including the date the contract will expire. Next, enter data for a related **Contract Line** (e.g., Microsoft 365 E5/G5), including details like the total number of licenses purchased, cost per license, and cost to charge out to the cost center. Once this is set up, you can then assign any of those licenses to a specific person and cost center. After that you can track who has what licenses assigned and set up an optional monthly chargeback invoice to that cost center.

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
  * ***Contract Owner***: Responsible for top level contract, sets Line Items, Costs, etc.
  * ***IT/Operations Staff***: Assigns/Deactivates licenses
  * ***Cost Center Owner***: Monitors Costs and Assignments
  * ***End User (Optional)***: Can view existing licenses assigned, and request new licenses from the available pool

## Installation and Setup

* Download the latest solution file from the [Releases](https://github.com/InformedPowerPlatform/license-assignment-accelerator/releases) page
* Install to your Power Platform environment
* Open the Solution in the PP Maker Site
* Navigate to the Cloud Flows section and manually run the flow titled ***MSFTPP.PA.Settings.InitialLoad***
  * This will set up 4 levels of Reminders for contract expiration notifications. These can be later modified to your liking by selecting the License Admin Settings area in the Model-driven app.
* There is a child flow that gathers the members of a team into a list usable by the Outlook action. You will need to open this flow for editing and save it so that it will be in a ***Published*** state.
  * MSFTPP.PA.ChildFlow.GetTeamMemberEmails 
* There are 2 Cloud flows that are turned OFF by default that will need to be turned ON if you wish to use the notification features related to contract expiry and license counts. You will need to edit each one and update the Outlook action that sends the email at the end of the flow to update the ***FROM*** address.
  * MSFTPP.PA.ContractLines.Scheduled.CheckLicenseCount 
  * MSFTPP.PA.Contract.Schedule.CheckForReminder 

* From the Apps navigation in the Power Apps maker tool, Open the **License Contracts** Model-Driven app
* Navigate to the Admin Grouping and select the **License Cost Centers** table
  * Enter your Cost Centers that you wish to track as part of the individual assignments. These could be your internal Agencies, Departments, Teams, etc.
* Navigate to the Admin Grouping and Select **License Assignees**
  * Enter or Import all of your possible assignees to this table. If possible, set the person’s default cost center to simplify the assignments.
  * If you plan to allow users to track their own license assignments, as well as to request new assignments, those employees will also need to be ***users*** in the Dataverse environment. Once they are added into the *systemuser* table, you would select their Related User on the License Assignee row.

## Contracts

* Navigate to **License Contracts** and enter any high-level contracts you want to track
  * Expiry Date is used to calculate the notifications sent out as contracts are about to expire.
  * The Owner can be an individual user, or a Team. When the notifications are sent, it will use the owner field to determine who to send the notification to.
* Next Enter the **Contract Lines**. These are the individual descriptive types of licenses that will be assigned to people.
  * e.g. For a Microsoft Enterprise Agreement, you might have licenses for Microsoft 365, Dynamics 365 Customer Service, Power Platform Per User, etc. These would be **Contract Lines**.
  * To simplify the entry of contract lines, click the ***+ New License Contract Line*** button in the Contract Lines subgrid for the specific Contract you are adding lines to.
    * Enter a Name, Owner, Total License Count, Charge Rate (cost to be charged to the cost center), and the Contract Rate.
    * Enter a **Notify Threshold Pct** as a decimal value. This is the percentage at which notifications will be sent when the total count of assigned licenses exceeds the threshold.
      * E.g. If you have 100 licenses and you want to be notified when your assigned count exceeds 85, enter 0.85 as a Notify Threshold Pct.

## Assignments

To set up a **License assignment**, the easiest place to do this is on the **License Contract Line** form.

* Open the **License Contract Line** form for the row you wish to add a new assignment for.
* Click the **+ New License Assignment** button in the Assignments sub-grid on this form. This should open a quick create form.
* Select the Assignee name from the list of available assignees. **Cost Center** should be selected for that assignee if they have a default set. You can always override the Cost Center assignment if needed.
* Optionally select the Date of this assignment for reference.
* Click **Save and Close** to save the assignment.

## Requests

If you are utilizing the Power Platform Canvas App(s) where users can request a license, you will have rows added to the **License Request** table.

* To review the requests, select **License Requests** from the left navigation in the Model-driven app
  * To approve a request, edit the row. In the Status Reason field, change the value to **Approved** then Save the row. This will copy the data from the request over to the License Assignment table.

## Simplified Entity Relationship Diagram

![ERD](ERDArchitecture.png?raw=true "ERD")
