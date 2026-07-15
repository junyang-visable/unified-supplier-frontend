# Unified Supplier Frontend - Runbook

<!-- metadata
generated: 2026-07-15
last_synced: 2026-07-15
-->

Agent-optimized knowledge base for the unified-supplier-frontend monorepo.

---

## Quick Links

### Getting Started
- [Big Picture](./00_overview/big_picture.md) - What the system does and supplier journey
- [Tech Stack](./00_overview/tech_stack.md) - All frameworks, tools, and infra

### Navigation

**Maps** - Find files and understand structure
- [System Map](./01_maps/system_map.md) - Core layers, auth flow, and integrations
- [Feature Map](./01_maps/feature_map.md) - Features → module and entry point
- [Module Map](./01_maps/module_map.md) - All six submodule apps

**Index** - File reference
- [File Index](./03_index/file_index.md) - Path to meaning, grouped by submodule

**Modules** - Deep dive
- [Product Editor](./04_modules/product-editor.md) - Product listing, quick editor, bulk import
- [Supplier Onboarding](./04_modules/supplier-onboarding.md) - Company editor, registration, members
- [Business Insights](./04_modules/business-insights.md) - Analytics dashboards, upsell, retargeting
- [User Account & Auth](./04_modules/user-account-auth.md) - Ory Kratos auth, account settings
- [Customer Dashboard](./04_modules/customer-dashboard.md) - Company overview SPA
- [Visitors](./04_modules/visitors.md) - Visitor profiling and website leads

### How-To

**Guides**
- [Development](./02_guides/dev.md) - Install, dev server, env vars, i18n
- [Debug](./02_guides/debug.md) - Common issues and auth debugging
- [Deploy](./02_guides/deploy.md) - Build, Docker, CI/CD pipeline

---

## Features

<!-- add links to 06_features/*.md as features are documented -->

---

## Rules

1. Always locate feature in feature_map before editing code
2. Each submodule is independently deployable — changes in one do not affect others at runtime
3. Auth is always Ory Kratos self-service flows — do not bypass CSRF tokens or flow IDs
4. Keep build asset namespaces per-app (CDN routing depends on them)
5. API clients (`open-api/`) are generated — do not hand-edit
6. Check module constraints in `./04_modules/*` before modifying fetch, auth, or i18n

---

## Navigation Strategy (for agents)

1. Start from [feature_map](./01_maps/feature_map.md) → identify which submodule owns the feature
2. Go to [module_map](./01_maps/module_map.md) → find the app entry point
3. Use [file_index](./03_index/file_index.md) → locate exact files
4. Read the module doc in `04_modules/` for constraints before modifying
5. Follow [dev guide](./02_guides/dev.md) for commands and env setup
