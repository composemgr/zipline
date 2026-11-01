## 👋 Welcome to zipline 🚀

Self-hosted file sharing for images and videos

## 📋 Description

Zipline is a self-hosted file sharing service built for images and video, with
resumable uploads, expiring share links, and thumbnail generation. This stack
runs the Zipline server with a PostgreSQL backend behind the host nginx proxy.

## 🚀 Services

- **app**: ghcr.io/diced/zipline:latest

### Infrastructure Components

- **zipline-db**: Postgres database

## 📦 Installation

### Option 1: Quick Install
```bash
curl -q -LSsf "https://raw.githubusercontent.com/composemgr/zipline/main/docker-compose.yaml" -o compose.yml
```

### Option 2: Git Clone
```bash
git clone "https://github.com/composemgr/zipline" ~/.local/srv/docker/zipline
cd ~/.local/srv/docker/zipline
cp default.env .env
docker compose --env-file .env up -d
```

### Option 3: Using composemgr
```bash
composemgr install zipline
```

## 🔧 Configuration

### Environment Variables

```shell
TZ=America/New_York
DB_USER_NAME=zipline
DB_USER_PASS=replace-with-a-random-password
APP_SECRET_KEY=replace-with-a-random-32-byte-hex-string
APP_ADMIN_USER=admin
APP_ADMIN_PASS=replace-with-a-random-password
```

See `docker-compose.yaml` for complete list of configurable options.

### Environment Files

`default.env.sample` holds the full set of variables and is copied to `.env`
before starting the stack:

```bash
cp default.env .env
docker compose --env-file .env up -d
```

`app.env.sample` holds the app-specific overrides and is copied to `app.env`:

```bash
cp app.env.sample app.env
```

`composemgr up` applies `app.env` and `default.env` automatically. Raw
`docker compose` commands need `--env-file .env` to pick up the same values.

### Reverse Proxy

Three standalone compose files ship in this repo:

| File | Use |
|------|-----|
| `docker-compose.yaml` | Default, published on `172.17.0.1:64397` |
| `docker-compose.traefik.yaml` | Behind an existing external `traefik` network |
| `docker-compose.tunnel.yaml` | Behind an existing external `cloudflare` tunnel network |

```bash
docker compose -f docker-compose.traefik.yaml --env-file .env up -d
```

The proxy variants default `ZIPLINE_RETURN_RELATIVE_URLS` to `true` so share
links stay relative behind the proxy. Set it back to `false` if you want
absolute URLs.

## 🌐 Access

- **Web Interface**: http://172.17.0.1:64397

## 📂 Volumes

- `./volumes/data/zipline/uploads` - Uploaded files
- `./volumes/data/zipline/public` - Generated thumbnails and public assets
- `./volumes/data/zipline/themes` - Custom themes
- `./volumes/db/postgres/zipline` - Database data

## 🔐 Security

- Set `APP_SECRET_KEY` to a random 32 character or longer value, it encrypts
  share links and sessions
- Change `APP_ADMIN_PASS` before the first start, Zipline creates that user
  when `ZIPLINE_CREATE_USER` is `true`
- Use HTTPS via the reverse proxy in production, uploads and downloads do not
  work over plain HTTP in a useful way
- The database is reachable only on the internal project network
- Restrict who can create anonymous share links
- Regularly update Docker images for security patches
- Backup your data regularly

## 🔍 Logging

```shell
docker compose --env-file .env logs -f app
```

## 🛠️ Management

```bash
# Start services
docker compose --env-file .env up -d

# Stop services
docker compose --env-file .env down

# Update to latest images
docker compose --env-file .env pull && docker compose --env-file .env up -d

# View logs
docker compose --env-file .env logs -f

# Restart services
docker compose --env-file .env restart
```

## 📋 Requirements

- Docker Engine 20.10+
- Docker Compose V2+

## 🤝 Author

🤖 casjay: [Github](https://github.com/casjay) 🤖  
🦄 composemgr: [Github](https://github.com/composemgr) 🦄
