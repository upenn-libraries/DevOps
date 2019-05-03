# Best practices for registries

Docker registries are shared locations for docker image distribution build on the model of GitHub. As such, a docker image can be pushed to a registry and tagged, and then, of course, pulled from the same location. The default registry for an image named in a Dockerfile (e.g. `mysql:latest`) is [Docker Hub](https://hub.docker.com/).

## General guidelines

Whenever possible, create a Docker image of your application and push it to a registry. This speeds up and allows flexible application deployment and version rollback (when needed).

## Which registry? Quay or LTS private?

LTS's preferred Registry is [quay.io](https://quay.io/organization/upennlibraries), which is free for public registries. LTS does not have an account for private registries on Quay.

LTS also provides a [private registry](https://tuka.library.upenn.int) for applications that for some reason cannot or should not be hosted on Quay, but its use is strongly discouraged unless absolutely necessary.

If you're not sure which registry to use or how to prepare an image for a public registry, first, read the other Docker best practices documents, especially those mentioned below, and then consult with an LTS engineer.

## Coding

In general, even when coding for the private registry, code defensively, so that secrets and LTS network-specific dependencies, like paths and server URLs, are not in the code but configurable at runtime.

For more information, see the [coding](coding.md) and [source code management](source_code.md) best practices documents.

## Tagging

Tags enable easy deployment and rollback of images. See the [tagging](tagging.md) best practices document for more information.

## Secrets

Never store secrets in Docker images that will be hosted on a registry. See the [secrets](secrets_management.md) best practices document for more information.
