# Ansible Role: ansible_role_gitea_server

![Ansible](https://img.shields.io/badge/ansible-ready-blue.svg)
![Platform](https://img.shields.io/badge/platform-Ubuntu-lightgrey)
![License](https://img.shields.io/badge/license-MIT-green)

## Description

This Ansible role installs and configures **Gitea** using **Docker Compose**.  
It prepares a secure and structured deployment of Gitea and PostgreSQL, suitable for self-hosted Git repositories.  
The role also installs a `systemd` service to manage the Docker Compose lifecycle.

### Features:
- Installs and configures **Gitea** and **PostgreSQL** containers
- Uses **Docker Compose** for service orchestration
- Sets up a **systemd service** for easy management
- Supports external configuration via a secure `.env` file
- Ensures clean separation of config, data, and logs using volumes

---

## Role Variables

| Variable                      | Default Value            | Description                                                                 |
|-------------------------------|--------------------------|-----------------------------------------------------------------------------|
| `gitea_server_path`           | `/opt/gitea`             | Directory where Gitea will be deployed                                      |
| `gitea_server_user`           | `gitea`                    | System user to run Gitea and own files                                      |
| `gitea_server_group`          | `docker`                    | System group to run Gitea                                                   |
| `gitea_server_domain`         | `git.example.com`        | Public domain under which Gitea is reachable                                |
| `gitea_server_network`        | `gitea_network`          | Docker network name used by the containers                                  |
| `gitea_server_docker_image`   | `gitea/gitea:latest`     | Docker image used for the Gitea service                                     |
| `gitea_server_docker_image_postgress` | `postgres:latest`     | Docker image used for the PostgreSQL service                                |
| `gitea_server_timezone`       | `Europe/Berlin`          | Timezone to configure in the containers                                     |

> **Important:**  
> ⚠️ **If you skip this step, the role will fail.**
> Before running this role, you **must** set the PostgreSQL DB password in the `.env` file.
>
> The simplest way to do this:
>
> 1. Copy the provided example file to the target path:  
>    ```bash
>    cp example.env {{ gitea_server_path }}/.env
>    ```
> 2. Manually edit the resulting `{{ gitea_server_path }}/.env` file and set a secure value for the `GITEA_SERVER_DB_PASSWORD` variable.
>
> ---
>
> **Advanced alternatives for production setups:**
>
> - **Ansible Vault:**  
>   Use `ansible-vault` to encrypt a custom `.env` file and store it in your private fork or inventory.  
>   It will gets copied to `{{ gitea_server_path }}/.env` during the playbook execution.
>
> - **Secret Manager Integration:**  
>   In production environments, it's recommended to use a secret manager such as HashiCorp Vault, AWS Secrets Manager, or similar.  
>   Ansible supports dynamic secrets injection via plugins, allowing you to securely retrieve and set `GITEA_SERVER_DB_PASSWORD` at runtime.
>
> These methods provide better security and are especially useful for automated deployments in CI/CD pipelines or other non-interactive environments.

---

## Usage Example

```yaml
- name: Setup Gitea with Docker Compose
  hosts: git
  become: true
  roles:
    - role: gitea_docker_compose
```

---

## Tags

| Tag               | Purpose                                   |
|--------------------|------------------------------------------|
| gitea_user_setup   | Create user and directory structure      |
| gitea_docker_setup | Deploy Docker Compose and network setup  |
| gitea_service_setup| Install and enable systemd unit          |

---

## Sanity Checks

This role ensures:
- Docker Compose files are templated and deployed to `{{ gitea_server_path }}`
- System user and group are set as owners
- A systemd unit is installed for managing Gitea via `systemctl`
- All file paths and permissions are sane and secure

---

## Directory Structure

Follows standard Ansible role layout:
```
gitea_docker_compose/
├── defaults/
│   └── main.yml
├── files/
│   └── example.env
├── handlers/
│   └── main.yml
├── meta/
│   └── main.yml
├── tasks/
│   └── main.yml
├── templates/
│   ├── docker-compose.yml.j2
│   └── gitea-docker.service.j2
```
✔️ Clean, modular, and easy to extend.

---

## License

MIT

---

## Author Information

Role maintained by **Max Bergmann**.  
Feedback, suggestions, and improvements are welcome! 🚀
