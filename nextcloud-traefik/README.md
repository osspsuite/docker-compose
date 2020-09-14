# Basics
The docker compose file uses [environment variables](https://docs.docker.com/compose/environment-variables/).
One of the ways to set these is to put the following content in a file called `.env` in the same directory
as `docker-compose.yml`.

```
DATABASE_NAME=postgres
DATABASE_PASSWORD=my-password
DATABASE_USER=postgres
NEXTCLOUD_ADMIN_PASSWORD=my-admin
NEXTCLOUD_ADMIN_USER=my-admin-password
NEXTCLOUD_PUBLIC_FQDN=cloud.example.com
NEXTCLOUD_TRUSTED_DOMAINS=cloud.example.com
```

# Traefik
This project assumes you have the traefik docker provider configured elsewhere.
