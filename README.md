# Warrant (warrant-dev)

Warrant was a centralized, fine-grained authorization (FGA) and access control service inspired by Google Zanzibar. It exposed a real-time REST API to define an authorization model, store relationships (called **warrants**) between objects, and run low-latency **access checks** and relationship **queries**. It supported relationship-based (ReBAC), role-based (RBAC), and attribute-based (ABAC) access control, plus entitlements such as pricing tiers and feature gating. The core engine is open source (Apache-2.0, [github.com/warrant-dev/warrant](https://github.com/warrant-dev/warrant)) and self-hostable against MySQL, Postgres, or SQLite.

> **Status: RETIRED (transition).** Warrant was acquired by **WorkOS** on **2024-04-23** and folded into **WorkOS FGA**. The standalone hosted Warrant service (`api.warrant.dev`) and the Warrant-based WorkOS FGA were **deprecated and sunset on 2025-11-15**; a re-architected FGA is in development at WorkOS. The `docs.warrant.dev` host no longer resolves. This entry documents Warrant's public API **historically** — the endpoints below are **modeled** from Warrant's documented `api.warrant.dev` v1/v2 API and the open-source engine, not from a live contract. Current fine-grained authorization is offered through [WorkOS FGA](https://workos.com/docs/fga); the **open-source engine remains self-hostable** and is not retired.

**APIs.json:** [https://raw.githubusercontent.com/api-evangelist/warrant-dev/refs/heads/main/apis.yml](https://raw.githubusercontent.com/api-evangelist/warrant-dev/refs/heads/main/apis.yml)

## Access Model

Warrant is an implementation of Google's Zanzibar design. You authorize by storing **relationship tuples** and then asking the check engine questions:

- **Authorization model** — you declare **object types** (e.g. `document`, `folder`, `tenant`, `role`) and the **relations** between them, including inheritance/implication rules (a `document` can inherit `editor` from its parent `folder`).
- **Warrants** — access rules of the form `subject is relation of object` (e.g. `user:1 is editor of document:xyz`). Writing a warrant grants access; deleting it revokes access.
- **Check** — the hot-path decision: `POST /v2/authorize` returns whether a subject has a relation/permission on an object, optionally batched with `anyOf`/`allOf` and evaluated against runtime attributes (ABAC).
- **Query** — `GET /v1/query` runs the **Warrant Query Language (WQL)** to enumerate which subjects can access an object, or which objects a subject can access.
- **RBAC & entitlements** — convenience surfaces for `roles`, `permissions`, `users`, `tenants`, `features`, and `pricing-tiers`, all built on top of objects and warrants.

**Authentication:** API key in the `Authorization: ApiKey <key>` header. The hosted base URL was `https://api.warrant.dev`; the open-source self-hosted server listens on `http://localhost:8000` by default.

## Tags

- Access Control
- Authorization
- Fine-Grained Authorization
- FGA
- RBAC
- ReBAC
- ABAC
- Zanzibar
- Permissions
- Open Source
- Retired

## Timestamps

- **Created:** 2026-07-11
- **Modified:** 2026-07-11

## APIs

### Warrant Objects API

Create, list, get, update, and delete the objects (resources and subjects — users, tenants, documents, folders, roles) that participate in the authorization model. Objects are identified by an object type and an object id and can carry arbitrary metadata.

- **Base URL:** `https://api.warrant.dev/v1`
- **Documentation:** [github.com/warrant-dev/warrant](https://github.com/warrant-dev/warrant)
- [OpenAPI](openapi/warrant-dev-openapi.yml)

### Warrant Relationships (Warrants) API

Manage warrants — the access rules that assign a relation between a subject and an object. Create, list, query, and delete warrants; warrants are the Zanzibar-style relationship tuples the check engine evaluates.

- **Base URL:** `https://api.warrant.dev/v1`
- **Documentation:** [workos.com/docs/fga/warrants](https://workos.com/docs/fga/warrants)
- [OpenAPI](openapi/warrant-dev-openapi.yml)

### Warrant Check (Authorization) API

The real-time, low-latency authorization surface. Check whether a subject has a given permission/relation on an object (`POST /v2/authorize`) and run relationship queries with the Warrant Query Language (`GET /v1/query`). This is the hot-path API applications called on every access decision.

- **Base URL:** `https://api.warrant.dev`
- **Documentation:** [workos.com/docs/fga](https://workos.com/docs/fga)
- [OpenAPI](openapi/warrant-dev-openapi.yml)

### Warrant Object Types API

Define and manage the authorization model itself — the object (resource) types and the relations between them, including inheritance and implication rules. Create, list, get, update, and delete object types.

- **Base URL:** `https://api.warrant.dev/v1`
- **Documentation:** [workos.com/docs/fga/resource-types](https://workos.com/docs/fga/resource-types)
- [OpenAPI](openapi/warrant-dev-openapi.yml)

### Warrant Roles & Permissions API

Convenience RBAC and entitlements surface built on top of objects and warrants — manage roles, permissions, users, tenants, features, and pricing tiers, and assign them to each other (a permission to a role, a role to a user, a user to a tenant, a feature to a pricing tier).

- **Base URL:** `https://api.warrant.dev/v1`
- **Documentation:** [workos.com/docs/fga](https://workos.com/docs/fga)
- [OpenAPI](openapi/warrant-dev-openapi.yml)

## Artifacts

- [OpenAPI](openapi/warrant-dev-openapi.yml) — modeled from the documented v1/v2 API
- [Postman Collection](collections/warrant-dev.postman_collection.json)
- [Open Collection](collections/warrant-dev.opencollection.json)
- [Authentication](authentication/warrant-dev-authentication.yml)
- [Plans / Pricing](plans/warrant-dev-plans-pricing.yml)
- [Rate Limits](rate-limits/warrant-dev-rate-limits.yml)
- [FinOps](finops/warrant-dev-finops.yml)
- [Domain Security](security/warrant-dev-domain-security.yml)
- [Review](review.yml)

## Open Source

The core Warrant engine is open source under **Apache-2.0** and self-hostable via Docker ([github.com/warrant-dev/warrant](https://github.com/warrant-dev/warrant)), backed by MySQL, Postgres, or SQLite. Client SDKs (Node, Go, Java, React, Vue, JS) and `warrant-cli` are MIT/Apache licensed. The self-hosted engine exposes the same REST surface (default port `8000`) and remains usable independent of the retired hosted service.

## Common Properties

- [Authentication](authentication/warrant-dev-authentication.yml)
- [Domain Security](security/warrant-dev-domain-security.yml)
- [GitHub Organization](https://github.com/warrant-dev)
- [LinkedIn](https://www.linkedin.com/company/warrant-dev)
- [Website](https://warrant.dev)
- [Documentation (WorkOS FGA, successor)](https://workos.com/docs/fga)
- [Plans](plans/warrant-dev-plans-pricing.yml)
- [Rate Limits](rate-limits/warrant-dev-rate-limits.yml)
- [Fin Ops](finops/warrant-dev-finops.yml)

## Maintainers

**FN:** Kin Lane
**Email:** kin@apievangelist.com
