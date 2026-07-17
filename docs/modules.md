[← Back to README](../README.md)

# 🧩 Middleware, Messages, Events

The smaller modules that don't warrant their own doc yet — see [docs/context.md](context.md), [docs/settings.md](settings.md), [docs/auth.md](auth.md), and [docs/health.md](health.md) for the higher-detail ones.

## Middleware

Django middleware (`middleware/request_id.py`, `timing.py`, `security_headers.py`) — registered in Django's `MIDDLEWARE` list, the equivalent of FastAPI's `app.add_middleware`.

## Messages and events

Messages bridges Django's built-in `django.contrib.messages` framework into `greentechhub-core`'s `FlashMessage` type, so `greentechhub-ui`'s `gth-toast` renders identically regardless of which framework produced the message — feeds the `flashes` key the [context processor](context.md) exposes. Events wires Django's app-ready/request lifecycle into `greentechhub-core`'s event publisher.
