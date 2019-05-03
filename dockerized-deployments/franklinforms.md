## Franklinforms

Name: Franklinforms

Maintainer: Christopher Clement

Base Image: codeforkjeff/passenger-ruby23:0.9.19-ruby-build

Tag(s): franklinforms:development, franklinforms:master

Container(s): N/A

Relationships between container(s): N/A

Ports: 80

Env vars:

|   |   |
|---|---|
|ALMA_API_KEY | Alma API key |
| PASSENGER_APP_ENV | development OR test OR production |
| ILLIAD_DBHOST | ILLiad DB host to connect to |
| ILLIAD_USERNAME | username to connect to ILLiad DB |
| ILLIAD_PASSWORD | password to connect to ILLiad DB |
| ILLIAD_DATABASE | name of the ILLiad database |
| PCOM_DBI | DBI string to connect to Penn Community |
| PCOM_USERNAME | DB username to connect to Penn Community |
| PCOM_PASSWORD | DB password to connect to Penn Community |
| SECRET_KEY_BASE | secret key for Rails |

Networks: N/A

Storage requirements: N/A

Logging: Rails logs to container filesystem; Phusion logs to standard out

Using swarm mode?: Yes

Using docker-compose?: Yes (for local development)

Service(s) provided by containers running this image: Provides forms for stuff

Production status: in production

Availability/uptime requirements: 24/7

Technical challenges/points of failure: Needs to connect to ILLiad and Penn Community

Deployment method:
1. Build image, tag and push to indexing-dev registry
1. Run /opt/discovery/update_franklinforms.sh on franklinswarm to use new image in swarm

Backup methods: N/A

Process management: N/A

Registries: indexing-dev.library.upenn.int

Source code (link): https://gitlab.library.upenn.edu/clemenc/franklinforms

Swarm-specific:

Franklinforms runs as a service named franklin_forms_service in the swarm and is restricted to nodes solr01 and solr02, since deploying to VM-based nodes results in downtime. Franklinforms connects to the front_network overlay network. Franklinforms service containers use the environment file located at /opt/discovery/.franklinforms-environment.
