# Best practices for coding for Docker

Docker takes a lot of guesswork out of coding for smooth deployments, however there are some principles and style guidelines developers can follow for easier Dockerizing.

## General guidelines

Where possible make your code generalizable and configurable. Values likes secrets, paths, URLs to external services, including things like the database, web server, and caches should be configurable and picked up runtime.

## Logging

Meaningful behavior in the software should always be logged.  Logging to `STDOUT` for non-error messages and `STDERR` for error messages is preferred, as messages transmitted this way can be monitored and aggregated with other tools in LTS.

## Secrets management — swarm & not

Don't put secrets -- values like passwords, keys, and other local network information -- in your code or Docker. See the [secrets management page](secrets_management.md) for more information.

NOTE: If your application contains secrets, it is strongly recommended that you use Docker swarm for your deployment, as secrets and docker configs are only available to applications managed this way.  Docker swarm favors stateless applications, however stateful applications can be managed on a single node for easier deployment.

## Internal dependencies

Because our registry, Docker Quay, is public, any reliance on LTS-specific services should be avoided or made configurable. For example, Ruby gems only available via the TUKA gem server or that rely on `git clone` behavior should be avoided. Services that rely in `.int` domain services should be configurable. 

## Forms of access — HTTP, SSH, etc.

Because the Docker network imposes its own names for services and adds at least one network layer between your application and remote users, dependence on IP address or domain name should be avoided within your application. App internal URLs should be handled as much as possible as relative paths. If there is need for the application to know its public URL, as may be necessary for emails containing links sent to user, those values should be configurable or other means used to determine the public-facing web address at runtime. 

Access to services within a Docker container are mediated by the container. For example, log access will show client IP addresses as that of the docker container. If the IP address of the client is needed for logging, this can sometimes be handled with header manipulation in nginx or Apache.

Note that load balancers, like NetScaler, can add another layer between the Docker network and the public. 

## Documentation - of docker config

Software documentation should be kept up to date, with a section on populating example configuration files, and a section on deployment.  Docker requirements such as `docker-compose` and Docker swarm should also be explicitly documented.

## File structure

When you begin Dockerizing, it is advisable to structure your files so that each custom image's code in the deployment is in its own image-named directory under a directory called `images/` at the top level of the git repository.  Deployment files for the entire application, such as `docker-compose.yml`, should be stored at the top level.  Any pre-deployment build commands necessary should be documented.

Example file structure ([source](https://github.com/upenn-libraries/omeka_classic)):
```bash
omeka_classic/
├── .dockerignore
├── README.md
├── docker-compose.yml
└── images
    ├── omeka_classic
    │   ├── Dockerfile
    │   ├── db.ini
    │   ├── db.ini.example
    │   ├── entrypoint.sh
    │   └── files
    │       ├── ini
    │       │   └── 2.6
    │       │       ├── .htaccess.example
    │       │       ├── config.ini.example
    │       │       └── php.ini.example
    │       ├── plugins
    │       │   └── 2.6
    │       │       ├── AdminImages.zip
    │       │       ├── Annotator.zip
    │       │       ├── BlogShortcode.zip
    │       │       ├── Blogger.zip
    │       │       ├── BulkMetadataEditor.zip
    │       │       ├── CSSEditor-1.0.1.zip
    │       │       ├── CollectionTree-2.1.zip
    │       │       ├── Commenting-2.2.zip
    │       │       ├── CsvExport.zip
    │       └── themes
    │           └── 2.6
    │               ├── default.zip
    │               ├── upennlib_default.zip
    │               ├── upennlib_default_shadowpage.zip
    │               ├── upennlib_emiglio.zip
    │               └── upennlib_emiglio_shadowpage.zip
    └── omeka_db
        ├── .env
        ├── .env.example
        ├── Dockerfile
        └── entrypoint.sh
```

## Resources

* [Don't embed configuration or secrets in Docker images](https://medium.com/@mccode/dont-embed-configuration-or-secrets-in-docker-images-7b2e0f916fdd)
* [Project Structure With Multiple Dockerfiles and Docker Compose](https://nickjanetakis.com/blog/docker-tip-10-project-structure-with-multiple-dockerfiles-and-docker-compose)
