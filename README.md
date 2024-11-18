# Postgres and PGAdmin setup with Docker

- [PGAdmin with SSL and Docker](#pgadmin-with-ssl-and-docker)
  - [Setup of docker](#setup-of-docker)
  - [Setup of proxy](#setup-of-proxy)
  - [Before first run](#before-first-run)
  - [Run postgres and pgadmin](#run-postgres-and-pgadmin)
  - [Logs](#logs)

## Setup of docker

You need to have, on your server, the installed docker. The installation can be done with an official script, following the following steps:
- Download the docker
```bash
curl -fsSL https://get.docker.com -o get-docker.sh
```
- run the script
```bash
sh get-docker.sh
```
- Give permissions to execute the Docker command to your user
```bash
sudo usermod -aG docker $USER
```
- Remove the installation script
```bash
rm get-docker.sh
```

## Setup of proxy

Follow the instructions of this repository:

https://github.com/LibreCodeCoop/nginx-proxy

## Before first run

Copy the `.env.example` to `.env` and set the values.

```bash
cp .env.example .env
```

| Environment | service | Description |
|-------------|---------|-------|
| [`VIRTUAL_HOST`](https://github.com/nginx-proxy/nginx-proxy#usage) | `web` | Your domain |
| [`LETSENCRYPT_HOST`](https://github.com/nginx-proxy/docker-letsencrypt-nginx-proxy-companion/blob/master/docs/Basic-usage.md#step-3---proxyed-containers) | `web` | Your domain |
| [`LETSENCRYPT_EMAIL`](https://github.com/nginx-proxy/docker-letsencrypt-nginx-proxy-companion/blob/master/docs/Let's-Encrypt-and-ACME.md#contact-address) | `web` | Your sysadmin email |
| `POSTGRES_PASSWORD` | `postgres` | Password for the PostgreSQL database superuser. Should be changed from default for security. |
| `POSTGRES_DB` | `postgres` | Name of the default database that is created when the PostgreSQL image is first started. |
| `POSTGRES_USER` | `postgres` | Username for the PostgreSQL database superuser. |
| `PGADMIN_DEFAULT_EMAIL` | `pgadmin` | Email address used as the login username for the pgAdmin web interface. |
| `PGADMIN_DEFAULT_PASSWORD` | `pgadmin` | Password for accessing the pgAdmin web interface. Should be changed from default for security. |


> **PS**: Let's Encrypt only work in servers when the `VIRTUAL_HOST` and `LETSENCRYPT_HOST` have a valid public domain registered in a DNS server. Don't try to use in localhost, don't work!

Create a network 

```bash
docker network create reverse-proxy
docker network create postgres
```

## Run postgres and pgadmin

* Crie uma pasta para o postgres e acesse ela depois:
  ```bash
  mkdir -p ~/projects/postgres
  ```
* Crie o arquivo `~/projects/postgres/compose.yml` com o conteúdo do arquivo `compose.yml`:
* Levante os serviços:
```bash
docker compose up -d

```

## Logs

If you want to see the logs, run:

```bash
docker compose logs -f --tail=100
```

