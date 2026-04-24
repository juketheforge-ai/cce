# Miku Miku Beam

Network stress testing tool with a Miku-themed React frontend and a Go backend that exposes a REST + Socket.IO API. Single binary serves both the API and the static frontend.

## Stack
- **Backend**: Go 1.24 (Echo + zishang520 socket.io)
- **Frontend**: React 18 + Vite 5 + Tailwind, built into static assets
- **Single port**: server listens on port 5000 and serves the built frontend, the REST endpoints (`/attacks`, `/configuration`), and the Socket.IO endpoint (`/socket.io/*`).

## Layout
- `cmd/mmb-server/` — main web/api server entrypoint
- `cmd/mmb-cli/` — standalone CLI binary
- `internal/` — engine, attack workers, config, proxy/netutil
- `pkg/api`, `pkg/target` — shared types/parsing
- `web-client/` — React frontend; `npm run build` outputs `web-client/dist/public/`
- `bin/web-client/` — static assets the server serves at runtime
- `data/proxies.txt`, `data/uas.txt` — runtime input lists

## Replit setup
- Workflow `Start application` runs `PORT=5000 ALLOW_NO_PROXY=true ./bin/mmb-server` on port 5000 (webview).
- Server reads two env vars added for hosting:
  - `PORT` — overrides default port
  - `ALLOWED_ORIGIN` — overrides CORS allow origin
- `ALLOW_NO_PROXY=true` lets attacks run without entries in `data/proxies.txt`.
- Build steps:
  1. `cd web-client && npm install && npm run build`
  2. Copy `web-client/dist/public/*` into `bin/web-client/`
  3. `go build -o bin/mmb-server ./cmd/mmb-server`

## Deployment
- Target: `vm` (the server keeps long-lived Socket.IO connections and per-client attack state in memory).
- Build/run commands are configured in `.replit` via the deploy config.
