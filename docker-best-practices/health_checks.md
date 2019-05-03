# Best practices for health checks

Health checks are commands that run inside a Docker container to indicate the health of the application running within it. These tend to be a one-line script that have an exit code of zero to indicate success or one to indicate failure. Note that additional packages may have to be installed (such as `curl`).

An unhealthy container will be restarted by Docker. See the Dockerfile `HEALTHCHECK` resource listed at the bottom of this document for details on the options available to the `HEALTHCHECK` instruction.

## Health check guidelines

* Health checks should be used whenever possible.
* Health checks should emit a useful error message (less than 4096 character) on failure. This output can be viewed via `docker inspect` for diagnostic purposes.
* Health checks should be specified in an image's Dockerfile.
* Health checks should be specified in any related docker-compose file if the default health check and/or related settings need to be altered.
* Health checks should be stateless whenever possible.
* Health check commands should be specified in the exec form rather than the shell form.
* Example:
```dockerfile
HEALTHCHECK CMD ["/usr/bin/curl", "--fail", "http://localhost"]
```

### Diagnostic(?) considerations

For applications that require more advanced health checks (for example, something that cannot be checked with `curl`), consider implementing a reasonable way to check application health. This could be a shell script or an application-specific language script (Ruby, Python, etc).

## Requirements

* Docker: v1.12+
* docker-compose: v1.9.0+
* docker-compose file version: 2.1+

## Resources
* [Dockerfile Reference: HEALTHCHECK](https://docs.docker.com/engine/reference/builder/#healthcheck)
* [docker-compose healthcheck options](https://docs.docker.com/compose/compose-file/#healthcheck)
