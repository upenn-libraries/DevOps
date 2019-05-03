# Best practices for source code management

As a general rule source code should made public to support openness, portability between environments, and adoption by others.

## Location of Dockerfile

It is best practice to store the Dockerfile with the code repository. See the [coding](coding.md) guidelines for a diagram of recommended repository structure.

## Prefer public github repos

Penn Libraries encourages the use of [Quay.io](https://quay.io) registry for Docker images. Quay relies on public git repositories to build and manage images. For this reason, unless it's necessary not to, git repositories should be public.

It is general good practice (and good citizenship) to create public repositories. This encourages the practices recommended in the [coding](coding.md) and [secrets management](secrets_management.md) documents. As recommended in those documents, secrets like passwords and usernames should be configurable and managed outside the code repository.

## Management of deployment specific configuration

Configuration specific to a deployment environment (e.g. IP addresses, performance tuning) might not be relevant to the public repository. Tools and configs like Ansible playbooks can be kept in a separate config repository. This repository can be kept private if appropriate.

Swarm host inventories, environment variables, encrypted secrets, environment-specific Compose files, and other deployment details can be stored in this config repository and supplied at deploy time according to the recommendations in the [coding](coding.md) document.
