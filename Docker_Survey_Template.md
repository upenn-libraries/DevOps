# Docker Survey Template
Template for surveying existing Dockerized applications as of 8/3/17.  The goal of these documents is to collect information about patterns emerging in use of Docker for deployments, in order to develop recommendations for best practices in retooling existing applications/deploying new ones.

## Links
* https://github.com/upenn-libraries/DevOps/tree/gitlab-updates/dockerized-deployments

## Custom Docker Images Listing
Below is a preliminary list of known applications/scripts that have custom Docker images.

* Franklin
  * Blacklight_dev
  * Franklin Forms - FIRST PASS DONE
  * Apache_shib
  * Solr service (cores)
  * Solr-haproxy
  * Solr-zookeeper
  * Franklin-mysql 
* Bulwark
* Bulwark FCRepo
* Bulwark Solr5
* POP
* Image Sync
* Marmite - FIRST PASS DONE
* Acorn
* Omeka_a11y
* Omeka_classic
* ET Run
* VIVO
* Schoenberg Database of Manuscripts
* Bibhistory
* Jenkins
* PHALT
* Colenda-cantaloupe

## Composed Services Listing
Below is a preliminary list of services utilizing exclusively stock Docker images with composed/configured deployment options.

Coming soon.

# Template
Maintainer:

Base Image:

Tag(s):

Container(s):
Indicate base images for each container

Relationships between container(s):

Ports:

Env vars:

Networks:

Storage requirements:
Volumes, storage mounts, mappings

Logging:
What is being logged?  What are retention needs/policies?

Using swarm mode?:

Using docker-compose?:

Service(s) provided by containers running this image:

Availability/uptime requirements:

Technical challenges/points of failure:

Deployment method:
How is this being deployed in production?

Backup methods:
What backup measures are in place, for which piece(s) of the application

Process management:
How are processes running in the container managed?

Swarm-specific:

* Deployment criteria:
* Why things run where they do, informs labeling
* Labeling
* Overlay networks
* Relationship map

Registries:

Source code (link):
