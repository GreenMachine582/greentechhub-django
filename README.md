# 🌉 greentechhub-django

[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg?logo=opensourceinitiative&logoColor=white)](LICENSE)
[![Status: Planning](https://img.shields.io/badge/Status-Planning-yellow.svg)](TODO.md)
[![Python](https://img.shields.io/badge/Python-3776AB.svg?logo=python&logoColor=white)](https://www.python.org/)
[![Django](https://img.shields.io/badge/Django-092E20.svg?logo=django&logoColor=white)](https://www.djangoproject.com/)
[![pytest](https://img.shields.io/badge/pytest-0A9EDC.svg?logo=pytest&logoColor=white)](https://docs.pytest.org/)

> Django integration layer for the GreenTechHub ecosystem. Adapts `greentechhub-core`'s framework-independent config, identity, health, and messaging contracts into Django middleware, context processors, and views — bringing GreenTechHub onto the same foundation as the FastAPI services without changing its framework.

This is the Django adapter layer over `greentechhub-core`: it depends on `greentechhub-core` and exists specifically because GreenTechHub — the ecosystem's public-facing portal — runs Django, not FastAPI.

## 🎯 Objective

The Django-specific adapter package, deliberately small: a thin set of middleware, context processors, and views that let GreenTechHub (currently the only Django service in the ecosystem) use the same configuration, logging, identity model, health checks, and event publishing as the FastAPI services — without migrating off Django, and without `greentechhub-core` having to know Django exists. It wires GreenTechHub's own database/cache into `greentechhub-core`'s checks via the health view — it provisions no infrastructure itself.

## 🧩 Scope

| Module | Responsibility | Doc |
|---|---|---|
| `settings` | A small shim connecting Django's `settings.py` to the same `greentechhub-core` `GTHBaseSettings` fields (`SECRET_KEY`, `LOG_LEVEL`, `AUTH_ADAPTER`, etc.) so env var names and validation match the FastAPI services exactly | [docs/settings.md](docs/settings.md) |
| `middleware` | Request-ID injection, timing, security headers — Django middleware wrapping `greentechhub-core`'s pure logic (e.g. `proxy` header parsing) | [docs/modules.md](docs/modules.md#middleware) |
| `context_processors` | Injects `nav_items`, `current_user`, `flashes`, `brand` into every template's context — this is what satisfies `greentechhub-ui`'s framework-agnostic template contract from the Django side | [docs/context.md](docs/context.md) |
| `messages` | Bridges Django's built-in `django.contrib.messages` framework into `greentechhub-core`'s `FlashMessage` type, so `greentechhub-ui`'s `gth-toast` renders identically regardless of which framework produced the message | [docs/modules.md](docs/modules.md#messages-and-events) |
| `auth` | A Django auth backend/middleware built on `greentechhub-core`'s `IdentityProvider` — sets `request.user` from its `local`/`forward_auth` provider implementations | [docs/auth.md](docs/auth.md) |
| `health` | A `/health` view running `greentechhub-core`'s health checks against this service's actual dependencies | [docs/health.md](docs/health.md) |
| `events` | Wires Django's app-ready/request lifecycle into `greentechhub-core`'s event publisher | [docs/modules.md](docs/modules.md) |

## 📚 Docs

| Doc | Covers |
|---|---|
| [docs/context.md](docs/context.md) | 🔌 The template context processor (read this first) |
| [docs/architecture.md](docs/architecture.md) | 🏗️ Why this exists, why it stays small, package layout |
| [docs/settings.md](docs/settings.md) | ⚙️ The `GTHBaseSettings` ↔ Django settings shim |
| [docs/auth.md](docs/auth.md) | 🔐 The `GTHAuthMiddleware` identity bridge |
| [docs/health.md](docs/health.md) | 🩺 The `/health` view |
| [docs/modules.md](docs/modules.md) | 🧩 Middleware, messages, events |
| [docs/versioning.md](docs/versioning.md) | 🏷️ Semver policy & distribution |
| [docs/testing.md](docs/testing.md) | 🧪 `pytest-django` + shared contract testing strategy |

## 🗺️ Status & Roadmap

Shipped versions and their notes: [CHANGELOG.md](CHANGELOG.md) and the [Releases page](https://github.com/GreenMachine582/greentechhub-django/releases) (both written by release-please from conventional commits). Open work: [TODO.md](TODO.md). Branches, PRs and how a release is cut: [CONTRIBUTING.md](CONTRIBUTING.md).

## 📄 Licence

[MIT](LICENSE) © 2026 Matthew Johnson
