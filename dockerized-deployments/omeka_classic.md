## Omeka Classic

Name: omeka_classic

Maintainer: Kate Lynch

Base Image: php:5.6-apache

Tag(s): :latest

Container(s): omeka_app, db

Relationships between container(s): omeka_app relies on db for storing configuration

Ports: 80

Env vars:
MYSQL_ROOT_PASSWORD | Password for the MySQL root user
MYSQL_DATABASE | name for the database used by Omeka
MYSQL_USER | database user used to connect to MySQL
MYSQL_PASSWORD | password for database user in MYSQL_USER

Networks: N/A (uses default network)

Storage requirements: Uses three separate volumes for plugins, themes, and uploaded files. Uses a separate volume for database storage.

Logging: Application and HTTP server logs to container filesystem.

Using swarm mode?: No

Using docker-compose?: Yes

Service(s) provided by containers running this image: Provides a new single-site Omeka instance for managing and showcasing digital collections.

Production status: not in production

Availability/uptime requirements: N/A

Technical challenges/points of failure: N/A

Deployment method:
1. Build Docker image on target server.
2. Run `docker-compose up -d` on target server.

Backup methods: N/A

Process management: Docker Compose using `unless-stopped` restart policy.

Registries: N/A (built locally)

Source code (link): https://gitlab.library.upenn.edu/katherly/omeka_classic

Swarm-specific: N/A
