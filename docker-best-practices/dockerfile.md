# Best practices for designing Dockerfiles

When possible, non-custom Docker images should be used.  You should create a custom Dockerfile/image when the existing image that will be extended does not yet meet your needs.

## General guidelines

* Use an `-alpine` base image whenever possible.

* Alphabetize additional packages installed in the image, for ease in making pull requests.

* Add a maintainer to keep track of the image maintainer.  

  Example:
  ```bash
  LABEL maintainer="Kate Lynch <katherly@upenn.edu>"
  ```
* Use JSON array syntax for `CMD` commands.

  Example:
  ```bash
  CMD ["/usr/bin/wc","--help"]
  ```
* Use `ENV` for non-sensitive information, and only when necessary.

* Use `COPY` over `ADD` when possible, as `COPY` transparently copies resources into an image.  `ADD` should be used when the resource being added is a local TAR file that needs to be automatically extracted.

* When possible, use `ENTRYPOINT` to specify the image's main command, and if applicable, `CMD` to pass in default flags.

  Example ([source](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/#entrypoint)):
  ```yml
  ENTRYPOINT ["s3cmd"]
  CMD ["--help"]
  ```
### RUN instructions and Alpine package management

Typically you will have a `RUN` instruction in your Dockerfile that installs all required development and runtime dependencies for your image. In Alpine-based images, you use the `apk` tool for this purpose. Here is an example of a typical `RUN` instruction that handles dependency management:

```dockerfile
RUN apk add --no-cache --virtual build-deps build-base ruby-dev ca-certificates && \
    apk add --no-cache ruby ruby-bundler && \
    bundle install && \
    apk del --no-cache build-deps
```

This instruction demonstrates a handful of best practices:

1. Concatenate subsequent commands with &&, resulting on only one additional layer in your final image.
1. Using the `--no-cache` parameter to prevent pulling the latest Alpine package index.
1. Using the `--virtual` parameter to tag a group of packages with a virtual name. This is used to remove build dependencies (via `apk del`) after they are no longer required.

## Deployment

### Image registries

* When possible, host Docker images on [Penn Libraries Quay.io registry](https://quay.io/organization/upennlibraries) rather than building them on the target system.  Work with LTS to add images to this registry.

  Example ([source](https://github.com/upenn-libraries/guardian/blob/master/docker-compose.yml#L39)):
  ```yml
  guardian:
     image: 'quay.io/upennlibraries/guardian:latest'
  ```
* Pull vetted images from Dockerhub or Quay.io.

### Multi-stage builds

Multi-stage builds are useful for creating artifacts that are required by the final image, reducing the final image size and minimizing its number of layers. For example, the first build stage could take place in a container that compiles your application into an executable (such as in Go) and the final build stage could copy the executable into an image based on scratch.

Example ([source](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/#use-multi-stage-builds)):

```dockerfile
FROM golang:1.9.2-alpine3.6 AS build

# Install tools required for project
# Run `docker build --no-cache .` to update dependencies
RUN apk add --no-cache git
RUN go get github.com/golang/dep/cmd/dep

# List project dependencies with Gopkg.toml and Gopkg.lock
# These layers are only re-built when Gopkg files are updated
COPY Gopkg.lock Gopkg.toml /go/src/project/
WORKDIR /go/src/project/
# Install library dependencies
RUN dep ensure -vendor-only

# Copy the entire project and build it
# This layer is rebuilt when a file changes in the project directory
COPY . /go/src/project/
RUN go build -o /bin/project

# This results in a single layer image
FROM scratch
COPY --from=build /bin/project /bin/project
ENTRYPOINT ["/bin/project"]
CMD ["--help"]
```

## Resources
* [Best practices for writing Dockerfiles](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/)
* [Best Practices When It Comes to Writing Docker Related Files](https://nickjanetakis.com/blog/best-practices-when-it-comes-to-writing-docker-related-files#dockerfile) - take this with a grain of salt, as it may not be applicable to your use case.
