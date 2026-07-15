# Debug Guide

## Logs

| Submodule | Log Method |
|-----------|-----------|
| product-editor, supplier-onboarding, business-insights, visitors | stdout via winston Console transport |
| user-frontend | HTTP request logs via morgan; access with `docker compose logs -f app` |
| supplier-onboarding, visitors | `docker compose logs -f app` |

## Common Issues

| Symptom | Resolution |
|---------|-----------|
| Private `@visable-dev/*` packages fail to install | Set `GITHUB_TOKEN` and run `pnpm config set '//npm.pkg.github.com/:_authToken' $GITHUB_TOKEN` |
| `product-editor-frontend` has no docker-compose.yml | Use `pnpm dev` directly; no local Docker compose available |
| `business-insights-frontend` docker requires company compose | Set `COMPOSE_FILE` env var pointing to company docker-compose; join VPN |
| `visitors-frontend` local Docker requires VPN | Join VPN; set `COMPOSE_FILE`; use `./dev/start.sh` |
| user-frontend logged-in pages show logged-out | Run `dotenv -e .env.staging -v TEST_USER_PROFILE_ID=<id> yarn dev` |
| business-insights / visitors tests fail without `.nuxt/` | Run build first: `yarn build` / `npm run build` (or `nuxi prepare`) |
| business-insights `yarn test` does nothing | No real test suite configured (placeholder echo command) |
| customer-dashboard HMR/proxy expects `local.europages.de` | Add `/etc/hosts` entry for that hostname |
| supplier-onboarding / product-editor local DNS issues | Add `127.0.0.1 local.wlw.de local-qa1.wlw.de local-qa2.wlw.de` to `/etc/hosts` |
| Debug mode (business-insights / visitors) | Browser console: `Shift+Ctrl+Option+3` then reload page |
| business-insights dependency upgrade breaks build | Check WEB-3772 (webpack v4 compat) before upgrading deps |

## SSR Auth Debugging

- Auth user is injected server-side via `x-forwarded-auth-user-profile` header (staging/prod)
- Locally: set `TEST_USER_PROFILE_ID` in `.env` or use `dotenv -e .env.staging`
- Server middleware `01.auth-proxy.ts` returns 401 if header absent and not in dev mode

## Sentry

- DSN set via `SENTRY_DSN` env var
- Enabled flag: `NUXT_PUBLIC_SENTRY_ENABLED=true`
- Errors viewable in Sentry project per app
