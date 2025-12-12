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
Although we can manually configure resources such as containers, web APIs, databases, and registries through a cloud provider’s UI, a more reliable approach is to define and deploy these resources using code.

This reduces potential errors when creating resources since we guarantee to have the exact same configuration on different environments.
## Multi-cloud Management (Cloud agnostic)
Allows to connect to different cloud providers.

# State file

This is a crucial component that stores information about the managed infrastructure and configuration. It is used by terraform to map resources to our configuration, track metadata, and improve performance for large infrastructures.

By default it is stored in `terraform.tfstate` as a local file but *it is advised to store it in a cloud provider*.

>[!IMPORTANT] TL;DR
>State file works as the **source of truth** which ==allows terraform to effectively being able to compare updates, creations or deletions of resources== by comparing actual infrastructure against desired state. 

