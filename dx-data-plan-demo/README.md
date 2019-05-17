# Script

### help script
sfdx force -h

### Create a new sfdx project
sfdx force:project:create --projectname data-plan-demo --template empty --defaultpackagedir force-app

### Create scratch org
sfdx force:org:create --definitionfile config/project-scratch-def.json --setalias=dug01 --durationdays 30 adminEmail=[use your own email]

### Open the org
sfdx force:org:open -u dug01

### Confirm the source is empty
sfdx force:source:status -u dug01

### Create some listviews on contact and account and make sure sharing is public
Or anything else basically

### Notice the system found a change because its tracking the orgs metadata.
sfdx force:source:status -u dug01

### Pull the changes and check what files were created.
sfdx force:source:pull -u dug01

### Change the order of the displayed fields by messing with the xml locally and then pushing your changes
sfdx force:source:push -u dug01

### Notice how we can list all our orgs and notice we have a data-org-dug (create your own data org and fill with some accounts and contacts)
sfdx force:org:list

### Open the data-org-dug and show the accounts and contacts
sfdx force:org:open -u data-org-dug

### Query the data
sfdx force:data:soql:query --query "select id, name, (select id, FirstName,  LastName, Email FROM Contacts) FROM Account limit 40" -u data-org-dug

### Eport the data
sfdx force:data:tree:export --query "select id, name, (select id, FirstName,  LastName, Email FROM Contacts) FROM Account limit 40" -u data-org-dug --outputdir  exports --plan

### Create a new scratch org
sfdx force:org:create --definitionfile config/project-scratch-def.json --setalias=dug02 --durationdays 5 adminEmail=[use your own email]

### Open the org and notice it has no data
sfdx force:org:open -u dug02

### Push the listviews
sfdx force:source:push -u dug02

### Import the data
sfdx force:data:tree:import --plan exports/Account-Contact-plan.json -u dug02

### Show the data
also show that there are no cases.

### clear the org
sfdx force:apex:execute -f help/clear-org.apex -u dug02

### Manually change the plan to include cases and import those cases
sfdx force:data:tree:import --plan exports/Account-Contact-plan.json -u dug02
