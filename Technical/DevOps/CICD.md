# Definition

Stands for *Continuous Integration / Continuous Delivery/Deployment*.
## Continuous Integration (CI)

This is a practice where developers merge (integrate) changes into a shared repository. 

>[!INFO]
Each integration triggers an automated build and test process to detect errors early.

## Continuous Delivery/Deployment (CD)

Extends [[CICD#Continuous Integration (CI)|CI]] by automating the release process. After code passes test it can be automatically deployed to Staging (**Continuous Delivery**) or to production (**Continuous Deployment**).

### Continuous Delivery VS Continuous Deployment

| Continuous Delivery                                                                                                                            | Continuous Deployment                                                                                                              |
| :--------------------------------------------------------------------------------------------------------------------------------------------- | :--------------------------------------------------------------------------------------------------------------------------------- |
| Code changes are **automatically built, tested and prepared for release**.<br><br>Deployment to production is **manual** (requires approvals). | <br>Every change that passes automated tests is **automatically deployed all the way to production**, with **NO manual approval**. |

