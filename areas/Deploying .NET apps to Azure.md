
# Azure Services Used 

~ [[Deploying .NET apps to Azure#Azure Container Apps|Azure Container Registry]]
~ [[Deploying .NET apps to Azure#Azure SQL Database|Azure SQL Database]]
~ [[Deploying .NET apps to Azure#Azure Container Apps|Azure Container Apps]]
~ [[Deploying .NET apps to Azure#Azure Log Analytics Workspace|Azure Log Analytics Workspace]]

# Why to use Azure Container Apps

![[azure-deploy-0.jpeg|299x387]]
### Serverless
Serverless service to run containerized applications which is built on top of Kubernetes (strong for scalability and security).
### Fully managed cloud native  
platform We won't have to worry about OS, updates, patches or configuration. Azure takes care of it.
### Scalability (dynamic nature)
As mention as it is build on top of Kubernetes it will be able to scale an application (create instances) as required by traffic and then scale down as it reduces.
### Security
Being built on Kubernetes as well as being part of Azure's ecosystem it brings a strong foundation for security and governance (allowing to leverage identity and RBAC solution into our container app).

# Why to use Azure Container Registry

![[azure-deploy-2.jpeg|301x362]]
## Automated container building and patching
Includes base images updates and task scheduling.

## Geo-replication
To efficiently manage a single registry across multiple regions.

## Integrated Security
with Entra ID \[formerly known as Azure Active Directory (Azure AD)], role-based access control, Docker Content Trust and virtual network integration.


# Why to use Azure SQL Database

![[azure-deploy-1.jpeg|296x341]]
## Hyperscale
Allows for rapid scale up and down based on our needs.
## Serverless compute
We won't be charged when not using the database, it will go to sleep when not being used by the app container thus saving us costs.
## AI-Ready
Capabilities to add AI features such as Azure OpenAI, and Azure AI search.

# Why to use Log Analytics Workspace

![[azure-deploy-3.jpeg|324x381]]

## Visualization Tools
Allows to take the logs from container apps, and create dashboards for visualization.
## Powerful Data Platform
Enables troubleshooting, diagnosing and analyzing telemetry data.
## Curated Insights
Customized monitoring experience with minimal configuration.


# Infrastructure as Code

![[a95993f5d3ca072d74468e7b27c42464_MD5.jpeg|624x194]]
# Terraform

![[71a241a9bd5939e2709bde1d633b9e9d_MD5.jpeg|477x253]]

## Declarative Language
It has a structure for us to follow and have resources created.
## Automation
We can connect with different CI/CD infrastructure in order to automate the creation of resources automatically.
## Consistency
While we rely on ourselves to setup resources directly (containers, web api, databases, container registry, etc.) on Azure portal UI it is a better approach to use code to do so. 

This reduces the possible errors to constantly creating resources since we guarantee to have the exact same configuration on different environments.
## Multi-cloud Management
Allows to connect to different cloud providers.

# Building the services

## Variables  
Essentially assigned typed values that will allow direct reference to static data which describes its purpose.

![[2f0caf8df6bcade69635a2760aad7126_MD5.jpeg|406x411]]

## Setup
Here we will build the required providers, that will then enable actions. 

E.g. Azure resource manager as provider to create a resources groups within our account.

![[6b29905c3893a71e68d2f7bfddda5420_MD5.jpeg|398x412]]


On ``terraform > required_providers`` is specified the name of the provider as well as the ``source`` and ``version`` of said provider within Terraform's container registry.

Then on ``provider`` it specifies the ``features`` to be used as well as other configurations.

## Azure Resource Group

![[ffb681abe0acb5e615144e354b4c096b_MD5.jpeg]]

In this case we are using an specific resource from the provider ([azurerm/resource_group](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/resource_group)).

This gets declared on the command ``resource`` followed by the name of the resource group, which contains its configuration within the curly braces. 

## Azure Container Registry

![[ab67448e3c2a7c2f3f4a71afd39bf6c1_MD5.jpeg]]

Here we basically follow the documentation from [azurerm/container_registry]([azurerm_container_registry | Resources | hashicorp/azurerm | Terraform | Terraform Registry](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/container_registry)), and re-use the same tags for resource group.

## Azure Log Analytics Workspace

![[5e49d8eb8f2214190062f527656a9332_MD5.jpeg]]

In order to be able to create [[Deploying .NET apps to Azure#Azure Container Apps|Azure Container Apps]] we'll need to start from its dependencies.

![[a63853ab3c5e6c7089143221004d8505_MD5.jpeg]]
Here we stick to the required fields in the documentation on [azurerm/log_analytics_workspace](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/log_analytics_workspace).

## Azure Container Workspace
 
![[4b3eb9072633484f49b3c64ebbf4ea4c_MD5.jpeg]]

Here we stick to the required fields in the documentation on [azurerm/container_app_environment](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/container_app_environment) .
## Azure Container Apps

![[f827f4b4fc352551b86b87f4bd1016d2_MD5.jpeg]]

Here we stick to the required fields in the documentation on [azurerm/container_app](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/container_app).

Important configs to have in mind:

Inside ``template`` we got ``min|max_replicas`` which refers to how many instances of our application will be running. Also there is a ``container`` where we specify the resources to be allocated.

Given that we are using Kubernetes image we need to set some rules on ``ingress``:
	**But why?**
		Our container will be deployed in a walled garden, which means that no one will be able to communicate to it and we won't be able to reach it.
	**What to do then?**
		We need to create some rules to allow the container app to connect to the internet so we are able to access it.

**Rules**
``allow_insecure_connections``: This will be ``false`` given that we want to ensure requests through https.

``external_enabled``: Allows external calls (``true``).

``target_port``: Set to our preferred port (8080 in this case). 

``traffic weight``: Used in case we have multiple applications running inside the container app (e.g. if there is a load balancer set, this will define how it scales up).

## Azure SQL Database

In order to create an Azure SQL database, first we require to have Azure SQL Server set.

![[098254ad574c3dadcabbdf6e41816432_MD5.jpeg]]

Here we stick to the required fields in the documentation on [azurerm/mssql_server](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/mssql_server).

Take on mind that with ``administrator_login`` and ``administrator_login_password`` we configure manual login (similar to SA credentials). But we could also set it up with Entra ID (formerly Azure AD).

Now we can create the database:
![[eaea67a6ec61d631ae089bd18eec36e9_MD5.jpeg]]

Here we stick to the required fields in the documentation on [azurerm/mssql_database](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/mssql_database).

Some configs to take on mind:

``zone_redundant``: Allows replicas of the database to be spread across multiple availability zones (not used for the matter of this PoC).

``lifecycle > prevent_destroy``:  This ensures that this database won't be deleted. 
E.g. The resource group gets deleted, this config prevents to delete the data allocated (useful for production environments).

And lastly we will setup firewall rule:
![[bd2933b664439047a3442fcdb4395eeb_MD5.jpeg]]

Here we stick to the required fields in the documentation on [azurerm/mssql_firewall_rule](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/mssql_firewall_rule).

This resource configuration essentially will allow all azure resources to connect to the database without needing to whitelist them.

# Command Workflow

1. ``terraform init`` - Scans the setup and checks for ``required_providers`` to pull the image.
2. ``terraform plan`` - Will compare against Azure (if the resource already exists) and then create a plan to materialize said resource.

	![[d22dd624a07176327ba82d6cb58b70d4_MD5.jpeg|641x268]]

3. ``terraform apply`` - Executes the plan from point 2 (requires input to approve the plan).

	![[1e08c7140eed5fde65776134a6df138f_MD5.jpeg|643x260]]
   

