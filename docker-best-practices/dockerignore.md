# Best practices for dockerignore files

The `.dockerignore` file is used to exclude files and directories from the Docker build context. This prevents unnecessary files from being added to the final image (including files containing secrets and environment-specific configuration) and keeps image sizes small. Another benefit is preventing files from unintentionally being added to your resulting image due to `ADD` or `COPY` instructions on directories.

## `.dockerignore` guidelines

* Use `.dockerignore` files.

* `.dockerignore` files should be placed at the root of the Docker build context.

* You may use the contents of the project's `.gitignore` as a guide (if appropriate) for the `.dockerignore` file. As a general rule, anything necessary to build your Docker image should be in version control somewhere. _TODO: link to `Dockerfile` and/or secrets documentation._

* Exclude any file or directory not required by the container runtime. Common examples are the `.git` directory, `Dockerfile`, `.dockerignore`, `.gitignore`, `README.md`, etc.
**NOTE:** Excluding the `.git` directory is especially important for preventing large image sizes or having an image that grows over time.

## Resources

* [`.dockerignore` reference](https://docs.docker.com/engine/reference/builder/#dockerignore-file)
