# Microft Dataverse Power Platform License Assignment Accelerator
Microsoft Power Platform Dataverse Accelerator to allow you to track Contracted License Assignments to People and Cost Centers/Agencies/Departments. These licenses can then be assigned out to individual employees and track which cost center the charges apply to.

Example: You contract for 1,000 Microsoft Power Platform per user liceses for 3 years at $xx per license, per month. You enter the contract (Microsoft EA), Contract Line (PP Per user) details into the respective tables. Then Assign 1 of those licenses to a specific person and cost center (department). After that you can track who has what licenses assigned and also setup an optional monthly chargeback invoice to that cost center.


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


### Simplified Entity Relationship Diagram

![ERD](ERDArchitecture.png?raw=true "ERD")

### Installation and Setup
* Download the latest solution file from the repository
* Install to your Power Platform environment
* Open the Solution in the PP Maker Site
* Navigate to the Cloud Flows section and manually run the flow titled ***MSFTPP.PA.Settings.InitialLoad***
    - This will setup 4 levels of Reminders for contract expiration notifications. These can be later modified to your liking by selecting the License Admin Settings area in the Model-driven app.
* From the Apps navigation in the Power Apps maker tool, Open the License Contracts Model-Driven app
* Navigate to the Admin Grouping and select the License Cost Centers table
    - Enter your Cost Centers that you wish to track as part of the individual assignments. These could be your internal Agencies, Departments, Teams, etc.
* Navigate to the Admin Grouping and Select License Assignees
    - Import any and all of your possible assignees to this table. If possible, set the persons default cost center to simplify the assignments.
    - If you plan to allow users to track their own license assignments, as well as to request new assignments, those employees will also need to be ***users*** in the Dataverse environment. Once they are added into the *systemuser* table, you would select their Related User on the License Assignee row.





    
    
