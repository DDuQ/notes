# Definition

Open-source platform that allows to package applications (and their dependencies) into containers.

Containers are lightweight, portable, and consistent environments that ensure your application runs the same way across development, testing, and production.

# Core concepts

## Containers VS Virtual Machines

|                                                                    Container                                                                    |                              Virtual Machine                               |
| :---------------------------------------------------------------------------------------------------------------------------------------------: | :------------------------------------------------------------------------: |
| Designed to run a single app that share a host kernel. <br><br>Minimal interaction through CLI.<br><br>Lightweight to run / delete (immutable). | <br>Full OS system.<br><br>Can be interacted with through UI and modified. |
## Image
Template for a container when first created. 
Makes easier setup for working in teams.
Containers are instances of an image.
## Tags 
Those are mutable, essentially pointers. 
E.g. Latest : could point to image id 1 or image id 2 depending on the preference.

``docker tag [image]:[name of the tag we want to create] : Create tags.
``docker rmi [image]:[name of the tag] : Removes images and also tags.

>[!NOTE]
by default latest will be used as the name of the tag if not specified.
## Layers
There are **layers** created on top of a base image (with incremental changes). 
Those are read-only (both layers and base image) and cached.

![[Docker-1.jpeg|470x262]]

## Dockerfile 
List of instructions (commands) used to generate an image.

Initial the IDE (e.g. Visual Studio, Rider, etc.) will generate a generic template for containerization with a various commands:

![[Docker-2.jpeg|520x352]]

>[!NOTE]
Take on mind that each command is a [[Docker#Layers|layer]].

At the left, always will start with a keyword (there are not a lot of them):
![[Docker-3.jpeg|121x277]]

Normally it will always start with ``FROM`` which refers to the image used for the build (dotnet SDK in this case):
![[Docker-4.jpeg]]

This creates a temporary intermediate container from the image, then it will run this command within the container.
![[Docker-5.jpeg|201x53]]

``WORKDIR`` creates a new directory (if it doesn't exists) and changes to it as the current directory.
![[Docker-6.jpeg]]

``COPY`` command copies [[Docker#Docker core commands|docker build]] context from docker daemon into the intermediate container, in this case it takes the .csproj file into *DockerCourseApi* 
(.csproj -> src/DockerCourseApi).

``RUN`` executes a shell command, in this case *restore* from dotnet SDK. This will create a cached layer (meaning we won't need to run a restore until the .csproj file changes).
	``dotnet restore`` installs the packages specified in .csproj.

![[Docker-7.jpeg|152x49]]
``COPY . .`` here we copy everything else from the [[Docker#Docker core commands|docker build]] context.

![[Docker-8.jpeg|433x45]]
``WORKDIR`` Changes directory again to src/DockerCourseApi.

![[Docker-9.jpeg|700x58]]
``RUN`` runs another command, to publish the application.

![[Docker-10.jpeg|660x237]]
Here we notice another ``FROM`` command, which means we have [[Docker#Multistage Dockerfiles|multiples stages]]. 
``FROM``: creates another temporary intermediate container from that image.
``WORKDIR``: creates *app* directory and moves to it.
``EXPOSE``: this is more of documentation to tell which ports are exposed (does not define which ports are exposed).
``COPY``:  Copies from the *build* stage specifically the */app/publish* which is the published directory.
``ENTRYPOINT``: Basically executes the command, which runs when the container spins up.

### Multistage Dockerfile

Allows us to distribute actions being made on how we want to generate our image, each stage accomplishes a goal.

E.g. build stage for building our image contents | publish to publish our application.

## Docker core commands

### ``Docker build [path] .``

``path``: Defines docker **build context directory**; copies all in that directory > sub-directories > files (ignoring what is in docker ignore file), then packages it into a .TAR file and sends it to the docker daemon.

``.``: Takes the current directory.

## Docker ignore 
Similar to .gitignore

# Persistent Storage

![[Docker-11.jpeg|700x359]]

## Bind mount
Leverages the hosts' file system to persist data, granting the privileges of the host.

![[Docker-12.jpeg|556x292]]
## tmpfs
Uses the host memory to allocate data, it differs from [[Docker#Bind mount |bind mounts]] and [[Docker#Volumes|volumes]] given that it's lifetime is the same as the container using it. 

More performant for in-memory use cases.

![[Docker-13.jpeg|560x294]]

## Volumes 
Directories managed by docker which allows to set a place to persist data. 

E.g. When a database container is deleted all of its contents will be lost unless it was pointing to a volume.  If a new database container ( from the same image ) points to that volume it can leverage the data created by other dropped databases.

![[Docker-14.jpeg|334x276]]

Multiple containers ( from the same image ) could point to a single volume at the same time.

![[Docker-15.jpeg|344x285]]


# Networking

To ensure communication between containers, it is required to have them existing in the same network. docker then will be able to resolve DNS of each container facilitating communication through service discovery (e.g. API project referencing a database).

## Types

There are 3 common types of networks: Bridge, Host, None

- **Bridge** – Default network for containers; provides NAT-based connectivity, isolating containers from the host but allowing communication through published ports.

- **Host** – Removes the network isolation; containers share the host’s network stack, using its IP and ports directly.
  
- **None** – Disables networking entirely; the container has no network interfaces apart from `lo` (localhost).

Other networks not so common (for devs):

- **Macvlan** – Each container gets a MAC address, it requires appropiate infrastructure. (has a drawback of promiscuous mode) 

- **IPvlan** – Each containers has an address matching the network's router subnet.

By default in docker compose will create a network based on bridge type.

## Docker internal host

Docker desktop adds DNS entries - ``host.docker.internal`` / ``gateway.docker.internal`` - into hosts file (works on Linux, Windows, Mac) which allows a common way to access hosts' data.

# Security

## Image scanning

``docker scout`` – This command allow us to scan images and check for CVEs (Common Vulnerabilities and Exposures). Essentially contrast them against a database of known CVEs.

![[Docker-16.jpeg]]

Docker has a couple of layers of security and getting to a container, though CVEs emphasizes the importance of keeping everything up-to-date.

## Running containers as non-root

```yaml
RUN adduser --disabled-password --gecos '' appuser 
USER appuser
```

This is a bash command that changes to a non-root user to reinforce security before we set the entrypoints for our app.
(gecos means don't interactively prompt for various info about the user)


## CMD VS Entrypoint

|                          CMD                           |                                     Entrypoint                                      |
| :----------------------------------------------------: | :---------------------------------------------------------------------------------: |
|      Can be overridden as it exists as a default       | Fixed actions, cannot be overridden by default (requires ``--entrypoint`` to do so) |
| Default args (in case not specified in docker command) |                                    Main process                                     |

## Hosting

This image summarizes the workflow to publish images into a container registry (in this case docker hub).

Said image can get pulled either by an user or a hosting solution (such as AKS / EKS).

![[Docker-17.jpeg|700x367]]



