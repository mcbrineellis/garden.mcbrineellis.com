---
created: 2024-08-21T12:36
updated: 2024-08-21T13:24
---
The `env_vars/.env_web` file contains the customizations for the web front end such as timezone and instance name.

We need to keep this customized file when updating:
- Let's make a backup of `env_vars/.env_web`
- Replace the customized version with the original from the repo
- Then we'll pull the latest changes with `git pull`
- Finally we can restore our customized file to the same location

```
connor@zabbix:~/zabbix-docker$ sudo docker compose -f ./docker-compose_v3_alpine_pgsql_latest.yaml down
connor@zabbix:~/zabbix-docker$ cp env_vars/.env_web ../
connor@zabbix:~/zabbix-docker$ git restore env_vars/.env_web
connor@zabbix:~/zabbix-docker$ git pull
connor@zabbix:~/zabbix-docker$ git checkout 7.0.3
Note: switching to '7.0.3'.
connor@zabbix:~/zabbix-docker$ cp ../.env_web ./env_vars/.env_web
connor@zabbix:~/zabbix-docker$ sudo docker compose -f ./docker-compose_v3_alpine_pgsql_latest.yaml up -d
```

Hmm, I switched to the 7.0.3 tag... but that didn't seem to do anything.
Let's look at the file I'm using (`docker-compose_v3_alpine_pgsql_latest.yaml`):

`${ZABBIX_SERVER_PGSQL_IMAGE}:${ZABBIX_ALPINE_IMAGE_TAG}${ZABBIX_IMAGE_TAG_POSTFIX}`

The values for those are in `.env` and will resolve as follows:
`zabbix/zabbix-server-pgsql:alpine-7.0-latest`

