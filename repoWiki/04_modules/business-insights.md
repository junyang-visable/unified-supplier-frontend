# Module: Business Insights

**Responsibility:** Logged-in supplier analytics and upsell UI for company profile traffic, product performance, visitors, and paid campaigns (display, Google Ads, retargeting) on WLW/Europages.

## Entry Points

| Path | Role |
|------|------|
| `business-insights-frontend/nuxt.config.ts` | Nuxt 3 SSR bootstrap, modules, i18n, runtime config |
| `business-insights-frontend/app.vue` | Nuxt root shell |
| `business-insights-frontend/pages/my-account/business-insights/[uuid]/index.vue` | Main analytics dashboard |
| `business-insights-frontend/pages/my-account/business-insights/[uuid]/retargeting.vue` | Retargeting page (access-gated) |
| `business-insights-frontend/server/routes/status/health.ts` | SSR health check |

## Key Files

| Path | Role |
|------|------|
| `composables/useApi.ts` | Central BI data fetching (traffic, visitors, products, origin, graph, scoring) |
| `composables/fetch.ts` | useCustomFetch wrapper over $customFetch |
| `plugins/customFetch.ts` | Cookie-authenticated API client with 401 → login redirect |
| `stores/context.ts` | Brand (WLW/EP), user, language, internal-view toggle |
| `stores/auth.ts` | User auth bootstrap from BI user endpoint |
| `stores/traffic.ts` | Selected company profile, traffic metrics, top products, visitors state |
| `stores/time.ts` | Shared date-range selection for analytics queries |
| `middleware/00.context.global.ts` | Init brand/language from `x-platform` header |
| `middleware/01.auth.global.ts` | Auth gate, uuid resolution, company-profile access control |
| `middleware/restricted-page.ts` | Retargeting eligibility redirect rules |
| `utils/endpoints.ts` | BI API paths (/business-insights/api/v1|v2) |
| `constants/tracking.ts` | GA page/event key registry |

## Constraints

- SSR enabled; build assets namespaced under `/business-insights-frontend/`
- Dual-brand invariant: WLW vs EP drives login path, API platform param, UI behavior, and favicons
- All routes require authenticated user; unauthenticated → external brand login with `redirect_path`
- Route param `[uuid]` must match an assigned company profile (403 otherwise; admins exempt)
- BI API calls must go through `$customFetch` with cookie headers, not raw fetch
- Retargeting page gated by `restricted-page` middleware (verification + country/campaign eligibility)
- Tracking keys centralized in `constants/tracking.ts`; page views fired on mount
- business-insights has no real test suite (placeholder `echo test` command)
- Do not upgrade dependencies without checking WEB-3772 (webpack v4 compat)

## Scope Table

| Layer | Item | Description |
|-------|------|-------------|
| Implementation | `pages/my-account/business-insights/[uuid]/index.vue` | Profile dashboard entry, self-service banner, page tracking |
| Implementation | `pages/my-account/business-insights/[uuid]/display.vue` | Display upsell surface |
| Implementation | `pages/my-account/business-insights/[uuid]/google-ads.vue` | Google Ads campaign view or upsell |
| Implementation | `pages/my-account/business-insights/[uuid]/retargeting.vue` | Retargeting campaign view or upsell |
| Implementation | `components/profile/` | Analytics widgets (platform, products, visitors, origin, incoming requests) |
| Implementation | `components/upsell/` | Upsell containers for display, Google Ads, retargeting |
| Implementation | `composables/useApi.ts` | BI REST integration layer |
| Implementation | `stores/traffic.ts` | Company profile selection and derived analytics state |
| Implementation | `stores/time.ts` | Time-range presets and custom date selection |
| Implementation | `utils/pdf.ts` | PDF report export |
| Implementation | `utils/csvFormatter.ts` | CSV export |
| Consumer | `layouts/default.vue` | Consumes useApi.getTraffic, context/traffic/time/auth stores |
| Consumer | `components/profile/index.vue` | Consumes traffic store, time store, PDF export event bus |
| Consumer | `composables/useApi.ts` | Consumes traffic store, time store, useCustomFetch, endpoint constants |
| Consumer | `plugins/tracking.client.ts` | Consumes auth, context, traffic, tracking stores for GA custom dimensions |
