# Definition
This is an [[How to handle Migrations#Incremental Approaches|incremental approach]] which allows maintenance and development of features without code freezes. 

The implementation of this is to work with a facade layer which will bring both applications together (legacy and modern) as if they were one, allowing to start moving endpoints and business logic without disrupting the user experience.

>[!INFO]
>A [[Reverse proxy#Definition|reverse proxy]] will be required for this strategy. Given the context of C#/.NET applications, [[YARP#Definition|YARP]] will be used.
>
>This approach leverages the [[How to handle Migrations#Phases of migration|phases of a migration]] in which [[How to handle Migrations#Upgrade source|upgrade source]] and [[How to handle Migrations#Test and deploy|test and deploy]] will happen in cycles given the incremental release of features.
