<!--
SPDX-FileCopyrightText: 2025 Julian-Samuel Gebühr

SPDX-License-Identifier: AGPL-3.0-or-later
-->

# listmonk Ansible Role

![listmonk Logo](assets/listmonk.png)

listmonk is a self-hosted newsletter and mailing list manager. This role helps you to set up listmonk:

- with everything run in [Docker](https://www.docker.com/) containers
- powered by [the official listmonk container image](https://hub.docker.com/r/listmonk/listmonk/)

## Installing

To configure and install listmonk on your own server(s), you should use a playbook like [Mother of all self-hosting](https://github.com/mother-of-all-self-hosting/mash-playbook) or write your own.

# Configuring this role for your playbook

```
listmonk_enabled: true
listmonk_hostname: 'example.com'

listmonk_database_host:
listmonk_database_port: 5432
listmonk_database_name:
listmonk_database_username:
listmonk_database_password:
```

💡 See this [document](docs/configuring-listmonk.md) for details about setting up the service with this role.
