# Listmonk Ansible Role

![Listmonk Logo](assets/listmonk.png)


Listmonk is a self-hosted newsletter and mailing list manager. This role helps you to set up Listmonk:

- with everything run in [Docker](https://www.docker.com/) containers
- powered by [the official Listmonk container image](https://hub.docker.com/r/listmonk/listmonk/)


## Installing

To configure and install Listmonk on your own server(s), you should use a playbook like [Mother of all self-hosting](https://github.com/mother-of-all-self-hosting/mash-playbook) or write your own.

# Configuring this role for your playbook

```
listmonk_enabled: true
listmonk_hostname: 'example.org'

listmonk_db_host:

listmonk_db_name:
listmonk_db_user:
listmonk_db_password:
```

