# Postgres e PGAdmin setup com Docker

- [PGAdmin com SSL e Docker](#pgadmin-com-ssl-e-docker)
  - [Requisitos](#requisitos)
  - [Configurações do proxy](#configurações-do-proxy)
  - [Antes de subir o serviço pela primeira vez](#antes-de-subir-o-serviço-pela-primeira-vez)
  - [Levantando os serviços de postgres e pgadmin](#levantando-os-serviços-de-postgres-e-pgadmin)
  - [Logs](#logs)

## Requisitos

Para rodar esse projeto, você deve ter o `docker` e `docker compose` instalados.

## Configurações do proxy

Siga as instruções no repositório abaixo:

https://github.com/LibreCodeCoop/nginx-proxy

## Antes de subir o serviço pela primeira vez

Clone este repositório:
  ```bash
  git clone https://github.com/LibreCodeCoop/postgres-docker.git
  ```
Copy the `.env.example` to `.env` and set the values.
  
  ```bash
  cp .env.example .env
  ```

| Ambiente | serviço | Descrição |
|-------------|---------|-------|
| [`VIRTUAL_HOST`](https://github.com/nginx-proxy/nginx-proxy#usage) | `web` | Seu domínio |
| [`LETSENCRYPT_HOST`](https://github.com/nginx-proxy/docker-letsencrypt-nginx-proxy-companion/blob/master/docs/Basic-usage.md#step-3---proxyed-containers) | `web` | Seu domínio |
| [`LETSENCRYPT_EMAIL`](https://github.com/nginx-proxy/docker-letsencrypt-nginx-proxy-companion/blob/master/docs/Let's-Encrypt-and-ACME.md#contact-address) | `web` | Seu e-mail de administrador de sistema |
| `POSTGRES_PASSWORD` | `postgres` | Senha para o superusuário do banco de dados PostgreSQL. Deve ser alterada do padrão para segurança. |
| `POSTGRES_DB` | `postgres` | Nome do banco de dados padrão que é criado quando a imagem PostgreSQL é iniciada pela primeira vez. |
| `POSTGRES_USER` | `postgres` | Nome de usuário para o superusuário do banco de dados PostgreSQL. |
| `PGADMIN_DEFAULT_EMAIL` | `pgadmin` | Endereço de e-mail usado como nome de usuário de login para a interface da web pgAdmin. |
| `PGADMIN_DEFAULT_PASSWORD` | `pgadmin` | Senha para acessar a interface da web pgAdmin. Deve ser alterada do padrão para segurança. |

> **PS**: O Let's Encrypt só funciona em servidores quando o `VIRTUAL_HOST` e o `LETSENCRYPT_HOST` têm um domínio público válido registrado em um servidor DNS. Não tente usar localhost, não funciona!

Crie as redes necessárias:

  ```bash
  docker network create reverse-proxy
  docker network create postgres
  ```

## Levantando os serviços de postgres e pgadmin

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
Se quiser ver os logs, rode o comando:
  ```bash
  docker compose logs -f --tail=100
  ```

