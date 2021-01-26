# Nextcloud with sidecar image for Collabora
This project uses multiple Compose files. See
[README](https://github.com/osspsuite/docker-compose/blob/master/README.md)
in the root of the repository for working with this structure.

See `docker-compose.override.yml` for the environment variables required
by each service.

## Custom Nextcloud image
A custom image for Nextcloud is built using the files under
`dockerfiles/nextcloud`.
The base entrypoint script is from
https://github.com/nextcloud/docker/blob/master/19.0/apache/entrypoint.sh

`wait-for-it.sh` from
https://github.com/vishnubob/wait-for-it
is used to address what seems to be a race in service startup when
using an external database with Nextcloud.
The call to `wait-for-it.sh` is added within `entrypoint.sh`.

A second customization in `entrypoint.sh` is to install a list of
Nextcloud apps from environment variable, which was taken from
https://github.com/nextcloud/docker/issues/820. If you use this
method to install app `richdocuments` (Collabora Online),
remember to configure `wopi_url` and `public_wopi_url` to match
your configuration.

## Note about client address
With this configuration, Nextcloud will see the Traefik container as the remote
client. It could be related to a regression in Traefik or simply a consequence
of using an overlay network. Unless you find a solution, be careful using any
application level rate limiting or security settings like brute force protection
because they will affect *all* client connections. With this configuration, it
seems like the better place to handle such things is in Traefik middleware.

## Upgrade Nextcloud
```
docker-compose -f docker-compose.yml -f config/fremont2.yml --env-file config/.env.fremont2 exec -u www-data app php occ maintenance:mode --on
docker run --rm -v nextcloud-app_data:/app -v nextcloud-backup:/backup instrumentisto/rsync-ssh rsync -Avax /app/ /backup
docker-compose -f docker-compose.yml -f config/fremont2.yml --env-file config/.env.fremont2 exec -u postgres db psql postgres > nextcloud-backup.psql
docker-compose -f docker-compose.yml -f config/fremont2.yml --env-file config/.env.fremont2 pull
docker-compose -f docker-compose.yml -f config/fremont2.yml --env-file config/.env.fremont2 build
docker-compose -f docker-compose.yml -f config/fremont2.yml --env-file config/.env.fremont2 down
docker-compose -f docker-compose.yml -f config/fremont2.yml --env-file config/.env.fremont2 up -d
docker-compose -f docker-compose.yml -f config/fremont2.yml --env-file config/.env.fremont2 stop app
docker run --rm -v nextcloud-app_data:/app -v nextcloud-backup:/backup instrumentisto/rsync-ssh rsync -Avax /backup/ /app
docker cp nextcloud-backup.pgsql nextcloud-postgres:/backup.sql
docker-compose -f docker-compose.yml -f config/fremont2.yml --env-file config/.env.fremont2 exec -u postgres db dropdb postgres
docker-compose -f docker-compose.yml -f config/fremont2.yml --env-file config/.env.fremont2 exec -u postgres db createdb postgres
docker-compose -f docker-compose.yml -f config/fremont2.yml --env-file config/.env.fremont2 exec -u postgres db bash
  $ psql postgres < /backup.sql
docker-compose -f docker-compose.yml -f config/fremont2.yml --env-file config/.env.fremont2 start app
docker-compose -f docker-compose.yml -f config/fremont2.yml --env-file config/.env.fremont2 exec -u www-data app php occ maintenance:mode --off
```
