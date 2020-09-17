# docker-compose
Various configurations for Docker Compose projects

- [Use Compose in production](https://docs.docker.com/compose/production/)
- [Multiple Compose files](https://docs.docker.com/compose/extends/)

The project structure is intended to align with the links above,
using multiple Compose files per project. While this repository
*can* be checked out in the deployment environment, the intention
is to have the repository checked out locally, for local testing
as well as remote deployment.

See
https://docs.docker.com/engine/security/https/
for information about securely exposing remote Docker daemons, and
https://docs.docker.com/engine/reference/commandline/cli/#environment-variables
for information about configuring your local Docker environment to
communicate securely with remote daemons
(`DOCKER_HOST`, `DOCKER_TLS_VERIFY`, `DOCKER_CERT_PATH`, etc).

Once secure access to remote daemons is in place, a *local* instance
of something like
https://www.portainer.io/
can be configured for access to both local and remote Docker daemons.
The project in `portainer-traefik` is for running a *remote* instance
of Portainer, with TLS terminated by Traefik.

## Project structure
The projects ending with *-traefik* in the name do not align with this structure
and are being phased out.

- `docker-compose.yml` is the basic or canonical definition of services
- `docker-compose.override.yml` is merged automatically by `docker-compose`
  for local development and testing
- YAML files in `config/` is a convention for this repository to store
  the Docker Compose details for remote deployments

Local development example: (assumes `DOCKER_HOST` is local)
```
docker-compose up -d
```
will by default load `docker-compose.yml` and merge `docker-compose.override.yml`
if present.

Remote deployment example: (assumes `DOCKER_HOST` is remote)
```
docker-compose -f docker-compose.yml -f config/prod-eu-2.yml --env-file config/.env.eu-2 up -d
```
*will not* merge `docker-compose.override.yml` even if present. Note the
file chaining here. The default file `docker-compose.yml` is given
explicitly, followed by another non-default file.

`docker-compose config` can help catch missing variables, but note
the caveat below:
```
$ docker-compose config --quiet
WARNING: The ACME_EMAIL variable is not set. Defaulting to a blank string.
WARNING: The TRAEFIK_API_CREDENTIALS variable is not set. Defaulting to a blank string.
WARNING: The TRAEFIK_API_FQDN variable is not set. Defaulting to a blank string.
```

**Note:** Using `docker-compose --file path/docker-compose.yml config`
to check for unset variables in a different project directory will cause
`docker-compose` to look for file `.env` in the current working directory,
not the project directory as expected. See osspsuite/docker-compose#1 and
docker/compose#7600.

## To do
- [ ] A shell script or some aliases would help with the long command
      lines like in the remote deployment example above
