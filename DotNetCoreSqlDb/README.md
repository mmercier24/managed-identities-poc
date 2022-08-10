# Managed Identity POC

Small proof of concept to showcase a .NET 5 application running on App Service connecting to an Azure SQL database using Managed Identities.

## Setup

Create an Azure SQL database and an Azure App Service.

If you would like to use User-Assigned Managed identities then search for "Managed Identities" in the Azure Portal and create a new User-Assigned Identity Managed Identity.
Then go in the "Identity" section of your Azure Web App and click on the User-Assigned tab and select your newly created User-Assigned Identity.

If you would rather use System-Assigned managed identities then open up the "Identity" section in your Azure Web App and turn on the option for System-Assigned Identities.

You will also have to go to the Access Control (IAM) settings of your Azure SQL server to add a role assignment with proper permissions for the newly created Managed Identity.

Open appsettings.json and change the placeholders with the information for your newly created Azure SQL database. If using User-Assigned identity then you will need to specify the Client ID of the identity in the User ID part of the connection string. If you are using System Assigned Managed Identity then just remove the User Id section althogether as App Service will pickup the proper identity information automatically.

Run the Entity Framework Core migration to setup your database.

Publish the webapp to App Service and it should work and use Managed Identities.

If you want to push things a bit further and integrate Private Endpoints then create a Virtual Network with two subnets. Open up the networking settings of your Azure SQL database server and turn off public access and turn on private access and specify your newly created vnet and subnet.

Then, open up the networking settings of your Azure App Service and turn on vnet integration and specify the second subnet you created. No further code changes are needed and the database should now only be accessible when the code is running on Azure App Service.
