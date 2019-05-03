# Best practices for secrets management

* [Secrets management guidelines](#secrets)
* [Requirements](#requirements)
* [What are Docker secrets?](#what)
* [Ansible Vault](#ansible)
* [Guidelines](#guidelines)
* [Common Solutions](#solutions)

## <a name="secrets"></a> Secrets management guidelines

* Secrets should be stored in version control.
  * Not storing secrets in version control means that they're either sitting on a production server, in somebody's head, or in some other surprising place. This increases the risk of somebody accidentally giving access to a secret. Not having secrets managed in a controlled way can also complicate automated deployment.
* Secrets should not be stored unencrypted.
  * Storing secrets unencrypted in version control, public or private, is strongly discouraged. Relying on the privacy of a repository to protect secrets doesn't do much for access control and makes it easier for people to make an honest mistake and leak a secret. Storing secrets unencrypted on a server with restricted permissions, while common in practice, can be made less risky with encryption.
* Secrets should not be made available as environment variables.
  * Environment variables are easy to leak in logs and stack traces and are visible within commands like `docker inspect`.

## <a name="requirements"></a> Requirements

* Docker 1.13+
* Docker running in [Swarm Mode](https://docs.docker.com/engine/swarm/)
* [Compose file](https://docs.docker.com/compose/compose-file/) version 3.x+

## <a name="what"></a> What are Docker secrets?

[Docker secrets](https://docs.docker.com/engine/swarm/secrets/) should be used to manage sensitive data such as passwords, SSL certs, and API credentials.  Secrets should be used to transmit information for Docker services when information should not be stored unencrypted in any code or configuration files.

## <a name="ansible"></a> Ansible Vault

[Ansible Vault](https://docs.ansible.com/ansible/2.6/user_guide/vault.html) handles encryption of files. Files encrypted with Ansible Vault can and should be kept in version control.

```bash
# Creating
ansible-vault encrypt --vault-id=./vault_passwd.py secret.yml

# Editing
ansible-vault edit --vault-id=./vault_passwd.py secret.yml

# Deploying
ansible-playbook -i production --vault-id=./vault_passwd.py site.yml
```

Where `./vault_passwd.py` will retrieve the vault password from an environment variable. This pattern is used on our Keystone server. `--vault-id` can also take a file containing the password as an argument.

## <a name="guidelines"></a> Guidelines

For storing and distributing secrets, a vault-based encryption strategy is strongly recommended.  This requires sensitive information to be configured into a file that is referenced in an [Ansible playbook](https://docs.ansible.com/ansible/latest/user_guide/playbooks_vault.html) that is deployed from Keystone.  Examples of sensitive information to be handled with secrets include passwords, SSH private keys, and API credentials.  Consult LTS for guidance and assistance with this.

Non-sensitive information that should not be hard-coded into the application should be handled as an environment variable or using [docker configs](https://docs.docker.com/engine/swarm/configs/).

## <a name="solutions"></a> Common Solutions

### Creating Docker secrets with Ansible

Ansible Vault is recommended for storing secrets in version control. If you create the secret externally with Ansible, you can avoid keeping the secrets file unencrypted on the target server or in version control.

```yaml
# tasks/main.yml

# With Vaulted variables files included in play from `vars` directory
# File should contain entries for `db_password` and `db_root_password`
- name: create docker secrets from Vault variables
  shell: >
    docker secret inspect {{ item }} ||
    echo '{{ hostvars[inventory_hostname][item] }}' |
    docker secret create {{ item }} -
  with_items:
    - db_password
    - db_root_password
  changed_when: false
  no_log: true

# With Vaulted ssh_private_key file in `files` directory
- name: create docker secrets from individual Vault files
  shell: >
    docker secret inspect {{ item }} ||
    echo '{{ lookup("file", item) }}' |
    docker secret create {{ item }} -
  with_items:
    - ssh_private_key
  changed_when: false
  no_log: true
```

### Using Docker secrets in images that rely on config files

A common pattern in popular Docker images, such as the official MySQL image, is to allow specifying a path to a secret file as an environment variable. The entrypoint scripts or config files will usually accept a variable like MYSQL_PASSWORD_FILE and reference that path instead of an environment variable containing the actual value of the secret. This approach is recommended for custom Docker images that support secrets, as well.

```yaml
# docker-compose.yml
version: '3'

services:
  db:
    image: mysql
    environment:
      MYSQL_DATABASE: app
      MYSQL_PASSWORD_FILE: /run/secrets/db_password
      MYSQL_ROOT_PASSWORD_FILE: /run/secrets/db_root_password
      MYSQL_USER: app_user
    secrets:
      - db_password
      - db_root_password

secrets:
  db_password:
    external: true
  db_root_password:
    external: true
```

### Embedding Docker secrets within config files

If you're working with a large file that contains a few secret values, and the config file does not support specifying a path to a secret file containing the value, encrypting the entire file could make frequent updates difficult. Instead, consider encrypting the secret values as Ansible Vault variables or files, templating their values into the unecrypted config file with Ansible at deployment, and then providing the entire config as a Docker secret. Using this approach allows you to easily edit the majority of the config file at rest while keeping the actual secrets encrypted at rest and in transit.

```yaml
# tasks/main.yml

# With sensitive_config_file.j2 file in `templates` directory
- name: create docker secrets from templated file containing Vault secrets
  shell: >
    docker secret inspect {{ item }} ||
    echo '{{ lookup("template", item + '.j2') }}' |
    docker secret create {{ item }} -
  with_items:
    - sensitive_config_file
  changed_when: false
  no_log: true
```
