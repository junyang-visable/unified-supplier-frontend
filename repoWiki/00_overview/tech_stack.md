# Tech Stack

## Runtime

- Node.js → 20–22 (pinned per submodule via `.tool-versions` / `.node-version`)
- Package managers → pnpm (product-editor, supplier-onboarding, customer-dashboard), Yarn 4 Berry (business-insights, user-frontend), npm (visitors-frontend)

## Frontend

- Vue 3 → component framework (all apps)
- Nuxt 3/4 → SSR + file-based routing + Nitro BFF (all except customer-dashboard-frontend)
- Vite → build tool for customer-dashboard-frontend (pure SPA)
- TypeScript → all apps
- Pinia → state management (all apps; user-frontend uses Nuxt `useState` instead)
- Tailwind CSS → utility styling (all apps)
- Sass/SCSS → supplemental styles (visitors-frontend, business-insights-frontend)

## UI Framework

- `@visable-dev/vue` → Visable shared component library (all apps)
- `@visable-dev/styleguide` → design tokens and base styles
- PrimeVue / chart → used in business-insights-frontend for charts

## Backend (BFF)

- Nuxt Nitro → SSR server routes, API proxies, auth middleware (all Nuxt apps)
- Ory Kratos → identity/session provider accessed via user-frontend BFF
- OAuth2/Hydra → authorization server; unified login flow in user-frontend

## Storage

- none (stateless frontend; external REST APIs as data source)
- `node-cache` → in-memory TTL cache for BFF responses in some apps

## External APIs

- `supplier-facts` API → company and product data
- `user-profile` / `user-backend` API → user identity and profile
- `UCL` API → category/sector taxonomy
- `supplier-dashboard-backend` → company overview tasks
- `business-insights` API → traffic and analytics data

## Infra

- Docker → all apps containerized (multi-stage builds)
- AWS ECR → container image registry
- AWS ECS → container runtime (`wlw:staging/production:wlw-1`)
- GitHub Actions → CI/CD per submodule (validate → test → docker_build → deploy)
- Visable platform (`visable.yaml`) → deployment metadata and pipeline hooks
- Datadog → runtime monitoring and APM (dd-trace)
- Sentry → frontend error tracking
- Lokalise → translation sync (21 locales per app)
- Google Tag Manager / GA4 → analytics tracking
