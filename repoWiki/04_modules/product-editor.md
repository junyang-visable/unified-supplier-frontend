# Module: Product Editor

**Responsibility:** Nuxt 4 SSR app for supplier product listing, inline quick editing, bulk import, and full create/edit/publish workflows.

## Entry Points

| Path | Role |
|------|------|
| `product-editor-frontend/app/pages/…/products-services/index.vue` | Product listing route |
| `product-editor-frontend/app/pages/…/products-services/product.vue` | Product create/edit/duplicate route |
| `product-editor-frontend/nuxt.config.ts` | App bootstrap (SSR, i18n, Pinia, devProxy, CDN namespace) |
| `product-editor-frontend/server/middleware/01.auth-proxy.ts` | Auth context injection for /api routes |

## Key Files

| Path | Role |
|------|------|
| `app/stores/product.ts` | Single-product editor state, validation, scoring, save/publish/delete |
| `app/stores/productList.ts` | Paginated listing, inline edit map, bulk publish/delete |
| `app/stores/bulkImport.ts` | Excel/URL/shop import jobs and status polling |
| `app/stores/productCategory.ts` | Category tree search and CPV attribute loading |
| `app/stores/productAiSuggestions.ts` | Per-field AI content suggestion fetch/apply |
| `app/composables/useCustomFetch.ts` | Authenticated useFetch for SSR-safe initial data |
| `app/plugins/customFetch.ts` | `$customFetch` for UI-triggered requests with 401 redirect |
| `app/constants/endpoints.ts` | Supplier-onboarding BFF path registry |
| `app/components/products-services/listing/QuickEditorListing.vue` | Primary listing + inline quick editor |
| `app/components/products-services/ImportDialog/ImportDialog.vue` | Bulk import modal |
| `app/middleware/auth.global.ts` | Session bootstrap; redirect unauthenticated users |
| `open-api/supplier-facts/` | Generated TypeScript fetch client for supplier-facts API |

## Constraints

- Use `useCustomFetch` for SSR/initial load; use `$customFetch` for user-triggered requests — one instance per request type
- Supplier scope from `route.params.supplierId` via `useSupplierId()`; pages use `layout: 'supplier'`
- `buildAssetsDir` must remain `product-editor-frontend` namespace for CDN routing
- Auth user state on `useState('authUser')` for SSR hydration; 401 → external login with `return_to` and `is_supplier=1`
- All user-facing copy via `product_editor.*` i18n keys (Lokalise tag `product-editor`); no hardcoded strings
- Server auth middleware applies only to `/api`; dev stage injects mock auth
- Product page mode is query-driven: `productId` (edit), `duplicate` (copy), `onboarding` (first-run modal)
- Import dialog supports URL, Excel, shop types only

## Scope Table

| Layer | Item | Description |
|-------|------|-------------|
| Implementation | `app/pages/…/products-services/index.vue` | Listing page shell, self-service banner gating |
| Implementation | `app/pages/…/products-services/product.vue` | Full product form lifecycle (load, validate, draft/publish/update/delete) |
| Implementation | `app/components/products-services/listing/` | QuickEditorListing desktop/mobile views, toolbars |
| Implementation | `app/components/products-services/quick-editor/` | Inline table cells, edit drawer, unsaved-changes modal |
| Implementation | `app/components/products-services/ImportDialog/` | Bulk import modal and tab contents |
| Implementation | `app/components/products-services/attributes/` | Dynamic category attribute field widgets |
| Implementation | `app/components/products-services/ai/` | AI optimize/suggest UI triggers |
| Implementation | `app/stores/product.ts` | Product CRUD, completion scoring, schema validation, media sync |
| Implementation | `app/stores/productList.ts` | List fetch/stats, inline edit map, batch update/delete |
| Implementation | `app/stores/bulkImport.ts` | Import job creation, status polling, cancel |
| Implementation | `server/middleware/01.auth-proxy.ts` | /api auth context from forwarded user profile header |
| Consumer | `app/middleware/auth.global.ts` | Bootstraps auth store; redirects 401 to localized login |
| Consumer | `app/layouts/supplier.vue` | Supplier page shell wrapping listing/editor pages |
| Consumer | `app/plugins/tracking.client.ts` | Reads brand, auth, supplierId for GA/GA4 event payloads |
| Consumer | `tests/pages/…/products-services/` | Integration tests for listing and product form pages |
