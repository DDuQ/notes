# Naming conventions

In order to **easily recognize what services we're dealing with**; ==who are their maintainers and for which environment(s) is being deployed==, it is a good practice to follow a standard for naming resources. 

For such resources this will be the norm when naming them:

`[Name of the department inside the company]-[Type of resource shorthand]-[Environment]-[Region]-[Functional description]`

For instance:

`appdev-rg-dev-we-travelinspiration`

>[!INFO]
>This is a biased naming convention from a course -> [Getting Started with Microsoft Azure - Dometrain](https://dometrain.com/course/getting-started-azure-for-developers/)
>
>While I find it consistent and valid, every team in any company will have their own naming conventions.

# Kudu

This is a tool that is installed in every [[App Service]].

It provides a set of self-diagnostic tools and management features. 

>[!INFO]
>This can be accessed by adding .scm before azurewebsites. 
>
>Like this:
>`https://<app-name>.scm.azurewebsites.net`

# Using App Settings from [[App Service]]

Within our app service we can overwrite the values in our configurations as well as connection strings. This will be helpful in such cases where no new features are pushed but just an update on such configs.

Example:
![[Best practices and tips (Azure)-1.png]]
![[Best practices and tips (Azure)-2.png]]

In here, the amount of destinations to return will be overwritten in the webapp we have created.