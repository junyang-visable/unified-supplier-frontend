# Module: User Account & Auth

**Responsibility:** End-to-end user identity lifecycle via Ory Kratos (login, registration, verification, recovery, consent, sudo) plus account settings, profile BFF, and session state for the Visable/wlw marketplace.

## Entry Points

| Path | Role |
|------|------|
| `user-frontend/routes.ts` | All locale-prefixed auth/account route definitions |
| `user-frontend/src/views/LoginPage.vue` | Password/OIDC login via Ory Kratos |
| `user-frontend/src/views/UnifiedLoginPage.vue` | OAuth2/Hydra unified login flow |
| `user-frontend/src/views/RegistrationPage.vue` | Purchaser self-service registration |
| `user-frontend/src/views/SettingsPage.vue` | Account settings hub |
| `user-frontend/src/middleware/auth.global.ts` | Global session hydration on every navigation |
| `user-frontend/src/server/routes/user-frontend/api/v1/user-profile.ts` | BFF GET/PATCH user profile |

## Key Files

| Path | Role |
|------|------|
| `src/utils/api.ts` | All Ory Kratos flow HTTP calls and BFF-facing client methods |
| `src/utils/oryHelpers.ts` | Flow parsing, sudo/OIDC resolution, redirect helpers |
| `src/utils/ssrHelpers.ts` | SSR cookie proxying and auth header parsing |
| `src/utils/routeHelpers.ts` | Canonical path builders for login, logout, registration, etc. |
| `src/utils/urlHelpers.ts` | `return_to` URL validation (same-origin / proxy allowlist) |
| `src/utils/oauthApiClients.server.ts` | Server-side OAuth client-credentials for user-backend/profile APIs |
| `src/composables/state.ts` | Nuxt `useState` for `userInfo`, `userUuid`, `userProperties` |
| `src/composables/cookies.ts` | Auth-related cookies (`user_email`, `admin`, etc.) |
| `src/composables/oryErrors.ts` | Map Ory UI errors to i18n messages |
| `src/server/middleware/oryProxy.ts` | Dev-only proxy for `/.ory/`, `/oauth2/`, `/user/` |
| `src/constants/login.ts` | OIDC provider enum (google, apple, alibaba, internal) |
| `src/constants/cookies.ts` | Auth cookie name constants |
| `src/plugins/5.nuxtServerInit.server.ts` | SSR init: brand, traffic source, user UUID from headers/env |

## Constraints

- Auth is Ory Kratos self-service flows; do not bypass CSRF tokens or flow IDs from Kratos UI nodes
- Session identity comes from `x-forwarded-auth-user-profile` header (staging/prod) or `TEST_USER_PROFILE_ID` (local) — not from client-side tokens
- `return_to` URLs must pass `validateReturnUrl`; reject open redirects
- Sensitive settings (edit email/password) require `needSudo` → redirect to SudoPage first
- Ory action URLs must be rewritten in dev via `convertOryActionUrl` / `generateOryReturnUrl`
- BFF profile routes resolve profile id via `/api/user-hub/me` cookies, then call user-backend with server-side OAuth credentials
- Logout must call `clearUserSession()`, clear cookies, and follow Kratos browser logout URL
- No Pinia/Vuex stores — user state lives in Nuxt `useState` in `src/composables/state.ts`

## Scope Table

| Layer | Item | Description |
|-------|------|-------------|
| Implementation | `routes.ts` | All auth/account page route definitions with locale prefix |
| Implementation | `src/views/LoginPage.vue` | Standard email/password + third-party login via Kratos |
| Implementation | `src/views/UnifiedLoginPage.vue` | Hydra OAuth2 login challenge flow |
| Implementation | `src/views/LogoutPage.vue` | Kratos logout browser flow |
| Implementation | `src/views/RegistrationPage.vue` | Purchaser registration browser/code flows |
| Implementation | `src/views/SettingsPage.vue` | Account hub: profile PATCH, company profiles, security, deletion |
| Implementation | `src/middleware/auth.global.ts` | `fetchUserInfo()` / `clearUserSession()` on every route |
| Implementation | `src/utils/api.ts` | All Kratos flow HTTP calls and BFF client methods |
| Implementation | `src/utils/oryHelpers.ts` | Flow node extraction, sudo method resolution |
| Implementation | `src/server/middleware/oryProxy.ts` | Development proxy for Ory/oauth/user-hub paths |
| Implementation | `src/server/routes/user-frontend/api/v1/user-profile.ts` | BFF profile read/update |
| Implementation | `src/composables/state.ts` | Shared reactive user session state |
| Consumer | `src/app.vue` | Renders logged-in header/footer using `useUserInfo()` |
| Consumer | `src/plugins/tracking.client.ts` | Sends `logged_in` and `user_id` GA4 properties from auth state |
| Consumer | `nuxt.config.ts` | Vite Ory proxy and Nitro route rules for auth calls |
