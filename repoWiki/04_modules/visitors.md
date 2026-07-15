# Module: Visitors

**Responsibility:** Supplier logged-in UI for visitor analytics (list, profiler, filters, CSV export) and website-leads pixel settings under `/my-account/visitors`.

## Entry Points

| Path | Role |
|------|------|
| `visitors-frontend/nuxt.config.ts` | Nuxt 3 bootstrap: custom routes, i18n (21 locales), CDN assets |
| `visitors-frontend/app.vue` | Nuxt root shell (NuxtLayout + NuxtPage) |
| `visitors-frontend/pages/visitors/index.vue` | Main visitors profiler page |
| `visitors-frontend/pages/visitors/website-leads.vue` | Website leads pixel settings page |
| `visitors-frontend/server/routes/visitors-frontend/status/health.ts` | Health check endpoint |

## Key Files

| Path | Role |
|------|------|
| `middleware/auth.global.ts` | Global login gate and supplierId fallback redirect |
| `layouts/default.vue` | VisBackOfficeLayout shell, GTM, favicons, page-view tracking |
| `plugins/customFetch.ts` | Cookie-authenticated $fetch with 401 → login redirect |
| `plugins/brand.ts` | Brand detection from `x-platform` request header |
| `plugins/tracking.client.ts` | Google Analytics via @visable-dev/tracking |
| `stores/visitors.ts` | Visitor list, details, pagination, fencing state |
| `stores/visitorFilters.ts` | Country, source-type, and time-range filter state |
| `stores/supplier.ts` | Supplier profile and website-leads pixel data |
| `stores/csvDownload.ts` | Async CSV export URL fetch and poll |
| `utils/api.ts` | Supplier API path builders and TypeScript response types |
| `composables/fetch.ts` | useCustomFetch wrapper over $customFetch |
| `components/visitors/VisitorProfiler.vue` | Core visitors UI orchestrator |
| `components/WebsiteLeads.vue` | Website leads pixel installation and status UI |

## Constraints

- Custom routes must stay locale-prefixed: `/my-account/visitors/:supplierId?` and sibling `/website-leads`
- Auth middleware redirects unauthenticated users to login; missing `supplierId` → redirect to first supplier profile
- Page data fetching runs client-side only (`onMounted`) to avoid SSR side effects
- Store actions expose errors; pages call `showError` — stores do not handle request failures internally
- Pinia option stores (not setup stores) for testability
- i18n namespace is `visitors`; keys synced via Lokalise tag `app:visitors-frontend`
- Brand white-label via `x-platform` header and `data-brand` HTML attribute (`ep`|`wlw`)
- All Supplier API calls go through `$customFetch` with cookie auth; 401 → external login redirect
- Preserve `data-testid` attributes — QA acceptance tests depend on them
- `AppIconSvgSprite` must render exactly once globally (SVG ID uniqueness)

## Scope Table

| Layer | Item | Description |
|-------|------|-------------|
| Implementation | `pages/visitors/index.vue` | Visitor page: data orchestration, tracking, CSV download, self-service banner |
| Implementation | `pages/visitors/website-leads.vue` | Website leads settings page |
| Implementation | `components/visitors/VisitorProfiler.vue` | Visitor list, filters, detail profiler, empty/fenced states |
| Implementation | `components/visitors/VisitorList.vue` | Paginated visitor list with load-more |
| Implementation | `components/visitors/FilterPanel.vue` | Active filter chips and reset |
| Implementation | `components/visitors/CompanyInsights.vue` | Company detail panel (address, contact, social) |
| Implementation | `components/WebsiteLeads.vue` | Pixel code snippet, GTM instructions, quota/status display |
| Implementation | `stores/visitors.ts` | fetchVisitors, fetchVisitorDetails, fetchNextPage, isFenced state |
| Implementation | `stores/visitorFilters.ts` | Filter presets, country/source API params, fetchCountries |
| Implementation | `stores/supplier.ts` | fetchSupplier, fetchWebsitePixel, feature getters |
| Implementation | `stores/csvDownload.ts` | POST download URL + poll until processed |
| Implementation | `utils/api.ts` | All Supplier API paths and response interfaces |
| Consumer | `pages/visitors/index.vue` | Consumes VisitorProfiler, visitors/visitorFilters/supplier/csvDownload stores |
| Consumer | `layouts/default.vue` | Consumes meta/user stores, VisBackOfficeLayout |
| Consumer | `middleware/auth.global.ts` | Consumes user store, routes utils, page route names |
| Consumer | `plugins/tracking.client.ts` | Consumes meta/tracking/user stores for GA custom dimensions |
