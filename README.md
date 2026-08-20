# uPack-dev/.github

Shared GitHub Actions workflows. Both deploy to adm.tools hosting and expect in
the calling repo (environment `production`): secrets `SERVER_SSH_KEY`,
`SERVER_HOST`, `SERVER_USER`, `ADM_TOOLS_KEY` and variable `HOST_ID` (or pass `host-id` per app).

## `nuxt-adm-deploy.yml`

Lint → build → rsync `.output/` into `<target>/.output` → write a start-script
`package.json` into the target root → reload Node.

```yaml
jobs:
  deploy:
    uses: uPack-dev/.github/.github/workflows/nuxt-adm-deploy.yml@main
    secrets: inherit
    with:
      target: site.com/www
      # working-directory: ./frontend
      build-env: |
        NUXT_STRAPI_URL=${{ vars.NUXT_STRAPI_URL }}
```

`secrets` cannot be used inside `with:`; instead every inherited secret is
exported as an env var of the same name during the build.

## `strapi-adm-deploy.yml`

Build admin → write `.env` → rsync project (without `node_modules`,
`public/uploads`, `.tmp`) → install prod deps on the server → reload Node.

```yaml
jobs:
  deploy:
    uses: uPack-dev/.github/.github/workflows/strapi-adm-deploy.yml@main
    secrets: inherit
    with:
      target: site.com/strapi/
      # working-directory: ./backend
      # package-manager: pnpm          # default yarn
      dotenv: |
        HOST=0.0.0.0
        APP_KEYS=$APP_KEYS        # secrets by name, substituted with envsubst
        DATABASE_NAME=${{ vars.DATABASE_NAME }}
```
