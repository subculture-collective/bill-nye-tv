# Bill Nye TV

![Bill Nye TV identity](docs/brand/bill-nye-tv/readme-lockup.png)

An independent episode archive and replay channel for *Bill Nye the Science Guy*, styled as a 1990s classroom television. Pick an episode from the guide, or tune in to the shared replay schedule and watch what is already on.

This repository holds the viewer's frontend, design and deployment configuration. The catalog and API come from SUBCULT's shared archive engine, which runs as a pinned container image. The episodes come from an [Internet Archive collection](https://archive.org/details/billnyethescienceguythecompleteseries-07-28-2021); the deployment serves preserved copies of the source MP4 files.

Public site: https://bill-nye.subcult.tv/

## Features

- An episode guide in grid or table view, with metadata filters.
- On-demand playback of any episode.
- A shared replay channel. **Tune in** joins the program at its current position. Your controls never change the channel's schedule.
- A television cabinet with a power button and an optional CRT picture effect. Episodes keep their original picture proportions.
- Theater mode.
- Share links by copying, by email or to Bluesky.
- Favorites saved in your browser, with a separate favorites shelf.
- Original broadcast dates where the source has them; unknown dates are shown as unknown.

Optional sign-in, saved preferences and a channel chat are built into the frontend but switched off in the standard deployment. Anonymous viewing does not need them.

## Quick start

Requirements: Node.js 24 and pnpm 10.33.0. The `@subcult` packages install from the registry set in [`web/.npmrc`](web/.npmrc).

```sh
pnpm --dir web install --frozen-lockfile
pnpm --dir web dev
```

Vite serves the app on port 5173 and proxies `/api` to `http://127.0.0.1:8080`. Set `VHNESS_API` to point it at another archive engine API.

Checks:

```sh
make verify   # engine and viewer pins, tool hashes, frozen install, typecheck, lint, tests, production build
```

## Deploy

[`deploy/core-compose.yml`](deploy/core-compose.yml) runs PostgreSQL, the archive engine API, this frontend and an `operator` profile for migrations and catalog commands. The frontend image is built from [`deploy/frontend.Dockerfile`](deploy/frontend.Dockerfile): nginx serving the static app and proxying `/api` to the engine.

```sh
docker compose --env-file deploy/archive-core.env -f deploy/core-compose.yml up -d
```

Set these before starting:

| Variable | Purpose |
| --- | --- |
| `ARCHIVE_ENGINE_IMAGE` | Engine image, pinned by digest in [`deploy/archive-core.env`](deploy/archive-core.env) |
| `FRONTEND_IMAGE` | Tag of the frontend image you built |
| `COLLECTION` | Engine collection to serve (`bill-nye`) |
| `MEDIA_ROOT` | Folder of preserved episode files, mounted read-only |
| `SECRET_ROOT` | Folder holding the `database-password`, `database-url` and `operator-database-url` files |
| `NPMRC_FILE` | npm configuration used while building the frontend image |
| `WEB_PORT` | Loopback port for the frontend (default 8080) |

Run the operator's `migrate` before the API, use a read-only database role for the public API, and put HTTPS in front of the frontend. [`deploy/community-compose.yml`](deploy/community-compose.yml) is an optional overlay that adds routing for accounts and chat; see [`docs/community.md`](docs/community.md).

## Project layout

```text
web/       React and Vite frontend (built on @subcult/archive-viewer)
deploy/    Compose files, nginx configuration, engine and viewer pins
scripts/   Pin and hash checks, image build and deployment helpers
docs/      Brand assets, asset provenance and deployment notes
```

## Contributing

- Bugs and feature requests: [GitHub issues](https://github.com/subculture-collective/bill-nye-tv/issues).
- Code: pull requests on GitHub against `main`. CI runs `make verify` and builds the frontend image on every pull request. Both must pass.

## License and rights

The software is licensed under GPL-3.0. See [LICENSE](LICENSE).

The episodes and the series logo belong to their owners and are not covered by the software license. [Asset provenance](docs/asset-provenance.json) records where the shipped logo came from. This is an independent archive viewer. It is not made or endorsed by Bill Nye or the series' producers.
