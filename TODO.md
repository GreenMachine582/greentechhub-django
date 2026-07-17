[← Back to README](README.md)

# ✅ TODO / Milestones

> This file is a living checklist — tick items off as they land instead of regenerating it. See [README.md](README.md) for context and [docs/](docs/) for the detailed design behind each item.

## 🗺️ Milestones

Tracks `greentechhub-core`'s phasing, one step behind — deliberately slower given there's one consumer today.

### v0.1 — Settings, middleware, health
- [ ] `settings` shim ([docs/settings.md](docs/settings.md))
- [ ] `middleware` — request-id, timing, security headers ([docs/modules.md](docs/modules.md#middleware))
- [ ] `health` view ([docs/health.md](docs/health.md))

### v0.2 — Context processor, messages
- [ ] `context_processors` ([docs/context.md](docs/context.md))
- [ ] `messages` bridge ([docs/modules.md](docs/modules.md#messages-and-events))

### v0.3 — Auth bridge (local)
- [ ] `auth` — `local` adapter ([docs/auth.md](docs/auth.md))

### v0.4 — Authentik-backed auth, events
Depends on an Authentik instance actually existing to test against.

- [ ] `auth`'s `forward_auth` path ([docs/auth.md](docs/auth.md))
- [ ] `events` wiring ([docs/modules.md](docs/modules.md#messages-and-events))

### v1.0 — Validated in production
- [ ] GreenTechHub actually running on it in production
- [ ] Contract tests have validated parity with `greentechhub-core`'s shared contracts

## 🔄 Migration Tracking

GreenTechHub's adoption order. Not a rewrite; GreenTechHub keeps its own Django views and models throughout.

- [ ] **Settings shim** — align env var naming with the rest of the ecosystem, zero visible behavior change
- [ ] **Logging + health view** — GreenTechHub's `/health` starts reporting the same shape as everyone else's
- [ ] **Context processor** — wired in even before GreenTechHub's templates are touched
- [ ] **Auth bridge** — lowest priority; only matters once GreenTechHub needs the same identity model as the FastAPI services
