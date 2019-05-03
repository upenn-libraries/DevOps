# README for `docker-best-practices`

This repository contains documentation on best practices for Docker images and deployments.  It is maintained by the [BPG DevOps working group](https://confluence.library.upenn.edu/display/LTBPG/DevOps+Working+Group) (currently a link to confluence, to be updated).

## Table of contents
in alphabetical order
* [Base images](base_images.md)
* [Coding](coding.md)
* [docker-compose](docker_compose.md)
* [Dockerfile](dockerfile.md)
* [.dockerignore](dockerignore.md)
* [Health checks](health_checks.md)
* [Registries](registries.md)
* [Secrets management](secrets_management.md)
* [Source code management](source_code.md)
* [Swarm](swarm.md)
* [Tagging](tagging.md)

## Levels of Compliance

Best practices should be adhered to as much as possible when building/using Docker images.  However, we recognize that all of the recommendations in these documents may not be feasible for all Dockerized services.  We have developed tiers of compliance, specifying what **must** be done, what **should** be done if possible, and what is **nice to have** if possible.  Further detail is available in [Levels of compliance](levels_of_compliance.md).

## License

This code is available as open source under the terms of the [Apache 2.0 License](https://opensource.org/licenses/Apache-2.0).
