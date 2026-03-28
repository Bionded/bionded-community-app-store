# Bionded app store Community App Store for Umbrel

A community app store for [umbrelOS](https://umbrel.com) containing curated self-hosted applications.

## Apps

### Koel

[Koel](https://koel.dev) is a personal music streaming server with a clean web interface built with Vue and Laravel.

## Adding this store to your Umbrel

1. Open your umbrelOS dashboard.
2. Go to **App Store** → **Community App Stores** (or the three-dot menu).
3. Enter this repository URL: `https://github.com/<your-username>/koel-community-app-store`
4. Click **Add**. The store will appear and you can install Koel from it.

## Koel usage

### Where to put music files

Music is stored at the Koel app data directory on your Umbrel:

```
/home/umbrel/umbrel/app-data/bionded-store-koel/music/
```

Copy or mount your music files there. Koel will scan `/music` inside the container.

### Default login credentials

| Field    | Value             |
|----------|-------------------|
| Email    | `admin@koel.dev`  |
| Password | `KoelIsCool`      |

**Change the default password immediately** after first login via the web UI (click avatar → Settings) or run:

```bash
docker exec -it bionded-store-koel_web_1 php artisan koel:admin:change-password
```

### Scanning the music library

Koel has a built-in daily scanner. To trigger an immediate rescan:

```bash
docker exec -it bionded-store-koel_web_1 php artisan koel:scan
```

### APP_KEY rotation

The app ships with a fixed `APP_KEY` for reliability across restarts. To rotate it:

```bash
docker exec -it bionded-store-koel_web_1 php artisan key:generate --show
```

Then update the `APP_KEY` value in the docker-compose.yml and restart the app from Umbrel.

## Persistence

All state is stored under `${APP_DATA_DIR}`:

| Path             | Contents                          |
|------------------|-----------------------------------|
| `db/`            | MariaDB data files                |
| `music/`         | Your music library                |
| `covers/`        | Album art and uploaded images     |
| `search-indexes/`| Full-text search indexes          |

Uninstalling the app from Umbrel will remove this data. Back up `music/` and `db/` if needed.

## Limitations

- Gallery screenshots in the app manifest link to koel.dev and may change; replace with local assets if needed.
- The `APP_KEY` is static by default. See rotation instructions above.
- HTTPS is handled by Umbrel's reverse proxy; `FORCE_HTTPS` is set to `false` inside the container.
