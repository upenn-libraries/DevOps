## Franklinforms

Name: Solr 5 for Bulwark

Maintainer: Katherine Lynch

Base Image: solr:5.3-alpine

Tag(s): solr5_dev:latest

Container(s): N/A

Relationships between container(s): N/A

Ports: 8983

Env vars: N/A

Networks: N/A

Storage requirements: Solr configuration (config_data) and Solr core (data) have their own Docker volumes.

Logging: N/A

Using swarm mode?: No

Using docker-compose?: Yes (for local development and production)

Service(s) provided by containers running this image: Provides forms for stuff

Production status: in production

Availability/uptime requirements: 9am-5pm, 7 days a week

Technical challenges/points of failure: N/A

Deployment method:
1. Pull latest code from repository
1. Use docker-compose to deploy latest image on colenda-back

Backup methods: N/A

Process management: N/A

Registries: N/A

Source code (link): https://gitlab.library.upenn.edu/repo/solr5_dev
