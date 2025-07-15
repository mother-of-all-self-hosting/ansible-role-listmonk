<!--
SPDX-FileCopyrightText: 2020 - 2024 MDAD project contributors
SPDX-FileCopyrightText: 2020 - 2024 Slavi Pantaleev
SPDX-FileCopyrightText: 2020 Aaron Raimist
SPDX-FileCopyrightText: 2020 Chris van Dijk
SPDX-FileCopyrightText: 2020 Dominik Zajac
SPDX-FileCopyrightText: 2020 Mickaël Cornière
SPDX-FileCopyrightText: 2022 François Darveau
SPDX-FileCopyrightText: 2022 Julian Foad
SPDX-FileCopyrightText: 2022 Warren Bailey
SPDX-FileCopyrightText: 2023 Antonis Christofides
SPDX-FileCopyrightText: 2023 Felix Stupp
SPDX-FileCopyrightText: 2023 Pierre 'McFly' Marty
SPDX-FileCopyrightText: 2024 - 2025 Suguru Hirahara

SPDX-License-Identifier: AGPL-3.0-or-later
-->

# Setting up listmonk

This is an [Ansible](https://www.ansible.com/) role which installs [listmonk](https://listmonk.app/) to run as a [Docker](https://www.docker.com/) container wrapped in a systemd service.

listmonk is a self-hosted, high performance one-way mailing list and newsletter manager.

See the project's [documentation](https://listmonk.app/docs/) to learn what listmonk does and why it might be useful to you.

## Prerequisites

To run a listmonk instance it is necessary to prepare a [Postgres](https://www.postgresql.org) database server.

If you are looking for an Ansible role for it, you can check out [this role (ansible-role-postgres)](https://github.com/mother-of-all-self-hosting/ansible-role-postgres) maintained by the [Mother-of-All-Self-Hosting (MASH)](https://github.com/mother-of-all-self-hosting) team.

## Adjusting the playbook configuration

To enable listmonk with this role, add the following configuration to your `vars.yml` file.

**Note**: the path should be something like `inventory/host_vars/mash.example.com/vars.yml` if you use the [MASH Ansible playbook](https://github.com/mother-of-all-self-hosting/mash-playbook).

```yaml
########################################################################
#                                                                      #
# listmonk                                                             #
#                                                                      #
########################################################################

listmonk_enabled: true

########################################################################
#                                                                      #
# /listmonk                                                            #
#                                                                      #
########################################################################
```

### Set the hostname

To enable the listmonk instance you need to set the hostname as well. To do so, add the following configuration to your `vars.yml` file. Make sure to replace `example.com` with your own value.

```yaml
listmonk_hostname: "example.com"
```

After adjusting the hostname, make sure to adjust your DNS records to point the domain to your server.

**Note**: hosting listmonk under a subpath does not seem to be possible due to listmonk's technical limitations.

### Set variables for connecting to a Postgres database server

To have the listmonk instance connect to your Postgres server, add the following configuration to your `vars.yml` file.

```yaml
listmonk_database_hostname: YOUR_POSTGRES_SERVER_HOSTNAME_HERE
listmonk_database_port: 5432
listmonk_database_name: YOUR_POSTGRES_SERVER_DATABASE_NAME_HERE
listmonk_database_username: YOUR_POSTGRES_SERVER_USERNAME_HERE
listmonk_database_password: YOUR_POSTGRES_SERVER_PASSWORD_HERE
```

Make sure to replace values for variables with yours.

### Extending the configuration

There are some additional things you may wish to configure about the component.

Take a look at:

- [`defaults/main.yml`](../defaults/main.yml) for some variables that you can customize via your `vars.yml` file. You can override settings (even those that don't have dedicated playbook variables) using the `listmonk_environment_variables_additional_variables` variable

See its [environment variables](https://listmonk.app/docs/configuration/#environment-variables) for a complete list of listmonk's config options that you could put in `listmonk_environment_variables_additional_variables`.

## Installing

After configuring the playbook, run the installation command of your playbook as below:

```sh
ansible-playbook -i inventory/hosts setup.yml --tags=setup-all,start
```

If you use the MASH playbook, the shortcut commands with the [`just` program](https://github.com/mother-of-all-self-hosting/mash-playbook/blob/main/docs/just.md) are also available: `just install-all` or `just setup-all`

## Usage

After running the command for installation, listmonk becomes available at the specified hostname like `https://example.com`.

## Troubleshooting

### Check the service's logs

You can find the logs in [systemd-journald](https://www.freedesktop.org/software/systemd/man/systemd-journald.service.html) by logging in to the server with SSH and running `journalctl -fu listmonk` (or how you/your playbook named the service, e.g. `mash-listmonk`).
