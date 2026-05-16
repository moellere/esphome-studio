# Deployment

Wirestudio can be self-hosted via a single multi-arch Docker image (`linux/amd64` + `linux/arm64`), where FastAPI serves both the API and the SPA from one process.

## Single-Image Deployment (Docker)

```sh
docker run --rm -p 8765:8765 \
  -e ANTHROPIC_API_KEY=sk-ant-... \
  -v wirestudio-data:/data \
  ghcr.io/moellere/wirestudio:v0.10.0
```

Open <http://localhost:8765>. The image bundles the FastAPI server + the built web UI in one process. `/api/*` is the JSON API, and `/` is the SPA. The `/data` volume holds the agent's session log and saved designs across upgrades.

### Available Tags

| Tag | What it tracks |
|---|---|
| `:v0.10.0` / `:0.10.0` / `:0.10` / `:latest` | the v0.10.0 release |
| `:main` | latest commit on `main` (rolling) |
| `:sha-<short>` | a specific commit |

### Optional Environment Variables

All feature-gating environment variables are optional. The studio runs without them; the corresponding features are simply turned off.

| Env var | What it gates |
|---|---|
| `ANTHROPIC_API_KEY` | the agent (`/agent/*` endpoints + the chat sidebar) |
| `FLEET_URL` + `FLEET_TOKEN` | fleet-for-esphome push (`/fleet/*`) |
| `THINGIVERSE_API_KEY` | enclosure search (`/enclosure/search`) |

## Advanced Deployments

For Kubernetes and proxy environments, configuration files are provided in the repository's `deploy/` directory.

- **Kubernetes:** See `deploy/k8s.yaml`.
- **Docker Compose (with Nginx):** See `deploy/README.md`.