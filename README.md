# Microsoft Dataverse Power Platform License Assignment Accelerator
Microsoft Power Platform Dataverse Accelerator to allow you to track Contracted License Assignments to People and Cost Centers/Agencies/Departments. These licenses can then be assigned out to individual employees and track which cost center the charges apply to.

Example: You contract for 1,000 Microsoft Power Platform per user liceses for 3 years at $xx per license, per month. You enter the contract (Microsoft EA), Contract Line (PP Per user) details into the respective tables. Then Assign any of those licenses to a specific person and cost center (department). After that you can track who has what licenses assigned and also setup an optional monthly chargeback invoice to that cost center.


### Includes:

* An Unmanaged solution, so you can easily modify it for your own needs.
* Model Driven App for overall data entry and license assignments
* Generic custom Dataverse tables
* Power Automate Flows for monitoring and notifying when contract expiration dates are approaching and usage parameters are exceeded
* Not specific to Microsoft licenses, it can be used to monitor any number of contract vendors (Amazon, Google, SAP, Adobe) that have monthly or annual per user fees.
* Ability to add optional markup to each monthly license for support or other associated costs.
* Cascading updates when Contracts are terminated, costs change, or employees are off-boarded.
* Tablet and mobile friendly based Canvas Apps that can be deployed to your employees for them to see what has been assigned to them, and they can request available licenses logged in contract lines.

### Requirements:
* Power Platform Environment with Dataverse database deployed
* Each Persona will need a Power Platform per user (or per app) license assigned to them in the Microsoft 365 admin center

### Installation and Setup
* Download the latest solution file from the repository [Releases](https://github.com/InformedPowerPlatform/license-assignment-accelerator/releases)
* Install to your Power Platform environment
* Open the Solution in the PP Maker Site
* Navigate to the Cloud Flows section and manually run the flow titled ***MSFTPP.PA.Settings.InitialLoad***
    - This will setup 4 levels of Reminders for contract expiration notifications. These can be later modified to your liking by selecting the License Admin Settings area in the Model-driven app
    - There are 2 Cloud flows that are turned OFF by default that will need to be turned ON if you wish to use the notification features related to contract expiry and license counts
* From the Apps navigation in the Power Apps maker tool, Open the **License Contracts** Model-Driven app
* Navigate to the Admin Grouping and select the **License Cost Centers** table
    - Enter your Cost Centers that you wish to track as part of the individual assignments. These could be your internal Agencies, Departments, Teams, etc.
* Navigate to the Admin Grouping and Select **License Assignees**
    - Import any and all of your possible assignees to this table. If possible, set the persons default cost center to simplify the assignments.
    - If you plan to allow users to track their own license assignments, as well as to request new assignments, those employees will also need to be ***users*** in the Dataverse environment. Once they are added into the *systemuser* table, you would select their Related User on the License Assignee row.

### Contracts
* Navigate to **License Contracts** and enter any high level contracts you want to track
    - Expiry Date is used to calculate the notifications sent out as contracts are about to expire.
    - The Owner can be a individual user, or a Team. When the notifications are sent, it will use the owner field to determine who to send the notification to.
* Next Enter the **Contract Lines**. These are the individual descriptive types of licenses that will be assigned to people.
    - e.g. For a Microsoft Enterprise Agreement, you might have licenses for Microsoft 365, Dynamics 365 Customer Service, Power Platform Per User, etc. These would be **Contract Lines**.
    - To simplify the entry of contracts lines, click the ***+ New License Contract Line*** button in the Contract Lines subgrid for the specific Contract you are adding lines to.
        - Enter a Name, Owner, Total License Count, Charge Rate (cost to be charged to the cost center), and the Contract Rate.
        - Enter a **Notify Threshold Pct** as a decimal value. This is the percentage at which notifications will be sent when the remaining count of licenses drops below the threshold. 
            - E.g. If you have 100 licenses and you want to be notified when your assigned count exceeds 85, enter 0.85 as a Notify Threshold Pct.


### Assignments
To setup a **License assignment**, the easiest place to do this is on the **License Contract Line** form. 
* Open the **License Contract Line** form for the row you wish to add a new assignment for.
* Click the **+ New License Assignment** button in the Assignments sub-grid on this form.
* Select the Assignee name from the list of available assignees. **Cost Center** should be selected for that assignee if they have a default set. You can always override the Cost Center assignment if needed.
* Optionally select the Date of this assignment for reference.
* Click **Save and Close** to save the assignment.

### Requests
If you are utilizing the Power Platform Canvas App(s) where users can request a license, you will have rows added to the **License Request** table.
* To review the requests, select **License Requests** from the left navigation in the Model-driven app
    - To approve a request, edit the row. In the Status Reason field, change the value to **Approved** then Save the row. This will copy the data from the request over to the License Assignment table.







### Simplified Entity Relationship Diagram

![ERD](ERDArchitecture.png?raw=true "ERD")

    
    
