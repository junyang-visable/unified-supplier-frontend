# Module: Supplier Onboarding

**Responsibility:** Nuxt 3 SSR app for supplier registration, company profile editing, product/services onboarding, and team member management on WLW/Europages.

## Entry Points

| Path | Role |
|------|------|
| `supplier-onboarding-frontend/nuxt.config.ts` | App bootstrap (SSR, i18n, Pinia, runtime config) |
| `supplier-onboarding-frontend/middleware/auth.global.ts` | Global route guard — loads session or redirects to login |
| `supplier-onboarding-frontend/pages/my-account/new-company/index.vue` | Supplier registration / new-company funnel |
| `supplier-onboarding-frontend/pages/company-editor/[supplierId]/index.vue` | Company editor shell with tabbed routes |
| `supplier-onboarding-frontend/pages/my-account/supplier/[supplierId]/products-services/index.vue` | Product & services listing |
| `supplier-onboarding-frontend/pages/member-management/[supplierId]/index.vue` | Team member management |
| `supplier-onboarding-frontend/server/middleware/01.auth-proxy.ts` | Server-side auth for /api/* routes |

## Key Files

| Path | Role |
|------|------|
| `composables/useCustomFetch.ts` | SSR-safe authenticated useFetch wrapper |
| `plugins/customFetch.ts` | Client `$customFetch` with 401 → login redirect |
| `utils/fetch.ts` | Shared fetch options: CSRF, X-Supplier-Id, X-Language |
| `constants/endpoints.ts` | Central API path constants |
| `stores/auth.ts` | User session state and profile fetch |
| `stores/company-editor/company.ts` | Company detail fetch and JSON-patch save |
| `stores/companyForm.ts` | New-company registration form state and validation |
| `stores/memberManagement.ts` | Member list, invite, remove, invitation acceptance |
| `server/api/supplier-editor/detail.get.ts` | BFF: company detail with access check |
| `server/api/supplier-editor/edit.post.ts` | BFF: company JSON-patch updates |
| `server/utils/supplierFactsService.ts` | OAuth-backed supplier-facts API client |
| `open-api/supplier-facts/` | Generated TypeScript client for supplier-facts API |
| `locales/en.json` | Primary i18n source synced to Lokalise |

## Constraints

- Use `useCustomFetch` for SSR data; use `$customFetch` for UI-triggered requests
- `buildAssetsDir` must stay `/apps/supplier/onboarding-frontend` (CDN requirement)
- All pages pass through `auth.global` middleware; unauthenticated → external LOGIN_ROUTE with `return_to`
- All server `/api/*` routes require `event.context.auth` from `01.auth-proxy` middleware
- Internal calls must include CSRF, X-Supplier-Id, X-Language headers
- i18n uses prefix strategy with lazy-loaded locale files; new strings in `locales/en.json` → Lokalise sync
- Brand (wlw/ep) resolved server-side from `x-platform` header
- OpenAPI clients regenerated from `data/api-schema`; do not hand-edit `open-api/` output

## Scope Table

| Layer | Item | Description |
|-------|------|-------------|
| Implementation | `pages/my-account/supplier/[supplierId]/products-services/` | Product listing, bulk ops, import |
| Implementation | `pages/company-editor/[supplierId]/` | Company profile editor (basic info, delivery, media/certs) |
| Implementation | `pages/my-account/new-company/` | Registration funnel with VAT validation and UTM capture |
| Implementation | `pages/member-management/` | Team invites, member removal, invitation acceptance |
| Implementation | `components/company-editor/` | Company editor widgets, tabs, validation schemas |
| Implementation | `components/products-services/` | Product UI (list, form, import dialog, AI, attributes) |
| Implementation | `components/member-management/` | Member list, invite modal, company info display |
| Implementation | `stores/company-editor/company.ts` | Company data load and patch submission |
| Implementation | `stores/companyForm.ts` | Registration form validation and company creation POST |
| Implementation | `stores/memberManagement.ts` | Membership CRUD and invitation hash handling |
| Implementation | `server/api/supplier-editor/` | BFF routes proxying supplier-facts with access control |
| Consumer | `pages/company-editor/[supplierId]/index/basic-info.vue` | Uses company store, company-editor components |
| Consumer | `pages/my-account/new-company/index.vue` | Uses companyForm store, company components |
| Consumer | `middleware/auth.global.ts` | Uses auth store, useBaseURL, LOGIN_ROUTE |
| Consumer | `layouts/base.vue` | Uses auth, brand stores; renders VisPageHeader/AccountHeader |
