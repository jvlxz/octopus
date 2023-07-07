# Octopus Project
Octopus is an Ansible-based project to deploy a poll application across multiple machines without using containers. This application consists of a Python Flask web client, a Redis queue, a Java worker, and a Node.js web client that displays the results.

## TABLE OF CONTENTS

  <ol>
    <li>
      <a href="#Getting-Started">Getting Started</a>
    </li>
      <li>
      <a href="#Features">Features</a>
    </li>
      <li>
      <a href="#Environment_Setup">Environment Setup</a>
    </li>
      <li>
      <a href="#Repository_Structure">Repository Structure</a>
    </li>
      <li>
      <a href="#Usage">Usage</a>
    </li>
    <li>
       <a href="#Important_Notes">Important Notes</a>
    </li>
  </ol>


## Getting Started

To get started with the project, make sure you have the following prerequisites:

Ansible installed on your local machine
Access to 5 Debian 10 (Buster) virtual machines
A cloud platform account (optional, but recommended)

## Features

The project includes 6 Ansible roles:

1. base: Configures all machines and installs useful packages.</br>
2. redis: Installs and sets up Redis.</br>
3. postgresql: Installs PostgreSQL 12, psql tool, and creates the necessary user and schema.</br>
4. poll: Uploads poll service, installs dependencies, and runs the poll web client.</br>
5. worker: Uploads worker service, installs dependencies, builds the worker, and runs the worker.</br>
6. result: Uploads result service, installs dependencies, and runs the result web client.</br>

## Environment Setup

Your inventory should have 5 groups, each containing 1 instance: redis, postgres, poll, result, and worker. You will need 5 virtual machines based on Debian 10 (Buster). You can run it locally, but it is recommended to use a cloud platform. Do not spend too much credit, as you may need cloud platforms for future DevOps projects.

## Repository Structure

```css
.
├── playbook.yml
├── poll.tar
├── result.tar
├── worker.tar
├── group_vars
│   └── all.yml
└── roles
    ├── base
    │   └── tasks
    │       └── main.yml
    ├── postgresql
    │   ├── files
    │   │   ├── pg_hba.conf
    │   │   └── schema.sql
    │   └── tasks
    │       └── main.yml
    ├── redis
    │   ├── files
    │   │   └── redis.conf
    │   └── tasks
    │       └── main.yml
    ├── poll
    │   ├── files
    │   │   └── poll.service
    │   └── tasks
    │       └── main.yml
    ├── result
    │   ├── files
    │   │   └── result.service
    │   └── tasks
    │       └── main.yml
    └── worker
        ├── files
        │   └── worker.service
        └── tasks
            └── main.yml
```

## Usage

* Set the ANSIBLE_VAULT_PASSWORD_FILE environment variable and store your vault password: : <br />

```shell
export ANSIBLE_VAULT_PASSWORD_FILE=/tmp/.vault_pass
echo verySecretPassword > /tmp/.vault_pass
```

* Run the Ansible playbook using your own inventory file named production: <br />

```shell
ansible-playbook -i production playbook.yml
```

## Important Notes

* Docker, Podman, and Ansible Galaxy are forbidden for this project.
* Clear-text passwords found in the repository will result in the project being considered as failed. Use Ansible Vault to secure sensitive information.
* All services must be managed by systemd and start automatically on boot.
* Services must be configured with environment variables, such as: host, port, user, password, database name, etc.
* Idempotence should be maintained. After applying the Ansible playbook twice, there should be as few changed tasks as possible in your PLAY RECAP.
* Avoid using the command / shell / raw modules as much as possible. Use dedicated modules like apt_key, apt_repository, or pip. Use the command module only if no dedicated module exists.


<p align="right">(<a href="#top">back to top</a>)</p>



