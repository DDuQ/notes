# **Core concepts**

## Containers VS Virtual Machines

|                                                                    Container                                                                    |                            Virtual Machine                             |
| :---------------------------------------------------------------------------------------------------------------------------------------------: | :--------------------------------------------------------------------: |
| Designed to run a single app that share a host kernel. <br><br>Minimal interaction through CLI.<br><br>Lightweight to run / delete (immutable). | Full OS system.<br><br>Can be interacted with through UI and modified. |
## Image
Template for a container when first created. 
Makes easier setup for working in teams.
Containers are instances of an image.
## Tags 
Those are mutable, essentially pointers. 
E.g. Latest : could point to image id 1 or image id 2 depending on the preference.

``docker tag [image]:[name of the tag we want to create] : Create tags.
``docker rmi [image]:[name of the tag] : Removes images and also tags.

***NOTE***: by default latest will be used as the name of the tag if not specified.
## Layers
There are **layers** created on top of a base image (with incremental changes). 
Those are read-only (both layers and base image) and cached.

![[docker-layers.jpeg|470x262]]

## Docker file 
List of instructions used to generate an image.

``Docker build [path]``  
	path: defines docker **build context directory**; copies all in that directory > sub-directories > files (ignoring what is in docker ignore file), then packages it into a .TAR file and sends it to the docker daemon.

## Docker ignore 
Similar to .gitignore

# **Persistent Storage**

![[docker-0.jpeg|700x359]]

## Bind mount
Leverages the hosts' file system to persist data, granting the privileges of the host.

![[docker-3.jpeg|556x292]]
## tmpfs
Uses the host memory to allocate data, it differs from [[Docker#Bind mount |bind mounts]] and [[Docker#Volumes|volumes]] given that it's lifetime is the same as the container using it. 

More performant for in-memory use cases.

![[docker-4.jpeg|560x294]]

## Volumes 
Directories managed by docker which allows to set a place to persist data. 

E.g. When a database container is deleted all of its contents will be lost unless it was pointing to a volume.  If a new database container ( from the same image ) points to that volume it can leverage the data created by other dropped databases.

![[docker-1.png|334x276]]

Multiple containers ( from the same image ) could point to a single volume at the same time.

![[docker-2.jpeg|344x285]]


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

# **Security**

## Image scanning

``docker scout`` – This command allow us to scan images and check for CVEs (Common Vulnerabilities and Exposures). Essentially contrast them against a database of known CVEs.

![[CVEs.jpeg]]

Docker has a couple of layers of security and getting to a container, though CVEs emphasizes the importance of keeping everything up-to-date.

## Running containers as non-root

``RUN adduser --disabled-password --gecos '' appuser`` 
``USER appuser``
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

![[container-registry.jpg|700x367]]



