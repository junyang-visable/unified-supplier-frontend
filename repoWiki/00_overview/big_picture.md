# Big Picture

## What This System Does

Unified monorepo of six Vue/Nuxt frontend apps serving the supplier-facing domain on WLW and Europages B2B marketplaces.

Supplier journey: Register → Edit company profile → Manage products → Monitor analytics → Review visitors → Inspect company overview

## Core Flow

```
Browser → Nuxt Nitro BFF/SSR → External REST APIs (supplier-facts, user-profile, OAuth/Ory Kratos)
```

Each submodule owns one vertical slice of the supplier journey:

- `user-frontend` → authentication and account lifecycle
- `supplier-onboarding-frontend` → company editor and product onboarding
- `product-editor-frontend` → product listing and inline quick editor
- `business-insights-frontend` → traffic, product, and campaign analytics
- `visitors-frontend` → visitor profiling and website leads
- `customer-dashboard-frontend` → company overview SPA (Vue 3 + Vite, no SSR)

## Constraints

- All apps share `@visable-dev/*` design system packages (vue, styleguide, design-tokens, tracking, routing)
- Auth is Ory Kratos self-service flows; session delivered via `x-forwarded-auth-user-profile` header
- Dual-brand invariant: WLW vs Europages drives login paths, API `platform` params, favicons, and UI tokens
- Each app is independently deployed via Docker + GitHub Actions to AWS ECS (Visable platform)
- Private npm packages require `GITHUB_TOKEN` for install from `npm.pkg.github.com`
