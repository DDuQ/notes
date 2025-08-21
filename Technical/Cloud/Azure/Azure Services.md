# Azure Container Apps

![[Deploying .NET apps to Azure-1.jpeg|299x387]]
## Serverless
Serverless service to run containerized applications which is built on top of Kubernetes (strong for scalability and security).
## Fully managed cloud native  
platform We won't have to worry about OS, updates, patches or configuration. Azure takes care of it.
## Scalability (dynamic nature)
As mention as it is build on top of Kubernetes it will be able to scale an application (create instances) as required by traffic and then scale down as it reduces.
## Security
Being built on Kubernetes as well as being part of Azure's ecosystem it brings a strong foundation for security and governance (allowing to leverage identity and RBAC solution into our container app).

# Azure Container Registry

![[Deploying .NET apps to Azure-2.jpeg|301x362]]
## Automated container building and patching
Includes base images updates and task scheduling.

## Geo-replication
To efficiently manage a single registry across multiple regions.

## Integrated Security
with Entra ID \[formerly known as Azure Active Directory (Azure AD)], role-based access control, [[Docker]] Content Trust and virtual network integration.


# Azure SQL Database

![[Deploying .NET apps to Azure-3.jpeg|296x341]]
## Hyperscale
Allows for rapid scale up and down based on our needs.
## Serverless compute
We won't be charged when not using the database, it will go to sleep when not being used by the app container thus saving us costs.
## AI-Ready
Capabilities to add AI features such as Azure OpenAI, and Azure AI search.

# Log Analytics Workspace

![[Deploying .NET apps to Azure-4.jpeg|324x381]]

## Visualization Tools
Allows to take the logs from container apps, and create dashboards for visualization.
## Powerful Data Platform
Enables troubleshooting, diagnosing and analyzing telemetry data.
## Curated Insights
Customized monitoring experience with minimal configuration.
