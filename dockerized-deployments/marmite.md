## Marmite

Name: marmite

Maintainer: Kate Lynch

Base Image: ruby-2.4.0

Tag(s): :latest

Container(s): sinatra (marmite:latest), db (mysql/mysql-server:5.7)

Relationships between container(s): sinatra container backed by db container for database storage

Ports: 9292

Env vars: MySql connection credentials, Alma Bibs read-only API key

Networks: Out of the box Docker networks

Storage requirements: Docker volume for MySQL data

### Volumes, storage mounts, mappings

Logging:

What is being logged?  What are retention needs/policies?:
No retention policies in place.  Standard rails and Sidekiq logging configurations in use in production.

Using swarm mode?: no

Using docker-compose?: yes

### Services

Service(s) provided by containers running this image:
Harvesting endpoint of various expressions of descriptive and structural metadata for objects in Library-based applications (example, Colenda, the DLA, OPenn).

Production status: In beta with active feature development

Availability/uptime requirements: 9a - 5pm, M-F (no requirement on file, this is an educated guess.)

Technical challenges/points of failure: None significant at this time. No logs are currently retained and no backups are being made of the data in the database due to production status, however this will need to change before the application can be considered live in production.

Deployment method: Documented in the readme; build marc_alma:latest image on deployment server with configured .env file, run docker-compose up with configured .env fi

Backup methods: None currently in place.  Working to create a backup method for mysql data store.

Process management: Minimal, using docker-compose with `restart:unless-stopped` policy.

Registries: Local

Source code (link): [https://gitlab.library.upenn.edu/katherly/marmite/](https://gitlab.library.upenn.edu/katherly/marmite/)

### Swarm-specific:

Deployment criteria: Not applicable

Labeling: Not applicable

Overlay networks: Not applicable

Relationship map: Not applicable
