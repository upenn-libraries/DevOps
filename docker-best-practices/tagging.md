## Best practices for image tagging

Image tagging is beneficial in Docker as it prevents unexpectedly deploying a breaking change and it allows for fast rollback of new deployments.

### Selecting tags

* When selecting base images, rely on versions of images to something other than `:latest`, because this tag has no expectations of stability or standardized meaning. Ideally, a major or minor `:alpine` release.

### Tagging images

* Do not rely on `:latest` in your custom images, since latest represents a constantly changing image that cannot be version-locked. Since the `:latest` tag is always moving, you cannot use it to rollback to a previous state.
* Build a `:*-stable` tagged image of the previous deployed stable image before cutting a new release.  This will make rollback an issue of changing the tag version in docker-compose, not rebuilding the image.  Example: `:1.1.0-stable`
* Employ some uniform system of tagging your images.  See resources below for examples.

### Resources

* [Docker Tagging: Best practices for tagging and versioning docker images](https://blogs.msdn.microsoft.com/stevelasker/2018/03/01/docker-tagging-best-practices-for-tagging-and-versioning-docker-images/)
* [Semantic Versioning](https://semver.org/)
