# Azure Services Used 

-  [[Deploying .NET apps to Azure#Azure Container Apps|Azure Container Registry]]
-  [[Deploying .NET apps to Azure#Azure SQL Database|Azure SQL Database]]
-  [[Deploying .NET apps to Azure#Azure Container Apps|Azure Container Apps]]
-  [[Deploying .NET apps to Azure#Azure Log Analytics Workspace|Azure Log Analytics Workspace]]
  
## Why to use Azure Container Apps

![[Deploying .NET apps to Azure-1.jpeg|299x387]]
### Serverless
Serverless service to run containerized applications which is built on top of Kubernetes (strong for scalability and security).
### Fully managed cloud native  
platform We won't have to worry about OS, updates, patches or configuration. Azure takes care of it.
### Scalability (dynamic nature)
As mention as it is build on top of Kubernetes it will be able to scale an application (create instances) as required by traffic and then scale down as it reduces.
### Security
Being built on Kubernetes as well as being part of Azure's ecosystem it brings a strong foundation for security and governance (allowing to leverage identity and RBAC solution into our container app).

## Why to use Azure Container Registry

![[Deploying .NET apps to Azure-2.jpeg|301x362]]
### Automated container building and patching
Includes base images updates and task scheduling.

### Geo-replication
To efficiently manage a single registry across multiple regions.

### Integrated Security
with Entra ID \[formerly known as Azure Active Directory (Azure AD)], role-based access control, [[Docker]] Content Trust and virtual network integration.


## Why to use Azure SQL Database

![[Deploying .NET apps to Azure-3.jpeg|296x341]]
### Hyperscale
Allows for rapid scale up and down based on our needs.
### Serverless compute
We won't be charged when not using the database, it will go to sleep when not being used by the app container thus saving us costs.
### AI-Ready
Capabilities to add AI features such as Azure OpenAI, and Azure AI search.

## Why to use Log Analytics Workspace

![[Deploying .NET apps to Azure-4.jpeg|324x381]]

### Visualization Tools
Allows to take the logs from container apps, and create dashboards for visualization.
### Powerful Data Platform
Enables troubleshooting, diagnosing and analyzing telemetry data.
### Curated Insights
Customized monitoring experience with minimal configuration.


# Infrastructure as Code

![[Deploying .NET apps to Azure-5.jpeg|624x194]]
# Terraform

![[Deploying .NET apps to Azure-6.jpeg|477x253]]

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

```tf
variable "env_id" {  
    type = string  
    description = "The environment id"  
    default = "dev"  
}  
  
variable "appname" {  
    type = string  
    description = "The name of the app"  
    default = "capybarapetapp"  
}  
  
variable "src_key" {  
    type = string  
    description = "The infrastructure source"  
    default = "terraform"  
}  
variable "subscription_id" {  
    type = string  
    description = "The Azure subscription id"  
    default = "[suscription_id]"  
}  
  
variable "sql_pass" {  
    type = string  
    description = "The SQL Server password"  
}
```
## Setup
Here we will build the required providers, that will then enable actions. 

E.g. Azure resource manager as provider to create a resources groups within our account.

```tf
terraform {  
  required_providers {  
    azurerm = {  
        source = "hashicorp/azurerm"  
        version = "4.39.0"  
    }  
  }  
}  
  
provider "azurerm" {  
  features {}  
  resource_provider_registrations = "none"  
  subscription_id = var.subscription_id  
}
```

On ``terraform > required_providers`` is specified the name of the provider as well as the ``source`` and ``version`` of said provider within Terraform's container registry.

Then on ``provider`` it specifies the ``features`` to be used as well as other configurations.

## Azure Resource Group

```tf
resource "azurerm_resource_group" "capybarapetapprg" {  
  location = "UK South"  
  name = "capybarapetapp-rg"  
  tags = {  
    environment = var.env_id  
    src = var.src_key  
  }  
}
```

In this case we are using an specific resource from the provider ([azurerm/resource_group](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/resource_group)).

This gets declared on the command ``resource`` followed by the name of the resource group, which contains its configuration within the curly braces. 

## Azure Container Registry

```tf
resource "azurerm_container_registry" "acr" {  
  location            = azurerm_resource_group.capybarapetapprg.location  
  name                = "${var.appname}registry${var.env_id}"  
  resource_group_name = azurerm_resource_group.capybarapetapprg.name  
  sku                 =  "Standard"  
  admin_enabled       = true  
  public_network_access_enabled = true  
  tags = {  
    environment = var.env_id  
    src = var.src_key  
  }  
}
```

Here we basically follow the documentation from [azurerm/container_registry]([azurerm_container_registry | Resources | hashicorp/azurerm | Terraform | Terraform Registry](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/container_registry)), and re-use the same tags for resource group.

## Azure Log Analytics Workspace

![[Deploying .NET apps to Azure-7.jpeg]]

In order to be able to create [[Deploying .NET apps to Azure#Azure Container Apps|Azure Container Apps]] we'll need to start from its dependencies.

```tf
resource "azurerm_log_analytics_workspace" "law" {  
  location            = azurerm_resource_group.capybarapetapprg.location  
  name                = "${var.appname}-law-${var.env_id}"  
  resource_group_name = azurerm_resource_group.capybarapetapprg.name  
  sku                 = "PerGB2018"  
  retention_in_days   = 30  
  
  tags = {  
    environment = var.env_id  
    src = var.src_key  
  }  
}
```

Here we stick to the required fields in the documentation on [azurerm/log_analytics_workspace](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/log_analytics_workspace).
## Azure Container Workspace

```tf
resource "azurerm_container_app_environment" "app_env" {  
  location            = azurerm_resource_group.capybarapetapprg.location  
  name                = "${var.appname}-appenv-${var.env_id}"  
  resource_group_name = azurerm_resource_group.capybarapetapprg.name  
  log_analytics_workspace_id = azurerm_log_analytics_workspace.law.id  
  
  tags = {  
    environment = var.env_id  
    src = var.src_key  
  }  
} 
```

Here we stick to the required fields in the documentation on [azurerm/container_app_environment](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/container_app_environment) .
## Azure Container Apps

```tf
resource "azurerm_container_app" "app" {  
  container_app_environment_id = azurerm_container_app_environment.app_env.id  
  name                         = "${var.appname}-api-${var.env_id}"  
  resource_group_name          = azurerm_resource_group.capybarapetapprg.name  
  revision_mode                = "Multiple"  
  template {  
    min_replicas = 1  
    max_replicas = 3  
    container {  
      cpu    = 0.25  
      image  = "mcr.microsoft.com/k8se/quickstart:latest"  
      memory = "0.5Gi"  
      name   = "${var.appname}-${var.env_id}"  
    }  
  }  
      
  ingress {  
    allow_insecure_connections = false  
    external_enabled = true  
    target_port = 8080  
    traffic_weight {  
      percentage = 100  
      label = "primary"  
      latest_revision = true  
    }  
  }  
  
  tags = {  
    environment = var.env_id  
    src = var.src_key  
  }  
}
```

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

```tf
resource "azurerm_mssql_server" "sql_server" {  
  location            = azurerm_resource_group.capybarapetapprg.location  
  name                = "${var.appname}-sqlserver-${var.env_id}"  
  resource_group_name = azurerm_resource_group.capybarapetapprg.name  
  version             = "12.0"  
  administrator_login = "capyadmin"  
  administrator_login_password = var.sql_pass  
  
  tags = {  
    environment = var.env_id  
    src = var.src_key  
  }  
}
```

Here we stick to the required fields in the documentation on [azurerm/mssql_server](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/mssql_server).

Take on mind that with ``administrator_login`` and ``administrator_login_password`` we configure manual login (similar to SA credentials). But we could also set it up with Entra ID (formerly Azure AD).

Now we can create the database:

```tf
resource "azurerm_mssql_database" "db" {  
  name      = "CapybaraPetApp"  
  server_id = azurerm_mssql_server.sql_server.id  
  collation = "SQL_Latin1_General_CP1_CI_AS"  
  license_type = "LicenseIncluded"  
  max_size_gb = 10  
  sku_name = "S1"  
  zone_redundant = false  
  
  lifecycle {  
    prevent_destroy = false  
  }  
    
  tags = {  
    environment = var.env_id  
    src = var.src_key  
  }  
}  
```

Here we stick to the required fields in the documentation on [azurerm/mssql_database](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/mssql_database).

Some configs to take on mind:

``zone_redundant``: Allows replicas of the database to be spread across multiple availability zones (not used for the matter of this PoC).

``lifecycle > prevent_destroy``:  This ensures that this database won't be deleted. 
E.g. The resource group gets deleted, this config prevents to delete the data allocated (useful for production environments).

And lastly we will setup firewall rule:

```tf
resource "azurerm_mssql_firewall_rule" "rule" {  
  end_ip_address   = "0.0.0.0"  
  name             = "all_azure"  
  server_id        = azurerm_mssql_server.sql_server.id  
  start_ip_address = "0.0.0.0"  
}
```

Here we stick to the required fields in the documentation on [azurerm/mssql_firewall_rule](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/mssql_firewall_rule).

This resource configuration essentially will allow all azure resources to connect to the database without needing to whitelist them.

# Command Workflow

1. ``terraform init``: Scans the setup and checks for ``required_providers`` to pull the image.
2. ``terraform plan``: Will compare against Azure (if the resource already exists) and then create a plan to materialize said resource.

	![[Deploying .NET apps to Azure-8.jpeg|641x268]]

3. ``terraform apply`` - Executes the plan from point 2 (requires input to approve the plan).

	![[Deploying .NET apps to Azure-9.jpeg|640x259]]
   


# CI/CD With GitHub Actions

![[Deploying .NET apps to Azure-10.jpeg|693x138]]

![[Deploying .NET apps to Azure-11.jpeg|0x0]]

## GitHub Actions 101

### Yaml
This will be the type of file we'll using to configure our pipelines.
### Keywords
#### ``name`` 
Will define the name of the pipeline.

```yaml
name: Build and Deploy API to Azure
```

Can be seen displayed on GitHub like this:
![[Deploying .NET apps to Azure-4.png]]

#### ``on`` 
Determines that an action will trigger the workflow.
##### ``push`` 
Action.
##### ``paths``
Defines which specific paths will trigger the workflow.
##### ``paths-ignore``
Defines which paths will be ignore (won't trigger the workflow).
##### ``branches``
Defines which branches will trigger the workflow, when the action happens.

```yaml 
on:  
  push:
	paths-ignore:  
	  - 'iac/**'
    branches:  
      - main
```

**NOTE**: Be careful about YAML spacing, it could drive into unexpected issues.

#### ``jobs``
Determines the start of the workflow.
##### ``Job_id``
Will define a name as identifier for the current job (can contain alphanumeric characters, including - and _ ).

Can be seen displayed on GitHub:
![[Deploying .NET apps to Azure-5.png]]
##### ``runs-on``
Defines the image used to run the step.
##### ``outputs``
Defines variables in the current job which can be used by other jobs (that depends on this job).

```yaml
jobs:  
  build-and-deploy:  
    runs-on: ubuntu-latest  
    outputs:   
      short_sha: ${{ steps.vars.outputs.short_sha }}  
```

##### ``steps``
Can run commands, run setup tasks, or run an action in your repository, a public repository, or an action published in a Docker registry.
##### ``name``
A name to display in GitHub for the step.
##### ``id``
A unique identifier for the step.
##### ``uses``
Leverages an existing workflow from GitHub.
##### ``with``
Specifies the arguments required for the workflow.

```yaml
steps:  
  - name: Checkout repository  
    id: github  
    uses: actions/checkout@v3  
   
  - name: Setup WebApi .NET  
    uses: actions/setup-dotnet@v2  
    with:  
      dotnet-version: '8.0.x'
```

##### ``env``
Sets environment variables for steps to use in the virtual environment.
##### ``run``
Runs command-line programs using the operating system's shell.

```yaml
- name: Build and push Docker image to ACR  
  env:  
    ACR_REGISTRY: ${{ vars.ACR_SERVER }}  
    IMAGE_TAG: ${{ steps.vars.outputs.short_sha }}  
    APP_NAME: ${{ vars.APP_NAME }}  
  run: |  
    docker build -f CapybaraPetApp.Api/Dockerfile -t $ACR_REGISTRY/$APP_NAME:$IMAGE_TAG -t $ACR_REGISTRY/$APP_NAME:latest .  
    docker push $ACR_REGISTRY/$APP_NAME --all-tags
```

##### ``needs``
Identifies any jobs that must complete successfully before this job will run.

```yaml
deploy-container-to-ACS:  
  runs-on: ubuntu-latest  
  needs: build-and-deploy
```

## GitHub Actions for IaC

In order to leverage the IaC created with [[Deploying .NET apps to Azure#Building the services|Terraform]], we'll create a new workflow:
```yaml
name: "IaC Deploy"  
  
on:  
  push:  
    paths:  
      - 'iac/**'  
    branches:  
      - main  
```

**Note:** Take on mind that both [[Deploying .NET apps to Azure#``paths``|paths]] and [[Deploying .NET apps to Azure#``paths-ignore``|paths-ignore]] helps us to isolate the scope of our workflows.

```yaml
jobs:  
  terraform-deploy:  
    runs-on: ubuntu-latest  
    env:  
      ARM_CLIENT_ID: ${{ secrets.ARM_CLIENT_ID }}  
      ARM_CLIENT_SECRET: ${{ secrets.ARM_CLIENT_SECRET }}  
      ARM_SUBSCRIPTION_ID: ${{ secrets.ARM_SUBSCRIPTION_ID }}  
      ARM_TENANT_ID: ${{ secrets.ARM_TENANT_ID }}  

```

Here we specify ARM (Azure Resource Manager) variables for the ARM template, which will be used by the terraform commands.

```yaml	  
    steps:  
      - name: Checkout Repo  
        uses: actions/checkout@v3  
```

This is a common step to make our source code available (for following actions to be able to run commands on it).

```yaml 
      - name: Azure Login  
        uses: azure/login@v1  
        with:  
          creds: ${{ secrets.AZURE_CREDENTIALS }}  
```

 Aside from the environment variables this specific credential allows us to authenticate in order to use azure services.

```yaml
      - name: Terraform install  
        uses: hashicorp/setup-terraform@v3  
        with:  
          terraform_version: 'latest'  
          - name: Terraform init  
        run: terraform init  
        working-directory: ./iac/  
          
      - name: Terraform Validate  
        run: terraform validate  
        working-directory: ./iac/  
          
      - name: Terraform Plan  
        run: terraform plan -var="sql_pass=${{ secrets.TF_VAR_DB_PASSWORD }}" -out=tfplan  
        working-directory: ./iac/  
          
      - name: Terraform Apply  
        run: terraform apply -auto-approve tfplan  
        working-directory: ./iac/  
          
      - name: Notify failure  
        if: failure()  
        run: echo "Terraform deployment failed for ${{ github.ref_name }}"
```

All those commands are part of the [[Deploying .NET apps to Azure#Terraform|Terraform]] CLI, allowing us to leverage the IaC we've created in the path: *./iac/*,  specifying ``working-directory`` will run the commands on the correct place.

Also, noteworthy mention:

``-var``: Allows us to specify the variable *sql_pass* which holds the password of our database (otherwise we would need to input the password manually).

``-out``: Allows us to set the output of the current command as a variable; **tfplan**.
This will be used later when applying it.

``-auto-approve``: Declares that it will automatically approve **tfplan**.


## GitHub secrets and variables


### Secrets

| Name                                                    | Value                                                                                                                                                                                                                                                                                                |
| ------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| ACR_PASSWORD                                            | When the Azure Container Registry gets created, it can be found on *Settings > Access keys*                                                                                                                                                                                                          |
| ACR_USERNAME                                            | Same as ACR_PASSWORD                                                                                                                                                                                                                                                                                 |
| AZURE_CREDENTIALS                                       | It will require to run a command in Terminal (being logged into Azure account):<br><br>```shell<br>az ad sp create-for-rbac --name github-auth --role contributor --scopes /subscriptions/dd28d209-7ba1-4ba1-ab36-0cd05e2e3308/resourcegroups/capybarapetapp-rg --json-auth --output json<br>```<br> |
| DB_CONNECTION_STRING                                    | When SQL Server as well as database gets created it will provide a connection string (might require changes depending on config). <br>It can be found on *Settings > Connection strings*                                                                                                             |
| TF_VAR_DB_PASSWORD                                      | Password defined for the SQL database.                                                                                                                                                                                                                                                               |
| ARM_CLIENT_ID<br>ARM_CLIENT_SECRET<br><br>ARM_TENANT_ID | It will require to run a command in Terminal (being logged into Azure account):<br><br>```shell<br>az ad sp create-for-rbac --name devaccess<br>```                                                                                                                                                  |
| ARM_SUBSCRIPTION_ID                                     | Can be found when the resource group gets created.                                                                                                                                                                                                                                                   |

### Variables

| Name       | Value                                |
| ---------- | ------------------------------------ |
| ACR_SERVER | capybarapetappregistrydev.azurecr.io |
| APP_NAME   | capybarapetapp                       |
