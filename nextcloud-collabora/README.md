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
