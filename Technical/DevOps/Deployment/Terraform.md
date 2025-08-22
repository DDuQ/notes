# Definition

Open-source Infrastructure as Code (IaC) tool that allows developers and IT teams to define, provision, and manage cloud resources using a declarative configuration language. 

With Terraform, you can automate deployments, ensure consistency across environments, and manage infrastructure at scale across multiple cloud providers.

# Qualities

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