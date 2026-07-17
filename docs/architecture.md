[← Back to README](../README.md)

# 🏗️ Architecture

## Why this exists (the "is Django worth supporting" question, answered)

The honest alternative to this package is: GreenTechHub stays permanently excluded from the shared foundation, reinvents its own config/logging/health-check conventions, and drifts from the rest of the ecosystem. Given GreenTechHub is specifically described as the ecosystem's public-facing portal and (eventually) identity hub, that drift matters more there than it would for an isolated internal tool. The fix isn't making `greentechhub-core` framework-agnostic (that was the mistake the core split already avoided) — it's this thin adapter, translating `greentechhub-core`'s framework-independent contracts into Django's idioms.

This package is shaped as a generic Django adapter, not GreenTechHub-specific glue, even though GreenTechHub is the only consumer today. The cost of that generality is small, and the alternative — an API tightly coupled to GreenTechHub's internals — would make a future second Django consumer, should one ever appear, a rewrite instead of an adoption.

## Why this can stay small

Everything genuinely reusable already lives in `greentechhub-core` and doesn't need re-explaining here — identity/permissions/health/events/config/logging/observability/feature-flags are exactly the same regardless of framework. What's Django-specific is narrow:

- **Middleware registration** (Django's `MIDDLEWARE` list vs. FastAPI's `app.add_middleware`).
- **Where "current user" lives** (`request.user`, Django's convention, vs. a `Depends`-injected value).
- **How messages surface to templates** (Django's `messages` framework, bridged into `greentechhub-core`'s `FlashMessage` type).
- **Routing style** (Django URLconf vs. FastAPI routers) — this package supplies views/URL patterns to `include()`, not a framework of its own.
- **Django version support** — pinned informally to whatever GreenTechHub currently runs, not a generalized compatibility matrix; revisit only if a second consumer needs something different.

## Package layout

```
greentechhub-django/
├── src/greentechhub_django/
│   ├── settings.py                 # GTHBaseSettings <-> Django settings shim
│   ├── middleware/
│   │   ├── request_id.py
│   │   ├── timing.py
│   │   └── security_headers.py
│   ├── context_processors.py       # nav_items, current_user, flashes, brand
│   ├── messages.py                 # Django messages -> FlashMessage bridge
│   ├── auth/
│   │   ├── backend.py              # Django AUTHENTICATION_BACKENDS entry
│   │   └── middleware.py           # sets request.user from IdentityProvider
│   ├── health/
│   │   ├── views.py
│   │   └── urls.py
│   └── events.py
├── tests/
├── pyproject.toml
└── README.md
```

See [docs/context.md](context.md) for the context processor (the standout piece), [docs/settings.md](settings.md), [docs/auth.md](auth.md), and [docs/health.md](health.md) for the higher-detail modules, and [docs/modules.md](modules.md) for the rest.
