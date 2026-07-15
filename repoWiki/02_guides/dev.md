# Development Guide

## Prerequisites

- GitHub PAT with `read:packages` scope → set as `GITHUB_TOKEN` (required for `@visable-dev/*` private packages)
- Add to `/etc/hosts`: `127.0.0.1 local.wlw.de local-qa1.wlw.de local-qa2.wlw.de local.europages.de`
- VPN access required for Docker-based local dev (company docker-compose)

## Install

Each submodule uses its own package manager:

| Submodule | Command |
|-----------|---------|
| `product-editor-frontend` | `pnpm config set '//npm.pkg.github.com/:_authToken' $GITHUB_TOKEN && pnpm install` |
| `supplier-onboarding-frontend` | same as above |
| `customer-dashboard-frontend` | `pnpm install` |
| `business-insights-frontend` | `yarn install --immutable` |
| `user-frontend` | `corepack enable && yarn` |
| `visitors-frontend` | `npm clean-install` |

## Dev Server

| Submodule | Command |
|-----------|---------|
| `product-editor-frontend` | `pnpm dev` / `pnpm dev:qa1` / `pnpm dev:qa2` |
| `supplier-onboarding-frontend` | `pnpm dev` or `docker compose up --detach` |
| `business-insights-frontend` | `yarn dev` |
| `user-frontend` | `yarn dev` or `dotenv -e .env.staging yarn dev` |
| `customer-dashboard-frontend` | `pnpm dev` |
| `visitors-frontend` | `npm run dev` or `./dev/start.sh` |

## Test

| Submodule | Unit | E2E |
|-----------|------|-----|
| `product-editor-frontend` | `pnpm test` | Cypress |
| `supplier-onboarding-frontend` | `pnpm test` | Cypress |
| `business-insights-frontend` | `yarn test` (placeholder only) | — |
| `user-frontend` | `yarn test` | — |
| `customer-dashboard-frontend` | `pnpm test:unit` | `pnpm test:e2e` |
| `visitors-frontend` | `npm run test` | — |

## Key Environment Variables

| Variable | Used By | Purpose |
|----------|---------|---------|
| `GITHUB_TOKEN` | all | Private npm package auth |
| `API_SUPPLIER_FACTS_URL` | product-editor, supplier-onboarding | Supplier-facts API base URL |
| `SUPPLIER_FACTS_API_CLIENT_ID/SECRET` | product-editor, supplier-onboarding | OAuth credentials |
| `API_USERS_URL` | product-editor, supplier-onboarding | User-profile API |
| `USERS_API_CLIENT_ID/SECRET` | product-editor, supplier-onboarding | User API OAuth credentials |
| `SENTRY_DSN` | product-editor, supplier-onboarding | Sentry error tracking |
| `NUXT_PUBLIC_TRACKING_ENV` | most apps | GA/GTM environment flag |
| `PROXY_BASE_URL` | user-frontend | Ory proxy base URL |
| `TEST_USER_PROFILE_ID` | user-frontend | Simulate logged-in user locally |
| `DD_AGENT_HOST` | user-frontend | Datadog agent host |
| `VAPID_PUBLIC_KEY` | user-frontend | Push notification key |

## i18n / Translations

- All apps sync translations via Lokalise CLI
- Add new strings to `locales/en.json` (or `i18n/locales/en.json`)
- Pull updates: `pnpm translations:download` / `yarn translations:download` / `npm run i18n:*`
