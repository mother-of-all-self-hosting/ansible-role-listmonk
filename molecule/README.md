<!--
SPDX-FileCopyrightText: 2018-2025 Slavi Pantaleev
SPDX-FileCopyrightText: 2019-2022 Aaron Raimist
SPDX-FileCopyrightText: 2019-2023 MDAD project contributors
SPDX-FileCopyrightText: 2023 QEDeD
SPDX-FileCopyrightText: 2024 Fabio Bonelli
SPDX-FileCopyrightText: 2024 Nikita Chernyi
SPDX-FileCopyrightText: 2024-2026 Suguru Hirahara
SPDX-FileCopyrightText: 2026 spatterlight

SPDX-License-Identifier: AGPL-3.0-or-later
-->

# Molecule Testing

This role supports [Molecule](https://docs.ansible.com/projects/molecule/), an Ansible testing framework designed for developing and testing Ansible collections, playbooks, and roles.

## Prerequisites

To utilize Molecule you need to prepare several requirements:

- **x86** computer running one of these operating systems that make use of [systemd](https://systemd.io/):
  - **Archlinux**
  - **CentOS**, **Rocky Linux**, **AlmaLinux**, or possibly other RHEL alternatives (although your mileage may vary)
  - **Debian** (10/Buster or newer)
  - **Ubuntu** (18.04 or newer, although [20.04 may be problematic](https://github.com/mother-of-all-self-hosting/mash-playbook/blob/main/docs/ansible.md#supported-ansible-versions) if you run the Ansible playbook on it)
- `root` access on the computer which Molecule runs against
- [Ansible](http://ansible.com/) program
- [Python](https://www.python.org/)
  - Most distributions install Python by default, but some don't (e.g. Ubuntu 18.04) and require manual installation (something like `apt-get install python3`)
- [Docker](https://www.docker.com)
  - Access to Docker UNIX socket (`/var/run/docker.sock`) is required by default

## Installation

To set up the environment for using Molecule, run the command below on the terminal:

```bash
python3 -m venv ./molecule/venv
source ./molecule/venv/bin/activate
pip3 install -r ./molecule/requirements.txt
```

## Scenarios

Currently there is one testing scenario available.

### `default`

Tests a standard listmonk installation, against a Postgres installed by [ansible-role-postgres](https://github.com/mother-of-all-self-hosting/ansible-role-postgres) and reached over a Unix socket.

The verification does not stop at "the systemd service is active". It:

- establishes, before anything else, that the stock listmonk image cannot start without what the role configures, and that a listmonk given everything *except* the role's `--install` step refuses to serve ("the database does not appear to be setup")
- waits for listmonk's own public page rather than for the unit
- checks that anonymous calls, forged sessions and wrong passwords are all refused
- logs in as an administrator that exists only because `listmonk_environment_variables_additional_variables` reached the container
- asserts the running process reports the version `listmonk_version` pins
- creates a list and a subscriber over the admin API, reads them back, and cross-checks the row in Postgres
- watches the service for long enough to catch a crash loop hiding behind `active`

The scenario deliberately runs listmonk on a port and against a database name that are not its defaults, so that a configuration file which never reached the process would show up as a failure rather than as a pass.

listmonk's SMTP settings are not part of this role - listmonk keeps them in its database, seeded during `--install` - so the scenario does not attempt a mail delivery probe. It would be testing listmonk's own defaults rather than the role.

## Running

By default it is configured to run the scenarios on Ubuntu 26.04.

```bash
molecule test --scenario-name default
```

You can utilize other distributions by setting one to the `MOLECULE_DISTRO` environment variable:

```bash
# Ubuntu 24.04
MOLECULE_DISTRO=ubuntu2404 molecule test --scenario-name default

# Debian 13
MOLECULE_DISTRO=debian13 molecule test --scenario-name default

# Debian 12
MOLECULE_DISTRO=debian12 molecule test --scenario-name default
```
