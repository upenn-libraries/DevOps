## blacklight_dev

Name: discovery_app

Maintainer: Chris Clement and Michael Gibney

Base Image: pennlib/passenger-ruby23:0.9.23-ruby-build

Tag(s): :develop, :master

Container(s): discovery_app

Relationships between container(s): Depends on a MySQL container

Ports: N/A, managed by swarm (80, 443 exposed through reverse proxy)

Env vars:
ALMA_API_KEY | API key for Alma instance
ALMA_AUTH_SECRET | used by the blacklight_alma gem to decode JSON Web Tokens for social authentication
ALMA_DELIVERY_DOMAIN | hostname of Alma instance, used in URL construction
ALMA_INSTITUTION_CODE | Our assigned institution code for using Alma, used in URL construction
BOUND_WITHS_DB_FILENAME | fully qualified path to an SQLite DB used for mapping boundwith records. Used by indexing scripts
DATABASE_URL | URL of Discovery MySQL instance
DEVISE_SECRET_KEY | used by Devise to generate random tokens
NUM_INDEXING_PROCESSES | number of Solr indexing processes to run
PASSENGER_APP_ENV | development OR test OR production
RAILS_LOG_LEVEL | lowest level of log that will be used
SECRET_KEY_BASE | secret key for Rails
SOLR_URL | URL of Discovery Solr instance
SUMMON_ACCESS_ID | ID used by the BentoSearch gem to enable search result retrieval from Summon
SUMMON_SECRET_KEY | secret key used by the BentoSearch gem to enable search result retrieval from Summon

Networks: front_network, back_network (both managed by swarm)

Storage requirements: Needs to have access to a MySQL instance.

Logging: Rails logs to JSON files on host system, managed by Docker json-file log driver. Logs are only persisted on host node.

Using swarm mode?: Yes

Using docker-compose?: No

Service(s) provided by containers running this image: Provides a discovery UI as a read-only connection to a Solr search index.

Availability/uptime requirements: 24/7

Technical challenges/points of failure: Relies on the Shibboleth container (and the actual Shibboleth server) being up. Relies on Solr being up. Relies on a separate MySQL instance.

Deployment method:
1. Image is built and pushed using the build_blacklight_image Ansible playbook in the deploy-discovery repo.
2. Init script is created and added to target deploy server (franklinswarm).
3. On target server in `/opt/discovery/`, run `start_blacklight` or `update_blacklight` script.

Backup methods: N/A

Process management: Replicated and managed by swarm.

Swarm-specific:

Deployment criteria: Needs to have network access to Solr nodes, MySQL instance, and Shibboleth server (by proxy). Load balanced by swarm, should be exposed to public internet by reverse proxy.
Labeling: Restricted to nodes solr01 and solr02
Overlay networks: front_network, back_network

Registries: indexing-dev.library.upenn.int:5000

Source code (link): https://gitlab.library.upenn.edu/discovery/blacklight_dev
