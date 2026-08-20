# uPack-dev/.github

Shared GitHub Actions workflows.

## `dokploy-dev.yml`

Builds a Docker image on push to `dev`, pushes it to GHCR (`:dev`, `:dev-<sha>`)
and pings the repo's `DOKPLOY_WEBHOOK_URL` secret (environment `dev`).

```yaml
jobs:
  deploy:
    uses: uPack-dev/.github/.github/workflows/dokploy-dev.yml@main
    secrets: inherit
    with:
      image: ghcr.io/upack-dev/<name>
      # context: ./backend          # default .
      # dockerfile: ./backend/Dockerfile
      # runner: blacksmith-4vcpu-ubuntu-2404
      # build-args: |
      #   NUXT_STRAPI_URL=${{ vars.NUXT_STRAPI_URL_DEV }}
```
