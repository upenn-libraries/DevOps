# Best practices for base images

These guidelines refer to image selection for custom Docker images as well as services defined in the deployment.

## Guidelines

* Always use the official base image unless there is a compelling reason not to.
* Use official base images pulled from an official Docker registry such as Hub or Quay.
* Use images where source code is available for review online through a service such as GitHub, unless there is a compelling reason not to.
* When possible, specify a major or minor version release for the version of the image that is pulled.  Do not use `:latest` if possible outside of major base operating system images.
* When available, use images tagged with `*-alpine` or `*-slim`.
* When a service-oriented image is available (example: MySQL Server), do not include this in your image, but integrate it into the deployment using tools such as `docker-compose.yml`.
* Periodically clear partial cached image builds from your local environment using a command such `docker rmi $(docker images -f "dangling=true" -q)`.

## Recommended bases for custom images

* For Rails web applications:
  * `pennlib/passenger-ruby23:*-ruby-build`
* For Python web frameworks:
  * `phusion/passenger-full`
* For Node.js applictions:
  * `phusion/passenger-full`
* For Sinatra applications:
  * `ruby*-alpine` or `ruby*-slim`
* For Ruby scripts:
  * `ruby*-alpine` or `ruby*-slim`
* For PHP web applications:
  * `php:*-apache`
* For Java applications:
  * `tomcat*-alpine` or `tomcat*-slim`
* For base operating systems:
  * Alpine, Ubuntu, CentOs
  * Use `-lts` (long-term support) tagged versions of these images

## Recommended images for non-custom services

* MySQL
  * `mysql/mysql-server`
* Redis
  * `redis:*-alpine`
* RabbitMQ
  * `rabbitmq:*-management`

## Resources

* [Definition of Docker image](https://searchitoperations.techtarget.com/definition/Docker-image)
* Docker core concepts:
![infographic of Docker core concepts](assets/docker_images_graphic.png)
