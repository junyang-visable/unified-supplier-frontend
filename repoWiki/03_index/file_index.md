# File Index

## Root

| Path | Meaning |
|------|---------|
| `README.md` | Monorepo title and one-line description |
| `.gitmodules` | Git submodule configuration for all six frontend apps |
| `product-editor-frontend/` | Submodule: Nuxt 4 SSR app for supplier product listing and editing |
| `supplier-onboarding-frontend/` | Submodule: Nuxt 3 SSR app for supplier onboarding and company editor |
| `business-insights-frontend/` | Submodule: Nuxt 3 SSR app for supplier analytics dashboards |
| `user-frontend/` | Submodule: Nuxt 3 app for login, registration, and account settings |
| `customer-dashboard-frontend/` | Submodule: Vue 3 + Vite SPA for company overview dashboard |
| `visitors-frontend/` | Submodule: Nuxt 3 SSR app for visitor analytics and website leads |

## product-editor-frontend/

| Path | Meaning |
|------|---------|
| `nuxt.config.ts` | Nuxt 4 config: modules, i18n, proxies, Vite plugins, runtime config |
| `package.json` | Scripts, deps, OpenAPI generation, and i18n CLI |
| `app/pages/…/products-services/index.vue` | Product listing route |
| `app/pages/…/products-services/product.vue` | Product create/edit/duplicate route |
| `app/components/products-services/listing/QuickEditorListing.vue` | Primary listing + inline quick editor |
| `app/components/products-services/ImportDialog/ImportDialog.vue` | Bulk import modal (URL, Excel, shop) |
| `app/stores/product.ts` | Single-product editor state, validation, save/publish/delete |
| `app/stores/productList.ts` | Paginated listing, inline edit map, bulk publish/delete |
| `app/stores/bulkImport.ts` | Import job creation, status polling, cancel |
| `app/composables/useCustomFetch.ts` | SSR-safe authenticated useFetch |
| `app/plugins/customFetch.ts` | `$customFetch` for UI-triggered requests with 401 redirect |
| `app/constants/endpoints.ts` | API path registry |
| `app/middleware/auth.global.ts` | Session bootstrap and unauthenticated redirect |
| `server/middleware/01.auth-proxy.ts` | Auth context injection for /api routes |
| `open-api/supplier-facts/` | Generated TypeScript fetch client for supplier-facts API |
| `data/api-schema/` | OpenAPI JSON schemas for code generation |
| `Dockerfile` | Multi-stage container image for Nuxt SSR |
| `visable.yaml` | Visable platform deployment metadata |
| `.env` | Local environment variable defaults |

## supplier-onboarding-frontend/

| Path | Meaning |
|------|---------|
| `nuxt.config.ts` | Nuxt app config: modules, i18n, runtime config, build settings |
| `package.json` | Scripts, deps, OpenAPI codegen, Lokalise sync |
| `pages/my-account/new-company/index.vue` | Supplier registration funnel |
| `pages/my-account/supplier/[supplierId]/products-services/` | Product listing and edit pages |
| `pages/company-editor/[supplierId]/index.vue` | Company editor shell with tabbed sub-routes |
| `pages/member-management/[supplierId]/index.vue` | Team member management |
| `components/company-editor/` | Company editor widgets, tabs, validation |
| `components/products-services/` | Product UI (list, form, import, AI, attributes) |
| `components/member-management/` | Member list, invite modal |
| `stores/company-editor/company.ts` | Company detail fetch and JSON-patch save |
| `stores/companyForm.ts` | Registration form state and validation |
| `stores/memberManagement.ts` | Membership CRUD and invitation handling |
| `server/api/supplier-editor/` | BFF routes proxying supplier-facts API |
| `server/utils/supplierFactsService.ts` | OAuth-backed supplier-facts client |
| `composables/useCustomFetch.ts` | Authenticated SSR useFetch composable |
| `constants/endpoints.ts` | Central API path constants |
| `open-api/supplier-facts/` | Generated TypeScript client for supplier-facts API |
| `locales/en.json` | Primary i18n source synced to Lokalise |

## business-insights-frontend/

| Path | Meaning |
|------|---------|
| `nuxt.config.ts` | Nuxt 3 SSR config, i18n, PrimeVue chart, namespaced assets |
| `pages/my-account/business-insights/[uuid]/index.vue` | Main analytics dashboard |
| `pages/my-account/business-insights/[uuid]/display.vue` | Display campaign upsell page |
| `pages/my-account/business-insights/[uuid]/google-ads.vue` | Google Ads analytics/upsell |
| `pages/my-account/business-insights/[uuid]/retargeting.vue` | Retargeting page (access-gated) |
| `composables/useApi.ts` | Central BI data fetching (traffic, visitors, products, graph) |
| `stores/context.ts` | Brand, user, language, internal-view state |
| `stores/traffic.ts` | Company profile selection and analytics metrics |
| `stores/time.ts` | Shared date-range selection |
| `components/profile/` | Analytics widgets (platform, products, visitors, origin) |
| `components/upsell/` | Upsell containers for display/Google Ads/retargeting |
| `middleware/01.auth.global.ts` | Auth gate, UUID resolution, access control |
| `middleware/restricted-page.ts` | Retargeting eligibility check |
| `utils/endpoints.ts` | BI API path constants |
| `utils/pdf.ts` | PDF report export helper |
| `utils/csvFormatter.ts` | CSV export helper |

## user-frontend/

| Path | Meaning |
|------|---------|
| `routes.ts` | All locale-prefixed auth/account route definitions |
| `nuxt.config.ts` | Nuxt 3 config: srcDir, i18n, Nitro rules, runtime config |
| `src/views/LoginPage.vue` | Password/OIDC login via Ory Kratos |
| `src/views/UnifiedLoginPage.vue` | OAuth2/Hydra unified login flow |
| `src/views/RegistrationPage.vue` | Purchaser self-service registration |
| `src/views/SettingsPage.vue` | Account settings hub |
| `src/middleware/auth.global.ts` | Global session hydration |
| `src/utils/api.ts` | All Ory Kratos flow HTTP calls |
| `src/utils/oryHelpers.ts` | Flow parsing, sudo/OIDC resolution, redirect helpers |
| `src/utils/ssrHelpers.ts` | SSR cookie proxying and auth header parsing |
| `src/server/middleware/oryProxy.ts` | Dev proxy for Ory/oauth/user-hub paths |
| `src/server/routes/user-frontend/api/v1/user-profile.ts` | BFF user profile GET/PATCH |
| `src/composables/state.ts` | Nuxt useState for userInfo, userUuid, userProperties |
| `src/constants/cookies.ts` | Auth cookie name constants |
| `src/constants/login.ts` | OIDC provider enum |

## customer-dashboard-frontend/

| Path | Meaning |
|------|---------|
| `index.html` | Vite HTML shell mounting Vue app at #app |
| `src/main.ts` | App bootstrap: Vue, router, i18n, Pinia, Sentry |
| `src/router/index.ts` | Route `/:lang/company-overview/:companyId` → Index |
| `src/App.vue` | Auth gate, VisPageHeader/Footer, RouterView shell |
| `src/views/Index.vue` | Company overview page composition and data loading |
| `src/stores/context.ts` | User, brand, locale, env, GA/GA4 state |
| `src/utils/request.ts` | API clients for Ory whoami, business-insights, supplier-editor |
| `src/utils/init.ts` | Brand/locale/env detection from hostname |
| `src/utils/track.ts` | Legacy GA event helper |
| `src/utils/trackingGA4.ts` | GA4 wrapper via @visable-dev/tracking |
| `vite.config.ts` | Dev server, API proxy to wlw-staging, `@` alias |
| `default.conf` | nginx config for SPA routing under /company-overview |

## visitors-frontend/

| Path | Meaning |
|------|---------|
| `nuxt.config.ts` | Nuxt 3 config: custom routes, i18n, CDN assets |
| `pages/visitors/index.vue` | Main visitors profiler page |
| `pages/visitors/website-leads.vue` | Website leads pixel settings page |
| `components/visitors/VisitorProfiler.vue` | Core visitors UI orchestrator |
| `components/WebsiteLeads.vue` | Website leads pixel UI |
| `stores/visitors.ts` | Visitor list, details, pagination, fencing state |
| `stores/visitorFilters.ts` | Country, source-type, time-range filter state |
| `stores/supplier.ts` | Supplier profile and website-leads pixel data |
| `stores/csvDownload.ts` | Async CSV export URL fetch and poll |
| `utils/api.ts` | Supplier API path builders and response types |
| `composables/fetch.ts` | useCustomFetch wrapper over $customFetch |
| `middleware/auth.global.ts` | Global login gate and supplierId fallback |
| `plugins/customFetch.ts` | Cookie-authenticated $fetch with 401 redirect |
| `plugins/brand.ts` | Brand detection from `x-platform` header |
