# Best practices for docker-compose

* Requirements
* Guidelines
* Services
* Volumes
* Example
* Swarm considerations
  * Services
  * Volumes
  * Networks
  * Configs
  * Secrets
  * Example
* References

## `docker-compose` suggested structure

For readability, sections of the `docker-compose` files should be ordered as follows:

```yml
version: '3.7'

secrets:

config:

services:

networks:

volumes:
  
```

## General guidelines

* Use the latest version of docker-compose that is supported by your version of Docker.

* If you want to take advantage of Docker configs and Docker secrets, you will need to run in Swarm mode, which minimally requires Docker 1.13+ and docker-compose file version 3.x+.

* Use `docker-compose` for orchestration only when possible.  Do not build images using `docker-compose` in deployment.

* Store your `docker-compose.yml` file in version control, alongside the application code.

* Use the long syntax for all configuration options when possible, for clarity.

* Use one `docker-compose.yml` file for both development and production.

* Do not hard-code sensitive information in `docker-compose.yml` files.

## Services

* Pull pre-built images from a registry as opposed to using the build command.

* Pin versions of images to something other thant `:latest`.  Ideally, a major or minor `:alpine` release.

* Alphabetize your services' properties for uniform organization between projects and ease of readability for pull requests.

* Add an appropriate restart policy for services (not available in Swarm mode).

* Name your services so that they are traceable to the source code base.

* Make sure all services have an entrypoint defined that ensures container startup even in the event of service failure, such as `entrypoint: ["sleep", "10000"]`, to aid in debugging inside containers when services fail to restart.

## Volumes

* Volume definitions should appear after the network definitions.

* Do not create volumes unless you have data that needs to persist across deployments.

* Volumes should be used to separate data from the application.

## Example

## Swarm considerations

### Services

### Volumes

* Volumes in a swarm environment are created on one of the nodes in the swarm.

* Label the associated service such that it only runs on a specific swarm node to prevent undesired behavior.

### Networks

* The network definitions should appear after the service definitions.

* Container ports should match host ports where possible.

* In the event where the host port needs to be configurable, use an environment variable.

### Configs

* Docker configs can only be used in swarm mode.

* The configs section should appear before the service definitions.

* Environment-specific non-sensitive information should be mapped into a service via a config.

### Secrets

* Docker secrets can only be used in swarm mode.

* The secrets section should appear after the config definitions.

* Secrets should be used to map sensitive information into a service.

### Example

## References:

* https://docs.docker.com/compose/compose-file/

* https://nickjanetakis.com/blog/best-practices-when-it-comes-to-writing-docker-related-files
