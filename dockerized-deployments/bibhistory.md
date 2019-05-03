## Bibhistory

Maintainer: Chris Clement

Base Image: ruby:2.3.1-slim

Tag(s): :production

Container(s): bibhistory (ruby:2.3.1-slim)

Relationships between container(s): N/A

Ports: 80

Env vars: Voyager database read-only, Sinatra app environment variable

Networks: N/A

Storage requirements:
None required.  Data source is externally-managed Oracle database.

Logging:
Standard Sinatra application logging to /dev/null.  Logs not relevant for retention.

Using swarm mode?: no

Using docker-compose?: yes

Service(s) provided by containers running this image: Direct lookup interface for legacy Voyager BibID and MFHD data, connecting to external Oracle database.

Availability/uptime requirements: 9am - 5pm, Monday - Friday

Technical challenges/points of failure: Voyager database is externally deployed and therefore is not an explicit dependency in the docker-compose stack.  Application is available internal to the library.

Deployment method:
Using docker-compose, on md-proc

Backup methods:
N/A

Process management:
N/A

Swarm-specific: N/A

Swarm-specific deployment criteria: N/A

Registries: indexing-dev

Source code (link): [Bibhistory](https://gitlab.library.upenn.edu/clemenc/bibhistory)
