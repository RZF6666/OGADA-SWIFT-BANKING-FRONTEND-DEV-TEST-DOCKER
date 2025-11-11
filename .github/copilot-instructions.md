## Quick orientation for AI coding agents

This repository is a small demo that packages a frontend app as a Docker image and exposes it via a single docker-compose service. Use the files below to understand how to run and test locally.

- Key files:
  - `docker-compose.yaml` — single service `web-api-prod` using image `chineduogada/ogada-swift-banking-frontend:1`.
  - `README.md` — run steps and context (mentions React/Node/Express/Postgres).
  - `image.png`, `image-1.png` — screenshot assets showing the UI.

## Big-picture architecture (what you'll see here)
- This repo does not include application source code. It contains a prebuilt Docker image and a compose file that runs it on port 80.
- The `web-api-prod` service maps host port 80 to container port 80 and sets `NODE_ENV=production` while using `entrypoint: yarn dev`.
  - Note the mismatch: `yarn dev` is usually a development server, but the service is configured as production. Treat this as a discovered quirk and call it out when proposing changes.

## Developer workflows (explicit commands and shortcuts)
- Start the app locally (recommended):
  - Use Docker Desktop and run `docker-compose up` from repo root (as shown in `README.md`).
  - Open http://localhost in your browser.
- Pull the published image: `docker pull chineduogada/ogada-swift-banking-frontend:1`.
- Stop: `docker-compose down` or Ctrl+C in the terminal running `docker-compose up`.
- Debug running container:
  - View logs: `docker-compose logs web-api-prod`.
  - Run a shell in the container (if image includes shell): `docker exec -it <container_id_or_name> sh`.

## Project-specific patterns and important notes for changes
- There is no `Dockerfile`, `package.json`, or source tree in this repo — editing runtime behavior here usually requires rebuilding the image in the upstream/source repository.
- README references PostgreSQL and backend technologies (Express, Postgres) but `docker-compose.yaml` does not define a database service — this suggests the running demo connects to external services baked into the image or uses mocked/stubbed data.
- The single service is named `web-api-prod` (misleading — it looks like a frontend image). Prefer conservative, explicit language in commit messages and PR descriptions (explain whether you're changing compose vs. changing source image).

## When asked to modify code
- Before modifying code, check whether the source is present in this repo. If it isn't (as here), ask the user where the source/Dockerfile lives or whether they want composition-only changes (compose config, env overrides).
- For compose-only edits (port, env, entrypoint), update `docker-compose.yaml` and include testing steps in the PR description (how you started it locally and what you observed).

## Quick examples (copyable guidance)
- Start locally and stream logs:
  - `docker-compose up --build` (if you provide a Dockerfile or want to force image rebuild locally)
  - `docker-compose logs -f web-api-prod`
- Inspect the running container:
  - `docker ps` then `docker exec -it <name> sh`

## Files to reference when performing edits
- `docker-compose.yaml` — runtime wiring for the demo.
- `README.md` — describes how maintainers expect the demo to be run.

If anything here is unclear or if you want the agent to perform edits (compose changes vs. source changes vs. rebuilding images), tell me where the source or Dockerfile lives and I will proceed. 
