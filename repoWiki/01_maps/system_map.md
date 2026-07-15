# System Map

## Layers

```mermaid
flowchart LR
    Browser["Browser\n(Vue 3 / Nuxt)"]
    BFF["Nitro BFF\n(SSR + API Proxy)"]
    Auth["Ory Kratos\n(Auth)"]
    SupFacts["supplier-facts\nAPI"]
    UserAPI["user-profile\nAPI"]
    UCL["UCL / Category\nAPI"]
    BI["business-insights\nAPI"]

    Browser --> BFF
    BFF --> Auth
    BFF --> SupFacts
    BFF --> UserAPI
    BFF --> UCL
    BFF --> BI
```

## App Layer Map

| App | Render | Entry |
|-----|--------|-------|
| `user-frontend` | Nuxt SSR | Login, registration, account settings |
| `supplier-onboarding-frontend` | Nuxt SSR | Company editor, product onboarding, member mgmt |
| `product-editor-frontend` | Nuxt SSR | Product listing and quick editor |
| `business-insights-frontend` | Nuxt SSR | Analytics dashboards |
| `visitors-frontend` | Nuxt SSR | Visitor list and website leads |
| `customer-dashboard-frontend` | Vue 3 SPA (Vite) | Company overview dashboard |

## Auth Flow

```mermaid
flowchart LR
    User --> NuxtApp["Nuxt App\n(any submodule)"]
    NuxtApp --> AuthMiddleware["auth.global.ts"]
    AuthMiddleware --> OryKratos["Ory Kratos\n(session)"]
    OryKratos -->|x-forwarded-auth-user-profile| NuxtServer["Nitro Server\n(01.auth-proxy.ts)"]
    NuxtServer --> ExternalAPI["External APIs"]
```
