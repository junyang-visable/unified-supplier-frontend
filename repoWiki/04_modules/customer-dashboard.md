# Module: Customer Dashboard

**Responsibility:** Supplier company-overview SPA for WLW and Europages — authenticated dashboard for a selected company (tasks, messages, insights, ranking, contact).

## Entry Points

| Path | Role |
|------|------|
| `customer-dashboard-frontend/index.html` | Vite HTML shell; mounts Vue app at #app |
| `customer-dashboard-frontend/src/main.ts` | App bootstrap: Pinia, router, i18n, Unhead, Sentry |
| `customer-dashboard-frontend/src/router/index.ts` | Route `/:lang/company-overview/:companyId` → Index view |
| `customer-dashboard-frontend/src/App.vue` | Auth gate, VisPageHeader/Footer, RouterView shell |
| `customer-dashboard-frontend/src/views/Index.vue` | Company overview page composition and data loading |
| `customer-dashboard-frontend/vite.config.ts` | Dev server, API proxy to wlw-staging |

## Key Files

| Path | Role |
|------|------|
| `src/stores/context.ts` | Global context: user, brand, locale, env, GA/GA4 |
| `src/utils/request.ts` | API clients (Ory whoami, business-insights, supplier-editor, dashboard-backend) |
| `src/utils/init.ts` | Brand/locale/env detection from hostname; tracking init |
| `src/utils/track.ts` | Legacy GA event helper (page/click/show) |
| `src/utils/trackingGA4.ts` | GA4 wrapper via @visable-dev/tracking |
| `src/i18n.config.ts` | Vue I18n with 21 locale JSON files |
| `src/composables/useLocaleStorage.ts` | Reactive localStorage polling composable |
| `default.conf` | nginx config for SPA routing under `/company-overview` |
| `visable.yaml` | CI/CD: Docker image, staging/prod deploy |

## Constraints

- Requires authenticated session via `/.ory/backend/user/whoami`; unauthenticated → `/user/login`
- Route must stay `/:lang/company-overview/:companyId`; nginx serves per-locale paths (21 locales + fallback)
- Brand (wlw vs ep) and locale derived from hostname/path; preserve dual-brand styling (`data-brand`, Tailwind `ep:` variants)
- Production build uses relative base (`./`); dev uses `/`
- Translations managed via Lokalise tag `company-overview`
- No server-side code; all backend access via proxied REST APIs
- Depends on Visable shared packages (@visable-dev/vue, styleguide, design-tokens, tracking, routing)

## Scope Table

| Layer | Item | Description |
|-------|------|-------------|
| Implementation | `src/views/Index.vue` | Orchestrates company overview: fetch overview/membership/revisions, layout, tracking |
| Implementation | `src/components/CompanyTitle.vue` | Company header, profile actions, locale-aware hide-company |
| Implementation | `src/components/SuggestedTasks.vue` | Onboarding task list, completion via supplier-dashboard-backend API |
| Implementation | `src/components/BusinessInsight.vue` | Page views, SERP, received-requests metrics with links |
| Implementation | `src/components/TopRanking.vue` | Top ranking upsell/banner (brand-specific) |
| Implementation | `src/components/MessagesResponse.vue` | Request/message response summary widget |
| Implementation | `src/components/RequestHub.vue` | Request hub promo/entry card |
| Implementation | `src/stores/context.ts` | Pinia store for user, brand, locale, env, analytics instances |
| Implementation | `src/utils/request.ts` | HTTP layer for user auth, company overview, membership, revisions, task completion |
| Implementation | `src/utils/init.ts` | Runtime brand/locale/env/country detection and GA initialization |
| Implementation | `default.conf` | Per-locale nginx SPA fallback routing |
| Consumer | `src/App.vue` | Uses context store, init utils, request (whoami), VisPageHeader/Footer |
| Consumer | `src/views/Index.vue` | Consumes context store, request utils, all dashboard components |
| Consumer | `src/utils/track.ts` | Reads context store for brand/locale/GA when emitting events |
| Consumer | `vite.config.ts` | Dev proxy consumer of staging backends |
